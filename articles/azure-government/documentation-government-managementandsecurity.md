<properties
    pageTitle="Azure Government Dokumentation | Microsoft Azure"
    description="Dies stellt einen Vergleich der Features und Hinweise zur Entwicklung von Applications für Azure Government"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure Government Verwaltung und Sicherheit

## <a name="automation"></a>Automatisierung

Automatisierung steht in der Regel in Azure Government aus.

### <a name="variations"></a>Variationen

Die folgenden Automatisierungsfeatures sind nicht in Azure Government derzeit verfügbar.

+ Erstellung von Service Prinzip Anmeldeinformationen für die Authentifizierung

Weitere Informationen finden Sie unter [Automatisierung öffentliche Dokumentation](../automation/automation-intro.md).


##  <a name="key-vault"></a>Key Tresor
Weitere Informationen zu diesen Dienst und zur gemeinsamen Nutzung, finden Sie unter der <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure-Taste Tresor öffentliche Dokumentation.</a>
### <a name="data-considerations"></a>Aspekte der Daten
Die folgenden Informationen identifizieren die Begrenzungslinie Azure Government für Azure-Taste Tresor:

| Geregelt/gesteuert Daten erlaubt | Geregelt/gesteuert Daten nicht erlaubt |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle Daten, die mit einer Azure-Taste Tresor Schlüssel verschlüsselt werden, enthalten möglicherweise Daten Regulated/gesteuert. | Azure-Taste Tresor Metadaten darf nicht gesteuert exportieren Daten enthalten. Diese Metadaten umfasst alle Konfigurationsdaten, die beim Erstellen und Verwalten von Ihrem Tresor Schlüssel eingegeben.  Geben Sie in die folgenden Felder nicht Regulated/gesteuert Daten: Gruppe Ressourcennamen, Schlüssel Tresor Namen, Namen des Abonnements. |

Taste Tresor steht in der Regel in Azure Government aus. Wie in der Öffentlichkeit ist ohne Erweiterung, damit die Taste Tresor nur über PowerShell und CLI verfügbar ist.
## <a name="log-analytics"></a>Log Analytics
Log Analytics steht in der Regel in Azure Government aus. 

### <a name="variations"></a>Variationen

Die folgenden Log Analytics-Features und Lösungen sind nicht in Azure Government derzeit verfügbar. Diese Liste wird aktualisiert, wenn der Status des Features / Lösungen ändert.

+ Solutions, in der Vorschau in öffentlichen Azure sind, einschließlich:
  - Überwachung-Lösung
  - Überwachen der Anwendung Abhängigkeit
  - Office 365-Lösung
  - Windows 10 Upgrade Analytics-Lösung
  - Anwendung Einsichten
  - Azure Analytics Networking-Lösung
  - Azure Automatisierung Analytics
  - Key Tresor Analytics
+ Lösungen und Features, die erfordern Updates für lokale Software, einschließlich
  - Integration in System Center Operations Manager 2016 (frühere Versionen von Operations Manager werden unterstützt)
  - Computer Gruppen von System Center-Konfigurations-Manager
  - Surface Hub Lösung
+ Features, die in der Vorschau in öffentlichen Azure sind einschließlich
  - Exportieren von Daten zu PowerBI
+ Azure Metrik und Azure-Diagnose
+ OMS Mobile applications

Die URLs für Protokoll Analytics unterscheiden sich in Azure Government:

| Azure öffentlichen | Azure Government | Notizen |
|--------------|------------------|-------|
| MMS.Microsoft.com | OMS.Microsoft.US | Melden Sie sich Analytics-portal |
| *WorkspaceId*. ods.opinsights.azure.com | *WorkspaceId*. ods.opinsights.azure.us | [Datensammlung API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent-Kommunikation - [Konfigurieren der Firewall-Einstellungen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent-Kommunikation - [Konfigurieren der Firewall-Einstellungen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent-Kommunikation - [Konfigurieren der Firewall-Einstellungen](../log-analytics/log-analytics-proxy-firewall.md) |


Die folgenden Features der Log Analytics weisen unterschiedliche Verhalten in Azure Government:

+ Der Windows-Agent muss vom [Log Analytics-Portal](https://oms.microsoft.us) für Azure Government heruntergeladen werden.
+ Zum Log Analytics Ihr System Center Operations Manager Management-Server herstellen, müssen Sie herunterladen und aktualisierte Management Packs importieren.
  1. Herunterladen Sie und speichern Sie die [Management Packs aktualisiert](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Entzippen Sie die Datei, die Sie heruntergeladen haben
  3. Importieren Sie die Management Packs in Operations Manager. Informationen zum Importieren eines Management Packs von einem Datenträger finden Sie unter dem Thema [So importieren Sie eine Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) auf der Microsoft TechNet-Website.
  4. Operations Manager Log Analytics zum Verbinden mit, folgen Sie den Schritten [Log Analytics Operations Manager verbinden](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

+ Können Daten aus Log Analytics in öffentlichen Azure zu Azure Government werden migriert?
  - Nein. Es ist nicht möglich, Daten oder Ihren Arbeitsbereich aus öffentlichen Azure zu Azure Government zu verschieben.
+ Kann ich zwischen öffentlichen Azure und Azure Government Arbeitsbereiche vom OMS Log Analytics-Portal wechseln?
  - Nein. Die communityportalen für Öffentliche Azure und Azure Government unterscheiden und Informationen nicht freigeben. 

Weitere Informationen finden Sie unter [Log Analytics öffentliche Dokumentation](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Abonnieren Sie zusätzliche Informationen und Updates, die <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government Blog.</a>
 
