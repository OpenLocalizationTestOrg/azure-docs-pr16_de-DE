<properties
   pageTitle="Konfigurieren einen Docker Host mit VirtualBox | Microsoft Azure"
   description="Eine schrittweise Anleitung zum Konfigurieren einer Standardinstanz-Docker mit Docker Computer- und VirtualBox"
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
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Konfigurieren eines Docker Hosts mit VirtualBox

## <a name="overview"></a>(Übersicht)
In diesem Artikel führt Sie durch die Konfiguration einer Standardinstanz-Docker Docker Computer- und VirtualBox verwenden. Wenn Sie die [Docker für Windows Beta](http://beta.docker.com/)verwenden, ist diese Konfiguration nicht erforderlich.

## <a name="prerequisites"></a>Erforderliche Komponenten
Die folgenden Tools installiert werden müssen.

- [Toolbox docker](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Konfigurieren des Docker-Clients mit Windows PowerShell

Zum Konfigurieren eines Docker Clients einfach öffnen Sie Windows PowerShell, und führen Sie die folgenden Schritte aus:

1. Erstellen einer Standardinstanz von Docker Host an.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Stellen Sie sicher, dass die Standardinstanz konfiguriert ist und ausgeführt wird. (Es sollte eine Instanz mit dem Namen 'Default' Ausführung angezeigt.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![Docker-Computer ls Ausgabe][0]
 
1. Als dem aktuellen Host festlegen und Ihre Shell konfigurieren.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Anzeigen der aktiven Docker Container. Die Liste sollte leer sein.

    ```PowerShell
    docker ps
    ```

    ![Docker Ps Ausgabe][1]
 
> [AZURE.NOTE]Jedes Mal Sie Ihre Entwicklungscomputer neu starten, müssen Sie Ihrem lokalen Docker Host neu starten.
> Führen Sie hierzu die folgenden Befehl über die Befehlszeile: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
