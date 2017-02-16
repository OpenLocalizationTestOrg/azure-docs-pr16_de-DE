<properties
    pageTitle="Problembehandlung bei der Retraining eines Azure maschinellen Learning klassischen Webdiensts | Microsoft Azure"
    description="Identifizieren Sie und beheben Sie häufige Probleme ist aus, wenn Sie das Modell für einen Azure maschinellen Learning Webdienst Umschulung sind."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Problembehandlung bei der Umschulung eines Azure maschinellen Learning klassischen Webdiensts

## <a name="retraining-overview"></a>Umschulung (Übersicht)

Wenn Sie eine Vorhersage experimentieren als Punktzahl Webdienst bereitstellen ist es ein statisches Modell aus. Sobald neue Daten verfügbar sind oder wenn der Consumer der API ihre eigenen Daten enthält, muss das Modell einarbeiten. 

Umfassende Informationen des Prozesses von einem Webdienst klassische retraining finden Sie unter [Programmgesteuertes neu trainieren maschinellen Learning Modelle](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Umschulung Prozess

Wenn Sie den Webdienst neu trainieren müssen, müssen Sie einige zusätzlichen Textstellen hinzufügen:

* Ein Webdienst aus dem Versuch, Schulung bereitgestellt. Die experimentieren müssen ein **Web Service Ausgabe** Modul, die das Ergebnis des Moduls **Zug Modell** angefügt.  

    ![Fügen Sie die Ausgabe der Web-Dienst in das Datenmodell Zug.][image1]

* Einen neuen Endpunkt, Ihre Punktzahl Webdienst hinzugefügt wurde.  Sie können den Endpunkt programmgesteuert mithilfe des Stichprobe Codes neu trainieren maschinellen Learning-Modelle programmgesteuert referenzierte hinzufügen Thema oder über das Azure klassischen Portal.

Den C#-Code aus den Schulung Webdienst-API Hilfeseite können dann Modell neu trainieren. Nachdem Sie die Ergebnisse ausgewertet haben und mit ihnen zufrieden sind, aktualisieren Sie ausgebildete Modell bewerten Webdienst mithilfe des neuen Endpunkts, den Sie hinzugefügt haben.

Alle Teile direkte sind die wichtigsten Schritte, die Sie ausführen müssen, um das Modell neu trainieren wie folgt:

1.  Rufen Sie den Webdienst Schulung: der Anruf wird an den Stapel Ausführung Service (l), nicht die anfordern Antwort Service (RRS). Den C#-Code können auf der Hilfeseite API Sie um den Anruf zu tätigen. 
2.  Suchen Sie die Werte für die *BaseLocation*, *RelativeLocation*und *SasBlobToken*: Diese Werte werden in der Ausgabe aus den Anruf an den Webdienst Schulung zurückgegeben. 
      ![die Ausgabe der Stichprobe retraining und die Werte BaseLocation, RelativeLocation und SasBlobToken werden angezeigt.][image6]
3.  Aktualisieren Sie den hinzugefügten Endpunkt aus dem Bewertungssystem Webdienst mit dem neuen ausgebildeten Modell: mithilfe des programmgesteuert, in die neu trainieren maschinellen Learning-Modellen bereitgestellten Beispielcodes aktualisieren Sie den neuen Endpunkt Sie das Modell wird mit dem Modell neu ausgebildeten bewertet aus dem Webdienst Schulung hinzugefügt.

## <a name="common-obstacles"></a>Allgemeine Hindernisse

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Überprüfen Sie, wenn Sie die richtige URL PATCH haben

Die PATCH-URL, die Sie verwenden, muss das Schema der neuen Punktzahl Endpunkt an den Punktzahl Webdienst hinzugefügte zugeordnet sein. Es gibt eine Reihe von Methoden zum Abrufen der URL PATCH aus:

**Die Option 1: Programmgesteuertes**

So erhalten Sie die richtige PATCH-URL:

1.  Führen Sie den [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) Beispiel-Code ein.
2.  Die Ausgabe der AddEndpoint suchen Sie den Wert *HelpLocation* , und kopieren Sie die URL.

    ![HelpLocation in der Ausgabe des Beispiels AddEndpoint.][image2]

3.  Fügen Sie die URL in einem Browser, um zu einer Seite zu navigieren, die Hilfelinks für den Webdienst enthält.
4.  Klicken Sie auf den Link **Update Ressourcen** zum Öffnen der Hilfeseite Patch.

**Option 2: Verwenden des Azure klassischen-Portals**

1.  Melden Sie sich zum [Azure klassischen Portal](https://manage.windowsazure.com)aus.
2.  Öffnen der Registerkarte maschinellen Schulung. 
     ![Registerkarte für maschinelle Schiefer.][image4]
3.  Klicken Sie auf Ihren Arbeitsbereichsnamen, klicken Sie dann **Webdienste**.
4.  Klicken Sie auf der Punktzahl Webdienst, mit dem Sie arbeiten. (Wenn Sie den Standardnamen des Webdiensts nicht geändert haben, wird diese in [bewerten Exp.] beenden.)
5.  Klicken Sie auf **Endpunkt hinzufügen**.
6.  Nachdem Sie der Endpunkt hinzugefügt haben, klicken Sie auf den Endpunktnamen. Klicken Sie dann auf **Ressource aktualisieren** , um den Patch Hilfeseite zu öffnen.

>[AZURE.NOTE] Wenn Sie den Endpunkt an den Webdienst Schulung anstelle der Vorhersage Webdienst hinzugefügt haben, wird die folgende Fehlermeldung, wenn Sie klicken Sie auf den Link **Update Ressource** : Es tut uns leid, aber dieses Feature ist nicht in diesem Kontext oder unterstützt. Dieser Webdienst weist keine Ressourcen aktualisiert werden kann. Wir entschuldigen uns für Ihr Verständnis und zur Verbesserung der dieser Workflow arbeiten.

![Neues Endpunkt Dashboard.][image3]

Die Hilfeseite PATCH enthält den PATCH-URL, die Sie verwenden müssen und gegeben, die Sie verwenden können, sie aufzurufen.

![Patch-URL.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Stellen Sie sicher, dass Sie den richtigen Punktzahl Endpunkt aktualisieren

* Führen Sie die Schulung Webdienst nicht patch: der Patch-Vorgang im Punktzahl Webdienst ausgeführt werden muss.
* Gehen Sie wie folgt nicht den standardmäßigen Endpunkt auf Webdienst patch: der Patch Vorgang muss ausgeführt werden, klicken Sie auf der neuen Punktzahl Webdienst-Endpunkt, die Sie hinzugefügt haben.

Sie können überprüfen, welche Webdienst der Endpunkt des Besuchs des Azure klassischen Portals aktiviert ist. 

>[AZURE.NOTE] Achten Sie darauf, dass Sie den Endpunkt Vorhersage Webdienst nicht den Schulung Webdienst hinzufügen möchten. Wenn Sie sowohl eine Schulung und einem Webdienst Vorhersage ordnungsgemäß bereitgestellt haben, sollte zwei getrennten aufgeführten Webdienste angezeigt werden. Die Vorhersage Webdienst sollte mit "[Vorhersage exp.]" enden.

1.  Melden Sie sich zum [Azure klassischen Portal](https://manage.windowsazure.com)aus.
2.  Öffnen der Registerkarte maschinellen Schulung. 
     ![Maschinelle Learning Arbeitsbereich Benutzeroberfläche.][image4]
3.  Wählen Sie aus dem Arbeitsbereich.
4.  Klicken Sie auf **Webdienste**.
5.  Wählen Sie Ihre Vorhersage Webdienst.
6.  Stellen Sie sicher, dass Ihre neue Endpunkt an den Webdienst hinzugefügt wurde.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Überprüfen Sie den Arbeitsbereich, dem der Webdienst befindet, um sicherzustellen, dass sie die richtige Region ist

1.  Melden Sie sich zum [Azure klassischen Portal](https://manage.windowsazure.com)aus.
2.  Wählen Sie Computer Learning aus dem Menü aus.
      ![Maschinelle Learning Region Benutzeroberfläche.][image4]
3.  Überprüfen Sie den Speicherort Ihres Arbeitsbereichs aus.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png