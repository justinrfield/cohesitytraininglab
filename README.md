# cohesitytraininglab

# Need to run PowerShell as an Administrator
# Run command -> Set-ExecutionPolicy Unrestricted
# Type -> A for All

# Download Commands
$scriptName = 'createVMProtectionJob'
$repoURL = 'https://raw.githubusercontent.com/bseltz-cohesity/scripts/master/powershell'
(Invoke-WebRequest -Uri "$repoUrl/$scriptName/$scriptName.ps1").content | Out-File "$scriptName.ps1"; (Get-Content "$scriptName.ps1") | Set-Content "$scriptName.ps1"
(Invoke-WebRequest -Uri "$repoUrl/cohesity-api/cohesity-api.ps1").content | Out-File cohesity-api.ps1; (Get-Content cohesity-api.ps1) | Set-Content cohesity-api.ps1
# End Download Commands



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

