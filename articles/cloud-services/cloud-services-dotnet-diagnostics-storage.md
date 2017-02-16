<properties
    pageTitle="Speichern und Ansicht Diagnostic Daten in Azure-Speicher | Microsoft Azure"
    description="Azure-Diagnosedaten in Azure-Speicher abrufen und anzeigen"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Speichern und Ansicht diagnostic Daten in Azure-Speicher

Diagnose Daten werden nicht dauerhaft gespeichert, es sei denn, Sie Microsoft Azure Speicheremulator oder Azure-Speicher übertragen werden. Einmal kann in Speicher, es mit einem der verschiedenen verfügbaren Tools angezeigt werden.

## <a name="specify-a-storage-account"></a>Geben Sie ein Speicherkonto

Sie geben Sie das Speicherkonto, das Sie in der Datei ServiceConfiguration.cscfg verwenden möchten. Die Kontoinformationen wird als eine Verbindungszeichenfolge in einer Einstellung definiert. Im folgenden Beispiel wird die Standard-Verbindungszeichenfolge für ein neues Projekt Cloud-Dienst in Visual Studio erstellt:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Sie können diese Verbindungszeichenfolge zum Bereitstellen von Kontoinformationen für ein Konto Azure-Speicher ändern.

Je nach Art der Diagnose Daten, die gesammelt wird, verwendet Azure-Diagnose entweder den Blob-Dienst oder den Dienst Tabelle. Die folgende Tabelle zeigt die Datenquellen, die beibehalten werden und deren Format.

|Datenquelle|Speicherformat|
|---|---|
|Azure Protokolle|Tabelle|
|IIS 7.0 Protokolle|BLOB|
|Azure Diagnose Infrastrukturprotokolle|Tabelle|
|Fehler beim Anfordern der Überwachungsprotokolle|BLOB|
|Windows-Ereignisprotokollen|Tabelle|
|-Datenquellen|Tabelle|
|Speichert abstürzen|BLOB|
|Benutzerdefinierte Fehlerprotokolle|BLOB|

## <a name="transfer-diagnostic-data"></a>Übertragen von Diagnoseprotokollen Daten

Für SDK 2,5 und höher, kann die Anfrage diagnostic Datenübertragung über die Konfigurationsdatei auftreten. Sie können diagnostische Daten in regelmäßigen Abständen als in der Konfiguration angegebenen übertragen.

SDK 2.4 und die vorherige können Sie anfordern, die Diagnose Daten wie programmgesteuert über die Konfigurationsdatei ebenfalls übertragen. Der programmgesteuerten Ansatz können Sie bei Bedarf übertragen möchten.


>[AZURE.IMPORTANT] Wenn Sie mit einer Firma Azure-Speicher diagnostische Daten übertragen, entstehen Sie Kosten für den Speicherressourcen, die Ihre Daten diagnostic verwendet.

## <a name="store-diagnostic-data"></a>Speichern von Diagnoseprotokollen Daten

Log-Daten werden in entweder Blob oder Tabelle Speicher mit den folgenden Namen gespeichert:

**Tabellen**

- **WadLogsTable** - Protokolle, die in der Spur Zuhörer mit Code geschrieben wurde.

- **WADDiagnosticInfrastructureLogsTable** - Diagnose überwachen und Konfiguration ändert.

- **WADDirectoriesTable** – Verzeichnisse durchsuchen, die der Diagnoseprotokollen Monitor überwacht werden.  Dies umfasst IIS-Protokolle, IIS Fehler bei Anforderungsprotokolle und benutzerdefinierte Verzeichnisse durchsuchen.  Die Position der Protokolldatei Blob in das Feld "Container" angegeben ist, und der Namen des Blob in das Feld "RelativePath" ist.  Das Feld "AbsolutePath" gibt an, den Speicherort und den Namen der Datei auf den Azure-virtuellen Computern vorhanden war.

- **WADPerformanceCountersTable** – Leistungsindikatoren.

- **WADWindowsEventLogsTable** – Windows-Ereignisprotokollen.

**BLOBs**

- **Wad-Steuerelement-Container** – enthält (nur für SDK 2.4 und vorherigen) die XML-Konfiguration-Dateien, die die Azure Diagnose steuert.

- **Wad-Iis-Failedreqlogfiles** – ' Protokolle ' enthält Informationen aus IIS Fehler beim anfordern.

- **Wad-Iis-Protokolldateien** – ' Protokolle ' enthält Informationen zur IIS aus.

- **"Benutzerdefiniert"** – einen benutzerdefinierten Container basierend auf Konfigurieren Verzeichnisse durchsuchen, die von der Diagnose Monitor überwacht werden.  Der Name des diesem Blob-Container wird in WADDirectoriesTable angegeben werden muss.

## <a name="tools-to-view-diagnostic-data"></a>Tools für Diagnose Daten anzeigen
Verschiedene Tools zur Verfügung, die Daten anzuzeigen, nachdem sie an Speicher übertragen werden. Beispiel:

- Server-Explorer in Visual Studio -, wenn Sie die Azure-Tools für Microsoft Visual Studio installiert haben den Knoten Azure-Speicher im Server-Explorer können Sie aus Ihren Konten Azure-Speicher schreibgeschützt Blob und Tabellendaten anzeigen. Sie können Daten aus Ihrem lokalen Speicher Emulator Konto anzeigen und auch aus Speicherkonten erstellt haben für Azure. Weitere Informationen finden Sie unter [Durchsuchen und Verwalten von Speicherressourcen mit Server-Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) ist eine eigenständige Anwendung, die Sie einfach mit Azure-Speicher Daten unter Windows, OSX und Linux arbeiten kann.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) enthält, können Sie anzeigen, herunterladen und Verwalten von Clientanwendungen auf Windows Azure ausgeführte gesammelten Diagnosedaten Manager von Azure Diagnose.


## <a name="next-steps"></a>Nächste Schritte

[Verfolgen des Ablaufs eines Cloud Services-Anwendung mit Azure-Diagnose](cloud-services-dotnet-diagnostics-trace-flow.md)
