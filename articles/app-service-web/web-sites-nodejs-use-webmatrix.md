<properties 
    pageTitle="Erstellen und Bereitstellen einer Node.js Web app Azure mithilfe von WebMatrix" 
    description="Ein Lernprogramm zeigt, die Ihnen wie WebMatrix eine Anwendung Node.js entwickeln und auf Azure App Dienst Web Apps bereitstellen." 
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
    ms.author="robmcm"/>


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Erstellen und Bereitstellen einer Node.js Web app Azure mithilfe von WebMatrix

In diesem Lernprogramm erfahren Sie, wie WebMatrix eine Anwendung Node.js entwickeln und auf [App-Verwaltungsdienst Azure](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps bereitstellen. WebMatrix ist ein kostenlos zur Verfügung Entwicklungstool von Microsoft, die alle Elemente enthält, die Sie für die Website oder Web app-Entwicklung müssen. WebMatrix enthält einige Features, die einfach zu verwenden, einschließlich Code vervollständigen, vordefinierten Vorlagen und Editor-Unterstützung für Jade, weniger Node.js und CoffeeScript zu machen. Weitere Informationen zu [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Nach Abschluss dieses Handbuch, haben Sie eine Node.js Web app im App-Verwaltungsdienst Azure ausgeführt.
 
Ein Screenshot der fertigen Anwendung lautet wie folgt:

![Azure Knoten-Website][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="sign-into-azure"></a>Melden Sie sich bei Azure

Wie folgt vor, um eine Web app in Azure-App-Verwaltungsdienst zu erstellen.

1. Starten Sie WebMatrix
2. Ist dies das erste Mal haben Sie WebMatrix, verwendet Sie werden aufgefordert, melden Sie sich bei Azure.  Andernfalls können Sie auf die Schaltfläche **Anmelden** klicken Sie auf, und wählen **Konto hinzufügen**.  Wählen Sie aus, **Melden Sie sich** mit Ihrem Microsoft-Account.

    ![Konto hinzufügen][addaccount]

3. Wenn Sie sich für ein Azure-Konto angemeldet haben, können Sie mit Ihrem Microsoft-Account anmelden:

    ![Melden Sie sich bei Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Erstellen einer Website mit einer integrierten Vorlage für Azure

1. Klicken Sie auf dem Startbildschirm klicken Sie auf die Schaltfläche **neu** , und wählen Sie **Office-Vorlagen** zum Erstellen einer neuen Website aus dem Vorlagenkatalog:

    ![Neue Website aus Office-Vorlagen][sitefromtemplate]

2. Klicken Sie im Dialogfeld **Website aus Vorlage** wählen Sie **Knoten aus** , und wählen Sie dann auf **Express-Website**. Klicken Sie auf **Weiter**. Wenn Sie alle erforderlichen Komponenten für die **Express-** Websitevorlage fehlt, werden Sie aufgefordert, diese zu installieren.

    ![Wählen Sie express-Vorlage aus.][webmatrix-templates]

3. Wenn Sie in Azure angemeldet sind, haben Sie jetzt die Option zum Erstellen einer App-Dienst Web app für Ihre lokale Website.  Wählen Sie einen eindeutigen Namen aus, und wählen Sie das Rechenzentrum, in dem Sie Ihre App-Dienst Web app erstellt werden soll: 

    ![Erstellen der Website auf Azure][nodesitefromtemplateazure]
    
4. Nach WebMatrix endet am lokalen Standort erstellen und die App-Dienst Web app erstellen, wird der WebMatrix IDE angezeigt.

    ![WebMatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Veröffentlichen Sie die Anwendung in Azure

1. Klicken Sie im WebMatrix **Veröffentlichen** Sie im Menüband **Home** Veröffentlichen im Dialogfeld **Vorschau** der für die Website angezeigt werden.

    ![Veröffentlichen der Vorschau][webmatrix-node-publishpreview]

2. Klicken Sie auf **Weiter**. Wenn die Veröffentlichung abgeschlossen ist, wird die URL für die App-Dienst Web app am unteren Rand der IDE WebMatrix angezeigt.

    ![Veröffentlichen abgeschlossen][webmatrix-publish-complete]

3. Klicken Sie auf den Link, um die App-Dienst Web app in Ihrem Browser zu öffnen.

    ![Express-Web app][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Ändern Sie und veröffentlichen Sie die Anwendung

Sie können ganz einfach ändern und veröffentlichen Sie die Anwendung. Hier werden Sie eine einfache zur Überschrift im in der Datei **index.jade** ändern und erneut veröffentlichen der Anwendungs vornehmen.

1. Wählen Sie in WebMatrix **Dateien**aus, und erweitern Sie dann auf den Ordner " **Ansichten** ". Öffnen Sie die Datei **index.jade** , indem Sie darauf doppelklicken.

    ![WebMatrix Anzeige index.jade][webmatrix-modify-index]

2. Ändern Sie die Zeile des Absatzes wie folgt aus:

        p Welcome to #{title} with WebMatrix on Azure!

3. Speichern Sie die Änderungen zu, und klicken Sie dann auf das Symbol veröffentlichen. Schließlich, klicken Sie auf **Weiter** klicken Sie im Dialogfeld **Vorschau veröffentlichen** und warten Sie, bis das Update veröffentlicht werden soll.

    ![Veröffentlichen der Vorschau][webmatrix-republish]

4. Wenn die Veröffentlichung abgeschlossen hat, verwenden Sie den Link, der zurückgegeben wird, wenn das Veröffentlichen das aktualisierte App Dienst Web app sehen abgeschlossen ist.

    ![Azure Knoten Web app][webmatrix-node-completed]

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Node.js-Versionen, die mit Azure und wie Sie die Version mit Ihrer Anwendung zu verwendende angeben bereitgestellt werden, finden Sie unter [Festlegen einer Version Node.js in Azure-Anwendung](../nodejs-specify-node-version-azure-apps.md).

Wenn Probleme mit der Anwendung auftreten, nachdem es für Azure bereitgestellt wurde, finden Sie unter [Debuggen eine Node.js Web app im App-Verwaltungsdienst Azure](web-sites-nodejs-debug.md) Informationen über das Problem diagnostizieren.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 