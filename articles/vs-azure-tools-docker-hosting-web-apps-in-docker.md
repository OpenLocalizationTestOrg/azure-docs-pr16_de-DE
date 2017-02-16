<properties
   pageTitle="Bereitstellen einen ASP.NET Core Linux Docker Container mit einem entfernten Docker Host | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit Visual Studio-Tools für Docker bereitstellen eine ASP.NET Core Web app zu einem Docker Container Ausführen einer Azure Docker Host Linux virtuellen Computers."   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Bereitstellen eines Containers ASP.NET mit einem entfernten Docker host

## <a name="overview"></a>(Übersicht)
Docker ist eine einfache Container-Engine, ähnlich wie im folgenden Verfahren können Sie eine virtuellen Computern, die Sie zum Host-Anwendungen und Dienste verwenden können.
In diesem Lernprogramm führt Sie durch die Erweiterung [Visual Studio 2015 Tools für Docker](http://aka.ms/DockerToolsForVS) bereitstellen eine ASP.NET Core-app mit einem Docker Host Azure mithilfe der PowerShell verwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten
Folgendes ist erforderlich, zum Bearbeiten dieses Lernprogramms:

- Erstellen Sie eine Azure Docker Host virtueller Computer im [Umgang mit Docker-Computer mit Azure](./virtual-machines/virtual-machines-linux-docker-machine.md) beschriebenen
- Installieren Sie [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- Installieren Sie [Visual Studio 2015 Tools für Docker - Vorschau](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Erstellen einer ASP.NET Core Web app
Die folgenden Schritte führt Sie durch die Erstellung einer einfachen ASP.NET Core app, die in diesem Lernprogramm verwendet werden soll.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Fügen Sie Docker Support hinzu

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3 verwenden Sie 3 die DockerTask.ps1 PowerShell-Skript 

1.  Öffnen Sie eine PowerShell Aufforderung zum Stammverzeichnis Ihres Projekts. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Überprüfen der Remote-Host ausgeführt wird. Sollten Sie den Zustand erkennen = Ausführung 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Wenn Sie die Betaversion Docker verwenden, wird nicht der Host hier aufgeführt.

1.  Erstellen der app arbeiten mit dem Parameter erstellen

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Wenn Sie die Betaversion Docker verwenden, Auslassen des Arguments Computer
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Führen Sie die app, mit dem Parameter ausführen

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Wenn Sie die Betaversion Docker verwenden, Auslassen des Arguments Computer
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Nachdem Docker abgeschlossen ist, sollte ähnlich wie der folgende Ergebnisse angezeigt:

    ![Anzeigen der app][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
