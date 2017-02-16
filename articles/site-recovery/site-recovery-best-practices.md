<properties
    pageTitle="Vorbereiten für die Bereitstellung der Website Wiederherstellung | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie mit Replikation mit Azure Website Wiederherstellung bereitstellen vorbereiten."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Bereiten Sie für die Website Wiederherstellung Azure-Bereitstellung vor

Lesen Sie diesen Artikel auf hoher Ebene eine Übersicht der Bereitstellung Vorschriften für jedes Replikation Szenario vom Dienst Azure Website Wiederherstellung unterstützt. Nachdem Sie die allgemeinen Anforderungen für jedes Szenario lesen möchten, können Sie mit bestimmten Deployment-Details für jede Bereitstellung verknüpfen.

Nach dem Lesen diesen Artikel Beitrag Kommentare oder Fragen am Fuß des Artikels oder im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>(Übersicht)

Organisationen benötigen eine BCDR Strategie, die bestimmt, wie apps, Auslastung und Daten bleiben während der geplanten und ungeplanten Ausfallzeiten ausgeführt werden und verfügbar, und zum normalen Arbeit Umständen so früh wie möglich wiederherzustellen. Strategische BCDR sollten Geschäftsdaten beibehalten, sicherer und wiederhergestellt, und vergewissern Sie sich Auslastung kontinuierlich verfügbar bleiben, wenn bei Datenverlusten. 

