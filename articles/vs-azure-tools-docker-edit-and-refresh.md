<properties
   pageTitle="Für das Debuggen apps in einem lokalen Docker Container | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine app zu ändern, die in einem lokalen Docker Container ausgeführt wird, aktualisieren Sie den Container über bearbeiten und aktualisieren und Festlegen von Debughaltepunkten"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Für das Debuggen apps in einem lokalen Docker container

## <a name="overview"></a>(Übersicht)
Visual Studio-Tools für Docker bietet ein konsistentes Verfahren zum Entwickeln in ein, und überprüfen Sie die Anwendung lokal in einem Linux Docker Container.
Sie müssen nicht den Container jedes Mal neu gestartet, die einen Code ändern vorgenommen werden.
In diesem Artikel wird zum Verwenden des Features "Bearbeiten und aktualisieren" zum Starten einer ASP.NET Core Web-app in einem lokalen Docker Container, nehmen Sie alle erforderlichen Änderungen vor, und aktualisieren Sie den Browser, um diese Änderungen finden Sie unter veranschaulichen.
Es wird auch gezeigt, wie für das Debuggen Haltepunkte festgelegt werden.

> [AZURE.NOTE] Windows-Container-Unterstützung wird in zukünftigen Versionen in Kürze werden

## <a name="prerequisites"></a>Erforderliche Komponenten
Die folgenden Tools installiert werden müssen.

- [Visual Studio 2015 Update 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Installieren Sie [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Zum Ausführen von Docker Container lokal, benötigen einen lokalen Docker Client Sie.
Veröffentlichte [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox) einzugeben Hyper-V deaktiviert werden können, oder Sie können [Docker für Windows Beta](https://beta.docker.com) Hyper-V verwendet und erfordert Windows 10 verwenden.

Wenn Docker Toolbox verwenden zu können, müssen Sie Sie zum [Konfigurieren des Docker-Clients](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. erstellen Sie 1. eine Web app

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Fügen Sie Docker Support hinzu

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3 bearbeiten Sie 3 Ihre Code und aktualisieren

Um schnell Änderungen durchlaufen, können Sie Ihrer Anwendung innerhalb eines Containers starten und weiterhin vorzunehmen, wie bei IIS Express Betrachter.

1. Legen Sie die Lösung-Konfiguration auf `Debug` , und drücken Sie ** &lt;STRG + F5 >** Docker Bild erstellen und lokal ausführen.

    Nachdem das Bild Container erstellt wurde und in einem Container Docker ausgeführt wird, wird in Visual Studio das Web app in Ihrem Standardbrowser gestartet.
    Wenn Sie Microsoft Edge Browser verwenden oder andernfalls Fehler auf, siehe Abschnitt " [Problembehandlung treten](vs-azure-tools-docker-troubleshooting-docker-errors.md) ".

1. Wechseln Sie zu der Seite "Info", also wo wir unsere Änderungen vornehmen.

1. Kehren Sie zu Visual Studio zurück, und öffnen Sie `Views\Home\About.cshtml`.

1. Fügen Sie den folgenden HTML-Inhalt an das Ende der Datei, und speichern Sie die Änderungen zu.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Anzeige von im Ausgabefenster angezeigt, wenn der .NET Build abgeschlossen ist, und diese Zeilen angezeigt wird, wechseln Sie zurück zu Ihrem Browser und aktualisieren Sie die Seite "Info".

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Die Änderungen wurden angewendet!

## <a name="4-debug-with-breakpoints"></a>4. Debuggen Sie mit Haltepunkten

Häufig werden Änderungen weitere Prüfung, Nutzung von den Features von Visual Studio debuggen benötigen.

1.  Kehren Sie zu Visual Studio und öffnen`Controllers\HomeController.cs`

1.  Ersetzen Sie den Inhalt der About() Methode durch den folgenden Code ein:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Festlegen einer fortzuschreiten links neben der `string message`... Linie.

1.  Drücken Sie ** &lt;F5 >** für das Debuggen.

1.  Navigieren Sie zu der Seite "Info" auf Ihrem fortzuschreiten Treffer.

1.  Wechseln Sie zu Visual Studio für die fortzuschreiten anzeigen, und prüfen Sie den Wert der Nachricht.

    ![][2]

##<a name="summary"></a>Zusammenfassung

Mit [Visual Studio 2015 Tools für Docker](https://aka.ms/DockerToolsForVS)können Sie die Produktivität lokal, arbeiten mit realistische Herstellung der Entwicklung innerhalb eines Containers Docker erhalten.

## <a name="troubleshooting"></a>Behandlung von Problemen

[Problembehandlung bei der Entwicklung von Visual Studio Docker](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Weitere Informationen zu Docker mit Visual Studio und Windows Azure

- [Docker Tools für Visual Studio](http://aka.ms/dockertoolsforvs) - Entwicklung von .NET Core-Code in einem container
- [Docker Tools für Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – erstellen und Bereitstellen von Docker Container
- [Docker Tools für Visual Studio-Code](http://aka.ms/dockertoolsforvscode) - Sprachdienste zur Bearbeitung Docker-Dateien mit mehr e2e-Szenarien in Kürze
- [Informationen zu Windows Container](http://aka.ms/containers)- Windows Server und Nano Serverinformationen
- [Container Azure Service](https://azure.microsoft.com/services/container-service/) - [Container Azure Service Inhalt](http://aka.ms/AzureContainerService)
-    Weitere Beispiele für die Arbeit mit Docker finden Sie unter [Arbeiten mit Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) von [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinden. Weitere Schnellstart aus HealthClinic.biz anschauen finden Sie unter [Azure Developer Tools Schnellstart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Verschiedene Docker-tools

[Einige großartige Docker-Tools (Blog des Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Gute Artikel

[Einführung in Microservices von NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Präsentationen

- [Steve Lasker: Im Vergleich mit einer Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Einführung in ASP.NET Core @ 2016 -, wo Sie am Demo erstellen](https://channel9.msdn.com/Events/Build/2016/B810)
- [Entwicklung von apps von .NET in Containern, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
