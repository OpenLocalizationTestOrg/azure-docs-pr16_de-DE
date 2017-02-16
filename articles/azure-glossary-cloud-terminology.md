<properties
    pageTitle="Azure-Glossar - Wörterbuch Azure | Microsoft Azure"
    description="Verwenden des Azure-Glossars um zu Cloud Terminologie auf der Azure-Plattform zu verstehen. Dieses kurze Azure Wörterbuch enthält Definitionen für häufig verwendete Cloud Begriffe Azure."
    keywords="Azure Wörterbuch, Cloud-Terminologie, Azure-Glossar, Terminologie Definitionen, Cloud-Ausdrücke"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure-Glossar: ein Wörterbuch der Cloud Terminologie auf der Azure-Plattform

Das Microsoft Azure-Glossar handelt es sich um eine kurze Wörterbuch von Cloud Terminologie für die Azure-Plattform.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Suchen nach Service-Definitionen und anderen Cloud-Ausdrücke

* Definitionen Azure Dienste und deren AWS finden Sie unter Gegenstücken [Microsoft Azure und Amazon-Webdiensten](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Allgemeine Industry finden Sie unter Cloud Ausdrücke [Cloud computing Ausdrücke](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Das Azure-Glossar mit den oben angegebenen zwei Bezügen liegen stellt eine End-to-End-Taxonomie für Azure und die Cloud Industrie.  

## <a name="azure-glossary-list"></a>Liste der Azure-Glossar


### <a name="a-nameaccountaaccount"></a><a name="account"></a>Konto  
Eine Arbeit oder Schule oder persönlichen Microsoft-Konto, das zugreifen und Verwalten eines Azure-Abonnements verwendet wird.  
Siehe auch [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="a-nameavailability-setaavailability-set"></a><a name="availability-set"></a>Festlegen der Verfügbarkeit  
Eine Zusammenstellung von virtuellen Computern, die miteinander zu Anwendung und ein Zuverlässigkeit verwaltet werden. Die Verwendung von eine Sammlung Verfügbarkeit ist sichergestellt, dass während eines geplanten oder nicht geplanten Wartung-Ereignisses mindestens einen virtuellen Computern verfügbar ist.  
Siehe auch [verwalten die Verfügbarkeit von Windows-virtuellen Computern](./virtual-machines/virtual-machines-windows-manage-availability.md) oder [Verwalten Sie die Verfügbarkeit von Linux virtuellen Computern](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="a-nameclassic-modelaazure-classic-deployment-model"></a><a name="classic-model"></a>Klassische Azure-Bereitstellungsmodell  
Einer der beiden [Bereitstellungsmodelle](resource-manager-deployment-model.md) zur Bereitstellung von Ressourcen in Azure (das neue Modell ist Azure Ressourcenmanager) verwendet. Einige Azure Ressourcen können in einem Modell oder das andere bereitgestellt werden, während andere Personen in beiden Modellen bereitgestellt werden können. Leitfaden für einzelne Azure Ressourcen Details der model(s) eine Ressource kann mit bereitgestellt werden.


### <a name="a-namecliaazure-command-line-interface-cli"></a><a name="cli"></a>Azure line Interface (CLI)  
Eine [Befehlszeile](xplat-cli-install.md) , die zum Verwalten der Azure-Dienste von Windows, OSX und Linux PCs verwendet werden können.


### <a name="a-namepowershellaazure-powershell"></a><a name="powershell"></a>Azure PowerShell  
Eine [Line Benutzeroberfläche](powershell-install-configure.md) zum Verwalten der Azure-Diensten über eine Befehlszeile von Windows-PCs. Einige Dienste oder Servicefunktionen können nur über PowerShell oder der CLI verwaltet werden. Leitfaden für jede einzelne Azure Ressourcendetails, die model(s) eine Ressource kann mit bereitgestellt werden.   
Siehe auch [So installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md)


### <a name="a-namearm-modelaazure-resource-manager-deployment-model"></a><a name="arm-model"></a>Modell zur Bereitstellung von Azure Ressourcenmanager  
Einer der beiden [Bereitstellungsmodelle](resource-manager-deployment-model.md) zur Bereitstellung von Ressourcen in Microsoft Azure (die andere ist das Bereitstellungsmodell klassischen). Einige Azure Ressourcen können in einem Modell oder das andere bereitgestellt werden, während andere Personen in beiden Modellen bereitgestellt werden können. Leitfaden für einzelne Azure Ressourcen Details der model(s) eine Ressource kann mit bereitgestellt werden.


### <a name="a-namefault-domainafault-domain"></a><a name="fault-domain"></a>Fehlerstrukturanalyse-Domäne  
Die Sammlung von virtuellen Computern in einer Verfügbarkeit festlegen, die zur gleichen Zeit möglicherweise ein Fehler auftreten können. Ein Beispiel ist eine Gruppe von Computern in einer den Shapes für Gestelle, die eine allgemeine Power Quell- und Netzwerk wechseln freigeben. In Azure den virtuellen Computern in einem Satz Verfügbarkeit automatisch in mehreren Fehlerstrukturanalyse-Domänen voneinander zu trennen.  
Siehe auch [verwalten die Verfügbarkeit von Windows-virtuellen Computern](./virtual-machines/virtual-machines-windows-manage-availability.md) oder [Verwalten Sie die Verfügbarkeit von Linux virtuellen Computern](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="a-namegeoageo"></a><a name="geo"></a>Geo  
Ein definierter Begrenzungslinie für Ort Daten, die in der Regel zwei oder mehr Bereiche enthält. Die Begrenzung möglicherweise innerhalb oder außerhalb der nationalen Rahmen und werden nach Steuern Regel beeinflusst. Jeder Geo enthält mindestens eine Region. Beispiele für Geos sind asiatisch-pazifischen Raum und Japan. Auch als *"geography"*bezeichnet.  
Siehe auch [Azure Regionen](best-practices-availability-paired-regions.md)


### <a name="a-namegeo-replicationageo-replication"></a><a name="geo-replication"></a>Geo-Replikation  
Der Vorgang automatisch repliziert Inhalte wie Blobs, Tabellen und innerhalb einer regionalen Paar.  
Siehe auch [Aktive Geo-Replikation für SQL Azure-Datenbank](./sql-database/sql-database-geo-replication-overview.md)


### <a name="a-nameimageaimage"></a><a name="image"></a>Bild  
Eine Datei, die das Betriebssystem und Anwendungskonfiguration, die verwendet werden kann, erstellen Sie eine beliebige Anzahl von virtuellen Computern enthält. In Azure es gibt zwei Arten von Bildern: virtueller Computer und OS Bild. Ein virtueller Computer Bild enthält ein Betriebssystem und alle Datenträger, eines virtuellen Computers angefügt werden, wenn das Bild erstellt wird. Ein OS Bild enthält nur eine GRG Betriebssystem mit Konfigurationen keine Daten an.  
Siehe auch [Navigieren und Auswählen von Windows virtuellen Computern Bilder in Azure mit PowerShell oder der CLI](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="a-namelimitsalimits"></a><a name="limits"></a>Grenzwerte  
Die Anzahl der Ressourcen, die erstellt werden können oder der Leistung Maßstab, die erreicht werden können. In der Regel Abonnements, Services und Angebote zugeordnet ist.  
Siehe auch [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](azure-subscription-service-limits.md)


### <a name="a-nameload-balanceraload-balancer"></a><a name="load-balancer"></a>Lastenausgleich  
Eine Ressource, die eingehenden Datenverkehr zwischen Computern in einem Netzwerk verteilt. In Azure verteilt ein Lastenausgleich den Datenverkehr in virtuellen Computern, die in einem Lastenausgleich definiert. Ein [System zum Lastenausgleich laden](./load-balancer/load-balancer-overview.md) kann Internet zugänglichen oder intern sein können.  


### <a name="a-nameofferaoffer"></a><a name="offer"></a>Angebot  
Die Preise, Gutschriften und zusammengehöriger Ausdrücke zu einem Azure Abonnement verfügbar.  
Finden Sie unter der [Detailseite anbieten Azure](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="a-nameportalaportal"></a><a name="portal"></a>Portal  
Das sichere Web-Portal zum Bereitstellen und Verwalten von Azure-Diensten verwendet.  Es gibt zwei communityportalen: [Azure-Portal](http://portal.azure.com/) und das [klassische Portal](http://manage.windowsazure.com/). Einige Dienste stehen in beiden communityportalen, während andere nur in eine oder das andere zur Verfügung stehen. Die [von Azure Portals Verfügbarkeitsdiagramm](https://azure.microsoft.com/features/azure-portal/availability/) -Listen auf welche Dienste welche-Portal zur Verfügung stehen.  


### <a name="a-nameregionaregion"></a><a name="region"></a>Region  
Ein Bereich innerhalb einer Geo, das nicht Kreuz nationale Rahmen, und enthält mindestens Rechenzentren. Preise, Landes-/ Services und Angebotstypen werden auf der Regionsebene verfügbar gemacht. Ein Bereich ist in der Regel mit einem anderen Bereich, kombiniert, die bis zu mehreren hundert Meilen abwesend sein kann, ein paar regionalen bilden. Landes-/ Paare können als ein Verfahren für Wiederherstellung und Szenarios mit hoher Verfügbarkeit verwendet werden. So genannte im allgemeinen *Speicherort*.  
Siehe auch [Azure Regionen](best-practices-availability-paired-regions.md)


### <a name="a-nameresourcearesource"></a><a name="resource"></a>Ressource  
Ein Element, das Bestandteil Ihrer Azure Lösung ist. Jede Azure Service können Sie verschiedene Arten von Ressourcen, wie z. B. Datenbanken oder virtuellen Computern bereitstellen.   
Siehe auch [Azure Ressourcenmanager (Übersicht)](azure-resource-manager/resource-group-overview.md)


### <a name="a-nameresource-grouparesource-group"></a><a name="resource-group"></a>Ressourcengruppe  
Ein Container in Ressourcenmanager, der zugehörige Ressourcen für eine Anwendung enthält. Die Ressourcengruppe kann einbeziehen aller Ressourcen für eine Anwendung oder nur die Ressourcen, die logisch zusammen gruppiert sind. Sie können entscheiden, wie Zuordnen von Ressourcen zu Ressourcengruppen basierend auf am sinnvollsten für Ihre Organisation Besonderheiten werden soll.  
Siehe auch [Azure Ressourcenmanager (Übersicht)](azure-resource-manager/resource-group-overview.md)


### <a name="a-namearm-templatearesource-manager-template"></a><a name="arm-template"></a>Ressourcenmanager-Vorlage  
Eine JSON-Datei, die eine oder mehrere Azure Ressourcen deklarativ definiert und Abhängigkeiten zwischen den bereitgestellten Ressourcen definiert. Die Vorlage kann verwendet werden, die Ressourcen einheitlich und wiederholt bereitstellen.  
Siehe auch [Azure Ressourcenmanager Authoring-Vorlagen](resource-group-authoring-templates.md)


### <a name="a-nameresource-provideraresource-provider"></a><a name="resource-provider"></a>Anbieter für Ressourcen  
Ein Dienst, der die Ressourcen liefert, können Sie bereitstellen und Verwalten von durch Ressourcenmanager. Jeder Anbieter für Ressourcen bietet Vorgänge für das Arbeiten mit den Ressourcen, die bereitgestellt werden. Ressourcenanbieter können über die Azure-Portal, Azure PowerShell und mehrere programming SDKs zugegriffen werden.  
Siehe auch [Azure Ressourcenmanager (Übersicht)](azure-resource-manager/resource-group-overview.md)


### <a name="a-namerolearole"></a><a name="role"></a>Rolle  
Eine Möglichkeit zum Steuern des Zugriffs auf, die Benutzer, Gruppen und Dienste zugeordnet werden kann. Rollen sind Aktionen durchführen zu erstellen, zum Verwalten und Azure Ressourcen zu lesen.  
Siehe auch [RBAC: integrierte Rollen](./active-directory/role-based-access-built-in-roles.md)


### <a name="a-nameslaaservice-level-agreement-sla"></a><a name="sla"></a>Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL)  
Vereinbarung, die von Microsoft Zusagen für Verfügbarkeit und Verbindungsproblemen beschreibt. Jede Azure Service verfügt über eine bestimmte Vereinbarung zum SERVICELEVEL.  
Siehe auch [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/)


### <a name="a-namestorage-accountastorage-account"></a><a name="storage-account"></a>Speicher-Konto  
Ein Speicherkonto, das Sie bietet Zugriff auf die Dienste Azure Blob, Warteschlange, Tabelle und Datei Azure-Speicher. Ihr Speicherkonto stellt den eindeutigen Namespace für Ihre Datenobjekte Azure-Speicher.  
Siehe auch [zur Azure-Speicher-Konten](./storage/storage-create-storage-account.md)


### <a name="a-namesubscriptionasubscription"></a><a name="subscription"></a>Abonnement  
Zustimmung des Kunden mit Microsoft, mit deren Azure Dienste abrufen kann. Die Preise Abonnement und verwandte Begriffe unterliegen das Angebot für das Abonnement ausgewählt wurde. Finden Sie unter [Microsoft Online-Abonnement-Vertrag](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Siehe auch [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="a-nametagatag"></a><a name="tag"></a>Kategorie  
Eine Indizierung Ausdruck, den Sie Ressourcen nach Ihren Anforderungen zu verwalten oder Abrechnung kategorisieren kann. Sie können die Kategorien verwenden, wenn Sie eine komplexe Sammlung von Ressourcen- und Ressourcen haben, und Sie müssen diese Ressourcen auf die Art und Weise zu visualisieren, die am sinnvollsten ist. Beispielsweise könnten Sie Ressourcen markieren, die eine ähnliche Rolle in Ihrer Organisation dienen oder zu derselben Abteilung gehören.  
Siehe auch [Verwenden von Kategorien zum Organisieren Ihrer Azure-Ressourcen](resource-group-using-tags.md)


### <a name="a-nameupdate-domainaupdate-domain"></a><a name="update-domain"></a>Domäne aktualisieren  
Die Sammlung von virtuellen Computern in einer Gruppe von Verfügbarkeit, die zur gleichen Zeit aktualisiert werden. Virtuellen Computern in der gleichen Domäne aktualisieren werden zusammen während der geplanten Wartung neu gestartet. Azure startet nie mehr als ein Update Domain nacheinander neu. Wird auch als Upgrade Domäne bezeichnet.  
Siehe auch [verwalten die Verfügbarkeit von Windows-virtuellen Computern](./virtual-machines/virtual-machines-windows-manage-availability.md) oder [Verwalten Sie die Verfügbarkeit von Linux virtuellen Computern](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="a-namevmavirtual-machine"></a><a name="vm"></a>virtuellen Computern  
Die Software-Implementierung von einem physischen Computer, der ein Betriebssystem ausgeführt wird. Mehrere virtuelle Computer können gleichzeitig auf derselben Hardware ausgeführt werden. In Azure sind die virtuellen Computern in einer Vielzahl von Größen verfügbar.  
Siehe auch [Dokumentation virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="a-namevm-extensionavirtual-machine-extension"></a><a name="vm-extension"></a>Erweiterung virtuellen Computern  
Eine Ressource, das Verhalten oder Funktionen, die entweder bei anderen Programmen arbeiten, oder es ermöglichen Sie interagieren mit einer laufenden Computer implementiert. Beispielsweise können Sie die Erweiterung Zugriff virtueller Computer zurücksetzen oder RAS Werten in einer Azure-virtuellen Computern ändern.  
Siehe auch [über Erweiterungen virtuellen Computern und Funktionen (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) oder [über virtuellen Computern Extensions und Features (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="a-namevnetavirtual-network"></a><a name="vnet"></a>virtuelles Netzwerk  
Ein Netzwerk, das stellt eine Verbindung zwischen Ihrem Azure Ressourcen, die von allen anderen Azure Mandanten isoliert ist. Sie können mit anderen Azure virtuellen Netzwerken über ein [VPN-Gateway Azure](./vpn-gateway/vpn-gateway-about-vpngateways.md) und mit Ihrem lokalen Netzwerk mit [mehreren Optionen](./vpn-gateway/vpn-gateway-cross-premises-options.md)verbunden sein. Sie können die IP-Adressblöcke, DNS-Einstellungen, Sicherheitsrichtlinien und Routing-Tabellen in diesem Netzwerk vollständig steuern.  
Siehe auch [virtuelle Network (Übersicht)](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Siehe auch**  
- [Erste Schritte mit Azure](https://azure.microsoft.com/get-started/)
- [Ressourcencenter für die Cloud](https://azure.microsoft.com/resources/)  
- [Azure für Ihre geschäftliche Anwendung](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure in Ihrem Datencenter](https://azure.microsoft.com/overview/business-apps-on-azure/) 





