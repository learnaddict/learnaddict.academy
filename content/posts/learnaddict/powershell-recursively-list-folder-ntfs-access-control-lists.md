---
title: "Powershell: Recursively List Folder NTFS Access Control Lists"
date: "2015-07-30"
author: "Matt Saunders"
description: "Powershell: Recursively List Folder NTFS Access Control Lists"
---

This script will recursively list the NTFS Access Control Lists on folders in a large folder structure.

It outputs the non-inherited ACLs on each folder recursively and shows where inheritance has been disabled. The NTFSSecurity module supports path lengths up to 32,768 characters.

The output formatting could do with some improvement. In the future, I am looking to dump this data into a database, instead of the console.

* PowerShell Version: 4
* Required PowerShell Module: [NTFSSecurity](https://gallery.technet.microsoft.com/scriptcenter/1abd77a5-9c0b-4a2b-acef-90dbb2b84e85)
* Further Reading: [Weekend Scripter: Use PowerShell to Get, Add, and Remove NTFS Permissions](http://blogs.technet.com/b/heyscriptingguy/archive/2014/11/22/weekend-scripter-use-powershell-to-get-add-and-remove-ntfs-permissions.aspx)


```powershell
[CmdletBinding()]
Param(
  [Parameter(Mandatory=$True,Position=1)]
   [string]$path
)

Import-Module NTFSSecurity

function ProcessChildren ([string]$path)
{
    $children = Get-ChildItem2 $path -Directory
    foreach ($child in $children)
    {
        ProcessChild $child.FullName
        ProcessChildren $child.FullName
    }
}

function ProcessChild ([string]$path, $excl=$True)
{
    $header = "`r`n$($path)"
    if ($excl -eq $True)
    {
        $acls = Get-NTFSAccess -ExcludeInherited $path
    }
    else
    {
        $acls = Get-NTFSAccess $path
    }
    if ($(Get-NTFSAccess $path).InheritanceEnabled -NotContains "True")
    {
        [console]::Writeline($header)
        $header = ""
        [console]::Writeline("Inheritance: Disabled")
    }
    if ($acls.Count -gt 0)
    {
        $aclmap = @{}
        foreach ($acl in $acls)
        {
            $accessrights = $($acl.AccessRights -join ",")
            $account = $acl.Account.AccountName
            if ($exclude -NotContains $account)
            {
                if ($aclmap.ContainsKey($accessrights))
                {
                    [void]$aclmap[$accessrights].Add($account)
                }
                else
                {
                    [void]$aclmap.Add($accessrights, [System.Collections.ArrayList]@($account))
                }
            }
        }
    }
    if ($aclmap.Keys.Count -gt 0)
    {
        if ($header)
        {
            [console]::Writeline($header)
        }
        foreach ($key in $aclmap.Keys | Sort)
        {
            $accounts = $($aclmap[$key] -join "`r`n".PadRight(42, ' '))
            [console]::Writeline("  {0,-38}{1}", $key, $accounts)
        }
    }
}

$exclude = @("BUILTIN\Administrators",    `
             "CREATOR OWNER",             `
             "BUILTIN\Users",             `
             "NT AUTHORITY\SYSTEM")

ProcessChild $path $False
ProcessChildren $path
```
