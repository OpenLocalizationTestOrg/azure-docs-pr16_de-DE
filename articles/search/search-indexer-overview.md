<properties
    pageTitle="Indexer in Azure suchen | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Durchforsten von SQL Azure-Datenbank, DocumentDB, oder Azure-Speicher um durchsuchbare Daten extrahieren und Füllen einer Azure Suchindex."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="indexers-in-azure-search"></a>Indexer in Azure suchen
> [AZURE.SELECTOR]
- [(Übersicht)](search-indexer-overview.md)
- [Portal](search-import-data-portal.md)
- [SQL Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [BLOB-Speicher (Preview)](search-howto-indexing-azure-blob-storage.md)
- [Table Storage (Preview)](search-howto-indexing-azure-tables.md)

**Indexer** in Azure suchen ist ein Agent, die durchsucht werden Daten und Metadaten aus einer externen Datenquelle extrahiert und füllt einen Index basierend auf nach Feld Zuordnungen den Index und der Datenquelle. Dieser Ansatz ist manchmal als 'Abruf Model' bezeichnet, da der Dienst zieht Daten ohne dass Sie keinen Code schreiben, der Daten auf einen Index legt.

Sie können einen Indexer verwenden, wie die einzige für Daten Aufnahme bedeutet, oder verwenden eine Kombination der Verfahren, die die Verwendung eines Indexers zum Laden nur einige Felder in der Index enthalten.

Sie können Indexer bei Bedarf ausführen oder auf einer periodischen Daten aktualisieren planen, die so oft wie alle 15 Minuten ausgeführt wird. Häufigere Updates erfordern ein Pushbenachrichtigungen Modell, die gleichzeitig Daten in Azure suchen und in der externen Datenquelle aktualisiert.

## <a name="approaches-for-creating-and-managing-indexers"></a>Verfahren zum Erstellen und Verwalten von Indexern

Für allgemein verfügbar Indexer wie SQL Azure oder DocumentDB können Sie erstellen und Verwalten von Indexern mithilfe der folgenden Verfahren:

- [Portal > Datenimport-Assistent](search-get-started-portal.md)
- [Dienst REST-API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

Anzeigen einer Vorschau Indexern, z. B. Azure Blob oder Table Storage, Code erforderlich und Anzeigen einer Vorschau APIs solche [Azure Suche Preview REST API für Indexer](search-api-indexers-2015-02-28-preview.md). Portal Tools ist in der Regel nicht verfügbar für Preview-Features.

## <a name="basic-configuration-steps"></a>Grundlegenden Konfigurationsschritte

Indexer bieten Features, die in der Datenquelle eindeutig sind. In diesem Zusammenhang sind einige Aspekte des Indexer oder Daten zum Konfigurieren von Datenquellen von Indexertyp unterschiedlich. Alle Indexer jedoch freigeben, die gleiche grundlegende Zusammensetzung und Anforderungen. Schritte, die alle Indexer feldbezogene werden unten behandelt.

### <a name="step-1-create-an-index"></a>Schritt 1: Erstellen eines Indexes

Indexer werden einige Vorgänge im Zusammenhang mit Daten Erfassung automatisieren, einen Index erstellen, nicht jedoch eine von ihnen. Voraussetzung ist müssen Sie einen vordefinierten Index mit Feldern, die Zeilen in der externen Datenquelle entsprechen. Weitere Informationen zur Strukturierung eines Indexes finden Sie unter [Erstellen eines Indexes (Azure Suche REST-API)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### <a name="step-2-create-a-data-source"></a>Schritt 2: Erstellen einer Datenquelle

Ein Indexer zieht Daten aus einer **Datenquelle** die Informationen, wie etwa einer Verbindungszeichenfolge enthält. Derzeit werden die folgenden Datenquellen unterstützt:

- [Azure SQL-Datenbank oder SQL Server auf eine Azure-virtuellen Computern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob-Speicher (Preview)](search-howto-indexing-azure-blob-storage.md), zum Extrahieren von Text aus der PDF-Datei, Office-Dokumente, HTML- oder XML-verwendet wird
- [Azure Table Storage (Preview)](search-howto-indexing-azure-tables.md)

Was bedeutet, dass eine Datenquelle von mehrere Indexer verwendet werden kann, laden Sie mehr als einen Index jeweils Datenquellen konfiguriert und verwaltet werden, unabhängig von der Indexer, in denen diese verwendet. 

### <a name="step-3create-and-schedule-the-indexer"></a>Schritt 3: Erstellen und planen den Indexer

Die Definition Indexer ist eine Angabe des Indexes, Datenquelle und einen Zeitplan erstellen. Indexer kann eine Datenquelle aus einem anderen Dienst verweisen, solange die Datenquelle aus dem gleichen Abonnement ist. Weitere Informationen zur Strukturierung Indexer finden Sie unter [Erstellen Indexer (Azure Suche REST-API)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie grundlegende Kenntnisse haben, besteht der nächste Schritt Anforderungen und spezifische Aufgaben für jeden Datenquellentyp überprüfen.

- [Azure SQL-Datenbank oder SQL Server auf eine Azure-virtuellen Computern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob-Speicher (Preview)](search-howto-indexing-azure-blob-storage.md), zum Extrahieren von Text aus der PDF-Datei, Office-Dokumente, HTML- oder XML-verwendet wird
- [Azure Table Storage (Preview)](search-howto-indexing-azure-tables.md)
- [Indizierung CSV-Blobs unter Verwendung des Indexers BLOB-Azure-Suche (Preview)](search-howto-index-csv-blobs.md)
- [Indizierung JSON-Blobs mit Azure suchen Blob Indexer (Preview)](search-howto-index-json-blobs.md)

