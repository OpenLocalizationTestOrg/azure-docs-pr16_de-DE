<properties
    pageTitle="Eine Liste der Azure-Speicherressourcen mit Microsoft Azure-Speicher Clientbibliothek für C++ | Microsoft Azure"
    description="Erfahren Sie, wie Sie mit den Eintrag APIs in Microsoft Azure-Speicher-Client-Bibliothek für C++ Container, Blobs, Queues, Tabellen und Personen aufgelistet."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Ressourcen in C++ Azure-Speicher

Auflistung Vorgänge sind Schlüssel zu viele Entwicklungsszenarien mit Azure-Speicher. Dieser Artikel beschreibt, wie Objekte in Azure-Speicher mit den Eintrag in der Bibliothek Microsoft Azure Speicher Client für C++ bereitgestellten APIs am effizientesten aufgelistet werden.

>[AZURE.NOTE] Dieses Handbuch richtet den Azure-Speicher Clientbibliothek C++-Version 2.x, die über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](https://github.com/Azure/azure-storage-cpp)zur Verfügung steht.

Die Speicher-Client-Bibliothek bietet verschiedene Methoden zur Liste oder Abfrage Objekte in Azure-Speicher. In diesem Artikel werden die folgenden Szenarien:

-   Listencontainer in einem Konto
-   Liste Blobs in einem Container oder virtuelle Blob-Verzeichnis
-   Liste Warteschlangen in einem Konto
-   Liste der Tabellen in einem Konto
-   Abfrage Elemente in einer Tabelle

Jede dieser Methoden werden mit anderen überladenen für unterschiedliche Szenarien.

## <a name="asynchronous-versus-synchronous"></a>Asynchrone im Vergleich zu synchroner

Da der Speicher-Client-Bibliothek für C++ auf die [restlichen C++ Bibliothek](https://github.com/Microsoft/cpprestsdk)erstellt wurde, wird grundsätzlich asynchrone Vorgänge mithilfe von [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html)unterstützt. Beispiel:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Synchroner Vorgänge umbrechen die entsprechenden asynchrone Vorgänge:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Wenn Sie mit mehreren Thread Applikationen oder Diensten arbeiten, empfehlen wir die Verwendung der asynchronen APIs direkt anstelle eines Threads synchronisieren-APIs aufzurufen, die Leistung Ihres erheblich wirkt sich auf.

## <a name="segmented-listing"></a>Segmentierter Auflistung

Die Skalierung der Cloud-Speicher erfordert segmentierter Auflistung. Beispielsweise können Sie über eine Millionen Blobs in einem Azure Blob-Container oder eine Milliarden Elemente in einer Tabelle Azure haben. Dies sind keine theoretische Zahlen, aber real Kunden Verwendungsfällen.

Daher ist es nicht sinnvoll, um alle Objekte in einer einzelnen Antwort aufzulisten. Stattdessen können Sie Objekte mit Seitennavigation auflisten. Jeder den Eintrag APIs verfügt über eine *unterteilt* überladen.

Die Antwort für einen Vorgang segmentierter Eintrag enthält:

-   <i>_segment</i>, die den Satz von Ergebnissen für einen einzelnen Anruf an die Auflistung API zurückgegeben enthält.
-   *Continuation_token*, die an den nächsten Anruf akzeptieren, um die nächste Seite der Ergebnisse zu erhalten übergeben wird. Wenn es keine weiteren Ergebnisse sind zurückgegeben, ist das Fortsetzungstoken null.

Eine typische eines Anrufs an die Liste alle Blobs in einem Container kann beispielsweise, wie im folgenden Codeausschnitt aussehen. Der Code ist in unseren [Beispiele](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp)zur Verfügung:

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Beachten Sie, dass die Anzahl der Ergebnisse zurückgegeben, die in einer Seite von der Parameter *Max_results* die Überlastung der einzelnen-API, beispielsweise gesteuert werden kann:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Wenn Sie den Parameter *Max_results* nicht angeben, wird der maximale Standardwert von bis zu 5000 Ergebnisse in einer einzelnen Seite zurückgegeben.

Beachten Sie auch, dass eine Abfrage für Azure Table Storage zurückgeben möglicherweise keine Datensätze oder weniger Datensätze als der Wert des Parameters *Max_results* , die Sie angegeben haben, auch wenn das Fortsetzungstoken nicht leer ist. Ein Grund möglicherweise, dass die Abfrage in fünf Sekunden nicht abgeschlossen werden konnte. Solange das Fortsetzungstoken nicht leer ist, sollte die Abfrage fortzusetzen, und Code sollte nicht davon ausgegangen werden die Größe des Segments Ergebnisse.

Das Schreiben von Code empfohlene Muster in den meisten Fällen ist die explizite Fortschritt der Auflistung oder Abfragen und der Dienst wie jede Anforderung beantwortet bietet aufgeteilt aufgelistet ist. Besonders für C++ Applications oder Dienstleistungen kann Low-Level-Kontrolle über den Fortschritt Auflistung Steuerelement Arbeitsspeicher und Leistung hilfreich sein.

## <a name="greedy-listing"></a>Gierige Auflistung

Früheren Versionen der Bibliothek Speicher Client für C++ (Versionen 0.5.0 Anzeigen einer Vorschau und frühere Versionen) enthalten nicht unterteilt Auflistung APIs für Tabellen und Warteschlangen, wie im folgenden Beispiel gezeigt:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Diese Methoden wurden als Wrapper segmentierter APIs implementiert. Für jede Antwort des segmentierter Eintrag der Code an einem Vektor angefügt, um die Ergebnisse und alle Ergebnisse zurückgegeben, nachdem die vollständige Container gescannt wurden.

Dieser Ansatz funktioniert möglicherweise, wenn die Speicher-Konto oder die Tabelle eine kleine Anzahl von Objekten enthält. Mit einer Erhöhung der Anzahl von Objekten, jedoch konnte der erforderlichen Speicher, da alle Ergebnisse im Speicher geblieben ohne Beschränkung, vergrößern. Eine Auflistung Vorgang dauert sehr viel Zeit, die keine Informationen über deren Status Anrufer hatte.

Diese gierige Auflistung APIs im SDK sind in c#, Java oder der Umgebung JavaScript Node.js nicht vorhanden. Um die Verwendung dieser gierige APIs potenzielle Probleme zu vermeiden, wir entfernt diese Version 0.6.0 Vorschau.

Wenn diese Code gierige APIs anruft:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Ändern Sie dann den Code, um den Eintrag segmentierter APIs verwenden:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Durch die Angabe des Parameters *Max_results* des Abschnitts, können Sie zwischen den Zahlen von Besprechungsanfragen und arbeitsspeicherauslastung die Leistungsaspekte für eine Anwendung stattfinden gegeneinander abwägen.

Darüber hinaus, wenn Sie segmentierte Auflistung APIs verwenden, aber die Daten in einer lokalen Sammlung in der Formatvorlage "gierige" speichern, wird auch dringend empfohlen, Sie den Code gestalten, um das Speichern von Daten in einer lokalen Auflistung sorgfältig bei behandeln.

## <a name="lazy-listing"></a>Verzögerte Auflistung

Zwar gierige Auflistung potenzieller Probleme ausgelöst, bietet es sich, wenn keine zu viele Objekte im Container enthalten sind.

Wenn Sie auch c# oder Oracle Java SDKs verwenden, sollten Sie das Modell aufzählbare Programmierung vertraut sein seiner aus--Formatvorlage auflisten, in dem die Daten an einem bestimmten Offset nur abgerufen wird, wenn dies erforderlich ist. In C++ bietet die Vorlage basierende Iterator auch einen ähnlichen Ansatz aus.

Eine typische aus-Auflistung API, z. B., **List_blobs** mit sieht wie folgt aus:

    list_blob_item_iterator list_blobs() const;

Ein typische Codeausschnitt, der das aus-Auflistung Muster verwendet sieht ungefähr so aus:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Beachten Sie, dass aus-Auflistung nur synchroner Modus zur Verfügung steht.

Im Vergleich mit gierige Auflistung abgerufen aus-Auflistung Daten nur bei Bedarf an. Im Hintergrund ruft sie Daten aus Azure-Speicher nur bei der nächste Iterator in das nächste Segment verschoben. Daher arbeitsspeicherauslastung wird gesteuert, mit dem eine begrenzte Größe, und der Vorgang ist schnell.

Verzögerte Auflistung APIs sind für C++ in der Speicher-Client-Bibliothek enthalten, in der Version 2.2.0.

## <a name="conclusion"></a>Abschluss

In diesem Artikel erläutert die verschiedene überladenen für die APIs für verschiedene Objekte in der Bibliothek der Speicher-Client für C++ auflisten. Zusammenfassung:

-   Asynchrone APIs sind unter Szenarien mit mehreren Thread dringend empfohlen.
-   Segmentierter Eintrag wird in den meisten Fällen empfohlen.
-   Verzögerte Angebot wird in der Bibliothek als geeignete Wrapper in synchroner Szenarien bereitgestellt.
-   Gierige Auflistung wird nicht empfohlen und wurde aus der Bibliothek entfernt.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure-Speicher und Client-Bibliothek für C++ finden Sie unter den folgenden Ressourcen.

-   [So verwenden Sie BLOB-Speicher von C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Zum Verwenden von C++ Tabellenspeicher](storage-c-plus-plus-how-to-use-tables.md)
-   [So Warteschlange-Speicher von C++ verwenden](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure-Speicher Client-Bibliothek für C++-API-Dokumentation.](http://azure.github.io/azure-storage-cpp/)
-   [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)
