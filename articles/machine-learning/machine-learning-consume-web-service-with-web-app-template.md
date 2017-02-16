<properties
    pageTitle="Nutzen von einem Webdienst Learning Computer mit einer Web app-Vorlage | Microsoft Azure"
    description="Mithilfe einer Web app-Vorlage in Azure Marketplace einen Vorhersage Webdienst Azure Computer interessante nutzen."
    keywords="Webdienst, Operationalization, REST-API, Computer-Schulung"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Verwenden Sie einen Webdienst Azure maschinellen Learning mit einer Web app-Vorlage

>[AZURE.NOTE] In diesem Thema werden die Techniken zu einem klassischen Webdienst verfügbar. 

Einmal Sie Vorhersage Modell entwickelt und es als mit maschinellen Learning Studio Azure Webdienst bereitgestellt haben oder Sie können mithilfe von Tools wie R oder Python, operationalized Modell mithilfe einer REST-API zugreifen.

Es gibt eine Reihe von Methoden nutzen die REST-API und Zugriff auf den Webdienst. Beispielsweise können Sie eine Anwendung schreiben, in c#, R oder Python mit dem Beispielcode für Sie generiert wird, wenn Sie den Webdienst (verfügbar auf die API-Hilfeseite im Web Service Dashboard in Computer Learning Studio) bereitgestellt. Oder Sie können die Stichprobe Microsoft Excel-Arbeitsmappe, die für Sie erstellt (auch im Web Service Dashboard in Studio verfügbar) verwenden.

Die schnellste und einfachste Möglichkeit zum Zugreifen auf Ihre Webdiensts über das Web-App-Vorlagen in der [Azure Web App-Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/)ist jedoch.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Die Web App-Vorlagen Learning Azure-Computern

Die Web app verfügbaren Vorlagen in der Azure Marketplace können eine benutzerdefinierte Web app erstellen, die Eingabedaten und die erwarteten Ergebnisse des Webdiensts bekannt ist. Sie müssen lediglich Web app Zugriff auf Ihre Webdienst und Daten zu gewähren, und die Vorlage erledigt den Rest.

Zwei Vorlagen stehen zur Verfügung:

- [Azure ML Anforderung / Antwort-Dienst Web App-Vorlage](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Vorlage für Azure ML Stapel Ausführung Dienst Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Jede Vorlage erstellt eine Stichprobe ASP.NET-Anwendung, mit dem URI-API und Schlüssel für Ihren Webdienst, und es als eine vorhandene Website so Azure bereitstellt. Die Anforderung / Antwort-Dienst (RRS) Vorlage erstellt eine Web-app, die Sie eine einzelne Zeile mit Daten an den Webdienst zum Abrufen eines einzelnen Ergebnisses senden kann. Die Vorlage Stapel Ausführung Service (l) erstellt eine Web-app, die Sie senden viele Zeilen mit Daten, die mehrere Ergebnisse abrufen kann.

Keine Codierung ist erforderlich, diese Vorlagen verwendet. Sie einfach die URI-API und ein Schlüssel eingeben und die Vorlage wird die Anwendung für Sie erstellt.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>So verwenden Sie die Vorlage Anforderung / Antwort-Dienst (RRS)

Nachdem Sie Ihre Webdienst bereitgestellt haben, folgen Sie die Schritten unten, um die Vorlage RRS Web app verwenden, wie in der folgenden Abbildung gezeigt.

![Prozess RRS Webvorlage verwenden][image1]

1. In Computer Learning Studio öffnen der Registerkarte **Webdienste** und dann den gewünschten für den Zugriff auf Webdienst. Kopieren Sie die Taste aufgeführt wird, klicken Sie unter **API-Schlüssel** und gespeichert.

    ![API-Schlüssel][image3]

2. Öffnen Sie die **Anforderung/Antwort** -API Hilfe-Seite ein. Klicken Sie am oberen Rand der Seite "Hilfe", klicken Sie unter **anfordern**kopieren Sie den Wert **URI anfordern** und gespeichert. Dieser Wert sieht wie folgt aus:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Anfrage-URI][image4]

