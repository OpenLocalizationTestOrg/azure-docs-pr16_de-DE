<properties 
    pageTitle="Verwenden der PM2 Konfiguration für NodeJS im Web Apps auf Linux | Microsoft Azure" 
    description="Verwenden der PM2 Konfiguration für NodeJS im Web Apps auf Linux" 
    keywords="app Azure Service, Online, Nodejs, pm2, Linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Verwenden der PM2 Konfiguration für Node.js in Web Apps auf Linux

Wenn Sie den Anwendungsstapel auf Node.js für Web Apps auf Linux festlegen, erhalten Sie die Option zum Festlegen einer Startdatei Node.js, wie in der nachstehenden Abbildung gezeigt.

![][1]

Hiermit können Sie entweder

-   Geben Sie das Skript zum Starten des für Ihre app Node.js (zum Beispiel: /bin/server.js)
-   Angeben die PM2 Konfigurationsdatei für Ihre app Node.js verwendet werden soll (beispielsweise: /foo/process.json)

 >[AZURE.NOTE] Wenn Ihre Knoten Prozesse automatisch neu gestartet, wenn bestimmte Dateien geändert werden soll, müssen Sie die Konfiguration PM2 verwenden. Andernfalls wird die Anwendung nicht neu gestartet, wenn es Benachrichtigungen Ändern von Aspekten wie fortlaufender Bereitstellung empfangen, wenn Ihre Anwendungscode geändert wird.

Sie können Sie in der Node.js- [Prozess Datei Dokumentation](http://pm2.keymetrics.io/docs/usage/application-declaration/) für alle Optionen, aber es folgt eine Stichprobe des was Sie, wie Ihre process.json-Datei verwenden möchten

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Wichtige Aspekte, die in dieser Konfiguration zu beachten sind. 

-   Die Eigenschaft "Skript" gibt an Ihrer Anwendung Start Skript.
-   Die Eigenschaft "Instanzen" gibt an, wie viele Instanzen des Prozesses Knoten zu starten. Wenn Sie eine Anwendung auf größere virtueller Computer, die mehrere Kerne aufweisen ausgeführt werden, möchten Sie Ressourcen maximieren, indem Sie hier einen höheren Wert festlegen.
-   Die Matrix "Überwachung" gibt an, alle Dateien, die Änderung Sie Ihre Prozesse Knoten neu starten möchten.
-   Für die "Watch_options" müssen Sie aktuell "UsePolling" als WAHR aufgrund der Art und Weise angeben, die der Anwendungsinhalt bereitgestellt wird.


## <a name="next-steps"></a>Nächste Schritte ##

* [Was ist die App-Verwaltungsdienst auf Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png