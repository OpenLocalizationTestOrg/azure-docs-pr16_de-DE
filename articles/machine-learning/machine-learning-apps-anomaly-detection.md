<properties 
    pageTitle="Computer-Learning-app: Anomalie Erkennungsdienst | Microsoft Azure" 
    description="Anomalie Erkennung API ist ein Beispiel für mit Microsoft Azure maschinellen Learning, die Bildschirmdarstellung auftreten in Reihe Zeitdaten mit numerischen Werten erkennt, die einheitlich Zeitpunkt verteilt sind erstellt." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Maschinelle Learning Anomalie Erkennung Service#

##<a name="overview"></a>(Übersicht)

[Anomalie Erkennung API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) ist ein Beispiel für integriert Azure maschinellen Schulung, die Bildschirmdarstellung auftreten in Reihe Zeitdaten mit numerischen Werten erkennt, die Zeitpunkt einheitlich verteilt sind. 

Diese API kann die folgenden Arten von abweichenden Muster in Reihe Zeitdaten erkennen:

* **Positive und negative Trends**: beispielsweise beim arbeitsspeicherauslastung in einer Trends computing für die Überwachung von Interesse sein kann, wie er möglicherweise auf Speicher freigegeben, schließen

* **Änderungen in der dynamischen Wertebereich**: Z. B. beim Überwachen von einem Cloud-Dienst ausgelösten Ausnahmen Änderungen in der dynamischen Wertebereich könnte hinweisen Instabilität in die Integrität des Diensts, und

* **Spitzen und fällt**: beispielsweise, wenn die Anzahl der Anmeldefehler in einem Dienst oder die Anzahl der Auscheckens in einer e-Commerce-Website für die Überwachung, Spitzen oder Dips könnte hinweisen ungewöhnliches Verhalten.

Diese Computer Learning müssen Nachverfolgen von solchen Änderungen in Werte über die Zeit und Bericht laufenden Änderungen in deren Werte als Anomalie Bewertungen der Lesbarkeit. Sie benötigen keine Ad-hoc-Schwellenwert optimiert, und die einzelnen Studenten zum Steuern false-positive Rate verwendet werden können. Die API eignet sich mehrere Szenarien wie Dienst Überwachung, indem Sie nachverfolgen KPIs im Laufe der Zeit Normalbetriebswerte Verwendung Überwachung durch Kennzahlen wie die Anzahl der Suchbegriffe, Zahlen Klicks, Systemleistung durch Indikatoren wie Arbeitsspeicher, CPU Datei liest, usw. über einen Zeitraum.

Das Angebot Normalbetriebswerte im Lieferumfang von nützliche Tools, um Ihnen den Einstieg. 

