<properties 
    pageTitle="So verwenden Sie io.js mit Azure App Dienst Web Apps" 
    description="Erfahren Sie, wie eine Web app in Azure-App-Verwaltungsdienst mit io.js verwenden." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>So verwenden Sie io.js mit Azure App Dienst Web Apps

Beliebte Knoten Verzweigung [io.js] features verschiedene Unterschiede des Joyent Node.js Projekt, einschließlich eine größere Anzahl von geöffneten governancemodell, eine schnellere Releasezyklus und eine schnellere Annahme neuer und Versuche JavaScript-Features.

Während der [Azure-App-Verwaltungsdienst](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps viele Node.js Versionen vorinstalliert wurde, können auch für eine vom Benutzer bereitgestellte Node.js Binärzahl. Dieser Artikel beschreibt zwei Methoden, ermöglichen der Verwendung von io.js auf App-Dienst Web Apps: die Verwendung von einer erweiterten Bereitstellungsskript, das Azure, um die neueste Version von verfügbar io.js als auch das manuelle Hochladen einer io.js binäre verwenden automatisch konfiguriert. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Verwenden ein Bereitstellungsskript

Nach der Bereitstellung einer App Node.js führt App Dienst Web Apps eine Reihe von kleinen Befehle, um sicherzustellen, dass die Umgebung ordnungsgemäß konfiguriert ist. Verwenden ein Bereitstellungsskript, kann dieses Verfahren angepasst werden, um den Download und die Konfiguration von io.js enthalten.

Die [io.js Bereitstellungsskript](https://github.com/felixrieseberg/iojs-azure) ist auf GitHub verfügbar. Um io.js auf Web app zu aktivieren, kopieren Sie einfach **darauf verweist das**, **deploy.cmd** und **IISNode.yml** in den Stammordner Ihrer Anwendung und Bereitstellen für Web Apps.  

Die erste Datei, **darauf verweist das**, weist Web Apps **deploy.cmd** bei der Bereitstellung ausführen. Dieses Skript führt die üblichen Schritte für eine Anwendung Node.js, aber auch downloads die neueste Version von io.js. Schließlich konfiguriert **IISNode.yml** Web-Apps, um nur die heruntergeladene binäre io.js anstelle eines integrierten Node.js Binärzahl verwenden.

> [AZURE.NOTE] Zum Aktualisieren der Binärdatei verwendeten io.js einfach erneut bereit Ihrer Anwendung - wird das Skript eine neue Version von io.js jedem einzelnen herunterladen, die die Anwendung bereitgestellt wird.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Verwenden der manuellen Installation

Die manuelle Installation von einer benutzerdefinierten io.js Version enthält nur zwei Schritte. Laden Sie zunächst die binäre **Win-X64** direkt von der [io.js Verteilung]an. Erforderlich sind zwei Dateien - **iojs.exe** und **iojs.lib**. Speichern Sie beide Dateien in einem Ordner im Web app, beispielsweise in den **Papierkorb/Iojs**ein.

Web Apps, um **iojs.exe** anstelle einer vorinstallierten Version von Knoten verwenden um zu konfigurieren, erstellen Sie eine **IISNode.yml** -Datei im Stammverzeichnis der Anwendung, und fügen Sie die folgende Zeile hinzu.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel, die Sie gelernt verwenden bereitgestellten io.js mit App-Dienst Web Apps, verwenden beide Bereitstellungsskripts manuelle Installation. 

> [AZURE.NOTE] IO.js befindet sich beanspruchen Entwicklung und häufiger als Node.js aktualisiert. Eine Anzahl von Node.js Module funktioniert möglicherweise nicht mit io.js - Bitte können Sie sich [auf GitHub io.js] zur Behandlung dieses Problems.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

[IO.js]: https://iojs.org
[IO.js Verteilung]: https://iojs.org/dist/
[Klicken Sie auf GitHub IO.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 