<properties
    pageTitle="Azure Website Wiederherstellung Support-Matrix | Microsoft Azure"
    description="Enthält eine Übersicht über die unterstützten Betriebssysteme und-Komponenten für Azure Website Wiederherstellung"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure Website Wiederherstellung Support-matrix

In diesem Artikel wird erörtert, unterstützten Betriebssysteme und -Komponenten für die Website Wiederherstellung Azure-Bereitstellungen. Eine Liste der unterstützten Komponenten und Komponenten steht für jedes Bereitstellungsszenario in den einzelnen im entsprechenden Bereitstellung Artikel und dieses Dokument enthält eine Übersicht über diese.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Unterstützte Betriebssysteme für Virtualisierung servers


**Datenquelle** | **Ziel** | **Host-Betriebssystem**
---|---|---|--- 
**Hyper-V-Hosts (ohne VMM)** | Azure | Windows Server 2012 R2 mit den neuesten updates
**Hyper-V-Hosts (mit VMM)** | Azure | Windows Server 2012 R2 mit den neuesten updates
**Hyper-V-Hosts (mit VMM)** | Sekundäre VMM-Website | Mindestens WindowsServer 2012 mit den neuesten Updates
**VMware Hosts/vCenter** | Azure | vCenter 5.5 oder 6.0 (Unterstützung für nur 5,5 Features) <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit den neuesten updates
**VMware Hosts/vCenter** | Sekundäre VMware-Website | vCenter 5.5 oder 6.0 (Unterstützung für nur 5,5 Features) <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit den neuesten updates

## <a name="supported-requirements-for-replicated-machines"></a>Unterstützte Anforderungen für repliziert Autos

**Datenquelle** | **Was ist repliziert** | **Ziel** | **Host-Betriebssystem**
---|---|---|--- 
**Hyper-V virtuellen Computern** | Alle Arbeitsbelastung | Azure | Alle Gast OS [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> Virtuelle Computer müssen [Azure Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) entsprechen.
**Hyper-V virtueller Computer (mit VMM)** | Alle Arbeitsbelastung | Azure | Alle Gast OS [von Azure unterstützt werden](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> Virtuelle Computer müssen [Azure Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) entsprechen.
**Hyper-V virtueller Computer (mit VMM)** | Alle Arbeitsbelastung | Sekundäre VMM-Website | Alle Gast OS [von Hyper-V unterstützt werden](https://technet.microsoft.com/library/mt126277.aspx)
**Virtuelle VMware-Computer** | Alle Arbeitsbelastung Windows virtuellen Computers ausgeführt | Azure | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit am minimalen SP1<br/><br/> Virtuelle Computer müssen [Azure Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) entsprechen.
**Virtuelle VMware-Computer** | Alle Arbeitsbelastung für Linux VM ausgeführt | Azure | Red Hat Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter der Red Hat kompatibel Kernel oder Enterprise Kernel unverwüstliche Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: File System (EXT3, ETX4, ReiserFS, XFS); Mehrere Pfade Software Gerät Mapper (mehrere Pfade)); Volumen-Manager: (LVM2). Physische Server mit HP CCISS Controller Speicher werden nicht unterstützt. Das Dateisystem ReiserFS wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/> Virtuelle Computer müssen [Azure Anforderungen](site-recovery-best-practices.md#azure-virtual-machine-requirements) entsprechen.
**Virtuelle VMware-Computer** | Alle Arbeitsbelastung Windows virtuellen Computers ausgeführt | Sekundäre VMware-Website | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 mit am minimalen SP1
**Virtuelle VMware-Computer** | Alle Arbeitsbelastung für Linux VM ausgeführt | Sekundäre VMware-Website | Red Hat Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter der Red Hat kompatibel Kernel oder Enterprise Kernel unverwüstliche Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: File System (EXT3, ETX4, ReiserFS, XFS); Mehrere Pfade Software Gerät Mapper (mehrere Pfade)); Volumen-Manager: (LVM2). Physische Server mit HP CCISS Controller Speicher werden nicht unterstützt. Das Dateisystem ReiserFS wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.
**Physische Server** | Alle Arbeitsbelastung unter Windows | Azure | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 mit am minimalen SP1
**Physische Server** | Alle Arbeitsbelastung für Linux ausgeführt | Azure | Red Hat Enterprise Linux 6.7,7.1,7.2 <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter der Red Hat kompatibel Kernel oder Enterprise Kernel unverwüstliche Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: File System (EXT3, ETX4, ReiserFS, XFS); Mehrere Pfade Software Gerät Mapper (mehrere Pfade)); Volumen-Manager: (LVM2). Physische Server mit HP CCISS Controller Speicher werden nicht unterstützt. Das Dateisystem ReiserFS wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.
**Physische Server** | Alle Arbeitsbelastung unter Windows | Sekundären | 64-Bit-Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 mit am minimalen SP1
**Physische Server** | Alle Arbeitsbelastung für Linux ausgeführt | Sekundären | Red Hat Enterprise Linux 6.7,7.1,7.2 <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter der Red Hat kompatibel Kernel oder Enterprise Kernel unverwüstliche Release 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: File System (EXT3, ETX4, ReiserFS, XFS); Mehrere Pfade Software Gerät Mapper (mehrere Pfade)); Volumen-Manager: (LVM2). Physische Server mit HP CCISS Controller Speicher werden nicht unterstützt. Das Dateisystem ReiserFS wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.


## <a name="provider-versions"></a>Anbieter-Versionen

**Namen** | **Beschreibung** | **Neueste version** | **Support** | **Details**
---|---|---|---| ---
**Anbieter für Azure Websites Wiederherstellung** | Koordinaten Kommunikation zwischen lokalen Servern und Azure/sekundären-Website <br/><br/> Auf lokale VMM Servern oder Hyper-V-Servern installiert werden, wenn kein VMM Server | 5.1.1700 (Portal verfügbar) | [Neuesten Features und Updates](https://support.microsoft.com/kb/3155002)
**Azure Website Wiederherstellung Unified einrichten (VMware zur Azure)** | Koordinaten Kommunikation zwischen lokalen VMware Servers und Azure <br/><br/> Auf VMware-Server lokal installiert | 9.3.4246.1 (aus dem Portal verfügbar) | [Neuesten Features und Updates](https://support.microsoft.com/kb/3155002)
**Mobilität-Dienst** | Koordiniert die Replikation zwischen lokalen VMware-Servern/physische Servers und Azure/sekundären-Website | NV (Portal verfügbar) | Installiert auf jede VMware VM oder physische Server, den repliziert werden soll. **Microsoft Azure Wiederherstellung Services (MARS) agent** | Replikation zwischen Hyper-V virtuellen Computern und Azure-Koordinaten<br/><br/> Klicken Sie auf lokale Hyper-V-Servern (mit oder ohne VMM-Server) installiert | 2.0.8689.0 (aus dem Portal verfügbar) | Dieser Agent wird von Azure Website Wiederherstellung und Azure Sicherung verwendet). [Weitere] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Nächste Schritte

[Bereiten für Bereitstellung vor](site-recovery-best-practices.md)

