<properties
    pageTitle="Node.js erste Schritte mit | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer einfachen Node.js Web-Anwendungs, und klicken Sie auf eine Azure-Cloud-Dienst bereitgestellt."
    services="cloud-services"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Erstellen und Bereitstellen einer Anwendungs Node.js zu Azure-Cloud-Dienst

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

In diesem Lernprogramm erfahren, wie Sie eine einfache in Azure-Cloud-Dienst ausgeführt Node.js-Anwendung zu erstellen. Cloud Services sind die Bausteine in Azure skalierbare Cloud-Anwendung. Sie ermöglichen die Trennung und unabhängigen Management und Skalierung der Front-End- und Back-End-Komponenten der Anwendung.  Cloud Services bieten eine robuste dedizierten virtuellen Computern für jede Rolle zuverlässig hosten.

Weitere Informationen zum Cloud Services und wie diese Azure-Websites und virtuellen Computern vergleichen finden Sie unter [Azure Websites, Cloud-Diensten und virtuellen Computern Vergleich].

>[AZURE.TIP] Suchen Sie nach einer einfachen Website erstellen? Wenn Ihr Szenario nur eine einfache Front-End-Website umfasst, sollten erwägen Sie, [eine einfache Web app]zu verwenden. Sie können ganz einfach in einen Cloud-Service wie aktualisieren die Web-app immer umfangreicher wird, und ändern Sie Ihren Anforderungen.

Anhand dieses Lernprogramms erstellen Sie eine einfache Webanwendung, die in einer Webrolle gehostet. Sie werden die Serveremulator verwenden, um die Anwendung lokal zu testen und dann mithilfe der PowerShell Befehlszeile Tools bereitstellen.

Die Anwendung wird eine einfache "Hallo Welt":

