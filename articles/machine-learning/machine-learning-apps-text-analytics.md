<properties
    pageTitle="Computer-Learning-APIs: Text Analytics | Microsoft Azure"
    description="Microsoft Computer Learning Text Analytics-APIs können verwendet werden, unstrukturierten Text für Grüße Analyse, Extraktion von wichtig sind die Worte, Spracherkennung und Thema Erkennung zu analysieren."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Computer-Learning-APIs: Text Analytics für Grüße, wichtiger Ausdruck Extraktion, Spracherkennung und Thema Erkennung

>[AZURE.NOTE] Dieses Handbuch richtet sich an Version 1 der API. Version 2, [**finden Sie in diesem Dokument**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Version 2 ist nun die bevorzugte Version dieser API.

## <a name="overview"></a>(Übersicht)

Der Text Analytics-API ist eine Suite mit Azure maschinellen Learning integrierten Text Analytics [-Webdiensten](https://datamarket.azure.com/dataset/amla/text-analytics) . Die API kann verwendet werden, um unstrukturierte Text für Aufgaben wie Grüße Analyse, Extraktion von wichtig sind die Worte, Spracherkennung und Thema Erkennung zu analysieren. Keine Schulungsdaten ist erforderlich, um diese API verwenden: Zeigen Sie einfach Ihre Textdaten. Diese API verwendet erweiterte natürlicher Sprache Verarbeitung Techniken, um am besten in der Klasse Vorhersagen zu übermitteln.

Text Analytics in Aktion sehen in unserer [Demo-Website](https://text-analytics-demo.azurewebsites.net/), Sie, wo Sie [Beispiele](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) auch zum Implementieren der Text Analytics in c# und Python finden können.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Grüße Analyse

Die API gibt einen numerischen Wert zwischen 0 und 1 zurück. Bewertungen der Lesbarkeit nahe 1 anzugeben positive Grüße, während Bewertungen der Lesbarkeit nahe 0 negative Grüße anzugeben. Grüße Punktzahl wird mithilfe von Klassifizierung Techniken generiert. Die Eingabewerte Features der Klassifizierung gehören n-g, Features von Tags Teil der Sprachein-/Ausgabe und Word ein eingefügtes generiert. Englisch ist derzeit die einzige unterstützte Sprache.
 
## <a name="key-phrase-extraction"></a>Extraktion von wichtig sind die Worte

Die API gibt eine Liste von Zeichenfolgen, die die wichtigsten Gesprächsthemen Eingabewerte Text bezeichnet. Wir setzen Techniken aus anspruchsvolle Verarbeitung natürlicher Sprache des Microsoft Office-Toolkit. Englisch ist derzeit die einzige unterstützte Sprache.

## <a name="language-detection"></a>Spracherkennung

Die API gibt die erkannte Sprache und eine numerische Bewertung zwischen 0 und 1 zurück. Ergebnisse in der Nähe 1 anzugeben (100 %) vor, dass die identifizierte Sprache true ist. Insgesamt 120 Sprachen werden unterstützt.

## <a name="topic-detection"></a>Thema Erkennung

Dies ist eine neu veröffentlichte API, welche gibt die Top Themen für eine Liste der erkannten Texteinträge übermittelt. Wird ein Thema mit einem Key Ausdruck, der eine sein kann identifiziert oder weitere verwandte Wörter. Diese API erfordert mindestens 100 Text-Datensätze, die gesendet werden, aber zur Erkennung von Themen Unterstützung für Tausende von Datensätzen ausgelegt ist. Beachten Sie, dass diese API 1 Transaktion pro Textdatensatz übermittelten Gebühren. Die API soll gut für kurze, human geschriebenem Text wie Prüfungen und Benutzerfeedback funktioniert.

---

## <a name="api-definition"></a>API Definition

### <a name="headers"></a>Kopfzeilen

Stellen Sie sicher, dass Sie die richtigen Überschriften in Ihrer Anforderung enthält, die wie folgt werden sollen:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Sie können Ihren kontoschlüssel aus Ihrem Konto auf dem [Azure Data Market](https://datamarket.azure.com/account/keys)suchen. Beachten Sie, dass aktuell nur JSON für Eingabe- und Formate akzeptiert wird. XML wird nicht unterstützt.

---

## <a name="single-response-apis"></a>Einzelne Antwort-APIs

### <a name="getsentiment"></a>GetSentiment

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Beispiel für eine Anforderung**

In den Anruf unter bitten wir Grüße Analyse nach dem Begriff "Hallo Welt":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Dadurch wird wie folgt eine Antwort zurückgegeben:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Beispiel für eine Anforderung**

In dem Anruf werden wir bitten, dass wichtigsten Ausdrücke im Text gefunden "Es eine wunderbare Hotel um bei mit eindeutigen schöne und angezeigter Personal bleiben wurde":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Dadurch wird wie folgt eine Antwort zurückgegeben:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Beispiel für eine Anforderung**

In den folgenden GET-Anruf anfordern wir für die Absicht für die wichtigsten Ausdrücke in den Text *Hallo Welt*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Dadurch wird wie folgt eine Antwort zurückgegeben:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Optionale Parameter**

`NumberOfLanguagesToDetect`ist ein optionaler Parameter. Die Standardeinstellung ist 1.

---

## <a name="batch-apis"></a>Stapel-APIs

Der Text Analytics-Dienst können Sie Grüße und Key-Satz Extraktionen im Stapelverarbeitungsmodus ausführen. Beachten Sie, dass jede der Datensätze zählt als eine Transaktion bewertet. Beispiel: Wenn Sie die Stimmung Ausdrücken für 1000 Datensätzen in einer einzelnen Anruf anfordern werden 1000 Transaktionen belastet.

Beachten Sie, dass die IDs in das System eingegeben die IDs vom System zurückgegeben werden. Der Webdienst überprüft nicht, dass diese IDs eindeutig sind. Es ist den Zuständigkeitsbereich den Anrufer Eindeutigkeit zu überprüfen. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Beispiel für eine Anforderung**

In den Beitrag Anruf unter bitten wir für die Ansichten die Ausdrücke "Hallo Welt", "Hallo Foo World" und "Meine Welt" in den Textkörper der Anfrage:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Anforderungstexts:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

In der Antwort unten erhalten Sie die Liste der Ergebnisse Ihres Texts Ids zugeordnet:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Beispiel für eine Anforderung**

In diesem Beispiel werden wir für die Liste der Ansichten für die wichtigsten Ausdrücke in den folgenden Texten anfordern: 

* "Es wurde ein wunderbare Hotel um bei mit eindeutigen schöne und angezeigter Personal bleiben"
* "Es wurde ein von außergewöhnlichen Build Konferenz mit sehr interessante spricht"
* "Der Datenverkehr wurde sehr schlecht, ich aufgewendet drei Stunden gezeigt, die Airport"

Diese angefordert wird als Beitrag Anruf an den Endpunkt:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Anforderungstexts:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

In der Antwort unten erhalten Sie die Liste der wichtigsten Ausdrücke Ihres Texts Ids zugeordnet:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Englisch",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Französisch",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Thema Erkennung APIs

Dies ist eine neu veröffentlichte API, welche gibt die Top Themen für eine Liste der erkannten Texteinträge übermittelt. Wird ein Thema mit einem Key Ausdruck, der eine sein kann identifiziert oder weitere verwandte Wörter. Beachten Sie, dass diese API 1 Transaktion pro Textdatensatz übermittelten Gebühren.

Diese API erfordert mindestens 100 Text-Datensätze, die gesendet werden, aber zur Erkennung von Themen Unterstützung für Tausende von Datensätzen ausgelegt ist.


### <a name="topics--submit-job"></a>Themen – Absenden Position

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Beispiel für eine Anforderung**


In den folgenden Beitrag Anruf werden wir Themen für eine Reihe von 100 Artikeln, bitten, wobei die vor- und Nachnamen von Artikeln angezeigt werden und zwei StopPhrases enthalten sind.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Anforderungstexts:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

In der Antwort unten erhalten Sie die Auftrags für den gesendeten Auftrag an:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Eine Liste der einzelne Wörter oder mehrere Word-Sätze, die nicht als Themen zurückgegeben werden soll. Kann verwendet werden, um sehr generische Themen zu filtern. Angenommen, in einem Dataset zu Hotel Prüfungen "Hotel" und "hostel" sinnvolle beenden Ausdrücke möglicherweise.  

### <a name="topics--poll-for-job-results"></a>Themen – Umfrage für Position Ergebnisse

**URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Beispiel für eine Anforderung**

Übergeben der Auftrags aus dem Schritt 'Absenden Position' zurückgegeben, um die Ergebnisse abzurufen. Es empfiehlt sich, dass Sie diesen Endpunkt pro Minute bis Status aufrufen = 'Abgeschlossen' in der Antwort. Es dauert ungefähr 10 Minuten für ein Projekt abgeschlossen oder mehr für Projekte mit vielen Tausende von Datensätzen.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Während verarbeitet wird, wird die Antwort wie folgt aussehen:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Die API gibt Ausgabe im JSON-Format in folgendem Format ein:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Die Eigenschaften für jeden Teil der Antwort sind wie folgt aus:

**TopicInfo Eigenschaften**

| Schlüssel | Beschreibung |
|:-----|:----|
| TopicId | Ein eindeutiger Bezeichner für jedes Thema. |
| Punktzahl | Zählen von Datensätzen, Thema zugewiesen ist. |
| KeyPhrase | Ein Zusammenfassen von Wort oder Satz nach dem Thema. 1 oder mehrere Wörter sind möglich. |

**TopicAssignment Eigenschaften**

| Schlüssel | Beschreibung |
|:-----|:----|
| ID | Bezeichner für den Eintrag. Die ID, die sich in der Eingabe entspricht. |
| TopicId | Die Thema-ID die der Eintrag zugewiesen wurde. |
| Abstand | KONFIDENZ, der Eintrag mit dem Thema gehört. Abstand näher an 0 (null) gibt höhere vertrauen. |
