<properties
    pageTitle="Service Grenzwerte in Azure suchen | Microsoft Azure"
    description="Dienst Grenzwerte für die Planung der Kapazität verwendet und Höchstgrenzen bei der Besprechungsanfragen und Antworten für Azure suchen."
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

# <a name="service-limits-in-azure-search"></a>Dienst Grenzwerte in Azure suchen

Höchstgrenzen bei der Speicher, Auslastung und Mengen von Indizes, Dokumenten und anderen Objekten hängen davon ab, ob Sie Azure Suche bei einer **frei**, **grundlegende**oder **Standard** Preise Ebene hinzuzufügen.

- **Free** ist mit mehreren Mandanten freigegebener Dienst, der mit Ihrem Azure-Abonnement enthalten ist. Es ist eine Option für keine zusätzliche Kosten für vorhandenen Abonnenten, die Sie mit dem Dienst experimentieren, bevor für dedizierte Ressourcen anmelden kann. 
- **Grundlegende** bietet dedizierte computing-Ressourcen für Produktionsarbeitslasten bei kleinerem Maßstab.
- **Standard** auf dedizierten Computern, die mit mehr Speicher und Verarbeitung Kapazität auf jeder Ebene ausgeführt wird. Standard Datenbankobjekt vier Ebenen: S1, S2, S3 und S3 HD (S3 HD).

Alle Ebenen können [im Portal bereitgestellt](search-create-service-portal.md)werden. Dienst wird anfangs eine Partition und eine Replikation reserviert, aber Sie können die Zuteilung der Ressource erhöhen, sobald der Dienst erstellt wird. 

Dienst wird auf eine bestimmte Stufe bereitgestellt. Wenn Sie benötigen, und Ebenen, um weitere Kapazität zu erhalten, müssen Sie einen neuen Dienst bereitstellen (es ist keine in-Place-Aktualisierung). Weitere Informationen zu Ebenen finden Sie unter [Auswählen eines SKU oder Ebene](search-sku-tier.md). Weitere Informationen zum Anpassen der Kapazität in einem Dienst, die, den Sie bereits bereitgestellt haben, finden Sie unter [Skalieren Ressourcenebenen für die Abfrage und Indizierung Auslastung](search-capacity-planning.md).

## <a name="per-subscription-limits"></a>Pro Abonnement Beschränkungen

