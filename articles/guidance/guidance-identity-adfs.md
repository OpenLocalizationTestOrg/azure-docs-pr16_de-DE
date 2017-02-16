<properties
   pageTitle="Implementierung von ADFS in Azure | Microsoft Azure"
   description="Wie Sie eine sichere Hybrid Netzwerkarchitektur mit Active Directory Federation Service Autorisierung in Azure implementieren."
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
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Implementierung von Active Directory Federation Services (ADFS) in Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel werden die optimale Methoden zum Implementieren eines sicheren Hybrid Netzwerk, Ihrem lokalen Netzwerk in Azure erweitert, und [Active Directory Federation Services (ADFS)] verwendet[ active-directory-federation-services] partnerverbundkontakte Authentifizierung und Autorisierung für Komponenten, die in der Cloud ausgeführt ausführen. Diese Architektur erweitert die Struktur von [Active Directory erweitern, um Azure]beschriebenen[implementing-active-directory].

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Bezug Architektur verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

ADFS lokal ausgeführt werden kann, aber die in einem Hybriden Szenario, in dem Applications in Azure ansässig sind, es effizienter, diese Funktionalität in der Cloud zu implementieren. Typische Nutzung Fällen für diese Architektur umfassen:

- Hybrid Applications, wo Auslastung ausführen teilweise lokalen, und teilweise in Azure.

- Lösungen, die partnerverbundkontakte Autorisierung Webanwendungen zum Partnerorganisationen verfügbar machen nutzen.

- Systeme, die von außerhalb der Organisation Firewalls ausgeführt Webbrowsern unterstützen.

- Systeme, die Benutzern der Zugriff auf Webanwendungen durch Herstellen einer Verbindung von autorisierten externen Geräten wie Remotecomputern, Notizbücher und andere mobile Geräte zu ermöglichen. 

Weitere Informationen zur Funktionsweise von ADFS, finden Sie unter [Active Directory Federation Services Overview][active-directory-federation-services-overview]. Darüber hinaus im Artikel [ADFS-Bereitstellung in Azure] [ adfs-intro] enthält eine ausführliche schrittweise Einführung in ADFS in Azure implementieren.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur (*Klicken Sie zum Vergrößern auf*). Weitere Informationen zu einem beliebigen Element nicht im Zusammenhang mit ADFS, finden Sie unter [Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure][implementing-a-secure-hybrid-network-architecture], [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Internetzugang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], und [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Active Directory-Identitäten in Azure][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] Diese Abbildung zeigt die folgenden Fällen verwenden:
>
>- Code der Anwendung, die innerhalb der Organisation eines greift auf einer Webanwendung innerhalb Ihrer Azure VNet gehostet.
>
>- Einen externen, registrierte Benutzer (mit Anmeldeinformationen, die innerhalb der ADDS gespeichert) den Zugriff auf eine Web-Anwendung, die in Ihrem VNet Azure gehostet.
>
>- Ein Benutzer, der Herstellen einer Verbindung mit Ihrem VNet mithilfe einer autorisierten Geräte und Ausführen einer Webanwendung, die in Ihrem VNet Azure gehostet wird.
>
>Darüber hinaus diese Architektur Schwerpunkt auf passiven Verbund, wo die Föderation Server, wie und wann entscheiden einen Benutzer authentifiziert. Der Benutzer soll anmelden Informationen bereitstellen, wenn eine Anwendung gestartet wird. Dies ist das Verfahren, die am häufigsten verwendeten Webbrowser und umfasst ein Protokoll, das im Browser zu einer Website umgeleitet, wo der Benutzer ihre Anmeldeinformationen erhalten. ADFS unterstützt auch die aktiven Verbund, bei dem eine Anwendung übernimmt Zuständigkeit für die Angabe Anmeldeinformationen ohne weitere Interaktion des Benutzers, aber dieser Fall liegt außerhalb des Gültigkeitsbereichs dieser Architektur.

- **ADDIERT Subnetz.** Die ADDS-Server sind in einem eigenen Subnetz enthalten. NSG Regeln zum Schutz der Server addiert Hilfe und eine Firewall gegen den Datenverkehr von unerwarteten Quellen bereitstellen.

- **Fügt Server hinzu.** Hierbei handelt es sich um Domänencontroller als virtuellen Computern in der Cloud ausgeführt. Diese Server können Authentifizierung lokale Identitäten innerhalb der Domäne bereitstellen.

- **ADFS Subnetz.** Die ADFS-Server können eigenen Subnetz, mit NSG Regeln fungiert als Firewall befinden.

