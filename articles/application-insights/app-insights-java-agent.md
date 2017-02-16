<properties 
    pageTitle="Überwachen von Abhängigkeiten, Ausnahmen und aufgezeichneten Zeiten in Java Web apps" 
    description="Überwachen Ihrer Java-Website mit der Anwendung Einsichten erweiterten" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Überwachen von Abhängigkeiten, Ausnahmen und aufgezeichneten Zeiten in Java Web apps

*Anwendung Einsichten ist in der Vorschau.*

Wenn Sie [Ihre Java Web app mit Anwendung Einsichten instrumentiert]haben[java], können Sie die Java-Agent um tiefere Einblicken, ohne Code Änderungen zu erhalten:


* **Abhängigkeiten:** Daten zu Anrufe, die andere unsichere Komponenten, einschließlich Ihrer Anwendung ist:
 * **REST Anrufe** über HttpClient, OkHttp und RestTemplate (Frühling).
 * **Redis** -Aufrufe über den Client Jedis. Wenn Sie der Anruf dauert länger als 10 s, abgerufen der Agent auch die Argumente Anruf.
 * **[Anrufe JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB oder Apache Derby DB. "ExecuteBatch"-Aufrufe werden unterstützt. Wenn Sie der Anruf dauert länger als 10 s, Berichte der Agent für MySQL und PostgreSQL den Abfrageplan. 
* **Abgefangen Ausnahmen:** Daten zu Ausnahmen, die vom Code verarbeitet werden.
* **Methode Ausführung dauert:** Daten über die Zeit, die benötigt wird, um bestimmte Methoden ausführen.

Wenn den Java-Agent verwenden möchten, installieren Sie es auf dem Server. Von Web apps instrumentiert werden müssen, mit der [Anwendung Einsichten Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Installieren des Anwendung Einsichten Agents für Java

1. Auf dem Computer ausgeführt Ihrer Java-Server, [den Agent herunterladen](https://aka.ms/aijavasdk).
2. Bearbeiten Sie das Anwendung Server Startskript, und fügen Sie die folgende JVM:

    `javaagent:`*vollständigen Pfad zu der Agent JAR-Datei*

    Beispielsweise in Tomcat auf einem Computer Linux:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Starten Sie den Anwendungsserver neu.

## <a name="configure-the-agent"></a>Konfigurieren des Agents

Erstellen Sie eine Datei mit dem Namen `AI-Agent.xml` und platzieren Sie es in denselben Ordner wie die Agent JAR-Datei.

Legen Sie den Inhalt der XML-Datei ein. Bearbeiten Sie im folgende Beispiel zum einschließen oder die Features nicht bilden sollen. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Sie müssen den Berichte Ausnahme und Methode Anzeigedauer für einzelne Methoden aktivieren.

Standardmäßig `reportExecutionTime` ist WAHR und `reportCaughtExceptions` ist "false".

## <a name="view-the-data"></a>Anzeigen von Daten

In der Anwendung Einsichten Ressource zusammengefasster remote Abhängigkeit und Methode aufgezeichneten-Zeiten angezeigt wird, [unter der Kachel Leistung][metrics]. 

Öffnen Sie zum für einzelne Instanzen Abhängigkeit, Ausnahme und Methode Berichte zu suchen, [Suchen][diagnostic]. 

[Diagnostizieren Abhängigkeitsprobleme – erfahren Sie mehr](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Fragen? Probleme?

* Keine Daten? [Die Firewallausnahmen festlegen](app-insights-ip-addresses.md)
* [Problembehandlung bei Java](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
