<properties
    pageTitle="Neu Trainieren von einem Webdienst klassischen | Microsoft Azure"
    description="Informationen Sie zum programmgesteuerten neu trainieren ein Modell und aktualisieren den Webdienst, um das Modell neu ausgebildeten Azure Computer interessante verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-classic-web-service"></a>Neu einen Webdienst klassischen trainieren

Die Vorhersage Webdienst, den Sie bereitgestellt wird standardmäßig Endpunkt bewerten. Standardendpunkte werden mit der ursprünglichen Schulung und Versuche bewerten synchron gehalten, und kann für den standardmäßigen Endpunkt ausgebildete Modell kann nicht ersetzt werden. Wenn Sie den Webdienst neu trainieren, müssen Sie einen neuen Endpunkt an den Webdienst hinzufügen. 

## <a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen eine Schulung experimentieren und eine Vorhersage experimentieren eingerichtet haben Siehe [neu trainieren maschinellen Learning programmgesteuert Modelle](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Die Vorhersage experimentieren muss als Webdienst learning klassischen Computer bereitgestellt werden. 
 
Weitere Informationen zum Bereitstellen von Web Services finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Hinzufügen eines neuen Endpunkts
 
Der Vorhersage-Webdienst, den Sie bereitgestellt enthält einen Standardwert bewerten Endpunkt, die mit der ursprünglichen Schulung synchron gehalten wird und Versuche ausgebildeten Modell wird bewertet. Wenn Ihr Webdienst mit einem neuen ausgebildeten Modell aktualisieren möchten, müssen Sie einen neuen Punktzahl Endpunkt erstellen. 

So erstellen Sie einen neuen Punktzahl Endpunkt Vorhersage Web-Dienst, die mit dem Modell ausgebildeten aktualisiert werden können:

>[AZURE.NOTE] Achten Sie darauf, dass Sie den Endpunkt Vorhersage Webdienst nicht den Schulung Webdienst hinzufügen möchten. Wenn Sie sowohl eine Schulung und einem Webdienst Vorhersage ordnungsgemäß bereitgestellt haben, sollte zwei getrennten aufgeführten Webdienste angezeigt werden. Die Vorhersage Webdienst sollte mit "[Vorhersage exp.]" enden.

Es gibt drei Möglichkeiten, in denen Sie einen neuen Endpunkt zu einem Webdienst hinzufügen können:

3. Programmgesteuert
1. Verwenden des Microsoft Azure-Webdienste-Portals
2. Verwenden des Azure klassischen-Portals

### <a name="programmatically-add-an-endpoint"></a>Fügen Sie einen Endpunkt programmgesteuert hinzu

Sie können die Punktzahl Endpunkte mit dem in diesem [Github Repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs)bereitgestellten Beispielcode hinzufügen.

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a>Verwenden des Portals Microsoft Azure-Webdiensten zum Hinzufügen von außen liegenden Tabellenblättern

1. Klicken Sie in Computer Learning Studio auf die Spalte linken Navigationsbereich auf Webdienste.
2. Klicken Sie am Ende der Web Service-Dashboard **Verwalten Endpunkte Vorschau**auf.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie einen Namen und eine Beschreibung für den neuen Endpunkt an. Wählen Sie aus der Protokolliergrads und ob Beispieldaten aktiviert ist. Weitere Informationen zur Protokollierung finden Sie unter [Aktivieren der Protokollierung für maschinelle Learning-Webdiensten](machine-learning-web-services-logging.md).

### <a name="use-the-azure-classic-portal-to-add-an-endpoint"></a>Verwenden des klassischen Azure-Portals Hinzufügen von außen liegenden Tabellenblättern