- **ADFS-Server.** Die ADFS-Server bieten partnerverbundkontakte Autorisierung und Authentifizierung. In dieser Architektur führen sie die folgenden Schritte aus:

    - Sie können, die von einem Partner Föderation Server im Auftrag eines Partnerbenutzers Ansprüche enthält Sicherheitstokens erhalten. ADFS können stellen Sie sicher, dass diese Token gültig sind, bevor Sie übergeben Claims to Web-Anwendung, die in Azure ausgeführt. Die corporate Webanwendung (in Azure) können diese Ansprüche um Anfragen zu autorisieren. In diesem Szenario die corporate Webanwendung ist die *Party verlassen*, und es ist, dass die Zuständigkeit des Partners Föderation Servers ausgeben Ansprüche, dass, die von der Webanwendung corporate verstanden werden. Die Partner Föderation Servern werden als *Kontopartner* bezeichnet, da diese zugriffsanforderungen für authentifizierte Konten in der Partnerorganisation übermitteln. Die ADFS-Server werden als *Ressourcenpartner* bezeichnet, da sie Zugriff auf Ressourcen (in diesem Fall Webanwendungen) bieten.

    - Sie können sich authentifizieren (über addiert und den [Active Directory-Gerät Registrierungsdienst][ADDRS]) und autorisieren eingehende Anfragen aus externen Benutzern, die ausgeführt werden, einen Webbrowser oder Gerät, die Zugriff auf Ihre corporate Webanwendungen benötigt. 

    Als Farm, über ein Azure Lastenausgleich zugegriffen werden die ADFS-Server konfiguriert. Diese Struktur hilft zur Verbesserung der Verfügbarkeit und Skalierung. Beachten Sie, dass die ADFS-Server nicht direkt mit dem Internet offen gelegt, lieber gesamten Datenverkehr im Internet über ADFS-Web-Anwendung Proxy-Servern und einer DMZ gefiltert wird.

- **ADFS Proxy Subnetz.** ADFS-Proxy-Servern können in einem eigenen Subnetz, mit NSG Regeln Schutz enthalten sein. Die Server in diesem Subnetz werden über eine Reihe von virtuellen Netzwerkgeräte mit dem Internet verfügbar gemacht, die eine Firewall zwischen Ihrem Azure virtuelle Netzwerk und im Internet bereitstellen.

- **ADFS-Web-Anwendung (WAP) Proxyserver.** Diese Computer dienen als ADFS-Server für die eingehenden Anfragen von Organisationen und externen Geräte. WAP-Servern dienen als Filter, die ADFS-Server von direkten Zugriff vom öffentlichen Internet Schutz. Wie bei der ADFS-Server Bereitstellen von der WAP Servern in einer Farm mit Lastenausgleich ermöglicht höhere Verfügbarkeit und Skalierbarkeit als eine Sammlung von eigenständigen Servern bereitzustellen.

    >[AZURE.NOTE] Ausführliche Informationen zum Installieren von WAP-Servern finden Sie unter [Installieren und Konfigurieren des Proxyservers für Web-Anwendung][install_and_configure_the_web_application_proxy_server]

- **Die Organisation.** Dies ist ein Beispiel Partnerorganisation, der eine Webanwendung ausgeführt wird, die zugriffsanforderungen mit der Webanwendung, die in Azure ausgeführt. Der Föderation Server in der Partnerorganisation authentifiziert lokal Anfragen und Sicherheitstokens mit Ansprüche in ADFS in Azure ausgeführt übermittelt. ADFS in Azure überprüft die Sicherheitstoken und sie gültig sind können sie mit der Webanwendung in Azure ausgeführt, um sie zu autorisieren Claims geleitet.

    >[AZURE.NOTE] Sie können auch einen VPN-Tunnel mit Azure Gateway direkten Zugriff auf ADFS für vertrauenswürdige Partner bereit konfigurieren. Besprechungsanfragen, die von diesen Partnern empfangen übergeben nicht über die WAP-Server.

## <a name="recommendations"></a>Empfehlungen

In diesem Abschnitt werden die Vorgehensweisen für die Implementierung von ADFS in Azure zusammengefasst darüber liegendem:

- Virtueller Computer Empfehlungen.

- Empfehlungen für Netzwerke.

- Verfügbarkeit Empfehlungen.

- Empfehlungen im Zusammenhang mit Sicherheit.

- ADFS mit der Installation beginnen.

- Empfehlungen ADFS vertrauen.

### <a name="vm-recommendations"></a>Virtueller Computer Empfehlungen

Erstellen Sie virtuellen Computern mit ausreichenden Ressourcen, um der erwarteten Umfang des Datenverkehrs zu behandeln. Verwenden Sie die Größe der vorhandenen Computer Hostinganbieter ADFS lokal als Ausgangspunkt. Die Nutzung von Ressourcen zu überwachen. Sie können Ändern der Größe der virtuellen Computern und verkleinern, wenn sie zu groß sind.

Befolgen Sie die Ratschläge aufgeführt, die bei der [Ausführung eines Windows virtuellen Computers auf Azure][vm-recommendations].

### <a name="networking-recommendations"></a>Netzwerke Empfehlungen

Konfigurieren der Schnittstelle für jede der virtuellen Computern Hostinganbieter ADFS und WAP-Servern statische private IP-Adressen.

Geben Sie dies nicht die ADFS-virtuellen Computern öffentlichen IP-Adressen ein. Weitere Informationen finden Sie unter [Sicherheitsaspekte][security-considerations].

Legen Sie die IP-Adresse die bevorzugte und des sekundären DNS-Server für die Netzwerkschnittstellen für jede ADFS und WAP VM die virtuellen Computern addiert Bezug (dem DNS ausgeführt werden soll). Dieser Schritt ist erforderlich, aktivieren jeden virtuellen Computer, der Domäne beizutreten.

### <a name="availability-recommendations"></a>Verfügbarkeit Empfehlungen

Erstellen einer ADFS-Farm mit mindestens zwei Servern zum Erhöhen der Verfügbarkeit des ADFS-Diensts.

