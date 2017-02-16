<properties
    pageTitle="Melden Sie sich Datenschutz Analytics | Microsoft Azure"
    description="Informationen Sie zu wie Log Analytics Schutz Ihrer Privatsphäre und Ihre Daten sichert."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Melden Sie sich Analytics-Daten-Sicherheit

Microsoft sieht sich verpflichtet, Ihre Privatsphäre zu schützen und Schutz der Daten, während der Vorführung-Software und-Diensten, die Ihnen helfen die IT-Infrastruktur Ihrer Organisation verwalten. Wir erkennen, wenn Sie Ihre Daten vertrauen, die strenge Sicherheit erforderlich ist. Microsoft befolgt strict Compliance- und Richtlinien – von der Codierung zum Ausführen eines Dienstes.

Sichern und Schützen von Daten ist höchste Priorität bei Microsoft. Wenden Sie sich an uns mit Fragen, Vorschläge oder über die folgenden Informationen, einschließlich unsere Sicherheitsrichtlinien bei [Azure Supportoptionen](http://azure.microsoft.com/support/options/)Probleme.

In diesem Artikel wird erläutert, wie Daten gesammelt, verarbeitet und durch Log Analytics in Vorgänge Management Suite (OMS) gesichert werden. Sie können Agents Verbinden mit dem Webdienst, System Center Operations Manager Betrieb Daten erfassen oder Abrufen von Daten aus Azure Diagnose für die Verwendung durch Log Analytics verwenden. Die gesammelten Daten werden gesendet, über das Internet mithilfe von Zertifikaten basierende Authentifizierung und SSL 3 an den Log Analytics-Dienst, der in Microsoft Azure gehostet wird. Daten werden vom Agent komprimiert, bevor sie gesendet wird.

Der Log Analytics-Dienst verwaltet die Daten cloudbasierten sicher mithilfe der folgenden Methoden:

- Trennung der Daten
- Beibehaltung der Daten
- physische Sicherheit
- Problem-management
- Compliance
- Sicherheit Standards Zertifizierung


## <a name="data-segregation"></a>Trennung der Daten

Kundendaten werden auf jede Komponente in der gesamten OMS-Dienst logisch getrennt gespeichert. Alle Daten pro Organisation gekennzeichnet ist. Kategorisieren von diesem im gesamten Datenlebenszyklus weiterhin besteht, und jeder Ebene des Diensts wird erzwungen. Jeder Customer hat einen dedizierten Azure Blob, der die Daten langfristigen aufnimmt.

## <a name="data-retention"></a>Beibehaltung der Daten

Indizierte Log Suchdaten gespeichert und gemäß Ihres Plans Preisgestaltung beibehalten. Weitere Informationen finden Sie unter [Log Analytics Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft löscht Kundendaten 30 Tage nach dem Schließen des Arbeitsbereichs OMS. Microsoft löscht auch der Azure-Speicher-Konto, in dem die Daten befinden. Kundendaten entfernt werden, werden keine physische Laufwerke gelöscht.

Die folgende Tabelle enthält einige der jeweiligen der Lösung im OMS sowie Beispiele für die Arten von Daten, dass er sammeln.

| **Lösung** | **Datentypen** |
| --- | --- |
| Konfiguration Bewertung | Von Konfigurationsdaten, Metadaten und von Zustandsdaten |
| Planen der Kapazität | Performance-Daten und Metadaten |
| Modul | Von Konfigurationsdaten und Metadaten |
| System Update Bewertung | Metadaten und Zustand von Daten |
| Log-Management | Benutzerdefinierte Ereignisprotokollen, Windows-Ereignisprotokollen und/oder IIS-Protokolle |
| Nachverfolgen von Änderungen | Software Inventory und Windows-Dienst-Metadaten |
| SQL und Active Directory-Bewertung | WMI-Daten, Registrierungsdaten, Performance-Daten und SQL Server dynamische Management Ergebnisse anzeigen |

Die folgende Tabelle zeigt Beispiele für Datentypen:

| **Datentyp** | **(Felder)** |
| --- | --- |
| Benachrichtigen | Benachrichtigen Sie Name, Beschreibung der Warnung, BaseManagedEntityId, Problem-ID, IsMonitorAlert, Regel-ID, ResolutionState, Priorität, schwere, Kategorie, Besitzer, ResolvedBy, TimeRaised, TimeAdded, LastModified, zuletzt geändert von, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguration | "CustomerID", AgentID, %EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Ereignis | EventId, EventOriginalID, BaseManagedEntityInternalId, Regel-ID, des PublisherId, PublisherName, FullNumber, Anzahl, Kategorie, ChannelLevel, LoggingComputer, EventData, EventParameter, TimeGenerated, TimeAdded <br>**Hinweis:** Wenn Sie Ereignisse für benutzerdefinierte Felder in der Windows-Ereignisprotokoll protokollieren, sammelt OMS diese aus. |
| Metadaten | BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, Netzwerkname, IP-Adresse, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-Adresse, NetbiosDomainName, LogicalProcessors, DNS-Name, DisplayName, DomainDnsName, ActiveDirectorySite, ' PrincipalName ', OffsetInMinuteFromGreenwichTime |
| Leistung | Objektname CounterName PerfmonInstanceName PerformanceDataId PerformanceSourceInternalID SampleValue TimeSampled, TimeAdded |
| Bundesstaat | StateChangeEventId, State-ID, NewHealthState, OldHealthState, Kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Physische Sicherheit

Der Analytics Log in OMS-Dienst wird von Microsoft-Personal besetzt und alle Aktivitäten werden protokolliert und überwacht werden können. Der Dienst wird vollständig in Azure ausgeführt und mit den Azure common engineering Criteria übereinstimmen. Sie können Details eines Anlagegutes Azure die physische Sicherheit auf Seite 18 der [Übersicht über die Sicherheit von Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)anzeigen. Physische Zugriffsrechte für Bereiche secure sind für jeden in einen Arbeitstag geändert, die nicht mehr Zuständigkeit für den OMS-Dienst, einschließlich durchstellen und Beendigung hat. Sie können über die globale physische Infrastruktur lesen, die wir in [Microsoft-Rechenzentren](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx)verwenden.

## <a name="incident-management"></a>Problem-management

OMS verfügt über ein Vorfall Management-Prozess, dem die alle Microsoft-Dienste zu entsprechen. Zusammenfassung, wir:

- Verwenden eines freigegebenen Zuständigkeit-Modells, in dem Microsoft gehört Anteil Zuständigkeitsbereich Sicherheit, während einige Anteil an den Kunden gehört
- Verwalten der Sicherheit von Azure Fälle
  - Erkennen Sie einen Vorfall bei den ersten Hinweis darauf, um eine Untersuchung zu starten.
  - Bewerten der Auswirkung und Schwere der ein Vorfall durch ein Teammitglied Vorfall Antwort auf Anrufen. Auf der Grundlage von Beweisen auch nicht in weiteren Weiterleitung Team Antwort Sicherheit die Bewertung führen oder.
  - Diagnostizieren Sie einen Vorfall von Sicherheit Antwort Experten beantwortet die technische oder gerichtliche Untersuchung durchführen, dieses Problem zu umgehen, Reduzierung und Eingrenzung Strategien zu identifizieren. Wenn das Sicherheitsteam zu sein Kundendaten an eine rechtswidrige oder nicht autorisierte Person bereitgestellt werden scheint, beginnt parallele Ausführung des Prozesses Kunden Vorfall Benachrichtigung parallel.  
  - Stabilisieren und dem Vorfall wiederherstellen. Mitglied des Teams erstellt einen Wiederherstellungsplan, um das Problem zu verringern. Krise Eingrenzung Schritte wie isolieren betroffene Systeme können sofort und parallel mit Diagnose auftreten. Möglicherweise mehr Ausdruck Problembehebungen geplant werden die nach Ablauf des Risikos sofortige auftreten.  
  - Schließen Sie den Vorfall und durchführen Sie einer Projektabschluss. Mitglied des Teams erstellt eine Projektabschluss, die die Details der Vorfall, mit der Absicht, Richtlinien, Verfahren und Prozesse zu verhindern, dass Maßnahmen eine Wiederholung des Ereignisses überarbeiten werden.
- Benachrichtigen von Kunden handelt
  - Bestimmen Sie den Umfang des betroffenen Kunden und jeder bereitzustellen, die wie eine Mitteilung möglichst detaillierte beeinträchtigt wird
  - Erstellen Sie eine Mitteilung zum angeben, dass Kunden mit genügend Informationen erhalten Sie, damit sie Untersuchungen ihrem Ende ausführen und aller Zusagen, die sie an die Endbenutzer vorgenommen haben, während nicht unangemessen verzögern des Benachrichtigungsprozess entsprechen können.
  - Bestätigen und den Vorfall, je nach Bedarf deklarieren.
  - Benachrichtigen Sie Kunden mit einer Vorfall Benachrichtigung ohne unangemessene Verzögerung und gemäß Gesetz oder Vertrag darüber hinaus. Benachrichtigung handelt wird auf eine oder mehrere Administratoren des Kunden in keiner Weise übermittelt werden, die Microsoft per e-Mail einschließlich wählt.
- Durchführen von Team-Bereitschaft und Schulung
  - Microsoft-Personal sind ausführen, Sicherheit und Präsenz Schulung, die leichter zu erkennen und melden Sie Sicherheitsprobleme mit einer verdächtigen erforderlich.  
  - Arbeiten an den Microsoft Azure Service Operatoren haben Erweiterung Schulung Verpflichtung, deren Zugriff auf vertrauliche Systeme Hostinganbieter Kundendaten umgebenden.
  - Microsoft Sicherheit Antwort Personal erhalten spezielle Schulungen für ihre Rollen


Bei einem Verlust der Daten des Kunden benachrichtigen wir einzelnen Kunden innerhalb eines Tages aus. Verlust von Daten ist jedoch nie mit OMS aufgetreten. Darüber hinaus wir aufbewahren Kopien von Daten, die erstellt wurde, und geografischen wird.

Weitere Informationen darüber, wie Microsoft zu Sicherheit Ereignisse reagieren soll finden Sie unter [Microsoft Azure Sicherheitsantwort in der Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Compliance

Die OMS Software Entwicklung und Service Teams Informationen Sicherheit und Governance Programm unterstützt seine geschäftliche Anforderungen und bei [Microsoft Azure-Trust Center](https://azure.microsoft.com/support/trust-center/) und [Microsoft für das Trust Center Compliance](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx)beschriebenen Gesetzen und Vorschriften befolgt. Wie OMS Sicherheit Anforderungen herstellt, gibt Sicherheit Steuerelemente, verwaltet und überwacht die Risiken werden auch es beschrieben. Verzinst wird, führen wir eine Bewertung von Richtlinien, Standards, Prozeduren und Richtlinien.

Jedes OMS Entwicklung Teammitglied empfängt formalen Anwendung-Schulung. Verwenden Sie intern, ein System mit Version Steuerelement für die Softwareentwicklung. Jedes Softwareprojekt geschützt ist durch Version Control System.

Microsoft hat ein Sicherheits- und Compliance-Team, die überwacht und bewertet alle Microsoft-Dienste. Informationen Security Officer bilden das Team und sind Sie nicht mit der engineering Abteilungen, die OMS entwickeln verknüpft. Sicherheits-Managern eigene Managementkette und Durchführen von unabhängigen Bewertung von Produkten und Dienstleistungen zu Sicherheit und Einhaltung von Vorschriften zu gewährleisten.

Microsoft Aufsichtsrat durch benachrichtigt, und Jahresbericht über alle Sicherheit bei Microsoft Programme.

Das OMS Software Entwicklung und Service-Team arbeitet mit Microsoft Legal and Compliance-Teams und andere Industry Partner zum Erfassen von einer Vielzahl von Zertifizierung aktiv.

## <a name="security-standards-certifications"></a>Sicherheit Standards Zertifizierung

Log Analytics in OMS wird derzeit die folgenden Sicherheitsstandards erfüllen:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) und [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) kompatibel
- Zahlung-Karte (PCI-Compliance) Daten Sicherheit Industriestandard (PCI DSS) vom PCI Security Standards Rat.
- [Dienst Organisation Steuerelemente (SOC) 1 Type 1- und SOC 2 type1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) kompatibel
- Windows allgemeine technisch Kriterien
- Microsoft vertrauenswürdigen Computing-Zertifizierung
- Als Azure-Dienst erfüllen Azure Vorschriften die Komponenten, die OMS verwendet. Sie können weitere Informationen an [Microsoft für das Trust Center Compliance](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Cloud-Sicherheitsdatenfluss computing
Das folgende Diagramm veranschaulicht eine Cloud Security Architektur als des Informationsflusses aus Ihrem Unternehmen, und wie sie gesichert wird ungeändert wechselt zur Log Analytics Dienst schließlich von Ihnen im Portal OMS angezeigt. Weitere Informationen zu jedem Schritt folgt auf das Diagramm.

![Abbildung des OMS Datensammlung und Sicherheit](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Melden Sie sich bei Log Analytics und Sammeln von Daten

Für Ihre Organisation zum Senden von Daten zu Log Analytics konfigurieren Sie die Windows-Agents, Agents unter Azure-virtuellen Computern oder OMS-Agents für Linux. Wenn Sie Operations Manager-Agents verwenden, erhalten Sie einen Konfigurations-Assistenten so konfigurieren Sie diese in der Operations verwenden. Benutzer (die, andere einzelne Benutzer oder eine Gruppe von Personen möglicherweise) erstellen eine oder mehrere OMS-Konten (OMS Arbeitsbereiche) und Agents registrieren, indem Sie eine der folgenden Konten:


- [Organisations-ID](../active-directory/sign-up-organization.md)

- [Microsoft-Konto - Outlook Office Live, MSN](http://www.microsoft.com/account/default.aspx)

Ein Arbeitsbereich OMS ist, in dem Daten gesammelt, zusammengefasster, analysiert und präsentiert werden. Ein Arbeitsbereich wird vorrangig als eine Möglichkeit, Daten, und jeder Arbeitsbereich ist eindeutig. Sie möchten beispielsweise haben Ihrer Daten mit einer OMS Workspace verwaltet und Ihre Testdaten, die mit einem anderen Arbeitsbereich verwaltet wird. Arbeitsbereiche können auch ein Administrator Steuern des Benutzerzugriffs mit den Daten aus. Jeder Arbeitsbereich kann mehrere Benutzerkonten zugeordnet haben, und jedes Benutzerkonto mehrere OMS Arbeitsbereich zugreifen kann. Erstellen Sie Arbeitsbereiche, die auf der Grundlage Datacenter Region ein. Jeder Arbeitsbereich wird auf anderen Rechenzentren in der Region, vor allem für OMS dienstverfügbarkeit repliziert.

Nach Abschluss des Assistenten, stellt jedes Management Group unter Operations Manager für Operations Manager eine Verbindung mit dem Dienst Log Analytics her. Klicken Sie dann verwenden Sie den Assistenten zum Hinzufügen von Computern auswählen, welche Computer in der Gruppe Management zum Senden von Daten mit dem Dienst zulässig sind. Für andere Typen Agent verbindet jede sicher an den OMS-Dienst aus.

Die gesamte Kommunikation zwischen verbundenen Systeme und das Protokoll Analytics-Dienst ist verschlüsselt.  Das Protokoll TLS (HTTPS) wird für die Verschlüsselung verwendet.  Der Microsoft SDL-Prozess wird verfolgt, um sicherzustellen, dass Log Analytics mit den jüngsten Fortschritten in cryptographic Protokolle aktualisiert wird.

Jede Art von Agent sammelt Daten für Protokoll Analytics. Der Typ des gesammelten Daten hängt von den vorstehend beschriebenen Lösungen verwendet. Sie können eine Zusammenfassung der Datensammlung in [Hinzufügen Log Analytics Solutions aus dem Lösungskatalog](log-analytics-add-solutions.md)anzeigen. Ausführlichere Informationen der Websitesammlung steht darüber hinaus für die meisten Lösungen. Eine Lösung ist bündeln vordefinierte Ansichten, Log Suchabfragen, Daten Websitesammlung Regeln und Verarbeitungslogik. Nur Administratoren können Log Analytics verwenden, um eine Lösung zu importieren. Nachdem die Lösung importiert wurde, wird er verschoben auf die Operations Manager Management Server (falls verwendet), und klicken Sie dann auf alle Agents, die Sie ausgewählt haben. Die Agents sammeln danach die Daten.

## <a name="2-send-data-from-agents"></a>2. senden Sie die Daten von agents

Registrieren Sie alle Arten von Agents, mit einer Registrierungs-Taste, und eine sichere Verbindung zwischen dem Agent und der mithilfe von Zertifikaten basierende Authentifizierung und SSL mit Port 443 Log Analytics-Dienst hergestellt wird. OMS verwendet einen geheimen Speicher zum Generieren und pflegen von Tasten ein. Private Schlüssel werden alle 90 Tage gedreht werden in Azure gespeichert und verwaltet werden die Azure Vorgänge, die nur behördliche und Konformität Methoden verwenden.

Sie Registrieren eines Arbeitsbereichs aus, mit dem Dienst Log Analytics Operations Manager und eine sichere HTTPS-Verbindung zwischen dem Operations Manager Management Server hergestellt wird.

Für Windows-Agents auf Azure-virtuellen Computern ausführen wird ein Speicherschlüssel schreibgeschützt verwendet, zum Lesen von Diagnoseprotokollen Ereignisse in Azure Tabellen.

Wenn alle Agent Kommunikation mit dem Dienst aus irgendeinem Grund nicht möglich ist, werden die gesammelten Daten lokal in einem temporären Cache gespeichert und der Management Server versucht, die Daten für 2 Stunden alle 8 Minuten erneut senden. Zwischengespeicherte Daten des Kundendienstmitarbeiters ist durch das Betriebssystem Anmeldeinformationsspeicher geschützt. Wenn die Daten nach 2 Stunden Dienst verarbeitet werden kann, werden die Agents Daten Warteschlange. Wenn die Warteschlange voll wird, beginnt OMS Datentypen, beginnend mit der Leistungsdaten ablegen. Die Beschränkung der Agent Warteschlange ist bei Bedarf eines Registrierungsschlüssels deaktiviert, sodass Sie bearbeitet werden können. Gesammelte Daten komprimiert und gesendet, um den Dienst, umgehen der lokalen Datenbanken, damit alle laden können nicht hinzugefügt wird. Nachdem die gesammelten Daten gesendet wurde, wird es aus dem Cache entfernt.

Wie zuvor beschrieben, werden Daten aus Ihrem Agents über SSL an Microsoft Azure-Datencentern gesendet. Optional können Sie ExpressRoute verwenden, um zusätzliche Sicherheit zu gewährleisten, für die Daten. ExpressRoute ist eine Möglichkeit, die direkt mit Azure vom vorhandenen WAN-Netzwerk verbunden, wie Multi-Protokoll beschriften Switch (MPLS) VPN, die von einem Netzwerk-Dienstanbieter bereitgestellt. Weitere Informationen finden Sie unter [ExpressRoute](https://azure.microsoft.com/services/expressroute/).


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3 der Log Analytics-Dienst empfängt und verarbeitet Daten

Log Analytics Dienst werden sichergestellt, dass eingehende Daten aus einer vertrauenswürdigen Quelle durch Überprüfen von Zertifikaten und die Datenintegrität mit Azure-Authentifizierung. Nicht verarbeiteten unformatierten Daten werden dann als Blob in [Microsoft Azure Storage](../storage/storage-introduction.md) gespeichert und nicht verschlüsselt ist. Jede Azure-Speicher Blob hat jedoch eine Reihe von eindeutigen Satzes von Tasten, die nur für diesen Benutzer zugegriffen werden kann. Der Typ der Daten, die gespeichert sind, hängt von den vorstehend beschriebenen Lösungen, die importiert wurden, und zum Sammeln von Daten. Klicken Sie dann verarbeitet der Log Analytics-Dienst die unformatierten Daten für den Blog Azure-Speicher.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Log-Analytics Zugriff auf die Daten verwenden

Sie können melden Sie sich bei Log Analytics im Portal OMS mithilfe der Organisations-Konto oder ein Microsoft-Konto, das Sie bereits haben eingerichtet. Alle Verkehr zwischen OMS-Portal und Log Analytics in OMS wird über einen HTTPS-Kanal gesendet. Bei Verwendung des Portals OMS wird auf dem Benutzer-Client (Webbrowser) eine Sitzung ID generiert und Daten in einem lokalen Cache gespeichert ist, bis die Sitzung beendet wird. Wenn beendet, wird der Cache gelöscht. Clientseitige Cookies, die keine persönliche Informationen enthalten, werden nicht automatisch entfernt. Sitzungscookies HTTPOnly gekennzeichnet und gesichert werden. Nach einem zuvor festgelegten Zeitraum im Leerlauf ist die Portalseite OMS-Sitzung beendet.

Über das OMS-Portal an, können Sie Daten in eine CSV-Datei exportieren, und Sie können Daten mithilfe der Suche APIs zugreifen. CSV-Dateien exportieren auf 50.000 Zeilen pro exportieren beschränkt ist, und API Daten auf 5.000 Zeilen pro Suche beschränkt ist.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Protokoll Analytics und Abrufen und Ausführung in Minuten [Erste Schritte mit Log Analytics](log-analytics-get-started.md) .
