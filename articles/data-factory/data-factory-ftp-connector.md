<properties 
    pageTitle="Verschieben von Daten von FTP-Server | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten aus einem FTP-Server mit Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Verschieben von Daten aus einem FTP-Server mit Azure Data Factory
Dieser Artikel beschreibt, wie die Aktivität kopieren in Azure Data Factory verwenden Sie zum Verschieben von Daten aus einem FTP-Server zu einem Datenspeicher unterstützten Empfänger. In diesem Artikel basiert auf Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , der bietet eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und die Liste der Datenspeicher wie Quellen/senken unterstützt. 

Daten Factory unterstützt derzeit nur Verschieben von Daten aus einem FTP-Server auf andere Datenspeicher, aber nicht zum Verschieben von Daten aus anderen Datenspeicher in einem FTP-Server. Es unterstützt sowohl lokalen und cloud FTP-Servern. 

Wenn Sie Daten aus einem **lokalen** FTP-Server in einen Cloud-Datenspeicher verschieben (Beispiel: Azure BLOB-Speicher), installieren und Verwenden des Datenverwaltungsgateways. Das Datenverwaltungsgateway ist ein Client-Agent, der auf dem lokalen Computer installiert ist, wodurch Cloud Services für die Verbindung zu lokalen Ressourcen. Auf dem Gateway Details finden Sie unter [Datenverwaltungsgateway](data-factory-data-management-gateway.md) . Finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) -Artikel, um eine schrittweise Anleitung auf dem Gateway einrichten und verwenden es ein. Verwenden Sie das Gateway Verbindung zu einem FTP-Server, auch wenn der Server auf eine IaaS Azure-virtuellen Computern (virtueller Computer) ist. 

Sie können das Gateway auf demselben lokalen Computer oder der Azure Neuerung als FTP-Server installieren. Allerdings empfiehlt es sich, dass die Installation des Gateways auf einem separaten Computer oder einem separaten Azure Neuerung zu vermeiden und für eine bessere Leistung. Bei der Installation des Gateways auf einem separaten Computer sollten der Computer auf den FTP-Server zugreifen. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten von einem FTP-Server kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, mit denen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus FTP-Server in Azure blob

Dieses Beispiel zeigt, wie Sie Daten aus einem FTP-Server in einer Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

- Eine verknüpfte Dienst vom Typ [FTP-Server](#ftp-linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [Dateifreigabe](#fileshare-dataset-type-properties).
- Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [FileSystemSource](#ftp-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Daten von einem FTP-Server auf eine Azure Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

**FTP verknüpft service** In diesem Beispiel wird die Standardauthentifizierung mit Benutzername und Kennwort im nur-Text ein. Sie können auch eine der folgenden Methoden verwenden: 

- Anonyme Authentifizierung 
- Standardauthentifizierung mit verschlüsselten Anmeldeinformationen
- FTP über SSL/TLS (FTPS)

Finden Sie unter [FTP verknüpft Dienst](#ftp-linked-service-properties) Abschnitt für unterschiedliche Arten von Authentifizierung, die Sie verwenden können. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
    }

**Azure verknüpft Speicherdienst**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Eingabe FTP-dataset** Dieses Dataset bezieht sich auf den FTP-Ordner `mysharedfolder` und `test.csv`. Der Verkaufspipeline kopiert die Datei an das Ziel. 

Festlegen von "externe": "true" werden dem Daten Factory-Dienst informiert, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure Blob ausgeben dataset**

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch ausgewertet basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunden Teile der Startzeit.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pipeline mit Aktivität kopieren**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **FileSystemSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>FTP-Dienst verknüpfte Eigenschaften

Die folgende Tabelle enthält eine Beschreibung für verknüpfte Dienst FTP-spezifischen JSON-Elemente.

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| -------- | ----------- | -------- | ------- | 
| Typ | Die Eigenschaft muss auf FTP-Server festgelegt werden | Ja | &nbsp;
| Host | Name oder IP-Adresse des FTP-Servers | Ja | &nbsp;
| authenticationType | Authentifizierungstyp angeben | Ja | Anonyme Basic |
| Benutzername | Benutzer mit Zugriff auf den FTP-server | Nein | &nbsp;
| Kennwort | Kennwort für den Benutzer (Username) | Nein | &nbsp;
| encryptedCredential | Verschlüsselte Anmeldeinformationen Zugriff auf den FTP-server | Nein | &nbsp;
| gatewayName | Name des Gateways Datenverwaltungsgateway Verbindung zu einem lokalen FTP-server | Nein    | &nbsp;
| Port | Port für den FTP-Server wartet | Nein | 21 |
| enableSsl | Geben Sie an, ob die Verwendung von FTP über SSL/TLS-Kanal | Nein | WAHR | 
| enableServerCertificateValidation | Geben Sie an, ob der Server SSL-Zertifikat Überprüfung aktivieren, bei der Verwendung von FTP über SSL/TLS-Kanal | Nein | WAHR | 

### <a name="using-anonymous-authentication"></a>Anonyme Authentifizierung

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Verwenden von Benutzername und Kennwort im nur-Text für die Standardauthentifizierung
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Port, EnableSsl, EnableServerCertificateValidation verwenden

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Verwenden für die Authentifizierung und Gateway encryptedCredential

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale FTP-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Kopieren von FTP-Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten. 

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren Typeigenschaften, je nach den Typen von Datenquellen und senken.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.

## <a name="next-steps"></a>Nächste Schritte
Finden Sie unter den folgenden Artikeln: 

- [Lernprogramm kopieren Aktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für eine schrittweise Anleitung zum Erstellen einer Verkaufspipeline mit einer Aktivität kopieren. 
