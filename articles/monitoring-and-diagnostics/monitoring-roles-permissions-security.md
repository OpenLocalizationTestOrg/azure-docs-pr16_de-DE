<properties
    pageTitle="Erste Schritte mit Rollen, Berechtigungen und Sicherheit mit Azure Monitor | Microsoft Azure"
    description="Erfahren Sie, wie mit Azure Monitors integrierte Rollen und Berechtigungen zum Einschränken des Zugriffs auf Ressourcen überwachen."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Erste Schritte mit Rollen, Berechtigungen und Sicherheit mit Azure Monitor

Viele Teams müssen grundsätzlich Zugriff auf Daten und Einstellungen für die Überwachung zu Regeln. Angenommen, oder, wenn Sie Teammitglieder arbeiten ausschließlich auf Überwachung (Supportmitarbeiter, Devops Engineers) Wenn Sie einen verwalteten Dienstanbieter verwenden, Sie möglicherweise ihnen Zugriff auf die Überwachung nur während beschränken die Möglichkeit zum Erstellen von Daten gewähren möchten ändern oder Löschen von Ressourcen. In diesem Artikel wird gezeigt, wie schnell ein Benutzer in Azure eine integrierte Überwachung RBAC-Rolle zuweisen oder erstellen Ihre eigenen benutzerdefinierten Rolle für einen Benutzer, der Berechtigungen für eingeschränkte Überwachung benötigt wird. Außerdem wird dann Sicherheitsaspekte Ihrer Azure Monitor-bezogene Ressourcen und wie Sie Zugriff auf die Daten beschränken können darin enthaltenen erläutert.

## <a name="built-in-monitoring-roles"></a>Integrierte Überwachung Rollen

Standardrollen Azure Monitor soll helfen, Einschränken des Zugriffs auf Ressourcen in einem Abonnement dennoch Verantwortlichen für die Überwachung Infrastruktur zum Abrufen und konfigurieren die benötigten Daten. Azure Monitor bietet zwei Out-of-Box-Rollen: A Überwachung Reader und einen Mitwirkenden überwachen.

### <a name="monitoring-reader"></a>Überwachung Reader

Personen, die die Rolle Leser Überwachung zugewiesen können Änderungen an den Einstellungen für die Überwachung von Ressourcen anzeigen alle Überwachung Daten in einem Abonnement aber kann keine Ressourcen ändern oder bearbeiten. Diese Rolle eignet sich für Benutzer in einer Organisation, z. B. Support oder Vorgänge Ingenieure, die entsprechenden werden sollen:

- Überwachung Dashboards im Portal anzeigen und eigene private Überwachung Dashboards erstellen.
- Abfrage mit [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-Cmdlets](insights-powershell-samples.md)oder [Plattformen CLI](insights-cli-samples.md)Kennzahlen.
- Abfrage der Aktivität Log mithilfe der Portal, Azure Monitor REST-API, PowerShell-Cmdlets oder Plattformen CLI an.
- Anzeigen der [diagnoseeinstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) für eine Ressource an.
- Anzeigen der [Log-Profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) für ein Abonnement.
- Automatisch skalieren Einstellungen anzeigen
- Benachrichtigen Aktivität anzeigen und Einstellungen.
- Zugriff auf Daten Anwendung Einsichten und Anzeigen von Daten in AI Analytics.
- Log Analytics (OMS) Arbeitsbereichsdaten, einschließlich der von Verwendungsdaten für den Arbeitsbereich zu suchen.
- Log Analytics (OMS) Management Gruppen anzeigen
- Abrufen der Suchschema Log Analytics (OMS).
- Liste Log Analytics (OMS) Intelligence Packs.
- Abrufen und Log Analytics (OMS) gespeicherte Suchen ausführen.
- Die Log Analytics (OMS) Speicherkonfiguration abrufen.

