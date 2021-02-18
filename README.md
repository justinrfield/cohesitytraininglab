# **** COHESITY TRAINING LAB ****

# Internet Explorer setup
For the download commands below to work, you need to open up Internet Explorer (not Edge) one time and complete the setup steps.  Close the browser once complete.

# Powershell setup
```powershell
# Need to run PowerShell as an Administrator and run the following command.
Set-ExecutionPolicy Unrestricted
# A for all and hit enter
```

# Download PowerShell Scripts
```powershell
# paste the below into powershell and execute
cd c:\users\admin\desktop
md cohesity
cd cohesity
$repoURL = 'https://raw.githubusercontent.com/justinrfield/cohesitytraininglab/main'
(Invoke-WebRequest -Uri "$repoUrl/createVMProtectionJob.ps1").content | Out-File "createVMProtectionJob.ps1"; (Get-Content "createVMProtectionJob.ps1") | Set-Content "createVMProtectionJob.ps1"
(Invoke-WebRequest -Uri "$repoUrl/createProtectionPolicy.ps1").content | Out-File "createProtectionPolicy.ps1"; (Get-Content "createProtectionPolicy.ps1") | Set-Content "createProtectionPolicy.ps1"
(Invoke-WebRequest -Uri "$repoUrl/addVMProtectionJob.ps1").content | Out-File "addVMProtectionJob.ps1"; (Get-Content "addVMProtectionJob.ps1") | Set-Content "addVMProtectionJob.ps1"
(Invoke-WebRequest -Uri "$repoUrl/cohesity-api.ps1").content | Out-File cohesity-api.ps1; (Get-Content cohesity-api.ps1) | Set-Content cohesity-api.ps1
(Invoke-WebRequest -Uri "$repoUrl/vmlist.txt").content | Out-File vmlist.txt; (Get-Content vmlist.txt) | Set-Content vmlist.txt
(Invoke-WebRequest -Uri "$repoUrl/vmadds.txt").content | Out-File vmadds.txt; (Get-Content vmadds.txt) | Set-Content vmadds.txt
(Invoke-WebRequest -Uri "$repoUrl/vmadds2.txt").content | Out-File vmadds2.txt; (Get-Content vmadds2.txt) | Set-Content vmadds2.txt
(Invoke-WebRequest -Uri "$repoUrl/vmremove.txt").content | Out-File vmremove.txt; (Get-Content vmremove.txt) | Set-Content vmaremove.txt
```

# Creates a policy
```powershell
$policyname = '35 day retention'
$daystokeep = '3'
.\createProtectionPolicy.ps1 -vip cohesity.example.com -username admin -policyName "$policyname" -daysToKeep "$daystokeep"
```

# Creates a protection job
```powershell
# add vm name(s) to the vmlist.txt text file in the Cohesity Folder on the desktop
$protectionjob = 'all vms'
$jobstarttime = '14:30'
.\createVMProtectionJob.ps1 -vip cohesity.example.com -username admin -jobName "$protectionjob" -policyName "$policyname" -vCenterName vcenter.example.com -startTime "$jobstarttime" -vmlist ./vmlist.txt
```

# Add VM to a protection job
```powershell
# add vm name(s) to the vmadds.txt text file in the Cohesity Folder on the desktop
./addVMtoProtectionJob.ps1 -vip cohesity.example.com -username admin -jobName "$protectionjob" -vmNames (Get-Content ./vmadds.txt)
```

# Add another VM to a proteciton job
```powershell
# add additional vm name(s) to the vmadds2.txt text file in the Cohesity Folder on the desktop
./addVMtoProtectionJob.ps1 -vip cohesity.example.com -username admin -jobName "$protectionjob" -vmNames (Get-Content ./vmadds2.txt)
```

# Remove a VM from a proteciton job
```powershell
# add vm name(s) you want to remove to the vmremove.txt text file in the Cohesity Folder on the desktop
./unprotectVM.ps1 -vip cohesity.example.com -username admin -domain local -vmName (Get-Content ./vmremove.txt)
```