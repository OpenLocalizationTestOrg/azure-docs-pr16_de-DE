<properties
    pageTitle="Konfiguration Bewertung-Lösung in Log Analytics | Microsoft Azure"
    description="Die Konfiguration Bewertung-Lösung in Log Analytics bietet Ihnen ausführliche Informationen zu den aktuellen Status der System Center Operations Manager Server-Infrastruktur Operations Manager-Agents oder einer Management Group unter Operations Manager zu verwenden."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="configuration-assessment-solution-in-log-analytics"></a>Konfiguration Bewertung-Lösung in Log Analytics

Die Konfiguration Bewertung-Lösung in Log Analytics hilft Ihnen potenzieller Server Konfigurationsprobleme bis Benachrichtigungen und Knowledge Empfehlungen finden.

Diese Lösung erfordert System Center Operations Manager. Konfiguration Bewertung nicht verfügbar, wenn Sie nur direkt angeschlossenes Agents verwenden.

Einige Informationen in Konfiguration Bewertung Lösung anzeigen erfordert die Silverlight-Plug-In für Ihren Browser.

>[AZURE.NOTE] Anfang Juli 5, 2016, die Lösung Konfiguration Bewertung nicht mehr Log Analytics-Arbeitsbereichen hinzugefügt werden kann und diese Lösung werden nicht mehr vorhandenen Benutzer bereitgestellt nach 1 August 2016. Für Kunden mit dieser Lösung für SQL Server oder Active Directory empfehlen wir, dass Sie stattdessen verwenden, die [SQL Server-Bewertung](log-analytics-sql-assessment.md), [Active Directory-Bewertung](log-analytics-ad-assessment.md) und [Active Directory-Replikationsstatus](log-analytics-ad-replication-status.md) Lösungen. Für Kunden mit Bewertung Konfiguration für Windows Hyper-V und System Center virtuellen Computern Manager, sollten Sie das Ereignis von Websitesammlungen und änderungsnachverfolgung-Funktionen, um ein vollständiger Überblick Probleme in Ihrer Umgebung zu erhalten.

