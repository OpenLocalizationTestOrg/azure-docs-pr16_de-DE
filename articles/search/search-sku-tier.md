<properties
    pageTitle="Wählen Sie eine SKU oder Preise Ebene für die Suche Azure | Microsoft Azure"
    description="Azure Suche bei dieser SKUs bereitgestellt werden kann: kostenlose, einfache und Standard, Standard darin in verschiedenen Ressourcenkonfigurationen und Kapazität Ebenen verfügbar."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Wählen Sie eine SKU oder Ebene für die Suche Azure Preise

In einem [Dienst wird nach der Bereitstellung](search-create-service-portal.md) bei einer bestimmten Preisgestaltung Ebene oder SKU Suche Azure. Optionen gehören **frei**, **grundlegende**oder **Standard**, **Standard** in mehreren Konfigurationen und Kapazität Einsatzbereich. 

Der Zweck dieses Artikels ist Ihnen helfen eine Stufe auszuwählen. Wenn eine Ebene Kapazität herausstellt zu niedrig sein, müssen Sie eine neue Dienstleistung auf der höheren Ebene bereitstellen, und Laden Sie Ihre Indizes erneut. Es gibt keine in-situ-Aktualisierung von denselben Dienst aus eine einzelne SKU zu einem anderen aus. 

> [AZURE.NOTE] Nachdem Sie eine Ebene und [Bereitstellen eines Suchdiensts](search-create-service-portal.md)ausgewählt haben, können Sie Replikat erhöhen und Partition innerhalb des Diensts ermittelt. Leitfaden für die finden Sie unter [Skalieren Ressourcenebenen für die Abfrage und Indizierung Auslastung](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>So Ansatz eine Preisgestaltung Ebene Entscheidung

In Azure suchen bestimmt die Ebene Kapazität, nicht verfügbare Funktionen. Im Allgemeinen sind die Features auf jeder Stufe, einschließlich der Preview-Features verfügbar. Eine Ausnahme wird nicht unterstützt Indexer in S3 HD.

> [AZURE.TIP] Es empfiehlt sich, dass Sie immer einen **Free** -Dienst (eine pro Abonnement permanenten) bereitstellen, damit deren sofort für Projekte, die leicht verfügbar. Verwenden Sie den **Free** Dienst zum Testen und bewerten; Erstellen einer zweiten fakturierbaren Service auf der ** **grundlegenden** oder** Stufe für die Herstellung oder größere Test Auslastung.

Wechseln Sie Kapazität und Kosten der mit dem Dienst Hand in Hand. Informationen in diesem Artikel können Sie besser entscheiden, die SKU das richtige Maß übermittelt, aber für diejenigen der es hilfreich sein, benötigen Sie mindestens groben Schätzung auf folgenden:

- Anzahl und Größe der Indizes, den, die Sie erstellen möchten.
- Anzahl und Größe der Dokumente hochladen
- Eine Vorstellung davon Abfrage Volumen, im Hinblick auf Abfragen pro zweiten (QPS)

Anzahl und Größe sind wichtig, da Höchstgrenzen bei über eine feste Einschränkung auf die Anzahl von Indizes oder Dokumenten in einem Dienst oder auf vom Dienst verwendeten Ressourcen (Speicher oder Replikate) erreicht werden. Ist der tatsächliche Grenzwert für den Dienst, je nachdem, was zuerst verwendet wird: Ressourcen oder Objekte.

Mit schätzt in Hand sollte die folgenden Schritte den Prozess zu vereinfachen:

- **Schritt 1** Überprüfen Sie die SKU Beschreibungen unten, um Informationen zu den verfügbaren Optionen an.
- **Schritt 2** Fragen Sie zu einem über der vorläufigen Entscheidung zu gelangen.
- **Schritt 3** Fertigstellen Sie Ihre Entscheidung durch harte Grenzwerte Speichermenge überprüfen und Preise an.

## <a name="sku-descriptions"></a>Beschreibungen SKU

Die folgende Tabelle enthält eine Beschreibung der einzelnen Ebenen. 

Ebene|Primäre Szenarien
----|-----------------
**Kostenlose**|Einen freigegebenen Dienst kostenlos, für die Bewertung, Untersuchung oder kleine Auslastung verwendet. Da es andere Abonnenten freigegeben wurde, Abfrage Durchsatz und Indizierung variiert je nach, wer sonst noch den Dienst verwendet wird. Kapazität ist klein (50 MB oder 3 Indizes mit von 10.000 Dokumente jede).
**Grundlegende**|Kleine Produktionsarbeitslasten auf dedizierter Hardware. Hochgradig verfügbar. Kapazität ist bis zu 3 Replikate und 1 Partition (2 GB).
**S1**|Standard 1 unterstützt flexible Kombinationen von Partitionen (12) und der (12), für das Medium Produktionsarbeitslasten auf dedizierter Hardware verwendet. Sie können Partitionen und der in Kombinationen unterstützt, um eine maximale Anzahl von 36 berechenbaren Suche Einheiten zuweisen. Diese Ebene Partitionen sind 25 GB und QPS ist etwa 15 Abfragen pro Sekunde.
**S2**|Standard 2 das größere Produktionsarbeitslasten mit den gleichen 36 Suche Einheiten aus, wie S1 jedoch mit größeren gleichgroße Teile und der ausgeführt wird. Diese Ebene Partitionen sind 100 GB und QPS ist zu 60 Abfragen pro Sekunde.
**S3**|Standard 3 wird ausgeführt proportional größeren Produktionsarbeitslasten auf höhere End-Systemen Konfigurationen mit bis zu 12 Partitionen oder 12 Replikate Einheiten unter 36 suchen. Diese Ebene Partitionen sind 200 GB und QPS ist mehr als 60 Abfragen pro Sekunde. 
**S3 HD**|Standard 3 HD wurde für eine große Anzahl von kleinere Indizes. Sie können bis zu 3 Partitionen, gleichzeitige 200 GB verwenden. QPS ist mehr als 60 Abfragen pro Sekunde. 

> [AZURE.NOTE] Replikat und Partition Höchstwerte werden, als suchen Einheiten (maximum pro Dienst 36 Unit), die eine effektive Untergrenze als impliziert das Maximum auf den ersten Blick auferlegt belastet. Angenommen, um die maximale Anzahl von 12 Replikate zu verwenden, könnten, haben Sie höchstens 3 Partitionen (12 * 3 = 36 Einheiten). Verringern Sie auf ähnliche Weise, um maximale Partitionen verwenden, Replikate zu 3. Ein Diagramm zulässigen Kombinationen finden Sie unter [Skalieren Ressourcenebenen für die Abfrage und Indizierung Auslastung in Azure suchen](search-capacity-planning.md) .

## <a name="review-limits-per-tier"></a>Überprüfen Sie die Grenzwerte pro Ebene

Das folgende Diagramm ist eine Teilmenge von den Grenzwerten von [Dienst Grenzwerte in Azure suchen](search-limits-quotas-capacity.md). Es listet die Faktoren wahrscheinlich auf eine SKU Entscheidung auswirken. Sie können auf die Tabelle verweisen, wenn Sie die folgenden Fragen zu überprüfen.

Ressource|Kostenlose|Grundlegende|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL)|Keine <sup>1</sup> |Ja |Ja  |Ja |Ja  |Ja 
Beschränkungen Indexes|3|5|50|200|200|1000 <sup>2</sup>
Grenzwerte für Dokument|10.000 gesamt|1 Millionen pro Dienst|15 Millionen pro partition |60 Millionen pro partition|120 Millionen pro partition |1 Millionen pro index
Maximale Partitionen|N/V |1 |12  |12 |12|3 <sup>2</sup>
Wert|50 MB gesamt|2 GB pro Dienst|25 GB pro partition |100 GB pro Partition (bis zu einem Maximum von 1,2 TB pro Service)|200 GB pro Partition (bis zu einem Maximum von 2,4 TB pro Service)|200 GB (bis zu 600 GB pro Service)
Maximale Replikate|N/V |3 |12 |12 |12|12
Abfragen pro Sekunde|N/V|~ 3 pro Replikation|~ 15 pro Replikation|~ 60 pro Replikation|> 60 pro Replikation|> 60 pro Replikation