Verwenden Sie verschiedene Speicher-Konten für jede ADFS VM in der Farm ein. Dieser Ansatz wird sichergestellt, dass ein Fehler in einem einzigen Speicher-Konto nicht die gesamte Farm nicht zur Verfügung.

Erstellen Sie separate Azure Verfügbarkeit Datensätze für ADFS und WAP virtuellen Computern an. Stellen Sie sicher, dass mindestens zwei virtuellen Computern in den einzelnen Sätzen vorhanden sind. Jeden Satz Verfügbarkeit müssen mindestens zwei aktualisieren und zwei Fehlerstrukturanalyse-Domänen.

Konfigurieren Sie den Lastenausgleich für die ADFS-virtuellen Computern und WAP virtuellen Computern wie folgt ein:

- Verwenden einer Azure Lastenausgleich den externen Zugriff auf die WAP virtuellen Computern und eine interne Lastenausgleich Verteilung die Last über die ADFS-Server in der Farm ADFS.

- Nur den Datenverkehr auf Port 443 (HTTPS) auf den Servern ADFS/WAP angezeigte zu übergeben.

- Geben Sie dem Lastenausgleich eine statische IP-Adresse.

- Erstellen Sie einen Systemzustand Prüfpunkt mithilfe des TCP-Protokoll verwenden, anstatt die HTTPS. Sie können anpingen Port 443 zu überprüfen, ob ein ADFS-Server funktioniert.

    >[AZURE.NOTE] ADFS-Server verwenden Sie das Server Namen Angabe (SNI)-Protokoll, versuchen, also Prüfpunkt mithilfe eines HTTPS-Endpunkts aus der laden Lastenausgleich fehlschlägt.

- Hinzufügen eines DNS- *A* -Eintrags zu der Domäne für den Lastenausgleich ADFS. 

    Geben Sie die IP-Adresse des Lastenausgleich, und geben sie einen Namen in der Domäne (wie etwa adfs.contoso.com). Dies ist der Name, über den Clients und den WAP-Servern die ADFS-Server-Farm zugreifen.

### <a name="security-recommendations"></a>Sicherheit Empfehlungen

Verhindern, dass direkte Anzeigen der ADFS-Server mit dem Internet. ADFS-Server sind Domänenverbund auf Computern, die vollständige Autorisierung Sicherheitstokens gewähren haben. Wenn ein ADFS-Server gefährdet ist, kann ein bösartiger Benutzer Vollzugriff Token an alle Webanwendungen und Föderation Server, die von ADFS geschützt sind ausführen. Wenn das System Anfragen aus externen Benutzern, die nicht unbedingt Verbinden von vertrauenswürdigen Partnerwebsites verarbeitet werden muss, verwenden Sie WAP-Servern, um diese Anfragen zu behandeln. Weitere Informationen finden Sie unter [Proxy Server Föderation angelegt][where-to-place-an-fs-proxy].

Ort-ADFS-Server und WAP-Servern in getrennten Subnetzen mit eigene Firewalls. NSG Regeln können Sie die Firewall-Regeln definieren. Wenn Sie umfassenderen Schutz anfordern können Sie eine zusätzliche Sicherheit Umfang Servern mithilfe von ein Paar von Adresssubnetzen und NVAs, implementieren, durch das Dokument [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Internetzugang in Azure]beschriebenen[implementing-a-secure-hybrid-network-architecture-with-internet-access]. Beachten Sie, dass alle Firewalls Datenverkehr auf Port 443 (HTTPS) zulassen soll.

Login direkten Zugriff auf den Servern ADFS und WAP einschränken. Nur DevOps Personal sollte Verbindung hergestellt werden.

Fügen Sie der WAP-Servers nicht zur Domäne.

### <a name="adfs-installation-recommendations"></a>ADFS mit der Installation beginnen

[Bereitstellen einer Serverfarm Föderation] Artikel[ Deploying_a_federation_server_farm] bietet detaillierte Anweisungen zum Installieren und Konfigurieren von ADFS. Führen Sie die folgenden Aufgaben vor den ersten ADFS-Server in der Farm konfigurieren:

1. Rufen Sie ein öffentlich vertrauenswürdiges Zertifikat zur Durchführung Serverauthentifizierung ein. Den *Betreff Name* muss den Namen enthalten, über den Clients den Dienst Föderation zugreifen. Dies kann der DNS-Name für den Lastenausgleich, z. B. *adfs.contoso.com* registriert sein (verwenden Sie keine Platzhalterzeichen Namen wie **. "contoso.com"*, aus Sicherheitsgründen). Verwenden Sie das gleiche Zertifikat auf alle ADFS-Server virtuellen Computern an. Sie können ein Zertifikat von einer vertrauenswürdigen Zertifizierungsstelle erwerben, aber wenn Ihre Organisation Active Directory Certificate Services verwendet können Sie erstellen Ihrer eigenen. 

    Der *Betreff alternativen Namen* wird von der DRS zum Zugriff von externen Geräte zu ermöglichen. Dies sollte das Formular *enterpriseregistration.contoso.com*aufweisen.

    Weitere Informationen finden Sie unter [erwerben und konfigurieren ein SSL-Zertifikat für ADFS][adfs_certificates].

