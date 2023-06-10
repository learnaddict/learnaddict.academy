---
title: "Using Gammu-smsd and MySQL to Send and Receive SMS Text Messages"
date: "2018-02-20"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/sms2/sim800incase.jpg"
  alt: "A SIM800 HAT sitting on a Raspberry Pi"
  relative: true 
description: "Using Gammu-smsd and MySQL to Send and Receive SMS Text Messages"
---

In the previous post, [Send and Receive SMS Text Messages with Raspberry Pi and SIM800 GSM Board](https://learnaddict.com/2018/02/17/send-and-receive-sms-text-messages-with-raspberry-pi-and-sim800-gsm-board/), we connected up an [Itead Raspberry Pi GSM Board (SIM800)](https://www.modmypi.com/raspberry-pi/communication-1068/raspberry-pi-sim800-gsm-breakout-board/) from [ModMyPi](https://www.modmypi.com/) and configured Gammu to send and receive SMS text messages from the command line.

In this post, we will install and configure Gammu-smsd to provide a MySQL database interface. This allows for more control and easier access to send and receive SMS text messages on your Raspberry Pi.

The Gammu-smsd software runs as a service so will almost immediately update the MySQL table with incoming messages to the inbox and send new messages in the outbox. This post assumes that you have a GSM modem already working with Gammu.

Let’s start by installing and configuring MySQL on Raspbian. It will actually install MariaDB, which is a fork of MySQL that will always remain as free Open Source software. We’ll also secure the MySQL configuration and install phpMyAdmin to provide a simple method of accessing the MySQL tables. The apt-get package names have changed over time, but I have left the previous names here in case it helps on your version of Linux.

```bash
sudo apt-get update
sudo apt-get install mariadb-server-10.0 mariadb-client-10.0
```

With the database installed, we now need to secure it. By default there is no password securing the database.

```bash
sudo mysql_secure_installation
```

The current password for root is blank, so just press enter. All questions are ‘y’ for yes. Enter a new secure password for root, and then again to confirm.

![Console Output](/images/learnaddict/sms2/console1.png)

Now that MySQL is installed and secured, we need a new database for Gammu-smsd. Let’s log into our MySQL server using the MySQL client and create a database for our SMS text messages. The following command will load the MySQL client and ask for root password that you just set.

```
sudo mysql -u root -p
```

The Gammu-smsd MySQL page suggests the following configuration of the database. Please change the password in the first line to something secure. The console output is shown below.

```sql
GRANT USAGE ON *.* TO 'smsd'@'localhost' IDENTIFIED BY 'password';
GRANT SELECT, INSERT, UPDATE, DELETE ON `smsd`.* TO 'smsd'@'localhost';
CREATE DATABASE smsd;
```

![Console Output](/images/learnaddict/sms2/console2.png)

The privileges need to be re-read from the database before the new user will work. The following command entered into the MySQL client will achieve this.

```
FLUSH PRIVILEGES;
```

Now that this has been completed, we can exit the MySQL client.

quit

The next step will be to install phpMyAdmin. If you are happy to use the MySQL client for writing SQL select statements to check for SMS text messages, this step may not be necessary for you. The following command will install phpMyAdmin. Select apache2 using space bar and press tab to move to OK, press enter.

```bash
sudo apt-get install phpmyadmin
```

![Console Output](/images/learnaddict/sms2/phpmyadmin1.png)

phpMyAdmin will require a database itself, which can be automatically created by the tool. Select Yes when the following box appears.

![Console Output](/images/learnaddict/sms2/phpmyadmin2.png)

Enter a password for the phpmyadmin database user. As you already have the root password and the smsd password, this could be left blank for a random password.

![Console Output](/images/learnaddict/sms2/phpmyadmin3.png)

Let’s check that phpMyAdmin is working correctly. You’ll need the IP Address of the Raspberry Pi. Enter the following command to find out what it is.

```bash
ifconfig
```

In a web browser on your computer, enter http://<IP Address>/phpmyadmin

For example, if the IP Address is 192.168.0.30, the URL would be http://192.168.0.30/phpmyadmin

![Console Output](/images/learnaddict/sms2/phpmyadmin4.png)

Log in to phpMyAdmin with the username that we created for Gammu-smsd called smsd and the password that you entered in the SQL command. Once logged in, the smsd database will appear on the left, but it will not have any tables yet.

![Console Output](/images/learnaddict/sms2/phpmyadmin5.png)

We can create the required tables once the Gammu-smsd software is installed. Let’s install Gammu-smsd now, although I have noticed that the install currently ends with an error. The reason is that the install tries to start the software before it is configured. We can clear the error after configuration.

```bash
sudo apt-get install gammu-smsd gammu
```
![Console Output](/images/learnaddict/sms2/gammuinstall.png)

Don’t worry, we will edit the configuration file next. Nano is a friendly text editor in the console.

```bash
sudo nano /etc/gammu-smsdrc
```
![Console Output](/images/learnaddict/sms2/gammuinstall1.png)

The configuration I am using with the SIM800 board is as follows. Please remember to change the password for the smsd database user to the secure password you set earlier.

![Console Output](/images/learnaddict/sms2/gammuinstall2.png)

After the file has been changed, use the keyboard combination Ctrl-o to write out the file, and press enter to accept the filename.

Next we need to create the default database tables for Gammu-smsd to use. The error will not be resolved until the tables are created. The great news is that it’s easy to create them.

```bash
cd /usr/share/doc/gammu/examples/sql
sudo gunzip mysql.sql.gz
sudo mysql -uroot -p smsd < mysql.sql
cd ~
```

The first line will change to the directory holding the default Gammu-smsd MySQL database file. The second line will uncompress the mysql.sql.gz file. The third line will import the SQL statements in the mysql.sql file. Enter the root database password that you set previously. The fourth command will change back to the home directory /home/pi.

The gammu-smsd service will need restarting in order to start using the new configuration.

```bash
sudo systemctl restart gammu-smsd
```

If you would like to check the service output, the journalctl command will provide the most recent output.

```bash
sudo journalctl -u gammu-smsd
```

If there are still problems, check the configuration file again for errors. If you’re stuck, running sudo gammu-smsd --config /etc/gammu-smsdrc will show the full error message. If it just displays the log file location, it’s now working, press Ctrl-c to quit.

![Console Output](/images/learnaddict/sms2/sms1.png)

In phpMyAdmin, we can see there are now database tables. If you send an SMS text message from your mobile phone to your GSM modems SIM card mobile number, it should appear in the inbox table. You may need to click the refresh link to update the rows displayed. Scrolling the web page across should show you all the details about the SMS text message, including the decoded text.

Now let’s send an SMS text message from the Raspberry Pi to your mobile phone. The following command will write a row into the MySQL database outbox table, but please change the xxxxxxxxx to your mobile phone number. Once the SMS text message is sent, it will disappear from the outbox table and appear in the sentitems table.

```bash
echo "Sending an SMS from my Raspberry Pi" | sudo gammu-smsd-inject TEXT 00447xxxxxxxxx
```

Hopefully you have been able to send and receive an SMS text message using your Raspberry Pi, Gammu-smsd, and MySQL.

For further information, please have a look at the [Gammu-smsd configuration manual](https://wammu.eu/docs/manual/smsd/config.html) and the [gammu-smsd-inject manual](https://wammu.eu/docs/manual/smsd/inject.html).
