<properties
    pageTitle="Azure Government Dokumentation | Microsoft Azure"
    description="Dies stellt einen Vergleich der Features und Anleitungen zur Entwicklung von Applications für Azure Government."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure Government die Überwachung und Verwaltung

In diesem Artikel werden die Überwachung und Management services Variationen und Aspekte für die Government Azure-Umgebung.

## <a name="automation"></a>Automatisierung

Automatisierung steht in der Regel in Azure Government aus.

### <a name="variations"></a>Variationen

Die folgenden Automatisierungsfeatures sind nicht in Azure Government derzeit verfügbar.

+ Erstellung von Service Prinzip Anmeldeinformationen für die Authentifizierung

Weitere Informationen finden Sie unter [Automatisierung öffentliche Dokumentation](../automation/automation-intro.md).

## <a name="log-analytics"></a>Log Analytics

Log Analytics steht in der Regel in Azure Government aus.

### <a name="variations"></a>Variationen

Die folgenden Log Analytics-Features und Lösungen sind nicht in Azure Government derzeit verfügbar.

+ Solutions, in der Vorschau in Microsoft Azure sind, einschließlich:
  - Überwachung-Lösung
  - Anwendung Abhängigkeit Überwachung Lösung
  - Office 365-Lösung
  - Windows 10 Upgrade Analytics-Lösung
  - Anwendung Einsichten Lösung
  - Azure Analytics Networking-Lösung
  - Azure Automatisierung Analytics-Lösung
  - Taste Tresor Analytics-Lösung
+ Lösungen und Features, die erfordern Updates für lokale Software, einschließlich:
  - Integration in System Center Operations Manager 2016 (frühere Versionen von Operations Manager werden unterstützt)
  - Computer Gruppen von System Center-Konfigurations-Manager
  - Surface Hub Lösung
+ Features, die in der Vorschau in öffentlichen Azure, einschließlich:
  - Exportieren von Daten mit Power BI
+ Azure Metrik und Azure-Diagnose
+ Mobile Vorgänge Management Suite-Anwendung

Die URLs für Protokoll Analytics unterscheiden sich in Azure Government:

| Azure öffentlichen | Azure Government | Notizen |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.US | Melden Sie sich Analytics-portal |
| *WorkspaceId*. ods.opinsights.azure.com | *WorkspaceId*. ods.opinsights.azure.us | [Datensammlung API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent-Kommunikation - [Konfigurieren der Firewall-Einstellungen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent-Kommunikation - [Konfigurieren der Firewall-Einstellungen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent-Kommunikation - [Konfigurieren der Firewall-Einstellungen](../log-analytics/log-analytics-proxy-firewall.md) |


Die folgenden Features der Log Analytics abweichendem Verhalten in Azure Government:

+ Der Windows-Agent muss vom [Log Analytics-Portal](https://oms.microsoft.us) für Azure Government heruntergeladen werden.
+ Zum Log Analytics Ihr System Center Operations Manager Management-Server herstellen, müssen Sie herunterladen und aktualisierte Management Packs importieren.
  1. Herunterladen Sie und speichern Sie die [Management Packs aktualisiert](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Entzippen Sie die Datei, die Sie heruntergeladen haben.
  3. Importieren Sie die Management Packs in Operations Manager. Informationen zum Importieren ein Management Packs von einem Datenträger, finden Sie unter [So importieren Sie eine Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) auf der Microsoft TechNet-Website.
  4. Operations Manager Log Analytics zum Verbinden mit, folgen Sie den Schritten [Log Analytics Operations Manager verbinden](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

+ Kann ich Daten von Log Analytics in Microsoft Azure zur Azure Government migrieren?
  - Nein. Es ist nicht möglich, die Daten oder Ihren Arbeitsbereich von Microsoft Azure Azure Government wechseln.
+ Kann ich zwischen Microsoft Azure und Azure Regierung Arbeitsbereiche vom Vorgänge Management Suite Log Analytics-Portal wechseln?
  - Nein. Die communityportalen für Microsoft Azure und Azure Government unterscheiden und Informationen nicht freigeben.

Weitere Informationen finden Sie unter [Log Analytics öffentliche Dokumentation](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Abonnieren Sie zusätzliche Informationen und Updates, die <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government Blog.</a>