![Konfiguration Bewertung Kachel](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Von Konfigurationsdaten von überwachten Servern erfasst und dann an die OMS-Dienst in der Cloud für die Verarbeitung gesendet werden. Logik wird angewendet, um die empfangenen Daten und der Cloud-Dienst Einträge die Daten. Verarbeitete Daten für die Server ist für die folgenden Bereiche dargestellt:

- **Benachrichtigungen:** Zeigt die Konfiguration relevant, proaktiven Benachrichtigungen, die für Ihre überwachten Server ausgelöst worden sein. Diese werden von Regeln, die von Microsoft Customer und Support-Organisation (CSS) mit bewährte Methoden aus dem Feld verfasst hergestellt.
- **Knowledge Empfehlungen:** Zeigt die Microsoft Knowledge Base-Artikeln, die für Auslastung vorgeschlagen werden, die in Ihrer Infrastruktur gefunden werden; Diese werden automatisch vorgeschlagen, basierend auf Ihrer Konfiguration über die Verwendung der Computer Schulung.
- **Servers und Auslastung analysiert:** Zeigt die Server und Auslastung, die vom OMS überwacht werden.

![Konfiguration Bewertung dashboard](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Technologien können Sie mit der Konfiguration Bewertung analysieren.

OMS Konfiguration Bewertung analysiert die folgenden Auslastung:

- WindowsServer 2012 und Microsoft Hyper-V Server 2012
- Windows Server 2008 und Windows Server 2008 R2, einschließlich:
    - Active Directory
    - Hyper-V-host
    - Allgemeine Betriebssystem
- SQLServer 2008 und höher
    - SQL Server-Datenbank-Engine
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 und Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 und Lync Server 2010
- System Center 2012 SP1 – Manager virtuellen Computern

Für SQL Server werden die folgenden 32-Bit- und 64-Bit-Editionen für die Analyse unterstützt:

- SQLServer 2016 - alle Editionen
- SQLServer 2014 - alle Editionen
- SQLServer 2008 und 2008 R2 - alle Editionen

Die SQL Server-Datenbank-Engine wird auf allen unterstützten Editionen analysiert. Darüber hinaus wird die 32-Bit-Version von SQL Server unterstützt, wenn die Implementierung WOW64 ausgeführt.

## <a name="installing-and-configuring-the-solution"></a>Installieren und konfigurieren die Lösung
Verwenden Sie die folgende Informationen zum Installieren und konfigurieren die Lösung.

- Operations Manager ist für die Bewertung der Konfiguration Lösung erforderlich.
- Sie müssen Agent Operations Manager auf jedem Computer verfügen, für die Konfiguration bewerten möchten.
- Fügen Sie die Konfiguration Bewertung-Lösung in Ihren OMS Arbeitsbereich mithilfe des Prozesses [Hinzufügen Log Analytics Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschrieben.  Es ist keine weitere Konfiguration erforderlich.

## <a name="configuration-assessment-data-collection-details"></a>Konfiguration Bewertung Einzelheiten zur Datensammlung

Konfiguration Bewertung sammelt Konfigurationsdaten, Metadaten und Bundesstaat-Daten, die über die Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt Datensammlungsmethoden und andere Details, wie Daten für die Bewertung der Konfiguration erfasst werden.

| Plattform | Direkte Agent | SCOM agent | Azure-Speicher | SCOM erforderlich? | SCOM Agentdaten per Management Group unter gesendeten | Häufigkeit Collection |
|---|---|---|---|---|---|---|
|Windows|![Nein](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Nein](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| zweimal pro Tag|

Die folgende Tabelle zeigt Beispiele für Datentypen, die von der Konfiguration Bewertung gesammelt:

|**Datentyp**|**(Felder)**|
|---|---|
|Konfiguration|"CustomerID", AgentID, %EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate|
|Metadaten|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, Netzwerkname, IP-Adresse, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-Adresse, NetbiosDomainName, LogicalProcessors, DNS-Name, DisplayName, DomainDnsName, ActiveDirectorySite, ' PrincipalName ', OffsetInMinuteFromGreenwichTime|
|Bundesstaat|StateChangeEventId, State-ID, NewHealthState, OldHealthState, Kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Konfiguration Bewertung Benachrichtigungen
Sie können anzeigen und Verwalten von Benachrichtigungen über die Konfiguration Bewertung mit der Seite Benachrichtigungen. Benachrichtigungen informieren, wenn Sie das Problem, das erkannt wurde, die Ursache und wie Sie das Problem zu beheben. Sie bieten auch Informationen zur Konfiguration von Einstellungen in Ihrer Umgebung, die Leistungsprobleme verursachen können.

![Anzeigen von Benachrichtigungen](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Die Konfiguration Bewertung Benachrichtigungen unterscheiden sich von den anderen Warnungen im Log Analytics. Anzeigen von Benachrichtigungen erfordert eine Silverlight-Plug-In für Ihren Browser.

Bei der Auswahl eines Elements oder einer Kategorie von Elementen auf der Seite Benachrichtigungen sehen Sie eine Liste von Servern oder Auslastung mit Benachrichtigungen, die für jedes Element gelten.

![Benachrichtigen Liste](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Jede Warnung enthält einen Link zu einem Artikel in der Microsoft Knowledge Base. Die folgenden Artikel bieten zusätzliche Informationen zu dieser Fehlermeldung.

>[AZURE.TIP] Standardmäßig ist die maximale Anzahl der angezeigten Warnungen 2.000. Um weitere Benachrichtigungen anzeigen möchten, klicken Sie auf der Benachrichtigungsleiste oberhalb der Liste der Benachrichtigungen.

Klicken Sie auf ein beliebiges Element in der Liste, um die KB-Artikel anzeigen, mit der Hilfe Sie die Ursache des Problems zu beheben, die die Benachrichtigung generiert.

![KB-Artikel](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Sie können die Warnungsregeln zum Ignorieren von bestimmter Regeln oder eine Kategorien von Regeln verwalten.

![Verwalten von Warnungsregeln](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Knowledge Empfehlungen
Wenn Sie Knowledge Empfehlungen anzeigen, sind Sie Log Suchergebnisse Auflisten von Microsoft KB-Artikel für die Auslastung und Computern, die zusätzliche Informationen über die Benachrichtigung bereitstellen empfohlen dargestellt.

![Suchergebnisse für Knowledge Empfehlungen](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Servers und Auslastung analysiert
Wenn Sie Knowledge Empfehlungen anzeigen, sind Sie Log Suchergebnisse, in die Servers und Auslastung, bei die OMS von Operations Manager bekannt dargestellt.

![Servers und Auslastung](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log Analytics Log durchsucht](log-analytics-log-searches.md) , um detaillierte Konfiguration Bewertungsdaten anzuzeigen.
