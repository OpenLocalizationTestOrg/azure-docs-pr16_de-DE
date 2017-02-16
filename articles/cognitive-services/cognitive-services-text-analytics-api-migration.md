<properties
    pageTitle="Upgrade auf Version 2 der Text Analytics API | Microsoft Azure"
    description="Learning Text Analytics - Upgrade auf Version 2 Azure-Computern"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Upgrade auf Version 2 der Text Analytics-API #

In diesem Handbuch gelangen Sie über die Vorgehensweise zum Aktualisieren Ihres Codes aus, die [erste Version der API](../machine-learning/machine-learning-apps-text-analytics.md) bei der Verwendung von der zweiten Version verwenden. 

Wenn Sie nicht die API verwendet haben und mehr erfahren möchten, können Sie **[Weitere Informationen über die API Hier erfahren Sie,](//go.microsoft.com/fwlink/?LinkID=759711)** oder **[Führen Sie die Schnellstartübersicht](//go.microsoft.com/fwlink/?LinkID=760860)**. In der technischen Referenz finden Sie in der **[API Definition](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Teil 1. Erhalten Sie einen neuen Product key ###

Zuerst müssen Sie einen neuen API Product Key vom **Azure-Portal**zu erhalten:

1. Navigieren Sie zu der Text Analytics-Dienst durch den [Cortana Intelligence-Katalog](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Hier finden Sie auch Links zu der Dokumentation und den Codebeispielen.

1. Klicken Sie auf **Anmelden**. Dieser Link gelangen Sie zu der Azure-Verwaltungsportal, auf der Sie sich für den Dienst anmelden können.

1. Wählen Sie einen Plan aus. Sie können die **kostenlose Ebene für 5.000 Transaktionen/Monat**auswählen. Während ein kostenloser Plan ist, werden Sie nicht für die Verwendung des Diensts berechnet. Sie müssen bei Ihrem Abonnement Azure anmelden. 

1. Nachdem Sie sich für Text Analytics haben registriert, erhalten Sie einen **API-Schlüssel**angegeben sein. Kopieren Sie die Schlüssel, wie Sie ihn benötigen, wenn die API-Dienste verwenden.

### <a name="part-2-update-the-headers"></a>Teil 2. Aktualisieren Sie die Überschriften ###

Aktualisieren der gesendete Kopfzeile Werte wie unten dargestellt. Beachten Sie, dass die Taste Konto nicht mehr codiert ist.

**Version 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Version 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Teil 3. Aktualisieren Sie die base-URL ###

**Version 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Version 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Webpart 4a. Aktualisieren Sie die Formate für Grüße, Key Ausdrücke und Sprachen ###

#### <a name="endpoints"></a>Endpunkte ####

Abrufen von Endpunkten sind jetzt veraltet, damit alle Eingaben als Beitrag Anforderung gesendet werden soll. Aktualisieren Sie die Endpunkte auf diejenigen abgebildet.

| |Einzelnen Endpunkt Version 1|Version 1 Stapel Endpunkt|Version 2-Endpunkt|
|---|---|---|---|
|Anruftyp|Erhalten|Bereitstellen|Bereitstellen|
|Grüße|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Key Ausdrücke|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Sprachen|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Eingabeformate ####

Beachten Sie, dass das einzige Beitrag Format jetzt akzeptiert wird, sodass Sie jede Eingabe formatierenden sollte die zuvor die Endpunkte einzelnes Dokument entsprechend verwendet. Eingaben sind nicht Groß-/Kleinschreibung beachtet.

**Version 1 (Stapel)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Die Ausgabe Grüße ####

**Version 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Die Ausgabe von Key Ausdrücke ####

**Version 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Die Ausgabe von Sprachen ####


**Version 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Teil 4 b. Aktualisieren Sie die Formate für Themen ###

#### <a name="endpoints"></a>Endpunkte ####

| |Version 1-Endpunkt | Version 2-Endpunkt|
|---|---|---|
|Übermitteln Sie für Thema Erkennung (POST)|```StartTopicDetection```|```topics```|
|Abrufen von Ergebnissen Thema (GET)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Eingabeformate ####

**Version 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Bereitstellen von Ergebnissen ####

**Version 1 (POST)**

In früheren Versionen Sie abschließend der Auftrag möchten Sie die folgende JSON-Ausgabe erhalten, in dem die Auftrags-IDs zu einer URL zum Abrufen der Ausgabe angefügt werden möchten.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Version 2 (POST)**

Die Antwort wird jetzt gehören Header-Wert wie folgt, wobei `operation-location` dient als Endpunkt für die Ergebnisse abgefragt werden soll:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Der Ergebnisse ####

**Version 1 (GET)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2 (GET)**

Wie zuvor, wird **die Ausgabe daher in regelmäßigen Abständen** (vorgeschlagene Periode ist pro Minute), bis die Ausgabe zurückgegeben. 

Wenn die Themen-API wurde abgeschlossen, ein Status Lesebereich `succeeded` zurückgegeben wird. Dies umfasst dann die Ausgabeergebnisse im unten gezeigten Format:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Teil 5. Testen Sie es aus! ###

Sie sollten jetzt mit glänzen sein! Testen Sie den Code mit einem kleinen Beispiel, um sicherzustellen, dass Ihre Daten verarbeitet werden können.