![Einem Webbrowser die Webseite Hallo Welt anzeigen][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Erforderliche Komponenten

> [AZURE.NOTE] In diesem Lernprogramm verwendet die erfordert Windows Azure PowerShell.

- Installieren und Konfigurieren von [Azure Powershell].
- Herunterladen Sie und installieren Sie das [Azure SDK für .NET 2.7]. Wählen Sie in der Einrichtung installieren:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Erstellen Sie ein Projekt Azure-Cloud-Dienst

Führen Sie die folgenden Aufgaben zum Erstellen eines neuen Projekts der Azure-Cloud-Dienst, zusammen mit grundlegenden Node.js Gerüstbau an:

1. **Windows PowerShell** als Administrator ausführen. Suchen Sie über das **Menü Start** oder **-Startbildschirm**für **Windows PowerShell**.

2. [Verbinden von PowerShell] für Ihr Abonnement.

3. Geben Sie das folgende PowerShell-Cmdlet erstellen, um das Projekt zu erstellen:

        New-AzureServiceProject helloworld

    ![Das Ergebnis des Befehls Helloworld neu-AzureService][The result of the New-AzureService helloworld command]

    Das **Neue AzureServiceProject** Cmdlet generiert eine einfache Struktur für die Veröffentlichung einer Anwendungs Node.js in einen Cloud-Service. Sie enthält Konfigurationsdateien, die für das Veröffentlichen zur Azure erforderlich sind. Das Cmdlet wird ebenfalls Ihre Arbeitsverzeichnis auf Verzeichnis für den Dienst an.

    Das Cmdlet erstellt die folgenden Dateien:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** und **ServiceDefinition.csdef**: Azure-spezifische Dateien für die Veröffentlichung der Anwendungs erforderlich. Weitere Informationen finden Sie unter [Übersicht über das Erstellen eines gehosteten Diensts für Azure].

    -   **deploymentSettings.json**: Speichert lokale Einstellungen, die durch die Bereitstellung Azure PowerShell-Cmdlets verwendet werden.

4.  Geben Sie den folgenden Befehl aus, um eine neue Webrolle hinzuzufügen:

        Add-AzureNodeWebRole

    ![Die Ausgabe des Befehls hinzufügen-AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    Das Cmdlet **Hinzufügen-AzureNodeWebRole** erstellt eine einfache Node.js-Anwendung. Es werden auch die **.csfg** und **.csdef** -Dateien zum Hinzufügen von Konfigurationseinträgen für die neue Rolle geändert.

    > [AZURE.NOTE] Wenn Sie einen Rollennamen nicht angeben, wird ein Standardname verwendet. Sie können einen Namen als erster Parameter Cmdlet angeben:`Add-AzureNodeWebRole MyRole`

Die app Node.js wird in der Datei **server.js**, befindet sich im Verzeichnis für die Web-Rolle (standardmäßig**WebRole1** ) definiert. Hier ist der Code ein:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Dieser Code ist im Wesentlichen identisch mit der Stichprobe "Hallo Welt" auf der Website [nodejs.org] mit mit Ausnahme von der Cloud-Umgebung zugewiesene verwendet wird.

## <a name="deploy-the-application-to-azure"></a>In Azure bereitzustellen

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Laden Sie die Einstellungen für Azure für die Veröffentlichung

Um die Anwendung in Azure bereitstellen zu können, müssen Sie zuerst die Einstellungen für die Veröffentlichung für Ihr Abonnement Azure herunterladen.

1.  Führen Sie das folgende Azure PowerShell-Cmdlet ein:

        Get-AzurePublishSettingsFile

    Hiermit wird Ihr Browser verwendet, um zur Downloadseite Einstellungen veröffentlichen zu navigieren. Sie möglicherweise aufgefordert, sich mit einem Microsoft-Account anmelden. Ist dies der Fall ist, verwenden Sie das Konto mit Ihrem Azure-Abonnement verknüpft ist.

    Speichern Sie das heruntergeladene Profil mit einem Speicherort, den Sie problemlos zugreifen können.

2.  Führen Sie folgende Cmdlet aus, um das Profil für die Veröffentlichung zu importieren, die, das Sie heruntergeladen haben:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Nach dem Import der Einstellungen veröffentlichen, erwägen Sie die heruntergeladene PUBLISHSETTINGS-Datei löschen, da es Informationen, die einer Person enthält auf Ihr Konto zugreifen können.

### <a name="publish-the-application"></a>Veröffentlichen Sie die Anwendung

Wenn Sie veröffentlichen möchten, führen Sie folgende Befehle aus:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **ServiceName-** gibt den Namen für die Bereitstellung an. Dies muss einen eindeutigen Namen, andernfalls Veröffentlichungsprozesses schlägt fehl. Der Befehl **Get-Date** ergreift auf ein Datum/Uhrzeit-Zeichenfolge, die den Namen sollten eindeutig zu machen.

- **-Speicherort** gibt an, das Rechenzentrum, die in die Anwendung gehostet wird. Um eine Liste der verfügbaren Rechenzentren anzuzeigen, verwenden Sie das Cmdlet " **Get-AzureLocation** " ein.

- **-Schnellstartleiste** wird ein Browserfenster geöffnet und navigiert an den gehosteten Dienst nach Abschluss der Bereitstellung.

Nachdem die Veröffentlichung erfolgreich ist, wird eine Antwort ähnlich wie der folgende angezeigt:

![Die Ausgabe des Befehls veröffentlichen-AzureService][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Es kann einige Minuten, damit die Anwendung bereitstellen und erst verfügbar, wenn Sie erstmals veröffentlicht dauern.

Nach dem Abschluss der bereitstellungs wird ein Browserfenster öffnen, und navigieren Sie zu der Cloud-Dienst.

![Ein Browserfenster anzeigen der Seite Hallo Welt; die URL gibt an, dass auf die Seite auf Azure gehostet wird.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Die Anwendung wird jetzt auf Azure ausgeführt.

Das Cmdlet **Veröffentlichen-AzureServiceProject** führt die folgenden Schritte aus:

1.  Erstellt ein Paket bereitstellen. Das Paket enthält alle Dateien im Ordner "Anwendung".

2.  Erstellt ein neues **Speicher-Konto** an, wenn eine nicht vorhanden ist. Das Konto Azure-Speicher dient zum Speichern von des Anwendungspakets während der Bereitstellung. Sie können das Speicherkonto sicheres löschen, nach der Bereitstellung.

3.  Erstellt eine neue **Cloud-Dienst** an, wenn Sie noch nicht vorhanden ist. Einen **Cloud-Dienst** handelt es sich um den Container, in dem die Anwendung gehostet wird, wenn es in Azure bereitgestellt wird. Weitere Informationen finden Sie unter [Übersicht über das Erstellen eines gehosteten Diensts für Azure].

4.  Das Bereitstellungspaket veröffentlicht Azure.


## <a name="stopping-and-deleting-your-application"></a>Beenden und Löschen von Ihrer Anwendung

Nach der Bereitstellung Ihrer Anwendungs, empfiehlt es sich um es zu deaktivieren, sodass Sie zusätzliche Kosten vermeiden können. Azure Rechnung web Rolleninstanzen pro Stunde Server Zeit verbraucht. Serverzeit ist verbraucht, nach der Bereitstellung Ihrer Anwendungs, selbst wenn die Instanzen nicht ausgeführt werden und beendet werden.

1.  Beenden Sie in der Windows PowerShell-Fenster die Service-Bereitstellung erstellt, die im vorherigen Abschnitt mit das folgende Cmdlet aus:

        Stop-AzureService

    Beenden des Diensts möglicherweise einige Minuten dauern. Wenn der Dienst angehalten wird, erhalten Sie eine Nachricht, die angibt, dass es beendet wurde.

    ![Der Status des Befehls beenden-AzureService][The status of the Stop-AzureService command]

2.  Um den Dienst zu löschen, rufen Sie das folgende Cmdlet aus:

        Remove-AzureService

    Wenn Sie dazu aufgefordert werden, geben Sie **Y** ein, um den Dienst zu löschen.

    Löschen den Dienst möglicherweise einige Minuten dauern. Nach der Dienst gelöscht wurde, erhalten Sie eine Nachricht, die angibt, dass der Dienst gelöscht wurde.

    ![Der Status des Befehls entfernen-AzureService][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Löschen den Dienst Speicher-Konto, das erstellt wurde, wenn der Dienst ursprünglich veröffentlicht wurde nicht löschen, und erhalten Sie weiterhin in Rechnung gestellt für Speicher verwendet wird. Wenn keine weiteren Daten im Speicher verwendet wird, können Sie löschen möchten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter der [Node.js Developer Center].

<!-- URL List -->

[Vergleich der Azure Websites, Cloud-Diensten und virtuellen Computern]: ../app-service-web/choose-web-site-cloud-service-vm.md
[verwenden eine einfache Web app]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[Azure SDK für .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Verbinden von PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Übersicht über das Erstellen eines gehosteten Diensts für Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js-Entwicklercenter]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
