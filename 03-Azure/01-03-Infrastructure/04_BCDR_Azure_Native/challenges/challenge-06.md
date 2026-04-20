# Challenge 6 - Restore Web Application and verify Azure Storage DR

[Previous Challenge](challenge-05.md) - **[Home](../Readme.md)** - [Next Challenge](challenge-07.md)

### Goal 🎯

In this challenge 6, you will re-establish the connection to your web application from the failed-over region and then test disaster recovery for an Azure Storage account with GRS enabled. The primary objective is to ensure business continuity by protecting critical data stored in Azure storage accounts against potential disasters.

After the failover, the VMs are running in the secondary region but the load balancer backend pool needs to be updated to point to them.

![Architecture after failover](../walkthrough/challenge-06/img/asrdemo%20architecture.png)

### Actions
* Task 1: Re-establish your connection to the Web Application from the secondary region.
  * Add your failed-over Virtual Machines in the secondary region to the backend pool of your Load Balancer.
  * Test the connection to the Web Application.
  * High Availability & SLA Discussion: Use Azure Copilot to calculate the composite SLAs for your application.
* Task 2: Disaster Recovery for Azure Storage Account.
  * Verify the configuration of the Azure Storage Account redudancy, and confirm GRS is enabled and data is replicated to a secondary region. Which region is used as secondary region?
  * Perform a failover test for the storage account to validate the disaster recovery setup.

### Calculate Composite SLAs with Azure Copilot

After re-establishing your web application, use Azure Copilot to learn about composite SLAs.

#### Example Prompts

```
What are the SLAs for my Azure resources?
```

```
How do I calculate the composite SLA for my application?
```

<details close>
<summary>💡 Hint: Composite SLA Calculation</summary>
<br>

1. Identify the Azure services (components) that are connected.
2. Determine the chains of components within the application.
3. Use the latest SLA provided by Microsoft in [Service Level Agreements (SLA) for Online Services](https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services?lang=1&year=2024) to find the SLA for each component in the chain.
4. Multiply the SLA values of each individual component (link) in the chain to get the composite SLA for that chain.
5. Identify the weakest link – the component/composites with the lowest SLA.

</details>


<details close>
<summary>💡 Storage Account with GRS enabled</summary>
<br>

![grs1](./exploration/5.png)
![grs2](./exploration/6.png)
![grs3](./exploration/7.png)

</details>

---

### Success Criteria ✅
* You have successfully re-established connection to your web application from the secondary region.
* You have verified that the Azure Storage Account has GRS enabled and identified the secondary region used for data replication.
* You have successfully performed a failover test for the Azure Storage Account.

### 📚 Learning Resources
* [Geo-redundant storage (GRS) for cross-regional durability](https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy-grs)
* [Disaster recovery and storage account failover](https://learn.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance)
