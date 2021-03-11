# **** COHESITY TRAINING LAB ****

# Internet Explorer setup
For the download commands below to work, you need to open up Internet Explorer (not Edge) one time and complete the setup steps.  Close the browser once complete.

# Powershell setup
```powershell
# Need to run PowerShell as an Administrator (right-click and select run-as administrator) then run the following command.
Set-ExecutionPolicy Unrestricted
# Type A for all and hit enter
```

# Download PowerShell Scripts and files
```powershell
# paste the below into powershell and execute, note - you need to make sure you've launched Internet Explorer at least once on the system, even if you don't use IE as you need to get passed the "First Run" wizard.
cd c:\users\admin\desktop
md cohesity
cd cohesity
$repoURL = 'https://raw.githubusercontent.com/justinrfield/cohesitytraininglab/main'
(Invoke-WebRequest -Uri "$repoUrl/createVMProtectionJob.ps1").content | Out-File "createVMProtectionJob.ps1"; (Get-Content "createVMProtectionJob.ps1") | Set-Content "createVMProtectionJob.ps1"
(Invoke-WebRequest -Uri "$repoUrl/createProtectionPolicy.ps1").content | Out-File "createProtectionPolicy.ps1"; (Get-Content "createProtectionPolicy.ps1") | Set-Content "createProtectionPolicy.ps1"
(Invoke-WebRequest -Uri "$repoUrl/addVMtoProtectionJob.ps1").content | Out-File "addVMtoProtectionJob.ps1"; (Get-Content "addVMtoProtectionJob.ps1") | Set-Content "addVMtoProtectionJob.ps1"
(Invoke-WebRequest -Uri "$repoUrl/unprotectVM.ps1").content | Out-File "unprotectVM.ps1"; (Get-Content "unprotectVM.ps1") | Set-Content "unprotectVM.ps1"
(Invoke-WebRequest -Uri "$repoUrl/cohesity-api.ps1").content | Out-File cohesity-api.ps1; (Get-Content cohesity-api.ps1) | Set-Content cohesity-api.ps1
(Invoke-WebRequest -Uri "$repoUrl/vmlist.txt").content | Out-File vmlist.txt; (Get-Content vmlist.txt) | Set-Content vmlist.txt
(Invoke-WebRequest -Uri "$repoUrl/vmadds.txt").content | Out-File vmadds.txt; (Get-Content vmadds.txt) | Set-Content vmadds.txt
(Invoke-WebRequest -Uri "$repoUrl/vmadds2.txt").content | Out-File vmadds2.txt; (Get-Content vmadds2.txt) | Set-Content vmadds2.txt
(Invoke-WebRequest -Uri "$repoUrl/vmremove.txt").content | Out-File vmremove.txt; (Get-Content vmremove.txt) | Set-Content vmremove.txt
(Invoke-WebRequest -Uri "$repoUrl/registerPhysical.ps1").content | Out-File registerPhysical.ps1; (Get-Content registerPhysical.ps1) | Set-Content registerPhysical.ps1
(Invoke-WebRequest -Uri "$repoUrl/reg-physical-servers.txt").content | Out-File reg-physical-servers.txt; (Get-Content reg-physical-servers.txt) | Set-Content reg-physical-servers.txt
(Invoke-WebRequest -Uri "$repoUrl/addPhysicalToProtectionJob.ps1").content | Out-File "addPhysicalToProtectionJob.ps1"; (Get-Content "addPhysicalToProtectionJob.ps1") | Set-Content "addPhysicalToProtectionJob.ps1"
(Invoke-WebRequest -Uri "$repoUrl/add-physicals-to-job.txt").content | Out-File add-physicals-to-job.txt; (Get-Content add-physicals-to-job.txt) | Set-Content add-physicals-to-job.txt
```

