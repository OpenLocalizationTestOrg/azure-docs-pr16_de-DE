<properties
    pageTitle="Erstellen Sie einen Ereignis Hubs Namespace mit Ereignis Hub und Archiv mithilfe einer Vorlage Azure Ressourcenmanager aktivieren | Microsoft Azure"
    description="Erstellen Sie einen Ereignis Hubs Namespace mit Ereignis Hub und aktivieren Archiv mit Azure Ressourcenmanager Vorlage"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Erstellen Sie einen Ereignis Hubs Namespace mit Ereignis Hub und aktivieren Archiv mithilfe einer Vorlage Azure Ressourcenmanager

In diesem Artikel veranschaulicht, wie eine Ressourcenmanager Azure-Vorlage verwenden, die einen Ereignis Hubs Namespace mit einem Ereignis-Hub erstellt und Archiv auf Ihre Ereignis Hub ermöglicht. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die sind angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen][].

Weitere Informationen zum Übungsbeispiel und Muster werden für Ressourcen Azure-Benennungskonventionen finden Sie unter [Benennungskonventionen Azure Ressourcen][].

Vollständige Vorlage finden Sie unter [Ereignis Hub und aktivieren Archiv Vorlage][] auf GitHub.

>[AZURE.NOTE]
>Um nach der neuesten Vorlagen überprüfen zu können, finden Sie auf den Katalog [Azure Schnellstart Vorlagen][] und suchen Sie nach Ereignis Hubs.

## <a name="what-you-deploy"></a>Was Sie bereitstellen?

Mit dieser Vorlage einen Ereignis Hubs Namespace mit einem Ereignis-Hub bereitstellen und Archiv aktivieren.

[Hubs Ereignis](../event-hubs/event-hubs-what-is-event-hubs.md) ist ein Ereignis Verarbeitung Service zur Ereignis und werden eingehende Azure bei umfangreichen Skalieren mit niedriger Wartezeit und hohe Zuverlässigkeit zur Verfügung stellen. Ereignis Hubs Archiv ermöglicht Ihnen die streaming Daten automatisch in Ihrem Ereignis Hubstandorte Azure Blob-Speicher Ihrer Wahl in eine angegebene Zeit oder Größe Intervall Ihrer Wahl zu übermitteln.

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Ressourcenmanager, definieren Sie Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Sie sollten einen Parameter für diese Werte definieren, die variieren basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleibt. Jeden Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Der Name des Ereignisses Hubs Namespace zu erstellen.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Der Name des Ereignisses Hub erstellt im Hubs Namespace.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Die Anzahl der Tage, die in Ihrem Ereignis Hub die Nachrichten, die beibehalten soll werden soll. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Die Anzahl der Partitionen in Ihrem Ereignis Hub gewünschten.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Aktivieren Sie das Archiv auf Ihre Ereignis-Hub an.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Das Standardformat für Verschlüsselung geben Sie die Daten zu serialisieren.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Das Zeitintervall, an dem das Archiv beginnt Archivierung die Daten in Azure Blob-Speicher.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Das Intervall Größe, an dem das Archiv beginnt Archivierung die Daten in Azure Blob-Speicher.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Das Archiv erfordert eine Konto Speicher Ressource-Id Archiv zu der gewünschten Azure-Speicher aktivieren.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

Der Blob-Container, werden Ihre Daten Ereignis soll, archiviert werden.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

Die API-Version der Vorlage.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Ressourcen für die Bereitstellung

Erstellt einen Namespace des Typs **EventHubs**, mit einem Ereignis-Hub und Archiv ermöglicht.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Authoring Ressourcenmanager Azure-Vorlagen]: ../resource-group-authoring-templates.md
[Schnellstart Azure-Vorlagen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure Benennungskonventionen Ressourcen]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Ereignis Hub und aktivieren Archiv-Vorlage]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
