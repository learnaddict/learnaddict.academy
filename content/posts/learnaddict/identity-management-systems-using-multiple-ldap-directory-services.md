---
title: "Identity Management Systems: Using Multiple LDAP Directory Services"
date: "2015-10-05"
author: "Matt Saunders"
description: "Identity Management Systems: Using Multiple LDAP Directory Services"
---

As an IT Professional with over fifteen years of technical experience and specialising in Identity Management, I have been involved with designing, supporting, and debugging identity management systems in several academic institutions.

Identity management is present in every IT environment. It determines whether a user can log in, who they are, and what they can do.

Even though identity plays such a crucial role in IT, it is often neglected, understood by only one or two people, or thought of in overly simplified terms compared to the actual complicated business logic and user lifecycle processes involved.

The events that trigger actions in an identity management system are usually driven by an external ‘source of truth’ database, which could be a student records and/or HR system. It can be very difficult sometimes to identify all of the possible ways that this information can change, in what order, and accurately define what this means for the current status of an individual.

The record for a student, in a student record system, can be manipulated in ways that do not always follow the configured standard business logic. The user interface on a student records system allows changes to a multitude of attributes, set statuses like pre-enrolled, enrolled, failed to complete, withdrawn, and completed, and the process may not always follow the same path for each and every student.

All of this affects the quality of the identity management system, and the user experience. It is this reason why you should always have an understanding of what attributes are changing, how they are changing, and why they are changing.

Testing of an identity management system is therefore extremely difficult, and should not be taken lightly or rushed. A single misconfiguration in the business logic could result in thousands of user accounts being deleted, disabled, or left insecure, and unexpected results may only appear when a certain number of other factors coexist at the same time.

Authentication and authorisation are commonly confused with each other and misunderstood. They are two different functions, and not the same thing.

Authentication is solely responsible for checking the supplied username and password combination matches that of a user previously configured in an identity store.

Authorisation is solely responsible for providing a level of access to systems and services, usually based on group membership, roles, attributes, flags, tokens, etc.

It is the identity management function which is responsible for knowing who these users are, their name, department, telephone, office, etc.

One of the most common authentication and authorisation protocols is called Lightweight Directory Access Protocol and examples of LDAP directory services include Active Directory, Active Directory Lightweight Directory Services, and e-Directory.

After experiencing environments with multiple LDAP directories, each with their own purpose, I have fully explored the benefits of having a well designed, well structured, and secure LDAP directory services infrastructure.

It is possible to have an LDAP directory service that only provides authentication. This means that the only information in the directory are usernames and passwords. The security can be further improved by only allowing the authenticated user access to their own details. This minimises the risk of losing data, especially data that isn’t required for the specific service.

Using a single Active Directory directory service for all authentication purposes limits the ability to control authorisation on third party systems that rely solely on authentication to provide it’s authorisation. i.e. username and password is correct so you may use this system. In this case there is no ‘off button’ for selectively stopping access to services, and anyone can point a new system at Active Directory with no IT Admin intervention.

It is for this reason that I would not recommend Active Directory being the ‘source of truth’ for all authentication, and also not to hold any further information than required to support desktop, server, and email services.

I would definitely recommend using Active Directory for managing Windows environments, but the schema and functional level must be kept up to date. Upgrading Active Directory is a ‘business as usual’ task in the millions of businesses that use it around the world.

The attractive solution is to have an identity vault that holds details of all the user accounts ever created. The external ‘sources of truth’ update the identity vault and as the accounts are updated, the business logic ensures that other connected LDAP directory services are also updated to reflect the changes where required.

The passwords in the identity vault are stored using a reversible encryption, so they can be decrypted in the future. It is therefore extremely important to secure the clustered identity vault infrastructure and never let any user authenticate against it.

The ability to decrypt passwords allows us to create and delete accounts in other LDAP directory services at any point in the future. So, for example, when a user should no longer have access to log on to desktop computers, the account can be deleted from Active Directory. If at a later date the user is then authorised to access desktop computers again, the account can be recreated in Active Directory with the up to date information from the identity vault.

Using LDAP directory services for specific purposes can also greatly improve information security as only the minimum amount of information is being exposed. Sensitive information, for example, date of birth, shouldn’t be stored in Active Directory because of the inherited read permissions by default, but could be stored safely in an independent directory service.

Identity is an topic that can be discussed for hours, can be elegantly designed, and enable a secure and reliable user experience.
