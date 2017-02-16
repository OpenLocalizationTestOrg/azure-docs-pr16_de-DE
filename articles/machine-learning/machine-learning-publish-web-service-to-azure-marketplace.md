<properties 
    pageTitle="Veröffentlichen maschinellen Learning-Webdienst ab zu Azure Marketplace | Microsoft Azure" 
    description="So veröffentlichen Sie Ihre Azure maschinellen Learning-Webdienst zu Azure Marketplace" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Veröffentlichen von Azure-Computern Learning Webdienst in Azure Marketplace 

Die Azure Marketplace bietet die Möglichkeit zum Veröffentlichen von Azure maschinellen Learning-Webdiensten als bezahlt oder Diensten zur Nutzung von externen Kunden frei. Dieser Artikel enthält eine Übersicht über den Prozess mit Links zu den Richtlinien, die Ihnen den Einstieg. Mithilfe dieses Verfahren können Sie Ihre Webdienste für andere Entwickler in deren Clientanwendungen nutzen verfügbar machen.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Übersicht des Veröffentlichungsprozesses 

Im folgenden finden die Schritte zum Veröffentlichen eines Webdiensts Azure maschinellen Learning zu Azure Marketplace:

1. Erstellen und Veröffentlichen von einem Computer Learning Anforderung-Antwort-Dienst (RRS)
2. Den Dienst in der Herstellung bereitstellen und die API-Schlüssel und OData-Endpunktinformationen zu erhalten.
3. Verwenden Sie die URL der veröffentlichten Webdienst [Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/) veröffentlichen 
4. Nachdem übermittelt, wird Ihr Angebot überprüft und muss genehmigt werden, bevor Sie Ihre Kunden können beginnen sie erwerben. Veröffentlichungsprozesses kann einige Business Tage dauern. 

## <a name="walk-through"></a>Durchgehen
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Schritt 1: Erstellen und Veröffentlichen von einem Computer Learning Anforderung-Antwort-Dienst (RRS)###
 Wenn Sie nicht bereits geschehen, wenden Sie sich bitte betrachten Sie diese [durchgehen](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Schritt 2: Den Dienst in Herstellung bereitstellen und die API-Schlüssel und OData-Endpunktinformationen zu erhalten###
1. Wählen Sie die Option **Computer LEARNING** aus der linken Navigationsleiste im [Klassischen Azure-Portal](http://manage.windowsazure.com), und wählen Sie aus dem Arbeitsbereich. 

2. Klicken Sie auf der Registerkarte **Webdienste** auf, und wählen Sie den Webdienst aus, die, den Sie zum Marketplace veröffentlichen möchten.

    ![Azure Marketplace][workspace]

3. Wählen Sie den Endpunkt, den Sie haben den Marketplace nutzen möchten. Wenn Sie keine zusätzliche Endpunkte erstellt haben, können Sie den **Standard** -Endpunkt auswählen.

4. Nachdem Sie auf den Endpunkt geklickt haben, werden Sie die **Taste API**finden Sie unter sein. Sie benötigen diese Information Informationstyp höher in Schritt 3, also erstellen Sie eine Kopie davon.

    ![Azure Marketplace][apikey]

5. Klicken Sie auf die Methode **Anforderung/Antwort** an diesem Punkt, dass wir für die Veröffentlichung Stapel Ausführung Services zum Marketplace nicht unterstützen. Dauert, die Sie auf der Hilfeseite API für die Anfrage/Antwort-Methode.

6. Kopieren Sie die **Adresse der OData-Endpunkt**, benötigen Sie diese Informationen später in Schritt 3.

    ![Azure Marketplace][odata]




Bereitstellen des Diensts in Betrieb.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Schritt 3: Verwenden Sie die URL der veröffentlichten Webdienst veröffentlichen Azure Marketplace (DataMarket)###

1.  Navigieren Sie zur [Azure Marketplace (Data Market)](http://datamarket.azure.com/home) 
2.  Klicken Sie auf den Link " **Veröffentlichen** " am oberen Rand der Seite. Dadurch gelangen Sie zum [Microsoft Azure Veröffentlichungsportal](https://publish.windowsazure.com)
3.  Klicken Sie auf Abschnitt **Herausgeber** als eines Herausgebers registrieren.
4.  Wenn Sie ein neues Angebot erstellen möchten, wählen Sie **Data Services**, und klicken Sie auf **Erstellen eines neuen Data Service**. 
 
    ![Azure Marketplace][image1]

    <br />


5.  Klicken Sie unter **Pläne** finden Sie Informationen zu Ihrem Angebot, einschließlich eines Preisgestaltung Plans aus. Entscheiden Sie, ob Sie einen kostenlosen oder kostenpflichtigen Dienst anbieten werden. Bieten Sie um gezahlt, Zahlungsinformationen wie Ihre Bank und Steuer Informationen ein.

6.  Geben Sie unter **Marketing** Informationen über Ihr Angebot, wie etwa Titel und Beschreibung für Ihr Angebot aus.

7.  Sie können unter **Preise** setzen Sie den Preis für Ihre Pläne für bestimmte Länder oder lassen Sie das System "Autoprice" Ihr Angebot.

8. Klicken Sie auf der Registerkarte **Data Service** auf **Webdienst** als **Datenquelle**.

    ![Azure Marketplace][image2]

9.  Erhalten Sie die Web Service URL und API-Taste im klassischen Azure-Portal an, wie in Schritt 2 oben beschrieben.

10. Fügen Sie im Dialogfeld Setup Marketplace Data Service in das Textfeld **Dienst-URL** die Adresse der OData-Endpunkt.

11. Wählen Sie für die **Authentifizierung** **Kopfzeile** als **Authentifizierung Farbschema**aus.

    - Geben Sie "Autorisierung" für den **Namen der Kopfzeile**ein.
    - Geben Sie für den **Wert der Kopfzeile**"Person" (ohne die Anführungszeichen), klicken Sie auf **die LEERTASTE** , und fügen Sie die API-Taste.
    - Aktivieren Sie das Kontrollkästchen **diesen Dienst ist OData** .
    - Klicken Sie auf **Verbindung testen,** um die Verbindung zu testen.

12. Klicken Sie unter **Kategorien**stellen Sie sicher, dass **Computer Learning** ausgewählt ist.

13. Wenn Sie fertig sind klicken Sie auf **Veröffentlichen**, und **Drücken Sie zum Staging**auf alle Metadaten für Ihr Angebot, eingeben. Zu diesem Zeitpunkt benachrichtigt Sie alle verbleibenden Probleme, die Sie beheben müssen.

14. Nachdem Sie die Vervollständigung von alle offenen Probleme sichergestellt haben, klicken Sie auf **Anfordern der Genehmigung an Herstellung übertragen**. Veröffentlichungsprozesses kann einige Business Tage dauern. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
