<properties 
    pageTitle="So richten Sie Computer für die Entwicklung von Medien Services mit .NET" 
    description="Informationen Sie zu den erforderlichen Komponenten für Media-Dienste mit dem Media Services SDK für .NET. Auch erfahren Sie, wie Sie eine Visual Studio-app zu erstellen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Media-Dienste Entwicklung mit .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

In diesem Thema wird erläutert, wie So starten Sie die Entwicklung von Medienservices-Clientanwendungen, die mit .NET wird.

Die **Azure Media Services.NET SDK** -Bibliothek können Sie beim Programmieren mit .NET Media-Dienste. Um auch mit .NET entwickeln erleichtern, werden die Bibliothek **Azure Media Services .NET SDK Erweiterungen** bereitgestellt. Diese Bibliothek enthält eine Reihe von Erweiterungsmethoden und Helper-Funktionen, die von .NET Code vereinfachen werden. Beide Bibliotheken sind über **NuGet** und **GitHub**verfügbar.


##<a name="prerequisites"></a>Erforderliche Komponenten

-   Ein in einem neuen oder vorhandenen Azure-Abonnement Media Services-Konto. Finden Sie im Thema [zum Erstellen eines Media Services-Kontos](media-services-portal-create-account.md)ein.
-   Betriebssysteme: Windows 10, Windows 7, Windows 2008 R2 oder Windows 8.
-   .NET Framework 4.5.
-    Visual Studio 2015, Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 SP1 (Professional, Premium, Ultimate oder Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Erstellen Sie und konfigurieren Sie einer Visual Studio-Projekt

In diesem Abschnitt wird gezeigt, wie zum Erstellen eines Projekts in Visual Studio und Media-Dienste Entwicklung eingerichtet werden kann.  In diesem Fall das Projekt besteht eine C#-Windows-Console-Anwendung, die gleichen Setupschritte hier gezeigten gelten jedoch für andere Arten von Projekten, die Sie für Media-Dienste Applikationen (beispielsweise Windows Forms-Anwendung oder eine Anwendung ASP.NET-Webdienste) erstellen können.

In diesem Abschnitt zeigt, wie Sie **NuGet** Media Services .NET SDK und anderen abhängigen Bibliotheken hinzufügen.

Alternativ können Sie rufen Sie die neuesten Media Services .NET SDK Bits in GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) und [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), erstellen Sie die Lösung und fügen Sie die Verweise des Projekts Client. Beachten Sie, dass alle die notwendigen Abhängigkeiten heruntergeladen und automatisch extrahiert abrufen.

1. Erstellen Sie eine neue c# in Visual Studio 2010 SP1 oder höher im Vergleich mit einer. Geben Sie den **Namen**, **Speicherort**und **Namen der Lösung**, und klicken Sie dann auf OK.

2. Erstellen Sie die Lösung.

2. Formular mit **NuGet** installieren und **Azure Media Services .NET SDK Erweiterungen**hinzufügen. Installation dieses Pakets, **Media Services.NET SDK** installiert und alle anderen erforderlichen Abhängigkeiten addiert.

    Stellen Sie sicher, dass Sie die neueste Version von NuGet installiert haben. Weitere Informationen und installieren benötigen finden Sie unter [NuGet](http://nuget.codeplex.com/).

2. Klicken Sie im Explorer-Lösung mit der rechten Maustaste in des Namens des Projekts, und wählen Sie verwalten NuGet-Pakete...

    Das Dialogfeld "NuGet-Pakete verwalten" angezeigt wird.

3. Im Katalog Online Azure MediaServices Erweiterungen suchen, Azure Media Services .NET SDK Erweiterungen auswählen, und klicken Sie dann auf die Schaltfläche installieren.

    Das Projekt wurde geändert und Verweise auf die Medien Services .NET SDK Erweiterungen, Media Services .NET SDK und andere abhängige Assemblys hinzugefügt werden.

4. Um eine übersichtlichere Entwicklungsumgebung hochstufen möchten, in Betracht NuGet-Paket wiederherstellen. Weitere Informationen finden Sie unter [NuGet-Paket wiederherstellen "](http://docs.nuget.org/consume/package-restore).

3. Fügen Sie einen Verweis auf **System.Configuration** Assembly hinzu. Diese Assembly enthält System.Configuration. **ConfigurationManager** -Klasse, die Zugriff auf Konfigurationsdateien (z. B. App.config) verwendet wird.

    Verweise mit dem Dialogfeld Verweise verwalten um hinzuzufügen, und mit der rechten Maustaste im Explorer-Lösung des Projektnamens. Wählen Sie dann hinzufügen und Verweise aus.

    Das Dialogfeld Verweise verwalten wird angezeigt.

4. Finden Sie unter .NET Framework-Assemblys die System.Configuration-Assembly, und klicken Sie auf OK.
5. Öffnen Sie die App (die Datei wird zu einem Projekt hinzufügen, wenn sie nicht standardmäßig hinzugefügt wurde), und fügen Sie einen Abschnitt *AppSettings* zu der Datei.     
Legen Sie die Werte für Ihren Azure Media Services Servername und die Kontoinformationen kontoschlüssel, wie im folgenden Beispiel dargestellt.

    Um den Namen und Schlüssel Werte zu finden, wechseln Sie zu der Azure-Portal, und wählen Sie Ihr Konto. Das Fenster Einstellungen wird auf der rechten Seite angezeigt. Wählen Sie im Fenster Einstellungen Schlüssel aus. Klicken auf das Symbol neben jedem Textfeld kopiert den Wert in die Zwischenablage des Systems.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Überschreiben Sie die vorhandenen **mithilfe von** Anweisungen am Anfang der Datei Program.cs mit den folgenden Code ein.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

Zu diesem Zeitpunkt sind Sie bereit sind, starten die Entwicklung einer Anwendung Media-Dienste.    


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