2. Auf dem Domänencontroller müssen generieren Sie einen neuen Stammschlüssel für die Verteilung Schlüsselverwaltungsdienst. Legen Sie die effektive Zeit für die aktuelle Uhrzeit abzüglich 10 Stunden werden (diese Konfiguration reduziert die Verzögerung, die in verteilen und Synchronisieren von Tasten über die Domäne auftreten können). Dieser Schritt ist erforderlich, um Unterstützung für das Erstellen der Gruppe Dienstkontos, die zum Ausführen des ADFS-Diensts verwendet werden. Der folgende Powershell-Befehl zeigt ein Beispiel für die Vorgehensweise:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Jede ADFS-Server virtueller Computer der Domäne hinzufügen.

>[AZURE.NOTE] Um ADFS installieren zu können, muss der Domänencontroller ausführen den Betriebsmaster FSMO-Funktion für die Domäne ausgeführt wird und aus dem ADFS-virtuellen Computern verfügbar.

### <a name="adfs-trust-recommendations"></a>Empfehlungen im Zusammenhang mit ADFS vertrauen

Richten Sie Föderation Vertrauensstellung zwischen der ADFS-Installation, und die Föderation Server von einem beliebigen Organisationen. Konfigurieren Sie alle Ansprüche filtern und Zuordnung erforderlich. Dieses Verfahren erfordert:

- DevOps Mitarbeitern **bei jeder Partnerorganisation** zum Hinzufügen einer sich verlassen Partei Vertrauensstellung für die Webanwendungen über Ihre ADFS-Server zugegriffen werden kann.

- DevOps Mitarbeitern **in Ihrer Organisation** zum Konfigurieren der Trust Ansprüche-Anbieter, um ADFS-Server ein, die Ansprüche zu vertrauen, die Partnerorganisationen bereitstellen.

- DevOps Mitarbeitern **in Ihrer Organisation** zum Konfigurieren von ADFS um Ansprüche bei Webanwendungen Ihrer Organisation zu übergeben.

    Weitere Informationen finden Sie unter [Einrichten von Föderation vertrauen][establishing-federation-trust].

Veröffentlichen Sie Ihrer Organisation Webanwendungen und machen Sie verfügbar, für externe Partner mithilfe von Vorauthentifizierung über die WAP-Server. Weitere Informationen finden Sie unter [Veröffentlichen von Applications using ADFS Präauthentifizierung][publish_applications_using_AD_FS_preauthentication]

Beachten Sie, dass ADFS token Transformation und Ergänzung unterstützt. Azure-Active Directory bietet dieses Feature nicht. Mit ADFS Wenn Sie die Beziehungen Trust einrichten können Sie:

- Konfigurieren Sie für die Autorisierungsregeln erstellt. Beispielsweise können Sie Gruppe Sicherheit aus einer Darstellung von einer Partnerorganisation ein Element, das in Ihrer Organisation die ADDS autorisieren kann nicht-Microsoft-verwendeten zuordnen.

- Ansprüche von einem Format in ein anderes zu transformieren. Beispielsweise können Sie von SAML 2.0 SAML 1.1 zuordnen, wenn eine Anwendung nur SAML 1.1 Ansprüche unterstützt. 

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

SQL Server oder den internen Windows-Datenbank (AID) können Sie Informationen zur Konfiguration ADFS halten. WID bietet grundlegende Redundanz. Änderungen werden direkt an nur eine der Datenbanken ADFS im ADFS-Cluster geschrieben, während die anderen Server Abruf Replikation verwenden, um ihrer Datenbanken auf dem neuesten Stand zu halten. Verwenden von SQL Server kann vollständige Datenbank Redundanz und hohe Verfügbarkeit mit Failover Cluster oder Spiegelung bereitstellen.

## <a name="security-considerations"></a>Zur Sicherheit

ADFS verwendet das HTTPS-Protokoll, daher sollten Sie sicherstellen, dass die NSG Regeln für das Subnetz mit dem Internet virtuellen Computern zulassen HTTPS-Anfragen zu stufen. Diese Anfragen können die Subnetze mit der Webebene, Business Ebene, Datenebene, private DMZ, öffentliche DMZ und dem Subnetz, enthält die ADFS-Server aus dem lokalen Netzwerk stammen.

Erwägen Sie eine Reihe von virtuellen Netzwerkgeräte, die ausführliche Informationen zu durchlaufen den Rand des virtuellen Netzwerks für die Überwachung Zwecke Datenverkehr protokolliert.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Die folgenden Punkte, die aus dem Dokument [Planen der Bereitstellung von ADFS]zusammengefasst[plan-your-adfs-deployment], geben Sie diese als Ausgangsbasis für ADFS Farmen Ziehpunkt:

- Wenn Sie weniger als 1000 Benutzer haben, gehen Sie wie folgt dedizierte ADFS-Server erstellt, sondern stattdessen ADFS installieren, klicken Sie auf den Servern addiert, in der Cloud (Stellen Sie sicher, dass Sie mindestens zwei ADDS Server zum Verwalten der Verfügbarkeit haben). Erstellen eines einzelnen WAP-Servers.

- Wenn Sie zwischen 1000 und 15000 Benutzer verfügen, erstellen Sie zwei dedizierte ADFS-Server und zwei dedizierte WAP-Server.

- Wenn Sie zwischen 15000 und 60000 Benutzer verfügen, erstellen Sie zwischen drei und fünf dedizierte ADFS-Server und mindestens zwei dedizierte WAP-Server.