* Die [Web-Anwendung](http://anomalydetection-aml.azurewebsites.net/) hilft Ihnen der ausgewertet werden soll, und die Ergebnisse der Normalbetriebswerte APIs für Ihre Daten darstellen. 

* Im [Beispiel-Code](http://adresultparser.codeplex.com/) veranschaulicht, wie programmgesteuert Zugriff auf die API und analysieren die ergibt.

>[AZURE.NOTE]
>Versuchen Sie es **IT Anomalie Einsichten Lösung** [dieser API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) betrieben
>
>Diese durchgehende Lösung für Ihr Azure-Abonnement <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank">bereitgestellt abzurufenden **beginnen Sie hier >**</a>


##<a name="api-definition"></a>API Definition

Der Dienst bietet eine REST-basierten API usw. über HTTPS, die auf verschiedene Arten ein Web oder mobilen Anwendung, R, Python, Excel, einschließlich genutzt werden können.  Sie senden Ihrer Datenreihe Zeit für diesen Dienst über einen Anruf REST-API, und es wird eine Kombination der oben geschilderten Typen von drei Anomalie ausgeführt. Der Dienst ausgeführt wird, auf der Azure maschinellen Learning-Plattform skaliert an Ihre Bedürfnisse Business nahtlos und SLAs enthält.

###<a name="headers"></a>Kopfzeilen
Stellen Sie sicher, dass Sie die richtigen Überschriften in Ihrer Anforderung enthält, die wie folgt werden sollen:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Sie können Ihren kontoschlüssel aus Ihrem Konto auf dem [Azure Data Market](https://datamarket.azure.com/account/keys)suchen. 

###<a name="score-api"></a>Punktzahl-API

Der Punktzahl-API wird für die Ausführung von Normalbetriebswerte auf nicht saisonale Reihe Zeitdaten verwendet. Die API führt eine Reihe von Anomalie müssen auf der Registerkarte Daten, und gibt die einzelnen Anomalie Studenten. Die folgende Abbildung zeigt ein Beispiel für die Bildschirmdarstellung auftreten, die die Punktzahl-API erkennen können. Dieser Zeitreihe hat 2 distinct Änderungen auf Websiteebene vorzunehmen und 3 Spitzen an. Die rote Punkte zeigen die Zeit an, an der die Änderung die erkannt wird, während schwarze Punkte die erkannten Spitzen anzeigen.

![Punktzahl-API][1]
    
**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Beispiel für Anforderungstexts**

In der Anforderung senden wir eine Zeitreihe (Siehe abgeschnittene) mit Datenpunkten von 3/1/2016 bis 3/10/2016 und Parameter Sammlung müssen an die API für das erkennen, wenn eine der folgenden Datenpunkte abweichenden sind. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Die Antwort auf diese werden: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

Die ScoreWithSeasonality-API wird verwendet, für die Ausführung von Normalbetriebswerte auf Zeitreihe, die saisonale Mustern enthalten. Diese API ist sinnvoll, abweichungen in saisonale Muster zu erkennen.  

Die folgende Abbildung zeigt ein Beispiel für die Bildschirmdarstellung auftreten einer Reihe saisonale Zeit erkannt. Die Uhrzeit Reihe verfügt über eine Sammlung (den 1st schwarzen Punkt), zwei Dips (den 2. schwarzen Punkt und eine am Ende) und eine Ebene ändern (roter Punkt). Beachten Sie, dass Dip in der Mitte der Serie Zeit und die Änderung der sind nur erkennbar, nachdem die Reihe saisonale Komponenten entfernt wurden.

![Saisonbedingten API][2]

**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Beispiel für Anforderungstexts**

In der Anforderung senden wir eine Zeitreihe (Siehe abgeschnittene) mit Datenpunkten von 3/1/2016 bis 3/10/2016 und Parameter an die API für das erkennen, wenn eine der folgenden Datenpunkte abweichenden sind. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Die Antwort auf diese werden: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Müssen

Der Normalbetriebswerte API unterstützt müssen in 3 weitgefasste Kategorien. Details für bestimmte Eingabeparameter und Ausgaben für jede Erkennung finden Sie in der folgenden Tabelle.

|Erkennung Kategorie|Erkennung|Beschreibung|Eingabeparameter|Ausgaben
|---|---|---|---|---|
|Sammlung müssen|TSpike Erkennung|Erkennen von Spitzen und Dips basierend auf weit die Werte aus der ersten und dritten Quartiles sind|*tspikedetector.sensitivity:* akzeptiert ganze Zahl im Bereich 1 bis 10, Standard: 3; Höhere Werte werden weitere extrem Werte, wodurch es weniger vertrauliche abzufangen.|TSpike: binäre Werte – '1', wenn eine Sammlung/Dip erkannt wird, "0" andernfalls|
||ZSpike Erkennung|Erkennen von Spitzen und Dips basierend auf wie weit die Datapoints von ihrem Mittelwert zurück sind|*zspikedetector.sensitivity:* optimieren ganze Zahl im Bereich 1 bis 10, Standard: 3; Höhere Werte werden weitere extrem Werte, wodurch es weniger vertrauliche abzufangen.|ZSpike: binäre Werte – '1', wenn eine Sammlung/Dip erkannt wird, "0" andernfalls|
|Langsam Trend Erkennung|Langsam Trend Erkennung|Erkennen von langsam positive Trend gemäß der Vertraulichkeit festlegen|*trenddetector.sensitivity:* Schwellenwert auf Erkennung Punktzahl (Standard: 3,25, 3,25 – 5 ist angemessenen Bereich und wählen Sie diese aus; Je höher der weniger/Kleinschreibung)|TScore: beweglich Zahl darstellt Anomalie Punktzahl auf Trend|
|Ebene ändern müssen|Unidirektionale Ebene ändern Erkennung|Layoutbild Ebene erkennen gemäß der Vertraulichkeit festlegen ändern|*upleveldetector.sensitivity:* Schwellenwert auf Erkennung Punktzahl (Standard: 3,25, 3,25 – 5 ist angemessenen Bereich und wählen Sie diese aus; Je höher der weniger/Kleinschreibung)|PScore: beweglich Zahl nach oben, darstellt Anomalie Punktzahl auf Ebene ändern|
||Bidirektionaler Ebene ändern Erkennung|Erkennen von nach oben und nach unten weisenden Ebene ändern, die gemäß der Vertraulichkeit festlegen|*bileveldetector.sensitivity:* Schwellenwert auf Erkennung Punktzahl (Standard: 3,25, 3,25 – 5 ist angemessenen Bereich und wählen Sie diese aus; Je höher der weniger/Kleinschreibung)|RPScore: beweglich Zahl darstellt Anomalie, die Punktzahl auf nach oben und nach unten Etage ändern

###<a name="parameters"></a>Parameter

Weitere ausführliche Informationen zu diesen Eingabeparameter wird in der folgenden Tabelle aufgeführt:

|Eingabeparameter|Beschreibung|Standardeinstellung|Typ|Gültige Bereich|Vorgeschlagene Bereich|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Aggregation Intervall in Sekunden zum Aggregieren von Eingabemethoden Zeitreihe|0 (keine Aggregation ausgeführt wird)|ganze Zahl|0: überspringen Sie andernfalls Aggregation, > 0|1 Tag, Zeit-Serie abhängige 5 Minuten
|preprocess.aggregationFunc|Zum Aggregieren von Daten in der angegebenen AggregationInterval (Funktion)|Mittel|Aufzählung|Mittelwert, Summe, Länge|N/V|
|preprocess.replaceMissing|Werte verwendet, um Daten fehlen impute|Lkv (letzten Wert bezeichnet)|Aufzählung|0 (null), Lkv, Mittel|N/V|
|detectors.historyWindow|Verlauf (in Anzahl von Datenpunkten) für Anomalie Punktzahl Berechnung verwendet|500|ganze Zahl|10-2000|Time-Serie abhängige|
|upleveldetector.Sensitivity|Vertraulichkeit für Ebene nach oben Erkennung zu ändern. |3,25|Doppelte|Keine|3,25-5(Lesser values mean more sensitive)|
|bileveldetector.Sensitivity|Vertraulichkeit für bidirektionale Ebene Erkennung zu ändern. |3,25|Doppelte|Keine|3,25-5(Lesser values mean more sensitive)|
|trenddetector.Sensitivity|Empfindlichkeit für positive Trend Erkennung. |3,25|Doppelte|Keine|3,25-5(Lesser values mean more sensitive)|
|tspikedetector.Sensitivity |Empfindlichkeit für TSpike Erkennung|3|ganze Zahl|1-10|3-5(Lesser values mean more sensitive)|
|zspikedetector.Sensitivity |Empfindlichkeit für ZSpike Erkennung|3|ganze Zahl|1-10 |3-5(Lesser values mean more sensitive)|
|seasonality.Enable|Gibt an, ob saisonbedingten Analyse ausgeführt wird, die|WAHR|Boolesch|WAHR, falsch|Time-Serie abhängige|
|seasonality.numSeasonality |Maximale Anzahl von periodischen Zyklen erkannt werden|1|ganze Zahl|1, 2|1 und 2|
|seasonality.Transform |Ob saisonale (und) Trend Komponenten entfernenden vor dem Anwenden von Normalbetriebswerte|deseason|Aufzählung|keine, deseason, deseasontrend|N/V|
|postprocess.tailRows |Anzahl der neuesten Datenpunkte in den Ausgabeergebnissen beibehalten werden|0|ganze Zahl|0 (bleiben alle Datenpunkte), oder geben Sie die Anzahl von Punkten in Ergebnissen beibehalten|N/V|

###<a name="output"></a>Die Ausgabe
Die API alle müssen auf Ihrer Datenreihe Zeit ausgeführt wird und gibt Anomalie Bewertungen der Lesbarkeit und binäre Sammlung Indikatoren für jeden Punkt Zeitangabe zurück. Die folgende Tabelle enthält die Ausgaben aus der-API. 

|Ausgaben|Beschreibung|
|---|---|
|Zeit|Werden von unformatierten Daten und zusammengefasster (und/oder) Zeichen kalkulatorischen Daten Wenn Aggregation (und/oder) fehlen Daten Herausgabepflicht wird angewendet.|
|OriginalData|Werte von unformatierten Daten und zusammengefasster (und/oder) Zeichen kalkulatorischen Daten, wenn Aggregation (und/oder) fehlen Daten Herausgabepflicht wird angewendet.|
|ProcessedData|Eine der folgenden Aktionen aus: <ul><li>Wenn signifikante saisonbedingten wurde erkannt und ausgewählter Option deseason saisonbereinigt Zeitreihe;</li><li>saisonbedingt angepasst und trendbereinigten Zeitreihe signifikante saisonbedingten festgestellt wurde und aktivierter Option ' Deseasontrend '</li><li>Dies andernfalls ist OriginalData identisch</li>|
|TSpike|Binäre Indikator, um anzugeben, ob eine Sammlung von TSpike Erkennung erkannt wird|
|ZSpike|Binäre Indikator, um anzugeben, ob eine Sammlung von ZSpike Erkennung erkannt wird|
|Pscore|Eine unverankerte Zahl im Anomalie Punktzahl auf Ebene nach oben ändern|
|Palert|1/0-Wert, der angibt, dass es ist eine Ebene nach oben ändern Anomalie basierend auf der Eingabe Vertraulichkeit|
|RPScore|Eine unverankerte Zahl darstellt Anomalie Punktzahl auf bidirektionaler Ebene ändern|
|RPAlert|1/0-Wert, der angibt, dass es ist eine bidirektionale Ebene ändern, basierend auf der Eingabe Vertraulichkeit Anomalie|
|TScore|Eine unverankerte Zahl darstellt Anomalie Punktzahl auf positive trend|
|TAlert|1/0-Wert, der angibt, es ist eine positive Trend Anomalie basierend auf der Eingabe Vertraulichkeit|


Diese Ausgabe kann mit einer [einfachen Parser](https://adresultparser.codeplex.com/) analysiert werden – sie Beispielcode, der zeigt, wie eine Verbindung mit der-API und die Ausgabe analysiert hat. Die Bildschirmdarstellung erkannt auftreten können auf ein Dashboard visualisiert und/oder übergeben auf human Experten für Maßnahmen oder ticketingsystem Systeme integriert.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
