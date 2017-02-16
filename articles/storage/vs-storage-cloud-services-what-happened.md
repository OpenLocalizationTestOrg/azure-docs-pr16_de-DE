<properties
    pageTitle="Wo befindet sich mein Projekt Cloud-Dienst? | Microsoft Azure | Visual Studio verbunden services"
    description="Beschreibt, was in einen Cloud Services-Projekt geschieht nach Services Herstellen einer Verbindung mit einer Azure-Speicher-Konto mithilfe von Visual Studio verbunden werden."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Wo befindet sich mein Cloud Services-Projekt (Visual Studio Azure-Speicher verbunden Service)?

## <a name="references-added"></a>Verweise hinzugefügt

Das Azure-Speicher NuGet-Paket wurde Ihrer Visual Studio-Projekt hinzugefügt.  
Dieses Paket fügt die folgenden .NET Verweise hinzu:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure-Speicher hinzugefügt
Elemente mit Verbindungszeichenfolge und Schlüssel des ausgewählten Speicher-Kontos erstellt wurden. Die folgenden Dateien wurden Änderungen vorgenommen:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
