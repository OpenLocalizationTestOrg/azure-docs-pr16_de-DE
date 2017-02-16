<properties
    pageTitle="Wo befindet sich mein Projekt ASP.NET 5 (Visual Studio verbunden Services) | Microsoft Azure-Speicher"
    description="Was geschieht nach dem Herstellen einer Verbindung mit einer Firma Azure-Speicher in einem Visual Studio ASP.NET 5 Projekt mit Visual Studio Services verbunden werden"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Wo befindet sich mein Projekt ASP.NET 5 (Visual Studio Azure-Speicher verbunden Services)?

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

Darüber hinaus wurde das NuGet-Paket **Microsoft.Framework.Configuration.Json** hinzugefügt.

## <a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure-Speicher hinzugefügt
Klicken Sie in der Datei config.json Ihres Projekts wurde ein Element mit Verbindungszeichenfolge und Schlüssel des ausgewählten Speicher-Kontos erstellt.

Weitere Informationen finden Sie unter [ASP.NET 5](http://www.asp.net/vnext).
