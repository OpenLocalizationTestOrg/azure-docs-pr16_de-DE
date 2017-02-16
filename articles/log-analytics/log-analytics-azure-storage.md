<properties
    pageTitle="Sammeln von Daten von Azure-Speicher im Log Analytics Übersicht | Microsoft Azure"
    description="Azure Ressourcen schreiben Protokolle und Kennzahlen mit einer Firma Azure-Speicher häufig mit Azure-Diagnose. Log Analytics können indizieren diese Daten und gestalten durchsucht."
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

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Sammeln von Daten von Azure-Speicher im Log Analytics (Übersicht)

Viele Azure Ressourcen sind in der Lage, Protokolle und Kennzahlen mit einer Firma Azure-Speicher schreiben. Log Analytics können Sie diese Daten nutzen und damit es einfacher zu Azure Ressourcen überwachen.

Zum Schreiben in Azure-Speicher möglicherweise eine Ressource Azure Diagnose oder haben eine eigene Methode zum Schreiben von Daten. Diese Daten können in verschiedenen Formaten auf einen der folgenden Speicherorte geschrieben werden:

+ Azure-Tabelle
+ Azure blob
+ EventHub

Log Analytics unterstützt Azure-Dienste, die Schreiben von Daten mithilfe von [Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Darüber hinaus unterstützt Log Analytics andere Dienste, die Protokolle und Kriterien in verschiedenen Formaten und Speicherorte ausgeben.  

>[AZURE.NOTE] Sie unterliegen normalen Azure-Daten Sätzen für die Speicherung und Transaktionen, wenn Sie mit einem Speicherkonto Diagnose senden und wann Log Analytics die Daten aus Ihrem Speicherkonto liest.

![Diagramm Azure-Speicher](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Unterstützte Azure-Ressourcen

Log Analytics Sammeln von Daten für die folgenden Azure Ressourcen können:

| Ressourcenart | Protokolle (Diagnostic Kategorien) | Log Analytics-Lösung |
| --------------------------------------- | -------------------------------- | --------------- |
| Anwendung Einsichten | Verfügbarkeit <br> Benutzerdefinierte Ereignisse <br> Ausnahmen <br> Besprechungsanfragen <br> | Anwendung Einsichten (Preview) |
| API Management | | *keine* (Preview) |
| Automatisierung <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (Preview) |
| Key Tresor <br> Microsoft.KeyVault/Vaults               | AuditEvent.                       | KeyVault (Preview) |
| Application Gateway <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (Preview) |
| Netzwerk-Sicherheitsgruppe <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (Preview) |
| Dienst Fabric                          | ETWEvent <br> Betrieb Ereignis <br> Zuverlässigen Akteur-Ereignis <br> Zuverlässigen Service-Ereignis| ServiceFabric (Preview) |
| Virtuellen Computern | Linux Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows-ETWEvent | *keine* |
| Webrollen <br> Worker-Rollen | Linux Syslog <br> Windows-Ereignis <br> IIS-Protokoll <br> Windows-ETWEvent | *keine* |

>[AZURE.NOTE] Es wird empfohlen, für die Überwachung Azure-virtuellen Computern (Linux und Windows), [Erweiterung Log Analytics virtueller Computer](log-analytics-azure-vm-extension.md)installieren. Der Agent bietet Ihnen tieferen Einsichten auf Ihre virtuellen Computern als verwenden, wenn Sie die Diagnose zum Speicher geschrieben.

Sie können uns zusätzliche Protokolle für OMS zu analysieren, indem Sie auf unserer [Seite "Feedback"](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy)Abstimmung priorisieren helfen.


- Finden Sie weitere Informationen dazu, wie Log Analytics die Protokolle aus Azure Services lesen können, die [Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)unterstützen [Analysieren Azure Diagnoseprotokolle Log Analytics verwenden](log-analytics-azure-storage-json.md) :
  - Azure Key Tresor
  - Azure Automatisierung
  - Application Gateway
  - Netzwerk-Sicherheitsgruppen
- Finden Sie unter [Verwenden von Blob-Speicher für IIS und Tabellenspeicher für Ereignisse](log-analytics-azure-storage-iis-table.md) zu informieren Sie sich, wie Log Analytics die Protokolle für Azure die Diagnose schreiben Tabellenspeicher oder IIS-Protokolle geschrieben zu BLOB-Speicher, services lesen können einschließlich:
  - Dienst Fabric
  - Webrollen
  - Worker-Rollen
  - Virtuellen Computern


Anwendung Einsichten ist in privaten Seitenansicht und es fortlaufender exportieren verwendet, um BLOB-Speicher. Zum Teilnehmen an der privaten Vorschau, wenden Sie sich an Ihrem Microsoft Account Team oder verweisen Sie, um die Details der [Feedback-Website](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Nächste Schritte

- [Analysieren Azure Diagnoseprotokolle Log Analytics verwenden](log-analytics-azure-storage-json.md) , lesen Sie die Protokolle aus Azure services die Diagnose schreiben zu BLOB-Speicher im JSON-Format.
- [Blob-Speicher für IIS verwenden und Tabellenspeicher für Ereignisse](log-analytics-azure-storage-iis-table.md) , die Protokolle für Azure Lesen services die Diagnose schreiben Tabellenspeicher oder IIS-Protokolle geschrieben zu BLOB-Speicher.
- [Aktivieren von Lösungen](log-analytics-add-solutions.md) einen Einblick in die Daten bereitstellen.
- [Verwenden von Suchabfragen](log-analytics-log-searches.md) zum Analysieren der Daten.
