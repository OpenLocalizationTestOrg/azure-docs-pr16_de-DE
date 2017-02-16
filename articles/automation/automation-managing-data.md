<properties 
   pageTitle="Verwalten von Daten Azure Automatisierung | Microsoft Azure"
   description="Dieser Artikel enthält mehrere Themen für die Verwaltung einer Automatisierung Azure-Umgebung.  Aktuell enthält Daten Aufbewahrung und Azure Automatisierung Wiederherstellung in Azure Automatisierung sichern."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Verwalten von Automatisierung Azure-Daten

Dieser Artikel enthält mehrere Themen für die Verwaltung einer Automatisierung Azure-Umgebung.

## <a name="data-retention"></a>Beibehaltung der Daten

Wenn Sie eine Ressource in Azure Automatisierung löschen, wird es 90 Tage lang ü Zwecke, bevor Sie dauerhaft entfernt werden beibehalten.  Sie nicht sehen oder verwenden Sie die Ressource während dieses Zeitraums.  Diese Richtlinie gilt auch für Ressourcen, die mit einer Firma Automatisierung gehören, die nicht gelöscht werden.

Azure Automatisierung automatisch löscht und dauerhaft entfernt Aufträge, die älter als 90 Tage.

Die folgende Tabelle enthält eine Übersicht über die Aufbewahrungsrichtlinie für verschiedene Ressourcen.

|Daten|Richtlinie|
|:---|:---|
|Konten|Dauerhaft entfernt 90 Tage nach das Konto von einem Benutzer gelöscht wird.|
|Posten|Dauerhaft entfernt 90 Tage nach dem Löschen der Anlage von einem Benutzer oder 90 Tage nach dem Konto, das enthält, dass die Anlage von einem Benutzer gelöscht wird.|
|Module|Dauerhaft entfernt 90 Tage nach dem Löschen des Moduls von einem Benutzer oder 90 Tage nach dem Konto, das enthält, dass das Modul von einem Benutzer gelöscht wird.|
|Runbooks|Dauerhaft entfernt 90 Tage nach dem Löschen der Ressource von einem Benutzer oder 90 Tage nach dem Konto, das enthält, dass die Ressource von einem Benutzer gelöscht wird.|
|Aufträge|Gelöschte und dauerhaft entfernte 90 Tage nach dem letzten geändert wird. Nachdem Sie der Auftrag abgeschlossen ist, wird beendet oder unterbrochen ist möglicherweise.|
|Knoten Konfigurationen/MOF-Dateien| Konfiguration von alten Knoten wird 90 Tage nach dem Generieren einer neuen Knoten Konfigurations dauerhaft entfernt werden.|
|DSC Knoten| Dauerhaft entfernt 90 Tage nach der Knoten von Automatisierung Konto mithilfe von Azure-Portal oder das Cmdlet [' Registrierung aufheben '-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) in Windows PowerShell aufgehoben wird. Knoten werden auch dauerhaft 90 Tage nach dem Konto entfernt, die enthält, dass der Knoten von einem Benutzer gelöscht wird. |
|Knoten-Berichte| Dauerhaft entfernt 90 Tage nach dem Generieren eines neuen Berichts für diesen Knoten|

Die Aufbewahrungsrichtlinie gilt für alle Benutzer und derzeit nicht angepasst werden kann.

## <a name="backing-up-azure-automation"></a>Sichern von Azure Automatisierung

Wenn Sie ein Konto Automatisierung in Microsoft Azure löschen, werden alle Objekte in dem Konto einschließlich Runbooks, Module, Konfigurationen, Einstellungen, Aufträge und Anlagen gelöscht. Die Objekte können nicht wiederhergestellt werden, nachdem das Konto gelöscht wird.  Die folgende Informationen können Sie den Inhalt Ihres Kontos Automatisierung sichern, bevor Sie ihn löschen. 

### <a name="runbooks"></a>Runbooks

Sie können Ihre Runbooks in Skriptdateien, die unter der Azure-Verwaltungsportal oder das Cmdlet " [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) " in Windows PowerShell exportieren.  Diese Skriptdateien können in ein anderes Automatisierung Konto importiert werden, wie in [Erstellen oder Importieren einer Runbooks](https://msdn.microsoft.com/library/dn643637.aspx)erläutert.


### <a name="integration-modules"></a>Integrationsmodule

Sie können keine Integrationsmodule aus Azure Automatisierung exportieren.  Sie müssen sicherstellen, dass sie außerhalb des Kontos Automatisierung zur Verfügung stehen.

### <a name="assets"></a>Posten

Sie können keine [Anlagen](https://msdn.microsoft.com/library/dn939988.aspx) aus Azure Automatisierung exportieren.  Verwenden der Azure-Verwaltungsportal, müssen Sie die Details der Variablen, Anmeldeinformationen, Zertifikate, Verbindungen und Terminpläne beachten.  Sie müssen dann manuell alle Anlagen erstellen, die von Runbooks verwendet werden, die Sie in eine andere Automatisierung importieren.

[Azure Cmdlets](https://msdn.microsoft.com/library/dn690262.aspx) können Sie die Details des unverschlüsselter Posten und entweder speichern sie für die spätere abrufen oder gleichwertiger Posten in einem anderen Automatisierung Konto erstellen.

Sie können nicht den Wert für verschlüsselte Variablen oder das Feld Kennwort ein, der Anmeldeinformationen mithilfe des Cmdlets abrufen.  Wenn Sie nicht, dass diese Werte wissen, können Sie diese aus einer Runbooks mit den Aktivitäten [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) und [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) abrufen.

Sie können keine Zertifikate aus Azure Automatisierung exportieren.  Sie müssen sicherstellen, dass alle Zertifikate außerhalb Azure verfügbar sind.

### <a name="dsc-configurations"></a>DSC Konfigurationen

Sie können Ihre Konfigurationen in Skriptdateien, die unter der Azure-Verwaltungsportal oder das Cmdlet [Exportieren-AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) in Windows PowerShell exportieren. Diese Konfigurationen können importiert und in ein anderes Automatisierung-Konto verwendet werden.


##<a name="geo-replication-in-azure-automation"></a>Geo-Replikation in Azure Automatisierung

Geo-Replikation, Standard in Azure Automatisierung Konten sichert Kontodaten in ein anderes geografische Region Redundanzgründen. Wenn Ihr Konto einrichten, wählen Sie eine primäre Region können, und klicken Sie dann eine sekundäre Region zugeordnet ist es automatisch. Die sekundären, von der primären Region kopierten Daten werden bei Datenverlust kontinuierlich aktualisiert.  

Die folgende Tabelle zeigt die verfügbaren primären und sekundären Region Kombinationen.

|Primärschlüssel            |Sekundären
| ---------------   |----------------
|Süd zentralen US   |Nord-zentralen US
|US-OST 2          |USA – zentral
|Westen Europa        |North Europa
|Süd Ostasien    |Ostasien
|Japan OST         |Japan "Westen"

Microsoft versucht, in das wahrscheinlich nicht Ereignis, einer primären Region Daten verloren gehen ihn wiederherstellen. Wenn die primären Daten nicht wiederhergestellt werden können, dann Geo-Failover ausgeführt wird, und werden die betroffenen Kunden Informationen hierzu finden Sie durch ihr Abonnement benachrichtigt.