> [AZURE.NOTE] Diese Rolle hat nicht gelesen Log Daten Zugriff, die an ein Ereignis Verteiler gestreamt oder in einem Speicherkonto gespeichert wurde. Sie [finden Sie unter](#security-considerations-for-monitoring-data) Informationen zum Konfigurieren des Zugriffs auf diese Ressourcen.

### <a name="monitoring-contributor"></a>Überwachen von Mitwirkenden

Personen, die für die Überwachung Teilnehmerrolle zugewiesen können alle Überwachung Daten in einem Abonnement anzeigen und erstellen oder Ändern der Einstellungen für die Überwachung, aber keine anderen Ressourcen ändern. Diese Rolle ist eine Teilmenge der Rolle Leser für die Überwachung und eignet sich für Mitglieder eines Unternehmens Überwachung Team oder verwalteten Dienstanbieter, die auch zusätzlich zu den oben genannten Berechtigungen benötigen können:

- Veröffentlichen Sie Überwachung Dashboards als freigegebenen Dashboard.
- Festlegen von [diagnoseeinstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) für eine resource.*
- Festlegen der [Log-Profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) für eine subscription.*
- Legen Sie benachrichtigen Aktivität und Einstellungen.
- Erstellen von Anwendung Einsichten Web überprüft und Komponenten.
- Liste Log Analytics (OMS) Arbeitsbereich gemeinsamer Schlüssel.
- Aktivieren Sie oder deaktivieren Sie die Intelligence Packs Log Analytics (OMS).
- Erstellen und löschen und Log Analytics (OMS) gespeicherte Suchen ausführen.
- Erstellen Sie und löschen Sie den Log Analytics (OMS) Speicher zu konfigurieren.

* Benutzer muss auch separat ListKeys-Berechtigung für die Zielressource (Speicher-Konto oder das Ereignis Hub Namespace) eine Log-Profil oder einer Diagnoseprotokollen Einstellung festlegen erteilt werden.

> [AZURE.NOTE] Diese Rolle hat nicht gelesen Log Daten Zugriff, die an ein Ereignis Verteiler gestreamt oder in einem Speicherkonto gespeichert wurde. Sie [finden Sie unter](#security-considerations-for-monitoring-data) Informationen zum Konfigurieren des Zugriffs auf diese Ressourcen.

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Berechtigungen und benutzerdefinierte RBAC-Rollen für die Überwachung
Wenn die oben angegebenen integrierten Rollen die genauen Anforderungen Ihres Teams nicht entsprechen, können Sie mit spezifischere Berechtigungen [erstellen eine benutzerdefinierte RBAC-Rolle](../active-directory/role-based-access-control-custom-roles.md) . Nachstehend sind die allgemeinen Azure Monitor RBAC Vorgänge mit deren Beschreibung ein.

| Vorgang                                                   | Beschreibung                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, schreiben, löschen]         | Lesen/Schreiben/Löschen Warnungsregeln.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Liste der für Warnungsregeln Fälle (Verlauf der Benachrichtigung Regel wird ausgelöst wurde). Dies gilt nur für das Portal.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read, schreiben, löschen]  | Lesen/Schreiben/Löschen automatisch skalieren-Einstellungen.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read, schreiben, löschen] | Lesen/Schreiben/Löschen diagnoseeinstellungen.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Diese Berechtigung muss für Benutzer, die Zugriff auf Aktivitätsprotokolle über das Portal benötigen.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Liste Aktivität protokollieren von Ereignissen (Ereignisse Management) in einem Abonnement. Diese Berechtigung gilt für den programmgesteuerten und Portal Zugriff auf das Protokoll Aktivität. |
| Microsoft.Insights/LogDefinitions/Read                      | Diese Berechtigung muss für Benutzer, die Zugriff auf Aktivitätsprotokolle über das Portal benötigen.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Lesen Sie metrische Definitionen (Liste der verfügbaren metrischen Typen für eine Ressource) ein.                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Weitere Kriterien für eine Ressource an.                                                                                                                              |

> [AZURE.NOTE] Der Zugriff auf Benachrichtigungen, diagnoseeinstellungen und Kennzahlen für eine Ressource erfordert, dass der Benutzer Lesezugriff auf die Ressourcenart und Umfang dieser Ressource verfügt. Ein diagnostic Einstellung oder Log-Profil, das in einem Speicherkonto oder Streams an Ereignis Hubs archiviert ist die Benutzer auch ListKeys-Berechtigung für die Zielressource ("Speichern") zu erstellen.

Beispielsweise mithilfe der obigen Tabelle, der Sie eine benutzerdefinierte RBAC-Rolle erstellen können, für eine "Aktivität Log Reader" wie folgt aus:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Sicherheitsaspekte zur Überwachung von Daten
Überwachen von Daten – besonders Protokolldateien – können sensible Informationen, wie z. B. IP-Adressen oder Benutzernamen enthalten. Überwachen von Daten aus Azure ist in drei grundlegende Formen:
1. Das Protokoll Aktivitäten, das alle Aktionen der Steuerelement-Ebene für Ihr Abonnement Azure beschreibt.
2. Diagnoseprotokolle, die Protokolle ausgegeben, die eine Ressource sind.
3. Kennzahlen, die von Ressourcen ausgegeben werden.

Alle drei dieser Datentypen können in einem Konto Storage gespeichert oder auf Ereignis Hub beide allgemeine Azure Ressourcen sind gestreamt werden. Da diese allgemeine Ressourcen sind, ist erstellen, löschen und den Zugriff auf diese einen berechtigten Vorgang normalerweise nur für einen Administrator. Es wird empfohlen, dass Sie die folgenden Methoden für die Überwachung bezogene Ressourcen verwenden, um Missbrauch vorzubeugen:

