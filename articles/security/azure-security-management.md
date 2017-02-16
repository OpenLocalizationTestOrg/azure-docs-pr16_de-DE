<properties
   pageTitle="Verwaltung von Sicherheit in Azure | Microsoft Azure"
   description=" Dieser Artikel beschreibt die Schritte zur Verbesserung der remote-Verwaltung Sicherheit während der Verwaltung von Microsoft Azure-Umgebungen, einschließlich Cloud Services, virtuellen Computern und benutzerdefinierte Applications."
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="terrylan"/>

# <a name="security-management-in-azure"></a>Verwaltung von Sicherheit in Azure

Azure Abonnenten können ihre Cloud-Umgebungen verwalten, aus mehreren Geräten, einschließlich Management Arbeitsstationen, Entwicklertools PCs und sogar berechtigten Endbenutzergeräten, die Aufgabe-spezifischen Berechtigungen verfügen. In einigen Fällen sind Verwaltungsfunktionen über webbasierte Konsolen wie der [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)ausgeführt. In anderen Fällen möglicherweise direkte Verbindungen zu Azure aus lokalen Systemen über virtuelle Private Netzwerke (VPN), Terminal Services, Client-Anwendungsprotokolle oder (programmgesteuert) Azure Service Management-API (SMAPI). Darüber hinaus können Clientendpunkte entweder Domäne beigetreten oder isoliert und nicht verwaltete, wie etwa Tablets oder Smartphones sein.

Zwar mehrere Access und Verwaltungsfunktionen eine Rich-Reihe von Optionen bereit, kann diese Streuung erheblichen Risiko einer Cloudbereitstellung hinzufügen. Es kann schwierig sein, verwalten, verfolgen und Überwachen von administrativen Aktionen. Dieser wechselnden möglicherweise auch Sicherheitsrisiko für Ihr über verfügt Access Clientendpunkte vorstellen, die für die Verwaltung von Cloud-Diensten verwendet werden. Verwenden allgemeine oder persönlichen Arbeitsstationen für die Entwicklung und Verwaltung von Infrastruktur wird nicht vorhersehbar Bedrohungsüberträger wie web Durchsuchen (z. B. getränkt Loch Angriffen) oder die e-Mail-(z. B. Social Engineering und Phishing).

![][1]

Mögliche Angriffen in dieser Art von Umgebung erhöht, da es, zum Erstellen von Sicherheitsrichtlinien und Verfahren zum Verwalten des Zugriffs auf Azure ordnungsgemäß schwierig ist Schnittstellen (z. B. SMAPI) von stark verfügt über Endpunkte.

### <a name="remote-management-threats"></a>Remote-Verwaltung von Risiken

Angreifer versuchen häufig Berechtigungen durch die Anmeldeinformationen für das Konto (z. B. über Kennwort erzwingen, Phishing und Anmeldeinformationen sammeln) beeinträchtigen oder durch Benutzer in das Ausführen von gefährlichen Codes (beispielsweise von gefährlichen Websites mit Laufwerk durch Downloads oder gefährlichen e-Mail-Anlagen) dazu zugreifen. In einer Umgebung Remote verwalteten Cloud können Konto dienen zu eine höhere Risiko aufgrund von überall und jederzeit Zugriff führen.

Trotz enger Steuerelemente für primäre Administratorkonten können Low-Level-Benutzerkonten in der eine Strategie für die Schwächen ausnutzen verwendet werden. Fehlende entsprechende-Schulung kann auch zu Lücken durch unbeabsichtigte Veröffentlichung oder Anzeigen der Kontoinformationen führen.

Wenn ein Benutzer Arbeitsstationen auch für Verwaltungsaufgaben verwendet wird, kann es an vielen verschiedenen Punkten beeinträchtigt werden. Enthält, ob ein Benutzer ist im Web durchsuchen 3rd Drittanbietern und Open-Source-Tools verwenden oder das Öffnen eines Dokuments gefährlichen Datei, die eine Trojanische.

Im Allgemeinen können nahezu zielgerichtete Angriffen, die Daten Verletzung führen zum Browser Angriffen (z. B. Flash, PDF-Datei, Java)-Plug-ins und Spear Phishing (e-Mail) auf Desktopcomputern verfolgt werden. Diese Computer möglicherweise Administratorrechten oder Servicelevel Berechtigungen zum Zugriff auf live-Server oder Netzwerkgeräte für Vorgänge, wenn für die Entwicklung oder Verwaltung von anderen Geräten verwendet.

### <a name="operational-security-fundamentals"></a>Grundlagen der Sicherheit

Sie können für sicherer Verwaltung und Betrieb eines Kunden Angriffsfläche durch Verringern der Anzahl von Punkten möglicher Eintrag minimieren. Dies kann über die Sicherheit Grundsätze: "Trennung von Aufgaben" und "Trennung der Umgebungen".

Isolieren Sie sensible Funktionen voneinander, um die Wahrscheinlichkeit zu verringern, die ein Fehler auf eine Ebene zu einer Verletzung in einer anderen führt. Beispiele für:

- Verwaltungsaufgaben sollten nicht mit Aktivitäten, die (z. B. Malware eines Administrators e-Mail, die dann auf einen Infrastrukturserver infiziert) gefährdet möglicherweise, kombiniert werden.
- Eine Arbeitsstationen für höchst-Vertraulichkeit Operationen verwendet sollten nicht dasselbe System, z. B. Durchsuchen des Internets mit hohem Risiko Zwecken.

Verringern des Systems Angriffsfläche durch Entfernen unnötigen Software. Beispiel:

- Administrative Standard, Support oder Entwicklung Arbeitsstationen sollte Installation von einem-e-Mail-Client oder einer anderen Anwendung Produktivität ist Hauptfenster Zweck des Geräts zum Verwalten der Cloud Services erfordert.

Client-Systeme, die Administratorzugriff auf Infrastrukturkomponenten aufweisen sollte die geringste mögliche Richtlinie Reduzieren von Risiken ausgesetzt sein. Beispiele für:

- Sicherheitsrichtlinien können Gruppenrichtlinien einbeziehen, die den Zugriff auf das Internet öffnen aus dem Gerät und die Verwendung der eine eingeschränkte Firewall-Konfiguration verweigern.
- Verwenden Sie Internet Protocol Security (IPsec) VPN, wenn Direktzugriff erforderlich ist.
- Konfigurieren Sie separate Verwaltung und Entwicklung Active Directory-Domänen.
- Isolieren und Management Arbeitsstationen Netzwerkverkehr filtern.
- Verwenden Sie Antischadsoftware.
- Implementieren mehrstufige Authentifizierung, das Risiko von Anmeldeinformationen gestohlen zu verringern.

Konsolidierung Zugriff auf Ressourcen und Ausblenden nicht verwalteten Endpunkte vereinfacht auch Verwaltungsaufgaben aus.


### <a name="providing-security-for-azure-remote-management"></a>Bereitstellen von Sicherheit für Azure remote-Verwaltung

Azure bietet Sicherheitsmechanismen Administratoren zur Makrofunktion die Azure-Cloud-Diensten und virtuellen Computern verwalten. Diese Verfahren umfassen:

- Authentifizierung und [rollenbasierte Access-Steuerelement](../active-directory/role-based-access-control-configure.md).
- Überwachung, Protokollierung und Überwachung.
- Zertifikate und verschlüsselte Kommunikation.
- Ein Web-Verwaltungsportal.
- Netzwerk zu Paketfiltern.

In Kombination mit clientseitig Sicherheits- und Datacenter Bereitstellung eines Gateways Management ist es möglich, einschränken und Administratorzugriff Cloud Applikationen und Daten zu überwachen.

> [AZURE.NOTE] Bestimmte Empfehlungen in diesem Artikel können höhere Daten, Netzwerk- oder berechnen Ressource: Einsatz hinzu, und erhöhen Sie die Lizenz oder das Abonnement Kosten.

## <a name="hardened-workstation-for-management"></a>Gesicherte Arbeitsstationen für die Verwaltung

Sichern von einer Arbeitsstationen Ziel ist, außer den wichtigsten Funktionen dafür erforderlich ausgeführt werden, die Angriffsfläche kleinstmöglichen ausführenden alle zu beseitigen. System zum Sichern des enthält Minimieren der Anzahl der installierten Dienste und Anwendungen, beschränken die Ausführung der Anwendung, einschränken Netzwerkzugriff nur erforderlich sind, und aktualisieren das System immer auf dem neuesten Stand. Darüber hinaus getrennt werden mithilfe eines abgesicherten Arbeitsstationen für die Verwaltung Verwaltung und Aktivitäten, die von anderen Vorgängen Endbenutzer.

In einer lokalen Umgebung Enterprise können Sie einschränken die Angriffsoberfläche für Ihre physische Infrastruktur dedizierten Management Netzwerke, Serverräume, die Karte zugreifen und Arbeitsstationen, die auf geschützte Bereiche des Netzwerks ausgeführt werden. In einer Cloud oder Hybrid IT-Modell können über sicheres Management Services sorgfältig komplexere aufgrund der fehlenden physischen Zugriff auf IT-Ressourcen sein. Implementierung Schutz Lösung erfordert sorgfältige Softwarekonfiguration, Sicherheit konzentrieren Prozesse und umfassende Richtlinien.