Website Wiederherstellung ist eine Azure-Dienst, der zur strategische BCDR beiträgt, indem orchestriert Replikation von physische Server lokal und in der Cloud (Azure) oder zu einem sekundären Datencenter-virtuellen Computern an. Treten Ausfall in gewohnten Standort befinden, fehl Sie über den zweiten Standort zum Aktualisieren von apps und Auslastung zur Verfügung. Sie fehl zurück zur gewohnten Standort befinden, wenn sie normale Vorgänge zurückgibt. Erfahren Sie mehr in [Neuigkeiten Website Wiederherstellung?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Wählen Sie Ihr Bereitstellungsmodell

Azure weist zwei verschiedenen [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Arbeiten mit Ressourcen: das Modell Azure Ressourcenmanager und Modell Management Services klassischen. Azure, weist ebenfalls zwei communityportalen – [Azure klassischen Portal](https://manage.windowsazure.com/) , die das Bereitstellungsmodell klassischen unterstützt, und der [Azure-Portal](https://ms.portal.azure.com/) mit Unterstützung für beide Bereitstellungsmodelle.

Website Wiederherstellung steht in der klassischen Portal und Azure-Portal. In der klassischen Azure-Portal können Sie die Website Wiederherstellung mit dem klassischen Services-Modell Management unterstützen. Azure-Portal können Sie das klassische Modell oder Ressourcenmanager Bereitstellungen unterstützen. [Weitere Informationen finden Sie](site-recovery-overview.md#site-recovery-in-the-azure-portal) über die Bereitstellung von mit dem Portal Azure.

Wenn Sie einer Bereitstellung Modellnotiz, die Auswahl sind:

- Es empfiehlt sich, dass Sie mögliche [Azure-Portal](https://portal.azure.com) und dem Modell Ressourcenmanager verwenden.
- Website Wiederherstellung bietet ein einfacheres und mehr intuitive überfordert Erfahrung Azure-Portal.
- Verwenden des Azure-Portals an, kann Maschinen sowohl klassischen und Ressourcenmanager Speicher in Azure repliziert werden. Darüber hinaus können Sie die Azure-Portal LRS oder GRS Speicherkonten verwenden.
- Azure-Portal kombiniert die Sicherung und Wiederherstellung von Website-Dienste in einer einzelnen Wiederherstellung Services Tresor, sodass Sie einrichten und Verwalten von BCDR Dienste von einem einzigen Ort können.
- Benutzer mit nach der Bereitstellung mit der Cloud Lösung Provider (CSP) Programm Azure-Abonnements können jetzt Website Wiederherstellungsvorgängen Azure-Portal verwalten.
- Virtuelle VMware-Computer oder physische Computer auf Azure Azure-Portal repliziert bietet eine Reihe von neuen Funktionen, einschließlich der Unterstützung für Premium-Speicher und die Möglichkeit zum Ausschließen bestimmter Datenträger aus der Replikation.


## <a name="select-your-deployment-scenario"></a>Wählen Sie Ihre Bereitstellungsszenario

**Bereitstellung** | **Details** | **Azure-portal** | **Klassische-portal** | **PowerShell**
---|---|---|---|---
**VMware virtuelle Computer Azure** | Virtuelle VMware-Computer auf Azure-Speicher repliziert | In der Azure kann Portal virtuellen Computern Classic oder Ressourcenmanager Speicher über fehl<br/><br/> Bereitstellung der [Azure-Portal](site-recovery-vmware-to-azure.md) bietet eine optimierte Bereitstellung Erfahrung und eine Reihe von Vorteilen Feature. | In der klassischen Azure-Portal können Sie mit dem [erweiterten Modell](site-recovery-vmware-to-azure-classic.md) bereitstellen und nicht über klassische Azure-Speicher.<br/><br/> Es gibt auch ein älteres Modell, das für die Bereitstellung neuer verwendet werden. |  PowerShell wird derzeit nicht unterstützt.
**Hyper-V virtuelle Computer Azure** | Hyper-V virtuelle Computer Azure-Speicher. Virtuellen Computern können auf Hyper-V-Hosts in System Center VMM Wolken oder ohne VMM verwaltet werden. | In der Azure kann Portal virtuellen Computern über Classic oder Ressourcenmanager Speicher fehl.<br/><br/> Azure-Portal bietet die optimierten Bereitstellung. [Erfahren Sie mehr](site-recovery-vmm-to-azure.md) zur Replikation von Hyper-V virtuelle Computer in VMM Wolken. [Erfahren Sie mehr](site-recovery-hyper-v-site-to-azure.md) zur Replikation von Hyper-V virtuellen Computern (ohne VMM).| In der klassischen Azure-Portal können Sie virtuellen Computern über zum klassischen Azure-Speicher fehl. | PowerShell-Bereitstellung wird unterstützt.
**Physische Server in Azure** | Repliziert physische Windows/Linux-Servern zu Azure-Speicher | In der Azure können Portal-Servern über Classic oder Ressourcenmanager Speicher fehl.<br/><br/> Bereitstellung der [Azure-Portal](site-recovery-vmware-to-azure.md) bietet eine optimierte Bereitstellung Erfahrung und eine Reihe von Vorteilen Feature. | In der klassischen Azure-Portal können Sie mit dem [erweiterten Modell](site-recovery-vmware-to-azure-classic.md) bereitstellen und nicht über klassische Azure-Speicher.<br/><br/> Es gibt auch ein älteres Modell, das für die Bereitstellung neuer verwendet werden. | PowerShell-Bereitstellung wird derzeit nicht unterstützt.
**VMware virtuellen Computern/physische Servern zu sekundären* | Repliziert virtuelle VMware-Computer oder physischen Windows/Linux-Servern an einem sekundären Standort. [Weitere Informationen finden und herunterladen](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) der InMage Scout Benutzerhandbuch. | Azure-Portal nicht verfügbar. | In der klassischen Portal erhalten Sie InMage Scout aus einer Website Wiederherstellung Tresor herunterladen. | PowerShell-Bereitstellung wird derzeit nicht unterstützt
**Hyper-V virtuellen Computern an einem sekundären Standort** | Hyper-V virtuelle Computer in VMM Wolken in einer sekundären Cloud repliziert | Im [Portal Azure](site-recovery-vmm-to-vmm.md) kann Hyper-V virtuelle Computer in VMM Wolken an einem sekundären Standort nur mit Hyper-V Replica repliziert. SAN wird derzeit nicht unterstützt. | In der klassischen Azure-Portal können Sie Hyper-V virtuelle Computer in VMM Wolken an einem sekundären Standort mithilfe von [Hyper-V Replica](site-recovery-vmm-to-vmm-classic.md) oder [SAN-Replikation](site-recovery-vmm-san.md) repliziert | PowerShell-Bereitstellung wird unterstützt.



## <a name="check-what-you-need-for-deployment"></a>Aktivieren Sie die gewünschten Informationen für die Bereitstellung

### <a name="replicate-to-azure"></a>Klicken Sie auf Azure repliziert

**Anforderung** | **Details** 
---|---
**Azure-Konto** | Benötigen Sie ein [Microsoft Azure](http://azure.microsoft.com/) -Konto ein.<br/><br/> Sie können mit einer [kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/)beginnen. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Website Wiederherstellung Preise.
**Azure-Speicher** | Replizierte Daten in Azure-Speicher gespeichert und Azure-virtuellen Computern erstellt werden, wenn ausgeführt wird. Zur Replikation an Azure benötigen Sie ein [Konto Azure-Speicher](../storage/storage-introduction.md).<br/><br/>Wenn Sie in der klassischen Portal Website Wiederherstellung bereitstellen, benötigen Sie eine oder mehrere [standard GRS Speicher-Konten](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Wenn Sie die Azure-Portal bereitstellen, können Sie GRS oder LRS Speicher.<br/><br/> Wenn Sie virtuelle VMware-Computer oder im Speicher Azure Portals Premium physischen Servern repliziert sind, wird unterstützt. Beachten Sie, wenn Sie ein Premium Speicher-Konto verwenden Sie auch ein standard Speicherkonto benötigen, um die Replikation Protokolle speichern, die laufende Änderungen lokaler Daten zu erfassen. [Premium-Speicher](../storage/storage-premium-storage.md) wird in der Regel für virtuellen Computern verwendet, die eine konsistente leistungsstarke EA und niedrige Wartezeit zu Host EA stark Auslastung benötigen.<br/><br/> Wenn Sie ein Konto Premium speichern replizierte Daten verwenden möchten, benötigen Sie auch, ein standard Speicherkonto, um die Replikation Protokolle speichern, die laufende Änderungen lokaler Daten zu erfassen.
**Azure Netzwerk** | Zur Replikation an Azure benötigen Sie ein Azure-Netzwerk aus, die mit Azure-virtuellen Computern verbunden werden soll, wenn sie nach einem Failover erstellt werden.<br/><br/> Wenn Sie in der klassischen Portal bereitstellen, verwenden Sie eine klassische Netzwerk. Wenn Sie die Azure-Portal bereitstellen, können Sie eine Classic oder Ressourcenmanager Netzwerk verwenden.<br/><br/> Das Netzwerk muss sich in derselben Region als die Tresor.
**Netzwerk-Zuordnung (VMM in Azure)** | Wenn Sie von VMM in Azure repliziert, [Netzwerk-Zuordnung](site-recovery-network-mapping.md) ist sichergestellt, dass Azure-virtuellen Computern nach Failover mit richtigen Netzwerken verbunden sind.<br/><br/> Zum Einrichten der Netzwerk-Zuordnung müssen Sie virtueller Computer Netzwerke im Portal VMM konfigurieren.
**Lokal** | **Virtuelle VMware-Computer**: Sie benötigen einen lokalen Computer unter Website Wiederherstellung Komponenten, VMware vSphere Hosts/vCenter Server und virtuellen Computern repliziert werden soll. [Weitere Informationen finden](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Physische Server**: Wenn Sie physische Servern repliziert sind benötigen Sie eine lokale Computern Website Wiederherstellung Komponenten und physischen Servern repliziert werden soll. [Weitere Informationen finden](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Wenn Sie [wieder](site-recovery-failback-azure-to-vmware.md) treten nach Failover auf Azure möchten benötigen Sie eine VMware Infrastructure erledigen.<br/><br/> **Hyper-V virtuellen Computern**: Hyper-V virtuelle Computer in VMM Wolken repliziert werden soll Sie benötigen einen VMM-Server, und auf welche virtuellen Computern Sie schützen möchten Hyper-V-Hosts befinden. [Weitere Informationen finden](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Wenn Sie Hyper-V virtuelle Computer ohne VMM repliziert möchten benötigen Sie Hyper-V-Hosts auf denen virtuellen Computern befinden. [Weitere Informationen finden](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Geschützte Computer** | Geschützte Maschinen, die die Replikation in Azure müssen [Azure erforderliche Komponenten](#azure-virtual-machine-requirements) nachfolgend beschriebenen entsprechen.


### <a name="replicate-to-a-secondary-site"></a>Auf einem sekundären Standort repliziert

**Anforderung** | **Details** 
---|---
**Azure-Konto** | Benötigen Sie ein [Microsoft Azure](http://azure.microsoft.com/) -Konto ein.<br/><br/> Sie können mit einer [kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/)beginnen. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Website Wiederherstellung Preise.
**Lokal** | **Virtuelle VMware-Computer**: In der primäre Standort Sie benötigen einen Prozessserver zum Zwischenspeichern, komprimieren und Verschlüsseln von Replikationsdaten vor dem Senden an den sekundären Standort. Installieren Sie in der sekundäre Standort einen Konfigurationsserver zum Verwalten und Überwachen der Bereitstellung und einem master Zielserver. Es empfiehlt sich auch auf einen Server vContinuum zur einfacheren Verwaltung. Darüber hinaus benötigen Sie eine oder mehrere VMware vSphere Hosts und optional eine vCenter Server. [Weitere Informationen](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V virtuelle Computer in VMM Wolken**: Sie müssen VMM Server eingerichtet und Hyper-V-Hosts mit virtuellen Computern repliziert werden soll. [Weitere Informationen finden](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Netzwerk-Zuordnung (VMM VMM)** | Wenn Sie von VMM VMM repliziert, [Netzwerk-Zuordnung](site-recovery-network-mapping.md) ist sichergestellt, dass Replikat virtuellen Computern mit entsprechenden Netzwerken, die nach einem Failover verbunden sind und Hyper-V-Hosts optimal gesetzt. Zum Einrichten der Netzwerk-Zuordnung müssen Sie virtueller Computer Netzwerken auf den VMM-Servern zu konfigurieren.
**Speicher-Zuordnung** | Wenn Sie von VMM VMM repliziert sind können Sie optional [Speicher Zuordnung](site-recovery-storage-mapping.md) einrichten das Speicherziel für repliziert virtuelle Computer angeben. Speicher Zuordnung kann sowohl in Hyper-V Replica SAN Replikation eingerichtet werden.<br/><br/> Zuordnung Speicher wird derzeit Azure-Portal nicht unterstützt.


## <a name="verify-url-access"></a>Überprüfen Sie den URL-Zugriff

Stellen Sie sicher, dass diese URLs Servern zugegriffen werden kann.

**URL** | **VMM VMM** | **VMM in Azure** | **Hyper-V, um Azure** | **VMware zur Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Zulassen | Zulassen | Zulassen | Zulassen
*. backup.windowsazure.com | Nicht erforderlich. | Zulassen | Zulassen | Zulassen
*. hypervrecoverymanager.windowsazure.com | Zulassen | Zulassen | Zulassen | Zulassen
*. store.core.windows.net | Zulassen | Zulassen | Zulassen | Zulassen
*. blob.core.windows.net | Nicht erforderlich. | Zulassen | Zulassen | Zulassen
https://www.msftncsi.com/ncsi.txt | Zulassen | Zulassen | Zulassen | Zulassen
https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Nicht erforderlich. | Nicht erforderlich. | Nicht erforderlich. | Zulassen

## <a name="azure-virtual-machine-requirements"></a>Azure-virtuellen Computern Anforderungen

Sie können die Website Wiederherstellung zur virtuellen Computern und physischen Servern mit einem beliebigen Betriebssystem von Azure unterstützt Replikation bereitstellen. Dies umfasst die meisten Versionen von Windows und Linux. Sie müssen sicherzustellen, die lokal, dass virtuellen Computern, die Sie schützen möchten mit Azure Anforderungen entsprechen.


**Feature** | **Anforderungen** | **Details**
---|---|---
Hyper-V-host | Sollte Windows Server 2012 R2 ausgeführt werden | Prüfung der erforderlichen Komponenten schlägt fehl, wenn das Betriebssystem nicht unterstützt.
VMware hypervisor  | Unterstütztes Betriebssystem | [Überprüfen der](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Gast-Betriebssystem | Hyper-V so Azure Replikation: Website Wiederherstellung unterstützt alle Betriebssysteme, die [von Azure unterstützt](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)werden. <br/><br/> Für VMware und physischen Serverreplikation: Überprüfen der Windows und Linux [erforderliche Komponenten](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden.
Gast Betriebssystemarchitektur | 64-bit | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden
Größe des Betriebssystems Datenträger |  Bis zu 1023 GB | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden
Betriebssystem Datenträger zählen | 1 | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden.
Zählen von Daten Datenträger | 16 oder weniger (Höchstwert ist eine Funktion der Größe des virtuellen Computers erstellt wird. 16 = XL) | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden
Daten Datenträger virtuelle Festplattengröße | Bis zu 1023 GB | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden
Netzwerkadapter | Grafikkarten werden unterstützt. |
Statische IP-Adresse | Unterstützt | Wenn der primären virtuellen Computern eine statische IP-Adresse verwenden, die können Sie die statische IP-Adresse des virtuellen Computers angeben, die in Azure erstellt werden soll. Beachten Sie, dass statische IP-Adresse für einen Linux virtuellen Computer ausgeführt wird, klicken Sie auf Hyper-V nicht unterstützt wird.
iSCSI-Datenträger | Nicht unterstützt | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden
Freigegebene virtuelle Festplatte | Nicht unterstützt | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden
FC Datenträger | Nicht unterstützt | Prüfung der erforderlichen Komponenten schlägt fehl, wenn Sie nicht unterstützt werden
Festplatte formatieren| VIRTUELLE FESTPLATTE <br/><br/> VHDX | Zwar VHDX in Azure derzeit nicht unterstützt wird, konvertiert Website Wiederherstellung VHDX automatisch auf virtuellen Festplatte, wenn Sie über in Azure fehl. Wenn Sie wieder zur lokalen treten weiterhin den virtuellen Computern das Format VHDX verwenden.
BitLocker | Nicht unterstützt | BitLocker muss deaktiviert werden, bevor Sie einen virtuellen Computer schützen.
Name des virtuellen Computers| Zwischen 1 und 63 Zeichen. Buchstaben, Zahlen und Bindestriche beschränkt. Sollte beginnen und enden mit einem Buchstaben oder einer Zahl | Aktualisieren Sie den Wert in den virtuellen Computereigenschaften in Website Wiederherstellung
Typ des virtuellen Computern | <p>1 der zweiten Generation</p> <p>Der zweiten Generation 2: Windows</p> |  Generation 2 virtuellen Computern mit OS Datenträger Typs einfache Datenträger, wozu 1 oder 2 Datenbestände mit dem Datenträgerformat als VHDX also kleiner als 300GB unterstützt wird. Linux Generation 2-virtuellen Computern werden nicht unterstützt. [Lesen Sie weitere Informationen](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Optimieren der bereitstellungs

Verwenden Sie folgende Tipps, mit deren Hilfe Sie optimieren und Skalieren der bereitstellungs.

- **Betriebssystem Lautstärke Größe**: bei der Replikation eines virtuellen Computers an Azure muss die Lautstärke Betriebssystem weniger als 1 TB. Wenn Sie weitere Datenmengen als diese haben können Sie manuell verschieben diese auf einem anderen Datenträger vor dem start der Bereitstellung.
- **Größe des Datenträgers Daten**: Wenn Sie in Azure repliziert sind haben Sie bis zu 32 Datenfestplatten auf einem virtuellen Computer, jede mit bis zu 1 TB. Sie können effektiv repliziert und über eine ~ 32 TB virtuellen Computern fehl.
- **Grenzwerte für Wiederherstellung Plan**: Website Wiederherstellung für Tausende von virtuellen Computern skalieren können. Wiederherstellung Pläne dienen als Modell für Applikationen, die über zusammen ein Fehler auftreten sollte, damit wir die Anzahl der Computer in einem Wiederherstellungsplan 50 beschränken.
- **Grenzwerte für Azure Service**: alle Azure-Abonnement dann mit einer Reihe von Standardgrenzwerte Kerne, kommt cloud Services usw.. Es empfiehlt sich, dass Sie einen Test-Failover zum Überprüfen der Verfügbarkeit von Ressourcen Ihres Abonnements ausgeführt werden. Sie können diese Grenzwerte über Azure Support ändern.
- **Planen von Kapazität**: Weitere Informationen zum [Planen der Kapazität](site-recovery-capacity-planner.md) für die Website Wiederherstellung.
- **Bandbreite für die Replikation**: Wenn Sie sich auf die Replikation Bandbreite kurze Beachten Sie, dass:
    - **ExpressRoute**: Website Wiederherstellung mit Azure ExpressRoute und WAN-Optimierer beispielsweise Riverbed funktioniert. [Weitere Informationen finden Sie](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) Informationen zu ExpressRoute.
    - **Replikationsdatenverkehr**: Website Wiederherstellung verwendet führt eine smart erste Replikation mithilfe von nur Datenblöcke und nicht die gesamte virtuelle Festplatte aus. Nur die Änderungen werden während einer laufenden Replikation repliziert.
    - **Netzwerkverkehr**: Sie können steuern, Netzwerkdatenverkehr von [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) mit einer Richtlinie basierend auf die Ziel-IP-Adresse und den Port einrichten für die Replikation verwendet.  Darüber hinaus, wenn Sie zum Azure Website Wiederherstellung mithilfe des Sicherung von Azure-Agents repliziert sind können Sie konfigurieren für diesen Agent begrenzungsebene. [Weitere Informationen finden](https://support.microsoft.com/kb/3056159).
- **RTO**: Ziel der Wiederherstellung Zeit (RTO) messen, können Sie mit Website Wiederherstellung erwarten, wir empfehlen Ihnen, führen Sie einen Test-Failover und Ansicht die Website Wiederherstellung Einzelvorgänge zu analysieren wie viel Zeit es dauert, um den Vorgang abzuschließen. Wenn Sie über in Azure fehlschlägt, empfohlen für optimales RTO, durch die Integration mit Azure Automatisierung und Wiederherstellung Pläne alle manuelle Aktionen zu automatisieren.
- **RPO**: Website Wiederherstellung unterstützt ein in der Nähe synchroner Wiederherstellung Punkt Ziel (RPO) bei der Replikation an Azure. Dies setzt voraus ausreichenden Bandbreite zwischen Ihrem Datacenter und Azure.


##<a name="service-urls"></a>Service-URLs
Stellen Sie sicher, dass diese URLs vom Server zugegriffen werden


**URLs** | **VMM VMM** | **VMM in Azure** | **Website Azure Hyper-V** | **VMware zur Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Access erforderlich  | Access erforderlich  | Access erforderlich  | Access erforderlich
 \*. backup.windowsazure.com |  | Access erforderlich  | Access erforderlich  | Access erforderlich
 \*. hypervrecoverymanager.windowsazure.com | Access erforderlich  | Access erforderlich  | Access erforderlich  | Access erforderlich
 \*. store.core.windows.net | Access erforderlich  | Access erforderlich  | Access erforderlich  | Access erforderlich
 \*. blob.core.windows.net |  | Access erforderlich  | Access erforderlich  | Access erforderlich
 https://www.msftncsi.com/ncsi.txt | Access erforderlich  | Access erforderlich  | Access erforderlich  | Access erforderlich
 https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Access erforderlich


## <a name="next-steps"></a>Nächste Schritte

Nach erlernen und Vergleichen von allgemeinen Bereitstellung Anforderungen können Sie die detaillierte Vorkenntnisse lesen und Bereitstellen jedes Szenarios starten.

- [Virtuelle VMware-Computer auf Azure repliziert](site-recovery-vmware-to-azure-classic.md)
- [Physische Server auf Azure repliziert](site-recovery-vmware-to-azure-classic.md)
- [Hyper-V Server in VMM Wolken auf Azure repliziert](site-recovery-vmm-to-azure.md)
- [Hyper-V virtuellen Computern (ohne VMM) auf Azure repliziert](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V virtuellen Computern an einem sekundären Standort repliziert](site-recovery-vmm-to-vmm.md)
- [Repliziert Hyper-V virtuellen Computern an einem sekundären Standort mit SAN](site-recovery-vmm-san.md)
- [Repliziert Hyper-V virtuelle Computer mit einem einzigen VMM-server](site-recovery-single-vmm.md)
