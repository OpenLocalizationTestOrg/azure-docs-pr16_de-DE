## <a name="what-is-azure-file-storage"></a>Was ist Azure Dateispeicher?

Dateispeicher bietet freigegebene Speicher für Applikationen mit dem standardmäßigen SMB 2.1 oder SMB 3.0-Protokoll. Microsoft Azure-virtuellen Computern und Cloud Services können Daten über die Anwendungskomponenten über bereitgestellten Freigaben freizugeben und zu lokalen Applikationen Dateidaten in eine Freigabe über die Datei-Speicher-API zugreifen können.

In Azure-virtuellen Computern oder Cloud Services ausgeführt Applications können eine Dateifreigabe-Speicher Datei Daten zugreifen, genau wie eine desktop-Anwendung eine typische SMB-Freigabe bereitstellen möchten bereitgestellt werden. Beliebige Anzahl von Azure-virtuellen Computern oder Rollen kann bereitstellen und gleichzeitig Zugriff auf die Dateifreigabe-Speicher.

Da eine Dateifreigabe-Speicher standard-Dateifreigabe in Azure mit dem SMB-Protokoll ist, können in Azure ausgeführt Applications Daten in die Freigabe über eine Datei I/O APIs zugreifen. Entwickler können daher ihrer vorhandenen Code und Fähigkeiten zu vorhandene Applikationen migrieren nutzen. IT-Experten können PowerShell-Cmdlets zum Erstellen, bereitstellen und Verwalten von Speicher Dateifreigaben als Teil der Verwaltung von Azure Applications verwenden. Mit diesem Leitfaden wird Beispiele für beide angezeigt.

Häufige Verwendungen von Dateispeicher umfassen:

- Migrieren von lokalen Anwendungen, die auf Dateifreigaben Azure-virtuellen Computern ausgeführt oder cloud Services, ohne teure schreibt aufsetzen
- Speichern von freigegebenen Anwendungseinstellungen, beispielsweise in Dateien
- Speichern von Diagnoseprotokollen Daten wie protokolliert Kennzahlen, und Absturz bildet in einem freigegebenen Speicherort ab. 
- Speichern von Tools und Dienstprogramme für die Entwicklung oder Verwalten von Azure-virtuellen Computern oder Cloud Services erforderlich

## <a name="file-storage-concepts"></a>Datei Speicherkonzepte

Dateispeicher enthält die folgenden Komponenten:

![Konzepte-Dateien][files-concepts]

-   **Speicher-Konto:** Alle Zugriff auf Azure-Speicher erfolgt über ein Speicherkonto. Details zum Konto Speicherkapazität finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](../articles/storage/storage-scalability-targets.md) .

-   **Freigeben:** Eine Dateifreigabe-Speicher ist eine Dateifreigabe SMB in Azure. 
    In einer übergeordneten Freigabe müssen alle Verzeichnisse und Dateien erstellt werden. Ein Konto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien, bis zu 5 TB total Kapazität der Dateifreigabe speichern.

-   **Verzeichnis:** Eine optionale Hierarchie Verzeichnisse durchsuchen. 

-   **Datei:** Eine Datei freigeben. Eine Datei kann bis zu 1 TB groß sein.

-   **URL-Format:** Dateien werden in folgendem Format ein URL adressiert:   
    https://`<storage
    account>`.file.core.windows.net/`<share>`/`<directory/directories>`/`<file>`  
    
    Im folgenden Beispiel-URL könnte Adresse eine der Dateien im obigen Diagramm verwendet werden:  
    `http://samples.file.core.windows.net/logs/CustomLogs/Log1.txt`

Ausführliche Informationen zum Benennen Freigaben, Verzeichnisse und Dateien finden Sie unter [Naming und verweisen auf Freigaben, Verzeichnisse durchsuchen, Dateien und Metadaten](http://msdn.microsoft.com/library/azure/dn167011.aspx).

[files-concepts]: ./media/storage-file-concepts-include/files-concepts.png