[AZURE.INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="per-service-limits"></a>Pro Beschränkungen service ##

[AZURE.INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="per-index-limits"></a>Pro Beschränkungen Indexes ##

Es gibt eine 1: 1-Beziehung zwischen auf Indizes sowie Begrenzung auf Indexer aus. Maximal 200 Indizes wird angegeben, ist das maximale Limit für Indexer 200 für den gleichen Dienst.

Ressource|Kostenlose|Grundlegende |S1|S2|S3 |S3 HD
---|---|---|---|---- |---|----
Index: maximale Felder pro index|1000|100 <sup>1</sup>|1000|1000|1000|1000 
Index: Maximum bewerten Profile pro index|16|16|16|16|16|16 
Index: maximale Funktionen pro Profil|8|8|8|8|8|8 
Indexer: maximale Indizierung Last pro aufrufen|10.000 Dokumente|Beschränkung durch maximal Dokumente nur|Beschränkung durch maximal Dokumente nur|Beschränkung durch maximal Dokumente nur|Beschränkung durch maximal Dokumente nur|N/v <sup>2</sup> 
Indexer: laufende Höchstdauer|3 Minuten|24 Stunden|24 Stunden|24 Stunden|24 Stunden|N/v <sup>2</sup> 
BLOB Indexer: maximale Blob-Größe, MB|16|16|128|256|256|N/v <sup>2</sup> 
BLOB Indexer: maximale Anzahl von Zeichen von Inhalten aus einem Blob extrahiert|32.000|64.000|4 Millionen|4 Millionen|4 Millionen|N/v <sup>2</sup> 

<sup>1</sup> grundlegende Ebene ist der einzige SKU mit einer Untergrenze 100 pro Index für die Felder.

<sup>2</sup> : S3 HD unterstützt keine aktuell Indexer. Wenn Sie eine dringende für diese Funktion benötigen, wenden Sie sich an Azure-Support.

## <a name="document-size-limits"></a>Grenzwerte für die Nachrichtengröße ##

Ressource|Kostenlose|Grundlegende |S1|S2|S3|S3 HD 
---|---|---|---|---- |---|----
Einzelne Dokumentgröße pro Index-API|< 16 MB|< 16 MB|< 16 MB |< 16 MB|< 16 MB|< 16 MB

Verweist auf die maximale Dokumentgröße ein, wenn eine Index-API aufrufen. Dokumentgröße ist tatsächlich eine Beschränkung der Größe des Anforderungstexts Index-API. Da eine Reihe von mehreren Dokumenten an die Index-API gleichzeitig übergeben werden können, hängt das Größenlimit tatsächlich wie viele Dokumente im Stapel sind. Für eine Reihe für ein einzelnes Dokument ist die maximale Dokumentgröße 16 MB JSON.

Um die Dokumentgröße zu halten, denken Sie daran nicht abfragbar Daten aus der Anforderung ausschließen. Bilder und andere binären Daten können nicht direkt abgefragt werden und dürfen nicht im Index gespeichert werden. Um nicht abfragbar Daten in Suchergebnissen zu integrieren, definieren Sie ein nicht durchsuchbare Feld, das einen URL-Verweis auf die Ressource speichert.

## <a name="workload-limits-queries-per-second"></a>Arbeitsbelastung Grenzwerte (Abfragen pro Sekunde) ##

Ressource|Kostenlose|Grundlegende|S1|S2|S3|S3 HD
---|---|---|---|----|---|----
QPS|N/V|~ 3 pro Replikation|~ 15 pro Replikation|~ 60 pro Replikation|> 60 pro Replikation|> 60 pro Replikation

Abfragen pro Sekunde (QPS) ist eine Näherung basierend auf Heuristik, mit simulierten und tatsächlichen Kunden Auslastung geschätzte Werten abgeleitet werden. Genaue QPS Durchsatz abhängig von Ihrer Daten und der Art der Abfrage.

Zwar groben Schätzung über bereitgestellt werden, ist eine tatsächliche Rate schwierig zu bestimmen, wo Durchsatz auf verfügbare Bandbreite und induzierten Risikos für Systemressourcen basiert, vor allem in der kostenlosen gemeinsamen Dienste. In der kostenlosen Ebene sind Datenverarbeitung und Speicher Ressourcen von mehreren Abonnenten freigegeben, deshalb QPS für Ihre Lösung immer variieren je nachdem, wie viele aufgrund der Ergebnisse zur gleichen Zeit ausgeführt werden. 

Standard auf oberster Ebene können Sie QPS genauer schätzen, da Sie die Kontrolle über mehrere Parameter haben. Finden Sie unter bewährte Methoden für die in der im Abschnitt [Verwalten Ihrer Lösung für die Suche](search-manage.md) Leitfaden zum QPS für Ihre Last zu berechnen. 

## <a name="api-request-limits"></a>Grenzwerte-API-Anforderung

- Maximal 16 MB pro Anforderung <sup>1</sup>
- Maximale Länge von 8 KB-URL
- Maximal 1000 Dokumente pro Stapel des Indexes hochlädt, zusammengeführt oder gelöscht werden
- Maximale 32 Felder in $orderby-Klausel
- Maximale suchen Ausdruck Größe ist 32.766 Bytes (32 KB minus 2 Bytes) von UTF-8-codierte text

<sup>1</sup> in Azure Suche unterliegt der Hauptteil einer Anforderung eine Obergrenze von 16 MB, eine praktische Beschränkung auf den Inhalt einzelner Felder oder Websitesammlungen, die andernfalls nicht durch theoretischen Grenzwerte eingeschränkt werden (Weitere Informationen zu Feldaufbaus und Einschränkungen finden Sie unter [unterstützte Datentypen](https://msdn.microsoft.com/library/azure/dn798938.aspx) ) festlegen.

## <a name="api-response-limits"></a>Grenzwerte für API Antwort

- Maximal 1000 Dokumente pro Seite mit Suchergebnissen zurückgegeben
- Maximale 100 Vorschläge pro vorschlagen API Anforderung zurückgegeben

## <a name="api-key-limits"></a>Grenzwerte für API-Schlüssel

API-Schlüssel sind für Dienstauthentifizierung verwendet. Es gibt zwei Arten aus. Administrator Tasten in der Kopfzeile Anforderung angegeben sind, und gewähren Lese-und Schreibzugriff Zugriff auf den Dienst. Abfrage Schlüssel sind schreibgeschützt, angegeben haben, klicken Sie auf die URL, und in der Regel an Clientanwendungen verteilt.

- Bis zu 2 pro Dienst Admin-Taste
- Bis zu 50 Abfrage Tasten pro Dienst
