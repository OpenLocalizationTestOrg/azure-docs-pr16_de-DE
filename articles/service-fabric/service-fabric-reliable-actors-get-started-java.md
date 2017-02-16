<properties
   pageTitle="Erste Schritte mit Service Fabric zuverlässigen Akteuren | Microsoft Azure"
   description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen, Debuggen und Bereitstellen eines einfachen Akteur-basierten-Diensts mithilfe der Dienst Fabric zuverlässigen Akteuren dar."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Erste Schritte mit zuverlässigen Akteuren

> [AZURE.SELECTOR]
- [C# unter Windows](service-fabric-reliable-actors-get-started.md)
- [Java auf Linux](service-fabric-reliable-actors-get-started-java.md)

In diesem Artikel wird erläutert, die Grundlagen von Azure Service Fabric zuverlässigen Akteuren und führt Sie durch das Erstellen und Bereitstellen einer einfachen zuverlässigen Akteur-Anwendungs in Java.

## <a name="installation-and-setup"></a>Installation und Einrichtung
Bevor Sie beginnen, stellen Sie sicher, dass Sie den Dienst Fabric Entwicklungsumgebung auf Ihrem Computer eingerichtet haben.
Wenn Sie festlegen möchten, wechseln Sie zu [Erste Schritte auf einem Mac](service-fabric-get-started-mac.md) oder [Erste Schritte auf Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Um mit zuverlässigen Akteuren anzufangen, müssen Sie nur einige grundlegende Konzepte verstehen:

 * **Akteur-Dienst**. Zuverlässige Akteuren werden im zuverlässigen Services zusammengefasst, die in der Dienst Fabric-Infrastruktur bereitgestellt werden können. Akteur Instanzen werden in einer benannten Serviceinstanz aktiviert.
 
 * **Akteur-Registrierung**. Als muss mit den Diensten von zuverlässigen, ein zuverlässigen Akteur-Dienst in der Dienst Fabric Runtime registriert sein. Darüber hinaus muss der Akteurtyp in der Akteur Runtime registriert sein.
 
 * **Akteur-Benutzeroberfläche**. Die Benutzeroberfläche Akteur wird verwendet, um die eine Stimme eingegebene öffentliche Schnittstelle der Akteur definieren. In der Terminologie zuverlässigen Akteur-Modell definiert die Akteur-Schnittstelle die Arten von Nachrichten, die der Akteur verstanden werden kann und Prozess. Die Benutzeroberfläche Akteur wird von anderen Akteuren und Clientanwendungen "(asynchrone) zu den Akteur kontaktieren" verwendet. Zuverlässige Akteuren können mehrere Schnittstellen implementieren.
 
 * **ActorProxy Class**. Die Klasse ActorProxy wird von Clientanwendungen verwendet, um die Methoden wird über die Benutzeroberfläche Akteur aufzurufen. Die ActorProxy-Klasse bietet zwei wichtige Funktionen:
    * Benennen Sie die Auflösung: Dies ist möglich, suchen Sie den Akteur im Cluster (Suchen den Knoten im Cluster gehostet wird).
    * Die Fehlerbehandlung: Methodenaufrufe wiederholen und lösen erneut den Akteur-Speicherort nach, beispielsweise einem Fehler, der den Akteur auf einen anderen Knoten im Cluster verschoben werden kann.

Die folgenden Regeln, die Akteur-Schnittstellen betreffen, werden erwähnt:

- Akteur Schnittstellenmethoden können überladen werden.
- Akteur-Benutzeroberfläche, die Methoden nicht installiert haben, müssen, Ref oder optionale Parameter.
- Generische Schnittstellen werden nicht unterstützt.

## <a name="create-an-actor-service"></a>Erstellen eines Akteur-Diensts
Durch Erstellen einer neuen Fabric Service-Anwendung zu starten. Der Dienst Fabric SDK für Linux umfasst eine Yeoman-Generator das Gerüst für eine Fabric Service-Anwendung mit einem statusfreie Dienst bereitstellen. Starten Sie durch Ausführen der folgenden Yeoman Befehl:

```bash
$ yo azuresfjava
```

Folgen Sie den Anweisungen zum Erstellen einer **Zuverlässigen Akteur-Dienst**an. In diesem Lernprogramm nennen Sie die Anwendung "HelloWorldActorApplication" und der Akteur "HelloWorldActor." Das folgende Gerüst wird erstellt:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Zuverlässigen Akteuren grundlegenden Bausteine

Die grundlegenden Konzepte, die zuvor beschriebenen übersetzen in die grundlegenden Bausteine einer zuverlässigen Akteur-Diensts.

### <a name="actor-interface"></a>Akteur-Benutzeroberfläche

Die Definition der Benutzeroberfläche für den Akteur enthält. Diese Schnittstelle definiert den Akteur-Vertrag, der durch die Akteur-Implementierung und der Clients, die den Akteur aufrufen, damit es in der Regel sinnvoll ist, die sie an einem Ort definieren, die von der Akteur-Implementierung getrennt ist und gemeinsam für mehrere Dienste oder Clientanwendungen freigegeben ist.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Akteur-Dienst 
Dies enthält die Akteur-Implementierung und Registrierungscode Akteur. Die Akteurklasse implementiert die Akteur-Schnittstelle. Dies ist die Stelle, an der der Akteur seine Arbeit unterstützt.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Akteur-Registrierung

Der Akteur-Dienst muss mit einem Diensttyp in der Dienst Fabric Runtime registriert sein. Damit für den Akteur-Dienst Ihre Instanzen Akteur ausführen können muss der Akteurtyp auch mit dem Akteur-Dienst registriert sein. Die `ActorRuntime` Registrierungsmethode führt diese Arbeit für Akteuren dar.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testclient

Dies ist eine einfache Test-Clientanwendung Sie getrennt von der Fabric Service-Anwendung zum Testen des Diensts Akteur ausführen können. Dies ist ein Beispiel der Stelle, an der die ActorProxy zum Aktivieren und zur Kommunikation mit Akteur Instanzen verwendet werden können. Es ist nicht mit Ihrem Dienst bereitgestellt.

### <a name="the-application"></a>Die Anwendung 

Schließlich verpackt die Anwendung der Akteur-Dienst und andere Dienste, die Sie in der Zukunft zusammen für Bereitstellung hinzufügen können. Die Inhaber *ApplicationManifest.xml* und den Ort für das Akteur-Service-Paket enthält.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Die Yeoman Gerüstbau enthält ein Gradle Skript, um die Anwendung erstellen und Bereitstellen von Skripts und heben Sie die Markierung bash-Anwendung bereitstellen. Wenn Sie die Anwendung ausführen zu können, erstellen Sie zuerst die Anwendung mit Gradle:

```bash
$ gradle
```

Dies erzeugt Paket eines Fabric Service-Anwendung, die mit Service Fabric Azure CLI bereitgestellt werden können. Das Skript install.sh enthält die notwendigen Azure CLI Befehle zur Bereitstellung des Anwendungspakets. Führen Sie einfach das Skript install.sh bereitgestellt:

```bask
$ ./install.sh
```
