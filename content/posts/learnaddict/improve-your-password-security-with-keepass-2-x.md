---
title: "Improve your password security with KeePass 2 x"
date: "2015-07-25"
author: "Matt Saunders"
cover: 
  image: "/images/learnaddict/keepass/keepass.png"
  alt: "A picture of KeePass"
  relative: true
description: "Improve your password security with KeePass 2.x"
---

Would you continue to use your toothbrush after using it to clean the toilet?

Obviously not, but often we like to reuse passwords for multiple websites and services. It’s not easy to remember great passwords for every website, so we need a secure method of storing them.

The reusing of passwords is dangerous, and these days we regularly hear of websites that have been hacked and the user information has been made available on the Internet for fraudsters and hackers to abuse.

The most important account, in addition to your online banking, is your email account. If a hacker can access your email, they can then try to password reset your other accounts on most other websites and services.

It’s not unusual for a hacked email account to have inbox rules or filters to automatically forward and then delete password reset emails, so the real user never sees them.

KeePass 2.x is a secure password safe to help you be better at password management.

The old KeePass version 1.x, and the version 1.x .kdb file extension, are no longer considered secure, so please do ensure you are using version 2.x, with the version 2.x .kdbx file extension.

Always make sure you backup your KeePass database file to a secure offsite location. Some easy backup methods include uploading to your Google Drive or OneDrive, or copying to a memory stick and asking a family member who lives away from you to take care of it. If you add passwords to your KeePass database, you’ll need to create a new backup.

Features of KeePass 2.x

Fully encrypted database with AES-256 encryption and SHA-256 hashing
In-memory protection
[Keylogger protection available](http://keepass.info/help/v2/autotype_obfuscation.html)
Multiple platform support (Linux, Mac, Windows)
Portable and Installable versions of KeePass 2.x for Windows
Run KeePass 2.x from a memory stick for a portable solution (Keep a backup!)
3rd party apps for Android and iPhone
Keepass2Android Offline
Import of passwords from existing password stores
Automatic clipboard clearing
Secure random password creator
Securely distribute passwords within teams
Support for multi-user environments
Store KeePass database files on team shared storage
Access your passwords in a Disaster Recovery situation
Regularly backup your KeePass database files to a secure Cloud storage platform
Ensure 2 Step Authentication (2FA) is configured on any Cloud storage platform for extra protection


[KeePass Website](http://keepass.info/)
Download [KeePass Professional Edition 2.x.x](http://keepass.info/download.html)

Creating a new KeePass database file
After installing KeePass 2.x, create a new database using File–>New… Ensure the Master password you enter is very secure, as this unlocks all your passwords.

It is easy to create long secure passwords by joining several unconnected words together with symbols and numbers in between. For example, Bottled&Squidy%Bobbles8Boating is 30 characters long, but memorable, and doesn’t feel difficult to remember.

You are aiming for a large number of different characters in your password that can’t be guessed out of a dictionary of words and common phrases. The only way to crack this type of password is using brute force, and that takes a long time.

It’s also possible to use Key file and Windows user account authentications methods, which you can learn about more in the [KeePass 2.x documentation](http://keepass.info/help/base/keys.html)

![Keepass screenshot](/images/learnaddict/keepass/keepass1.png)

After choosing your Master password, the Database Settings dialogue will appear. This shows that the KeePass database file will be encrypted with string AES 256 encryption.

![Keepass screenshot](/images/learnaddict/keepass/keepass2.png)

On the other tabs it is possible to change further settings, but the defaults are usually sufficient.

![Keepass screenshot](/images/learnaddict/keepass/keepass3.png)

## Adding passwords to KeePass

When the main KeePass window opens with your new database, it will be unsaved. Click save to write the database to disk. It’s advisable at this point to close KeePass and re-open your database again to check the Master password works as expected.

The KeePass database supports groups to keep related passwords together. The default groups and password entries can be deleted or renamed based on your preferences.

A recycle bin will appear when you delete your first item, but they can be permanently deleted from the recycle bin by deleting them again from there.

![Keepass screenshot](/images/learnaddict/keepass/keepass4.png)

It’s also possible to add your own groups.

![Keepass screenshot](/images/learnaddict/keepass/keepass5.png)

New password entries can be added by clicking Edit–>Add Entry… The following dialogue will ask for a title, e.g. the place where you will use this password, and the username and password that you use.

A password will be pre-populated, which can be viewed by clicking the 3 dots button on the right. If this is sufficient for your needs, finish entering the details and click OK. It’s best to save the KeePass database before using the password, as if you forget to save later on, you may lose the password forever.

![Keepass screenshot](/images/learnaddict/keepass/keepass6.png)

If the pre-populated password is not suitable, click the new key button (below the 3 dots button), and open the Password Generator. Here you can select any strength of password you require. Clicking OK will generate the new password and populate the password box.

It’s generally advisable to use a password with the longest length that a service or website supports, like 20+ characters, containing upper-case, lower-case, numbers, and symbols. On the advanced tab, it is possible to ignore easily confused characters if typing in the password, but this obviously reduces the possible combinations slightly.

![Keepass screenshot](/images/learnaddict/keepass/keepass7.png)

On the Add Entry dialogue, it is possible to include metadata about the account and password, and also attach small files. This information is encrypted within the KeePass database, so this provides a safe place to store related sensitive information.

![Keepass screenshot](/images/learnaddict/keepass/keepass8.png)

## Using KeePass to login to websites and other services

KeePass uses the clipboard so that you don’t need to type in your passwords. The clipboard is cleared automatically after a configurable delay, as long as you copy the password using the Copy Password option.

It’s easy to copy the password, just select the entry you require to log in as, and right click–>Copy Password (or Ctrl-C). You can also copy the username using the right click menu or Ctrl-B.

![Keepass screenshot](/images/learnaddict/keepass/keepass9.png)

Some websites don’t allow you to paste in passwords, therefore, you have to open the entry properties and type the password in manually. Sometimes the characters can be hard to identify if you have some easily confusable characters, so pasting into a new unsaved Notepad document temporarily can help.

Always remember: Your passwords are only as secure as your Master password. If you leave your KeePass database open when you leave your computer, anyone can log into any of your accounts. If you lock your computer with KeePass open, it’s still only as secure as your unlock password.

KeePass is great for allowing you to have a different password on every single account you have. It’s still important to remember to change these passwords regularly, as every time it is used, there is a possibility that it has been compromised. If you can use 2 factor authentication on websites in addition to a strong password, then only someone with your mobile phone will be able to authenticate successfully.
