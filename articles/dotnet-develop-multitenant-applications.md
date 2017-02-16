<properties
    pageTitle="Die Anwendung Muster mit mehreren Mandanten Web | Microsoft Azure"
    description="Suchen Sie nach strukturierte Übersichten und entwurfmustern, die beschreiben, wie eine Webanwendung mit mehreren Mandanten auf Azure implementiert wird."
    services=""
    documentationCenter=".net"
    authors="wadepickett" 
    manager="wpickett"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/05/2015"
    ms.author="wpickett"/>

# <a name="multitenant-applications-in-azure"></a>Mandantenfähigen Applications in Azure

Eine mandantenfähigen Anwendung ist eine freigegebene Ressource, die Berechtigung ermöglicht Benutzern separate oder "Mandanten," zum Anzeigen der Anwendung, als ob es eigene wurde. In dem alle Benutzer der Anwendung möchten möglicherweise andernfalls über die gleiche grundlegende geschäftliche Anforderungen jedoch die benutzererfahrung ist eine Situation, die zu einer mandantenfähigen Anwendung führt. Beispiele für große mandantenfähigen Applikationen sind Office 365, Outlook.com und visualstudio.com.

Verknüpfen die Vorteile von Tenancy im Hinblick auf eine Anwendung-Anbieter hauptsächlich mit Betrieb und Kosten Effizienz aus. Eine Version Ihrer Anwendung kann die Anforderungen viele Mandanten/Kunden, gleicht Konsolidierung des Systems administrativen Aufgaben wie Überwachung, Leistung optimieren, Software-Wartung und Sicherungskopien von Daten aus.

Die folgenden enthält eine Übersicht über die wichtigsten Ziele und Anforderungen hinsichtlich des Anbieters.

- **Provisioning**: Sie müssen möglicherweise neue Mandanten für die Anwendung bereitstellen.  Für mandantenfähigen Applikationen mit einer großen Anzahl von Mandanten ist es normalerweise erforderlich, um dieses Verfahren zu automatisieren, durch das Aktivieren der Self-service-Bereitstellung.
- **Wartung**: Sie müssen möglicherweise aktualisieren Sie die Anwendung und anderen Wartungsaufgaben durchführen, während Sie mehrere Mandanten daran arbeiten.
- **Überwachung**: Sie müssen möglicherweise die Anwendung jederzeit Probleme erkennen und um ihnen bei der Problembehandlung zu überwachen. Dies umfasst die Überwachung wie jede Mandanten der Anwendung verwendet wird.

Eine ordnungsgemäß implementierte mandantenfähigen Anwendung bietet die folgenden Vorteile für Benutzer.

- **Grad der Isolation**: die Aktivitäten der einzelnen Mandanten wirken sich nicht auf die Verwendung der Anwendung von anderen Mandanten. Mandanten zugreifen nicht gegenseitig Daten. Es wird, den Mandanten angezeigt, als wäre sie ausschließlich die Verwendung der Anwendung haben.
- **Verfügbarkeit**: einzelne Mandanten möchten, zur beständig, vielleicht mit in einer Vereinbarung zum SERVICELEVEL festgelegten Garantien zur Verfügung. In diesem Fall sollte die Aktivitäten von anderen Mandanten keinen Einfluss auf die Verfügbarkeit der Anwendung.
- **Skalierbarkeit**: die Anwendung skaliert Nachfrage der einzelnen Mandanten zu. Die Anwesenheitsinformationen und anderen Mandanten Aktionen sollte keinen Einfluss auf die Leistung der Anwendung.
- **Kosten**: Kosten geringer als das eine dedizierte, Single-Mandanten Anwendung ausführen, da mehrere Mandanten kann Ressourcen gemeinsam verwendet werden.
- **Berichtsebene**. Die Möglichkeit zum Anpassen der Anwendung für eine einzelne Mandanten auf verschiedene Weise wie hinzufügen oder Entfernen von Features, Farben und Logos ändern oder sogar Hinzufügen ihrer eigenen Code oder Skripts.

Kurz gesagt, während es viele Aspekte, die Sie berücksichtigen müssen gibt, eine hochgradig skalierbare Dienste bereitzustellen, gibt es auch eine Reihe von der Ziele und Anforderungen, die viele mandantenfähigen Clientanwendungen gemeinsam sind. Einige sind möglicherweise nicht in bestimmten Szenarien relevant, und die Bedeutung der einzelnen Ziele und Anforderungen in allen diesen Szenarios unterscheiden sich. Als Anbieter der Anwendung mandantenfähigen auch müssen Ziele und Anforderungen als, Sie der Mandanten Ziele und Anforderungen Rentabilität, Abrechnung, mehrere Service-Level, Bereitstellung, Verwaltung und Überwachung Automatisierung Besprechung.

