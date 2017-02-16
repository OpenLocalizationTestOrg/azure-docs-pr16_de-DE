<properties
    pageTitle="Verwenden Sie Speicher Analytics Protokolle und Kennzahlen Datensammlung | Microsoft Azure"
    description="Speicher Analytics können Sie zum Nachverfolgen von Kennzahlen Daten für alle Speicherdienste und Protokolle für Blob, Warteschlange, und Tabellenspeicher sammeln."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Speicher Analytics

## <a name="overview"></a>(Übersicht)

Azure-Speicher Analytics führt Protokollierung sowie Kennzahlen Daten für ein Speicherkonto. Sie können diese Daten zum Verfolgen von Anfragen und Analysieren von Verwendungstrends diagnostizieren Probleme mit Ihrem Speicherkonto verwenden.

Um Speicher Analytics verwenden zu können, müssen Sie es einzeln für jeden Dienst aktivieren, die Sie überwachen möchten. Sie können ihn aus dem [Azure-Portal](https://portal.azure.com)aktivieren. Weitere Informationen finden Sie unter [Monitor ein Speicherkonto Azure-Portal](storage-monitor-storage-account.md). Sie können auch Speicher Analytics programmgesteuert über die REST-API oder der Clientbibliothek aktivieren. Verwenden Sie die [Erste BLOB-Diensteigenschaften](https://msdn.microsoft.com/library/hh452239.aspx), [Warteschlange Diensteigenschaften ermitteln](https://msdn.microsoft.com/library/hh452243.aspx), [Erhalten Tabelleneigenschaften Dienst](https://msdn.microsoft.com/library/hh452238.aspx)und [Erhalten Service Dateieigenschaften](https://msdn.microsoft.com/library/mt427369.aspx) -Operationen Speicher Analytics für jeden Dienst aktivieren.

Aggregierten Daten werden gespeichert, in einer bekannten Blob (für die Protokollierung) und bekannten Tabellen (für Kennzahlen), die mit den Blob-Dienst und Tabelle Service-APIs zugegriffen werden können.

Speicher Analytics sind maximal 20 TB auf den Umfang der gespeicherten Daten, die von den total Grenzwert für Ihr Speicherkonto unabhängig ist. Weitere Informationen zur Abrechnung und Aufbewahrungsrichtlinien Daten finden Sie unter [Speicher Analytics und Abrechnung](https://msdn.microsoft.com/library/hh360997.aspx). Weitere Informationen zu Konto Speichergrenzwerte finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md).

Ein detailliertes Handbuch mit Speicher Analytics und andere Tools zu identifizieren, diagnostizieren und Azure-Speicher-bezogene Probleme zu beheben finden Sie unter [überwachen, diagnostizieren und Behandeln von Problemen mit Microsoft Azure-Speicher](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Informationen zur Protokollierung Analytics-Speicher

Speicher Analytics meldet ausführliche Informationen zu dem erfolgreiche und fehlgeschlagene Anfragen einer Speicherdienst an. Diese Informationen kann einzelne Anfragen überwachen und diagnostizieren Probleme mit einer Speicherdienst verwendet werden. Anfragen werden auf Basis beste Leistung protokolliert.

Protokolleinträge werden nur erstellt, wenn Speicher Dienst Aktivität vorhanden ist. Wenn ein Speicherkonto Aktivität in seinem Blob-Dienst, jedoch nicht in die Tabelle oder Warteschlange-Dienste hat, werden beispielsweise nur in Verbindung mit dem Dienst Blob Gruppenrichtlinien Protokolle erstellt.

Speicher Analytics Protokollierung ist nicht verfügbar für den Dienst der Azure-Datei.

### <a name="logging-authenticated-requests"></a>Protokollierung authentifiziert Besprechungsanfragen

Die folgenden Arten von authentifizierte Anforderungen angemeldet sind:

- Erfolgreiche Anfragen.

- Fehler bei Besprechungsanfragen, einschließlich Timeout, begrenzungsebene, Netzwerk-, Autorisierung und andere Fehler

- Besprechungsanfragen, die mit einer freigegebenen Access Signatur (SAS), einschließlich fehlerhafte und erfolgreiche Serviceanfragen.

- Anfragen zu Analytics-Daten.

Besprechungsanfragen, die von Speicher Analytics selbst, wie z. B. Log erstellen oder löschen, sind nicht angemeldet. Eine vollständige Liste der protokollierten Daten wird im [Speicher Analytics angemeldet Vorgänge und Statusnachrichten](https://msdn.microsoft.com/library/hh343260.aspx) und [Log-Speicherformat Analytics](https://msdn.microsoft.com/library/hh343259.aspx) -Themen beschrieben.

### <a name="logging-anonymous-requests"></a>Anonyme Anfragen Protokollierung
Die folgenden Arten von anonyme Anfragen angemeldet sind:

- Erfolgreiche Anfragen.

- Serverfehler.

- Timeoutfehler für Client und Server.

- Fehler beim Abrufen Anfragen mit dem Fehlercode 304 (keine Änderung).

Alle anderen Fehler beim anonyme Anfragen sind nicht angemeldet. Eine vollständige Liste der protokollierten Daten wird im [Speicher Analytics angemeldet Vorgänge und Statusnachrichten](https://msdn.microsoft.com/library/hh343260.aspx) und [Log-Speicherformat Analytics](https://msdn.microsoft.com/library/hh343259.aspx) -Themen beschrieben.

### <a name="how-logs-are-stored"></a>Speichern von Protokollen
Alle Protokolle werden gespeichert in blockieren Blobs in einem Container mit dem Namen $logs, die automatisch erstellt wird, wenn Speicher Analytics für ein Speicherkonto aktiviert ist. Der Container $logs befindet sich im Namespace BLOB-Speicher-Kontos, beispielsweise: `http://<accountname>.blob.core.windows.net/$logs`. Dieser Container kann nicht gelöscht werden, sobald Speicher Analytics aktiviert wurde, und zwar deren Inhalt gelöscht werden können.

>[Azure.NOTE] Der Container $logs wird nicht angezeigt, wenn ein Containers auflisten Vorgang, z. B. die Methode [ListContainers ausgeführt wird](https://msdn.microsoft.com/library/azure/dd179352.aspx) . Es muss direkt zugegriffen werden. Sie können beispielsweise die [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) -Methode verwenden, die Blobs in Zugriff auf die `$logs` Container.
Anfragen angemeldet sind, wird Speicher Analytics Zwischenergebnisse als Blöcke hochladen. Speicher Analytics wird regelmäßig, diese Blöcke abzuschließen und als Blob verfügbar machen.

Für die Protokolle in der gleichen Stunde erstellt möglicherweise Duplikate vorhanden. Sie können feststellen, ob ein Datensatz ein Duplikat ist, indem er die Anzahl **RequestId** und **Vorgang** .

### <a name="log-naming-conventions"></a>Melden Sie sich Benennungskonventionen
Im folgenden Format wird jedes Protokoll geschrieben werden.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Die folgende Tabelle beschreibt die einzelnen Attribute in der Name der Protokolldatei.

| Attribut         | Beschreibung                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < Service-Name >    | Der Name des der Speicherdienst. Beispiel: Blob, Tabelle oder Warteschlange.                                                                                                                          |
| JJJJ              | Der vierstelligen Jahreszahl für das Protokoll. Beispiel: 2011.                                                                                                                                           |
| MM                | Der zweistelligen Monat für das Protokoll. Beispiel: 07.                                                                                                                                             |
| TT                | Der zweistelligen Monat für das Protokoll. Beispiel: 07.                                                                                                                                             |
| hh                | Die Stunde in zwei Ziffern gibt an, dass die erste Stunde für die Protokolle, im 24-Stunden-UTC-Format. Beispiel: 18.                                                                                     |
| mm                | Die zweistellige Zahl, die die Minute Start-, damit die Protokolle angibt. Dieser Wert wird in der aktuellen Version von Speicher Analytics nicht unterstützt, und der Wert wird immer 00 sein.     |
| <counter>         | Ein nullbasierte Zähler mit sechs Ziffern, der die Anzahl der Log Blobs für die Speicherdienst in eine Stunde Zeitraum generierten angibt. Der Zähler beginnt am 000000. Beispiel: 000001.     |

Im folgenden finden eine vollständige Log Beispielname, die in den vorherigen Beispielen kombiniert.

    blob/2011/07/31/1800/000001.log

So sieht ein Beispiel-URI, die Zugriff auf das vorherigen Protokoll verwendet werden kann.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Wenn eine Anforderung Speicher angemeldet ist, entsprechen der sich ergebende Log Name die Stunde, wenn der Vorgang abgeschlossen. Beispielsweise, wenn die Anforderung einer getBlob-6:30 Uhr abgeschlossen wurde Klicken Sie auf 7/31/2011 würde das Protokoll mit dem folgenden Präfix geschrieben werden:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Melden Sie sich Metadaten
Mit Metadaten, die verwendet werden können, welche Protokollierungsdaten identifizieren das Blob enthält, werden alle Log Blobs gespeichert. Die folgende Tabelle beschreibt jedes Metadatenattribut.

| Attribut     | Beschreibung                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Beschreibt, ob das Protokoll Informationen zum Lesen enthält, schreiben oder Löschvorgängen betreffen. Dieser Wert kann einen Typ oder eine Kombination aus allen drei, durch Kommas getrennt ein enthalten. Beispiel 1: Schreiben; Beispiel 2: Lesen, schreiben; Beispiel 3: Lesen, schreiben, löschen.    |
| Startzeit     | Das früheste eines Eintrags in das Protokoll, in Form von JJJJ-MM-: ssZ. Beispiel: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Endzeit       | Die neuesten Zeit für einen Eintrag in das Protokoll, in Form von JJJJ-MM-: ssZ. Beispiel: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | Die Version der Log-Format. Aktuell ist der einzige unterstützte Wert 1.0.                                                                                                                                                                                     |

Die folgende Liste zeigt die vollständige Beispiel Metadaten, die mit den vorherigen Beispielen.

- LogType = schreiben

- Startzeit = 2011-07-31T18:21:46Z

- Endzeit = 2011-07-31T18:22:09Z

- LogVersion = 1.0

### <a name="accessing-logging-data"></a>Zugreifen auf Protokollierungsdaten

Alle Daten in der `$logs` Container mithilfe der Blob-Service-APIs zugegriffen werden kann, einschließlich der von der Azure bereitgestellten .NET APIs verwaltete Bibliothek. Speicher Konto-Administrator kann lesen und Löschen von Protokollen, aber nicht erstellt oder aktualisiert diese. Namen und das Protokoll des Metadaten können verwendet werden, bei der Abfrage für ein Protokoll. Es ist möglich, dass einer bestimmten Uhrzeit Protokolle falschen Reihenfolge angezeigt werden, aber die Metadaten gibt immer die Zeitspanne der Protokolleinträge in ein Protokoll. Daher können Sie eine Kombination aus Log Namen und Metadaten bei der Suche nach einem bestimmten Protokoll.

## <a name="about-storage-analytics-metrics"></a>Informationen zum Speicher Analytics Kennzahlen

Speicher Analytics können Kennzahlen gespeichert werden, die aggregierte Transaktion Statistiken und Kapazität Daten zu Anfragen zu einer Speicherdienst enthalten. Auf der API Vorgang Ebene und auf Dienstebene Transaktionen gemeldet, und Kapazität, wird bei der Dienst Speichergrenze angegeben werden. Kennzahlen Daten können verwendet werden, zum Analysieren der Verwendung von Diensten diagnostizieren Probleme mit anhand der Speicherdienst Anforderungen, und um die Leistung der Anwendung zu verbessern, in denen einen Dienst verwendet.

Um Speicher Analytics verwenden zu können, müssen Sie es einzeln für jeden Dienst aktivieren, die Sie überwachen möchten. Sie können ihn aus dem [Azure-Portal](https://portal.azure.com)aktivieren. Weitere Informationen finden Sie unter [Monitor ein Speicherkonto Azure-Portal](storage-monitor-storage-account.md). Sie können auch Speicher Analytics programmgesteuert über die REST-API oder der Clientbibliothek aktivieren. Verwenden Sie die **Erste Diensteigenschaften** Vorgänge, um Speicher Analytics für jeden Dienst zu aktivieren.

### <a name="transaction-metrics"></a>Transaktion Kennzahlen

Eine umfangreiche Sammlung von Daten pro Stunde oder Minute Abständen für jede Speicherdienst aufgezeichnet wird und eingehende/Ausgang, Verfügbarkeit, Fehlern, einschließlich API Operation angefordert und Anforderung Prozentsätze eingestuft. Sie können eine vollständige Liste der die Details im [Speicher Analytics Kennzahlen Tabellenschema](https://msdn.microsoft.com/library/hh343264.aspx) Thema finden Sie unter.

Transaktionsdaten werden auf zwei Ebenen – Service und API-Vorgang Ebene aufgezeichnet. Ebene der Dienst angeforderte Statistik Zusammenfassen von alle API Vorgänge in einer Tabellenentität stündlich geschrieben werden, auch wenn keine Besprechungsanfragen mit dem Dienst vorgenommen wurden. Ebene der API Vorgang werden Statistiken nur in einer Entität geschrieben, des Vorgangs innerhalb dieser Stunde angefordert wurde.

Wenn Sie einen Vorgang **GetBlob** auf dem Blob-Dienst ausführen, wird Speicher Analytics Kennzahlen beispielsweise melden Sie sich die Anfrage und in der aggregierten Daten für sowohl den Blob-Dienst als auch der Vorgang **GetBlob** einzuschließen. Jedoch, wenn keine **GetBlob** Operation während der Stunde angefordert wird, eine Entität wird nicht in geschrieben werden die `$MetricsTransactionsBlob` für diesen Vorgang.

Für Benutzeranfragen und Besprechungsanfragen von Speicher Analytics selbst vorgenommen werden Transaktion Kennzahlen aufgezeichnet. Beispielsweise werden Anfragen von Speicher Analytics zum Schreiben von Protokollen und Personen Tabelle aufgezeichnet. Weitere Informationen darüber, wie diese Anfragen in Rechnung gestellt werden finden Sie unter [Speicher Analytics und Abrechnung](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Kapazität Kennzahlen

>[AZURE.NOTE] Kapazität Kennzahlen stehen derzeit nur für den Blob-Dienst zur Verfügung. Kapazität Kennzahlen für die Tabelle-Dienst und Warteschlange-Dienst in zukünftigen Versionen von Speicher Analytics zur Verfügung.

Kapazität Daten für ein Speicherkonto Blob-Dienst täglich aufgezeichnet werden, und zwei Tabelle Einheiten geschrieben werden. Eine Entität bietet Statistiken für Benutzerdaten und anderen stellt Statistiken über die `$logs` Blob Container von Speicher Analytics verwendet. Die `$MetricsCapacityBlob` Tabelle enthält die folgenden Statistiken:

- **Kapazität**: der Speichermenge, die vom Dienst BLOB-Speicher-Konto in Byte verwendet.

- **ContainerCount**: die Anzahl der Blob-Container in das Speicherkonto Blob-Dienst.

- **ObjectCount**: die Anzahl der zugesicherte und blockieren oder eine Seite Blobs in das Speicherkonto Blob-Dienst.

Weitere Informationen zu den Kennzahlen Kapazität finden Sie unter [Speicher Analytics Kennzahlen Tabellenschema](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Wie Kennzahlen gespeichert sind

Alle Kennzahlen Daten für jeden der Speicherdienste in drei Tabellen, die für diesen Dienst reserviert gespeichert: Transaktionsinformationen in einer Tabelle, einer Tabelle Minute Transaktion Informationen und einer anderen Tabelle Kapazität Informationen. Transaktion und Minute Transaktionsinformationen besteht aus Anfragen und Antworten und Kapazitätsinformationen Verwendungsdaten Speicher besteht aus. Kennzahlen Stunde, Minute Kennzahlen und Kapazität für Speicher-Konto Blob Service können in Tabellen, die in der folgenden Tabelle beschriebenen lauten zugegriffen werden.

| Kennzahlen Ebene                         | Tabellennamen                                                                                                                   | Unterstützte Versionen                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Stündlich Kennzahlen, primärer Speicherort      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Versionen vor der 2013-08-15 nur. Während diese Namen weiterhin unterstützt werden, empfiehlt es sich, dass Sie bei der Verwendung von der unten aufgeführten Tabellen wechseln.  |
| Stündlich Kennzahlen, primärer Speicherort      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Alle Versionen, einschließlich 2013-08-15.                                                                                                               |
| Minute Kennzahlen, primärer Speicherort      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Alle Versionen, einschließlich 2013-08-15.                                                                                                               |
| Stündlich Kennzahlen, sekundäre Speicherort    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Alle Versionen, einschließlich 2013-08-15. Geo redundante Replikation Lese-Zugriff muss aktiviert sein.                                                    |
| Minute Kennzahlen, sekundäre Speicherort    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Alle Versionen, einschließlich 2013-08-15. Geo redundante Replikation Lese-Zugriff muss aktiviert sein.                                                    |
| Kapazität (nur Blob-Dienst)          | $MetricsCapacityBlob                                                                                                          | Alle Versionen, einschließlich 2013-08-15.                                                                                                               |


In diesen Tabellen werden automatisch erstellt, wenn Speicher Analytics für ein Speicherkonto aktiviert ist. Sie können über den Namespace des Kontos Speicher, beispielsweise zugreifen:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Zugreifen auf Kennzahlen Daten

Alle Daten in den Kennzahlen Tabellen mithilfe der Tabelle Service-APIs zugegriffen werden kann, einschließlich der von der Azure bereitgestellten .NET APIs verwaltete Bibliothek. Speicher Konto-Administrator kann lesen und löschen die Elemente der Tabelle, aber nicht erstellt oder aktualisiert diese.

## <a name="billing-for-storage-analytics"></a>Rechnung für Speicher Analytics

Speicher Analytics ist in der Besitzer einer Speicher-Konto aktiviert. Es ist nicht standardmäßig aktiviert. Alle Kennzahlen Daten durch die Dienste eines Kontos Speicher geschrieben werden. Jeder Schreibvorgang ausgeführte Arbeit von Speicher Analytics daher wird hinaus. Darüber hinaus ist die Speichermenge, die verwendeten Kennzahlen Daten auch berechenbare.

Die folgenden von Speicher Analytics ausgeführten Aktionen werden berechenbarer:

- Anfragen Blobs für die Protokollierung zu erstellen. 

- Anfragen Tabelle Personen für Metrik zu erstellen.

Wenn Sie eine Datenaufbewahrungsrichtlinie konfiguriert haben, sind Sie nicht Löschtransaktionen berechnet Wenn Speicher Analytics alte Protokollierung und Kennzahlen Daten gelöscht werden. Es gibt jedoch Löschtransaktionen von einem Client berechenbarer. Weitere Informationen zu Aufbewahrungsrichtlinien finden Sie unter [Festlegen einer Aufbewahrungsrichtlinie für Speicher Analytics Daten](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Grundlegendes zu berechenbarer Besprechungsanfragen

Jeder Anforderung an des Kontos Speicherdienst ist berechenbaren oder nicht abzurechnende. Speicher Analytics protokolliert jede einzelne Anforderung versucht, einen Dienst, einschließlich der Statusmeldung, die angibt, wie die Anforderung verarbeitet wurde. Auf ähnliche Weise speichert Speicher Analytics Kennzahlen für einen Dienst und die Vorgänge API dieses Diensts, einschließlich Prozentsätzen und Anzahl von bestimmter Statusnachrichten. Zusammen, helfen diese Features Ihnen analysieren Ihre berechenbaren Anfragen, stellen Verbesserungen in Ihrer Anwendung und diagnostizieren Probleme mit Anfragen zu Ihrer Dienste. Weitere Informationen zur Abrechnung finden Sie unter [Grundlegendes zu Azure Speicher Abrechnung - Bandbreite, Transaktionen, und Kapazität](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Beim Speicher Analytics-Daten zu betrachten, können Sie die Tabellen im [Speicher Analytics angemeldet Vorgänge und Statusnachrichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) Thema bestimmen, welche Anfragen berechenbaren sind. Vergleichen Sie dann Ihre Protokolle und Kennzahlen Daten der Status-Nachrichten, um festzustellen, ob Sie eine bestimmte Anforderung in Rechnung gestellt wurden. Sie können auch die Tabellen in das vorherige Thema verwenden, Verfügbarkeit für eine Speicherdienst oder einzelne API Vorgang untersuchen.

## <a name="next-steps"></a>Nächste Schritte

### <a name="setting-up-storage-analytics"></a>Einrichten von Speicher Analytics
- [Überwachen von einem Speicherkonto Azure-Portal](storage-monitor-storage-account.md)
- [Aktivieren und Konfigurieren von Speicher Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Speicher Analytics Protokollierung  
- [Informationen zum Speicher Analytics Protokollierung](https://msdn.microsoft.com/library/hh343262.aspx)
- [Log-Speicherformat Analytics](https://msdn.microsoft.com/library/hh343259.aspx)
- [Speicher Analytics angemeldet, Vorgänge und Statusnachrichten](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Speicher Analytics Kennzahlen
- [Informationen zum Speicher Analytics Kennzahlen](https://msdn.microsoft.com/library/hh343258.aspx)
- [Speicher Analytics Kennzahlen Tabellenschema](https://msdn.microsoft.com/library/hh343264.aspx)
- [Speicher Analytics angemeldet, Vorgänge und Statusnachrichten](https://msdn.microsoft.com/library/hh343260.aspx)  
