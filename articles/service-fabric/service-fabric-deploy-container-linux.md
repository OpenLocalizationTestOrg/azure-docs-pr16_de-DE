<properties
   pageTitle="Service Fabric und Bereitstellen von Containern in Linux | Microsoft Azure"
   description="Bereitstellen von Applications Microservice Dienst Fabric und die Verwendung von Docker Container. In diesem Artikel werden die Funktionen, die Dienst Fabric für Container enthält und zum Bereitstellen einer Docker Container Bild in einem Cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Bereitstellen eines Containers Docker mit Fabric Service

> [AZURE.SELECTOR]
- [Bereitstellen von Windows-Container](service-fabric-deploy-container.md)
- [Bereitstellen von Docker Container](service-fabric-deploy-container-linux.md)

In diesem Artikel führt Sie durch das Erstellen von Sammelartikeleinheit Diensten in Docker Container unter Linux.

Dienst Fabric weist verschiedene Container-Funktionen, mit die Hilfe Sie mit der Erstellung von Microservices, die in Containern werden bestehen Applications. Diese werden Sammelartikeleinheit Services genannt.

Die Funktionen umfassen.

- Container Image-Bereitstellung und-Aktivierung
- Ressource governance
- Repository-Authentifizierung
- Container Port Host Port-Zuordnung
- Containern, Suche und Kommunikation
- Möglichkeit zum Konfigurieren und Einrichten der Umgebungsvariablen


## <a name="packaging-a-docker-container-with-yeoman"></a>Verpacken eines Docker Containers mit yeoman
Beim eines Containers auf Linux packen, können Sie entweder eine Yeoman Vorlage oder [Erstellen Sie das Anwendungspaket manuell](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container)verwenden auswählen.

Eine Fabric Service-Anwendung kann eine oder mehrere Container, jede mit einer bestimmten Rolle in Vorführung der Anwendungsfunktionalität enthalten. Der Dienst Fabric SDK für Linux enthält einen [Yeoman](http://yeoman.io/) -Generator, der eine Anwendung erstellen und Hinzufügen eines Containers erleichtert. Verwenden Sie uns Yeoman zum Erstellen einer neuen Anwendungs mit einem einzelnen Docker Container *SimpleContainerApp*bezeichnet. Sie können weitere Dienste später Dateien durch Bearbeiten der generierten manifest hinzufügen.

## <a name="create-the-application"></a>Erstellen Sie die Anwendung

1. Geben Sie in einer Terminal **yo Azuresfguest**.

2. Wählen Sie für das Framework **Container** aus.

3. Nennen Sie die Anwendung z. B. SimpleContainerApp

4. Geben Sie die URL für das Bild Container aus einer DockerHub Repo aus. Sie hat das Format [Repo] / [image Name]

![Dienst Fabric Yeoman-Generator für Container][sf-yeoman]

## <a name="deploy-the-application"></a>Bereitstellen der Anwendungs

Nachdem die Anwendung erstellt wurde, können Sie es auf dem lokalen Cluster mithilfe der CLI Azure bereitstellen.

1. Verbinden Sie mit dem lokalen Dienst Fabric Cluster.

    ```bash
    azure servicefabric cluster connect
    ```

2. Verwenden Sie in der Vorlage bereitgestellte Skript für die Installation Anwendungspaket an die Cluster Bild Store kopieren, den Anwendungstyp registrieren, und erstellen Sie eine Instanz der Anwendung.

    ```bash
    ./install.sh
    ```

3. Öffnen Sie einen Browser, und navigieren Sie zum Dienst Fabric Explorer, am Http://localhost:19080/Explorer (ersetzen Localhost mit der privaten IP-von den virtuellen Computer, wenn Vagrant unter Mac OS X verwenden).

4. Erweitern Sie den Knoten Applikationen und beachten Sie, dass nun einen Eintrag für Ihre Anwendungstyp und einen anderen für die erste Instanz dieses Typs vorhanden ist.

5. Verwenden Sie das in der Vorlage bereitgestellte deinstallieren Skript Instanz der Anwendung löschen und die Anwendung aufgehoben werden.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Dienst Fabric und Container](service-fabric-containers-overview.md)
- [Interagieren mit Service Fabric Cluster mithilfe der Azure-CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