1. Melden Sie sich mit dem [klassischen Azure-Portal](https://manage.windowsazure.com)aus.
2. Klicken Sie auf **Computer Schulung**, klicken Sie im Menü links.
3. Klicken Sie unter Name klicken Sie auf den Arbeitsbereich, und klicken Sie dann auf **Webdienste**.
4. Klicken Sie unter Name auf **Erhebung Modell [Vorhersage exp.]**.
5. Klicken Sie am unteren Rand der Seite auf **Hinzufügen Endpunkt**. Weitere Informationen zum Hinzufügen von Endpunkten finden Sie unter [Erstellen von Endpunkten](machine-learning-create-endpoint.md). 

## <a name="update-the-added-endpoints-trained-model"></a>Aktualisieren Sie des hinzugefügten Endpunkts ausgebildeten Modell

Um die retraining abzuschließen, müssen Sie ausgebildete Modell des neuen Endpunkts aktualisieren, die Sie hinzugefügt haben.

* Wenn Sie den neuen Endpunkt über das klassische Azure-Portal hinzugefügt haben, können Sie den neuen Endpunkt Namen im Portal, klicken Sie dann auf den **UpdateResource** Link können Sie die URL zu gelangen, Sie den Endpunkt des Modells aktualisieren müssen, klicken.
* Wenn Sie den Endpunkt, wobei den Stichprobe Code hinzugefügt haben, sind dies Orts für die Hilfe-URL, die durch den Wert *HelpLocationURL* in der Ausgabe identifiziert.
 
Die Pfad-URL abrufen:

1.  Kopieren Sie und fügen Sie die URL in Ihrem Browser.
2.  Klicken Sie auf den Link Ressource aktualisieren.
3.  Kopieren Sie die URL der Beitrag der Anfrage PATCH. Beispiel:

        PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2

Das Modell ausgebildete können jetzt um den Endpunkt Bewertungssystem zu aktualisieren, den Sie zuvor erstellt haben.

Im folgenden Code wird gezeigt, wie mithilfe der *BaseLocation*, *RelativeLocation*, *SasBlobToken*und PATCH-URL den Endpunkt aktualisiert werden kann.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };
    
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);
    
            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);
    
                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }
    
                // Do what you want with a successful response here.
            }
        }
    }

Die *ApiKey* und die *EndpointUrl* für den Anruf können über Endpunkt Dashboard abgerufen werden.

Der Wert des Parameters *Name* in *Ressourcen* sollte den Namen der Ressource des Modells ausgebildeten gespeicherten den Vorhersage Versuch übereinstimmen. Um den Namen der Ressource zu erhalten:

1.  Melden Sie sich mit dem [klassischen Azure-Portal](https://manage.windowsazure.com)aus.
2.  Klicken Sie auf **Computer Schulung**, klicken Sie im Menü links.
3.  Klicken Sie unter Name klicken Sie auf den Arbeitsbereich, und klicken Sie dann auf **Webdienste**.
4.  Klicken Sie unter Name auf **Erhebung Modell [Vorhersage exp.]**.
5.  Klicken Sie auf der neuen Endpunkt, die, den Sie hinzugefügt haben.
6.  Klicken Sie auf dem Dashboard Endpunkt **Update Ressourcen**auf.
7.  Auf der Seite Update Ressource API-Dokumentation für den Webdienst können Sie den **Namen der Ressource** unter **Aktualisierbar Ressourcen**finden.

Wenn Ihr Token SAS abläuft, bevor Sie abgeschlossen haben, aktualisieren den Endpunkt, müssen Sie einen GET mit den Auftrag Id zum Abrufen der ein frisch Token durchführen.

Wenn der Code erfolgreich ausgeführt wurde, sollte der neue Endpunkt verwenden retrained Modell in etwa 30 Sekunden.

##<a name="summary"></a>Zusammenfassung

Verwenden die APIs Umschulung, können Sie ausgebildete Modell eines Webdiensts Vorhersage UFI-Szenarien, z. B. aktualisieren:

* Periodische Modell Umschulung mit neuen Daten.
* Verteilung eines Modells für Kunden mit dem Ziel lassen sie das Modell verwenden ihre eigenen Daten neu trainieren.

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung bei der Umschulung eines klassischen Azure maschinellen Learning-Webdiensts](machine-learning-troubleshooting-retraining-models.md)