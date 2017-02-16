<properties
   pageTitle="Erste Schritte mit Docker mit Punktschwarms auf Azure"
   description="Beschreibt, wie Sie eine Gruppe von virtuellen Computern mit der Erweiterung Docker virtueller Computer erstellen und Punktschwarms um einen Docker Cluster zu erstellen."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Verwenden von Docker mit Punktschwarms

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In diesem Thema wird eine sehr einfache Möglichkeit, [Docker](https://www.docker.com/) mit [swarm](https://github.com/docker/swarm) verwenden, um einen Cluster Punktschwarms verwaltete auf Azure zu erstellen. Vier virtuellen Computern erstellt in Azure, eine fungiert als Manager Punktschwarms und drei als Teil der Cluster Docker Hosts. Wenn Sie fertig sind, können Sie Punktschwarms finden Sie unter Cluster, und beginnen Sie mit Docker daran. Darüber hinaus verwenden Azure CLI Anrufe in diesem Thema den Dienst Management (Asm) Modus. 

> [AZURE.NOTE] In diesem Thema verwendet Docker mit Punktschwarms und der Azure CLI *ohne* **Docker-Computer** verwenden, um anzuzeigen, wie verschiedene Tools zusammenarbeiten, aber unabhängig bleiben. **Docker-Computer** weist **– swarm** Schalter, mit denen Sie **Docker-Computer** verwenden, auf einen Knoten direkt hinzufügen können. Ein Beispiel finden Sie in der Dokumentation [Docker-Computer](https://github.com/docker/machine) . Wenn Sie **Docker-Computer** mit Azure-virtuellen Computern ausführen verpasst haben, finden Sie unter [Docker-Computer mit Azure verwenden](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Erstellen von Docker Hosts mit Azure virtuellen Computern

In diesem Thema erstellt vier virtuellen Computern, aber Sie können eine beliebige Nummer, die Sie verwenden. Rufen Sie die folgenden mit * &lt;Kennwort&gt; * ersetzt werden, indem Sie das Kennwort ein, die Sie ausgewählt haben.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Wenn Sie fertig sind, sollten Sie **Azure-virtuellen Computer Liste** verwenden, um Ihre Azure-virtuellen Computern finden Sie unter sein:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Installieren von Punktschwarms auf den Folienmaster Punktschwarms virtueller Computer

In diesem Thema verwendet [Container Modell der Installation von der Docker swarm Dokumentation](https://github.com/docker/swarm#1---docker-image) – aber könnten Sie auch SSH an den **Punktschwarms-Master-Shape**. In diesem Modell wird **swarm** als Docker Container ausgeführt Punktschwarms heruntergeladen. Führen Sie anhand der wir dieser Schritt *Remote aus unserem Laptop mit Docker* um eine Verbindung mit der virtuellen Computer **Punktschwarms-Master** und anweisen, **Punktschwarms erstellen**Cluster Id Erstellungsbefehl verwenden. Die Cluster-Id ist, wie die Mitglieder der Gruppe Punktschwarms erkennt **swarm** . (Sie können auch Klonen Repository und erstellen sie selbst, die vollständige Steuerung und aktivieren Sie das Debuggen wird.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Die letzte Zeile ist die Id Cluster; Kopieren sie dort, da Sie ihn beim Verknüpfen von des Knotens virtuellen Computern an den Punktschwarms Master zum Erstellen des "Punktschwarms" erneut verwenden möchten. In diesem Beispiel ist die Cluster-Id **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Einfach, um die deutlich zu sagen, verwenden wir unserer Installation lokale Docker Verbindung zum **Punktschwarms-Master** virtueller Computer in Azure und Anweisung **Punktschwarms-Master** zum Herunterladen, installieren, und führen Sie den Befehl **Erstellen** , der unsere Cluster-Id zurückgibt, den wir später für die Suche verwenden.
<!-- -->
> Um dies zu überprüfen, ausführen `docker -H tcp://` * &lt;Hostname&gt; * ` images` zu die Containers Prozessen, die auf dem Computer **Punktschwarms-Master-Shape** , und klicken Sie auf einen anderen Knoten für den Vergleich Liste (da wir haben den vorherigen Punktschwarms Befehl mit dem Schalter **– Rm** ausgeführt, der Container wurde entfernt, nachdem sie den Vorgang abgeschlossen haben, also mit **Docker Ps - ein** nichts zurückgeben wird nicht).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Wenn Sie mit **Docker**vertraut sind, wissen Sie, dass die anderen Knoten keine Einträge aufweisen, da keine Bilder heruntergeladen und noch ausgeführt wurde.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Verbinden des Knotens virtuellen Computern mit unseren Docker cluster

Listen Sie für jeden Knoten Endpunktinformationen über den mit der CLI Azure ein. Weiter unten tun wir, die für den **Punktschwarms-Knoten-1** Docker Host um des Knotens Docker Port zu erhalten.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Mithilfe von **Docker** und die `-H` Option, um den Docker Client bei Ihrem Knoten virtueller Computer, zeigen Sie Verknüpfung diesen Knoten des Punktschwarms Sie sind erstellen, indem Sie die Cluster-Id und des Knotens Docker Port übergeben (letztere **– Kontaktliste**verwenden):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Das sieht nicht schlecht. Um zu bestätigen, dass die **swarm** auf **Punktschwarms-Knoten-1** ausgeführt wird, den wir eingeben:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Wiederholen Sie für alle anderen Knoten im Cluster. In diesem Fall nicht wir, **Punktschwarms-Knoten-2** und **Punktschwarms Knoten 3**ein.

## <a name="begin-managing-the-swarm-cluster"></a>Beginnen Sie mit der Verwaltung von Punktschwarms cluster

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

und Sie können Sie Ihre-Knoten auflisten, in Ihren Cluster:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie die Dinge auf Ihre Punktschwarms ausgeführt werden. Wenn Beispielvorlagen leiten lassen suchen möchten, finden Sie unter [https://github.com/docker/swarm/](https://github.com/docker/swarm/)oder vielleicht ein [video](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