Diese Zahlen wird davon ausgegangen, dass Sie zwei Quadcore-virtuellen Computern (Standard D4_v2 oder besser) auf die Servern in Azure gehostet verwenden.

Beachten Sie, wenn Sie die Windows-interne Datenbank zum Speichern von ADFS Konfigurationsdaten verwenden, um acht ADFS-Server in der Farm sind beschränkt. Wenn Sie mehr benötigen erwarten, verwenden Sie SQL Server. Weitere Informationen finden Sie unter [Der Rolle der Datenbank Konfiguration ADFS][adfs-configuration-database].

## <a name="management-considerations"></a>Gesichtspunkte

DevOps Mitarbeiter soll den anderen Teilnehmern die folgenden Aufgaben ausführen:

- Verwalten von den Servern Föderation (Verwalten der ADFS-Farm, Richtlinien auf den Servern Föderation Vertrauenswürdigkeit, und die Zertifikate von Federation Services verwendeten) aus.

- Verwalten von den WAP-Servern (Verwalten der WAP-Farm, die Zertifikate WAP verwalten) aus.

- Verwalten von Webanwendungen (Identitätsempfänger, Authentifizierungsmethoden und Ansprüche Zuordnungen konfigurieren) aus.

- Sichern ADFS-Komponenten.

## <a name="monitoring-considerations"></a>Überlegungen für die Überwachung

Das [Microsoft System Center Management Pack für Active Directory Federation Services 2012 R2] [ oms-adfs-pack] bietet proaktive und reaktivieren Überwachung der Bereitstellung ADFS für den Partnerverbund-Server. Dieses Management Pack überwacht:

- Ereignisse, dass die ADFS-Einträge in den Ereignisprotokollen ADFS-Dienst.

- Die Leistungsdaten, die die ADFS Leistungsindikatoren sammeln. 

- Den allgemeinen Zustand des ADFS-System und Web Applications (Identitätsempfänger), und stellt von Benachrichtigungen für kritische Probleme und Warnungen.

## <a name="solution-components"></a>Komponenten einer Lösung

Ein Beispiel Lösung Skript [Bereitstellen-ReferenceArchitecture.ps1][solution-script], steht zur Verfügung, die Sie verwenden können, die Architektur implementiert wird, die die in diesem Artikel beschriebenen Empfehlungen folgt. Dieses Skript nutzt Ressourcenmanager Azure-Vorlagen. Die Vorlagen stehen als eine Reihe von grundlegender Bausteine, von die jede eine bestimmte Aktion wie eine VNet erstellen oder Konfigurieren einer NSG ausführt. Der Zweck des Skripts ist Vorlage Bereitstellung koordinieren.

Die Vorlagen werden mit den Parametern, die als separate JSON-Dateien parametrisierte. Sie können die Parameter in diesen Dateien so konfigurieren Sie die Bereitstellung Ihrer eigenen Bedürfnisse zugeschnitten ändern. Sie müssen nicht die Vorlagen selbst zu ändern. Beachten Sie, dass Sie die Schemas der Objekte in der Parameterdateien nicht ändern müssen.

Beim Bearbeiten von Vorlagen erstellen Objekte, die die Benennungskonventionen [Benennungskonventionen für Ressourcen Azure empfohlen]beschrieben folgen[naming-conventions].

Die Lösung für die Stichprobe erstellt und konfiguriert die Umgebung in der Cloud aus der ADDS Subnetz und -Servern, die Subnetz ADFS-Server, ADFS Proxy Subnetz und -Servern, DMZ, Webebene, Business-Ebene und-Komponenten der Datenebene Access, VPN-Gateway und Management Ebene. Die Lösung Beispiel enthält darüber hinaus eine optionale Konfiguration zum Erstellen einer simulierten lokalen Umgebung.

In den folgenden Abschnitten werden die Elemente, die lokal beschrieben und Konfigurationen cloud.

### <a name="on-premises-components"></a>Lokale Komponenten

>[AZURE.NOTE] Diese Komponenten sind nicht im Mittelpunkt dieses in diesem Dokument beschriebenen Architektur und einfach um Sie Gelegenheit Cloud-Umgebung testen sicher, anstatt mit einer realen Herstellung Umgebung bereitgestellt werden. Aus diesem Grund werden in diesem Abschnitt nur die wichtigsten Parameterdateien zusammengefasst. Sie können die Einstellungen für die IP-Adressen oder die Größe der virtueller Computer ändern, aber es ist ratsam, die viele der anderen Parameter unverändert lassen.

Diese Umgebung umfasst eine Active Directory-Struktur für eine Domäne mit dem Namen "contoso.com". Die Domäne enthält zwei ADDS-Servern mit IP-Adressen 192.168.0.4 und 192.168.0.5. Diese zwei Servern wird außerdem den DNS-Dienst ausgeführt. Das lokale Administratorkonto auf beide virtuellen Computern heißt `testuser` mit Kennwort `AweS0me@PW`. Darüber hinaus richtet die Konfiguration ein VPN-Gateway für das Herstellen einer Verbindung mit der VNet in der Cloud. Ändern Sie die Konfiguration, indem Sie die folgenden JSON-Dateien in den [**Parameter/lokale Edition**] [ on-premises-folder] Ordner:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Diese Datei definiert den Netzwerk Adresse Speicherplatz für den lokalen Umgebung.

