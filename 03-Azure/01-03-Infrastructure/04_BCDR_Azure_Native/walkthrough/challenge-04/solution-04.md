# Walkthrough Challenge 4 - Regional Disaster Recovery (DR)

[Previous Challenge Solution](../challenge-03/solution-03.md) - **[Home](../../Readme.md)** - [Next Challenge Solution](../challenge-05/solution-05.md)

⏰ Duration: 1 hour

## Solution Overview

This challenge focuses on implementing zone-to-zone disaster recovery within a single Azure region using Azure Site Recovery. You will configure replication between Availability Zones in Germany West Central and simulate a failover to demonstrate DR capabilities within the same region.

## Prerequisites

Ensure the lab environment from Challenge 2 is successfully deployed with:
- Linux VM (`mh-linux`) deployed in Germany West Central in a specific Availability Zone
- Recovery Services Vault in Germany West Central

## Task 1: Set up disaster recovery for the Linux VM across Availability Zones

> **Note:** To enable disaster recovery (DR) between Availability Zones, you might need to grant the Recovery Services Vault appropriate **access permissions**. If needed, follow the instructions below.

<details>
<summary>💡 How-to: Access permissions for Disaster Recovery (DR)</summary>
<br>

### Enable System Managed Identity for the Recovery Services Vault

Navigate to the **Recovery Services Vault** in the Primary Region (Germany West Central) and select the **Identity** tab.

**Status:** On
![image](./img/066.png)

✅ System-assigned managed identity successfully enabled!

#### Assign Required Azure Roles

Click **Azure role assignments** to begin configuring permissions.

![image](./img/067.png)

Click **Add role assignment** to add the first required role.

![image](./img/068.png)

#### Role Assignment 1: Storage Blob Data Contributor

**Select scope:**
- Choose the specific Resource Group or a larger scope (e.g. your subscription) where disaster recovery will operate.

**Select Role:** ["Storage Blob Data Contributor"](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/storage#storage-blob-data-contributor)

![image](./img/068a.png)

#### Role Assignment 2: Contributor

Click **Add role assignment** again to add the second required role.

![image](./img/068b.png)

**Select scope:**
- Use the same scope as the previous role assignment.

**Select Role:** ["Contributor"](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/privileged#contributor)

![image](./img/068c.png)

✅ Successfully assigned all required permissions for disaster recovery (DR)!

![image](./img/069.png)

</details>
<br>

### Enable Zone-to-Zone Disaster Recovery

1. Navigate to the Linux VM in Germany West Central. Go to **Disaster recovery** in the left menu.
2. Choose a different Availability Zone than the current one as **Target**.

   ![image](./img/071.png)

3. Review and start replication.

   ![image](./img/074.png)

4. Wait until the replication is finished.

   ![image](./img/075.png)

5. The Linux Virtual Machine is protected with Azure Site Recovery between Availability Zones.

   ![image](./img/076.png)

> **Note:** Zone-to-zone DR protects against datacenter-level failures within a region by replicating your VM to a different Availability Zone in the same region.

## Task 2: Simulate a zone-to-zone failover

### Perform Test Failover

1. Navigate to the Linux VM's Disaster Recovery blade
2. Select **Test failover** from the top menu

   ![image](./img/077.png)

3. Choose a recovery point (typically "Latest" is selected by default) and select the target virtual network in the same region

   ![image](./img/078.png)

4. Start the test failover

   ![image](./img/079.png)

### Monitor Failover Progress

1. Navigate to **Site Recovery jobs** in the Recovery Services Vault
2. Monitor the test failover job

   ![image](./img/080.png)

3. Verify a test VM is created in the target Availability Zone

   ![image](./img/081.png)

### Cleanup Test Failover

1. Return to the Disaster Recovery blade
2. Select **Cleanup test failover**
3. Add notes about the test results
4. Complete the cleanup to remove the test VM

   ![image](./img/082.png)

   ![image](./img/083.png)

## Success Criteria Validation ✅

Confirm you have completed:
- ✅ Enabled disaster recovery for the Linux VM between Availability Zones
- ✅ Successfully performed a test failover to another Availability Zone
- ✅ Validated the test VM functionality
- ✅ Cleaned up the test failover resources

You have successfully completed Challenge 4! 🚀

## Additional Notes

**Zone-to-Zone DR Benefits:**
- Protection against datacenter-level failures
- Lower latency than region-to-region replication
- Same-region data residency compliance
- Faster failover and failback operations

**Best Practices:**
- Regularly test failover to ensure DR readiness
- Monitor replication health continuously
- Document recovery procedures
- Update recovery plans as infrastructure changes

---

## Troubleshooting & FAQ

### Error: Installing Mobility Service and Preparing Target

**Error ID:** `151192`

**Error Message:**  
```
Site recovery configuration failed.
```

**Possible Causes:**  
Connection cannot be established to Office 365 authentication and identity IPv4 endpoints.

**Resolution:**  
Allow outbound access to required Azure Site Recovery endpoints in your **Network Security Group (NSG)**, **firewall**, or **proxy** settings.
- Use service tags like `AzureActiveDirectory` and `Office365` for NSG rules.

**Related Resources:**  
- [Azure Site Recovery - Firewall and Proxy Guidance](https://aka.ms/a2a-firewall-proxy-guidance)

