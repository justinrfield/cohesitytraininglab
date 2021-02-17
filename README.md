# cohesitytraininglab

# powershell setup
Need to run PowerShell as an Administrator
Run command -> Set-ExecutionPolicy Unrestricted
Type -> A for All

# Download Commands - paste the below into powershell and execute
$repoURL = 'https://raw.githubusercontent.com/justinrfield/cohesitytraininglab/main'
(Invoke-WebRequest -Uri "$repoUrl/createVMProtectionJob.ps1").content | Out-File "createVMProtectionJob.ps1"; (Get-Content "createVMProtectionJob.ps1") | Set-Content "createVMProtectionJob.ps1"
(Invoke-WebRequest -Uri "$repoUrl/createProtectionPolicy.ps1").content | Out-File "createProtectionPolicy.ps1"; (Get-Content "createProtectionPolicy.ps1") | Set-Content "createProtectionPolicy.ps1"
(Invoke-WebRequest -Uri "$repoUrl/addVMProtectionJob.ps1").content | Out-File "addVMProtectionJob.ps1"; (Get-Content "addVMProtectionJob.ps1") | Set-Content "addVMProtectionJob.ps1"
(Invoke-WebRequest -Uri "$repoUrl/cohesity-api.ps1").content | Out-File cohesity-api.ps1; (Get-Content cohesity-api.ps1) | Set-Content cohesity-api.ps1

# Creates a policy
$policyname - 'enterpolicynamehere'
.\createProtectionPolicy.ps1 -vip cohesity.example.com -username admin -policyName "$policyname" -daysToKeep 3

# Creates a protection job
$protectionjob = 'enterprotectionjobhere'
$jobstarttime = '14:30'
.\createVMProtectionJob.ps1 -vip cohesity.example.com -username admin -jobName "$protectionjob" -policyName "$policyname" -vCenterName vcenter.example.com -startTime "$jobstarttime" -vmlist ./vmlist.txt

# Add VM to a protection job
./addVMtoProtectionJob.ps1 -vip cohesity.example.com -username admin -jobName JustinBackup -vmNames (Get-Content ./vmadds.txt)

# Add another VM to a proteciton job
./addVMtoProtectionJob.ps1 -vip cohesity.example.com -username admin -jobName JustinBackup -vmNames (Get-Content ./vmadds2.txt)

