<properties
    pageTitle="RBAC: Standardrollen | Microsoft Azure"
    description="In diesem Thema werden die integrierten in Rollen für die Steuerung des Benutzerzugriffs rollenbasierte (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Standardrollen

Azure rollenbasierte Access Steuerelement (RBAC) verfügt über die folgenden integrierten Rollen, die Benutzer, Gruppen und Dienste zugeordnet werden können. Sie können die Definitionen der integrierten Rollen nicht ändern. Allerdings können Sie [benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md) , um den Bedürfnissen Ihrer Organisation erstellen.

## <a name="roles-in-azure"></a>Rollen in Azure

Die folgende Tabelle enthält die integrierten Rollen kurzen Beschreibung an. Klicken Sie auf den Rollennamen die ausführliche Liste von **Aktionen** und **Notactions** für die Rolle angezeigt. Die Eigenschaft **Aktionen** gibt die zulässigen Aktionen auf Azure Ressourcen. Aktion Zeichenfolgen können Platzhalterzeichen verwenden. Die Eigenschaft " **Notactions** " gibt an, die Aktionen, die die zulässigen Aktionen ausgeschlossen werden.

>[AZURE.NOTE] Die Definitionen Azure-Rolle werden beständig Weiterentwicklung. In diesem Artikel wird als Stand wie möglich beibehalten, jedoch können Sie die neuesten Rollen Definitionen immer im Azure PowerShell finden. Verwenden Sie die Cmdlets `(get-azurermroledefinition "<role name>").actions` oder `(get-azurermroledefinition "<role name>").notactions` nach Bedarf.

