---
title: "Shared File Storage (Part 1): Identifying Technical Debt"
date: "2015-08-25"
author: "Matt Saunders"
description: "Shared File Storage (Part 1): Identifying Technical Debt"
---

I would like to share some ideas on designing an elegant structure for shared file storage. It is something that every organisation in academia has to design, tame, reorganise, redesign, and continually manage. Even in the cloud era, most of us won’t be saying goodbye to our fast, local, and reliable on-campus Windows shared storage. These thoughts on shared storage are my own and have been shaped by 15 years of experience in academic and business IT environments dealing with storage and identity management.

Within every IT environment, it is important to use the most efficient and effective authentication and authorisation design to minimise the technical debt that is accrued by unnecessary complexities. The greater the technical debt, the more resources it will take to manage, problem solve, and later improve. Identity management can accrue a high level of technical debt if not designed and tested properly, and carries the possibility of exposing personal and confidential information to the wrong audience, or worse.

The Windows NTFS file and folder permissions are very flexibility in their configuration. Every file and folder has it’s own copy of an Access Control List containing these permissions, which makes changes to a large folder structure very time consuming to change as each file has to be updated in turn. It is therefore important to plan for and fully understand all the different use cases for your storage before creating a new shared volume.

The majority of NTFS permissions are applied to allow access, but they can also deny access. Denying access can be unpredictable, especially where users are members of multiple groups. Deny permissions are prioritised over allow permissions, therefore problem solving can be painful, especially with nested group memberships. The simple solution is to never use deny permissions.

It is possible to disable the inheritance of NTFS permissions at any point down the folder structure, but every time this occurs, the technical debt increases. The use case for this is where a folder needs to be locked down to a different group of people than the folder above. Disabling the inheritance will make it impossible to later grant new or update existing permissions to the entire volume, meaning lots of extra work, especially if the change is because of a time sensitive security incident.

Disabling the inheritance may also cause some people difficulty in traversing the folder structure to get to their folders. It is likely that some ugly folder traversal permissions would be required, exposing numerous file and folder names to people who maybe shouldn’t see them. Folders that are named similar to someone else’s job, project, include the words tribunal or investigation, or a person’s name may cause unnecessary attention and complaints.

If hosting the file storage on a Windows server, one seemingly attractive solution may be to create a new share within the already shared volume, which gets around the folder traversal problem. Again, this is can be dangerous, as your audience changes part way through the volume and inherited permissions, and not every support analyst may understand the repercussions of making their changes.

Windows shares can also be protected by the permissions on the share itself. This allows Read, Modify, or Full Control permissions to users and groups. It is best practice to let the NTFS file and folder permissions control the access, rather than placing business logic in two places, which can make it harder to problem solve.

It is useful to know that the share permissions can be extremely useful in a security incident where an entire volume needs to be immediately made read-only for everyone except domain admins, for example, where CryptoLocker is encrypting your files.

Technical debt is accrued whilst placing sticky plasters over a problem, instead of resolving the root cause. The elegant way to use NTFS permissions is to allow them to flow from the top down, adding additional permissions and applying inheritance to ‘This Folder Only’ rather than the entire folder structure.

It is possible to design a standard folder structure that is easy to manage and minimises technical debt. The design should be repeatable, scriptable, and automated.

This series continues with [Shared File Storage (Part 2): Knowing Your Audience](https://learnaddict.com/2015/09/29/shared-file-storage-part-2-knowing-your-audience/).
