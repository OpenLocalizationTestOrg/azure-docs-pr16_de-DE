<properties
    pageTitle="Ihre Arbeit Stream Analytics mit Azure maschinellen Learning Funktionen skalieren | Microsoft Azure"
    description="Erfahren Sie, wie Stream Analytics Aufträge (Partitionierung SU Menge und mehr) ordnungsgemäß skalieren Wenn Azure maschinellen Learning-Funktionen verwenden."
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>Skalieren Sie Ihrer Position Stream Analytics mit Azure maschinellen Learning-Funktionen

Es ist oft recht einfach zu einer Position Stream Analytics einrichten und einige Beispieldaten durchlaufen. Was tun wir, wenn wir dasselbe Projekt mit höherer Datenmenge ausführen müssen? Dies erforderlich macht uns zu verstehen, wie Sie den Auftrag Stream Analytics so konfigurieren, dass es skaliert werden. In diesem Dokument liegt der Schwerpunkt auf den Besonderheiten von der Skalierung Stream Analytics Aufträge mit maschinellen Learning-Funktionen. Informationen zum Stream Analytics Aufträge im Allgemeinen Skalieren finden Sie im Artikel [Skalierung Aufträge](stream-analytics-scale-jobs.md)aus.

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Was ist eine Azure maschinellen Learning-Funktion in Stream Analytics?

