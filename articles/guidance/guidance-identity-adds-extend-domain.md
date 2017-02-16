<properties
   pageTitle="Azure-Architektur Referenz - IaaS: Erweitern von Active Directory in Azure | Microsoft Azure"
   description="Wie Sie eine sichere Hybrid Netzwerkarchitektur mit Active Directory-Autorisierung in Azure implementieren."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Erweitern Sie in Azure Active Directory-Verzeichnisdienst (ADDS)

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel werden bewährte Methoden für die Erweiterung Ihrer Umgebung Active Directory (AD) in Azure, um verteilte Authentifizierung-Dienste bereitzustellen, mithilfe von [Active Directory-Domänendiensten (AD DS)][active-directory-domain-services]. Diese Architektur erweitert, die in den Artikeln [Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure] beschriebenen[ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Internetzugang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Bezug Architektur verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

Verwenden Sie AD DS mit Identitäten authentifiziert. Diese Identitäten können Benutzer, Computer, Applikationen oder andere Ressourcen, die Teil einer Sicherheitsdomäne angehören. Sie können AD DS lokalen hosten, aber die in einem Hybriden Szenario, in dem Elemente einer Anwendung in Azure ansässig sind, es effizienter repliziert diese Funktionalität und dem AD Repository in der Cloud. Dieser Ansatz kann die Verzögerung durch Senden Authentifizierung verringern und lokale Autorisierungsanfragen aus der Cloud in AD DS ausgeführt lokalen zurück. 

Es gibt zwei Methoden zum Hosten Ihrer Verzeichnisdienste in Azure aus:

1. Sie können [Azure Active Directory (AAD)] [ azure-active-directory] zum Erstellen einer neuen AD-Domäne in der Cloud und verknüpfen Sie es mit einer lokalen AD-Domäne. Setup wird [Azure AD verbinden] [ azure-ad-connect] Versprechen repliziert Identitäten frei, die der lokalen Repository in der Cloud. Beachten Sie, dass das Verzeichnis in der Cloud **** keine Erweiterung des lokalen Systems lieber es ist eine Kopie, die die gleiche Identität enthält. An diese Identitäten, die lokal in der Cloud, aber in der Cloud **nicht** vorgenommenen Änderungen kopiert werden vorgenommene Änderungen werden wieder auf die lokale Domäne repliziert. Angenommen, müssen das Zurücksetzen von Kennwörtern werden lokale ausgeführte und Azure AD verbinden verwenden, um die Änderung in der Cloud zu kopieren. Beachten Sie auch, mit die gleiche Instanz von AAD kann mehr als eine Instanz von AD DS verknüpft sein. AAD enthält die Identitäten der einzelnen AD-Repository, mit dem sie verknüpft ist.

    AAD eignet sich für Situationen, in dem die lokale und Azure virtuelle Netzwerk Hostinganbieter die Cloudressourcen nicht direkt verknüpft sind mithilfe eines VPN-Tunnel oder ExpressRoute Verbindung. Obwohl diese Lösung einfach ist, es möglicherweise nicht für Systeme geeignet, wo konnte Komponenten über die lokale/Cloud Grenze migrieren wie folgt neu konfiguriert AAD benötigen könnte. Darüber hinaus übernimmt AAD nur Benutzerauthentifizierung statt Computerauthentifizierung. Einige Anwendungen und Dienste, wie etwa SQL Server, Computerauthentifizierung erfordern möglicherweise in diesem Fall diese Lösung nicht geeignet ist.

2. Sie können einen virtuellen AD DS als Domänencontroller in Azure ausgeführt, erweitern Ihre vorhandene AD-Infrastruktur aus Ihrem lokalen Datacenter bereitstellen. Dieser Ansatz empfiehlt häufiger für Szenarien, wo die lokale und Azure virtuelle Netzwerk durch eine Verbindung VPN und/oder ExpressRoute verbunden sind. Diese Lösung unterstützt auch bidirektionale Replikation aktivieren Sie nehmen Sie Änderungen in der Cloud und lokalen, wo es am besten geeignet ist. Die AD-Installation in der Cloud sind je nach Ihren Anforderungen Sicherheit möglich:
    - Teil der gleichen Domäne wie die belegten lokal
    - eine neue Domäne innerhalb einer freigegebenen
    - eine separate Gesamtstruktur

<!-- we might want to add info on how to choose from the three options above -->

Diese Architektur befasst sich mit Lösung 2, mit der gleichen AD DS-Domäne in Azure und lokale.

Typische Nutzung Fällen für diese Architektur umfassen:

- Hybrid Applications, wo Auslastung ausführen teilweise lokalen, und teilweise in Azure.

- Anwendungen und Dienste, die Authentifizierung mithilfe von Active Directory ausführen.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur (*Klicken Sie zum Vergrößern auf*). Weitere Informationen zu den Elementen abgeblendet angezeigt werden, finden Sie unter [Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure] [ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Internetzugang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Lokale Netzwerk.** Das lokale Netzwerk enthält lokale AD-Servern, die Authentifizierung und Autorisierung für-Komponenten lokal ausführen können.

- **AD-Servern.** Hierbei handelt es sich um Domänencontroller implementieren Directory Services (AD DS) als virtuellen Computern in der Cloud ausgeführt. Diese Server können Authentifizierung von Komponenten, die im virtuellen Netzwerks Azure bereitstellen.

- **Active Directory-Subnetz.** AD DS-Servern gehostet werden in einem separaten Subnetz. NSG Regeln Schützen der AD DS-Servers und eine Firewall gegen den Datenverkehr von unerwarteten Quellen bereitstellen.

- **Azure Gateway und Active Directory-Synchronisierung.**. Das Gateway Azure bietet eine Verbindung zwischen dem lokalen Netzwerk und die VNet Azure. Dies kann eine [VPN-Verbindung] sein[ azure-vpn-gateway] oder [Azure ExpressRoute][azure-expressroute]. Alle Synchronisierungsanfragen zwischen den AD-Servern in der Cloud und lokale passieren des Gateways. Benutzerdefinierte leitet (UDRs) behandeln routing für den Datenverkehr für die Synchronisierung übergibt direkt an den AD-Server in der Cloud und verläuft nicht durch die vorhandene virtuelle Netzwerkgeräte (NVAs) in diesem Szenario verwendet.

    Weitere Informationen zum Konfigurieren von UDRs und der NVAs finden Sie unter [Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Empfehlungen

In diesem Abschnitt werden die Vorgehensweisen für die Implementierung von AD DS in Azure, zusammengefasst darüber liegendem:

- Virtueller Computer Empfehlungen.

- Empfehlungen für Netzwerke.

- Empfehlungen im Zusammenhang mit Sicherheit. 

- Active Directory-Standort Empfehlungen.

- Active Directory FSMO-Platzierung Empfehlungen.

>[AZURE.NOTE] Das Dokument [Richtlinien zum Bereitstellen von Windows Server Active Directory auf Azure virtuellen Computern] [ ad-azure-guidelines] enthält ausführliche Informationen zu viele dieser Punkte.

### <a name="vm-recommendations"></a>Virtueller Computer Empfehlungen

Erstellen Sie virtuellen Computern mit ausreichenden Ressourcen, um der erwarteten Umfang des Datenverkehrs zu behandeln. Verwenden Sie die Größe der Computer, auf denen AD DS lokal als Ausgangspunkt. Die Nutzung von Ressourcen zu überwachen; Sie können Ändern der Größe der virtuellen Computern und verkleinern, wenn sie zu groß sind. Weitere Informationen zu AD DS-Domänencontroller Ziehpunkt, finden Sie unter [Kapazität, Planung Active Directory-Domänendiensten][capacity-planning-for-adds].

Erstellen Sie einen separaten virtuellen Datenträger zum Speichern von der Datenbank, Protokolle und SYSVOL für Active Directory. Speichern Sie diese Elemente nicht auf der gleichen Festplatte wie das Betriebssystem. Beachten Sie, dass standardmäßig Datenfestplatten, die um einen virtuellen Computer angeschlossen sind, Schreiben durch Zwischenspeichern. Jedoch kann diese Form des Zwischenspeichern mit den Anforderungen der AD DS in Konflikt stehen. Aus diesem Grund legen Sie die Einstellung *Host Cache bevorzugte* auf dem Datenträger Daten *keine*aus. Weitere Informationen finden Sie unter [Windows Server AD DS-Datenbank und SYSVOL Platzierung][adds-data-disks].

Bereitstellen von mindestens zwei virtuellen Computern AD DS als Domäne-Controller mit Ihrem Azure virtuelle Netzwerk aus Gründen der [Verfügbarkeit](#Availability-considerations) ausgeführt.

### <a name="networking-recommendations"></a>Netzwerke Empfehlungen

Konfigurieren der Schnittstelle für jede der virtuellen Computern Hostinganbieter AD DS statische private IP-Adressen. Diese Konfiguration unterstützt besser DNS auf jeder der AD virtueller Computer an. Weitere Informationen finden Sie unter [So legen Sie eine statische private IP-Adresse der Azure-Portal][set-a-static-ip-address].

> [AZURE.NOTE] Geben Sie die öffentlichen IP-Adressen von AD DS virtuellen Computern nicht. [Zur Sicherheit] finden Sie unter[ security-considerations] Weitere Details.

Sie müssen sicherstellen, dass Datenverkehr zwischen den AD-Servern in der Cloud und lokalen frei Datenfluss kann:

- Hinzufügen von NSG Regeln mit dem AD-Subnetz, die eingehenden Datenverkehr aus lokalen ermöglichen. Ausführliche Informationen zu den Ports, die AD DS verwendet wurden, finden Sie unter [Active Directory und Active Directory Domain Services Port Anforderungen][ad-ds-ports].

- Stellen Sie sicher, dass UDR Tabellen AD DS-Verkehr über die in diesem Szenario verwendeten NVAs nicht weiterleiten, gehen Sie wie folgt. Für Ihre eigenen Bereitstellungen, je nachdem welche NVAs Sie verwenden möchten, können Sie dieser Datenverkehr umleiten möchten.

### <a name="security-recommendations"></a>Sicherheit Empfehlungen

AD DS-Server Authentifizierung verarbeitet und daher sehr vertrauliche Elemente. Verhindern, dass direkte Anzeigen der AD DS-Server mit dem Internet. Platzieren Sie in einem separaten Subnetz, mit einem eigenen Firewall AD DS-Servers ein. Stellen Sie sicher, dass die erforderlichen AD DS für Authentifizierung und Autorisierung und für Synchronisation Server verwenden Port geöffnet bleiben, aber schließen Sie alle anderen Ports. Weitere Informationen finden Sie unter [Active Directory und Active Directory Domain Services Port Anforderungen][ad-ds-ports]. Die [Komponenten einer Lösung] [ solution-components] weiter unten in diesem Dokument zeigt die NSG Regeln für die von die Stichprobe Lösung wird verwendet, um die Ports zu öffnen.

Sie können NSG Regeln zum Erstellen eines einfachen Firewalls verwenden. Wenn Sie umfassenderen Schutz anfordern können Sie eine zusätzliche Sicherheit Umfang Servern mithilfe von ein Paar von Adresssubnetzen und NVAs, implementieren, durch das Dokument [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Internetzugang in Azure]beschriebenen[implementing-a-secure-hybrid-network-architecture-with-internet-access].

Verwenden Sie entweder BitLocker oder Azure Datenträger Verschlüsselung zum Verschlüsseln des Datenträgers Hostinganbieter AD DS-Datenbank.

### <a name="active-directory-site-recommendations"></a>Active Directory-Standort Empfehlungen

Sie können Websites in AD DS für die Gruppe nicht trennen Domänencontroller verwenden, die durch eine schnelle Verbindung verbunden sind. Controller in der gleichen AD DS-Website die Änderungen Directory automatisch repliziert und etwas Konfiguration zur Behandlung von der Replikations erforderlich ist.

Um Replikationsdatenverkehr zwischen Azure und Datencenters lokale steuern möchten, empfiehlt es sich zum Hinzufügen einer separaten AD DS-Website, um die Darstellung der Adresse Leerzeichen von Azure verwendet. Sie können einen Link zu konfigurieren zwischen der lokalen AD DS Websites und websiteübergreifende Replikation noch effektiver zu steuern.

Trennung der Website können Sie auch andere Richtlinienobjekte gruppieren (GPOs) zu verbindenden Computern in Azure und Nutzen von Applications, "Website bekannt", z. B. System Center-Konfigurations-Manager stehen, anwenden.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO-Platzierung Empfehlungen

Flexible Single Master Operation (FSMO) Servern sind spezielle Domänencontroller, Reposnsible für Datenkonsistenz in verschiedenen Einstellungen in AD DS, aufgeführten.

- **Schema Master-Shape**. Dies ist eine gesamtstrukturweite Rolle, die die Struktur des Schemas AD DS untersuchten unterhält.

- **DNS-Master**. Dies ist eine gesamtstrukturweite Rolle aus, die Informationen zu Domänennamen innerhalb der Gesamtstruktur verwaltet werden.

- **Primäre Domäne Domänencontroller(PDC)**. Dies ist eine Domäne organisationsweite Rolle aus. Der PDC übernimmt Kennwort Updates und Konto sperren. Es wird von anderen DCs gehört, wenn Authentifizierung Serviceanfragen nicht übereinstimmende Kennwörter haben. Der PDC auch Gruppenrichtlinien Updates behandelt, und ist der Zieldomänencontroller für Applikationen älterer Versionen und einige Administrator-Tools, die einige beschreibbare Vorgänge ausführen.

- **Relative ID (RID) Master-Shape**. RIDs werden verwendet, um die Objekte innerhalb eines Verzeichnisses eindeutig identifizieren helfen. Dieser Server ist für das Generieren einer Domäne organisationsweite Ressourcenpool RID für die Verwendung von allen AD-Servern in der Domäne verantwortlich.

- **Infrastrukturrolle**. Ein Objekt in eine Domäne kann ein Objekts in einer anderen Domäne verwiesen werden. Diese Rolle Domänen-ist zum Aktualisieren des Objekts SID und Name [distinguished Name in einen Bezug Domain-übergreifende Objekt verantwortlich. Beachten Sie, dass ein Server implementieren diese Rolle auch als globalen Katalog (globalen Katalog) Server handeln kann.

Weitere Informationen finden Sie unter [Active Directory-FSMO-Funktionen in Windows][AD-FSMO-roles-in-windows].

In diesem Szenario wird empfohlen, dass Sie die Bereitstellung von FSMO-Funktionen für die Domänencontroller in Azure vermeiden. 

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Erstellen einer Verfügbarkeit für die AD-Server festlegen. Stellen Sie sicher, dass mindestens zwei Server festlegen. Die AD-Servern in der Cloud sollte Controller innerhalb derselben Domäne sein. Dadurch wird die automatische Replikation zwischen Servern ermöglicht.

Erwägen Sie auch, Festlegen von Servern als [Master standby Vorgänge] [ standby-operations-masters] für den Fall, dass Sie Verbindung zu einem Server fungiert als eine FSMO-Rolle schlägt fehl.

## <a name="security-considerations"></a>Zur Sicherheit

Wenn alle Domäne Administratoraufgaben wahrscheinlich unter Verwendung der lokalen DCs durchgeführt werden, erwägen Sie die Erstellung von DCs in der Cloud, schreibgeschützt. Ein schreibgeschützte DC nur unterhält eine Teilmenge der Anmeldeinformationen von Benutzern (ausreichend zur Durchführung der Authentifizierung lokal) und Informationen zwischengespeichert werden nur für bestimmte Benutzer konfiguriert werden kann. Daher ist ein schreibgeschützter DC gefährdet, sind nur die Informationen für den Cache Benutzer, und die Details für jedes Konto in der Domäne nicht betroffen. Weitere Informationen finden Sie unter [Schreibgeschützten Domain Controller][read-only-dc].

Das Sicherheitsrisiko von einzelnen Benutzerkonten zu minimieren, und Versuche, Einbruch zu verhindern, führen Sie die bewährte Methode für einrichten und Verwalten von Benutzerkennwörter in Active Directory. Weitere Informationen finden Sie unter [Bewährte Methoden für Kennwortrichtlinien erzwingen][best_practices_ad_password_policy]. Darüber hinaus werden Sie hierbei darauf, dass Sie die Gruppen aus, die Benutzern zuweisen. Nehmen Sie beispielsweise nicht gewöhnliche Benutzer Mitglieder der Gruppe Organisations-Admins, Gruppe Schema-Admins und Domänen-Admins.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

AD ist automatisch skalierbar für Domänencontroller, die Teil der gleichen Domäne sind. Anfragen werden auf alle Controller innerhalb einer Domäne verteilt. Sie können eine andere Domänencontroller hinzufügen, und es werden automatisch mit der Domäne synchronisiert. Konfigurieren Sie ein separates Lastenausgleich, um den Datenverkehr in Controller innerhalb der Domäne direkte nicht. Stellen Sie sicher, dass alle Controller ausreichenden Arbeitsspeicher und Speicherressourcen die Domänendatenbank verarbeitet haben; Stellen Sie alle Domänencontroller virtuellen Computern idealerweise die gleiche Größe.

## <a name="management-considerations"></a>Gesichtspunkte

Kopieren Sie die Dateien virtuelle Festplatte der Domänencontroller kein regelmäßigen Sicherungen durchgeführt werden, weil Wiederherstellen von Benutzern Inkonsistenzen Zustand zwischen Domänencontroller auftreten kann nicht.

Beenden und neu starten eines virtuellen Computers, die die Domänencontrollerrolle in Azure innerhalb des Betriebssystems Gast anstelle von die Option Beenden im Portal Azure ausgeführt wird. Über das Azure-Portal um zu beenden eines virtuellen Computers bewirkt, dass den virtuellen Computer freigegeben werden. Diese Aktion setzt virtueller Computer-GenerationID, welche für eine Domänencontroller unerwünscht ist. Wenn die virtuellen Computer-GenerationID zurückgesetzt wird, wird auch der InvocationID des Repositorys AD zurückgesetzt., der RID-Pool verworfen und SYSVOL als nicht autoritativer gekennzeichnet ist.

## <a name="monitoring-considerations"></a>Überlegungen für die Überwachung

Weiß nicht, überwachen und Verwalten eines Netzwerks von AD-Servern kann Probleme wie führen:

- **Fehler bei der Anmeldung**. Anmeldefehler kann in der gesamten Domäne oder der Gesamtstruktur auftreten, wenn eine Vertrauensstellung Beziehung oder Namen Auflösung fehlschlägt oder ein globalen Katalogserver universeller Gruppenmitgliedschaft bestimmen kann.

- **Konto sperren**. Benutzer- und Dienstkonten können gesperrt werden, wenn der primäre Domänencontroller nicht verfügbar ist oder Replikation zwischen mehreren Domain Controller schlägt fehl.

- **Fehler bei der Domänencontroller**. Wenn das Laufwerk mit der Datei Ntds.dit nicht genügend Speicherplatz ausgeführt wird, kann der Domänencontroller funktionieren nicht mehr.

- **Anwendungsfehler**. Anwendungen, die sind entscheidend, Ihr Unternehmen, wie etwa Microsoft Exchange oder eine andere e-Mail-Anwendung können fehlschlagen Adresse Adressbuch Abfragen, in dem Verzeichnis fehlschlagen.

- **Inkonsistente Directory-Daten**. Wenn die Replikation für eine längere Zeit fehlschlägt, doppelte Objekte im Verzeichnis erstellt werden können und für möglicherweise umfangreiche Diagnose- und die Uhrzeit, zu beseitigen erforderlich.

- **Konto Fehler beim Erstellen eines**. Ein Domänencontroller kann keine Benutzer- oder Konten zu erstellen, wenn sie ihre Abgabe relativer IDs verbraucht und die RID-nicht verfügbar ist.

- **Bei der Richtlinie Sicherheit**. Wenn der freigegebene Ordner SYSVOL nicht ordnungsgemäß repliziert wird, sind Gruppenrichtlinien Objekte und Sicherheitsrichtlinien nicht ordnungsgemäß mit Clients angewendet.

Überwachen Sie AD-Servern sorgfältig und bereiten Sie sich auf Maßnahmen schnell zu treffen. Erstellen einer Checkliste für die Überwachung Aufgaben zu einem geeigneten Zeitpunkt ausgeführt werden. Beispielsweise könnten Sie die folgenden wichtigen Aufgaben täglich planen:

- Untersuchen und Beheben von Domain Controller gemeldeten Benachrichtigungen, 

- Stellen Sie sicher, dass alle Controller kommunizieren können und die Replikation funktioniert.

- Stellen Sie sicher, dass die freigegebenen SYSVOL erhalten bleibt.

- Korrigieren Sie Probleme bei der Zeit-Synchronisierung.

- Prüfen Sie Speicherplatz auf physischen Laufwerken von AD verwendet.

Sie können andere routinemäßige weniger häufig Aufgaben. Angenommen, Sie überprüfen Trust Beziehungen und veraltete oder fehlerhafte Vertrauensstellungen wöchentlich überprüfen und stellen Sie sicher, dass alle Controller mit der gleichen Servicepacks und Patches wichtiges beheben monatlich ausgeführt werden.

Weitere Informationen finden Sie unter [Überwachen von Active Directory][monitoring_ad]. Sie können Tools wie [Microsoft Systems Center] installieren[ microsoft_systems_center] auf dem überwachenden Server (finden Sie unter [Architekturdiagramm][architecture]) helfen, die folgenden Aufgaben ausführen. 

## <a name="solution-components"></a>Komponenten einer Lösung

Die Lösung für diese Architektur eine sichere Hybrid Netzwerk erstellt, wie durch [Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure] Dokumente beschrieben bereitgestellten[ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Internetzugang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], aber durch das Hinzufügen der folgenden Elemente:

- Eine Azure Ressourcengruppe mit dem Namen *Basename*- Dns-Rg, wo finde ich *Basename* ein Präfix Sie angeben, wenn Sie die Lösung bereitstellen.

- Zwei Azure virtuellen Computern aufgerufen *Basename*-ad1-virtueller Computer und *Basename*-ad2-virtueller Computer, in der Gruppe *Basename*- Dns-Rg Ressource erstellt. Diese virtuellen Computern sind als AD-Servern mit Directory Services und DNS installiert und konfiguriert konfiguriert.

- Eine NSG benannte *Basename*-Ad-Nsg in der Ressourcengruppe *Basename*- Ntwk-Rg Azure. Diese Ressourcengruppe ist Bestandteil der Infrastruktur, die im Netzwerk sichere Hybrid bilden, aber die neue NSG ist eine Ergänzung, die definiert eingehende Sicherheitsregeln für die AD-Server wie in der folgenden Tabelle dargestellt:


    Priorität|Namen|Datenquelle|Ziel|Dienst|Aktion|
    --------|----|------|-----------|-------|------|
    170|Vnet-zu-port53|10.0.0.0/16|Alle|Custom(Any/53)|Zulassen|
    180|Vnet-zu-port88|10.0.0.0/16|Alle|Custom(Any/88)|Zulassen|
    190|Vnet-zu-port135|10.0.0.0/16|Alle|Custom(Any/135)|Zulassen|
    200|Vnet bis port137 9|10.0.0.0/16|Alle|Custom(Any/137-139)|Zulassen|
    210|Vnet-zu-port389|10.0.0.0/16|Alle|Custom(Any/389)|Zulassen|
    220|Vnet-zu-port464|10.0.0.0/16|Alle|Custom(Any/464)|Zulassen|
    230|Vnet Rpc-dynamic|10.0.0.0/16|Alle|Custom(Any/49152-65535)|Zulassen|
    240|Onprem Ad zu port53|192.168.0.0/24|Alle|Custom(Any/53)|Zulassen|
    250|Onprem Ad zu port88|192.168.0.0/24|Alle|Custom(Any/88)|Zulassen|
    260|Onprem Ad zu port135|192.168.0.0/24|Alle|Custom(Any/135)|Zulassen|
    Um 270 Grad|Onprem Ad zu port389|192.168.0.0/24|Alle|Custom(Any/389)|Zulassen|
    280|Onprem Ad zu port464|192.168.0.0/24|Alle|Custom(Any/464)|Zulassen|
    290|Management Rdp zulassen|10.0.0.128/25|Alle|Custom(Any/3389)|Zulassen|
    300|Gateway zulassen|10.0.255.224/27|Alle|Custom(Any/Any)|Zulassen|
    310|Self zulassen|10.0.255.192/27|Alle|Custom(Any/Any)|Zulassen|
    320|Vnet-verweigern|Alle|Alle|Custom(Any/Any)|Zulassen|

    AD DS verwendet Ports 53, 89, 135, 389 und 464 eingehende Replikation und Authentifizierungsdatenverkehr annehmen. In dieser Tabelle ist der lokale Domänencontroller in der Adresse Leerzeichen 192.168.0.0/24 (Ihren Space Adresse kann variieren: Geben Sie diese Informationen als Parameter auf die Vorlagen bereitgestellt werden, indem Sie die Lösung.

    Die NSG definiert auch die folgenden Regeln ausgehende Sicherheit die Synchronisierung und Autorisierung Datenverkehr wieder zu dem lokalen Netzwerk übertragen zu aktivieren:

    Priorität|Namen|Datenquelle|Ziel|Dienst|Aktion|
    --------|----|------|-----------|-------|------|
    100|Out-port53|Alle|192.168.0.0/24|Custom(Any/53)|Zulassen|
    110|Out-port88|Alle|192.168.0.0/24|Custom(Any/88)|Zulassen|
    120|Out-port135|Alle|192.168.0.0/24|Custom(Any/135)|Zulassen|
    130|Out-port389|Alle|192.168.0.0/24|Custom(Any/389)|Zulassen|
    140|Out-Port 445|Alle|192.168.0.0/24|Custom(Any/445)|Zulassen|
    150|Out-port464|Alle|192.168.0.0/24|Custom(Any/464)|Zulassen|
    160|Out-Rpc-dynamische|Alle|192.168.0.0/24|Custom(Any/49152-65535)|Zulassen|

Das Skript mit der Lösung bereitgestellte auch die folgenden Aufgaben ausführen:

- Fügt die *Basename*-ad1-virtueller Computer und *Basename*-ad2-virtueller Computer-Servern als Domänencontroller, um die Domäne. Sie können diese Server in der Verwaltungskonsole *Active Directory-Benutzer und Computer* in der lokalen Domänencontroller anzeigen:

![[1]][1]

- Es wird ein neues Subnetz (10.0.0.0/16) für eine AD-Website mit dem Namen der Domäne Azure-VNet-Ad-Standort erstellt. Diese Website enthält die *Basename*-ad1-virtueller Computer und *Basename*-ad2-Servern virtueller Computer. 

- Es fügt zwischen Standorten Transport IP-Einstellungen, die das Intervall zwischen dem lokalen Standort und die Domänencontroller in der Cloud zu konfigurieren. Sie können die Einstellungen für das Subnetz, Websites und Transportregel-Einstellungen in der Verwaltungskonsole *Active Directory-Websites und -Servern* in der lokalen Domänencontroller angezeigt werden:

![[2]][2]

## <a name="deployment"></a>Bereitstellung

Die Lösung für die Stichprobe weist die folgenden Prerequsites:

- Verbinden mit dem Gateway Azure VPN, Sie Ihre Domäne lokalen bereits konfiguriert haben, und dass Sie DNS konfiguriert und Routing und Remote Access Services zur Unterstützung eines VPN installiert haben.


- Sie haben die neueste Version der CLI Azure installiert. [Befolgen Sie diese Anweisungen hierzu][cli-install].

- Wenn Sie die Windows-Lösung bereitstellen, müssen Sie ein Tool für eine Bash Shell, beispielsweise [GitHub Desktop]installieren[github-desktop].

>[AZURE.NOTE] Wenn Sie keinen Zugriff zu einer vorhandenen lokalen Domäne haben, können Sie eine testumgebung, die mithilfe der [onpremdeploy.sh] erstellen[ onpremdeploy] bash Skript. Dieses Skript erstellt ein Netzwerk und virtueller Computer in der Cloud, die simuliert eine sehr einfache lokalen eingerichtet wurde. Bearbeiten Sie dieses Skript, bevor er ausgeführt wird, und legen Sie die folgenden Variablen am Anfang der Datei definiert:
>
> - **BASE_NAME**. Ein benutzerdefiniertes Präfix für die Ressourcengruppe und virtueller Computer erstellt haben, indem Sie das Skript. Dieses Element darf **nicht mehr als 5 Zeichen** andernfalls sein, die das Skript versucht, einen virtuellen Computer mit einem ungültigen Namen generieren und nicht bestanden.
> 
> - **ABONNEMENTS**. Ihre Azure-Abonnement-ID an. Die Ressourcengruppe wird in diesem Abonnement erstellt.
> 
> - **Speicherort**. Die Azure Position in dem Sie die Ressourcengruppe aus, wie z. B. *Eastus* oder *Westus*erstellen.
> 
> - **ADMIN_USER_NAME**. Der Name für das Administratorkonto auf dem virtuellen Computer verwendet werden soll.
> 
> - **ADMIN_PASSWORD**. Das Kennwort für das Administratorkonto.

Führen Sie die folgenden Schritte aus, um die Lösung für die Stichprobe zu erstellen:

1. Herunterladen und bearbeiten Sie die [azuredeploy.sh] [ azuredeploy] Skripts, und legen Sie die folgenden Parameter am Anfang der Datei:

    - **BASE_NAME**. Ein benutzerdefiniertes Präfix für die Ressourcengruppen und virtuellen Computern erstellt werden, indem Sie das Skript. Wie zuvor, sollte dieses Element **nicht mehr als 5 Zeichen**enthalten.

    - **ABONNEMENTS**. Ihre Azure-Abonnement-ID an.

    - **CPU-Typ**. Das Betriebssystem (*Windows* oder *Linux*) für das Web und Business Data Access Ebene virtuellen Computern verwendet werden soll. Beachten Sie, dass alle AD-Servern das Skript erstellte Windows Server 2012, unabhängig von dieser Einstellung ausführen.

    - **Domänenname**. Der Name der lokalen Domäne. Beachten Sie, dass dies bei Verwendung die Umgebung erstellt werden, indem Sie das Skript onpremdeploy.sh *contoso.com*werden muss.

    - **Speicherort**. Die Azure Speicherort für die Ressourcengruppen erstellen.

    - **ADMIN_USER_NAME**. Der Name für den Administratorkonten in den verschiedenen virtuellen Computern verwendet werden soll.

    - **ADMIN_PASSWORD**. Das Kennwort für das Administratorkonto.

    - **ON_PREMISES_PUBLIC_IP**. Die öffentliche IP-Adresse des lokalen Computers VPN.

    - **ON_PREMISES_ADDRESS_SPACE**. Der interne Adresse Speicherplatz des lokalen Netzwerks. Wenn Sie die Umgebung erstellt werden, indem Sie das Skript onpremdeploy.sh verwenden, muss dies 192.168.0.0/16 sein.

    - **VPN_IPSEC_SHARED_KEY**. Der freigegebene IPSec Schlüssel zum Herstellen der Verbindung VPN zwischen dem lokalen Netzwerk und Azure VPN-Gateway verwendet.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. Die IP-Adresse des lokalen DNS-Servers. Wenn Sie die Umgebung erstellt werden, indem Sie das Skript onpremdeploy.sh verwenden, muss dies 192.168.0.4 sein.

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Das Adresspräfix der im lokalen Subnetz. Wenn Sie die Umgebung erstellt werden, indem Sie das Skript onpremdeploy.sh verwenden, muss dies 192.168.0.0/24 sein.

    >[AZURE.NOTE] UM Ressourcen und Zeit zu sparen, wird das Skript nicht die Geschäfts- oder Access Ebenen erstellt. Wenn Sie diese Elemente benötigen, können Sie im folgenden Abschnitt in das Skript azuredeploy.sh kommentieren:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Öffnen einer Bash Shell-Aufforderung, und wechseln Sie zu dem Ordner mit dem Skript azuredeploy.sh.

3. Melden Sie sich bei Ihrem Azure-Konto an. Geben Sie in der Bash Shell den folgenden Befehl aus:

    ```
    azure login
    ```

    Folgen Sie den Anweisungen zum Azure herstellen.

4. Führen Sie den Befehl `./azuredeploy.sh`, und warten Sie, während das Skript Infrastruktur des erstellt.

5. Verwenden Sie bei der Aufforderung, *Stellen Sie sicher, dass die VNet erstellt wurde*Azure-Portal um zu überprüfen, dass eine Ressourcengruppe mit dem Namen *Basename*- Ntwk-Rg erstellt wurde und sie Elemente ähnlich wie in der folgenden Abbildung enthält ein:

    ![[3]][3]

    >[AZURE.NOTE] In den Beispielen, die angezeigt wird wurde *Basename* *Cloud* festgelegt, wenn das Skript ausgeführt wurde.

    Klicken Sie auf die *Basename*- Vnet VNet, klicken Sie auf *Subnets*, und stellen Sie sicher, dass die nachfolgend aufgeführten Subnetze erstellt wurden:

    ![[4]][4]

6. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während der Webebene und Lastenausgleich erstellt werden.

7. Verwenden Sie bei der Aufforderung, *Stellen Sie sicher, dass die Web-Ebene ordnungsgemäß erstellt wurde*Azure-Portal zu prüfen, ob eine Ressourcengruppe mit der Bezeichnung *Basename*Web-Ebene-Rg erstellt wurde, und sie ähnlich wie die nachfolgend aufgeführten Elemente enthält:

    ![[5]][5]

8. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während die NVAs erstellt werden.

9. Verwenden Sie bei der Aufforderung, *Stellen Sie sicher, dass die NVA ordnungsgemäß erstellt wurde*Azure-Portal zu um überprüfen, ob eine Ressourcengruppe mit der Bezeichnung *Basename*- Management-Rg mit dem folgenden Inhalt erstellt wurde:

    ![[6]][6]

10. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während die Jumpbox erstellt wird.

11. Verwenden Sie bei der Aufforderung, *Stellen Sie sicher, dass die Jumpbox ordnungsgemäß erstellt wurde*Azure-Portal zu um überprüfen, ob die folgenden Elemente der *Basename*- Management-Rg Ressourcengruppe hinzugefügt wurden:

    - Eine Sammlung Verfügbarkeit aufgerufen *Basename*- JB LKW-als.

    - Ein virtueller Computer mit dem Namen *Basename*- JB LKW-virtuellen Computer aus.

    - Netzwerk-Schnittstellen aufgerufen *Basename*- JB LKW-NIC.

12. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während die Azure VPN-Gateway und die Verbindung erstellt werden. Beachten Sie, dass dieses Schritts bis zu 30 Minuten in Anspruch nehmen kann.

13. Verwenden Sie bei der Aufforderung, *Stellen Sie sicher, dass das Option VPN Gateway ordnungsgemäß erstellt wurde*Azure-Portal zu um überprüfen, ob die folgenden Elemente der *Basename*- Ntwk-Rg Ressourcengruppe hinzugefügt wurden:

    - Ein Gateway lokales Netzwerk als auf lokale Lgw bezeichnet.
    
    - Ein Gateway virtuelles Netzwerk als *Basename*- Vpngw bezeichnet.

    - Eine Gateway-Verbindung mit dem Namen *Basename*- Vnet-Vpnconn aus. Beachten Sie, dass der Status dieser Verbindung möglicherweise *nicht verbunden* , wenn das lokale Ende der Verbindung noch nicht konfiguriert haben. Sie können dies später beheben.

14. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während die virtuellen Computern und anderen Ressourcen für die DMZ erstellt werden.

15. Verwenden Sie bei der Aufforderung, *Stellen Sie sicher, dass die DMZ ordnungsgemäß erstellt wurde*Azure-Portal zu um überprüfen, ob eine Ressourcengruppe mit der Bezeichnung *Basename*-dmz-Rg mit dem folgenden Inhalt erstellt wurde:

    ![[7]][7]

16. Drücken Sie bei der entsprechenden Aufforderung im Bash Shell-Fenster eine Taste aus. Die folgenden Anweisungen sollten angezeigt werden:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Melden Sie sich mit Ihrem lokalen Computer, der mit dem Azure Gateway verbindet, und konfigurieren Sie die Verbindung entsprechend. Fügen Sie statischer leitet am lokalen Gateway-Gerät, mit der Requestsfor Bereich 10.0.0.0/16 über das Gateway zu den VNet weitergeleitet hinzu. Die Schritte zur Ausführung Dies variiert nach, wie Sie eine Verbindung herstellen. Finden Sie unter [Implementieren eines Hybrid Netzwerkarchitektur mit Azure und lokalen VPN] [ implementing-a-hybrid-network-architecture-with-vpn] Weitere Informationen.

    Beachten Sie, dass Sie die öffentliche IP-Adresse des Gateways Azure VPN mithilfe des Azure-Portals *Basename*- Vpngw Gateways in *Basename*- Ntwk-Rg Ressourcengruppe untersuchen finden können:

    ![[8]][8]

    Sie können festlegen, ob die Verbindung ordnungsgemäß eingerichtet wurde, indem Sie den Status der Verbindung *Basename*- Vnet-Vpnconn. Sie sollten auf *verbunden*festgelegt werden.

    Um die Verbindung zu testen, öffnen Sie eine Remotedesktop-Verbindung die Jumpbox (10.0.0.254) von einem Computer in Ihrem lokalen Netzwerk befindet.

17. Drücken Sie bei der entsprechenden Aufforderung im Bash Shell-Fenster eine Taste aus. Auf der nächsten auffordern Sie, *Drücken Sie eine Taste, um die Einstellung VNet für die VNet verweisen auf lokale DNS-Einträge aktualisieren*, drücken Sie die Taste, und warten Sie, während die DNS-Einstellungen für die VNet auf den Wert aktualisiert werden, die Sie als **ON_PREMISES_DNS_SERVER_ADDRESS** im azuredeploy.sh Skript angegeben.

18. Verwenden Sie dazu aufgefordert werden, *Stellen Sie sicher, dass die DNS-Server-Einstellung auf die VNet aktualisiert wurde*, Azure-Portal zum Überprüfen der *DNS-Server* -Einstellung für die *Basename*- Vnet VNet in der Gruppe *Basename*- Ntwk-Rg Ressource ein. Es sollte auf *DNS benutzerdefiniert*festgelegt werden, und der *primären DNS-Server* sollte die Adresse Ihres lokalen DNS-Servers sein:

    ![[9]][9]

19. *Taste drücken, um das Erstellen der Ressourcengruppe für die AD-Server* dazu aufgefordert werden im Bash Shell-Fenster drücken Sie die Taste und warten Sie, während die Ressourcengruppe für den AD-Servern in der Cloud gedrückt erstellt wird.

20. Bei der Aufforderung *Taste drücken, um den virtuellen Computern für die AD-Server erstellen* in der Bash Shell-Fenster Drücken einer Taste und warten, bis der virtuelle Computer erstellt und der Ressourcengruppe hinzugefügt werden.

21. Wenn die *Taste drücken, um den virtuellen Computern der lokalen Domäne teilnehmen* angezeigt wird, wechseln Sie zu der Azure-Portal, und stellen Sie sicher, dass eine Gruppe namens *Basename*- Dns-Rg erstellt wurde und zwei virtuellen Computern enthalten (*Basename*-ad1-virtueller Computer und *Basename*-ad2-virtueller Computer):

    ![[10]][10]

22. In der Gruppe *Basename*- Ntwk-Rg Ressource überprüfen, dass eine NSG erstellt wurde aufgerufen *Basename*-Ad-Nsg. Überprüfen Sie die eingehende und ausgehende Sicherheitsregeln für diese NSG aus. Sie sollten die aufgeführten Datentypen sind in den Tabellen in der [Lösungskomponenten] übereinstimmen[ solution-components] Abschnitt.

23. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während die virtuellen Computern der lokalen Domäne hinzugefügt werden.

24. Bei der *Wechseln Sie zu der lokalen AD-Server, um sicherzustellen, dass der Computer mit den Domänen hinzugefügt wurden* auffordern, Verbinden mit dem lokalen Computer, und verwenden Sie die Konsole *Active Directory-Benutzer und Computer* zu um überprüfen, ob die Domäne, die beide virtuellen Computern hinzugefügt wurden:

    ![[11]][11]

25. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während die Replikation Active Directory-Standort in der Domäne erstellt wird.

26. Bei der *Wechseln Sie zu der lokalen AD-Server, um sicherzustellen, dass die Website Replikation erstellt wurde* dazu aufgefordert werden, verwenden Sie die *Active Directory-Websites und-Diensten* Konsole zu um überprüfen, ob eine Replikation-Website mit dem Namen *Azure-Vnet-Ad-Standort* erfolgreich, zusammen mit einem IP-Transport zwischen Standorten Link *AzureToOnpremLink*und einem Subnetz, die die VNet verweist bezeichnet erstellt wurde:

    ![[12]][12]

27. Drücken Sie die Taste dazu aufgefordert werden im Bash Shell-Fenster und warten Sie, während das Skript Directory Services und DNS auf jeder der AD virtueller Computer installiert.

28. Wenn Sie aufgefordert *Melden Sie sich bitte jedes Azure AD-Servers stellen Sie sicher, dass das Verzeichnis Services erfolgreich konfiguriert wurde* , öffnen eine Remotedesktop-Verbindung aus einem lokalen Computer auf die Jumpbox (*Basename*- JB LKW-virtueller Computer), und dann eine andere Remotedesktop-Verbindung aus der Jumpbox auf dem ersten AD-Server (*Basename*-ad1-virtuellen Computer). Melden Sie sich mit der **Domänenname**, **ADMIN_USER_NAME**und **ADMIN_PASSWORD** , die Sie in das Skript azuredeploy.sh angegeben haben. Mit dem Server-Manager, stellen Sie sicher, dass die Rollen AD DS und DNS beide hinzugefügt wurden. Wiederholen Sie diesen Vorgang für den zweiten AD-Server (*Basename*-ad2-virtuellen Computer).

29. Drücken Sie bei der entsprechenden Aufforderung im Bash Shell-Fenster eine Taste aus. Wenn Sie *eine beliebige Taste zum Festlegen der Einstellungen Azure VNet DNS-Einträge auf DNS in Azure verweisen drücken* aufgefordert, drücken Sie die Taste und zulassen Sie das Skript zum Aktualisieren der DNS-Einstellungen für die VNet.

30. Wenn die Aufforderung *Stellen Sie sicher, dass die Einstellung wurde VNet DNS Bezug der Azure-virtuellen Computer DNS aktualisiert Servern* angezeigt wird, mit der Azure Portals Überprüfen der *DNS-Server* -Einstellung für die *Basename*- Vnet VNet in der Gruppe *Basename*- Ntwk-Rg Ressource. Der primären und sekundären DNS-Server sollte jetzt die zwei AD-virtuellen Computern verweisen:

    ![[13]][13]

31. Starten Sie jede der AD-virtuellen Computern an, bevor Sie Sie fortfahren. Dieser Schritt ist erforderlich, um sicherzustellen, dass sie jeweils die korrekten DNS-Einstellungen aus Azure abholen. Warten Sie, bis beide virtuellen Computern, bevor Sie fortfahren ausgeführt werden.

32. Drücken Sie bei der entsprechenden Aufforderung im Bash Shell-Fenster eine Taste aus. Nächste dazu aufgefordert werden, *Drücken Sie alle, um das Gateway mit dem Gateway Subnetz UDR anwenden (es möglicherweise entfernt wurden)*, drücken Sie die Taste und zulassen Sie das Skript zum Aktualisieren des Gateways UDR.

33. Stellen Sie sicher, dass das Skript erfolgreich abgeschlossen wurde.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, die bewährte Methoden zum [Erstellen einer ADDS Ressourcengesamtstruktur] [ adds-resource-forest] in Azure.

- Erfahren Sie, die bewährte Methoden zum [Erstellen einer ADFS-Infrastruktur] [ adfs] in Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Secure Hybrid Netzwerkarchitektur mit Active Directory"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Die Active Directory-Benutzer und Computer-Konsole die zwei Azure-virtuellen Computern, als Server auflisten"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Die Active Directory-Websites und-Diensten Console mit der Replikation Einstellungen für die Website in der cloud"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "Der Inhalt der Basename-Ntwk-Rg Ressourcengruppe"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Die Subnetze in die Basename-Vnet VNet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Die Elemente in der Webebene"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "Die NVAs in Basename-Management-Rg Ressourcengruppe"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Die Ressourcen in der Gruppe Basename-dmz-Rg Ressource"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Suchen die öffentliche IP-Adresse des Gateways Azure VPN"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "Die DNS-Server-Einstellungen für die * Basename *-Vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "Der * Basename *-Dns-Rg Ressourcengruppe mit dem AD-Server virtuellen Computern"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Die Active Directory-Benutzer und Computer-Konsole den AD-Server virtuellen Computern als Mitglieder der Domäne auflisten"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Die Active Directory-Websites und-Diensten Console mit der Replikation-Website für den Azure AD-Servern"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "Die DNS-Server-Einstellungen für die Basename-Vnet VNet verweisen auf den AD-Servern in der cloud"