3. Wechseln Sie zu der [Azure-Portal](https://portal.azure.com) **Login**, klicken Sie auf **neu**, suchen und auswählen **Azure ML Anforderung / Antwort-Dienst Web App**, und klicken Sie auf **Erstellen**. 

    - Geben Sie der Web app einen eindeutigen Namen. Die URL des Web app werden dieser Name, gefolgt von `.azurewebsites.net.` beispielsweise`http://carprediction.azurewebsites.net.`

    - Wählen Sie die Azure-Abonnement und Services unter denen der Webdienst ausgeführt wird.

    - Klicken Sie auf **Erstellen**.

    ![Erstellen von Web-app][image5]

4. Wenn Azure bereitstellen des Web app abgeschlossen ist, klicken Sie auf die **URL** auf der Seite Web app-Einstellungen in Azure, oder geben Sie die URL in einem Webbrowser. Beispielsweise`http://carprediction.azurewebsites.net.`

5. Wenn das Web app zuerst ausgeführt wird bitten sie Sie für die **URL der API ablegen** und **API-Schlüssel**.
Geben Sie die Werte, die Sie zuvor gespeichert haben:
    - Auf der Hilfeseite API für **API Beitrag URL** **URI anfordern**
    - **API-Schlüssel** aus dem Web Service-Dashboard für den **API-Schlüssel**.

    Klicken Sie auf **Absenden**.

    ![Geben Sie ein Beitrag URI und API-Schlüssel][image6]

6. Das Web app zeigt **Web App-Konfiguration** Seite mit den aktuellen Web Service-Einstellungen. Hier können Sie Änderungen an den Einstellungen von der Web-app verwendet vornehmen.

    > [AZURE.NOTE] Ändern der Einstellungen hier ändert nur diese für diese Web-app. Es wird nicht die Standardeinstellungen des Web Service ändern. Wenn Sie hier die **Beschreibung** ändern ändern nicht es beispielsweise die Beschreibung wird angezeigt, auf dem Web Service Dashboard in Computer Learning Studio.

    Wenn Sie fertig sind, klicken Sie auf **Änderungen speichern**, und klicken Sie dann auf **Wechseln Sie zur Startseite**.

7. Der Homepage können eingegebene Werte senden an den Webdienst, klicken Sie auf **Senden**, und das Ergebnis zurückgegeben wird.

Wenn Sie zur Seite **Konfiguration** zurückkehren möchten, wechseln Sie zu der `setting.aspx` Seite des Web app. Beispiel: `http://carprediction.azurewebsites.net/setting.aspx.` werden Sie aufgefordert, die Taste API erneut eingeben – Sie benötigen, um die Seite zu öffnen, und aktualisieren die Einstellungen.

Sie können beenden, neu starten oder löschen die Web app im Azure-Portal wie eine beliebige andere Web app. Wie es ausgeführt wird, können Sie navigieren Sie zu Start Webadresse und neue Werte eingeben.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>So verwenden Sie die Vorlage Stapel Ausführung Service (l)

Die l Web app-Vorlage können auf die gleiche Weise wie die Vorlage RRS mit dem Unterschied, dass die Web-app, die erstellt wird zulässt, Sie mehrere Datenzeilen senden und empfangen mehrerer Ergebnisse.

Die Ergebnisse von einem Stapel Ausführung Webdienst werden in einem Container Azure-Speicher gespeichert. die Eingabewerte können aus Azure-Speicher oder eine lokale Datei stammen.
Ja, Sie benötigen einen Container Azure-Speicher, die von der Web-app zurückgegebenen Ergebnisse enthalten soll, und Sie müssen die Eingabedaten vorbereiten.

![Prozess l Webvorlage verwenden][image2]

1. Befolgen Sie dasselbe Verfahren zum Erstellen des l Web app wie für die Vorlage RRS außer an:
    - Rufen Sie den **URI anfordern** von **Stapel Ausführung** API-Hilfeseite für den Webdienst.
    - Wechseln Sie zu der [Vorlage Azure ML Stapel Ausführung Dienst Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) öffnen Sie die Vorlage "l" auf Azure Marketplace, und klicken Sie auf **Web-App erstellen**.

2. Um anzugeben, wo die Ergebnisse gespeichert werden sollen, geben Sie die Ziel-Containerinformationen auf der Startseite der Web-app. Auch Geben Sie an, wo das Web app die Eingabewerte in einer lokalen Datei oder eines Containers Azure-Speicher erhält.
Klicken Sie auf **Absenden**.

    ![Informationen zu Speicher][image7]

Das Web app wird eine Seite mit Projektstatus angezeigt.
Wenn der Job abgeschlossen ist können Sie den Speicherort der Ergebnisse in Azure Blob-Speicher angegeben sein. Sie haben auch die Möglichkeit, die Ergebnisse auf eine lokale Datei herunterladen.

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen zu...

- ein Computer Learning Experimentieren mit maschinellen Learning Studio erstellt haben, finden Sie unter [Erstellen Ihrer ersten Versuch in Azure maschinellen Learning Studio](machine-learning-create-experiment.md)

- wie Sie Ihren Computer Wissensstand bereitstellen als Webdienst experimentieren, finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Learning](machine-learning-publish-a-machine-learning-web-service.md)

- Weitere Methoden zum Zugreifen auf Ihre Webdiensts Informationen, [wie einen Webdienst Azure maschinellen Learning nutzen](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