<sup>1</sup> Free und Vorschau SKUs nicht mit SLAs kommen. SLAs werden erzwungen, sobald eine SKU generell zur Verfügung steht.

<sup>2</sup> werden S3 und S3 HD durch dieselbe hohe Kapazität Infrastruktur jedoch können Sie jeweils das maximale Limit auf verschiedene Arten erreicht. S3 richtet sich an eine kleinere Anzahl von sehr großen Indizes. Als solches ist das maximale Limit gebundene Ressource (2,4 TB für jeden Dienst). S3 HD richtet sich an eine große Anzahl von geringen Indizes. Bei 1.000 Indizes erreicht S3 HD seine Grenzen in Form eines Index Einschränkungen. Wenn Sie mehr als 1.000 Indizes erfordert ein S3 HD-Kunde sind, wenden Sie sich an den Microsoft-Support Informationen über die Vorgehensweise.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Unterdrücken SKUs, die nicht zu erfüllen 

Die folgenden Fragen können Einblenden der zugehörigen die richtige SKU Entscheidung für Ihre Arbeitsbelastung helfen.

1. Haben Sie **Dienst Ebene Vertrag SERVICELEVEL** Anforderungen? Grenzen Sie die Entscheidung SKU in einfache oder nicht-Vorschau Standard.
2. **Wie viele Indizes** führen Sie erforderlich? Einer der wichtigsten Variablen in einer SKU Entscheidung der Faktor ist die Anzahl der Indizes nach jeder SKU unterstützt. Index-Unterstützung wird bei deutlich verschiedenen Ebenen in den unteren Preisgestaltung Stufen. Anzahl der Indizes Anforderungen könnte einer primären Determinante einer SKU Entscheidung.
3. **Wie viele Dokumente** werden in jedem Index geladen werden? Die Anzahl und die Größe von Dokumenten werden die tatsächliche Größe des Indexes fest. Vorausgesetzt, dass Sie die geplante Größe des Indexes schätzen können, können Sie die Zahl mit dem Wert pro SKU, durch die Anzahl der Partitionen erforderlich, um einen Index dieser Größe speichern erweitert vergleichen. 
4. **Was ist die erwartete Abfrage laden**? Sobald Speicher Anforderungen verstanden werden, sollten Sie die Abfrage Auslastung. S2 und beide S3 SKUs bieten in der Nähe Entsprechung Durchsatz jedoch Vereinbarung zum SERVICELEVEL Anforderungen werden eine Vorschau SKUs auszuschließen. 
5. Wenn Sie die Ebene S2 oder S3 erwägen, bestimmen Sie, ob Sie [Indexer](search-indexer-overview.md)benötigen. Indexer sind noch nicht verfügbar für die S3 HD-Leiste. Alternativer Ansatz ist ein Modell Pushbenachrichtigungen für die Aktualisierung von Indizes verwenden, Schreiben Sie Code der Anwendung eine Datengruppe zurück an einen Index zu übertragen.