- ** [VirtualMachines-adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Diese Datei enthält die Konfiguration für die lokale virtuellen Computern Hostingdienste addiert. Standardmäßig werden zwei *Standard-DS3-Version 2* -virtuellen Computern erstellt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** und ** [connection.parameters.json][on-premises-connection-parameters]**. Diese Dateien enthalten die Einstellungen für die VPN-Verbindung mit dem Gateway Azure VPN in der Cloud, einschließlich der freigegebenen-Taste, um zum Schutz des Datenverkehrs Durchlaufen des Gateways verwendet werden.

Die verbleibenden Dateien im Ordner enthalten die Konfigurationsinformationen zum Erstellen der lokalen Domäne verwenden diese Infrastruktur verwendet. Sie verwenden können zum Installieren addiert, DNS-Einträge für die Einrichtung, Gesamtstruktur erstellen und Konfigurieren der Replikation Websites für die Gesamtstruktur.

### <a name="cloud-components"></a>Cloud-Komponenten

Diese Komponenten bilden das Herzstück dieser Architektur. Der [**Parameter/Azure**] [ azure-folder] Ordner enthält die folgenden Parameterdateien für diese Komponenten konfigurieren:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Diese Datei definiert die Struktur der VNet für den virtuellen Computern und andere unsichere Komponenten in der Cloud. Er enthält Einstellungen wie der Name, Adresse Leerzeichen, Subnetze und die Adressen aller DNS-Server erforderlich. Beachten Sie, dass die DNS-Adressen in diesem Beispiel werden die IP-Adressen der DNS-Server lokal und auch dem standardmäßigen Azure DNS-Server verweisen. Ändern Sie diese Adressen Ihrer eigenen DNS-Konfiguration verweisen, wenn Sie nicht die Stichprobe lokalen Umgebung verwenden:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [VirtualMachines-adds.parameters.json ] [ virtualmachines-adds-parameters] **. Diese Datei konfiguriert die virtuellen Computern addiert in der Cloud ausgeführt wird. Die Konfiguration besteht aus zwei virtuellen Computern. Ändern Sie den Administrator-Benutzernamen und das Kennwort in das `virtualMachineSettings` Abschnitt, und Sie können optional die virtueller Speicher entsprechend die Anforderungen der Domäne ändern:

    Weitere Informationen finden Sie unter [Active Directory erweitern, um Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [hinzufügen-Fügt-Domäne-controller.parameters.json][add-adds-domain-controller-parameters]**. Diese Datei enthält die Einstellungen für das Erstellen der CONTOSO-Domäne, die die Servern ADDS dauern. Es wird verwendet, benutzerdefinierte Erweiterungen, die die Domäne eingerichtet und fügen Sie die ADDS-Server hinzu. Es sei denn, Sie zusätzliche ADDS Server erstellen (in diesem Fall Sie diese hinzufügen sollte der `vms` Array), deren Namen von Standard ändern oder eine Domäne mit einem anderen Namen zu erstellen, Sie nicht zum Ändern dieser Datei müssen, möchten.

- ** [Systems zum Lastenausgleich-adfs.parameters.json] [loadbalancer-adfs-parameters] ** Die Datei enthält zwei Sätze von Konfigurationsparametern. Die `virtualMachineSettings` Abschnitt definiert die virtuellen Computern, die den ADFS-Dienst in der Cloud zu hosten. Standardmäßig erstellt zwei dieser das Skript virtuellen Computern in der gleichen Verfügbarkeit festlegen:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    Die `loadBalancerSettings` Abschnitt enthält die Beschreibung des Lastenausgleich für diese virtuelle Computer. Lastenausgleich übergibt eine oder eine andere von der virtuellen Maschinen Datenverkehr, der auf Port 443 (HTTPS) angezeigt wird:

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [Adfs-Farm-Domäne-join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Diese Datei enthält die Einstellungen, die ADFS-Server mit der CONTOSO-Domäne hinzugefügt. Sie müssen nur diese Datei zu ändern, wenn Sie zusätzliche ADFS-Server erstellt haben (Aktualisieren der `vms` in diesem Fall array), oder Sie haben den Domänennamen geändert.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [Adfs-Farm-first.parameters.json][adfs-farm-first-parameters]**, und ** [Adfs-Farm-rest.parameters.json][adfs-farm-rest-parameters]**. Das Skript verwendet die Einstellungen in diesen Dateien zum Erstellen der ADFS-Server-Farm ein. 

    Die Datei *gmsa.parameters.json* enthält die Einstellungen für die Gruppe verwaltete Dienstkonto vom ADFS-Dienst verwendet. Sie können diese Datei ändern, wenn Sie den Namen der Firma oder die Domäne ändern möchten.

    Die Datei *Adfs-Farm-first.parameters.json* enthält Informationen zum Erstellen der ADFS-Server-Farm und Hinzufügen des ersten Servers erforderlich sind. Wenn Sie die Domäne oder den Namen des Dienstkontos verwaltete Gruppe geändert haben, sollten Sie diese Datei aktualisieren.

    Die Datei *Adfs-Farm-rest.parameters.json* wird verwendet, um die verbleibenden ADFS-Server in der Farm hinzufügen. Wenn Sie die Domäne oder den Namen des Dienstkontos verwaltete Gruppe geändert haben, sollten Sie diese Datei erneut aktualisieren. Aktualisieren der `vms` array fest, wenn Sie zusätzliche ADFS-Server erstellt haben.

- ** [Systems zum Lastenausgleich-adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Diese Datei ähnelt Struktur und Inhalt der Datei *Systems zum Lastenausgleich-adfs.parameters.json* . Sie enthält die Daten zur Erstellung der ADFS Proxy-Servern und Lastenausgleich.

- ** [Adfsproxy-Farm-first.parameters.json][adfsproxy-farm-first-parameters]**, und ** [Adfsproxy-Farm-rest.parameters.json][adfsproxy-farm-rest-parameters]**. Das Skript verwendet die Einstellungen in diesen Dateien die ADFS Proxy-Serverfarm zu erstellen. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Diese Datei enthält die Einstellungen zum Erstellen des Gateways Azure VPN in der Cloud verwendet, um eine Verbindung mit dem lokalen Netzwerk herzustellen. Sie sollten ändern die `sharedKey` Wert in der `connectionsSettings` Abschnitt mit dem lokalen VPN-Gerät übereinstimmt. Weitere Informationen finden Sie unter [Implementieren eines Hybrid Netzwerkarchitektur mit Azure und lokalen VPN][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** und ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Diese Dateien konfigurieren die eingehende (öffentlich) und ausgehenden (privaten) Rückseite der virtuellen Computern, die die DMZ, Schützen von den Servern in der Cloud umfassen. Weitere Informationen zu diesen Elementen und deren Konfiguration, finden Sie unter [Implementierung einer zwischen Azure und im Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [Systems zum Lastenausgleich-web.parameters-Json][loadBalancer-web-parameters]**, ** [Json-Systems zum Lastenausgleich-biz.parameters-][loadBalancer-biz-parameters]**, und ** [Json-Systems zum Lastenausgleich-data.parameters-][loadBalancer-data-parameters]**. Diese Dateien Parameter enthält die virtuellen Computer Spezifikationen für die Ebenen der Access Web, Business und Daten und Lastenausgleich für jede Ebene konfigurieren. Hierbei handelt es sich um den virtuellen Computern, die der Web apps und Datenbanken hosten, und führen Sie die Business Auslastung für die Organisation. Sie können die Größe und Anzahl von virtuellen Computern in jede Ebene gemäß Ihren Anforderungen ändern.

- ** [VirtualMachines-mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Diese Datei enthält die Konfiguration für die Sprung Feld/Verwaltung virtueller Computer. Es ist jedoch nur möglich, Anmelde- und Administratorzugriff auf den virtuellen Computern in der Web, Geschäfts- und Datenebenen aus dem Feld Sprung zu erhalten. Standardmäßig das Skript erstellt eine einzelne *Standard_DS1_v2* virtueller Computer, aber Sie können diese Datei zum Vergrößern oder zusätzliche virtuellen Computern erstellen, ist die Arbeitsbelastung Management wahrscheinlich wesentlichen ändern.

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Die Lösung setzt die folgenden Komponenten:

- Sie haben ein vorhandenes Azure Abonnement, in dem Sie eine Ressourcengruppen erstellen können.

- Sie haben die heruntergeladen und installiert die neueste Erstellen von Azure Powershell. Finden Sie [hier] [ azure-powershell-download] Anweisungen.

So führen Sie das Skript aus, das die Lösung bereitgestellt wird.

1. Wechseln zu einem geeigneten Ordner auf Ihrem lokalen Computer, und erstellen Sie die folgenden Unterordner:

    - Skripts

    - Skripts/Parameter

    - Lokale Skripts/Parameter/Edition

    - Skripts/Parameter/Azure

2. Herunterladen der [Bereitstellen-ReferenceArchitecture.ps1] [ solution-script] Datei zu dem Ordner Skripts

3. Herunterladen des Inhalts der [Parameter/lokale Edition] [ on-premises-folder] Ordner in den Ordner lokale Skripts/Parameter/Edition:

4. Herunterladen des Inhalts der [Parameter/Azure] [ azure-folder] Ordner in den Ordner Skripts/Parameter/Azure.

5. Bearbeiten Sie die Datei bereitstellen-ReferenceArchitecture.ps1 in den Ordner Skripts, und ändern Sie die folgenden Linien, um die Ressourcengruppen angeben, die verwendet, um die Ressourcen erstellt, das Skript aufzunehmen oder erstellt werden:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Bearbeiten Sie die Parameterdateien in den Skripts/Parameter/Azure-Ordnern und Skripts/Parameter/lokale Edition. Aktualisieren Sie die Ressource Gruppe Bezüge in diese Dateien auf den Namen der Ressourcengruppen zugewiesen an die Variablen in der Datei bereitstellen-ReferenceArchitecture.ps1 übereinstimmen. Die folgende Tabelle zeigt, welche Parameterdateien der Ressourcengruppe verwiesen werden. Beachten Sie, dass die *RAS-Adfs-Arbeitsbelastung-Rg*, *RAS-Adfs-Sicherheit-Rg*, *RAS Adfs addiert Rg*, *RAS-Adfs-Adfs-Rg*und *RAS-Adfs-Proxy-Rg* Gruppen sind nur in der PowerShell-Skript verwendet, und nicht in den Parameterdateien treten.

  	|Ressourcengruppe|Parameter-Dateien|
  	|--------------|--------------|
  	|RAS-Adfs-lokale Edition-rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|RAS-Adfs-Netzwerk-rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-private.Parameters.JSON<br />parameters\azure\dmz-public.Parameters.JSON<br />parameters\azure\loadBalancer-ADFS.Parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.Parameters.JSON<br />parameters\azure\loadBalancer-biz.Parameters.JSON<br />parameters\azure\loadBalancer-Data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-with-OnPremise-and-Azure-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*zwei Vorkommen*)

    Darüber hinaus Festlegen der Konfiguration für die lokale und cloud-Komponenten, wie im Abschnitt [Lösung Components] [Komponenten der Lösung] beschrieben.

7. Öffnen Sie ein Azure PowerShell-Fenster zu, wechseln Sie zu dem Ordner Skripts und führen Sie den folgenden Befehl aus:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Ersetzen Sie `<subscription id>` mit Ihrer Azure-Abonnement-ID an.

    Für `<location>`, geben Sie eine Azure Region, wie `eastus` oder `westus`.

    Die `<mode>` Parameter kann einen der folgenden Werte aufweisen:

    - `Onpremise`, um die simulierten lokalen Umgebung zu erstellen.

    - `Infrastructure`, erstellen die VNet Infrastruktur und Feld in der Cloud springen.

    - `CreateVpn`, Azure-virtuellen Netzwerk-Gateway erstellen und verbinden Sie es mit dem lokalen Netzwerk.

    - `AzureADDS`, um den virtuellen Computern, die als ADDS Server erstellen, Bereitstellen von Active Directory für diesen virtuellen Computern und die Domäne in der Cloud zu erstellen.

    - `AdfsVm`die ADFS virtuelle Computer erstellen, und fügen sie der Domäne in der Cloud.

    - `ProxyVm`Erstellen von virtuellen Computern der ADFS-Proxy, und fügen sie der Domäne in der Cloud.

    - `Prepare`, das alle oben genannten Aufgaben ausführt. **Dies ist die empfohlene Option, wenn Sie eine vollständig neue Bereitstellung erstellen, und Sie nicht über eine vorhandene lokale Infrastruktur verfügen.**

    >[AZURE.NOTE] Sie können auch führen Sie das Skript mit einer `<mode>` Parameter der `Workload` im Web, Business, und Datenebene virtuellen Computern und Netzwerk zu erstellen. Dieses Setup ist nicht Bestandteil der `Prepare` Modus.

    Wenn Sie verwenden die `Prepare` Option das Skript dauert mehrere Stunden und abgeschlossen ist, mit der Meldung *Vorbereitung abgeschlossen ist. Bitte installieren Sie ein Zertifikat für alle ADFS und Proxy-virtuellen Computern.*

8.  Starten Sie im Feld Gehe zu (*RAS-Adfs-Management-vm1* in der Gruppe *RAS-Adfs-Sicherheit-Rg* ), um seine DNS-Einstellungen wirksam zulassen.

9.  [Abrufen eines SSL-Zertifikats für ADFS] [ adfs_certificates] und dieses Zertifikat auf dem ADFS-virtuellen Computern installieren. Beachten Sie, dass Sie eine Verbindung mit den ADFS-virtuellen Computern, bis das Feld Sprung herstellen können. Die IP-Adressen sind *10.0.5.4* und *10.0.5.5*. Der Standard-Benutzername ist *Contoso\testuser* mit Kennwort *AweSome@PW*.

    >[AZURE.NOTE] Die Kommentare im Bereitstellen-ReferenceArchitecture.ps1 Skript zu diesem Zeitpunkt erhalten Sie detaillierte Informationen zum Erstellen einer selbst signiertes Testzertifikat und Zertifizierungsstelle mithilfe der `makecert` Befehl. Verwenden Sie die Zertifikate von Makecert in einer Umgebung für die Herstellung generiert jedoch nicht.

10. Führen Sie den folgenden Powershell-Befehl zum Konfigurieren der ADFS-Server-Farm ein:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. Wechseln Sie im Feld Sprung auf *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* zum Testen der ADFS-Installation (Sie erhalten möglicherweise eine Warnung Zertifikat, das Sie für diesen Test ignorieren können). Stellen Sie sicher, dass die Unternehmen Contoso-Anmeldeseite angezeigt wird. Melden Sie sich als *Contoso\testuser* mit Kennwort *AweS0me@PW*.

12. Installieren eines Zertifikats der Zertifizierungsstelle auf dem ADFS-Proxy-virtuellen Computern an. Die IP-Adressen sind *10.0.6.4* und *10.0.6.5*.

13. Führen Sie den folgenden Powershell-Befehl auf den ersten ADFS Proxyserver konfigurieren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Folgen Sie den Anweisungen, die angezeigt werden, indem Sie das Skript zum Testen der Installation des ersten ADFS Proxyservers.

15. Führen Sie den folgenden Powershell-Befehl so konfigurieren Sie den zweiten ADFS Proxy Ausgangsserver:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Anweisungen Sie angezeigt, indem Sie das Skript die gesamte ADFS Proxy-Konfiguration zu testen.

## <a name="next-steps"></a>Nächste Schritte

- [Azure-Active Directory][aad].

- [Azure-Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Secure Hybrid Netzwerkarchitektur mit Active Directory"
