<properties
    pageTitle="Mithilfe von Blob-Speicher für IIS und Tabellenspeicher für Ereignisse | Microsoft Azure"
    description="Log Analytics können die Protokolle für Azure-Dienste, die Diagnose zum Tabellenspeicher schreiben oder IIS-Protokolle geschrieben zu BLOB-Speicher lesen."
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
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Verwenden von Blob-Speicher für IIS und Tabellenspeicher für Ereignisse

Log Analytics können die Protokolle für die folgenden Dienste, die Diagnose zum Tabellenspeicher schreiben oder IIS-Protokolle geschrieben zu BLOB-Speicher lesen:

+ Dienst Fabric Cluster (Preview)
+ Virtuellen Computern
+ Web/Worker-Rollen

Bevor Log Analytics Sammeln von Daten für diese Ressourcen können, muss Azure Diagnose aktiviert sein.

Nachdem die Diagnose aktiviert sind, können das Azure-Portal oder PowerShell konfigurieren Log Analytics die Protokolle sammeln.

Azure-Diagnose ist eine Azure-Erweiterung, die zum Sammeln von Diagnoseprotokollen Daten aus einer Worker-Rolle, Webrolle oder virtuellen Computern in Azure ausgeführt werden können. Die Daten in einem Konto Azure-Speicher gespeichert ist und dann vom Log Analytics gesammelt werden können.

Log Analytics diese Azure-Diagnose Protokolle sammeln muss sich die Protokolle an folgenden Speicherorten:

| Log-Datentyp | Ressourcenart | Speicherort |
| -------- | ------------- | -------- |
| IIS-Protokolle | Virtuellen Computern <br> Webrollen <br> Worker-Rollen | WAD-Iis-Protokolldateien (BLOB-Speicher) |
| Syslog | Virtuellen Computern | LinuxsyslogVer2v0 (Tabelle Speicher) |
| Dienst Fabric Betrieb Ereignisse | Dienst Fabric Knoten | WADServiceFabricSystemEventTable |
| Dienst Fabric zuverlässigen Akteur Ereignisse | Dienst Fabric Knoten | WADServiceFabricReliableActorEventTable |
| Dienst Fabric zuverlässigen Service Ereignisse | Dienst Fabric Knoten | WADServiceFabricReliableServiceEventTable |
| Windows-Ereignisprotokollen | Dienst Fabric Knoten <br> Virtuellen Computern <br> Webrollen <br> Worker-Rollen | WADWindowsEventLogsTable (Table Storage) |
| Windows ETW Protokolle | Dienst Fabric Knoten <br> Virtuellen Computern <br> Webrollen <br> Worker-Rollen | WADETWEventTable (Table Storage) |

>[AZURE.NOTE] IIS-Protokolle von Azure Websites werden derzeit nicht unterstützt.

Für virtuelle Maschinen müssen Sie die Option Installation von [Log Analytics-Agent](log-analytics-azure-vm-extension.md) in Ihrer virtuellen Computers zu Weitere Einsichten zu aktivieren. IIS-Protokolle und Ereignisprotokollen analysieren, sondern können Sie zusätzliche Analyse einschließlich Konfiguration änderungsnachverfolgung, SQL-Bewertung und Update Bewertung durchführen.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Aktivieren von Azure Diagnose in einer virtuellen Computern für Ereignisprotokoll und IIS Websitesammlung melden

Gehen Sie folgendermaßen vor, Azure Diagnose in einer virtuellen Computern für mit Microsoft Azure-Portal Ereignisprotokoll und IIS Log Websitesammlung aktivieren.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>So aktivieren Sie Azure Diagnose in einem virtuellen Computer mit dem Azure-portal

1. Installieren Sie die virtuellen Computer-Agent beim Erstellen eines virtuellen Computers. Wenn des virtuellen Computers bereits vorhanden ist, stellen Sie sicher, dass die virtuellen Computer-Agent bereits installiert ist.
    - Klicken Sie im Portal Azure navigieren Sie zu der virtuellen Computern, wählen Sie **Optional Konfiguration**, und klicken Sie dann auf **Diagnose** , und legen Sie **Status** auf **auf**.

    Nach Abschluss des Vorgangs hat der virtuellen Computer die Erweiterung Azure-Diagnose installiert sein und ausgeführt. Diese Erweiterung ist zum Sammeln von Diagnosedaten verantwortlich ist.

2. Aktivieren Sie Überwachung und konfigurieren Sie der Protokollierung auf einer vorhandenen virtuellen Computer. Sie können auf der Ebene virtueller Computer Diagnose aktivieren. Um Diagnose aktivieren und konfigurieren Sie dann auf Protokollierung, führen Sie die folgenden Schritte aus:
    1. Wählen Sie den virtuellen Computer aus.
    2. Klicken Sie auf **Überwachung**.
    3. Klicken Sie auf **Diagnose**.
    4. Legen Sie den **Status** **auf**.
    5. Wählen Sie jede Diagnose protokollieren, die Sie erfassen möchten.
    7. Klicken Sie auf **OK**.

Mithilfe der PowerShell Azure können Sie genauer die Ereignisse angeben, die in den Azure-Speicher geschrieben werden. Zum [Sammeln von Daten mithilfe von Azure Tabellenspeicher oder IIS-Protokolle geschrieben zu BLOB-geschrieben Diagnose](log-analytics-azure-storage-json.md)verweisen.


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Aktivieren Sie in einer Webrolle für IIS-Protokolldateien und Ereignis Websitesammlung Azure-Diagnose

