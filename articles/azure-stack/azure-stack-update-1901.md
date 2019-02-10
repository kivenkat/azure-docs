---
title: Azure Stack 1901 update | Microsoft Docs
description: Learn about the 1901 update for Azure Stack integrated systems, including what's new, known issues, and where to download the update.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''

ms.assetid:  
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: sethm
ms.reviewer: adepue

---

# Azure Stack 1901 update

*Applies to: Azure Stack integrated systems*

This article describes the contents of the 1901 update package. The update includes improvements, fixes, and new features for this version of Azure Stack. This article also describes known issues in this release, and includes a link to download the update. Known issues are divided into issues directly related to the update process, and issues with the build (post-installation).

> [!IMPORTANT]  
> This update package is only for Azure Stack integrated systems. Do not apply this update package to the Azure Stack Development Kit.

## Build reference

The Azure Stack 1901 update build number is **1.1901.0.95**.

## Hotfixes

Azure Stack releases hotfixes on a regular basis. Be sure to install the [latest Azure Stack hotfix](#azure-stack-hotfixes) for 1811 before updating Azure Stack to 1901.

Azure Stack hotfixes are only applicable to Azure Stack integrated systems; do not attempt to install hotfixes on the ASDK.

> [!TIP]  
> Subscribe to the following *RSS* or *Atom* feeds to keep up with Azure Stack hotfixes:
> - [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss)
> - [Atom](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom)

### Azure Stack hotfixes

- **1809**: [KB 4481548 – Azure Stack hotfix 1.1809.12.114](https://support.microsoft.com/help/4481548/)
- **1811**: No current hotfix available.
- **1901**: No current hotfix available.

## Prerequisites

> [!IMPORTANT]
- Install the [latest Azure Stack hotfix](#azure-stack-hotfixes) for 1811 (if any) before updating to 1901.

- Before you start installation of this update, run [Test-AzureStack](azure-stack-diagnostic-test.md) with the following parameters to validate the status of your Azure Stack and resolve any operational issues found, including all warnings and failures. Also review active alerts, and resolve any that require action:

    ```PowerShell
    Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary, AzsHostingServiceCertificates
    ```

## New features

This update includes the following new features and improvements for Azure Stack:

- Managed images on Azure Stack enable you to create a managed image object on a generalized VM (both unmanaged and managed) that can only create managed disk VMs going forward. For more information, see [Azure Stack Managed Disks](user/azure-stack-managed-disk-considerations.md#managed-images).

## Fixed issues

- Fixed an issue in which the portal showed an option to create policy-based VPN gateways, which are not supported in Azure Stack. This option has been removed from the portal.

<!-- 16523695 – IS, ASDK -->
- Fixed an issue in which after updating your DNS Settings for your Virtual Network from **Use Azure Stack DNS** to **Custom DNS**, the instances were not updated with the new setting.

- <!-- 3235634 – IS, ASDK -->
Fixed an issue in which deploying VMs with sizes containing a **v2** suffix; for example, **Standard_A2_v2**, required specifying the suffix as **Standard_A2_v2** (lowercase v). As with global Azure, you can now use **Standard_A2_V2** (uppercase V).

<!--  2795678 – IS, ASDK --> 
- Fixed an issue that produced a warning when you used the portal to create virtual machines (VMs) in a premium VM size (DS,Ds_v2,FS,FSv2). The VM was created in a standard storage account. Although this did not affect functionally, IOPs, or billing, the warning has been fixed.

<!-- 1264761 - IS ASDK -->  
- Fixed an issue with the **Health controller** component that was generating the following alerts. The alerts could be safely ignored:

    - Alert #1:
       - NAME:  Infrastructure role unhealthy
       - SEVERITY: Warning
       - COMPONENT: Health controller
       - DESCRIPTION: The health controller Heartbeat Scanner is unavailable. This may affect health reports and metrics.  

    - Alert #2:
       - NAME:  Infrastructure role unhealthy
       - SEVERITY: Warning
       - COMPONENT: Health controller
       - DESCRIPTION: The health controller Fault Scanner is unavailable. This may affect health reports and metrics.


<!-- 3507629 - IS, ASDK --> 
- Fixed an issue when setting the value of Managed Disks quotas under [compute quota types](azure-stack-quota-types.md#compute-quota-types) to 0, it is equivalent to the default value of 2048 GiB. The zero quota value now is respected.

<!-- 2724873 - IS --> 
- Fixed an issue when using the PowerShell cmdlets **Start-AzsScaleUnitNode** or  **Stop-AzsScaleunitNode** to manage scale units, in which the first attempt to start or stop the scale unit might fail.

<!-- 2724961- IS ASDK --> 
- Fixed an issue in which you registered the **Microsoft.Insight** resource provider in the subscription settings, and created a Windows VM with Guest OS Diagnostic enabled, but the CPU Percentage chart in the VM overview page did not show metrics data. The data now correctly displays.

- Fixed an issue in which running the **Get-AzureStackLog** cmdlet failed after running **Test-AzureStack** in the same privileged endpoint (PEP) session. You can now use the same PEP session in which you executed **Test-AzureStack**.

<!-- bug 3615401, IS -->
- Fixed issue with automatic backups where the scheduler service would go into disabled state unexpectedly. 

<!--2850083, IS ASDK -->
- Removed the **Reset Gateway** button from the Azure Stack portal, which threw an error if the button was clicked. This button serves no function in Azure Stack, as Azure Stack has a multi-tenant gateway rather than dedicated VM instances for each tenant VPN Gateway, so it was removed to prevent confusion. 

<!-- 3209594, IS ASDK -->
- Removed the **Effective Security Rules** link from the **Networking Properties** blade as this feature is not supported in Azure Stack. Having the link present gave the impression that this feature was supported but not working. To alleviate confusion, we removed the link.

<!-- 3139614 | IS -->
- Fixed an issue in which after an update was applied to Azure Stack from an OEM, the **Update available** notification did not appear in the Azure Stack administrator portal.

## Changes

<!-- 3083238 IS -->
- Security enhancements in this update result in an increase in the backup size of the directory service role. For updated sizing guidance for the external storage location, see the [infrastructure backup documentation](azure-stack-backup-reference.md#storage-location-sizing). This change results in a longer time to complete the backup due to the larger size data transfer. This change impacts integrated systems. 

- Starting in January 2019, you can deploy Kubernetes clusters on Active Directory Federated Services (AD FS) registered, connected Azure Stack stamps (internet access is required). Follow the instructions [here](azure-stack-solution-template-kubernetes-cluster-add.md) to download the new Kubernetes Marketplace item. Follow the instructions [here](user/azure-stack-solution-template-kubernetes-adfs.md) to deploy a Kubernetes cluster. Note the new parameters for indicating whether the target system is ADD or AD FS registered. If it is AD FS, new fields are available to enter the Key Vault parameters in which the deployment certificate is stored.

   Note that even with AD FS support, the deployment of Kubernetes clusters requires internet access.

- After installing updates or hotfixes to Azure Stack, new features may be introduced which require new permissions to be granted to one or more identity applications. Granting these permissions requires administrative access to the home directory, and so it cannot be done automatically. For example:

   ```powershell
   $adminResourceManagerEndpoint = "https://adminmanagement.<region>.<domain>"
   $homeDirectoryTenantName = "<homeDirectoryTenant>.onmicrosoft.com" # This is the primary tenant Azure Stack is registered to

   Update-AzsHomeDirectoryTenant -AdminResourceManagerEndpoint $adminResourceManagerEndpoint `
     -DirectoryTenantName $homeDirectoryTenantName -Verbose
   ```

- There are currently extensions in Azure Stack that successfully deploy without explicitly needing to download the extensions through marketplace syndication. The following versions of these extensions are being removed. Azure Stack operators must now explicitly syndicate these extensions from the Azure Stack marketplace:

   | Type                     | Version        |
   |--------------------------|----------------|
   | DSC                      | 2.19.0.0       |
   | IaaSAntimalware          | 1.4.0.0        |
   | BGInfo                   | 2.1            |
   | VMAccessAgent            | 2.0            |
   | CustomScriptExtension    | 1.8            |
   | MicrosoftMonitoringAgent | 1.0.10900.0    |
   | IaaSDiagnostics          | 1.10.1.1       |
   | VMAccessForLinux         | 1.4.0.0        |
   | CustomScriptForLinux     | 1.5.2.0        |
   | DockerExtension          | 1.1.1606092330 |
   | JsonADDomainExtension    | 1.3            |
   | OSPatchingForLinux       | 2.3.0.1        |
   | WebRole                  | 4.3000.14.0    |

   It is recommended that when deploying extensions, Azure Stack users set `autoUpgradeMinorVersion` to **true**. For example:

   ```json
   "type": "Extension",
           "publisher": "ExtensionPublisher",
           "typeHandlerVersion": "1.2",
           "autoUpgradeMinorVersion": "true"
   ```

- There is a new consideration for accurately planning Azure Stack capacity. We have set limits on the total number of VMs that can be deployed within Azure Stack, to ensure all of our internal services fulfill the scale at which customers run. The limit is 60 VMs per host, with a maximum of 700 for the entire stamp (if the 60 per host limit is reached). For more information, see the [new release of the capacity planner](http://aka.ms/azstackcapacityplanner).

- The Compute API version has increased to 2017-12-01.

- Infrastructure backup now requires a certificate with a public key only (.CER) for encryption of backup data. Symmetric encryption key support is deprecated starting in 1901. If infrastructure backup is configured before updating to 1901, the encryption keys will remain in place. You will have at least 2 more updates with backwards compatibility support to update backup settings. For more information, see [Azure Stack infrastructure backup best practices](azure-stack-backup-best-practices.md).

## Common vulnerabilities and exposures

This update installs the following security updates:  

- [CVE-2018-8477](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8477)
- [CVE-2018-8514](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8514)
- [CVE-2018-8580](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8580)
- [CVE-2018-8595](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8595)
- [CVE-2018-8596](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8596)
- [CVE-2018-8598](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8598)
- [CVE-2018-8621](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8621)
- [CVE-2018-8622](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8622)
- [CVE-2018-8627](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8627)
- [CVE-2018-8637](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8637)
- [CVE-2018-8638](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8638)
- [ADV190001](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190001)
- [CVE-2019-0536](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0536)
- [CVE-2019-0537](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0537)
- [CVE-2019-0545](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0545)
- [CVE-2019-0549](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0549)
- [CVE-2019-0553](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0553)
- [CVE-2019-0554](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0554)
- [CVE-2019-0559](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0559)
- [CVE-2019-0560](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0560)
- [CVE-2019-0561](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0561)
- [CVE-2019-0569](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0569)
- [CVE-2019-0585](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0585)
- [CVE-2019-0588](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2019-0588)


For more information about these vulnerabilities, click on the preceding links, or see Microsoft Knowledge Base articles [4480977](https://support.microsoft.com/en-us/help/4480977).

## Known issues with the update process

- When running [Test-AzureStack](azure-stack-diagnostic-test.md), if either the **AzsInfraRoleSummary** or the **AzsPortalApiSummary** test fails, you are prompted to run **Test-AzureStack** with the `-Repair` flag.  If you run this command, it fails with the following error message:  `Unexpected exception getting Azure Stack health status. Cannot bind argument to parameter 'TestResult' because it is null.`

- When you run [Test-AzureStack](azure-stack-diagnostic-test.md), a warning message from the Baseboard Management Controller (BMC) is displayed. You can safely ignore this warning.

- <!-- 2468613 - IS --> During installation of this update, you might see alerts with the title `Error – Template for FaultType UserAccounts.New is missing.`  You can safely ignore these alerts. The alerts close automatically after the installation of this update completes.

## Post-update steps

- After the installation of this update, install any applicable hotfixes. For more information, see [Hotfixes](#hotfixes), as well as our [Servicing Policy](azure-stack-servicing-policy.md).  

- Retrieve the data at rest encryption keys and securely store them outside of your Azure Stack deployment. Follow the [instructions on how to retrieve the keys](azure-stack-security-bitlocker.md).

## Known issues (post-installation)

The following are post-installation known issues for this build version.

### Portal

<!-- 2930820 - IS ASDK --> 
- In both the administrator and user portals, if you search for "Docker," the item is incorrectly returned. It is not available in Azure Stack. If you try to create it, a blade with an error indication is displayed. 

<!-- 2931230 – IS  ASDK --> 
- Plans that are added to a user subscription as an add-on plan cannot be deleted, even when you remove the plan from the user subscription. The plan will remain until the subscriptions that reference the add-on plan are also deleted. 

<!-- TBD - IS ASDK --> 
- The two administrative subscription types that were introduced with version 1804 should not be used. The subscription types are **Metering subscription**, and **Consumption subscription**. These subscription types are visible in new Azure Stack environments beginning with version 1804 but are not yet ready for use. You should continue to use the **Default Provider** subscription type.

<!-- 3557860 - IS ASDK --> 
- Deleting user subscriptions results in orphaned resources. As a workaround, first delete user resources or the entire resource group, and then delete the user subscriptions.

<!-- 1663805 - IS ASDK --> 
- You cannot view permissions to your subscription using the Azure Stack portals. As a workaround, use [PowerShell to verify permissions](/powershell/module/azs.subscriptions.admin/get-azssubscriptionplan).

<!-- ### Health and monitoring -->

### Compute

- When creating a new Windows Virtual Machine (VM), the following error may be displayed:

   `'Failed to start virtual machine 'vm-name'. Error: Failed to update serial output settings for VM 'vm-name'`

   The error occurs if you enable boot diagnostics on a VM but delete your boot diagnostics storage account. To work around this issue, recreate the storage account with the same name as you used previously.

<!-- 2967447 - IS, ASDK, to be fixed in 1902 -->
- The virtual machine scale set (VMSS) creation experience provides CentOS-based 7.2 as an option for deployment. Because that image is not available on Azure Stack, either select another operating system for your deployment, or use an Azure Resource Manager template specifying another CentOS image that has been downloaded prior to deployment from the marketplace by the operator.  

<!-- TBD - IS ASDK --> 
- After applying the 1901 update, you might encounter the following issues when deploying VMs with Managed Disks:

   - If the subscription was created before the 1808 update, deploying a VM with Managed Disks might fail with an internal error message. To resolve the error, follow these steps for each subscription:
      1. In the Tenant portal, go to **Subscriptions** and find the subscription. Select **Resource Providers**, then select **Microsoft.Compute**, and then click **Re-register**.
      2. Under the same subscription, go to **Access Control (IAM)**, and verify that **Azure Stack – Managed Disk** is listed.
   - If you have configured a multi-tenant environment, deploying VMs in a subscription associated with a guest directory might fail with an internal error message. To resolve the error, follow these steps in [this article](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) to reconfigure each of your guest directories.

- An Ubuntu 18.04 VM created with SSH authorization enabled will not allow you to use the SSH keys to log in. As a workaround, use VM access for the Linux extension to implement SSH keys after provisioning, or use password-based authentication.

### Networking  

<!-- 3239127 - IS, ASDK -->
- In the Azure Stack portal, when you change a static IP address for an IP configuration that is bound to a network adapter attached to a VM instance, you will see a warning message that states 

    `The virtual machine associated with this network interface will be restarted to utilize the new private IP address...`.

    You can safely ignore this message; the IP address will be changed even if the VM instance does not restart.

<!-- 3632798 - IS, ASDK -->
- In the portal, if you add an inbound security rule and select **Service Tag** as the source, several options are displayed in the **Source Tag** list that are not available for Azure Stack. The only options that are valid in Azure Stack are as follows:

    - **Internet**
    - **VirtualNetwork**
    - **AzureLoadBalancer**
  
    The other options are not supported as source tags in Azure Stack. Similarly, if you add an outbound security rule and select **Service Tag** as the destination, the same list of options for **Source Tag** is displayed. The only valid options are the same as for **Source Tag**, as described in the previous list.

- Network security groups (NSGs) do not work in Azure Stack in the same way as global Azure. In Azure, you can set multiple ports on one NSG rule (using the portal, PowerShell, and Resource Manager templates). In Azure Stack however, you cannot set multiple ports on one NSG rule via the portal. To work around this issue, use a Resource Manager template or PowerShell to set these additional rules.

<!-- 3203799 - IS, ASDK -->
- Azure Stack does not support attaching more than 4 Network Interfaces (NICs) to a VM instances today, regardless of the instance size.

<!-- ### SQL and MySQL-->

### App Service

<!-- 2352906 - IS ASDK --> 
- You must register the storage resource provider before you create your first Azure Function in the subscription.


<!-- ### Usage -->

 
<!-- #### Identity -->
<!-- #### Marketplace -->

## Download the update

You can download the Azure Stack 1901 update package from [here](https://aka.ms/azurestackupdatedownload). 

In connected scenarios only, Azure Stack deployments periodically check a secured endpoint and automatically notify you if an update is available for your cloud. For more information, see [managing updates for Azure Stack](azure-stack-updates.md#using-the-update-tile-to-manage-updates).

## Next steps

- For an overview of the update management in Azure Stack, see [Manage updates in Azure Stack overview](azure-stack-updates.md).  
- For more information about how to apply updates with Azure Stack, see [Apply updates in Azure Stack](azure-stack-apply-updates.md).
- To review the servicing policy for Azure Stack integrated systems, and what you must do to keep your system in a supported state, see [Azure Stack servicing policy](azure-stack-servicing-policy.md).  
- To use the Privileged End Point (PEP) to monitor and resume updates, see [Monitor updates in Azure Stack using the privileged endpoint](azure-stack-monitor-update.md).  