Weitere Informationen zum Weitere Entwurfsaspekte einer mandantenfähigen-Anwendung finden Sie unter [eine Anwendung mit mehreren Mandanten auf Azure hosten][]. Informationen zu allgemeinen Daten Architektur Mustern von mit mehreren Mandanten Software als Service (SaaS) datenbankanwendungen finden Sie unter [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit Azure SQL-Datenbank](./sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure bietet viele Funktionen, mit die Sie die wichtigsten Probleme beim Entwerfen eines Systems mandantenfähigen behoben werden können.

**Grad der Isolation**

- Segment Website Mandanten durch Hostheader mit oder ohne SSL-Kommunikation
- Segment Website Mandanten durch Abfrageparameter
- Webdienste in Worker-Rollen
    - Worker-Rollen. zu verarbeiten, die in der Regel Daten auf die Back-End-einer Anwendung.
    - Webrollen, die in der Regel als der Front-End für Applikationen dienen.

**Speicher**

Datenverwaltung wie Azure SQL-Datenbank oder Azure-Speicher Diensten wie die Tabelle-Dienst, der Dienste für Speicher große Datenmengen unstrukturierte Daten und der Blob-Dienst bietet Dienste zum Speichern von großer Textmengen der unstrukturierten bereitstellt oder binäre Daten wie Video, Audio und Bilder.

- Mehrere zum Schutz von Daten in SQL-Datenbank, die entsprechenden pro-Mandanten-SQL Server-Benutzernamen.
- Mit Azure Tabellen für die Anwendung Ressourcen durch Angabe einer Ebene Zugriffsrichtlinie Container können Sie die Möglichkeit, die Berechtigungen so anpassen, dass die neuen URLs für die Ressourcen, die mit freigegebenen Access Signaturen geschützt ausgeben müssen.
- Azure Warteschlangen für Anwendung Ressourcen Azure Warteschlangen werden häufig verwendet, um Laufwerk Verarbeitung für den Mandanten, aber möglicherweise auch verwendet werden, um die Verteilung der Aufgaben, die für die Bereitstellung oder Verwaltung erforderlich ist.
- Bus Servicewarteschlangen für Ressourcen für die Anwendung, die legt eignen sich, gemeinsam genutzten Dienst, Sie können eine einzelne Warteschlange verwenden, bei denen ist jeden Absender Mandanten nur Berechtigungen (wie von Ansprüchen ausgestellt von ACS abgeleitet), um an diese Warteschlange während er sich nur die Empfängern vom Dienst berechtigt, die Daten aus mehreren Mandanten stammen aus der Warteschlange zu ziehen.


**Verbindung und Security Services**

- Azure Service Bus, eine Infrastruktur, die zwischen Applikationen so, dass sie zum Austauschen von Nachrichten in einer grob verknüpften Weise für verbesserte skalieren und Stabilität befindet.

**Netzwerkdienste**

Azure bietet mehrere Netzwerke Dienste, die Authentifizierung unterstützt, und die Verwaltung Ihrer gehosteten Programme verbessert. Diese Services umfassen Folgendes:

- Azure virtuelles Netzwerk können Sie bereitstellen und Verwalten von VPN (VPN) in Azure sowie sicher verknüpfen diese mit lokalen IT-Infrastruktur.
- Virtuelle Netzwerk Datenverkehr Manager können Sie Saldo eingehenden Datenverkehr für mehrere gehostete Azure-Dienste zu laden, ob er im gleichen Datencenter oder für andere Rechenzentren auf der ganzen Welt ausführen.
- Azure Active Directory (Azure AD) ist ein modernes, REST-basierten Dienst, der Identität Verwaltungs- und Access-Steuerelement-Funktionen für die Cloud-Anwendung bereitstellt. Mithilfe von Azure AD für Anwendungsressourcen, die in Azure AD bietet eine einfache Möglichkeit authentifizieren und Autorisieren von Benutzern den Zugriff auf Ihre Webanwendungen und Dienste, während Sie die Features der Authentifizierung und Autorisierung zu nutzen von Code angepasst werden.
- Azure Service dieses Bedienfeld enthält ein sicheres messaging und verteilt Daten Nachrichtenfluss für Hybrid Applications, z. B. Kommunikation zwischen Azure gehostet Applikationen und lokalen Anwendungen und Dienste ohne komplexe Infrastruktur für Firewall und Sicherheit. Zu den Mandanten (z. B. außerhalb des Systems, wie z. B. lokal gehostet) gehören möglicherweise mit Service Bus Relay für Ressourcen Anwendung mit den Diensten, die als Endpunkte verfügbar gemacht werden, oder es handelt es sich um Dienste, die nach der Bereitstellung speziell für den Mandanten (da vertrauliche, Mandanten-spezifische Daten in diese übermittelt wird).



**Bereitstellen von Ressourcen**

Azure bietet eine Reihe von Methoden Bereitstellen von neuen Mandanten, für die Anwendung. Für mandantenfähigen Applikationen mit einer großen Anzahl von Mandanten ist es normalerweise erforderlich, um dieses Verfahren zu automatisieren, durch das Aktivieren der Self-service-Bereitstellung.

- Worker-Rollen können Sie bereitstellen und Freigabe bereitstellen pro Mandant Ressourcen (z. B. wenn ein neuer Mandanten Vorzeichen auszurichten oder abgebrochen wird), sammeln Kennzahlen aus, für die Erfassung verwenden und Verwalten von Skalierung nach einem bestimmten Zeitplan oder als Reaktion auf dem überschreiten Schwellenwerte von Key Performance Indicators. Diese dieselbe Rolle möglicherweise auch verwendet werden, um Pushbenachrichtigungen, Updates und Upgrades auf die Lösung.
- Azure Blobs verwendet, um die Bereitstellung von berechnen oder vorab Initialisierung können Speicherressourcen für den neuen Mandanten während der Ebene Container-Richtlinien zum Schutz der Computer-service-Paketen virtuelle Festplatte Bildern und anderen Ressourcen.
- Optionen für die Bereitstellung von Ressourcen SQL-Datenbank für einen Mandanten umfassen Folgendes:

    -   DDL in Skripts oder als Ressourcen in Assemblys eingebettete
    -   SQL Server 2008 R2 DAC verteilten Pakete programmgesteuert.
    -   Kopieren aus einer Datenbank master Bezug
    -   Verwenden von beim Bereitstellen von neuen Datenbanken aus einer Datei exportieren und importieren.



<!--links-->

[Eine Anwendung mit mehreren Mandanten auf Azure Hostinganbieter]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
