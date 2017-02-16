<properties
   pageTitle="Erste Schritte mit den Diensten von zuverlässigen | Microsoft Azure"
   description="Einführung in das Erstellen einer Microsoft Azure Service Fabric-Anwendung mit zustandslosen und dynamische Dienste."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Erste Schritte mit zuverlässigen Services

> [AZURE.SELECTOR]
- [C# unter Windows](service-fabric-reliable-services-quick-start.md)
- [Java auf Linux](service-fabric-reliable-services-quick-start-java.md)

Dieser Artikel erläutert die Grundlagen der Azure Service Fabric zuverlässigen Services und führt Sie durch das Erstellen und Bereitstellen einer einfachen zuverlässigen Service-Anwendungs in Java geschrieben.

## <a name="installation-and-setup"></a>Installation und Einrichtung
Bevor Sie beginnen, stellen Sie sicher, dass Sie den Dienst Fabric Entwicklungsumgebung auf Ihrem Computer eingerichtet haben.
Wenn Sie festlegen möchten, wechseln Sie zu [Erste Schritte auf einem Mac](service-fabric-get-started-mac.md) oder [Erste Schritte auf Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Um mit den Diensten von zuverlässigen anzufangen, müssen Sie nur einige grundlegende Konzepte verstehen:

 - **Diensttyp**: Dies ist Ihre Service-Implementierung. Es wird durch die Klasse Sie schreiben, erweitert definiert `StatelessService` und alle anderen Code oder Abhängigkeiten darin, zusammen mit einen Namen und eine Versionsnummer verwendet.

 - **Benannte Dienstinstanz**: zum Ausführen des Diensts erstellen benannte Instanzen von Ihrem Diensttyp ähnlich aus wie Objektinstanzen eines Klassentyps zu erstellen. Dienstinstanzen sind eigentlich Objekt Instanzen der Dienstklasse, die Sie schreiben. 

 - **Diensthost**: die benannte Dienstinstanzen Sie erstellen innerhalb einer Host ausführen müssen. Der Diensthost ist nur eines Prozesses, in dem Instanzen von Ihrem Dienst ausgeführt werden kann.

 - **Dienst Registrierung**: Registrierung vereint alles. Mit der Dienst Fabric Laufzeit in einem Diensthost Service-Struktur, bis es Instanzen erstellen dürfen ausführen muss der Diensttyp registriert sein.  

## <a name="create-a-stateless-service"></a>Erstellen Sie einen statusfreien Dienst

Durch Erstellen einer neuen Fabric Service-Anwendung zu starten. Der Dienst Fabric SDK für Linux umfasst eine Yeoman-Generator das Gerüst für eine Fabric Service-Anwendung mit einem statusfreie Dienst bereitstellen. Starten Sie durch Ausführen der folgenden Yeoman Befehl:

```bash
$ yo azuresfjava
```

Folgen Sie den Anweisungen zum Erstellen einer **Zuverlässigen statusfreie Dienst**an. In diesem Lernprogramm nennen Sie die Anwendung "HelloWorldApplication" und der Dienst "HelloWorld". Das Ergebnis wird Verzeichnisse für einschließen der `HelloWorldApplication` und `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Implementieren der Dienst

Öffnen Sie **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Diese Klasse definiert den Diensttyp und Code ausgeführt werden kann. Die Dienst-API liefert zwei Felder und Optionen für den Code ein:

 - Eine offene Eintrag Punkt Methode `runAsync()`, beginnen Sie können alle Auslastung, einschließlich langer berechnen Auslastung ausführen.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Einen Einstiegspunkt für die Kommunikation, wo Sie Ihre Kommunikationsstapel Wahl anschließen können. Dies ist die Stelle, an der Sie Anfragen von Benutzern und andere Dienste empfangen starten können.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

In diesem Lernprogramm wir liegt der Schwerpunkt auf der `runAsync()` Punkt Eingabemethode. Dies ist die Stelle, an der Sie sofort starten können Ihren Code ausführt.

### <a name="runasync"></a>RunAsync

Die Plattform ruft diese Methode, wenn eine Instanz eines Diensts platziert und ausgeführt werden kann. Der Zyklus Öffnen/Schließen der Services-Instanz kann über die Gültigkeitsdauer des Diensts oft als Ganzes auftreten. Dies kann geschehen, verschiedene Gründe geben, einschließlich:

- Das System verschiebt Ihrem Dienstinstanzen für Lastenausgleich Ressource.
- Fehler auftreten, in Ihrem Code.
- Die Anwendung oder das System wird aktualisiert.
- Die zugrunde liegende Hardware auftritt, einen Ausfall.

Diese Orchestrierung verwaltet wird vom Dienst-Struktur, bis Sie den Dienst hochgradig verfügbar und ordnungsgemäß angeglichene beibehalten.

#### <a name="cancellation"></a>Kündigung

Es ist wichtig, die Code in `runAsync()` können die Ausführung bei Benachrichtigung durch Dienst Fabric beenden. Die `CompletableFuture` zurückgegeben von `runAsync()` wird abgebrochen, wenn Dienst Fabric des Diensts Ausführung beenden erfordert. Im folgenden Beispiel wird veranschaulicht, wie ein Ereignis Absage behandeln: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Service-Registrierung

Service-Typen müssen mit der Dienst Fabric Runtime registriert sein. Der Diensttyp wird definiert, der `ServiceManifest.xml` und Ihrem Dienstklasse, implementiert `StatelessService`. Dienst Registration wird in den Prozess Einstiegspunkt ausgeführt. In diesem Beispiel wird der Prozess Einstiegspunkt `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Führen Sie die Anwendung

Die Yeoman Gerüstbau enthält ein Gradle Skript, um die Anwendung erstellen und Bereitstellen von Skripts und heben Sie die Markierung bash-Anwendung bereitstellen. Wenn Sie die Anwendung ausführen zu können, erstellen Sie zuerst die Anwendung mit Gradle:

```bash
$ gradle
```

Dies erzeugt Paket eines Fabric Service-Anwendung, die mit Service Fabric Azure CLI bereitgestellt werden können. Das Skript install.sh enthält die notwendigen Azure CLI Befehle zur Bereitstellung des Anwendungspakets. Führen Sie einfach das Skript install.sh bereitgestellt:

```bask
$ ./install.sh
```
