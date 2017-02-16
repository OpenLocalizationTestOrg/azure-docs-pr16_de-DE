<properties
    pageTitle="Wo befindet sich mein WebJob Project (Visual Studio Azure-Speicher verbunden Service)? | Microsoft Azure"
    description="Beschreibt, was in einem Projekt Azure WebJob ist nach Services Herstellen einer Verbindung mit einem Speicherkonto mithilfe von Visual Studio verbunden werden."
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

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Wo befindet sich mein WebJob Project (Visual Studio Azure-Speicher verbunden Service)?

## <a name="references-added"></a>Verweise hinzugefügt

Das Azure-Speicher NuGet-Paket wurde hinzugefügt oder im Projekt Visual Studio aktualisiert.  
Dieses Paket fügt die folgenden .NET Verweise hinzu:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure-Speicher hinzugefügt
Die Einträge **AzureWebJobsStorage** und **AzureWebJobsDashboard** wurden in der App Ihres Projekts mit der ausgewählten Speicherkonto Verbindungszeichenfolge und Schlüssel aktualisiert.

Weitere Informationen finden Sie unter [Azure WebJobs Dokumentationsressourcen](http://go.microsoft.com/fwlink/?linkid=390226).
