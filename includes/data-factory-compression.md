### <a name="compression-support"></a>Unterstützung für die Komprimierung  
Verarbeitung großen Datenmengen kann dazu führen, dass e/a und Netzwerkengpässen. Daher komprimierte Daten in Stores können nicht nur Datenübertragung im Netzwerk beschleunigen und Speicherplatz zu sparen, aber auch bringen Leistung erheblich verbesserte Verarbeitung big Data. Datei-basierten Datenspeicher wie Azure Blob oder lokalen Dateisystem wird derzeit Komprimierung unterstützt.  

> [AZURE.NOTE] Komprimierungseinstellungen werden Daten in der **AvroFormat**, **OrcFormat**oder **ParquetFormat**nicht unterstützt. 

Verwenden Sie Komprimierung für ein Dataset, um die Eigenschaft **Komprimierung** im Dataset JSON wie im folgenden Beispiel gezeigt:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
Im Abschnitt **Komprimierung** verfügt über zwei Eigenschaften:  
  
- **Typ:** Komprimierung Codecs, die **GZIP**, **Deflate** oder **BZIP2**dienen.  
- **Ebene:** das Komprimierungsverhältnis, der **Optimal** oder **am schnellsten**sein kann. 
    - **Am schnellsten:** Die Komprimierung sollte Vorgang so schnell wie möglich, selbst wenn die resultierende Datei nicht optimal komprimiert ist. 
    - **Optimal**: der Komprimierungsvorgang sollte optimal komprimiert werden, auch wenn der Vorgang eine längere Zeit in Anspruch nimmt. 
    
    Weitere Informationen finden Sie unter [Komprimierungsebene](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) Thema. 

Angenommen, das oben genannten Beispiel Dataset als die Ausgabe einer Aktivität kopieren verwendet wird, wird die Aktivität kopieren die Ausgabedaten mit GZIP Codec mit optimale Verhältnis komprimiert und Schreiben Sie dann die komprimierten Daten in einer Datei namens pagecounts.csv.gz in der Azure BLOB-Speicher.   

Wenn Sie in einer Eingabe JSON-Dataset Komprimierung Eigenschaft angeben, kann der Verkaufspipeline komprimierte Daten lesen kann, aus der Quelle, und wenn Sie die Eigenschaft in einem Dataset Ausgabe JSON, angeben die Aktivität kopieren komprimierte Daten in das Ziel geschrieben. Hier sind einige Beispielszenarien aus: 

- Lesen GZIP komprimierte Daten aus einer Azure Blob, es dekomprimieren und Ergebnisdaten in einer SQL Azure-Datenbank zu schreiben. In diesem Fall definieren Sie das Eingabe Azure Blob-Dataset mit der Komprimierung JSON-Eigenschaft. 
- Lesen von Daten aus einer nur-Text-Datei aus lokalen Dateisystem, mit GZip-Format komprimieren und die komprimierten Daten in einer Azure Blob schreiben. In diesem Fall definieren Sie ein Dataset Ausgabe Azure Blob, mit der Komprimierung JSON-Eigenschaft.  
- Lesen einer komprimierte GZIP-Datentyps aus einer Azure Blob, dekomprimieren Sie sie, sie mithilfe von BZIP2 komprimieren und Ergebnisdaten in einer Azure Blob schreiben. Sie definieren das Eingabe Azure Blob-Dataset mit vom Komprimierungstyp GZIP und die Ausgabe Dataset mit vom Komprimierungstyp BZIP2 in diesem Fall ein.   
