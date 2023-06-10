---
title: "Shared File Storage (Part 2): Knowing Your Audience"
date: "2015-09-29"
author: "Matt Saunders"
description: "Shared File Storage (Part 2): Knowing Your Audience"
---

Previously, [Shared File Storage (Part 1): Identifying Technical Debt](https://learnaddict.com/2015/08/25/shared-file-storage-part-1-identifying-technical-debt/) discussed the importance of identifying technical debt.

In part 2 of this series, we explore the importance of understanding your audience. The term audience is used here to describe the people accessing the storage, their role and participation in the business, and the groups that are configured in Active Directory.

Groups are the key to keeping your NTFS permissions simple to manage, so identifying your different audiences and adding them to a set of standardised groups is the first step.

If you don’t already manage your departmental groups with an automated process, doing so will make lots of other processes simpler too.

The names of your groups should be predictable and easy to understand, especially in order to make use of automation and scripts to manage your groups and storage. It will also help with ambiguity and accidental reuse of a group beyond its original intention.

Unstructured group names increases the technical debt and the resources it takes to manage the NTFS permissions, whilst decreasing the accuracy of those actions.

In this example, we will use the following groups to define our audiences. There is a group for each definable group of people in the business.

![Table](/images/learnaddict/knowyouraudience1.png)

At first this may look like a lot of groups, and a lot to manage, but it’s not. Every member of the business will be associated with a business unit, department, role, team, or project.

The group membership should be nested for business units and departments, so a role group will be a member of the department, and the department will be a member of the business unit. Business units can be named after academic schools, colleges, professional services, or any other business unit names.

The folder group names should be suffixed with the level of access provided by the group, for example, full control, modify, or read-only.

![Table](/images/learnaddict/knowyouraudience2.png)

(Traverse folder / execute data,List folder / read data,Read attributes,Read extended attributes,Read permissions)
The next part of this series will discuss how these audience groups translate into NTFS permissions.
