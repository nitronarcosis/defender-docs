---
title: Microsoft Defender for Endpoint on Linux
ms.reviewer: gopkr, pahuijbr, megphapriya
description: Describes how to install and use Microsoft Defender for Endpoint on Linux.
ms.service: defender-endpoint
ms.author: dansimp
author: dansimp
ms.localizationpriority: medium
manager: deniseb
audience: ITPro
ms.collection:
- m365-security
- tier3
- mde-linux
ms.topic: conceptual
ms.subservice: linux
search.appverid: met150
ms.date: 09/10/2024
---

# Microsoft Defender for Endpoint on Linux

[!INCLUDE [Microsoft Defender XDR rebranding](../includes/microsoft-defender.md)]

**Applies to:**

- [Microsoft Defender for Endpoint Plan 1](microsoft-defender-endpoint.md)
- [Microsoft Defender for Endpoint Plan 2](microsoft-defender-endpoint.md)
- [Microsoft Defender XDR](/defender-xdr)

> Want to experience Microsoft Defender for Endpoint? [Sign up for a free trial.](https://signup.microsoft.com/create-account/signup?products=7f379fee-c4f9-4278-b0a1-e4c8c2fcdf7e&ru=https://aka.ms/MDEp2OpenTrial?ocid=docs-wdatp-exposedapis-abovefoldlink)

This article describes how to install, configure, update, and use Microsoft Defender for Endpoint on Linux.

> [!CAUTION]
> Running other third-party endpoint protection products alongside Microsoft Defender for Endpoint on Linux is likely to lead to performance problems and unpredictable side effects. If non-Microsoft endpoint protection is an absolute requirement in your environment, you can still safely take advantage of Defender for Endpoint on Linux EDR functionality after configuring the antivirus functionality to run in [Passive mode](linux-preferences.md#enforcement-level-for-antivirus-engine).

## How to install Microsoft Defender for Endpoint on Linux

Microsoft Defender for Endpoint for Linux includes anti-malware and endpoint detection and response (EDR) capabilities.

### Prerequisites

- Access to the Microsoft Defender portal
- Linux distribution using the [systemd](https://systemd.io/) system manager

  > [!NOTE]
  > Linux distribution using system manager, except for RHEL/CentOS 6.x support both SystemV and Upstart.

- Beginner-level experience in Linux and BASH scripting
- Administrative privileges on the device (for manual deployment)

> [!NOTE]
> Microsoft Defender for Endpoint on Linux agent is independent from [OMS agent](/azure/azure-monitor/agents/agents-overview#log-analytics-agent). Microsoft Defender for Endpoint relies on its own independent telemetry pipeline.

### Installation instructions

There are several methods and deployment tools that you can use to install and configure Microsoft Defender for Endpoint on Linux.

In general you need to take the following steps:

- Ensure that you have a Microsoft Defender for Endpoint subscription.
- Deploy Microsoft Defender for Endpoint on Linux using one of the following deployment methods:
  - The command-line tool:
    - [Manual deployment](linux-install-manually.md)
   - Third-party management tools:
    - [Deploy using Puppet configuration management tool](linux-install-with-puppet.md)
    - [Deploy using Ansible configuration management tool](linux-install-with-ansible.md)
      - [Deploy using Chef configuration management tool](linux-deploy-defender-for-endpoint-with-chef.md)
      - [Deploy using Saltstack configuration management tool](linux-install-with-saltack.md)
      If you experience any installation failures, refer to [Troubleshooting installation failures in Microsoft Defender for Endpoint on Linux](linux-support-install.md).

> [!NOTE]
> It is not supported to install Microsoft Defender for Endpoint in any other location other than the default install path.
> Microsoft Defender for Endpoint on Linux creates an `mdatp` user with random UID and GID. If you want to control the UID and GID, create an `mdatp` user prior to installation using the  `/usr/sbin/nologin` shell option. Here's an example: `mdatp:x:UID:GID::/home/mdatp:/usr/sbin/nologin`.

### System requirements

- Disk space: 2 GB
> [!NOTE]
> An additional 2 GB disk space might be needed if cloud diagnostics are enabled for crash collections. Please make sure that you have free disk space in /var.
- Cores: 2 minimum, 4 preferred
> [!NOTE]
> If you are on Passive or RTP ON mode, 2 Cores are minimum and 4 Cores are preferred. If you are turning on BM, then a minimum of 4 Cores is required.
- Memory: 1 GB minimum, 4 preferred
- List of supported Linux server distributions and x64 (AMD64/EM64T) and x86_64 versions:
  - Red Hat Enterprise Linux 6.7 or higher (In preview)
  - Red Hat Enterprise Linux 7.2 or higher
  - Red Hat Enterprise Linux 8.x
  - Red Hat Enterprise Linux 9.x
  - CentOS 6.7 or higher (In preview)
  - CentOS 7.2 or higher
  - Ubuntu 16.04 LTS 
  - Ubuntu 18.04 LTS
  - Ubuntu 20.04 LTS
  - Ubuntu 22.04 LTS
  - Ubuntu 24.04 LTS
  - Debian 9 - 12
  - SUSE Linux Enterprise Server 12 or higher
  - SUSE Linux Enterprise Server 15 or higher
  - Oracle Linux 7.2 or higher
  - Oracle Linux 8.x
  - Oracle Linux 9.x
  - Amazon Linux 2
  - Amazon Linux 2023
  - Fedora 33-38 
  - Rocky 8.7 and higher
  - Rocky 9.2 and higher 
  - Alma 8.4 and higher
  - Alma 9.2 and higher 
  - Mariner 2

    > [!NOTE]
    > Distributions and version that are not explicitly listed are unsupported (even if they are derived from the officially supported distributions).
    > With RHEL 6 support for 'extended end of life' coming to an end by June 30, 2024; Defender for Endpoint on Linux support for RHEL 6 will also be deprecated by June 30, 2024
    > Defender for Endpoint on Linux version `101.23082.0011` is the last Defender for Endpoint on Linux release supporting RHEL 6.7 or higher versions (does not expire before June 30, 2024). Customers are advised to plan upgrades to their RHEL 6 infrastructure aligned with guidance from Red Hat.
    > Microsoft Defender Vulnerablity Management is not supported on Rocky and Alma currently.

- List of supported kernel versions

   > [!NOTE]
   > Microsoft Defender for Endpoint on Red Hat Enterprise Linux and CentOS - 6.7 to 6.10 is a Kernel based solution. You must verify that the kernel version is supported before updating to a newer kernel version.
   > Microsoft Defender for Endpoint for all other supported distributions and versions is kernel-version-agnostic. With a minimal requirement for the kernel version to be at or greater than 3.10.0-327.

  - The `fanotify` kernel option must be enabled

  - Red Hat Enterprise Linux 6 and CentOS 6:
    - For 6.7: 2.6.32-573.* (except 2.6.32-573.el6.x86_64)
    - For 6.8: 2.6.32-642.*
    - For 6.9: 2.6.32-696.* (except 2.6.32-696.el6.x86_64)
    - For 6.10: 
      - 2.6.32-754.10.1.el6.x86_64
      - 2.6.32-754.11.1.el6.x86_64
      - 2.6.32-754.12.1.el6.x86_64
      - 2.6.32-754.14.2.el6.x86_64
      - 2.6.32-754.15.3.el6.x86_64
      - 2.6.32-754.17.1.el6.x86_64
      - 2.6.32-754.18.2.el6.x86_64
      - 2.6.32-754.2.1.el6.x86_64
      - 2.6.32-754.22.1.el6.x86_64
      - 2.6.32-754.23.1.el6.x86_64
      - 2.6.32-754.24.2.el6.x86_64
      - 2.6.32-754.24.3.el6.x86_64
      - 2.6.32-754.25.1.el6.x86_64
      - 2.6.32-754.27.1.el6.x86_64
      - 2.6.32-754.28.1.el6.x86_64
      - 2.6.32-754.29.1.el6.x86_64
      - 2.6.32-754.29.2.el6.x86_64
      - 2.6.32-754.3.5.el6.x86_64
      - 2.6.32-754.30.2.el6.x86_64
      - 2.6.32-754.33.1.el6.x86_64
      - 2.6.32-754.35.1.el6.x86_64
      - 2.6.32-754.39.1.el6.x86_64
      - 2.6.32-754.41.2.el6.x86_64
      - 2.6.32-754.43.1.el6.x86_64
      - 2.6.32-754.47.1.el6.x86_64
      - 2.6.32-754.48.1.el6.x86_64
      - 2.6.32-754.49.1.el6.x86_64
      - 2.6.32-754.6.3.el6.x86_64
      - 2.6.32-754.9.1.el6.x86_64

   > [!NOTE]
   > After a new package version is released, support for the previous two versions is reduced to technical support only. Versions older than that which are listed in this section are provided for technical upgrade support only.

  > [!CAUTION]
  > Running Defender for Endpoint on Linux side by side with other `fanotify`-based security solutions is not supported. It can lead to unpredictable results, including hanging the operating system. If there are any other applications on the system that use `fanotify` in blocking mode, applications are listed in the `conflicting_applications` field of the `mdatp health` command output. The Linux **FAPolicyD** feature uses `fanotify` in blocking mode, and is therefore unsupported when running Defender for Endpoint in active mode. You can still safely take advantage of Defender for Endpoint on Linux EDR functionality after configuring the antivirus functionality Real Time Protection Enabled to [Passive mode](linux-preferences.md#enforcement-level-for-antivirus-engine).

- List of supported filesystems for RTP, Quick, Full and Custom Scan.
  
  |RTP, Quick, Full Scan| Custom Scan|
  |---|---|
  |btrfs|All filesystems supported for RTP, Quick, Full Scan|
  |ecryptfs|Efs|
  |ext2|S3fs|
  |ext3|Blobfuse|
  |ext4|Lustr|
  |fuse|glustrefs|
  |fuseblk|Afs|
  |jfs|sshfs|
  |nfs (v3 only)|cifs|
  |overlay|smb|
  |ramfs|gcsfuse|
  |reiserfs|sysfs|
  |tmpfs|
  |udf|
  |vfat|
  |xfs|


After you've enabled the service, you need to configure your network or firewall to allow outbound connections between it and your endpoints.

- Audit framework (`auditd`) must be enabled.

  > [!NOTE]
  > System events captured by rules added to `/etc/audit/rules.d/` will add to `audit.log`(s) and might affect host auditing and upstream collection. Events added by Microsoft Defender for Endpoint on Linux will be tagged with `mdatp` key.

- /opt/microsoft/mdatp/sbin/wdavdaemon requires executable permission. For more information, see "Ensure that the daemon has executable permission" in [Troubleshoot installation issues for Microsoft Defender for Endpoint on Linux](linux-support-install.md).

### External package dependency

The following external package dependencies exist for the mdatp package:

- The mdatp RPM package requires `glibc >= 2.17`, `audit`, `policycoreutils`, `semanage` `selinux-policy-targeted`, `mde-netfilter`
- For RHEL6 the mdatp RPM package requires `audit`, `policycoreutils`, `libselinux`, `mde-netfilter`
- For DEBIAN the mdatp package requires `libc6 >= 2.23`, `uuid-runtime`, `auditd`, `mde-netfilter`

The mde-netfilter package also has the following package dependencies:
- For DEBIAN the mde-netfilter package requires `libnetfilter-queue1`, `libglib2.0-0`
- For RPM the mde-netfilter package requires `libmnl`, `libnfnetlink`, `libnetfilter_queue`, `glib2`

If the Microsoft Defender for Endpoint installation fails due to missing dependencies errors, you can manually download the pre-requisite dependencies.

### Configuring Exclusions

When adding exclusions to Microsoft Defender Antivirus, you should be mindful of [Common Exclusion Mistakes for Microsoft Defender Antivirus](common-exclusion-mistakes-microsoft-defender-antivirus.md).

### Network connections

Ensure that connectivity is possible from your devices to Microsoft Defender for Endpoint cloud services. To prepare your environment, please reference [STEP 1: Configure your network environment to ensure connectivity with Defender for Endpoint service](configure-environment.md).

Defender for Endpoint on Linux can connect through a proxy server by using the following discovery methods:

- Transparent proxy
- Manual static proxy configuration

If a proxy or firewall is blocking anonymous traffic, make sure that anonymous traffic is permitted in the previously listed URLs. For transparent proxies, no additional configuration is needed for Defender for Endpoint. For static proxy, follow the steps in [Manual Static Proxy Configuration](linux-static-proxy-configuration.md).

> [!WARNING]
> PAC, WPAD, and authenticated proxies are not supported. Ensure that only a static proxy or transparent proxy is being used.
>
> SSL inspection and intercepting proxies are also not supported for security reasons. Configure an exception for SSL inspection and your proxy server to directly pass through data from Defender for Endpoint on Linux to the relevant URLs without interception. Adding your interception certificate to the global store will not allow for interception.

For troubleshooting steps, see [Troubleshoot cloud connectivity issues for Microsoft Defender for Endpoint on Linux](linux-support-connectivity.md).

## How to update Microsoft Defender for Endpoint on Linux

Microsoft regularly publishes software updates to improve performance, security, and to deliver new features. To update Microsoft Defender for Endpoint on Linux, refer to [Deploy updates for Microsoft Defender for Endpoint on Linux](linux-updates.md).

## How to configure Microsoft Defender for Endpoint on Linux

Guidance for how to configure the product in enterprise environments is available in [Set preferences for Microsoft Defender for Endpoint on Linux](linux-preferences.md).

## Common Applications to Microsoft Defender for Endpoint can impact

High I/O workloads from certain applications can experience performance issues when Microsoft Defender for Endpoint is installed. These include applications for developer scenarios like Jenkins and Jira, and database workloads like OracleDB and Postgres. If experiencing performance degradation, consider setting exclusions for trusted applications, keeping [Common Exclusion Mistakes for Microsoft Defender Antivirus](common-exclusion-mistakes-microsoft-defender-antivirus.md) in mind. For additional guidance, consider consulting documentation regarding antivirus exclusions from third party applications.

## Resources

- For more information about logging, uninstalling, or other articles, see [Resources](linux-resources.md).

## Related articles

- [Protect your endpoints with Defender for Cloud's integrated EDR solution: Microsoft Defender for Endpoint](/azure/defender-for-cloud/integration-defender-for-endpoint)
- [Connect your non-Azure machines to Microsoft Defender for Cloud](/azure/defender-for-cloud/quickstart-onboard-machines)
- [Turn on network protection for Linux](network-protection-linux.md)

[!INCLUDE [Microsoft Defender for Endpoint Tech Community](../includes/defender-mde-techcommunity.md)]
