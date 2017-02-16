<properties
   pageTitle="Erstellen von Docker Hosts in Azure mit Docker Computer | Microsoft Azure"
   description="Beschreibt die Verwendung des Docker Computers Docker Hosts in Azure zu erstellen."
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

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Erstellen von Docker Hosts in Azure mit Docker-Computer

Ausführen von [Docker](https://www.docker.com/) Container erfordert einen Host virtuelle Computer den Docker Daemon ausgeführt.
In diesem Thema beschrieben, wie Sie mithilfe des Befehls [Docker-Computer](https://docs.docker.com/machine/) erstellen neue virtuelle Linux Computer, mit der Ausführung im Azure Docker Administrationsdaemon konfiguriert wird. 

**Hinweis:** 
- *In diesem Artikel hängt Docker-Computer Version 0.7.0 oder höher*
- *Windows-Container wird durch Docker-Computer in Kürze unterstützt.*

## <a name="create-vms-with-docker-machine"></a>Erstellen von virtuellen Computern mit Docker Computer

Erstellen von Docker Host virtuellen Computern in Azure mit der `docker-machine create` Befehl mit der `azure` Treiber. 

Der Azure-Treiber benötigen Ihr Abonnement-ID an. Die [Azure CLI](xplat-cli-install.md) oder der [Azure-Portal](https://portal.azure.com) können Sie Ihr Abonnement Azure abzurufen. 

**Verwenden des Azure-Portals**
- Wählen Sie im linken Navigationsbereich Seite Abonnements aus, und Kopieren in die Abonnement-Id.

**Verwendung der Azure CLI**
- Typ ```azure account list``` , und kopieren Sie die Abonnement-Id.

Typ `docker-machine create --driver azure` um die Optionen und ihre Standardwerte anzuzeigen.
Sie können auch die [Dokumentation Docker Azure-Treiber](https://docs.docker.com/machine/drivers/azure/) für Weitere Informationen finden Sie unter. 

Im folgende Beispiel basiert, auf die Standardwerte, aber es optional Port 80 des virtuellen Computers für den Zugriff auf das Internet zu öffnen. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Wählen Sie einen Host Docker mit Docker-Computer
Sobald Sie einen Eintrag für Ihre Host Docker-Computer haben, können Sie den standardmäßigen Host festlegen, wenn Docker Befehle ausgeführt.
##<a name="using-powershell"></a>Mithilfe der PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Verwenden von Bash

```bash
eval $(docker-machine env MyDockerHost)
```

Sie können jetzt Docker Befehle für den angegebenen Host ausführen.

```
docker ps
docker info
```

## <a name="run-a-container"></a>Ausführen eines Containers

Mit einem Host so konfiguriert ist können Sie nun einen einfache Webserver zu testen, ob Ihr Host ordnungsgemäß konfiguriert wurden ausführen.
Hier wir verwenden eines Bilds standard Nginx, angeben, dass sie Port 80 überwachen soll, und, wenn der Host virtueller Computer neu gestartet wurde, wird der Container neu ebenfalls (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Die Ausgabe sollte nun wie folgt aussehen:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testen des Containers

Untersuchen der laufenden Container mit `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Und überprüfen Sie den laufenden Container, geben finden Sie unter `docker-machine ip <VM name>` finden Sie die IP-Adresse im Browser eingeben:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Laufende Ngnix container](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Zusammenfassung
Mit Docker-Computer können Sie problemlos Docker Hosts in Azure für Ihre einzelne Docker Host Validierungen bereitstellen.
Herstellung Hosten von Containern, finden Sie unter den [Azure Container Service](http://aka.ms/AzureContainerService)

Bei der Entwicklung .NET Core Applications mit Visual Studio finden Sie unter [Docker Tools für Visual Studio](http://aka.ms/DockerToolsForVS)