# Creates a policy
```powershell
# configure the variables below and then copy/paste the lines below into your powershell window and execute
$policyname = '35 day retention'
$daystokeep = '3'
$clustervip = 'enter the cluster vip you want to connect to here between single quotes'
$clusterusername = 'enter username here between single quotes'
.\createProtectionPolicy.ps1 -vip "$clustervip" -username "$clusterusername" -policyName "$policyname" -daysToKeep "$daystokeep"
```

# Creates a protection job for VMs
```powershell
# add vm name(s) to the vmlist.txt text file in the Cohesity Folder on the desktop
# configure the variables below and then copy/paste the lines below into your powershell window and execute
$protectionjob = 'all vms'
$jobstarttime = '14:30'
$clustervip = 'enter the cluster vip you want to connect to here between single quotes'
$clusterusername = 'enter username here between single quotes'
$vcentername = 'enter vCenter name here between single quotes'
.\createVMProtectionJob.ps1 -vip "$clustervip" -username "$clusterusername" -jobName "$protectionjob" -policyName "$policyname" -vCenterName "$vcentername" -startTime "$jobstarttime" -vmlist ./vmlist.txt
```

# Add VM to a protection job
```powershell
# add vm name(s) to the vmadds.txt text file in the Cohesity Folder on the desktop
# configure the variables below and then copy/paste the lines below into your powershell window and execute
$clustervip = 'enter the cluster vip you want to connect to here between single quotes'
$clusterusername = 'enter username here between single quotes'
./addVMtoProtectionJob.ps1 -vip "$clustervip" -username "$clusterusername" -jobName "$protectionjob" -vmNames (Get-Content ./vmadds.txt)
```

# Add another VM to a proteciton job
```powershell
# add additional vm name(s) to the vmadds2.txt text file in the Cohesity Folder on the desktop
# configure the variables below and then copy/paste the lines below into your powershell window and execute
$clustervip = 'enter the cluster vip you want to connect to here between single quotes'
$clusterusername = 'enter username here between single quotes'
./addVMtoProtectionJob.ps1 -vip "$clustervip" -username "$clusterusername" -jobName "$protectionjob" -vmNames (Get-Content ./vmadds2.txt)
```

# Remove a VM from a proteciton job
```powershell
# add vm name(s) you want to remove to the vmremove.txt text file in the Cohesity Folder on the desktop
# configure the variables below and then copy/paste the lines below into your powershell window and execute
$clustervip = 'enter the cluster vip you want to connect to here between single quotes'
$clusterusername = 'enter username here between single quotes'
$domainname = 'enter domain name for servers here between single quotes'
./unprotectVM.ps1 -vip "$clustervip" -username "$clusterusername" -domain "$domainname" -vmName (Get-Content ./vmremove.txt)
```

# Register a list of Physical servers to a cluster
```powershell
# add the physical server name(s) you want to add to a specific cluster to the reg-physical-servers.txt text file in the Cohesity Folder on the desktop
# configure the variables below and then copy/paste the lines below into your powershell window and execute
$clustervip = 'enter the cluster vip you want to connect to here between single quotes'
$clusterusername = 'enter username here between single quotes'
$domainname = 'enter domain name for servers here between single quotes'
./registerPhysical.ps1 -vip "$clustervip" -username "$clusterusername" -domain "$domainname" -serverList ./reg-physical-servers.txt
```

# Add a list of Physical servers to an existing protection job on a specified cluster
```powershell
# add the physical server name(s) you want to add to an existing protection job to the add-physicals-to-job.txt text file in the Cohesity Folder on the desktop
# configure the variables below and then copy/paste the lines below into your powershell window and execute
$clustervip = 'enter the cluster vip you want to connect to here between single quotes'
$clusterusername = 'enter username here between single quotes'
$existingjobname = 'enter existing job name you want to add the servers to here between single quotes'
$domainname = 'enter domain name for servers here between single quotes'
./addPhysicalToProtectionJob.ps1 -vip "$clustervip" -username "$clusterusername" -domain "$domainname" -jobName "$existingjobname" -serverList ./add-physicals-to-job.txt
```