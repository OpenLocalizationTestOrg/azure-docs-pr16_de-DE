<properties
   pageTitle="Erstellen Ihrer erste Fabric Service-Anwendung unter Linux mit c# | Microsoft Azure"
   description="Erstellen und Bereitstellen einer Fabric Service-Anwendungs, die mit C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Erstellen Sie Ihrer erste Azure Service Fabric Anwendung

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Fabric-Dienst bietet SDKs zum Erstellen von Diensten auf Linux in .NET Core und Java. In diesem Lernprogramm betrachten wir so erstellen eine Anwendung für Linux und Erstellen eines Diensts mithilfe von c# (.NET Core) ein.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, stehen Ihnen [Einrichten Ihrer Linux Entwicklung-Umgebung](service-fabric-get-started-linux.md). Wenn Sie Mac OS X verwenden, können Sie [eine Linux ein-Feld-Umgebung auf einem virtuellen Computer mit Vagrant einrichten](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Erstellen Sie die Anwendung

Eine Fabric Service-Anwendung kann einen oder mehrere Dienste, jede mit einer bestimmten Rolle in Vorführung der Anwendungsfunktionalität enthalten. Der Dienst Fabric SDK für Linux enthält einen [Yeoman](http://yeoman.io/) -Generator, der auf einfache Weise zum Erstellen Ihrer ersten Diensts und mehr zu einem späteren Zeitpunkt hinzufügen kann. Verwenden Sie uns Yeoman, um eine Anwendung mit einem einzelnen Dienst zu erstellen.

1. Geben Sie in einer Terminal zum Zuverlässigkeit das Gerüst den folgenden Befehl aus:`yo azuresfcsharp`

2. Nennen Sie die Anwendung.

3. Wählen Sie den Typ der dem ersten Dienst aus, und nennen Sie es. Wählen Sie für die Zwecke dieses Lernprogramms wir einer zuverlässigen Akteur-Dienst aus.

  ![Dienst Fabric Yeoman-Generator für C#][sf-yeoman]

>[AZURE.NOTE] Weitere Informationen zu den Optionen finden Sie unter [Service Fabric programming Übersicht über das Objektmodell](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Erstellen Sie die Anwendung

Die Dienst Fabric Yeoman-Vorlagen enthalten ein Skript erstellen, die Sie verwenden können, um die app aus dem Terminal (nach Navigieren zu dem Anwendungsordner) erstellen.

  ```bash
 cd myapp 
 ./build.sh 
  ```

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

## <a name="start-the-test-client-and-perform-a-failover"></a>Starten Sie den Testclient, und führen Sie einen failover

Akteur Projekte nicht auf eigene nichts ausgewählt werden. Sie benötigen einen anderen Dienst oder Client, Nachrichten zu senden. Die Akteur-Vorlage enthält ein einfacher Test-Skript, die Sie für die Interaktion mit dem Akteur-Dienst verwenden können.

1. Führen Sie das Skript, die mit dem Programm anzeigen die Ausgabe der Akteur-Dienst finden Sie unter.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie im Dienst Fabric-Explorer Knoten Hostinganbieter primäre Replikat für den Akteur-Dienst aus. In den folgenden Screenshot ist es Knoten 3.

    ![Suchen in Service Fabric Explorer primäre Replikat][sfx-primary]

3. Klicken Sie auf den Knoten, den Sie im vorherigen Schritt gefunden, und wählen Sie dann **deaktivieren (Neustart)** über das Menü Aktionen. Diese Aktion wird einer der fünf Knoten neu gestartet in Ihrem lokalen Cluster Erzwingen eines Failovers an einer sekundären auf einem anderen Knoten ausgeführt wird. Wenn Sie diese Aktion ausführen, achten Sie auf die Ausgabe aus dem Testclient und die Notiz, die der Zähler weiterhin trotz der Failover zu erhöhen.


## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md)
- [Interagieren mit Service Fabric Cluster mithilfe der Azure-CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
