<properties
    pageTitle="Hochladen von Daten in Azure suchen | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Informationen Sie zum Hochladen von Daten in einen Index in Azure suchen."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Hochladen von Daten in Azure-Suche
> [AZURE.SELECTOR]
- [(Übersicht)](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Hochladen Modelle in Azure Suchen von Daten
Es gibt zwei Methoden zum Azure Suchindex für Ihre Daten zu füllen. Die erste Option ist Ihre Daten manuell in den Index mithilfe der Suche Azure [REST-API](search-import-data-rest-api.md) oder [.NET SDK](search-import-data-dotnet.md)ablegen. Die zweite Option, die Ihre Azure Suchindex [zeigen eine unterstützte Datenquelle](search-indexer-overview.md) ist und Azure suchen, ziehen Sie die Daten automatisch in den Suchdienst lassen.

Mit diesem Leitfaden wird nur Anweisungen zur Verwendung von Pushbenachrichtigungen Modell von Daten hochladen (die nur in den [REST-API](search-import-data-rest-api.md) und [.NET SDK](search-import-data-dotnet.md)unterstützt wird) behandelt, jedoch noch weitere zum Abruf Modell unten.

### <a name="push-data-to-an-index"></a>Verschieben von Daten auf einen index

Dieser Ansatz bezieht sich auf programmgesteuert Senden von Daten zu Azure zur Verfügung steht, für die Suche. Für Applikationen Probleme sehr niedrig Wartezeit Anforderungen (z. B., wenn Sie Vorgänge synchron mit dynamischen Inventory Datenbanken benutzerspezifisch suchen müssen), das Modell Pushbenachrichtigungen ist Ihre einzige Option.

Sie können die [REST-API](https://msdn.microsoft.com/library/azure/dn798930.aspx) oder [.NET SDK](search-import-data-dotnet.md) Pushbenachrichtigungen von Daten in einen Index verwenden. Es gibt es zurzeit keine Unterstützung für wie Daten über das Portal Tools.

Dieser Ansatz ist flexibler als das Modell ziehen, da Sie einzeln oder in Stapeln Dokumente hochladen können (bis zu 1000 pro Blatt oder 16 MB, abhängig von dem Limit gehört zum ersten). Das Modell Pushbenachrichtigungen können auch zum Hochladen von Dokumenten zu Azure unabhängig davon, wo sich die Daten befinden.

### <a name="pull-data-into-an-index"></a>Abrufen von Daten in einen index

Das Modell Abruf unterstützte Datenquelle durchsucht und wird automatisch für Sie die Daten in Sie Azure Suchindex hochgeladen. Durch das Nachverfolgen von Änderungen und löscht vorhandenen Dokumenten neben neue Dokumente, entfernen Indexer müssen die Daten in Ihrem Index aktiv zu verwalten.

In Azure suchen wird diese Funktion über *Indexer*, derzeit verfügbar für [BLOB-Speicher (Preview)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [SQL Azure-Datenbank und SQL Server auf Azure-virtuellen Computern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)implementiert.

Die Funktionalität Indexer wird im [Portal Azure](search-import-data-portal.md) ebenfalls wie die [REST-API](https://msdn.microsoft.com/library/azure/dn946891.aspx)verfügbar gemacht.