Verwenden einer Präsenz minimierten Software mit den niedrigsten Berechtigungen in einem gesperrten Arbeitsstationen für die Verwaltung der Cloud – sowie für die Entwicklung – das Risiko handelt verringern können, indem Sie die remote Verwaltung und Entwicklung Umgebungen Standardisierung. Eine gesicherte Arbeitsstationskonfiguration kann verhindern, dass der Verlust von Konten, mit denen kritische Cloudressourcen zu verwalten, indem Sie viele allgemeine Wege untersuchten Schadsoftware und Angriffen zu schließen. Sie können insbesondere [Windows AppLocker](http://technet.microsoft.com/library/dd759117.aspx) und Hyper-V-Technologie zum Steuern und isolieren Client Systemverhalten verwenden und minimieren Risiken, einschließlich e-Mail oder Surfen im Internet.

Klicken Sie auf eine gesicherte Arbeitsstationen der Administrator führt ein standard Benutzerkonto (das Administratorrechten Ausführung blockiert) und die zugehörigen Anwendung mit einer Zulassungsliste gesteuert. Die grundlegenden Elemente für eine gesicherte Arbeitsstationen werden wie folgt aus:

- Aktive Scannen und Patch. Bereitstellen Sie Antischadsoftware, führen Sie reguläre Sicherheitsrisiko scannt aus, und aktualisieren Sie alle Arbeitsstationen mithilfe des neuesten Sicherheitsupdates zeitgerecht.
- Eingeschränkte Funktionalität. Deinstallieren Sie alle Programme, die nicht benötigt werden, und deaktivieren Sie unnötige (Start) Dienste.
- Zum Sichern des Netzwerks. Verwenden von Windows-Firewall Regeln dürfen nur gültige IP-Adressen, Ports und URLs im Zusammenhang mit Azure Management. Stellen Sie sicher, dass eingehende remote-Verbindungen mit der Arbeitsstationen auch blockiert werden.
- Einschränkung der Ausführung. Lassen Sie nur eine Reihe von vordefinierten ausführbare Dateien, die für die Verwaltung (verwiesen wird als "Standard-verweigern") ausführen erforderlich sind. Standardmäßig sollte Benutzern verweigert Berechtigung zum ein beliebiges Programm ausführen, es sei denn, sie in der Zulassungsliste explizit definiert ist.
- Minimalen Rechte. Um diesen Benutzern Management sollte auf dem lokalen Computer selbst keine über Administratorrechte verfügen. Auf diese Weise können sie die Systemkonfiguration oder die Systemdateien absichtlich oder unabsichtlich nicht ändern.

Sie können diesen erzwingen, durch [Group Policy Objects](https://www.microsoft.com/download/details.aspx?id=2612) (GPOs) in Active Directory-Domänendiensten (AD DS) verwenden, und sie über Ihre Domäne (lokal) Management auf alle Management-Konten angewendet werden.

### <a name="managing-services-applications-and-data"></a>Verwalten von Dienstleistungen,-Anwendungen und Daten

Azure Cloud Services-Konfiguration erfolgt über die Azure-Portal oder SMAPI, über die Windows PowerShell Command-Benutzeroberfläche oder eine benutzerdefinierte Anwendung, die diese Rest Schnittstellen nutzt. Mithilfe dieser Dienste gehören Azure Active Directory (Azure AD), Azure-Speicher, Azure-Websites und Azure-virtuellen Netzwerk und andere.

Virtuellen Computern – bereitgestellt Applikationen bieten eigene Client Tools und Schnittstellen nach Bedarf, wie etwa Microsoft Management Console (MMC), einer Enterprise-Verwaltungskonsole (wie etwa Microsoft System Center oder Windows Intune) oder einer anderen Anwendung von Management – Microsoft SQL Server Management Studio, beispielsweise. Diese Tools befinden sich in der Regel in einem Unternehmensnetzwerk Umgebung oder Client. Sie können auf bestimmte Netzwerk-Protokolle, wie z. B. (Remotedesktopprotokoll), abhängen, die direkte, dynamische Verbindungen erfordern. Einige eventuell webfähigen Schnittstellen, die nicht offen veröffentlichten oder über das Internet zugänglich sein sollen.

Sie können den Zugriff Infrastruktur und Plattform Services erfahrener in Azure mithilfe von [mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md), [x. 509-Verwaltungszertifikate](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/)und Firewall-Regeln einschränken. Die Azure-Portal und SMAPI erfordern Sicherheit TLS (Transport Layer). Jedoch erforderlich, Diensten und Anwendungen, die Sie in Azure bereitstellen Schutz Maßnahmen, die auf Grundlage Ihrer Anwendung geeignet sind. Diese Verfahren können häufig einfacher über eine standardisierte gesicherte Arbeitsstationskonfiguration aktiviert sein.

### <a name="management-gateway"></a>Management gateway

Um alle Administratorzugriff zentrales und für die Überwachung und Protokollierung zu vereinfachen, können Sie einen dedizierten [Remote Desktop-Gateway](https://technet.microsoft.com/library/dd560672) (RD Gateway) Server in Ihrem lokalen Netzwerk, bei einer Verbindung zu Ihrer Umgebung Azure bereitstellen.

Ein Remote Desktop-Gateway ist eine Policy-basierte RDP Proxy-Dienst, der Sicherheit Anforderungen erzwingt. Implementieren der RD-Gateway zusammen mit Windows Server Network Access Protection (NAP) verhindert, dass nur Clients, die Active Directory-Domänendiensten (AD DS) Gruppenrichtlinien Gruppenrichtlinienobjekte eingesetzten von bestimmten Sicherheit Gesundheit Kriterien erfüllen, eine Verbindung herstellen können. Außerdem:

- Bereitstellung von ein [Zertifikat Azure Management](http://msdn.microsoft.com/library/azure/gg551722.aspx) auf dem Gateway RD, damit es der einzige Host den Zugriff auf das Portal Azure Management zulässig ist.
- Teilnehmen an der RD-Gateway zu derselben [Management Domäne](http://technet.microsoft.com/library/bb727085.aspx) wie der Administratorarbeitsstationen. Dies ist erforderlich, wenn Sie eine Website-zu-Standort IPsec VPN oder ExpressRoute innerhalb einer Domäne verwenden, die eine unidirektionale Vertrauensstellung zu Azure AD enthält, oder wenn Sie eine Föderation Anmeldeinformationen zwischen der lokalen AD DS-Instanz und Azure AD-.
- Konfigurieren einer [Verbindung die Autorisierung Clientrichtlinie](http://technet.microsoft.com/library/cc753324.aspx) lassen Sie das RD Gateway überprüfen, dass der Name des Computers gültig (Domäne verbunden) ist und den Zugriff auf das Portal Azure Management zulässig.
- Verwenden Sie IPsec für [Azure VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) abhörsicher sind und von token Diebstahl weiteren Management Datenverkehr gewarnt oder einen isolierten Internetlink per [Azure ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/), sollten Sie.
- Aktivieren Sie kombinierte Authentifizierung (über [Azure kombinierte Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md)) oder Smartcard-Authentifizierung für Administratoren, die über RD-Gateway anmelden.
- Konfigurieren von Quell- [IP-Adresse Einschränkungen](http://azure.microsoft.com/blog/2013/08/27/confirming-dynamic-ip-address-restrictions-in-windows-azure-web-sites/) oder [Netzwerk Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) in Azure aus, um die Anzahl der zulässigen Management Endpunkte zu minimieren.

## <a name="security-guidelines"></a>Sicherheitsrichtlinien

Im Allgemeinen unterstützen der Sicherheit von Arbeitsstationen der Administratoren für mit der Cloud Die Methoden für alle Arbeitsstationen lokal verwendet sehr ähnlich ist – beispielsweise minimiert erstellen und eingeschränkten Berechtigungen. Einige eindeutige Aspekte des Cloud-Verwaltung sind Sie eher remote oder Out-of-Band-Enterprise-Management. Hierzu gehören die Verwendung und Überwachung von Anmeldeinformationen, RAS mit erhöhter Sicherheit und Erkennung und Antwort ein.

### <a name="authentication"></a>Authentifizierung

Mit können Azure Anmeldung Einschränkungen zum Einschränken der Quelle IP-Adressen für den Zugriff auf die Verwaltung und Überwachungsrichtlinien zugriffsanforderungen. Damit werden Azure Management-Clients (Arbeitsstationen und/oder Applications) zu identifizieren, können Sie sowohl SMAPI (über Kunden entwickelt von Tools wie Windows PowerShell-Cmdlets) und der Azure-Verwaltungsportal clientseitige Verwaltungszertifikate erforderlich zusätzlich SSL-Zertifikate installiert werden sollen, konfigurieren. Es empfiehlt sich auch, dass Administratorzugriff mehrstufige Authentifizierung erforderlich ist.

Einige Applikationen oder Dienste, die Sie in Azure bereitstellen möglicherweise eigene Authentifizierung Verfahren für sowohl Endbenutzer und Administratorzugriff, während andere Azure AD zu nutzen. Je nach, ob Sie sind eine Föderation Anmeldeinformationen über Active Directory Federation Services (AD FS), Verzeichnissynchronisation verwenden oder Verwalten von Benutzerkonten in der Cloud ausschließlich aus, mit dem [Microsoft-Identität-Manager](https://technet.microsoft.com/library/mt218776.aspx) (Bestandteil der Azure AD Premium) unterstützt Sie beim Identität Stunden-zwischen den Ressourcen verwalten.

### <a name="connectivity"></a>Konnektivität

Mehrere Verfahren stehen secure Clientverbindungen mit Ihrer Azure virtuelle Netzwerke helfen. Zwei der folgenden Verfahren, [Standort-zu-Standort VPN](https://channel9.msdn.com/series/Azure-Site-to-Site-VPN) (S2S) und [Punkt-zu-Standort VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) (P2S), aktivieren Sie die Verwendung der Industry standard IPsec (S2S) oder [Secure Sockets Tunnel-Protokoll](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) (SSTP) (P2S) für die Verschlüsselung und Tunnel. Wenn Azure mit öffentlich zugänglichen Azure Services Management wie der Azure-Verwaltungsportal verbunden ist, erfordert Azure Hypertext Transfer Protocol Secure (HTTPS).

Eine eigenständige gesicherte Arbeitsstationen, die nicht über ein Gateway RD Azure Verbindung soll das SSTP-basierten Punkt-zu-Standort VPN zum Erstellen der ersten Verbindungs mit dem Azure-virtuellen Netzwerk verwenden, und dann RDP-Verbindung zu einzelnen virtuellen Computern aus mit dem VPN-Tunnel herstellen.

### <a name="management-auditing-vs-policy-enforcement"></a>Überwachung im Vergleich zu Durchsetzung Management

In der Regel, es gibt zwei Vorgehensweisen für Management-Prozesse Sicherung: Überwachung Durchsetzung von Richtlinien. Beide Aktionen bieten eine umfassende steuert, aber möglicherweise nicht in allen Situationen möglich. Darüber hinaus weist jede Ansatz verschiedene Detailebenen Risiken, Kosten und Arbeitsaufwand bei der Verwaltung von Sicherheit, besonders im Hinblick auf die Ebene von Einzelpersonen und Systemarchitekturen platziert Trust betrachtet.

Überwachung, Protokollierung und Überwachung bieten eine Basis für das Nachverfolgen und Grundlegendes zu administrative Aktivitäten, aber es immer möglicherweise nicht möglich, alle Vorgänge abgeschlossen ausführlich aufgrund der Datenmenge generiert überwacht werden. Es empfiehlt sich, ist jedoch die Effektivität der Informationsverwaltungsrichtlinien Überwachung.

Durchsetzung von Richtlinien, die auf strenge Steuerelemente enthält versetzt programmgesteuerten Verfahren in den Ort, an dem Administratoraktionen steuern kann, und es wird sichergestellt, dass alle möglichen Schutz Measures verwendet werden. Protokollierung bietet Nachweis der Durchsetzung, sowie eine Aufzeichnung, wer was, von wo und wann getan hat. Protokollierung auch ermöglicht es Ihnen, überwachen und überprüfen die Informationen wie Administratoren Richtlinien folgen und es stellt Beweisen Aktivitäten

## <a name="client-configuration"></a>Client-Konfiguration

Es empfiehlt sich die drei wichtigsten Konfigurationen für eine gesicherte Arbeitsstationen. Die wichtigsten Alleinstellungsmerkmale dazwischen sind Kosten, Nutzbarkeit und Eingabehilfen, bei weitgehender ähnlichem Sicherheitsprofil über alle Optionen. Die folgende Tabelle enthält eine kurze Analyse der Vorteile und Risiken zu den einzelnen. (Beachten Sie, dass "corporate PC" auf einem standard desktop PC-Konfiguration verweist, die für alle Benutzer der Domäne, unabhängig von der Rolle bereitgestellt werden sollte.)

| Konfiguration | Vorteile | Aufzulisten |
| ----- | ----- | ----- |
| Eigenständige gesicherte Arbeitsstationen | Eng gesteuert Arbeitsstationen | höhere Kosten für dedizierte desktops
| | Das Risiko einer Anwendung Angriffen | Höhere Verwaltungsaufwand |
| | Klare Trennung von Aufgaben | |
| Corporate PC als virtuellen Computern | Geringere Hardware-Kosten | |
| | Trennung der Rolle und Anwendungen | |
| Windows so konfigurieren, wechseln Sie durch BitLocker Laufwerk Verschlüsselung | Kompatibilität mit den meisten PCs | Bestandsnachverfolgung |
| | Kostengünstiger und Portabilität | |
| | Getrennte Management-Umgebung | |

Es ist wichtig, den abgesicherten Arbeitsstationen der Host und nicht den Gast mit nichts zwischen des Hostbetriebssystems und der Hardware ist. Folgen (auch als "sicheren Origin" bezeichnet) "Säubern Quelle Prinzip" bedeutet, dass der Host den abgesicherten sollte. Andernfalls unterliegt den abgesicherten Arbeitsstationen (Gast) Angriffen System auf dem sich ihr gehostet wird.

Sie können weiteren Aufteilen des Verwaltungsfunktionen durch dedizierten Systemabbilder für die einzelnen gesicherte Arbeitsstationen, die nur die Tools enthalten und Berechtigungen erforderlich für die Verwaltung von Azure auswählen und cloud-Clientanwendungen, mit einem bestimmten lokalen AD DS Gruppenrichtlinienobjekte für die notwendigen Aufgaben.

Für IT-Umgebungen, die keine lokalen Infrastruktur verfügen (z. B. keinen Zugriff auf einen lokalen AD DS Instanz für Gruppenrichtlinienobjekte, da alle Server in der Cloud befinden), kann ein Dienst wie [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx) vereinfachen, bereitstellen und Verwalten der Arbeitsstationen Konfigurationen.

### <a name="stand-alone-hardened-workstation-for-management"></a>Eigenständige gesicherte Arbeitsstationen für die Verwaltung

Mit einem eigenständigen gesicherte Arbeitsstationen haben Administratoren an einem PC oder einem Laptop, mit denen sie Verwaltungsaufgaben und einer anderen PC oder Laptop für nicht administrative Aufgaben. Eine dedizierte zum Verwalten Ihrer Dienste Azure Arbeitsstationen benötigt keine anderen Anwendung installiert. Darüber hinaus mithilfe von Arbeitsstationen, auf denen ein [Trusted Platform Module](https://technet.microsoft.com/library/cc766159) (TPM) oder eine ähnliche Hardware-Ebene unterstützt Verschlüsselung Technologie Unterstützung in Gerät Anmelde- und Prevention von bestimmter Angriffen. TPM kann die vollständige Volume-Schutz mit dem Systemlaufwerk auch mithilfe von [BitLocker Laufwerk Verschlüsselung](https://technet.microsoft.com/library/cc732774.aspx)unterstützen.

In der eigenständigen gesicherte Arbeitsstationen-Szenario (siehe unten) wird die lokale Instanz von Windows-Firewall (oder ein nicht-Microsoft-Client-Firewall) für eingehenden Verbindungen, wie z. B. RDP blockieren konfiguriert. Der Administrator kann melden Sie sich bei der gesicherte Arbeitsstationen und starten eine RDP-Sitzung die Verbindung zu Azure nach dem Einrichten des ein VPN eine Verbindung mit einer Azure-virtuellen-Netzwerk aber können keine melden Sie sich bei einem corporate PC und RDP Verbindung mit den abgesicherten Arbeitsstationen selbst verwenden.

![][2]

### <a name="corporate-pc-as-virtual-machine"></a>Corporate PC als virtuellen Computern

In Fällen, wo eine separate eigenständigen gesicherte Arbeitsstationen hoch oder nicht passen Kosten ist, kann die gesicherte Arbeitsstationen ein virtuellen Computers nicht administrativen Aufgaben hosten.

![][3]

Um mehrere Risiken vermeiden, die mit einem Computer für Systemmanagement sowie zu anderen Arbeitsaufgaben für tägliche auftreten können, können Sie eine Windows Hyper-V virtuellen Computers zu den abgesicherten Arbeitsstationen bereitstellen. Dieses virtuellen Computers können als dem Computer Ihres Unternehmens verwendet werden. Der PC-unternehmensumgebung kann von der Host isoliert bleiben seiner Oberfläche Angriffen reduziert und Kompatibilität mit sensiblen Verwaltungsaufgaben entfernt tägliche Benutzeraktivitäten (beispielsweise e-Mail).

Der corporate PC virtuellen Computern ausgeführt wird, in einem geschützten Leerzeichen und Benutzer Applications bietet. Der Host als "Säubern"Quelle bleibt und erzwingt strenge Netzwerkrichtlinien im Stamm-Betriebssystem (z. B. blockierende RDP Access des virtuellen Computers).

### <a name="windows-to-go"></a>Fenster wechseln

Eine andere Alternative zum mit einem eigenständigen gesicherte Arbeitsstationen Anforderung ist, [Wechseln Sie zum Windows](https://technet.microsoft.com/library/hh831833.aspx) Laufwerk ein Feature zu verwenden, die eine clientseitige USB-Boot-Funktion unterstützt. Wechseln Sie zum Windows ermöglicht Benutzern, starten Sie einen kompatiblen PC zu einem isoliert Systemabbild aus einer verschlüsselten USB-Laufwerk ausgeführt. Es bietet weitere Steuerelemente für Remote-Administration Endpunkte, da das Bild vollständig durch eine corporate IT-Gruppe mit strenge Richtlinien einer minimalen OS erstellen, verwaltet werden kann und TPM unterstützen.

In der folgenden Abbildung ist das portable Bild ein Domänenverbund Systems, das Verbindung zum Azure, nur vorkonfigurierten ist erfordert mehrstufige Authentifizierung, und alle nicht Management Datenverkehr blockiert. Wenn ein Benutzer dem gleichen Computer zu dem standard corporate Bild startet und versucht, den Zugriff auf RD-Gateway für Azure Verwaltungstools, werden die Sitzung blockiert. Wechseln Sie zum Windows wird das Betriebssystem auf der Stammebene und keine weiteren Ebenen sind erforderlich (Host Betriebssystem, Hypervisor, virtuellen Computern), die möglicherweise schneller zu Angriffen von außen.

![][4]

Es ist zu beachten, dass USB-Laufwerke einfacher als eine durchschnittliche desktop-PC verloren. BitLocker verschlüsselt das ganze Volumen, zusammen mit der ein sicheres Kennwort, wird es weniger wahrscheinlich zu nutzen, dass ein Angreifer das Laufwerksabbild für gefährlichen Zwecke verwenden kann. Darüber hinaus ist das USB-Laufwerk verloren geht, können widerrufen und [ein neues Management Zertifikat ausgegeben wird](https://technet.microsoft.com/library/hh831574.aspx) , sowie eine schnelle Kennwortrücksetzung Offenlegung. Administrative Überwachungsprotokolle befinden sich in Azure, nicht auf dem Client, weitere mögliche Datenverluste verringert werden.

## <a name="best-practices"></a>Bewährte Methoden

Erwägen Sie die folgenden zusätzlichen Richtlinien, wenn Sie die Anwendung und Daten in Azure verwalten.

### <a name="dos-and-donts"></a>Informationen zum Umgang

Angenommen Sie nicht, da ein Arbeitsstationen gesperrt wurde, dass andere allgemeine Sicherheit Anforderungen nicht erfüllt sein müssen. Das Risiko ist aufgrund erhöhten Zugriffsebenen, die in der Regel Administratorkonten besitzen höher. Beispiele für Risiken und deren alternativen sicheren Methoden sind in der folgenden Tabelle aufgeführt.

| Tue nicht | Gehen Sie wie folgt |
| ----- | ----- |
| Keine e-Mail-Anmeldeinformationen für Administratorzugriff oder anderen vertraulichen (z. B. SSL oder Management Zertifikate) | Vertraulichkeit durch die Bereitstellung von Namen und Kennwörter durch VoIP (aber nicht in einer Voicemail gespeichert werden), führen Sie eine remote-Installation von Client-/Server-Zertifikate (über eine verschlüsselte Sitzung) Konto verwalten, von einer geschützten Netzwerkfreigabe herunterladen oder von hand über Wechselmedien verteilen. |
| | Die vorausschauende verwalten Sie Ihrer Management Zertifikat Lebenszyklus. |
| Speichern Sie keine Kontokennwörter unverschlüsselter oder dauerhaften gestrichelte im Anwendungsspeicher (wie etwa Kalkulationstabellen, Dateifreigaben oder SharePoint-Websites). | Sicherheit-Prinzipien und Sichern von Richtlinien System herstellen, und sie für die Entwicklungsumgebung gelten. |
| | Verwenden Sie [Erweiterte Reduzierung Experience Toolkit 5.5](https://technet.microsoft.com/security/jj653751) Zertifikat anheften und Regeln, um die korrekten Zugriff auf Websites Azure SSL/TLS sicherzustellen. |
| Nicht freigeben Sie Konten und Kennwörter zwischen Administratoren oder wiederzuverwenden Sie Kennwörter auf mehrere Benutzerkonten oder Dienste, insbesondere die für soziale Medien oder andere nicht administrative Aktivitäten. | Erstellen Sie ein dediziertes Microsoft-Konto zum Verwalten Ihrer Azure-Abonnement – ein Konto aus, die für persönliche e-Mail nicht verwendet wird. |
| Keine e-Mail-Konfigurationsdateien aus. | Konfigurationsdateien und Benutzerprofilen sollten aus einer vertrauenswürdigen Quelle (beispielsweise eine verschlüsselte USB-Laufwerk), nicht über ein Verfahren installiert werden, die einfach, wie z. B. e-Mail gefährdet können. |
| Verwenden Sie keine Anmeldung schwachen oder einfache Kennwörter. | Erzwingen sichere Kennwortrichtlinien Ablauf Zyklen (Changeon-ersten-verwenden) Console Zeitlimit und automatische Konto sperren. Verwenden von einem Client Kennwort Management-System mit kombinierte Authentifizierung für Kennwort Tresor Zugriff. |
| Machen Sie nicht verfügbar, Management-Ports mit dem Internet. | Sperren Sie Azure Ports und IP-Adressen zum Einschränken des Zugriffs Verwaltung. Weitere Informationen finden Sie unter der [Azure Network Security]-Whitepaper (http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx). |
| | Verwenden von Firewalls, VPN und NAP für alle Management Verbindungen. |

## <a name="azure-operations"></a>Azure Vorgänge

Verwenden innerhalb des Microsoft Azure Bedienung Vorgänge Engineers und Supportmitarbeiter, die den Azure Herstellung Systeme zugreifen [Arbeitsstationen PCs mit virtuellen Computern abgesichert](#stand-alone-hardened-workstation-for-management) nach der Bereitstellung diesen für Zugriff auf das interne Unternehmensnetzwerk und Applikationen (beispielsweise E-mail, Intranet usw.). Alle Management Arbeitsstationen über TPMs verfügen, das Host Boot-Laufwerk mit BitLocker verschlüsselt ist und sie mit einer spezielle (Organisationseinheit) verknüpft sind, in Microsoft des primären corporate Domäne.

System zum Sichern des wird über Gruppenrichtlinien, bei der Aktualisierung zentralisierte Software erzwungen. Überwachung und Analyse werden Ereignisprotokollen (z. B. Sicherheit und AppLocker) von Management Arbeitsstationen gesammelt und an einem zentralen Speicherort gespeichert.

Darüber hinaus werden dedizierte Sprung-Feldern in Microsoft Netzwerk zweifaktorielle Varianzanalyse Authentifizierung erforderlich ist, die Verbindung zum des Azure produktiv-Netzwerk verwendet.

## <a name="azure-security-checklist"></a>Checkliste für die Sicherheit von Azure

Die Anzahl der Aufgaben, die Administratoren für eine gesicherte Arbeitsstationen durchführen können minimiert hilft Angriffen Oberfläche in der Entwicklung und Management-Umgebung zu minimieren. Verwenden Sie die folgenden Technologien zum Schutz des Computers gesicherte aus:

- Zum Sichern des IE. Der Browser Internet Explorer (oder einer beliebigen Webbrowser für diese Angelegenheit) ist ein Key Einstiegspunkt für gefährlichen Code aufgrund dessen umfassende Interaktionen mit externen Servern. Überprüfen Sie Ihre Clientrichtlinien und erzwingen Sie im geschützten Modus ausgeführt, Add-ons zu deaktivieren, deaktivieren Dateidownloads und mithilfe von [Microsoft SmartScreen-](https://technet.microsoft.com/library/jj618329.aspx) filtern. Stellen Sie sicher, dass die Sicherheit Warnungen angezeigt werden. Nutzen Sie die Vorteile von Internet-Zonen, und erstellen Sie eine Liste der vertrauenswürdigen Websites, für die Sie angemessenen zum Sichern des konfiguriert haben. Blockieren von anderen Websites und im Browser-Code, wie etwa ActiveX und Java.
- Standard-Benutzer. Ausführen, wie ein standard-Benutzer eine Reihe von Vorteilen bringt, ist der größte der, Diebstahl Administratorberechtigungen über Schadsoftware schwieriger wird. Darüber hinaus ein standard-Benutzerkonto hat keinen erhöhten im Stamm-Betriebssystem und viele Konfigurationsoptionen und APIs standardmäßig gesperrt sind.
- AppLocker. [AppLocker](http://technet.microsoft.com/library/ee619725.aspx) können Sie bestimmte Programme und Skripts, die Benutzern ausgeführt werden kann. Sie können im Modus Audit oder Durchsetzung AppLocker ausführen. AppLocker verfügt standardmäßig über eine Regel zum gewähren, die Benutzern ermöglicht, die ein Administrator Token alle Code auf dem Client ausgeführt haben. Mit dieser Regel vorhanden ist, um zu verhindern, dass Administratoren selbst, sperren, und es gilt nur für erhöhten Token. Siehe auch Codeintegrität als Teil des Windows Server [Core Sicherheit](http://technet.microsoft.com/library/dd348705.aspx).
- Code signieren. Code signieren aller Tools und Skripts, die von Administratoren verwendet bietet es sich um eine verwaltbare Methode zum Bereitstellen von Anwendung Sperrung Richtlinien. Hashes nicht mit schnellen Änderungen an den Code skalieren und Dateipfade bieten eine hohe Sicherheitsstufe keine. Sie sollten AppLocker-Regeln mit einer PowerShell [Ausführungsrichtlinie](http://technet.microsoft.com/library/ee176961.aspx) kombinieren, die nur bestimmte signierte Code und Skripts [ausgeführt](http://technet.microsoft.com/library/hh849812.aspx)werden können.
- Gruppenrichtlinien. Erstellen Sie eine globale administrative Richtlinie, die angewendet wird, und auf diesen Arbeitsstationen authentifiziert Benutzerkonten alle Domäne Arbeitsstationen, die für Management (und Zugriff blockieren von allen anderen) verwendet wird.
- Erweiterter Sicherheit bereitgestellt. Geplante gesicherte Arbeitsstationen Bild zum Schutz vor Manipulation zu schützen. Verwenden Sie zum Speichern von Bildern, Skripts und virtuellen Computern Sicherheitsmaßnahmen wie Verschlüsselung und Isolation und schränken Sie des Zugriffs (zum Verwenden eines überwachenden in/Kontrollkästchen-Auschecken Prozess ein).
- Patch. Verwalten einer konsistenten erstellen (oder separate Bilder für die Entwicklung, Vorgänge und andere Verwaltungsaufgaben haben), Suche nach Änderungen Schadsoftware regelmäßig, den Build auf dem neuesten Stand halten und Autos nur aktivieren, wenn sie benötigt werden.
- Verschlüsselung. Stellen Sie sicher, dass Management Arbeitsstationen über ein TPM sicherer [Datenbankschutz File System](https://technet.microsoft.com/library/cc700811.aspx) (EFS) und BitLocker aktivieren verfügen. Wenn Sie Windows wechseln Sie zu arbeiten, verwenden Sie nur verschlüsselte USB-Schlüssel zusammen mit BitLocker.
- Governance. Verwenden Sie AD DS Gruppenrichtlinienobjekte alle der Administratoren Windows-Schnittstellen, z. B. Dateifreigabe steuern. Einbeziehen Sie von Verwaltungsarbeitsstationen in Überwachung, für die Überwachung und Protokollierung von Prozessen. Nachverfolgen von Administrator und Entwicklertools Zugriff und die Verwendung.

## <a name="summary"></a>Zusammenfassung

Verwenden eine gesicherte Arbeitsstationskonfiguration für die Verwaltung Ihrer Azure Cloud Services, virtuellen Computern und Applikationen kann Ihnen helfen zu vermeiden, zahlreiche Risiken und Risiken, die vom Remote verwalten kritische IT-Infrastruktur stammen. Sowohl Azure und Windows bieten Verfahren, mit denen Sie besser schützen und Kommunikation, Authentifizierung und Client Verhalten steuern können.

## <a name="next-steps"></a>Nächste Schritte
Die folgenden Ressourcen stehen zur Verfügung, um weitere allgemeine Informationen zu Azure bereitzustellen und verwandte Dienste von Microsoft, über bestimmte Elemente, die in diesem Dokument verwiesen wird:

- [Sichern von berechtigten Zugriff](https://technet.microsoft.com/library/mt631194.aspx) – technische Details für das Entwerfen und Erstellen einer secure administrativen Arbeitsstationen für Azure Management abrufen
- [Microsoft-Trust Center](https://www.microsoft.com/TrustCenter/Security/AzureSecurity) – erfahren Sie mehr über Azure-Plattform-Funktionen, die Schutz der Azure-Struktur und der Auslastung, die auf Azure ausgeführt werden.
- [Microsoft-Antwort Sicherheitscenter](http://www.microsoft.com/security/msrc/default.aspx) -, in dem Microsoft-Sicherheitsschwachstellen, Probleme mit Azure, einschließlich gemeldet werden können, oder per e-Mail an[secure@microsoft.com](mailto:secure@microsoft.com)
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – beibehalten, die auf dem neuesten Stand auf die aktuellsten Azure-Sicherheit

<!--Image references-->
[1]: ./media/azure-security-management/typical-management-network-topology.png
[2]: ./media/azure-security-management/stand-alone-hardened-workstation-topology.png
[3]: ./media/azure-security-management/hardened-workstation-enabled-with-hyper-v.png
[4]: ./media/azure-security-management/hardened-workstation-using-windows-to-go-on-a-usb-flash-drive.png
