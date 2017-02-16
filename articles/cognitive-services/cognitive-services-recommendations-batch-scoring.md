
<properties
    pageTitle="Erste Empfehlungen stapelweise: Computer learning Empfehlungen-API | Microsoft Azure"
    description="Learning Empfehlungen – Empfehlungen stapelweise erste Azure-Computern"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Lassen Sie sich in Stapeln

>[AZURE.NOTE] Erste Empfehlungen stapelweise ist komplizierter als erste Empfehlungen eine nacheinander. Überprüfen Sie die Informationen zum Abrufen von Empfehlungen für eine einzelne Anforderung-APIs:

> [Empfehlungen Elementen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Elements Benutzer Empfehlungen](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Stapel bewerten funktioniert nur für Builds, die nach 21 Juli 2016 erstellt wurden.


Es gibt Situationen, in denen müssen Sie Empfehlungen für mehrere Elemente gleichzeitig abzurufen. Beispielsweise interessiert Sie möglicherweise in einer Empfehlungen Cache erstellen oder sogar den vorstehend beschriebenen Vorschläge, die Sie abrufen zu analysieren.

Stapel Punktzahl Vorgänge, sind wir auch als, asynchrone. Sie müssen die Anforderung senden, warten, bis zum Abschluss des Vorgangs und Sammeln Sie die Ergebnisse.  

Um weitere werden die nachfolgend präzise, die folgenden Schritte:

1.  Wenn Sie eine bereits besitzen, erstellen Sie einen Container Azure-Speicher.
2.  Hochladen einer Eingabewerte Datei, die jeder der Ihre Empfehlungen Anfragen zu Azure Blob-Speicher beschreibt.
3.  Starten der Punktzahl Stapelverarbeitung.
4.  Warten Sie zum Abschluss des asynchronen Vorgangs aus.
5.  Wenn der Vorgang abgeschlossen ist, sammeln Sie die Ergebnisse in Blob-Speicher.

Werfen Sie einen durch jeden der folgenden Schritte aus.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Erstellen eines Containers Speicher aus, wenn Sie eine bereits besitzen

Wechseln Sie zum [Azure-Portal](https://portal.azure.com) an, und erstellen Sie ein neues Speicherkonto ein, wenn Sie eine bereits besitzen. Navigieren Sie zu diesem Zweck auf **neu** > **Daten** + **Speicher** > **Speicher-Konto**.

Nachdem Sie ein Speicherkonto haben, müssen Sie die Blob-Container erstellen, in dem Sie die Eingabe und die Ausgabe der Ausführung des Stapels gespeichert wird.

Hochladen einer einer Datei, die beschreibt jede empfehlen anfordert, um Blob-Speicher Wir rufen Sie die Datei input.json hier.
Nachdem Sie einen Container haben, müssen Sie zum Hochladen einer Datei, die jede Anforderung beschrieben, die vom Dienst Empfehlungen ausführen müssen.

Ein Stapel kann nur eine Art von Anforderung über einen bestimmten Build ausführen. Es wird erläutert, wie diese Informationen im nächsten Abschnitt definiert. Jetzt wird angenommen, dass wir Element Empfehlungen außerhalb eines bestimmten Build durchführen werden. Die Datei enthält die eingegebenen Informationen (in diesem Fall die Elemente Startwert) für jede Anforderung.

Dies ist ein Beispiel für die Datei input.json aussieht:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Wie Sie sehen können, ist die Datei eine JSON-Datei, in dem jede Anforderung die Informationen, die erforderlich sind enthält, zu einem Empfehlungen senden. Erstellen Sie eine ähnliche JSON-Datei für die Besprechungsanfragen, die Sie zum erfüllen müssen, und kopieren Sie ihn in den Container, den Sie gerade im BLOB-Speicher erstellt haben.

## <a name="kick-start-the-batch-job"></a>Starten der Stapelverarbeitung

Im nächsten Schritt wird eine neue Stapelverarbeitung übermitteln. Aktivieren Sie für Weitere Informationen [-API-Referenz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/)aus.

Hauptteil der Anforderung der API muss die Speicherorte definieren, in denen die Eingabe, Ausgabe und Fehlerdateien gespeichert werden müssen. Es muss auch die Anmeldeinformationen definieren, die zum Zugreifen auf diese Orte erforderlich sind. Darüber hinaus müssen Sie einige Parameter angeben, die gelten für die gesamte Stapelverarbeitung (den Typ von Empfehlungen anfordern, das Modell/Build zu verwenden, die Anzahl der Ergebnisse pro Anruf usw.).

Dies ist ein Beispiel für den Hauptteil der Anforderung sollte folgendermaßen aussehen:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Hier einige wichtige Hinweise:

-   Derzeit sollte **AuthenticationType** immer **PublicOrSas**festgelegt werden.

-   Sie müssen ein Token freigegeben Access Signatur (SAS) an, damit die Empfehlungen-API zum Lesen und Schreiben von/bei Ihrem Blob-Speicher-Konto zu erhalten. Weitere Informationen zum Generieren von SAS Token finden Sie auf [der Seite Empfehlungen-API](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   Derzeit unterstützt wird nur **ApiName** ist **ItemRecommend**, die für Empfehlungen Elementen verwendet wird. Batchverarbeitung unterstützt nicht Elements Benutzer Empfehlungen zurzeit.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Warten Sie zum Abschluss des asynchronen Vorgangs

Wenn Sie den Stapel Vorgang beginnen, gibt die Antwort den Vorgang-Location-Header mit den Informationen, die zum Nachverfolgen des Vorgangs erforderlich ist.
Sie nachverfolgen den Vorgang mithilfe der [Vorgang Status-API abrufen]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), wie Sie für den Vorgang, der einen Erstellungsvorgang nachverfolgen.

## <a name="get-the-results"></a>Abrufen der Ergebnisse

Nachdem der Vorgang, abgeschlossen wurde unter der Voraussetzung, dass keine Fehler aufgetreten, können Sie die Ergebnisse aus der Ausgabe sammeln BLOB-Speicher.

Im folgenden Beispiel wird angezeigt, wie die Ausgabe aussehen könnte. In diesem Beispiel anzeigen wir Ergebnisse für einen Stapel mit nur zwei Anfragen (für Platzgründen).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Erfahren Sie mehr über die Einschränkungen

-   Nur eine Stapelverarbeitung kann pro Abonnement nacheinander aufgerufen werden.
-   Eine Eingabe Auftrag Batchdatei darf nicht mehr als 2 MB sein.
