<properties
   pageTitle="Einrichten Ihrer Entwicklungsumgebung auf Linux | Microsoft Azure"
   description="Installieren Sie die Laufzeit und SDK, und erstellen Sie einen lokale Entwicklung Cluster unter Linux. Nach Abschluss der diese Installation werden Sie zum Erstellen von Applications bereit."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Bereiten Sie Ihrer Entwicklungsumgebung auf Linux vor


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Bereitstellen und Ausführen von [Applications Azure Service Fabric](service-fabric-application-model.md) auf Ihrem Computer der Linux Entwicklung, installieren Sie die Laufzeit und allgemeine SDK. Sie können auch optional SDKs für Java und .NET Core installieren.

## <a name="prerequisites"></a>Erforderliche Komponenten
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssysteme
Die folgenden Betriebssystem-Versionen werden für die Entwicklung unterstützt:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Aktualisieren Sie Ihrer apt Quellen

Um das SDK und die zugehörigen Runtime-Paket über apt Get installieren zu können, müssen Sie zuerst Ihre apt Quellen aktualisieren.

1. Öffnen Sie einen Terminal.
2. Fügen Sie den Dienst Fabric Repo zu Ihrer Liste der Datenquellen aus.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Fügen Sie den neuen GPG Schlüssel in Ihren Schlüsselbund apt hinzu.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Aktualisieren Sie Ihre Paket Listen basierend auf den neu hinzugefügten Repositorys.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Installieren und Einrichten des SDKS

Nachdem Sie Ihrer Quellen aktualisiert werden, können Sie das SDK installieren.

1. Installieren Sie das Service Fabric SDK-Paket ein. Sie werden zur Bestätigung der Installations und einen Lizenzvertrag zustimmen aufgefordert.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Führen Sie das SDK Setup-Skript ein.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Einrichten von Azure Plattformen CLI

Die [Azure Plattformen CLI] [ azure-xplat-cli-github] Befehle für die Interaktion mit Service Fabric Personen, einschließlich Cluster und Applikationen enthält. Er basiert auf Node.js so [Stellen Sie sicher, dass Sie Knoten installiert haben] [ install-node] bevor Sie mit den folgenden Anweisungen.

1. Klonen der Repo Github auf Ihren Entwicklungscomputer an.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Wechseln Sie in den duplizierten Repo, und Installieren der CLI Abhängigkeiten mit dem Knoten Paket-Manager (Npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Erstellen Sie einen Symlink aus dem Papierkorb/Azure-Ordner, der den duplizierten Repo zu /usr/bin/azure ein, damit es für Ihre Pfad hinzugefügt wird, und Befehle aus dem Verzeichnis verfügbar sind.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Aktivieren Sie schließlich AutoVervollständigen Dienst Fabric Befehle aus.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Richten Sie eine lokale cluster

Wenn alles erfolgreich installiert wurde, sollten Sie einen lokalen Cluster starten sein.

1. Das Cluster Setup-Skript ausführen.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Öffnen Sie einen Webbrowser, und navigieren Sie zu Http://localhost:19080/Explorer. Wenn der Cluster gestartet hat, sollte das Dienst Fabric Explorer Dashboard angezeigt werden.

    ![Dienst Fabric Explorer unter Linux][sfx-linux]

An diesem Punkt können Sie vorgefertigte Fabric Service-Anwendung-Paketen oder neue basierend auf Gast Container oder Gast Programmdateien bereitstellen. Führen Sie zum Erstellen neuer Services mit dem Java oder .NET Core SDKs Installation optionale Schritte aus.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Installieren Sie das Plug-in Java SDK und Ellipse Neon (optional)

Die Java SDK bietet die Bibliotheken und Vorlagen, die zur Erstellung von Fabric Service-Diensten mit Java erforderlich.

1. Installieren Sie das SDK Java-Paket ein.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Führen Sie das SDK Setup-Skript ein.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Sie können die Ellipse-Plug-In für Dienst Fabric über die Ellipse Neon IDE installieren.

1. In "Ellipse" Stellen Sie sicher, dass Sie Buildship Version 1.0.17 oder höher installiert sein. Sie können die Versionen der installierten Komponenten überprüfen, indem Sie auf **Hilfe > Installation Details**. Sie können mithilfe der Anweisungen Buildship aktualisieren [hier][buildship-update].

2. Wenn Sie den Dienst Fabric-Plug-In installiert haben, wählen Sie aus **Hilfe > neue Software installieren**

3. Geben Sie im Textfeld "Arbeiten mit": http://dl.windowsazure.com/eclipse/servicefabric

4. Klicken Sie auf Hinzufügen.

    ![Ellipse-Plug-Ins][sf-eclipse-plugin]

5. Wählen Sie den Dienst Fabric-Plug-in, und klicken Sie auf Weiter.

6. Führen Sie die Schritte der Installations und dem Endbenutzer-Lizenzvertrag.

## <a name="install-the-net-core-sdk-optional"></a>Installieren Sie das .NET Core SDK (optional)

.NET Core SDK stellt die Bibliotheken und Vorlagen, die zur Erstellung von Service Fabric-Diensten mit Plattformen .NET Core erforderlich.

1. Installieren Sie das .NET Core SDK-Paket ein.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Führen Sie das SDK Setup-Skript ein.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihrer erste Java-Anwendung unter Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [Bereiten Sie Ihrer Entwicklungsumgebung auf OSX vor](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
