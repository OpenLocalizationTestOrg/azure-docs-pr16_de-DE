## <a name="azure-storage-linked-service"></a>Azure-Speicher verknüpft Service

Der **Azure-Speicher verknüpft Dienst** , können Sie ein Konto Azure-Speicher eine Fabrik Azure-Daten mit der **kontoschlüssel**verknüpfen. Dadurch die Daten Factory globaler Zugriff auf den Azure-Speicher. Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für Azure verknüpft Speicherdienst.

| Eigenschaft | Beschreibung | Erforderlich |
| :-------- | :----------- | :-------- |
| Typ | Die Eigenschaft muss auf festgelegt sein: **AzureStorage** | Ja |
| connectionString | Geben Sie die Verbindung zum Azure-Speicher für die Eigenschaft ConnectionString erforderlichen Informationen an. | Ja |

Finden Sie im folgenden Artikel die Schritte zum Anzeigen/Kopieren der kontoschlüssel für eine Azure-Speicher: [Ansicht "," Kopieren "und" erstellen, jeweils Speicher Zugriffstasten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Beispiel:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure-Speicher Sas verknüpft Service  
Eine freigegebene Access-Signatur (SAS) bietet delegierten Zugriff auf Ihr Speicherkonto Ressourcen. Dies bedeutet, dass Sie, dass ein Client für einen angegebenen Zeitraum Zeit und mit einem zuvor festgelegten Berechtigungen, Berechtigungen für Objekte in Ihr Speicherkonto eingeschränkt gewähren können, ohne zu Ihrem Konto Zugriffstasten freigeben. Die SAS ist ein URI, der in der Abfrageparameter umfasst alle Informationen zur erforderlichen Zugriff auf eine Speicherressource authentifiziert. Um Speicherressourcen mit der SAS zugreifen zu können, muss der Client nur die SAS auf die entsprechenden oder Methode zu übergeben. Ausführliche Informationen zu SAS, finden Sie unter [freigegeben Access Signaturen: Grundlegendes zu SAS-Modell](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
Der Dienst Azure-Speicher SAS verknüpft, können Sie ein Azure-Speicher-Konto mithilfe einer freigegebenen Access Signatur (SAS) mit einer Factory Azure-Daten zu verknüpfen. Dadurch werden die Daten Factory beschränkt/Uhrzeit-gebundenen Zugriff auf alle/spezifische Ressourcen (Blob/Container) im Speicher. Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für Dienst Azure-Speicher SAS verknüpft. 

| Eigenschaft | Beschreibung | Erforderlich |
| :-------- | :----------- | :-------- |
| Typ | Die Eigenschaft muss auf festgelegt sein: **AzureStorageSas**  | Ja |
| sasUri | Geben Sie den Azure-Speicher Ressourcen wie Blob, Container oder Tabelle freigegeben Access Signatur-URI an. Finden Sie unter den folgenden Hinweisen Details. | Ja | 


**Beispiel:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Erwägen beim Erstellen einer **SAS URI**vor:  

- Azure Data Factory unterstützt nur **Dienst SAS**, nicht Konto SAS. Details zu diesen beiden Typen finden Sie unter [Typen von freigegebenen Access Signaturen](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) .
- Entsprechende Lese- **Berechtigungen** festgelegt werden, klicken Sie auf Objekte basierend auf wie verknüpfte Dienst (Lesen, schreiben, schreibgeschützt) in Ihrem Unternehmen Daten verwendet werden müssen.
- **Ablaufzeit** muss ordnungsgemäß festgelegt werden. Stellen Sie sicher, dass der Zugriff auf Azure-Speicher Objekte innerhalb des aktiven Zeitraums der Verkaufspipeline nicht abläuft.
- URI sollte an den richtigen Container BLOB- oder Tabellenebene je nach den Bedarf erstellt werden. Ein SAS-Uri zu einer Azure Blob kann der Daten Factory Dienst diesen bestimmten Blob zugreifen. Ein SAS-Uri zu einem Container Azure Blob ermöglicht den Daten Factory Dienst Blobs im Container durchlaufen. Wenn Sie bieten Zugriff mehr/weniger Objekte später, oder aktualisieren den SAS URI müssen, denken Sie daran, die mit dem neuen URI verknüpften Dienst aktualisieren.   
