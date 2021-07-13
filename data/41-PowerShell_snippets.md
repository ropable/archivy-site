---
date: 07-13-21
id: 41
path: ''
tags: []
title: PowerShell snippets
type: note
---

# Team Call Queue stats

```powershell
# Office 365 admin credentials
$o365_user = "<USERNAME>";
$o365_pass = "<PASSWORD>";
$cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $o365_user, $(ConvertTo-SecureString $o365_pass -AsPlainText -Force);

# Run for up to an hour, polling call queues every 60 secs
$TimeEnd = $(Get-Date).AddHours(1);

# Connect accounts
Connect-AzAccount -Credential $cred -Subscription bc1b9b1f-f768-42b8-b6b4-c7841269e454;
$blobstore = $(Get-AzStorageAccount -ResourceGroupName oim-appservices -Name dbcacallqueues).Context;

# Install-Module MicrosoftTeams
Import-Module -Name MicrosoftTeams;
Connect-MicrosoftTeams -Credential $cred;
Import-PSSession $(New-CsOnlineSession -Credential $cred);

# Upload online users and application services.
Get-CsOnlineUser | ConvertTo-Json -Compress | Set-Content -Encoding UTF8 C:\cron\blobs\csonlineusers.json
Set-AzStorageBlobContent -File "C:\cron\blobs\csonlineusers.json" -Container teams-call-queues -Blob "csonlineusers.json" -Context $blobstore -Properties @{"ContentType" = "application/json" } -Force
Get-CsOnlineApplicationInstance | ConvertTo-Json -Compress | Set-Content -Encoding UTF8 C:\cron\blobs\csonlineapplicationinstances.json
Set-AzStorageBlobContent -File "C:\cron\blobs\csonlineapplicationinstances.json" -Container teams-call-queues -Blob "csonlineapplicationinstances.json" -Context $blobstore -Properties @{"ContentType" = "application/json" } -Force

While ($(Get-Date) -le $TimeEnd) {
    # Upload call queues every 30 secs
    Get-CsCallQueue | ConvertTo-Json -Compress | Set-Content -Encoding UTF8 C:\cron\blobs\cscallqueues.json
    Set-AzStorageBlobContent -File "C:\cron\blobs\cscallqueues.json" -Container teams-call-queues -Blob "cscallqueues.json" -Context $blobstore -Properties @{"ContentType" = "application/json" } -Force
    # Output a single call queue (Service Desk):
    $sdqueue = Get-CsCallQueue -Identity 7fb290a2-066c-4794-b0cc-f5b882957785
    # Add the current timestamp to the SD queue object:
    $sdqueue | Add-Member -NotePropertyName OutputTimestamp -NotePropertyValue $(Get-Date -Format "o")
    $sdqueue | ConvertTo-Json -Compress | Set-Content -Encoding UTF8 C:\cron\blobs\cscallqueue-sd.json
    Set-AzStorageBlobContent -File "C:\cron\blobs\cscallqueue-sd.json" -Container teams-call-queues -Blob "cscallqueue-sd.json" -Context $blobstore -Properties @{"ContentType" = "application/json" } -Force
    Start-Sleep -Seconds 30;
}

Get-PSSession | Remove-PSSession;
```

# AD user data export

```
param( [Parameter(Mandatory = $true)] $Export)

# Office 365 admin credentials
$o365_user = "<USERNAME>";
$o365_pass = "<PASSWORD>"
$cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $o365_user, $(ConvertTo-SecureString $o365_pass -AsPlainText -Force);
Connect-AzAccount -Credential $cred -Subscription bc1b9b1f-f768-42b8-b6b4-c7841269e454;
$blobstore = $(Get-AzStorageAccount -ResourceGroupName dbca-messaging-pipeline -Name dbcamessagingpipeline).Context

if ($Export -eq "AzureAD") {
    # Connect to online accounts
    Connect-AzureAD -Credential $cred;

    # Export Azure AD data
    $aadusers = Get-AzureADUser -All $true
    $DistinguishedName = @{l = "DistinguishedName"; e = { $_.ExtensionProperty.onPremisesDistinguishedName } }
    # Cache managers until 8pm
    if ($(Get-Date -Format HH) -lt "20") {
        $mgrs = @{}
        $lastaadusers = Get-Content C:\cron\blobs\aadusers.json | ConvertFrom-Json
        $lastaadusers | where Manager -notlike '' | foreach { $mgrs[$_.ObjectId] = $_.Manager }
        $Manager = @{l = 'Manager'; e = { $mgrs[$_.ObjectId] } }
    } else {
        $Manager = @{l = 'Manager'; e = { Get-AzureADUserManager -ObjectId $_.ObjectId | select ObjectID, UserPrincipalName, $DistinguishedName } }
    }
    $aadusers | select *, $Manager | ConvertTo-Json -Compress | Set-Content -Encoding UTF8 C:\cron\blobs\aadusers.json
    Set-AzStorageBlobContent -File "C:\cron\blobs\aadusers.json" -Container azuread -Blob "aadusers.json" -Context $blobstore -Properties @{"ContentType" = "application/json" } -Force
} elseif ($Export -eq "OnPremAD") {
    # Export onprem AD data
    $adprops = @("Title", "DisplayName", "GivenName", "Surname", "Company", "physicalDeliveryOfficeName", "StreetAddress", 
        "Division", "Department", "Country", "State", "wWWHomePage", "Manager", "EmployeeID", "EmployeeNumber", "HomePhone",
        "telephoneNumber", "Mobile", "Fax", "employeeType", "EmailAddress", "UserPrincipalName", "Modified", 
        "AccountExpirationDate", "Info", "pwdLastSet", "targetAddress", "msExchRemoteRecipientType", "msExchRecipientTypeDetails", 
        "proxyAddresses", "DistinguishedName", "Enabled", "SamAccountName", "ObjectGUID", "SID")
    $adusers = Get-ADUser -Filter * -Properties $adprops; # DBCA DC
    $adusers += Get-ADUser -Filter * -Properties $adprops -Server <IP ADDRESS>; # RIA DC
    $adusers += Get-ADUser -Filter * -Properties $adprops -Server <IP ADDRESS>; # BGPA DC
    $adusers += Get-ADUser -Filter * -Properties $adprops -Server <IP ADDRESS>; # Zoo DC
    $adusers | select $adprops | ConvertTo-Json -Compress | Set-Content -Encoding UTF8 C:\cron\blobs\adusers.json
    Set-AzStorageBlobContent -File "C:\cron\blobs\adusers.json" -Container azuread -Blob "adusers.json" -Context $blobstore -Properties @{"ContentType" = "application/json" } -Force
} else {
    Write-Host 'Specify the type of export with -Export, valid options are "AzureAD" or "OnPremAD".'
}
```