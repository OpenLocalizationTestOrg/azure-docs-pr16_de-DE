<properties
    pageTitle="Verbinden mit einem Computer Learning-Webdienst | Microsoft Azure"
    description="Herstellen von Verbindungen Sie mit c# oder Python auf einen Azure maschinellen Learning Webdienst Schlüssel für die Autorisierung verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Verbinden mit einem Azure-Computern Learning Web-Dienst

Der Azure maschinellen Learning Entwicklertools wird ein Webdienst-API, Vorhersagen von Eingabedaten in Echtzeit oder im Stapelverarbeitungsmodus zu erstellen. Sie verwenden Azure maschinellen Learning Studio Vorhersagen erstellen und Bereitstellen eines Azure maschinellen Learning-Webdiensts ein.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

So erhalten Informationen zum Erstellen und Bereitstellen eines Computer Learning-Webdiensts maschinellen Learning Studio verwenden:

- Ein Lernprogramm zum Erstellen einer experimentieren in Computer Learning Studio finden Sie unter [Erstellen Ihrer ersten experimentieren](machine-learning-create-experiment.md).
- Details zum Bereitstellen von eines Webdiensts finden Sie unter [Bereitstellen von einem Computer Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).
- Weitere Informationen zur maschinellen Schulung im Allgemeinen finden Sie im [Arbeitsplatzes Learning-Dokumentation](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure maschinellen Learning-Webdienst ##

Mit dem Azure maschinellen Learning-Webdienst kommuniziert eine externe Anwendung mit einem Computer Learning Workflow Punktzahl Modell in Echtzeit ein. Ein Computer Learning Web Service-Anruf gibt Vorhersageergebnisse an eine externe Anwendung. Um einen Computer Learning-Webdienst anrufen zu machen, übergeben Sie einen API-Schlüssel, der erstellt wird, wenn Sie eine Vorhersage bereitstellen. Der Computer Learning Webdienst basiert auf REST, eine beliebte Architektur der Auswahlmöglichkeiten für Web programming Projekte.

Azure maschinellen Learning weist zwei Arten von Services:

- Anforderung / Antwort-Dienst (RRS) – eine niedrige Wartezeit, hochgradig skalierbare-Dienst, bietet eine Benutzeroberfläche für die statusfreie Modelle erstellt und bereitgestellt aus dem Computer Learning Studio.
- Stapel Ausführung Service (l) – eine asynchrone service die Ergebnisse einer Stapel für Datensätze.

Weitere Informationen zu Computer Learning-Webdiensten finden Sie unter [Bereitstellen von einem Computer Learning-Webdienst](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Abrufen eines Azure maschinellen Learning Autorisierung Schlüssels ##

Wenn Sie Ihre experimentieren bereitstellen, werden API Schlüssel für den Webdienst generiert. Sie können die Tasten aus mehreren Speicherorten abrufen.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Vom Microsoft Azure maschinellen Learning-Webdienste-portal

Melden Sie sich mit dem Portal [Microsoft Azure maschinellen Learning-Webdiensten](https://services.azureml.net) .

Zum Abrufen von des API-Schlüssels für einen neuen Computer Learning-Webdienst:

1. Klicken Sie im Portal Azure maschinellen Learning-Webdienste **-Webdiensten** im oberen Menü auf.
2. Klicken Sie auf den Webdienst, für den Sie die Taste abrufen möchten.
3. Klicken Sie im oberen Menü auf **verbrauchen**.
4. Kopieren Sie und speichern Sie den **Primärschlüssel**.


Zum Abrufen von des API-Schlüssels für einen klassischen maschinellen Learning-Webdienst:

1. Klicken Sie im Portal Azure maschinellen Learning-Webdiensten auf **Klassische Webdienste** im oberen Menü.
2. Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
3. Klicken Sie auf den Endpunkt, für den Sie die Taste abrufen möchten.
3. Klicken Sie im oberen Menü auf **verbrauchen**.
4. Kopieren Sie und speichern Sie den **Primärschlüssel**.

## <a name="classic-web-service"></a>Klassische Webdienst ##

 Sie können auch einen Schlüssel für einen klassischen Webdienst aus maschinellen Learning Studio oder das Azure-Portal abrufen.

### <a name="machine-learning-studio"></a>Maschinelle Learning Studio ###

1. Klicken Sie in Computer Learning Studio auf der linken Seite auf **Webdienste** .
2. Klicken Sie auf einem Webdienst. Die **API-Taste** befindet sich auf die Registerkarte **DASHBOARD** .

### <a name="azure-portal"></a>Azure-portal ###

1. Klicken Sie auf der linken Seite auf **Computer-Schulung** .
2. Klicken Sie auf den Arbeitsbereich, in dem sich der Webdienst befindet.
3. Klicken Sie auf **Webdienste**.
4. Klicken Sie auf einem Webdienst.
5. Klicken Sie auf einen Endpunkt. Die Taste"API" wird nach unten in der unteren rechten Ecke.

## <a name="a-idconnectaconnect-to-a-machine-learning-web-service"></a><a id="connect"></a>Verbinden Sie mit einem Computer Learning-Webdienst

Sie können Herstellen einer Verbindung mit einem Computer Learning-Webdienst mit jeder Programmiersprache, die HTTP-Anforderung und Antwort unterstützt. Sie können die Beispiele in c#, Python und R aus einem Computer Learning Webdienst-Hilfeseite anzeigen.

**Computer-Learning-API-Hilfe** Computer-API Learning-Hilfe wird erstellt, bei der Bereitstellung von eines Webdiensts. Finden Sie unter [Schulung Azure-Computern Exemplarische Vorgehensweise - Webdienst bereitstellen](machine-learning-walkthrough-5-publish-web-service.md).
Die maschinelle Learning-API-Hilfe enthält Details zu einer Vorhersage Webdienst.

2. Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
3. Klicken Sie auf den Endpunkt, für den Sie die API-Hilfeseite anzeigen möchten.
3. Klicken Sie im oberen Menü auf **verbrauchen**.
3. Klicken Sie auf **Hilfe-API** unter der Anforderung-Antwort oder Stapel Ausführung Endpunkte.

**Für einen neuen Webdienst-Hilfe zur Ansicht maschinellen Learning-API**

Im Web Services-Portal Learning Azure Computer:

1. Klicken Sie im oberen Menü auf **Webdienste** .
2. Klicken Sie auf den Webdienst, für den Sie die Taste abrufen möchten.

Klicken Sie auf **verbrauchen** , um die URIs für Anforderung-Reposonse und Stapel Ausführung Dienste und Beispiel-Code in c#, R und Python zu erhalten.

Klicken Sie auf **Swagger API** um basierend Swagger Dokumentation für die APIs der angegebenen URIs aufgerufen zu erhalten.

### <a name="c-sample"></a>C#-Beispiel ###

Verwenden Sie zum Verbinden mit einem Computer Learning-Webdienst, eine **HttpClient** ScoreData übergeben. ScoreData enthält eine FeatureVector, eine n mehrdimensional Vektor numerischen Features, die die ScoreData darstellt. Sie authentifizieren an den Computer Learning-Dienst mit einer API-Taste.

Zum Verbinden mit einem Computer Learning-Webdienst, muss das **Microsoft.AspNet.WebApi.Client** NuGet-Paket installiert sein.

**Installieren von Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**

1. Veröffentlichen von UCI Dataset Download: Erwachsenen 2 Klasse Dataset Webdienst.
2. Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Wählen Sie **Microsoft.AspNet.WebApi.Client Installationspaket**aus.

**Zum Ausführen des Beispiels code**

1. Veröffentlichen "Beispiel 1: Herunterladen Dataset aus UCI: oben 2 Klasse Dataset" experimentieren, Teil der Sammlung Computer Learning Stichprobe.
2. Zuweisen von ApiKey mit dem Schlüssel von einem Webdienst. Finden Sie unter **Abrufen einen Azure maschinellen Learning Autorisierungsschlüssel** oben.
3. Weisen Sie ServiceUri mit dem URI anfordern.


### <a name="python-sample"></a>Python-Beispiel ###

Verwenden Sie zum Verbinden mit einem Computer Learning-Webdienst **urllib2** Bibliothek ScoreData übergeben. ScoreData enthält eine FeatureVector, eine n mehrdimensional Vektor numerischen Features, die die ScoreData darstellt. Sie authentifizieren an den Computer Learning-Dienst mit einer API-Taste.


**Zum Ausführen des Beispiels code**

1. Bereitstellen "Beispiel 1: Herunterladen Dataset aus UCI: oben 2 Klasse Dataset" experimentieren, Teil der Sammlung Computer Learning Stichprobe.
2. Zuweisen von ApiKey mit dem Schlüssel von einem Webdienst. Finden Sie im Abschnitt **ein Azure maschinellen Learning Autorisierung Key erhalten** am Anfang des Artikels aus.
3. Weisen Sie ServiceUri mit dem URI anfordern.
