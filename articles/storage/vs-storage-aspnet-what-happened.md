<properties
    pageTitle="Wo befindet sich mein ASP.NET Project? | Microsoft Azure | Visual Studio verbunden services"
    description="Was geschieht nach dem Hinzufügen von Azure-Speicher zu einem ASP.NET-Projekt mit Visual Studio Services verbunden werden"
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

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Wo befindet sich mein Projekt ASP.NET (Visual Studio Azure-Speicher verbunden Service)?

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

##<a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure-Speicher hinzugefügt
Ein Element wurde in der Datei web.config Ihres Projekts mit Verbindungszeichenfolge und Schlüssel des ausgewählten Speicher-Kontos erstellt.

Weitere Informationen finden Sie unter [ASP.NET](http://www.asp.net).
