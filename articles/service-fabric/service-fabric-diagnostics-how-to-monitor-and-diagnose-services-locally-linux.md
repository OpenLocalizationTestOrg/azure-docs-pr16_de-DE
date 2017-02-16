<properties
   pageTitle="Lokal überwachen und diagnostizieren Services mit Azure Service Fabric geschrieben | Microsoft Azure"
   description="Informationen Sie zum Überwachen und diagnostizieren Ihrer Dienste mit Microsoft Azure Service Fabric auf einem Computer lokale Entwicklung geschrieben."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Überwachen und Diagnostizieren von Diensten in einer lokalen Computer Entwicklung einrichten


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Überwachung, erkennen, bei der Diagnose und Problembehandlung zu ermöglichen Services mit minimaler Unterbrechung für die Benutzerfunktionalität fortsetzen. Überwachung und Diagnose sind kritisch, in einer Umgebung tatsächlichen bereitgestellten Herstellung. Ein ähnliches Modell während der Entwicklung von Diensten eingeführt wird sichergestellt, dass die Diagnose Verkaufspipeline funktioniert, wenn Sie in einer Umgebung für die Herstellung verschieben. Dienst Fabric erleichtert Dienst Entwickler Diagnose implementiert wird, die sowohl in einem einzelnen Computer lokale Entwicklung Setups praktisches Herstellung Cluster Setups arbeiten können.


## <a name="debugging-service-fabric-java-applications"></a>Debuggen von Dienst Fabric Java

Für Applikationen Java stehen [mehrere Protokollierung Framework](http://en.wikipedia.org/wiki/Java_logging_framework) zur Verfügung. Da `java.util.logging` ist die Standardoption mit JRE, ist es auch für die [Codebeispielen in Github](http://github.com/Azure-Samples/service-fabric-java-getting-started)verwendet.  Die folgende Diskussion erläutert, wie Sie konfigurieren die `java.util.logging` Framework. 
 
Mit java.util.logging können Sie Ihre Anwendungsprotokolle Arbeitsspeicher, Ausgabe Streams, Console-Dateien oder Sockets umleiten. Für jede dieser Optionen es gibt standardmäßigen Ereignishandler bereits in Framework bereitgestellt. Sie können Erstellen einer `app.properties` zum Konfigurieren des Dateihandler für eine Anwendung alle Protokolle in einer lokalen Datei umleiten Datei. 

Der folgende Codeausschnitt enthält eine Beispiel für eine Konfiguration: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Der Ordner, auf die die `app.properties` Datei muss vorhanden sein. Nach der `app.properties` erstellt wird, müssen Sie auch Ihr Eintrag Punkt-Skript ändern `entrypoint.sh` in die `<applicationfolder>/<servicePkg>/Code/` Ordner zum Festlegen der Eigenschaft `java.util.logging.config.file` auf `app.propertes` Datei. Der Eintrag sollte wie im folgenden Codeausschnitt aussehen:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Diese Konfiguration führt Protokolle drehen Weise bei erfasst wird `/tmp/servicefabric/logs/`. Die **"% u"** und **%g** ermöglichen Weitere Dateien mit Dateinamen mysfapp0.log, mysfapp1.log usw. erstellen. Wenn keine Ereignishandler explizit konfiguriert ist, wird standardmäßig der Console Ereignishandler registriert. Eine kann die Protokolle in Syslog unter /var/log/syslog anzeigen.
 
Weitere Informationen finden Sie in den [Codebeispielen in Github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Nächste Schritte
Derselbe Tracing Code zur Anwendung hinzugefügt, funktioniert auch mit der Diagnose Ihrer Anwendung auf einem Azure Cluster. Schauen Sie sich diesen Artikel, die die verschiedenen Optionen für die Tools diskutieren und beschreiben, wie Sie diese einrichten können.
* [Zum Sammeln von Protokollen mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-lad.md)