| Name der Rolle | Beschreibung |
| --------- | ----------- |
| [API Management Service Mitwirkender](#api-management-service-contributor) | Verwalten von API Management Services können |
| [Anwendung Einsichten Komponente Mitwirkender](#application-insights-component-contributor) | Verwalten der Anwendung Einsichten Komponenten können |
| [Automatisierung Operator](#automation-operator) | Starten, beenden, Anhalten und fortsetzen Aufträge können |
| [BizTalk Mitwirkender](#biztalk-contributor) | Können BizTalk-Dienste verwalten |
| [ClearDB MySQL DB Mitwirkender](#cleardb-mysql-db-contributor) | Können ClearDB MySQL-Datenbanken verwalten |
| [Mitwirkenden](#contributor) | Können alles außer Zugriff verwalten. |
| [Factory Mitwirkenden für Daten](#data-factory-contributor) | Können erstellen und Verwalten von Daten Factory und untergeordneten Ressourcen darin enthaltenen. |
| [DevTest Labs Benutzer](#devtest-labs-user) | Alles anzeigen können, und verbinden, starten, neu und war(en) virtuelle Computern |
| [DNS-Zone Mitwirkender](#dns-zone-contributor) | Können DNS-Zonen und Einträge verwalten |
| [Mitwirkender DocumentDB-Konto](#documentdb-account-contributor) | Können DocumentDB Konten verwalten |
| [Intelligente Systeme Konto Mitwirkender](#intelligent-systems-account-contributor) | Intelligente Systeme Konten verwalten kann |
| [Netzwerk Mitwirkender](#network-contributor) | Kann alle Netzwerk-Ressourcen verwalten |
| [Neue Relic APM Konto Mitwirkender](#new-relic-apm-account-contributor) | Neue Relic Leistung Anwendungsverwaltung Konten und Applikationen können verwaltet werden. |
| [Besitzer](#owner) | Alles, einschließlich des Zugriffs verwalten können |
| [Reader](#reader) | Kann alle Elemente anzeigen, jedoch können keine Änderungen vornehmen |
| [Redis Cache Mitwirkender](#redis-cache-contributor) | Verwalten des Caches Redis können |
| [Scheduler Job Websitesammlungen Mitwirkender](#scheduler-job-collections-contributor) | Können Scheduler Job Websitesammlungen verwalten |
| [Suche Dienst Mitwirkender](#search-service-contributor) | Verwalten der Suchdienste können |
| [Sicherheits-Manager](#security-manager) | Können Sicherheitskomponenten, Sicherheitsrichtlinien und virtuellen Computern verwalten. |
| [SQL-DB Mitwirkender](#sql-db-contributor) | Können SQL-Datenbanken, aber nicht deren Sicherheits-Richtlinien verwalten. |
| [SQL-Sicherheits-Manager](#sql-security-manager) | Können die Sicherheits-Richtlinien von SQL Server und Datenbanken verwalten |
| [SQL Server-Teilnehmer](#sql-server-contributor) | Können SQL Server und Datenbanken, aber nicht deren Sicherheits-Richtlinien verwalten. |
| [Klassische Speicher Konto Mitwirkender](#classic-storage-account-contributor) | Klassische Speicherkonten verwalten kann |
| [Mitwirkender für Speicher-Konto](#storage-account-contributor) | Verwalten von Speicherkonten können |
| [Access-Administrator für Benutzer](#user-access-administrator) | Verwalten des Benutzerzugriffs auf Azure Ressourcen können |
| [Mitwirkender klassischen virtuellen Computern](#classic-virtual-machine-contributor) | Verwalten können, klassische virtuellen Computern, aber nicht das virtuelle Netzwerk oder Speicher-Konto, das sie verbunden sind |
| [Mitwirkender virtuellen Computern](#virtual-machine-contributor) | Verwalten von können virtuellen Computern, aber nicht das virtuelle Netzwerk oder Speicher-Konto, das sie verbunden sind |
| [Klassische Netzwerk Mitwirkender](#classic-network-contributor) | Können klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten |
| [Web Plan Mitwirkender](#web-plan-contributor) | Verwalten von Web-Pläne können |
| [Website Mitwirkender](#website-contributor) | Verwalten können, Websites, aber nicht die Web-Pläne, die mit denen sie verbunden sind |

## <a name="role-permissions"></a>Rollenberechtigungen
Die folgenden Tabellen beschreiben die spezifischen Berechtigungen für jede Rolle ergeben. Hierzu gehören **Aktionen**, die die Berechtigung erteilen, und **NotActions**, die sie einschränken.

### <a name="api-management-service-contributor"></a>API Management Service Mitwirkender
Verwalten von API Management Services können

| **Aktionen** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Erstellen und Verwalten von API Management Services |
| Microsoft.Authorization/*/read | Autorisierung lesen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Finden Sie hier Rollen und rollenzuweisungen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="application-insights-component-contributor"></a>Anwendung Einsichten Komponente Mitwirkender
Verwalten der Anwendung Einsichten Komponenten können

| **Aktionen** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Einsichten Komponenten |
| Microsoft.Insights/webtests/* | Erstellen und Verwalten von Webtests |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="automation-operator"></a>Automatisierung Operator
Starten, beenden, Anhalten und fortsetzen Aufträge können

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und rollenzuweisungen |
| Microsoft.Automation/automationAccounts/jobs/read | Lesen Sie Automatisierung Konto Aufträge |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Wiederaufnehmen einer Automatisierung Konto Position |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Beenden einer Automatisierung Konto Position |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Lesen Sie Automatisierung Konto Auftrag streams |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Einen Automatisierung Konto Auftrag anhalten |
| Microsoft.Automation/automationAccounts/jobs/write | Schreiben Sie Automatisierung Konto Aufträge |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Lesen einer Zeitplan Automatisierung Konto Aufträge |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Lesen einer Zeitplan Automatisierung Konto Aufträge |
| Microsoft.Automation/automationAccounts/read | Lesen Sie Automatisierung Konten |
| Microsoft.Automation/automationAccounts/runbooks/read | Automatisierung Runbooks lesen |
| Microsoft.Automation/automationAccounts/schedules/read | Lesen Sie Automatisierung Konto Zeitpläne |
| Microsoft.Automation/automationAccounts/schedules/write | Schreiben Sie Automatisierung Konto Zeitpläne |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Einsichten Komponenten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="biztalk-contributor"></a>BizTalk Mitwirkender
Können BizTalk-Dienste verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und rollenzuweisungen |
| Microsoft.BizTalkServices/BizTalk/* | Erstellen und Verwalten von BizTalk-Dienste |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Mitwirkender
Können ClearDB MySQL-Datenbanken verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |
| successbricks.cleardb/Databases/* | Erstellen und Verwalten von ClearDB MySQL-Datenbanken |

### <a name="contributor"></a>Mitwirkenden
Alles außer Zugriff verwalten können

| **Aktionen** ||
| ------- | ------ |
| * | Erstellen und Verwalten von Ressourcen aller Typen |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Rollen und rollenzuweisungen kann nicht gelöscht werden. |  
| Microsoft.Authorization/*/Write | Rollen und rollenzuweisungen kann nicht erstellt werden. |

### <a name="data-factory-contributor"></a>Factory Mitwirkenden für Daten
Erstellen und Verwalten von Daten Factory und untergeordneten Ressourcen darin enthaltenen.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.DataFactory/dataFactories/* | Erstellen und Verwalten von Daten Factory und untergeordneten Ressourcen darin enthaltenen. |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="devtest-labs-user"></a>DevTest Labs Benutzer
Alles anzeigen können, und verbinden, starten, neu und war(en) virtuelle Computern

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Compute/availabilitySets/read | Lesen Sie die Eigenschaften Mengen Verfügbarkeit |
| Microsoft.Compute/virtualMachines/*/read | Lesen Sie die Eigenschaften eines virtuellen Computers (virtueller Computer Größen, Runtime Status, virtueller Computer Erweiterungen usw.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Freigeben von virtuellen Computern |
| Microsoft.Compute/virtualMachines/read | Lesen Sie die Eigenschaften eines virtuellen Computers |
| Microsoft.Compute/virtualMachines/restart/action | Starten von virtuellen Computern |
| Microsoft.Compute/virtualMachines/start/action | Starten von virtuellen Computern |
| Microsoft.DevTestLab/*/read | Lesen Sie die Eigenschaften einer Übung |
| Microsoft.DevTestLab/labs/createEnvironment/action | Erstellen einer Übung-Umgebung |
| Microsoft.DevTestLab/labs/formulas/delete | Löschen von Formeln |
| Microsoft.DevTestLab/labs/formulas/read | Lesen von Formeln |
| Microsoft.DevTestLab/labs/formulas/write | Hinzufügen oder Ändern von Formeln |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Auswerten von Richtlinien Übung |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Teilnehmen an einer laden Lastenausgleich Back-End-Adresse Ressourcenpool |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Teilnehmen an einer Lastenausgleich eingehende NAT-Regel |
| Microsoft.Network/networkInterfaces/*/read | Lesen Sie die Eigenschaften des Netzwerk-Schnittstellen (beispielsweise alle den Lastenausgleich, die die Schnittstelle Teil ist) |
| Microsoft.Network/networkInterfaces/join/action | Teilnehmen an einer virtuellen Computers zu Netzwerk-Schnittstellen |
| Microsoft.Network/networkInterfaces/read | Lesen von Netzwerk-Schnittstellen |
| Microsoft.Network/networkInterfaces/write | Netzwerk-Schnittstellen schreiben |
| Microsoft.Network/publicIPAddresses/*/read | Lesen Sie die Eigenschaften einer öffentlichen IP-Adresse |
| Microsoft.Network/publicIPAddresses/join/action | Teilnehmen an einer öffentlichen IP-Adresse |
| Microsoft.Network/publicIPAddresses/read | Lesen Sie Netzwerk öffentliche IP-Adressen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Teilnehmen an einer virtuelles Netzwerk |
| Microsoft.Resources/deployments/operations/read | Lesen Sie Bereitstellungsvorgänge |
| Microsoft.Resources/deployments/read | Lesen Sie Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Storage/storageAccounts/listKeys/action | Liste Speicher Konto Tasten |

### <a name="dns-zone-contributor"></a>DNS-Zone Mitwirkender
Können DNS-Zonen und Einträge verwalten.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/\*/lesen | Finden Sie hier Rollen und rollenzuweisungen |
| Microsoft.Insights/alertRules/\* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Network/dnsZones/\* | Erstellen und Verwalten von DNS-Zonen und Einträge |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/\* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/\* | Erstellen und Verwalten von supporttickets |


### <a name="documentdb-account-contributor"></a>Mitwirkender DocumentDB-Konto
Können DocumentDB Konten verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.DocumentDb/databaseAccounts/* | Erstellen und Verwalten von Konten DocumentDB |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="intelligent-systems-account-contributor"></a>Intelligente Systeme Konto Mitwirkender
Intelligente Systeme Konten verwalten kann

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.IntelligentSystems/accounts/* | Erstellen und Verwalten von Konten intelligente Systeme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="network-contributor"></a>Netzwerk Mitwirkender
Kann alle Netzwerk-Ressourcen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Network/* | Erstellen und Verwalten von Netzwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="new-relic-apm-account-contributor"></a>Neue Relic APM Konto Mitwirkender
Neue Relic Leistung Anwendungsverwaltung Konten und Applikationen können verwaltet werden.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |
| NewRelic.APM/accounts/* | Erstellen und Verwalten von Konten für neue Relic Anwendung Leistung management |

### <a name="owner"></a>Besitzer
Alles, einschließlich des Zugriffs verwalten können

| **Aktionen** ||
| ------- | ------ |
| * | Erstellen und Verwalten von Ressourcen aller Typen |

### <a name="reader"></a>Reader
Kann alle Elemente anzeigen, jedoch können keine Änderungen vornehmen

| **Aktionen** ||
| ------- | ------ |
| * / Lesen | Weitere Ressourcen aller Typen, mit Ausnahme von vertraulichen Daten. |

### <a name="redis-cache-contributor"></a>Redis Cache Mitwirkender
Verwalten des Caches Redis können

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Cache/redis/* | Erstellen und Verwalten von Caches Redis |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="scheduler-job-collections-contributor"></a>Scheduler Job Websitesammlungen Mitwirkender
Können Scheduler Job Websitesammlungen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Scheduler/jobcollections/* | Erstellen und Verwalten von Websitesammlungen Position |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets  |

### <a name="search-service-contributor"></a>Suche Dienst Mitwirkender
Verwalten der Dienste suchen können

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Search/searchServices/* | Erstellen und Verwalten von Suchdiensten |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets  |

### <a name="security-manager"></a>Sicherheits-Manager
Können Sicherheitskomponenten, Sicherheitsrichtlinien und virtuellen Computern verwalten.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.ClassicCompute/*/read | Weitere Informationen zur Konfiguration klassischen berechnen virtuellen Computern |
| Microsoft.ClassicCompute/virtualMachines/*/write | Schreiben Sie Konfiguration für virtuellen Computern |
| Microsoft.ClassicNetwork/*/read | Weitere Informationen zur klassischen Netzwerk Konfiguration  |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Security/* | Erstellen und Verwalten von Sicherheitskomponenten und Richtlinien |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets  |

### <a name="sql-db-contributor"></a>SQL-DB Mitwirkender
Können SQL-Datenbanken, aber nicht deren Sicherheits-Richtlinien verwalten.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Finden Sie hier Rollen und Rolle Zuordnungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Sql/servers/databases/* | Erstellen und Verwalten von SQL-Datenbanken |
| Microsoft.Sql/servers/read | Lesen von SQL Server |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Überwachungsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungseinstellungen kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Audit-Datensätzen nicht gelesen werden. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Verbindungsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Daten masking Richtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Sicherheit benachrichtigen Richtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityMetrics/* | Sicherheit Kennzahlen können nicht bearbeitet werden. |

### <a name="sql-security-manager"></a>SQL-Sicherheits-Manager
Können die Sicherheits-Richtlinien von SQL Server und Datenbanken verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Microsoft Autorisierung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Sql/servers/auditingPolicies/* | Erstellen und Verwalten von SQL Server-Überwachungsrichtlinien |
| Microsoft.Sql/servers/auditingSettings/* | Erstellen und Verwalten von SQL Server ü Einstellung |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Erstellen und Verwalten von Überwachungsrichtlinien für SQL Server-Datenbank |
| Microsoft.Sql/servers/databases/auditingSettings/* | Erstellen und Verwalten von überwachungseinstellungen für SQL Server-Datenbank |
| Microsoft.Sql/servers/databases/auditRecords/read | Lesen von Audit-Datensätzen |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Erstellen und Verwalten von Richtlinien für SQL Server-Datenbank-Verbindung |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Erstellen und Verwalten von SQL Server-Datenbankdaten masking Richtlinien |
| Microsoft.Sql/servers/databases/read | Lesen von SQL-Datenbanken |
| Microsoft.Sql/servers/databases/schemas/read | Lesen von SQL Server-Datenbank mithilfe von schemas |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Lesen von SQL Server-Datenbanktabellenspalten |
| Microsoft.Sql/servers/databases/schemas/tables/read | Lesen von SQL Server-Datenbanktabellen |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Erstellen und Verwalten von SQL Server-Datenbank Sicherheit benachrichtigen Richtlinien |
| Microsoft.Sql/servers/databases/securityMetrics/* | Erstellen und Verwalten von SQL Server-Datenbank Sicherheit Kennzahlen |
| Microsoft.Sql/servers/read | Lesen von SQL Server |
| Microsoft.Sql/servers/securityAlertPolicies/* | Erstellen und Verwalten von Benachrichtigungssounds Richtlinien für SQL Server-Sicherheit |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="sql-server-contributor"></a>SQL Server-Teilnehmer
Können SQL Server und Datenbanken aber nicht ihre Sicherheits-Richtlinien verwalten.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisierung lesen|
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Sql/servers/* | Erstellen und Verwalten von SQL Server |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | SQL Server-Überwachungsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/auditingSettings/* | SQL Server-überwachungseinstellungen kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditingPolicies/* | SQL Server-Datenbank Überwachungsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungseinstellungen für SQL Server-Datenbank kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Audit-Datensätzen nicht gelesen werden. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | SQL Server-Datenbank Verbindungsrichtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL Server-Datenbankdaten masking Richtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL Server-Datenbank Sicherheit benachrichtigen Richtlinien kann nicht bearbeitet werden. |
| Microsoft.Sql/servers/databases/securityMetrics/* | SQL Server-Datenbank Sicherheit Kennzahlen können nicht bearbeitet werden. |
| Microsoft.Sql/servers/securityAlertPolicies/* | SQL Server-Sicherheit benachrichtigen Richtlinien kann nicht bearbeitet werden. |

### <a name="classic-storage-account-contributor"></a>Klassische Speicher Konto Mitwirkender
Klassische Speicherkonten verwalten kann

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisierung lesen |
| Microsoft.ClassicStorage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="storage-account-contributor"></a>Mitwirkender für Speicher-Konto
Können Speicherkonten verwalten, aber nicht darauf zugreifen.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen Sie alle Autorisierung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.Insights/diagnosticSettings/* | Verwalten von diagnoseeinstellungen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Storage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="user-access-administrator"></a>Access-Administrator für Benutzer
Verwalten des Benutzerzugriffs auf Azure Ressourcen können

| **Aktionen** ||
| ------- | ------ |
| * / Lesen | Weitere Ressourcen aller Typen, mit Ausnahme von vertraulichen Daten. |
| Microsoft.Authorization/* | Verwalten der Autorisierung |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="classic-virtual-machine-contributor"></a>Mitwirkender klassischen virtuellen Computern
Verwalten können, klassische virtuellen Computern, aber nicht das virtuelle Netzwerk oder Speicher-Konto, das sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisierung lesen |
| Microsoft.ClassicCompute/domainNames/* | Erstellen und Verwalten von Domänennamen klassischen berechnen |
| Microsoft.ClassicCompute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Teilnehmen an Netzwerk-Sicherheitsgruppen |
| Microsoft.ClassicNetwork/reservedIps/link/action | Reservierte IP-Adressen verknüpfen |
| Microsoft.ClassicNetwork/reservedIps/read | Weitere reservierte IP-Adressen |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Teilnehmen an virtuelle Netzwerke |
| Microsoft.ClassicNetwork/virtualNetworks/read | Lesen Sie virtuelle Netzwerke |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Lesen Sie Datenträger zum Speichern |
| Microsoft.ClassicStorage/storageAccounts/images/read | Lesen Sie Bilder für Speicher-Konto |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Liste Speicher Konto Tasten |
| Microsoft.ClassicStorage/storageAccounts/read | Lesen Sie klassische Speicherkonten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="virtual-machine-contributor"></a>Mitwirkender virtuellen Computern
Verwalten von können virtuellen Computern, aber nicht das virtuelle Netzwerk oder Speicher-Konto, das sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisierung lesen |
| Microsoft.Compute/availabilitySets/* | Erstellen und Verwalten von berechnen Verfügbarkeit Datensätze |
| Microsoft.Compute/locations/* | Erstellen und Verwalten von berechnen Speicherorte |
| Microsoft.Compute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
| Microsoft.Compute/virtualMachineScaleSets/* | Erstellen und Verwalten von virtuellen Computern skalieren Datensätze |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Teilnehmen an Netzwerk Gateway Back-End-Adresse Anwendungspools |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Teilnehmen an laden Lastenausgleich Back-End-Adresspools |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Teilnehmen an Lastenausgleich eingehende NAT Pools |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Teilnehmen an Lastenausgleich eingehende Regeln NAT |
| Microsoft.Network/loadBalancers/read | Lastenausgleich gelesen |
| Microsoft.Network/locations/* | Erstellen und Verwalten von Netzwerkspeicherorte |
| Microsoft.Network/networkInterfaces/* | Erstellen und Verwalten von Netzwerk-Schnittstellen |
| Microsoft.Network/networkSecurityGroups/join/action | Teilnehmen an Netzwerk-Sicherheitsgruppen |
| Microsoft.Network/networkSecurityGroups/read | Lesen von Netzwerk-Sicherheitsgruppen |
| Microsoft.Network/publicIPAddresses/join/action | Teilnehmen an Netzwerk öffentliche IP-Adressen |
| Microsoft.Network/publicIPAddresses/read | Lesen Sie Netzwerk öffentliche IP-Adressen |
| Microsoft.Network/virtualNetworks/read | Lesen Sie virtuelle Netzwerke |
| Microsoft.Network/virtualNetworks/subnets/join/action | Teilnehmen an virtuelle Netzwerksubnets |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Storage/storageAccounts/listKeys/action | Liste Speicher Konto Tasten |
| Microsoft.Storage/storageAccounts/read | Lesen von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="classic-network-contributor"></a>Klassische Netzwerk Mitwirkender
Können klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisierung lesen |
| Microsoft.ClassicNetwork/* | Erstellen und Verwalten von klassischen Netzwerken |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |

### <a name="web-plan-contributor"></a>Web Plan Mitwirkender
Verwalten von Web-Pläne können

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisierung lesen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |
| Microsoft.Web/serverFarms/* | Erstellen und Verwalten von Serverfarmen |

### <a name="website-contributor"></a>Website Mitwirkender
Verwalten können, Websites, jedoch nicht im Web-Pläne, die mit denen sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisierung lesen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln Einsichten |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Einsichten Komponenten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen Sie die Integrität der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcen Gruppe Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Weitere Ressourcengruppen |  
| Microsoft.Support/* | Erstellen und Verwalten von supporttickets |
| Microsoft.Web/certificates/* | Erstellen und Verwalten von Website-Zertifikate |
| Microsoft.Web/listSitesAssignedToHostName/read | Lesen von Websites, die ein Hostname zugewiesen ist |
| Microsoft.Web/serverFarms/join/action | Teilnehmen an Serverfarmen |
| Microsoft.Web/serverFarms/read | Serverfarmen lesen |
| Microsoft.Web/sites/* | Erstellen und Verwalten von websites |

## <a name="see-also"></a>Siehe auch
- [Rollenbasierte Access Control](role-based-access-control-configure.md): Erste Schritte mit RBAC Azure-Portal.
- [Benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md): Informationen zum Erstellen von benutzerdefinierter Rollen an Ihre Bedürfnisse Access anpassen.
- [Erstellen einer Access ändern Verlaufsbericht](role-based-access-control-access-change-history-report.md): Nachverfolgen von geändert rollenzuweisungen in RBAC.
- [Rollenbasierte Access Control Problembehandlung](role-based-access-control-troubleshooting.md): Abrufen von Vorschlägen für häufige Probleme beheben.