Eine Funktion maschinellen Learning in Stream Analytics kann wie eine normale Funktionsaufrufs in der Stream Analytics-Abfragesprache verwendet werden. Hinter der Szene sind Funktion Anrufe jedoch tatsächlich Azure maschinellen Learning Web Serviceanfragen. Maschinelle Learning-Webdiensten unterstützen "Batchverarbeitung" mehrer Zeilen, die in der gleichen Web Service-API Anruf zur Verbesserung der Gesamtdurchsatz Mini Stapel, bezeichnet wird. Bitte finden Sie unter den folgenden Artikeln Weitere Details; [Azure maschinellen Learning-Funktionen in Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) und [Azure maschinellen Learning-Webdiensten](machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Konfigurieren eines Auftrags Stream Analytics mit maschinellen Learning-Funktionen

Beim Konfigurieren einer maschinellen Learning-Funktion für Stream Analytics Auftrag, gibt es zwei Parametern zu berücksichtigen ist, wird die Größe Stapel Anrufe maschinellen Learning (Funktion) und Streaming Einheiten (SUs) für den Auftrag Stream Analytics bereitgestellt. Wenn Sie die entsprechenden Werte für diese ermitteln möchten, muss zuerst eine Entscheidung zwischen Wartezeit und Durchsatz, d. h., Wartezeit des Streams Analytics Position und Durchsatz der einzelnen SU vorgenommen werden. SUs möglicherweise immer mit einem Projekt Datendurchsatz, einer gut partitionierten Stream Analytics Abfrage hinzugefügt werden zwar zusätzliche SUs erhöht die Kosten der Durchführung der Aufgabe.

Daher ist es wichtig, um die *Fehlertoleranz* der Wartezeit Ausführen eines Auftrags Stream Analytics zu bestimmen. Zusätzliche Wartezeit Ausführung Azure maschinellen Learning Serviceanfragen erhöht die mit Stapelgröße, die die Wartezeit des Streams Analytics Auftrags zusammengesetzter wird. Andererseits, Stapel vergrößern den Auftrag Stream Analytics Verarbeitungszeit ermöglicht *mehr Ereignisse mit den *dieselbe Anzahl * maschinellen Learning Web Serviceanfragen. Häufig ist die Erhöhung der Computer Learning Web Service Wartezeit Sub linearen der Erhöhung Stapelgröße, damit es die Kosten am effizientesten Stapelgröße für einen Computer Learning-Webdienst in der jeweiligen Situation zu berücksichtigen ist. Stapel Standardgröße für das Web Serviceanfragen ist 1000 und möglicherweise entweder mit der [Stream Analytics REST-API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST-API") oder der [PowerShell-Client für Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "PowerShell-Client für Stream Analytics")geändert werden.

Nachdem eine Stapelgröße ermittelt wurde, der Betrag Streaming Einheiten (SUs) kann bestimmt, basierend auf der Anzahl von Ereignissen, die in der Funktion muss pro Sekunde Prozess. Weitere Informationen zu Streaming Einheiten finden Sie im Artikel [Stream Analytics Maßstab Aufträge](stream-analytics-scale-jobs.md#configuring-streaming-units).

Es gibt im Allgemeinen 20 aktiven Verbindungen an den Computer Learning Webdienst für jede 6 SUs, außer dass 1 SU und 3 SU Einzelvorgänge werden auch 20 aktiven Verbindungen abrufen.  Wenn die Eingabedaten Rate 200.000 Ereignisse pro Sekunde und wird die Stapelgröße auf den Standardwert von 1000 wird die resultierende Web Service Wartezeit mit 1000 Ereignisse Mini Blattnamen 200 ms. Dies bedeutet, dass jeder Verbindung 5 Anfragen an den Computer Learning Webdienst in eine Sekunde vorgenommen werden kann. Mit 20 Verbindungen kann der Auftrag Stream Analytics 20.000 Ereignisse in 200 ms und daher 100.000 Ereignisse in eine Sekunde verarbeiten. Daher benötigt zum Verarbeiten von 200.000 Ereignisse pro Sekunde der Auftrag Stream Analytics 40 gleichzeitige Verbindungen, der zu 12 SUs-Zeile ab. Die folgende Abbildung zeigt die Anfragen von den Auftrag Stream Analytics an den Computer Learning Webdienst-Endpunkt – alle 6 SUs umfasst 20 gleichzeitige Verbindungen mit maschinellen Learning Webdienst auf Max.

![Maßstab Stream Analytics mit maschinellen Learning Funktionen 2 Auftrag Beispiel] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Maßstab Stream Analytics mit maschinellen Learning Funktionen 2 Auftrag Beispiel")

Im Allgemeinen wird ***B*** für Stapelgröße, ***L*** für das Web Service Wartezeit in Stapelgröße B in Millisekunden, der Durchsatz eines Auftrags Stream Analytics mit ***N*** SUs:

![Stream Analytics mit maschinellen Erlernen von Funktionen Formel skalieren] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Stream Analytics mit maschinellen Erlernen von Funktionen Formel skalieren")

Eine weitere Aspekte möglicherweise 'max gleichzeitige Anrufe' Web-Dienst auf Computer Schulung, es wird empfohlen, diese an den größten Wert (aktuell 200) fest.

Weitere Informationen zu dieser Einstellung Bitte überprüfen [Skalierung Artikel für maschinelle Learning-Webdiensten](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Beispiel – Grüße Analyse

Im folgende Beispiel enthält einen Stream Analytics Auftrag mit der Analyse Grüße maschinellen Learning-Funktion, wie im [Stream Analytics maschinellen Learning Integration Lernprogramm](stream-analytics-machine-learning-integration-tutorial.md)beschrieben.

Die Abfrage ist eine einfache, gefolgt von der Funktion **Grüße** vollständig partitionierten Abfrage aus, wie unten dargestellt:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Erwägen Sie das folgende Szenario aus. mit einem Durchsatz von 10.000 Tweets pro Sekunde muss ein Stream Analytics Auftrag um Grüße der Tweets (Ereignisse) kompliziertere erstellt werden. Mithilfe von 1 SU, diesen Auftrag Stream Analytics können, um den Datenverkehr verarbeiten könnte? Mit der Stapelverarbeitung Standardgröße von 1000 sollte der Auftrag mit der Eingabe aufrechterhalten können. Weitere sollte die hinzugefügte Computer Learning-Funktion generieren nicht mehr als eine Sekunde der Wartezeit, also den allgemeinen Standard Wartezeit der Grüße Analyse maschinellen Learning Webdienst (mit Stapel Standardgröße 1000). Der Stream Analytics-Auftrag Wartezeit für **insgesamt** oder End-to-End-wäre in der Regel ein paar Sekunden. Sehen Sie ausführlichere in diesen Stream Analytics Auftrag *besonders* des Computers Learning Anrufe (Funktion). Haben Sie die Stapelgröße als 1000, dauert ein Durchsatz von 10.000 Ereignisse zu 10 Anfragen an Webdienst. Selbst bei 1 SU gibt es genügend gleichzeitigen Verbindungen mit diesem eingehender Verkehr aufnehmen zu können.

Erhöht aber was passiert, wenn die Eingabewerte Ereignis Rate um X 100 und jetzt muss der Auftrag Stream Analytics 1.000.000 Tweets pro Sekunde verarbeiten? Es gibt zwei Optionen:

1.  Erhöhen der Stapelgröße oder
2.  Partition Eingabewerte Streams zum Verarbeiten der Ereignisse parallel

Mit der ersten Option wird die Position **Wartezeit** vergrößern.

Mit der zweiten Option muss Weitere SUs bereitgestellt werden, und daher mehr gleichzeitige maschinellen Learning Web Serviceanfragen generieren. Dies bedeutet, dass die Position **Kosten** vergrößert wird.


Angenommen Sie, die Wartezeit der Grüße Analyse maschinellen Learning Webdienst 200 ms für Stapel 1000-Ereignis oder nach unten 250ms für 5.000-Ereignis Blattnamen, 300ms für 10.000-Ereignis Stapel oder 500 ms für Stapel 25.000-Ereignis.

1. Verwenden die erste Option (**keine** Weitere SUs bereitgestellt), könnten Stapel zu **25.000**vergrößert werden. Dies würde wiederum den Auftrag zum Verarbeiten von 1.000.000 Ereignisse mit 20 aktiven Verbindungen an den Computer Learning Webdienst (mit einer Wartezeit von 500 ms pro Anruf) ermöglichen. Daher würde die zusätzliche Wartezeit des Streams Analytics Auftrags aufgrund der Grüße Funktion Anfragen gegen den Computer Learning Web Serviceanfragen von **200 ms** auf **500 ms**erhöht werden. Notiz, die Größe **kann nicht** Stapel jedoch sein, höhere endlos wie die Computer Learning-Webdiensten erfordert die Nutzlastgröße einer Anforderung werden 4 MB oder kleiner Webdienst anfordert Timeout nach 100 Sekunden des Vorgangs.
2. Verwenden die zweite Option, mit 200 ms Web Service Wartezeit 1000, die Stapelgröße belassen, jeder 20 aktiven Verbindungen mit dem Webdienst wäre 1000 *20* 5 Ereignisse verarbeiten = 100,000 pro Sekunde. Daher müssen zum Verarbeiten von 1.000.000 Ereignisse pro Sekunde müssten der Auftrag 60 SUs. Im Vergleich zu die erste Option, würde Stream Analytics Weitere Web Stapel Serviceanfragen, wiederum generieren eine höhere Kosten zu gestalten.

Es folgt eine Tabelle mit den Durchsatz des Streams Analytics Auftrags für verschiedene SUs und Stapelgrößen (in Anzahl von Ereignissen pro Sekunde).

| Stapelgröße (Wartezeit ML)  | 500 (200 ms) | 1.000 (200 ms) | 5.000 (250ms) | 10.000 (300ms) | 25.000 (500 ms) |
|--------|-------------------------|---------------|---------------|----------------|----------------|
| **1 SU** | 2.500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **3 SUs** | 2.500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **6 SUs** | 2.500 | 5.000 | 20.000 | 30.000 | 50.000 |
| **12 SUs** | 5.000 | 10.000 | 40.000 | 60.000 | 100,000 |
| **18 SUs** | 7.500 | 15.000 | 60.000 | 90.000 | 150.000 |
| **24 SUs** | 10.000 | 20.000 | 80.000 | 120.000 | 200.000 |
| **…** | … | … | … | … | … |
| **60 SUs** | 25.000 | 50.000 | 200.000 | 300.000 | 500.000 |

Nun sollten Sie bereits eine gute Kenntnisse über die Funktionsweise der Computer Learning-Funktionen in Stream Analytics verfügen. Sie wissen wahrscheinlich auch, dass Stream Analytics Aufträge "Abruf" Daten aus Datenquellen "und" Jeder "abrufen" gibt eine Reihe von Ereignissen für das Projekt Stream Analytics Verarbeitungszeit. Wie web diese Abruf Modell Auswirkung der Computer Learning Serviceanfragen?

Normalerweise kann die Stapelgröße für maschinelle Learning Funktionen festgelegten genau durch die Anzahl der zurückgegebene jede Stelle Stream Analytics "Abruf" Ereignisse dividiert werden nicht. In diesem Fall der Computer Learning Webdienst mit "teilweise" Blattnamen aufgerufen wird. Dadurch wird nicht erweiterten Aufgaben Wartezeit Verwaltungsaufwand in zusammenfügen Ereignisse Abruf zum Abruf anfallen.

## <a name="new-function-related-monitoring-metrics"></a>Neue Funktion Zusammenhang Überwachung Kennzahlen

Im Bereich Monitor eines Auftrags Stream Analytics wurden drei zusätzliche Funktion-bezogene Statistiken hinzugefügt. Sie sind Funktion Anfragen, Ereignisse (Funktion) und fehlerhaften ANFORDERUNGEN für die Funktion, wie in der nachfolgenden Grafik dargestellt.

![Stream Analytics mit maschinellen Erlernen von Funktionen Kennzahlen skalieren] (./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Stream Analytics mit maschinellen Erlernen von Funktionen Kennzahlen skalieren")

Sind wie folgt definiert:

**Funktion Anfragen**: die Anzahl der Anfragen (Funktion).

**Funktion Ereignisse**: die Zahl Ereignisse in der Funktion Serviceanfragen.

**Fehlerhaften Funktion ANFORDERUNGEN**: die Anzahl der Anfragen fehlgeschlagene (Funktion).

## <a name="key-takeaways"></a>Hauptargumente  

Um die wichtigsten Punkte, um eine Position Stream Analytics mit maschinellen Learning Funktionen skalieren zu summieren, müssen die folgenden Elemente berücksichtigt werden:

1.  Die Eingabewerte Ereignis Zins
2.  Der zulässigen Wartezeit für den laufenden Stream Analytics Auftrag (und somit der Stapel Größe der Computer Learning Web Serviceanfragen)
3.  Der bereitgestellte Stream Analytics SUs und die Anzahl der Computer Learning Web Serviceanfragen (Funktion Zusammenhang Mehrkosten)

Eine vollständig partitionierte Stream Analytics Abfrage wurde als Beispiel verwendet. Wenn eine komplexe Abfrage benötigt wird das [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) ist eine großartige Ressource für weitere Hilfe aus dem Stream Analytics-Team.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Stream Analytics finden Sie unter:

- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
