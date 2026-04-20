# Walkthrough Challenge 5 - Disaster Recovery (DR) across Azure Regions

[Previous Challenge Solution](../challenge-04/solution-04.md) - **[Home](../../Readme.md)** - [Next Challenge Solution](../challenge-06/solution-06.md)

⏰ Duration: 1 hour

## Solution Overview

This challenge focuses on implementing cross-region disaster recovery using Azure Site Recovery. You will replicate web VMs from Germany West Central (primary) to Sweden Central (secondary) and perform both test and production failovers to demonstrate DR capabilities across Azure regions.

## Prerequisites

Ensure the lab environment from Challenge 2 is successfully deployed with:
- Web VMs (`mh-web1` and `mh-web2`) in Germany West Central
- Recovery Services Vault in Sweden Central (`mh-swedencentral-asrvault`)
- Target resource group in Sweden Central

## Task 1: Set up and enable disaster recovery with Azure Site Recovery

> **Note:** To enable inter-regional disaster recovery (DR), you might need to grant the Recovery Services Vault appropriate **access permissions**. If needed, follow the instructions below.

<details>
<summary>💡 How-to: Access permissions for inter-regional Disaster Recovery (DR)</summary>
<br>

### Enable System Managed Identity for the Recovery Services Vault

Navigate to the **Recovery Services Vault** in the **Secondary Region** (Sweden Central) and select the **Identity** tab.

**Status:** On
![image](./img/040.png)

✅ System-assigned managed identity successfully enabled!

#### Assign Required Azure Roles

Click **Azure role assignments** to begin configuring permissions.

Click **Add role assignment** to add the first required role.
![image](./img/041.png)

#### Role Assignment 1: Storage Blob Data Contributor

**Select scope:**
- Choose the specific Resource Groups (primary + secondary regions) or a larger scope (e.g. your subscription) where disaster recovery will operate.

**Select Role:** ["Storage Blob Data Contributor"](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/storage#storage-blob-data-contributor)

![image](./img/042.png)

#### Role Assignment 2: Contributor

Click **Add role assignment** again to add the second required role.

![image](./img/043.png)

**Select scope:**
- Use the same scope as the previous role assignment.

**Select Role:** ["Contributor"](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles/privileged#contributor)

![image](./img/044.png)

✅ Successfully assigned all required permissions for multi-region disaster recovery (DR)!

![image](./img/045.png)

</details>
<br>

### Enable Replication for Web VMs

**Method 1: From Recovery Services Vault**

1. Navigate to the Recovery Services Vault in Sweden Central (`mh-swedencentral-asrvault`). In **Protected Items**, select **Replicated Items**. Then select **Replicate** and from the dropdown list select **Azure virtual machines**.

   ![image](./img/001.png)

2. Configure replication settings:
   - **Source region**: Germany West Central
   - **Source resource group**: Select the resource group containing web VMs

   ![image](./img/002.png)

3. Select the Virtual machines: `mh-web1` and `mh-web2`

   ![image](./img/003.png)

4. Configure replication settings:
   - **Target location**: Sweden Central
   - **Target resource group**: Select the target resource group in Sweden Central

   ![image](./img/004.png)

   ![image](./img/005.png)

5. Review replication settings and enable replication

   ![image](./img/006.png)

6. In the deployment notification, navigate to the Site Recovery Jobs to monitor progress.

   ![image](./img/007.png)

7. Select in-progress jobs to check the status and progress. This task can take up to 10 minutes to finish.

   ![image](./img/008.png)

   ![image](./img/009.png)

8. Verify replication status shows as "Healthy" for both VMs.

   ![image](./img/011.png)

   ![image](./img/010.png)

**Method 2: From Virtual Machine (Alternative)**

1. Navigate to a web VM (e.g., `mh-web1`)
2. Select **Disaster recovery** from the left menu
3. Configure the target region as Sweden Central
4. Review and enable replication

   ![image](./img/100.png)

## Task 2: Create a recovery plan and run a test failover

### Create a Recovery Plan

1. Navigate to the Recovery Services Vault in Sweden Central. Under **Manage**, select **Recovery Plans (Site Recovery)** and create a recovery plan.

   ![image](./img/09.png)

2. Select `mh-web1` and `mh-web2` as the protected source machines and create the recovery plan.

   ![image](./img/10.png)

### Run a Test Failover

1. Navigate to the recovery plan created in the previous task.

   ![image](./img/11.png)

2. From the top menu select **Test failover**. Choose a recovery point and select the target virtual network in Sweden Central.

   ![image](./img/12.png)

   ![image](./img/13.png)

### Monitor Test Failover Progress

1. Navigate to **Site Recovery jobs** and select the test failover job which is in progress.

   ![image](./img/14.png)

   ![image](./img/15.png)

   ![image](./img/16.png)

2. After all jobs are finished successfully, navigate to the Virtual Machines list. New Virtual Machines have been created in the Sweden Central Region.

   ![image](./img/17.png)

### Cleanup Test Failover

1. Return to the recovery plan and click **Cleanup test failover**.

   ![image](./img/18.png)

   ![image](./img/19.png)

2. Add notes documenting the test results and complete the cleanup.

   ![image](./img/20.png)

   ![image](./img/21.png)

   ![image](./img/22.png)

## Task 3: Run a production failover

### Initiate Production Failover

1. Navigate to the recovery plan and click **Failover** from the top menu.

   ![image](./img/23.png)

2. Configure failover settings:
   - **Failover direction**: From Germany West Central to Sweden Central
   - **Recovery point**: Select the desired recovery point
   - **Shut down machines before beginning failover**: Check this option if possible (recommended)

   ![image](./img/24.png)

3. Confirm and start the failover.

   ![image](./img/25.png)

   ![image](./img/26.png)

### Verify Failover Completion

1. Check the virtual machine list. There are new virtual machines running in Sweden Central region.

   ![image](./img/27.png)

### Commit and Reprotect

After verifying the failover was successful:

1. Return to the recovery plan and click **Commit** to finalize the failover.

   ![image](./img/28.png)

2. **Reprotect** the virtual machines to enable reverse replication back to Germany West Central.

   ![image](./img/29.png)

   ![image](./img/30.png)

   ![image](./img/31.png)

3. Monitor the reprotection progress until synchronization is complete.

   ![image](./img/32.png)

   ![image](./img/33.png)

## Success Criteria Validation ✅

Confirm you have completed:
- ✅ Enabled replication for `mh-web1` and `mh-web2` to Sweden Central
- ✅ Created a recovery plan for the web application
- ✅ Successfully performed a test failover with zero production impact
- ✅ Cleaned up test failover resources
- ✅ Completed a production failover to Sweden Central
- ✅ Verified VMs are running in the secondary region

You have successfully completed Challenge 5! 🚀

## Additional Notes

**Cross-Region DR Best Practices:**
- Test failover regularly to ensure DR readiness
- Document RTO (Recovery Time Objective) and RPO (Recovery Point Objective)
- Keep recovery plans up to date with infrastructure changes
- Consider network connectivity and dependencies during failover
- Plan for failback procedures after regional recovery

**Important Considerations:**
- Production failover will stop the source VMs (if shutdown option is selected)
- After failover, you may need to reconfigure networking, DNS, and load balancers
- Commit the failover only after thorough validation in the target region
- Plan for reprotection if you want to enable failback capability