- Verwenden eines einzelnen, dedizierten Speicher-Kontos zur Überwachung von Daten an. Wenn Sie die Überwachung Daten in mehrere Speicherkonten trennen müssen, dürfen nie freigeben Verwendung eines Kontos Speicher zwischen Überwachung und nicht Überwachung Daten wie dieser versehentlich körperlich benötigen Sie nur Zugriff auf die Überwachung von Daten (z. b. eine Drittanbieter-SIEM) Zugriff auf nicht Überwachung Daten.
- Verwenden Sie einen einzelnen dedizierten Ereignis Hub oder Dienst genutzten Namespace über alle diagnoseeinstellungen Gründen wie oben.
- Beschränken Sie Zugriff auf Überwachung-bezogene Speicherkonten oder Ereignis Hubs werden, da diese in einer separaten Ressourcengruppe und [Bereich verwenden](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) , Ihre Überwachung Rollen Zugriff auf nur diese Ressourcengruppe beschränkt.
- Erteilen Sie nie ListKeys-Berechtigung für Speicherkonten oder Ereignis Hubs am Umfang Abonnements auf, wenn ein Benutzer nur Zugriff auf Daten für die Überwachung benötigt. Geben Sie diese Berechtigungen stattdessen für dem Benutzer bei einer Ressource oder Ressourcengruppe (Wenn Sie eine dedizierte Überwachung Ressourcengruppe sind) Bereich.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Einschränken des Zugriffs auf Überwachung-bezogene Speicherkonten
Wenn ein Benutzer oder eine Anwendung Zugriff auf Daten in einem Speicherkonto für die Überwachung benötigt, sollten Sie [ein Konto SAS generieren](https://msdn.microsoft.com/library/azure/mt584140.aspx) , auf dem Speicherkonto, das Überwachung Daten mit Servicelevel schreibgeschützten Zugriff auf Blob-Speicher enthält. In PowerShell kann dies wie aussehen:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Sie können dem Token klicken Sie dann auf die Entität verleihen, muss Lesen aus dieser Speicher-Konto, und es Liste und lesen können aus allen Blobs in diesem Storage-Konto.

Alternativ, wenn Sie diese Berechtigung mit RBAC steuern müssen, können Sie diese Person die Berechtigung Microsoft.Storage/storageAccounts/listkeys/action auf diesem bestimmten Storage-Konto gewähren. Dies ist für Benutzer, die müssen Sie möglicherweise Festlegen einer Diagnoseprotokollen Einstellung, oder melden Sie sich zu archivieren Profil einer Speicherkonto erforderlich. Beispielsweise konnten Sie die folgende benutzerdefinierte RBAC-Rolle für einen Benutzer oder eine Anwendung, die nur zum Lesen von einem Speicherkonto muss erstellen:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] Die ListKeys-Berechtigung ermöglicht den Benutzer, die primären und sekundären Speicher Konto Schlüssel aufgelistet. Diese Schlüssel erteilen Sie dem Benutzer alle Berechtigungen angemeldet (Lesen, schreiben, Blobs erstellen, löschen Sie Blobs usw.) auf allen angemeldet Services (Blob, Warteschlange, Table, Datei) in diesem Storage-Konto. Es empfiehlt sich mit einem Konto SAS oben beschriebenen, falls möglich.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Einschränken des Zugriffs auf Überwachung-Ereignistyp hubs
Ein ähnliches Muster mit Ereignis Hubs folgen kann, aber müssen Sie zunächst eine dedizierte Abhören Autorisierungsregel erstellen. Wenn Sie Zugriff auf eine Anwendung gewähren, die nur für die Überwachung-Ereignistyp Hubs anhören möchten, führen Sie folgende Schritte aus:

1. Erstellen einer freigegebenen Zugriffsrichtlinie auf das Ereignis Hub(s), die für das streaming Überwachung Daten mit nur Listen Ansprüche erstellt wurden. Dies kann im Portal erfolgen. Beispielsweise könnten Sie es "MonitoringReadOnly." aufrufen Falls möglich, sollten Sie geben Sie die Taste direkt an dem Consumer und fahren Sie im nächsten Schritt fort.
2. Wenn der Verbraucher Key Ad-hoc-abrufen können muss, erteilen Sie dem Benutzer die ListKeys-Aktion für diesen Hub Ereignis. Dies ist auch für Benutzer, die möglicherweise eine Diagnoseprotokollen Einstellung festlegen, oder melden Sie sich Profil Stream an Ereignis Hubs müssen erforderlich. Beispielsweise können Sie eine Regel RBAC erstellen:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über RBAC und Berechtigungen in Ressourcenmanager](../active-directory/role-based-access-control-what-is.md)
- [Lesen Sie den Überblick über die Überwachung in Azure](monitoring-overview.md)