Allgemeine Schritte zum Aktivieren der Azure-Diagnose finden Sie unter [Wie zu aktivieren Diagnose in einen Cloud-Dienst](../cloud-services/cloud-services-dotnet-diagnostics.md) . Die folgenden Anweisungen mithilfe dieser Informationen und es für die Verwendung mit Log Analytics anpassen.

Mit Azure Diagnose aktiviert:

- IIS-Protokolle werden standardmäßig mit den Zeitabständen durchstellen ScheduledTransferPeriod übertragenen Log-Daten gespeichert werden.
- Windows-Ereignisprotokollen werden standardmäßig nicht übertragen.

### <a name="to-enable-diagnostics"></a>So aktivieren Sie die Diagnose

Konfigurieren von Windows-Ereignisprotokollen aktivieren, oder ändern die ScheduledTransferPeriod, Azure-Diagnose mithilfe der XML-Konfigurationsdatei (diagnostics.wadcfg), siehe [Schritt 4: Erstellen Ihrer Konfigurationsdatei Diagnose und installieren Sie die Erweiterung](../cloud-services/cloud-services-dotnet-diagnostics.md)

Die folgende Beispielkonfigurationsdatei sammelt IIS-Protokolle und alle Ereignisse aus den Protokollen Anwendung und System:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Stellen Sie sicher, dass Ihre ConfigurationSettings Speicher-Konto, wie im folgenden Beispiel gibt:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Die Werte **Kontoname** und **AccountKey** finden Sie in der Azure-Portal im Speicher Konto Dashboard, klicken Sie unter Verwalten von Tastenkombinationen. Das Protokoll für die Verbindungszeichenfolge muss **Https**sein.

Nachdem die aktualisierte diagnostische Konfiguration in der Cloud-Service angewendet wird, und es in Azure-Speicher Diagnose schreibt, sind Sie bereit sind, Log Analytics konfigurieren.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Verwenden der Azure-Portal zum Sammeln von Protokollen von Azure-Speicher

Azure-Portal können Sie um Log Analytics sammeln Sie die Protokolle für die folgenden Azure-Dienste zu konfigurieren:

+ Dienst Fabric Cluster
+ Virtuellen Computern
+ Web/Worker-Rollen

Im Portal Azure navigieren Sie zu dem Arbeitsbereich Log Analytics, und führen Sie die folgenden Aufgaben:

1. Klicken Sie auf *Protokolle für Speicher-Konten*
2. Klicken Sie auf den Vorgang *Hinzufügen*
3. Wählen Sie das Speicher-Konto, das die Protokolle der Diagnose enthält.
  - Dieses Konto kann entweder ein klassischen Speicher oder einer Azure Ressourcenmanager Speicher Benutzerkonto sein.
4. Wählen Sie den Datentyp für die Protokolle sammeln möchten
  - Die Auswahlmöglichkeiten sind IIS-Protokolle. Ereignisse; Syslog (Linux); ETW Protokolle; Dienst Fabric Ereignisse
5. Der Wert für die Quelle wird automatisch ausgefüllt, basierend auf den Datentyp und kann nicht geändert werden
6. Klicken Sie auf OK, um die Konfiguration zu speichern

Wiederholen Sie die Schritte 2 bis 6 für zusätzlichen Speicherkonten und Datentypen, die gewünschte Log Analytics zu sammeln.

Innerhalb von 30 Minuten können Sie Daten aus dem Speicherkonto in Log Analytics sehen. Sie sehen nur Daten, die in den Speicher geschrieben ist, nach die Konfiguration angewendet wird. Log Analytics liest nicht bereits vorhandenen Daten aus dem Speicherkonto.

>[AZURE.NOTE] Im Portal nicht überprüft werden, dass die Quelle im Speicherkonto vorhanden ist oder wenn Sie neue Daten geschrieben werden.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Aktivieren von Azure Diagnose in einer virtuellen Computern für Ereignisprotokoll und IIS Websitesammlung mithilfe der PowerShell protokollieren

Führen Sie die Schritte in [Konfigurieren Log Analytics Azure Diagnose indizieren](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) PowerShell verwenden, lesen Sie aus Azure Diagnose, die Tabellenspeicher geschrieben werden.

Mithilfe der PowerShell Azure können Sie genauer die Ereignisse angeben, die in den Azure-Speicher geschrieben werden.
Weitere Informationen finden Sie unter [Aktivieren der Diagnose in Azure virtuellen Computern](../virtual-machines-dotnet-diagnostics.md).

Sie können aktivieren und Azure Diagnose mit dem folgenden PowerShell-Skript zu aktualisieren.
Sie können auch mit einer benutzerdefinierten Protokollierungskonfiguration dieses Skript verwenden.
Ändern Sie das Skript zum Festlegen von Speicher-Konto, Service Name und Name des virtuellen Computers.
Das Skript verwendet Cmdlets für klassische virtuellen Computern an.

Überprüfen Sie das folgende Muster, kopieren Sie es zu, ändern Sie ihn nach Bedarf, speichern Sie die Stichprobe als PowerShell Skriptdatei und führen Sie das Skript.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Nächste Schritte

- [Verwenden Sie JSON-Dateien im BLOB-Speicher](log-analytics-azure-storage-json.md) , lesen Sie die Protokolle aus Azure services die Diagnose schreiben zu BLOB-Speicher im JSON-Format.
- [Aktivieren von Lösungen](log-analytics-add-solutions.md) einen Einblick in die Daten bereitstellen.
- [Verwenden von Suchabfragen](log-analytics-log-searches.md) zum Analysieren der Daten.
