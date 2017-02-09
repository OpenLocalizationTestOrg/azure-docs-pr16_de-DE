<properties 
    pageTitle="Einführung in die App-Verwaltungsdienst auf Linux | Microsoft Azure" 
    description="Informationen Sie zu App-Verwaltungsdienst unter Linux." 
    keywords="app Azure Service, Linux, oss"
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

# <a name="introduction-to-app-service-on-linux"></a>Einführung in die App-Verwaltungsdienst auf Linux
App-Verwaltungsdienst auf Linux wird gerade Public Preview und laufenden Web apps systembedingt unter Linux unterstützt. 

## <a name="overview"></a>(Übersicht) ##
Kunden können Sie App-Dienst auf Linux bei Host Web apps auf Linux systembedingt für unterstützte Anwendung Stapel verwenden. Der folgenden Features Abschnitt listet die derzeit unterstützten Anwendung Stapel.

## <a name="features"></a>Features ##
App-Verwaltungsdienst auf Linux unterstützt derzeit die folgenden Anwendung Stapel

- Node.js
- PHP

Kunden können ihre mithilfe von Applications bereitstellen.

- FTP.
- Lokale Git.
- GitHub oder BitBucket.

Für die Anwendungsskalierung


- Kunden können ihre Online nach oben oder unten skalieren, durch Ändern der Ebene in ihren App-Dienst planen. 
- Kunden können skalieren, deren Anwendung, und führen ihre app über mehrere Instanzen innerhalb der Grenzen von deren SKU.

Für Kudu funktionieren einige der grundlegenden Funktionen

- Umgebung.
- Bereitstellungen.
- Grundlegende Console.

## <a name="limitations"></a>Einschränkungen ##

Azure Verwaltungsportal wird nur derzeit unterstützten Features für die App-Verwaltungsdienst auf Linux ein- und Ausblenden von den Rest. Als unser Team aktivieren Sie weitere Funktionen wir werden diese auf die Verwaltungsportal über die entsprechenden beibehalten. Einige Features wie VNET Integration und AAD / Drittanbieter-Authentifizierung oder Kudu Website Erweiterungen aktuell funktionieren nicht. Aber wie wir diese Arbeiten erhalten werden wir aktualisieren unsere Dokumentation und Blog über Änderungen.

Diese public Preview-Version steht derzeit nur in den folgenden Regionen

-   Westen US.
-   Westen Europe.
-   Oder Asien.

Web app auf Linux wird nur unterstützt dedizierte App Service-Pläne und verfügt nicht über eine Stufe Free oder freigegeben. Darüber werden hinaus Dienstpläne für normale und Linux Web apps app gegenseitig aus, daher können Sie eine Linux Web app in einer nicht Linux app-Serviceplan erstellen.

Web app auf Linux muss in einer Ressourcengruppe erstellt werden, die nicht Linux Web apps in derselben Region nicht enthält.

Aufgrund der fehlenden überlappende Wiederverwendung der Web apps erwarten die Kunden, dass eine kleine Ausfallzeit im Falle einer Web app neu gestartet haben. 

## <a name="next-steps"></a>Nächste Schritte ##

Führen Sie die folgenden Links zu den ersten Schritten mit App-Verwaltungsdienst unter Linux. Bitte veröffentlichen Sie Fragen und Probleme im Forum " [unsere](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview)" ein.

* [Erstellen von App-Verwaltungsdienst auf Linux Web Apps](./app-service-linux-how-to-create-a-web-app.md)
* [Verwenden der PM2 Konfiguration für Node.js in Web Apps auf Linux](./app-service-linux-using-nodejs-pm2.md)

