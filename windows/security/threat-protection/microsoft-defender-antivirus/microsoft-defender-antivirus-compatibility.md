---
title: Microsoft Defender Antivirus compatibility with other security products
description: Microsoft Defender Antivirus operates in different ways depending on what other security products you have installed, and the operating system you are using.
keywords: windows defender, atp, advanced threat protection, compatibility, passive mode
search.product: eADQiWindows 10XVcnh
ms.pagetype: security
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.localizationpriority: medium
author: denisebmsft
ms.author: deniseb
ms.custom: nextgen
ms.reviewer:
manager: dansimp
ms.date: 11/06/2020
---

# Microsoft Defender Antivirus compatibility

[!INCLUDE [Microsoft 365 Defender rebranding](../../includes/microsoft-defender.md)]


**Applies to:**

- [Microsoft Defender for Endpoint](https://go.microsoft.com/fwlink/p/?linkid=2146631)

## Overview

Microsoft Defender Antivirus is automatically enabled and installed on endpoints and devices that are running Windows 10. But what happens when another antivirus/antimalware solution is used? It depends on whether you're using [Microsoft Defender for Endpoint](https://docs.microsoft.com/windows/security/threat-protection) together with your antivirus protection.
- If your organization's endpoints and devices are protected with a non-Microsoft antivirus/antimalware solution, and Microsoft Defender for Endpoint is not used, then Microsoft Defender Antivirus automatically goes into disabled mode.
- If your organization is using Microsoft Defender for Endpoint together with a non-Microsoft antivirus/antimalware solution, then Microsoft Defender Antivirus automatically goes into passive mode. (Real-time protection and threats are not remediated by Microsoft Defender Antivirus.)
- If your organization is using Microsoft Defender for Endpoint together with a non-Microsoft antivirus/antimalware solution, and you have [EDR in block mode](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/edr-in-block-mode) enabled, then whenever a malicious artifact is detected, Microsoft Defender for Endpoint takes action to block and remediate the artifact.

## Antivirus and Microsoft Defender for Endpoint

The following table summarizes what happens with Microsoft Defender Antivirus when third-party antivirus products are used together or without Microsoft Defender for Endpoint.


| Windows version   | Antimalware protection  | Microsoft Defender for Endpoint enrollment | Microsoft Defender Antivirus state     |
|------|------|-------|-------|
| Windows 10      | A third-party product that is not offered or developed by Microsoft |                       Yes                       |           Passive mode            |
| Windows 10      | A third-party product that is not offered or developed by Microsoft |                       No                        |      Automatic disabled mode     |
| Windows 10      |                         Microsoft Defender Antivirus                         |                       Yes                       |            Active mode            |
| Windows 10      |                         Microsoft Defender Antivirus                         |                       No                        |            Active mode            |
| Windows Server 2016 or 2019 | A third-party product that is not offered or developed by Microsoft |                       Yes                       | Active mode<sup>[[1](#fn1)]</sup> |
| Windows Server 2016 or 2019 | A third-party product that is not offered or developed by Microsoft |                       No                        | Active mode<sup>[[1](#fn1)]<sup>  |
| Windows Server 2016 or 2019 |                         Microsoft Defender Antivirus                         |                       Yes                       |            Active mode            |
| Windows Server 2016 or 2019 |                         Microsoft Defender Antivirus                         |                       No                        |            Active mode            |

(<a id="fn1">1</a>)  On Windows Server 2016 or 2019, Microsoft Defender Antivirus will not enter passive or disabled mode if you have also installed a third-party antivirus product. If you install a third-party antivirus product, you should [consider uninstalling Microsoft Defender Antivirus on Windows Server 2016 or 2019](microsoft-defender-antivirus-on-windows-server-2016.md#need-to-uninstall-microsoft-defender-antivirus) to prevent problems caused by having multiple antivirus products installed on a machine.

If you are using Windows Server, version 1803 or Windows Server 2019, you can enable passive mode by setting this registry key:
- Path: `HKLM\SOFTWARE\Policies\Microsoft\Windows Advanced Threat Protection`
- Name: ForceDefenderPassiveMode
- Type: REG_DWORD
- Value: 1

See [Microsoft Defender Antivirus on Windows Server 2016 and 2019](microsoft-defender-antivirus-on-windows-server-2016.md) for key differences and management options for Windows Server installations.

> [!IMPORTANT]
> Microsoft Defender Antivirus is only available on endpoints running Windows 10, Windows Server 2016, and Windows Server 2019.
>
> In Windows 8.1 and Windows Server 2012, enterprise-level endpoint antivirus protection is offered as [System Center Endpoint Protection](https://technet.microsoft.com/library/hh508760.aspx), which is managed through Microsoft Endpoint Configuration Manager.
>
> Windows Defender is also offered for [consumer devices on Windows 8.1 and Windows Server 2012](https://technet.microsoft.com/library/dn344918#BKMK_WindowsDefender), although it does not provide enterprise-level management (or an interface on Windows Server 2012 Server Core installations).

## Functionality and features available in each state

The following table summarizes the functionality and features that are available in each state:

|State |[Real-time protection](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/configure-real-time-protection-microsoft-defender-antivirus) and [cloud-delivered protection](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/enable-cloud-protection-microsoft-defender-antivirus) | [Limited periodic scanning availability](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/limited-periodic-scanning-microsoft-defender-antivirus) | [File scanning and detection information](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/customize-run-review-remediate-scans-microsoft-defender-antivirus) | [Threat remediation](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/configure-remediation-microsoft-defender-antivirus) | [Security intelligence updates](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/manage-updates-baselines-microsoft-defender-antivirus) |
|--|--|--|--|--|--|
|Active mode <br/><br/>  |Yes |No |Yes |Yes |Yes |
|Passive mode  |Yes |No |Yes |Only during [scheduled or on-demand scans](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-antivirus/scheduled-catch-up-scans-microsoft-defender-antivirus) |Yes |
|[EDR in block mode enabled](../microsoft-defender-atp/edr-in-block-mode.md)  |No |No |Yes |Yes |Yes |
|Automatic disabled mode  |No |Yes |No |No |No |

- In Active mode, Microsoft Defender Antivirus is used as the antivirus app on the machine. All configuration made with Configuration Manager, Group Policy, Intune, or other management products will apply. Files are scanned and threats remediated, and detection information are reported in your configuration tool (such as Configuration Manager or the Microsoft Defender Antivirus app on the machine itself).
- In Passive mode, Microsoft Defender Antivirus is not used as the antivirus app, and threats are not remediated by Microsoft Defender Antivirus. Files are scanned and reports are provided for threat detections that are shared with the Microsoft Defender for Endpoint service. Therefore, you might encounter alerts in the Security Center console with Microsoft Defender Antivirus as a source, even when Microsoft Defender Antivirus is in Passive mode.
- When [EDR in block mode](../microsoft-defender-atp/edr-in-block-mode.md) is turned on, Microsoft Defender Antivirus is not used as the primary antivirus solution, but can still detect and remediate malicious items.
- When disabled, Microsoft Defender Antivirus is not used as the antivirus app. Files are not scanned and threats are not remediated.

## Keep the following points in mind

If you are enrolled in Microsoft Defender for Endpoint and you are using a third-party antimalware product, then passive mode is enabled. [The service requires common information sharing from Microsoft Defender Antivirus service](../microsoft-defender-atp/defender-compatibility.md) in order to properly monitor your devices and network for intrusion attempts and attacks.

When Microsoft Defender Antivirus is automatically disabled, it can automatically re-enabled if the protection offered by a third-party antivirus product expires or otherwise stops providing real-time protection from viruses, malware, or other threats. This is to ensure antivirus protection is maintained on the endpoint. It also allows you to enable [limited periodic scanning](limited-periodic-scanning-microsoft-defender-antivirus.md), which uses the Microsoft Defender Antivirus engine to periodically check for threats in addition to your main antivirus app.

In passive mode, you can still [manage updates for Microsoft Defender Antivirus](manage-updates-baselines-microsoft-defender-antivirus.md); however, you can't move Microsoft Defender Antivirus into the normal active mode if your endpoints have an up-to-date third-party product providing real-time protection from malware.

If you uninstall the other product, and choose to use Microsoft Defender Antivirus to provide protection to your endpoints, Microsoft Defender Antivirus will automatically return to its normal active mode.

> [!WARNING]
> You should not attempt to disable, stop, or modify any of the associated services used by Microsoft Defender Antivirus, Microsoft Defender for Endpoint, or the Windows Security app. This includes the *wscsvc*, *SecurityHealthService*, *MsSense*, *Sense*, *WinDefend*, or *MsMpEng* services and process. Manually modifying these services can cause severe instability on your endpoints and open your network to infections and attacks. It can also cause problems when using third-party antivirus apps and how their information is displayed in the [Windows Security app](microsoft-defender-security-center-antivirus.md).

> [!IMPORTANT]
> If you are using [Microsoft Endpoint DLP](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/information-protection-in-windows-overview), Microsoft Defender Antivirus real-time protection is enabled, even when Microsoft Defender Antivirus is running in passive mode. Endpoint DLP depends on real-time protection to operate.

## See also

- [Microsoft Defender Antivirus in Windows 10](microsoft-defender-antivirus-in-windows-10.md)
- [Microsoft Defender Antivirus on Windows Server 2016 and 2019](microsoft-defender-antivirus-on-windows-server-2016.md)
- [EDR in block mode](../microsoft-defender-atp/edr-in-block-mode.md)
- [Configure Endpoint Protection](https://docs.microsoft.com/mem/configmgr/protect/deploy-use/endpoint-protection-configure)
- [Configure Endpoint Protection on a standalone client](https://docs.microsoft.com/mem/configmgr/protect/deploy-use/endpoint-protection-configure-standalone-client)
