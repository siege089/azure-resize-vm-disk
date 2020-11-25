# Resize Azure VM Disks

## Shrink OS Disk
### Prereqs
* Must have [Powershell AZ Module](https://docs.microsoft.com/en-us/powershell/azure/new-azureps-module-az?view=azps-5.1.0) installed.
* Must have a windows based Azure VM

### Steps
* RDP into VM
* Open Computer Management
* Open Disk Management
* Check the amount of free space on the Disk containing your C: drive and make note of the disknumber
* Open Powershell as an Administrator
* Run the following command
```powershell
Get-Partition -DiskNumber <disknumber>
```
* Make note of partition containing your C: drive and run the following command choose a size that will make the entire disk smaller than an option in Azure
  * Here I'm resizing to 32GB and leaving room for the System Reserved partition on the same disk
```powershell
Get-Partition -DiskNumber <disknumber> -PartitionNumber <partitionnumber> | Resize-Partition -Size 31GB
```
* Edit the variables at the top of the `Shrink-VM-OSDisk.ps1` script
  * $DiskID
    * Click on Disks
    * Click on the OS Disk
    * Click on Properties
    * Copy entire Resource ID
  * $VMName
  * $DiskSizeGB
  * $AzSubscription
    * Name of your Azure Subscription
  * $Encrypted
    * $True or $False
    * If $True VM will need to be running and you will be prompted to confirm the removal of the Disk Encryption extension and to disable encryption. After resize you can manually reenable.
* Make sure you are connected with the [Connect-AzAccount](https://docs.microsoft.com/en-us/powershell/module/az.accounts/connect-azaccount?view=azps-5.1.0) command
* Run `Shrink-VM-OSDisk.ps1` script
* Once complete login to the VM and using Disk Managment extend the C: drive to take up any remaining space.