Die meisten Kunden können eine bestimmte SKU oder verkleinern Regel basierend auf ihren Antworten auf die obigen Fragen. Wenn Sie immer noch unsicher, welche SKU auf abgestimmt sind, können Sie stellen Sie Fragen in MSDN oder StackOverflow Foren, oder wenden Sie sich an Azure-Unterstützung für weitere Unterstützung.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Entscheidung Überprüfung: die SKU bietet über genügend Speicherplatz und QPS?

Als letzter Schritt besuchen Sie die [Seite Preise](https://azure.microsoft.com/pricing/details/search/) und den [Abschnitten pro Dienst und pro Index in Beschränkungen Service](search-limits-quotas-capacity.md) zum Überprüfen Sie sorgfältig, ob Ihre schätzt gegen Grenzwerte für Abonnement und Dienst ein. 

Wenn der Preis oder Speicher Anforderungen außerhalb des zulässigen Bereichs sind, sollten Sie die Auslastung zwischen mehrere kleinere Dienste (zum Beispiel) zu gestalten. Auf genauer könnten Sie Umgestaltung Indizes benutzerspezifisch kleinere oder Filter verwenden, um Abfragen effizienter zu gestalten.

> [AZURE.NOTE] Anforderungen Speicher können zu viel vergrößerte sein, wenn Dokumente irrelevante Daten enthalten. Idealerweise enthalten Dokumente nur durchsuchbare Daten oder Metadaten. Binäre Daten ist nicht durchsuchbare und sollte mit einem Feld im Index zu halten einen URL-Verweis auf die externen Daten getrennt (vielleicht in einer Azure Tabelle oder Blob-Speicher) gespeichert werden. Die maximale Größe eines einzelnen Dokuments ist 16 MB (oder weniger If Massen in einer Anforderung mehrere Dokumente hochladen werden). Weitere Informationen finden Sie unter [Service Grenzwerte in Azure suchen](search-limits-quotas-capacity.md) .

## <a name="next-step"></a>Als Nächstes

Nachdem Sie, welche SKU genau das richtige ist wissen, fahren Sie mit der folgenden Schritte aus:

- [Erstellen eines Suchdiensts im portal](search-create-service-portal.md)
- [Ändern der Zuordnung von Partitionen und der Skalierung des Diensts](search-capacity-planning.md)

