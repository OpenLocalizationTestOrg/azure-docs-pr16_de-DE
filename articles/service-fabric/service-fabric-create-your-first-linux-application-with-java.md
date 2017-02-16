<properties
   pageTitle="Erstellen Ihrer erste Fabric Service-Anwendung unter Linux mit Java | Microsoft Azure"
   description="Erstellen und Bereitstellen einer mit Java Fabric Service-Anwendung"
   services="service-fabric"
   documentationCenter="java"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="seanmck"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Erstellen Sie Ihrer erste Azure Service Fabric-Anwendung

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Fabric-Dienst bietet SDKs zum Erstellen von Diensten auf Linux in .NET Core und Java. In diesem Lernprogramm betrachten wir so erstellen eine Anwendung für Linux und Erstellen eines Diensts mithilfe von Java.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, stehen Ihnen [Einrichten Ihrer Linux Entwicklung-Umgebung](service-fabric-get-started-linux.md). Wenn Sie Mac OS X verwenden, können Sie [eine Linux ein-Feld-Umgebung auf einem virtuellen Computer mit Vagrant einrichten](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Erstellen Sie die Anwendung

Eine Fabric Service-Anwendung kann einen oder mehrere Dienste, jede mit einer bestimmten Rolle in Vorführung der Anwendungsfunktionalität enthalten. Der Dienst Fabric SDK für Linux enthält einen [Yeoman](http://yeoman.io/) -Generator, der den ersten Dienst erstellen und später weitere Aufgaben hinzufügen erleichtert. Verwenden Sie uns Yeoman zum Erstellen einer neuen Anwendungs mit einem einzelnen Dienst an.

1. Geben Sie in einer Terminal **yo Azuresfjava**.

2. Nennen Sie die Anwendung.

3. Wählen Sie den Typ der dem ersten Dienst aus, und nennen Sie es. Für die Zwecke dieses Lernprogramms wählen wir einer zuverlässigen Akteur-Dienst.

  ![Dienst Fabric Yeoman-Generator für Java][sf-yeoman]

>[AZURE.NOTE] Weitere Informationen zu den Optionen finden Sie unter [Service Fabric programming Übersicht über das Objektmodell](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Erstellen Sie die Anwendung

Die Dienst Fabric Yeoman Vorlagen umfassen ein Skript erstellen für [Gradle](https://gradle.org/), die Sie verwenden können, um die app aus dem Terminal zu erstellen.

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Bereitstellen der Anwendungs

Nachdem die Anwendung erstellt wurde, können Sie es auf dem lokalen Cluster mithilfe der CLI Azure bereitstellen.

1. Verbinden Sie mit dem lokalen Dienst Fabric Cluster.

    ```bash
    azure servicefabric cluster connect
    ```

2. Formular mit Skript für die Installation, die Sie in der Vorlage bereitgestellten Anwendungspaket an die Cluster Bild Store kopieren, den Anwendungstyp registrieren, und erstellen Sie eine Instanz der Anwendung.

    ```bash
    ./install.sh
    ```

3. Öffnen Sie einen Browser, und navigieren Sie zum Dienst Fabric Explorer, am Http://localhost:19080/Explorer (ersetzen Localhost mit der privaten IP-von den virtuellen Computer bei Verwendung von Vagrant unter Mac OS X).

4. Erweitern Sie den Knoten Applikationen und beachten Sie, dass nun einen Eintrag für Ihre Anwendungstyp und einen anderen für die erste Instanz dieses Typs vorhanden ist.

## <a name="start-the-test-client-and-perform-a-failover"></a>Starten Sie den Testclient, und führen Sie einen failover

Akteur Projekte nicht auf eigene nichts ausgewählt werden. Sie benötigen einen anderen Dienst oder Client, Nachrichten zu senden. Die Akteur-Vorlage enthält einen einfachen Test-Skript, die Sie verwenden können, um die Interaktion mit dem Dienst Akteur.

1. Führen Sie das Skript, die mit dem Programm anzeigen die Ausgabe der Akteur-Dienst finden Sie unter.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie im Dienst Fabric-Explorer Knoten Hostinganbieter primäre Replikat für den Akteur-Dienst aus. Im folgenden Bildschirmfoto ist es Knoten 3.

    ![Suchen in Service Fabric Explorer primäre Replikat][sfx-primary]

3. Klicken Sie auf den Knoten, den Sie im vorherigen Schritt gefunden, und wählen Sie dann **deaktivieren (Neustart)** über das Menü Aktionen. Starten Sie einen der fünf Knoten in Ihrem lokalen Cluster dadurch und erzwingen Sie ein Failover auf einen der sekundäre Replikate auf einem anderen Knoten ausgeführt wird. Wie Sie dies tun, achten Sie auf die Ausgabe der Desktopclient testen und beachten Sie, dass der Zähler wird weiter trotz der Failover erhöht.

## <a name="build-and-deploy-an-application-with-the-eclipse-neon-plugin"></a>Erstellen und Bereitstellen einer Anwendung mit der Ellipse Neon-Plug-Ins

Wenn Sie den Dienst-Plug-In für "Ellipse" Neon installiert haben, können Sie es zu erstellen, erstellen und Bereitstellen von Service Fabric entwickelte mit Java verwenden.  Wählen Sie die **Ellipse IDE für Java-Entwickler**bei der Installation von "Ellipse" aus.

### <a name="create-the-application"></a>Erstellen Sie die Anwendung

Der Dienst Fabric-Plug-in ist durch "Ellipse" Erweiterbarkeit verfügbar.

1. Wählen Sie in "Ellipse", **Datei > andere > Dienst Fabric**. Sie sehen eine Reihe von Optionen, einschließlich Akteuren und Container.

    ![Dienst Fabric Vorlagen in "Ellipse"][sf-eclipse-templates]

2. Wählen Sie in diesem Fall statusfreie Dienst aus.

3. Sie werden aufgefordert, die Verwendung der Dienst Fabric Perspektive, das optimiert "Ellipse" zur Verwendung mit Projekten Dienst Fabric zu bestätigen. Wählen Sie 'Ja' aus.

### <a name="deploy-the-application"></a>Bereitstellen der Anwendungs

Die Dienst Fabric-Vorlagen enthalten eine Reihe von Gradle Aufgaben für das Erstellen und Bereitstellen von bereit, mit denen Sie durch "Ellipse" auslösen können.

1. Wählen Sie **Ausführen > Ausführen Konfigurationen**.

2. Erweitern Sie **Gradle Projekt** , und wählen Sie **ServiceFabricDeployer**.

3. Klicken Sie auf **Ausführen**.

Die app wird erstellen und Bereitstellen in wenigen Minuten. Sie können ihr Status aus dem Dienst Fabric Explorer überwachen.

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md)
- [Interagieren mit Dienst Fabric Cluster mithilfe der Azure-CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
