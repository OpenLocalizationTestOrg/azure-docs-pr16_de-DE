<properties
   pageTitle="Ressourcenmanager Vorlage für Speicher | Microsoft Azure"
   description="Zeigt das Schema Ressourcenmanager für die Bereitstellung von Speicherkonten über eine Vorlage."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Speicher Konto Vorlage schema

Erstellt ein Speicherkonto an.

## <a name="schema-format"></a>Schemaformat

Um ein Speicherkonto zu erstellen, fügen Sie das folgende Schema zum Ressourcenabschnitt der Vorlage ein.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Werte

In den folgenden Tabellen werden die Werte, die Sie in das Schema festlegen müssen.

| Namen | Wert |
| ---- | ---- |
| Typ | Aufzählung<br />Erforderlich<br />**Microsoft.Storage/storageAccounts**<br /><br />Die Ressourcenart zu erstellen. |
| apiVersion | Aufzählung<br />Erforderlich<br />**2015-06-15** oder **2015-05-01-Vorschau**<br /><br />Die API-Version für die Ressource zu erstellen. | 
| Namen | Zeichenfolge<br />Erforderlich<br />Zwischen 3 und 24 Zeichen, nur Zahlen und Kleinbuchstaben.<br /><br />Der Name des Speicherkontos zu erstellen. Der Name muss auf allen Azure eindeutig sein. Sollten Sie die Funktion [UniqueString](resource-group-template-functions.md#uniquestring) mit Ihrer Benennungskonvention wie im folgenden Beispiel dargestellt. |
| Speicherort | Zeichenfolge<br />Erforderlich<br />Ein Bereich, der Speicherkonten unterstützt. Um gültige Regionen zu ermitteln, finden Sie unter [Regionen unterstützt](resource-manager-supported-services.md#supported-regions).<br /><br />Der Bereich, um das Speicherkonto zu hosten. |
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, das angibt, die Art des Speicherkontos zu erstellen. |

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Namen | Wert |
| ---- | ---- | 
| accountType | Zeichenfolge<br />Erforderlich<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**oder **Premium_LRS**<br /><br />Die Art des Speicher-Konto. Die zulässigen Werte entsprechen Standard lokal redundante, Standard Zone redundante, Standard Geo-redundante, Standard-Lesezugriff Geo-redundante und Premium lokal redundante. Informationen zu den folgenden Kontotypen finden Sie unter [Replikation Azure-Speicher](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Beispiele

Im folgende Beispiel wird ein Standard lokal redundante Speicher-Konto mit einem eindeutigen Namen auf Basis der Ressource Gruppe Id bereitgestellt.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Es gibt viele Schnellstart-Vorlagen, die ein Speicherkonto enthalten. Die folgenden Vorlagen veranschaulichen einige häufige Szenarien:

- [Erstellen eines Standard-Speicher-Kontos](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Einfache Bereitstellung von virtuellen ein Windows-Computer](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Einfache Bereitstellung von einer Linux VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Erstellen eines Profils CDN, einen Endpunkt CDN mit einem Konto Speicher als Ursprung](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Erstellen Sie eine hohe Availabilty SharePoint-Farm mit 9 virtuellen Computern unter Verwendung der Powershell DSC-Erweiterung](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Einfache Bereitstellung von einer 5 Knoten secure Dienst Fabric Cluster mit WAD aktiviert](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Erstellen eines virtuellen Computers aus einem Windows-Bild mit 4 leere Daten Datenträger](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zum Speicher finden Sie unter [Einführung in Microsoft Azure-Speicher](./storage/storage-introduction.md).
- Beispielsweise Vorlagen, mit denen Storage Neukunde mit einem virtuellen Computer, finden Sie unter [Bereitstellen einer einfachen Linux virtueller Computer](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) oder [Bereitstellen eines einfachen Windows virtuellen Computers](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
