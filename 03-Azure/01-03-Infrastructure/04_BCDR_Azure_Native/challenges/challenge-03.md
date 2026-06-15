# Challenge 3 - Regional Protection (Backup)

[Previous Challenge](challenge-02.md) - **[Home](../Readme.md)** - [Next Challenge](challenge-04.md)

### Goal 🎯

* Protect in Azure - **Backup / Restore**

In this challenge, you will learn how to back up and restore a Linux Virtual Machine and an Azure Blob Storage using Azure Backup and Azure Backup Vaults.

![Azure VM backup architecture](../img/c3-azure-vm-backup-architecture.png)

## Actions

### Protect in Azure - Backup / Restore
1. Enable Azure Backup for the Linux VM in the primary region.
2. Enable Azure Backup for Azure Blob Storage.
   > [!IMPORTANT]
   > The storage account deployed in Challenge 2 is **empty**. Azure Backup for blobs requires at least one **container** to exist in the storage account. **Before enabling backup, create a blob container** (e.g. named `backup-demo`) in the storage account, otherwise you will not be able to configure protection.
   >
   > 💡 See [Create a container](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container) for step-by-step guidance.
3. Restore a VM in Azure.
4. Optional: Restore a file vom Azure Blob.

> [!IMPORTANT]
> To enable backup for the storage account, you need to grant the Backup Vault appropriate **access permissions**.
> 
> 💡 **Need help?** Refer to the [step-by-step guidance](../walkthrough/challenge-03/solution-03.md#task-2-enable-azure-backup-for-blobs) in the solution.

### Success Criteria ✅

- You have successfully set up Azure Backup for a virtual machine.
- You have successfully restored a VM on Azure.
- Optional: You have successfully restored an Azure Blob Storage container.

### 📚 Learning Resources

- [Quickstart: Back up a VM with the Azure portal](https://learn.microsoft.com/en-us/azure/backup/quick-backup-vm-portal)
- [Apply a backup policy](https://learn.microsoft.com/en-us/azure/backup/quick-backup-vm-portal#apply-a-backup-policy)
- [Tutorial: Back up multiple VMs at scale](https://learn.microsoft.com/en-us/azure/backup/tutorial-backup-vm-at-scale)
- [Restore VMs from Azure Backup](https://learn.microsoft.com/en-us/azure/backup/backup-azure-arm-restore-vms)
- [Restore encrypted virtual machines](https://learn.microsoft.com/en-us/azure/backup/restore-azure-encrypted-virtual-machines)
- [Azure Blob Storage: Backup & Restore](https://learn.microsoft.com/en-us/azure/backup/blob-backup-overview)
