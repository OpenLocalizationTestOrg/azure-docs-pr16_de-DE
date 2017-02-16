<properties
    pageTitle="Schnellstarthandbuch: Computer Learning Text Analytics-APIs | Microsoft Azure"
    description="Azure-Computern Learning Text Analytics – Schnellstarthandbuch"
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

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Erste Schritte mit den Text Analytics-APIs Grüße, Key Ausdrücke, Themen und Sprache erkennen

<a name="HOLTop"></a>

Dieses Dokument beschreibt, wie zur integrierten Ihres Dienst oder einer Anwendung, die [Text Analytics-APIs](//go.microsoft.com/fwlink/?LinkID=759711)verwenden.
Diese APIs können Sie um von Ihrem Text Grüße, Key Ausdrücke, Themen und die Sprache zu erkennen. [Klicken Sie hier, um eine interaktive Demo die Erfahrung anzuzeigen.](//go.microsoft.com/fwlink/?LinkID=759712)

Lizenzinformationen finden Sie die [API Definitionen](//go.microsoft.com/fwlink/?LinkID=759346) für technische Dokumentation für die APIs.

Dieses Handbuch richtet sich an die APIs Version 2. Ausführliche Informationen zum Version 1 der APIs, [schlagen Sie in diesem Dokument](../machine-learning/machine-learning-apps-text-analytics.md).

Am Ende dieses Lernprogramms werden Sie Programmgesteuertes erkennen:

- **Grüße** - Text positiv oder negativ ist?

- **Taste Ausdrücke** – Was Personen sind in einem einzigen Artikel besprochen?

- **Themen** – Was Personen sind über zahlreiche Artikel besprochen?

- **Sprachen** - folgenden Sprachen Text ist geschrieben?

Beachten Sie, dass diese API 1 Transaktion pro gesendetes Dokument Gebühren. Beispiel: Wenn Sie die Stimmung Ausdrücken für 1000 Dokumente in einer einzelnen Anruf anfordern werden 1000 Transaktionen belastet.



<a name="Overview"></a>
## <a name="general-overview"></a>Allgemeine Übersicht ##

Dieses Dokument ist eine schrittweise Anleitung. Unser Ziel ist Ihnen Schritt für Schritt durch die Schritte zum Schulen von einem Datenmodell, und klicken Sie auf Ressourcen zu verweisen, mit denen Sie es in der Herstellung gesetzt werden. In dieser Übung wird ungefähr 30 Minuten dauern.

Für diese Aufgaben Sie benötigen einen Editor und rufen Sie die REST-Endpunkten in einer Sprache Ihrer Wahl.

Lassen Sie uns beginnen

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Aufgabe 1: Sie können den Text Analytics-APIs signierenden ####

In dieser Aufgabe werden Sie für den Text Analytics-Dienst anmelden.

1. Navigieren Sie zu **Kognitive Services** im [Portal Azure](//go.microsoft.com/fwlink/?LinkId=761108) , und stellen Sie sicher, dass **Text Analytics** als 'API Typ' ausgewählt ist.

1. Wählen Sie einen Plan aus. Sie können die **kostenlose Ebene für 5.000 Transaktionen/Monat**auswählen. Während ein kostenloser Plan ist, werden Sie nicht für die Verwendung des Diensts berechnet. Sie müssen bei Ihrem Abonnement Azure anmelden. 

1. Füllen Sie die anderen Felder aus, und erstellen Sie Ihr Konto.

1. Nachdem Sie sich für Text Analytics haben registriert, suchen Sie den **API-Schlüssel**. Kopieren Sie den Primärschlüssel, wie Sie diesen benötigen, wenn die API-Dienste verwenden.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Aufgabe 2: erkennen Grüße, Key Ausdrücke und Sprachen ####

Es ist einfach zu erkennen Grüße, Key Ausdrücke und Sprachen in den Text. Sie erhalten Programmgesteuertes die gleichen Ergebnisse wie die [Demo Erfahrung](//go.microsoft.com/fwlink/?LinkID=759712) zurückgibt.

>[AZURE.TIP] Grüße Analyse empfehlen wir, dass Sie Text in Sätze aufteilen. Dadurch im Allgemeinen eine höhere Genauigkeit in Grüße Vorhersagen.

Beachten Sie, dass die unterstützten Sprachen, wie folgt sind:

| Feature | Unterstützte Sprachen |
|:-----|:----|
| Grüße | `en`(In Englisch), `es` (Spanisch), `fr` (Französisch), `pt` (Portugiesisch) |
| Key Ausdrücke | `en`(In Englisch), `es` (Spanisch), `de` (Deutsch), `ja` (Japanisch) |


1. Sie müssen die Kopfzeilen der folgende festlegen. Beachten Sie, dass JSON aktuell nur zulässigen Eingabeformat für die APIs ist. XML wird nicht unterstützt.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Als Nächstes formatieren Sie Ihrer Eingaben Zeilen in JSON. Für Grüße, Key Ausdrücke und Sprache ist das Format identisch. Beachten Sie, dass jede-ID eindeutig sein sollte und die ID vom System zurückgegeben. Ein einzelnes Dokument, das gesendet werden kann die maximale Größe ist 10 KB groß ist und die maximale Gesamtgröße der übermittelten Eingabe 1MB. Nicht mehr als 1.000 Dokumente können in einem Anruf übermittelt werden. Zins einschränken, die mit einer Geschwindigkeit von 100 Anrufen pro Minute vorhanden ist – wir empfehlen daher, dass Sie große Mengen von Dokumenten in einem einzigen Anruf einreichen. Sprache ist ein optionaler Parameter, die angegeben werden sollten wenn nichtenglischen Text zu analysieren. Ein Beispiel für die Eingabe ist abgebildet, wo der optionale Parameter `language` für Grüße Extraktion von Analyse oder Schlüssel Ausdruck enthalten sind:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Anrufen Sie **Beitrag** an das System mit der Eingabe für Grüße, Key Ausdrücke und Sprache. Die URLs werden wie folgt aussehen:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Dieser Anruf zurückgegeben werden kann, eine JSON formatierte Antwort mit den IDs und Eigenschaften erkannt. Ein Beispiel für die Ausgabe für Stimmung Ausdrücken wird unterhalb (mit Fehlerdetails ausgeschlossen) angezeigt. Wenn Grüße wird für jedes Dokument eine Bewertung zwischen 0 und 1 zurückgegeben:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Aufgabe 3: Erkennen von Themen in einem Trennen von text ####

Dies ist eine neu veröffentlichte API, welche gibt die Top Themen für eine Liste der erkannten Texteinträge übermittelt. Wird ein Thema mit einem Key Ausdruck, der eine sein kann identifiziert oder weitere verwandte Wörter. Die API soll gut für kurze, human geschriebenem Text wie Prüfungen und Benutzerfeedback funktioniert.

Diese API erfordert **mindestens 100 Textdatensätze** gesendet werden, aber zur Erkennung von Themen Unterstützung für Tausende von Datensätzen ausgelegt ist. Alle nichtenglischen oder Datensätze mit weniger als 3 Wörter werden verworfen und können daher nicht zu Themen zugewiesen. Zum Thema Erkennung ein einzelnes Dokument, das gesendet werden kann die maximale Größe beträgt 30KB und die maximale Gesamtgröße der übermittelten Eingabe ist 30MB. Thema Erkennung ist Zins beschränkt auf 5 Übermittlungen 5 Minuten.

Es gibt zwei zusätzliche **optionale** Eingabeparameter, die zur Verbesserung der Qualität der Ergebnisse helfen können:

- **Beenden Sie Wörter ein.**  Diese Wörter und deren Formulare schließen (z. B. Pluralformen) werden die gesamte Thema Erkennung Verkaufspipeline ausgeschlossen werden müssen. Verwenden Sie diese für häufig verwendete Wörter, (z. B. "Problem", "zurück" und "Benutzer" geeignete Auswahl für Kundenbeschwerden zu Software sein können). Jede Zeichenfolge sollte ein einzelnes Wort sein.
- **Beenden Sie Ausdrücke** - dieser Ausdrücke aus der Liste der zurückgegebenen Themen ausgeschlossen. Verwenden Sie diese Option, um generische Themen auszuschließen, die nicht in den Ergebnissen angezeigt werden soll. Beispielsweise wäre "Microsoft" und "Azure" geeignete Auswahl nach Themen ausschließen. Zeichenfolgen können mehrere Wörter enthalten.

Folgen Sie diesen Schritten zum Ermitteln von Themen in Ihren Text ein.

1. Formatieren Sie die Eingabe in JSON. Diesmal, können Sie definieren Stoppwörter und Ausdrücke beenden.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Verwenden die gleichen Überschriften im Sinne Vorgang 2, stellen Sie einen **Beitrag** an den Endpunkt Themen anrufen:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Dadurch wird zurückgegeben, eine `operation-location` als Kopfzeile in der Antwort, wobei der Wert die URL für die resultierenden Themen Abfrage ist:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Abfrage zurückgegebenen `operation-location` regelmäßig mit einer **GET** -Anforderung. Einmal pro Minute wird empfohlen.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Der Endpunkt zurückgegeben werden kann eine Antwort einschließlich `{"status": "notstarted"}` vor der Verarbeitung, `{"status": "running"}` während der Verarbeitung und `{"status": "succeeded"}` mit der Ausgabe einmal durchgeführt. Sie können dann die Ausgabe nutzen, die in folgendem Format (Notiz Details wie in diesem Beispiel Fehlerformat und Datumsangaben ausgeschlossen wurden) werden:

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Beachten Sie, dass die erfolgreiche Antwort Themen aus der `operations` Endpunkt haben das folgende Schema:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Erläuterungen für jeden Teil des dieser Antwort werden wie folgt aus:

**Themen**

| Schlüssel | Beschreibung |
|:-----|:----|
| ID | Ein eindeutiger Bezeichner für jedes Thema. |
| Punktzahl | Anzahl der Dokumente, die Thema zugewiesen ist. |
| keyPhrase | Ein Zusammenfassen von Wort oder Satz nach dem Thema. |

**topicAssignments**

| Schlüssel | Beschreibung |
|:-----|:----|
| documentId | Bezeichner für das Dokument. Die ID, die sich in der Eingabe entspricht. |
| topicId | Die Thema-ID die das Dokument zugewiesen wurde. |
| Abstand | Dokument-Thema-Zugehörigkeit Punktzahl zwischen 0 und 1. Die untere einen Abstand Faktor, desto stärker wird die Zugehörigkeit Thema. |

**Fehler**

| Schlüssel | Beschreibung |
|:-----|:----|
| ID | Eindeutige von Dokument-ID des Fehlers bezieht sich auf. |
| Nachricht | Fehlermeldung. |

## <a name="next-steps"></a>Nächste Schritte ##

Herzlichen Glückwunsch! Sie haben nun abgeschlossen Text Analytics für Ihre Daten verwenden. Möglicherweise möchten Sie jetzt nähere darstellen Ihrer Daten mithilfe eines Tools, wie z. B. [Power BI](//powerbi.microsoft.com) sowie Automatisieren Ihrer Einsichten eine Ansicht der Textdaten in Echtzeit angezeigt.

Um anzuzeigen, wie Text Analytics-Funktionen, wie z. B. Grüße, als Teil eines Bot verwendet werden können, finden Sie unter [Emotionale Bot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) Beispiel auf der Website Bot Framework.
