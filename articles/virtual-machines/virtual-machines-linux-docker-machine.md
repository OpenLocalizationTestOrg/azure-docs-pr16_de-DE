<properties
    pageTitle="Erstellen von Docker Hosts in Azure mit Docker Computer | Microsoft Azure"
    description="Beschreibt die Verwendung des Docker Computers Docker Hosts in Azure zu erstellen."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Verwenden Sie Docker Computer mit der Azure-Treiber

[Docker](https://www.docker.com/) ist eine der am häufigsten verwendeten Virtualisierung Vorgehensweisen, die als eine Möglichkeit zum Isolieren Anwendungsdaten, und klicken Sie auf freigegebene Ressourcen computing Linux Container anstelle von virtuellen Computern verwendet. Dieses Thema wird beschrieben, wann und wie Sie [Docker Computer](https://docs.docker.com/machine/) verwenden (der `docker-machine` Befehl) So erstellen Sie neue Linux virtuellen Computern in Azure als Host für Ihre Linux Container Docker aktiviert.


## <a name="create-vms-with-docker-machine"></a>Erstellen von virtuellen Computern mit Docker Computer

Virtuelle Computer erstellen Docker Host in Azure mit den `docker-machine create` Befehl mithilfe der `azure` Treiber Argument für die Treiberoption (`-d`) sowie alle anderen Argumente. 

Im folgende Beispiel basiert, auf die Standardwerte, aber es wird geöffnet, Port 80 des virtuellen Computers mit dem Internet mit einem Nginx-Container ist testen `ops` den Benutzer Anmeldung für SSH und den neuen virtuellen Computer-Anrufe `machine`. 

Typ `docker-machine create --driver azure` sehen Sie die Optionen und ihre Standardwerte; Sie können auch die [Docker Azure-Treiber Dokumentation](https://docs.docker.com/machine/drivers/azure/)lesen. (Beachten Sie, wenn Sie einer zweistufigen Authentifizierung aktiviert haben, werden aufgefordert, wird für die Authentifizierung mithilfe des zweiten Faktors).

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Die Ausgabe sollte ungefähr wie folgt aussehen, je nachdem, ob Sie Authentifizierung mit zwei Faktoren in Ihr Konto konfiguriert haben.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Konfigurieren der Docker-Verwaltungsshell

Geben Sie nun `docker-machine env <VM name>` sehen, was Sie tun müssen, um die Verwaltungsshell konfigurieren. 

```bash
docker-machine env machine
```

Die druckt die Umgebungsinformationen preiszugeben, die sieht ungefähr wie folgt aus. Beachten Sie, dass die IP-Adresse zugewiesen wurde die müssen Sie den virtuellen Computer zu testen.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Sie können entweder den Befehl empfohlene Konfiguration ausführen, oder Sie können die Umgebungsvariablen selbst festlegen. 

## <a name="run-a-container"></a>Ausführen eines Containers

Nachdem Sie einen einfachen Webserver zum Testen ausgeführt werden können, ob alles ordnungsgemäß funktioniert. Hier wir verwenden eines Bilds standard Nginx, angeben, dass sie Port 80 überwachen soll, und, wenn Sie der virtuellen Computer neu gestartet wird, der Container sollte neu ebenfalls (`--restart=always`). 

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

Und überprüfen Sie den laufenden Container, geben finden Sie unter `docker-machine ip <VM name>` finden Sie die IP-Adresse (Wenn Sie vergessen, aus der `env` Befehl):

![Laufende Ngnix container](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie interessiert sind, können Sie der Azure [Docker virtueller Computer Erweiterung](virtual-machines-linux-dockerextension.md) für den gleichen Vorgang mithilfe der Azure-CLI oder Azure Ressource-Manager Vorlagen führen ausprobieren. 

Weitere Beispiele für die Arbeit mit Docker finden Sie unter [Arbeiten mit Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) von [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinden. Weitere Schnellstart aus HealthClinic.biz anschauen finden Sie unter [Azure Developer Tools Schnellstart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

