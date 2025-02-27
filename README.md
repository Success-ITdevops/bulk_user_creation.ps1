### PowerShell Script: Bulk User Creation in Active Directory
```powershell
# Import users from a CSV file and create them in Active Directory
Import-Module ActiveDirectory

$users = Import-Csv -Path "users.csv"

foreach ($user in $users) {
    $password = ConvertTo-SecureString $user.Password -AsPlainText -Force
    New-ADUser -Name $user.Name `
                -GivenName $user.FirstName `
                -Surname $user.LastName `
                -SamAccountName $user.Username `
                -UserPrincipalName "$($user.Username)@yourdomain.com" `
                -Path "OU=Users,DC=yourdomain,DC=com" `
                -AccountPassword $password `
                -Enabled $true
    Write-Host "Created User: $($user.Username)"
}
```

### PowerShell Script: Modify Users in AD
```powershell
# Modify Active Directory users (update attributes)
Import-Module ActiveDirectory

$users = Import-Csv -Path "modify_users.csv"

foreach ($user in $users) {
    Set-ADUser -Identity $user.Username `
               -Title $user.Title `
               -Department $user.Department `
               -OfficePhone $user.Phone `
               -EmailAddress $user.Email
    Write-Host "Updated User: $($user.Username)"
}
```

### PowerShell Script: Enforce Password Policies
```powershell
# Enforce Strong Password Policies in Active Directory
Import-Module ActiveDirectory

$policy = Get-ADDefaultDomainPasswordPolicy
Set-ADDefaultDomainPasswordPolicy `
    -ComplexityEnabled $true `
    -MinPasswordLength 12 `
    -LockoutThreshold 5 `
    -LockoutDuration (New-TimeSpan -Minutes 15)

Write-Host "Password policy updated successfully."
```

 **Usage Instructions:**
1. Create a CSV file named `users.csv` with columns: `Name, FirstName, LastName, Username, Password`
2. Create a CSV file named `modify_users.csv` with columns: `Username, Title, Department, Phone, Email`
3. Run the respective scripts in PowerShell as an **Administrator**

These scripts help automate **Active Directory user management** and enhance security!
