<properties
    pageTitle="Erstellen einer Azure Suchindex | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Was ist ein Index in Azure suchen, und wie wird es verwendet?"
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index"></a>Erstellen eines Indexes Azure-Suche
> [AZURE.SELECTOR]
- [(Übersicht)](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Was ist ein Index?

Einen *Index* ist ein beständiger Speicher für *Dokumente* und andere Konstrukte, die eine Azure Suchdienst verwendet. Ein Dokument handelt es sich um eine einzelne Dateneinheit durchsuchbare in Ihrem Index. Beispielsweise eine e-Commerce Einzelhandel möglicherweise müssen Sie ein Dokument für jedes Element, das sie verkaufen, eine Organisation News möglicherweise ein Dokument für jeden Artikel usw.. Diese Konzepte in vertrauter Datenbank entsprechenden Zuordnung: einen *Index* entspricht im Grunde einer *Tabelle*und *Dokumente* entsprechen ungefähr den *Zeilen* in einer Tabelle.

Wenn Sie Dokumente hinzufügen/Upload und Suchabfragen Azure zu übermitteln, senden Sie Ihre Anfragen an einem bestimmten Index in Ihrem Suchdienst.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Feldtypen und Attribute in einer Azure Suchindex

Wie Sie Ihre Schema zu definieren, müssen Sie den Namen und Typ Attribute der einzelnen Felder in Ihrem Index angeben. Der Feldtyp klassifiziert die Daten, die in diesem Feld gespeichert ist. Attribute werden auf einzelne Felder festgelegt, um anzugeben, wie das Feld verwendet wird. In den folgenden Tabellen aufgelistet werden, die Typen und Attributen, die Sie angeben können.


### <a name="field-types"></a>Feldtypen
|Typ|Beschreibung|
|------------|-----------|
|*Edm.String*|Text, den Sie optional für Voll-Textsuche (Word neuesten, Wortstamm usw.) Token.|
|*Collection(EDM.String)*|Eine Liste von Zeichenfolgen, die Sie optional für Voll-Textsuche Token. Es gibt keine theoretische Obergrenze auf die Anzahl der Elemente in einer Websitesammlung, aber die 16 MB Obergrenze auf Nutzlastgröße gilt für Websitesammlungen.|
|*Edm.Boolean*|Enthält Werte wahr/falsch.|
|*Edm.Int32*|32-Bit-Ganzzahlwerte.|
|*Int64*|64-Bit-Ganzzahlwerte.|
|*Edm.Double*|Mit doppelter Genauigkeit numerische Daten.|
|*Edm.DateTimeOffset*|Zeit Datumswerte im Format OData V4 dargestellt (z. B. `yyyy-MM-ddTHH:mm:ss.fffZ` oder `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Ein Punkt, eine geografische Position des Globus darstellt.|

Sie können ausführliche Informationen zu Azure-Suche [unterstützten Datentypen auf MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx)suchen.



### <a name="field-attributes"></a>Feldeigenschaften
|Attribut|Beschreibung|
|------------|-----------|
|*Schlüssel*|Eine Zeichenfolge, die eindeutige ID des jedes Dokument, für die Darstellung des Dokuments von verwendete bereitstellt. Jeder Index müssen nacheinander. Nur ein Feld kann die-Taste, und seinen Typ muss auf Edm.String festgelegt sein.|
|*Einer abrufbaren*|Gibt an, ob ein Feld in einem Suchergebnis zurückgegeben werden kann.|
|*Gefiltert*|Das Feld in Filterabfragen verwendet werden können.|
|*Sortierbar*|Ermöglicht es einer Abfrage zum Sortieren von Suchergebnissen mithilfe dieses Feld ein.|
|*Facetable*|Können Sie ein Feld in einer [facettierten](search-faceted-navigation.md) Navigationsstruktur für Benutzer Selbststudium absolviert Filtern verwendet werden. Arbeiten Felder, die sich wiederholende Werte, die Sie verwenden können, um mehrere Dokumente zu gruppieren enthält (z. B. mehrere Dokumente, die in einer einzelnen Marke oder Dienstkategorie fallen) in der Regel am besten Aspekte.|
|*Durchsucht*|Markiert das Feld als Volltextindex durchsucht.|

Sie können ausführliche Informationen zu Azure-Suche [Indexattribute auf MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx)suchen.



## <a name="guidance-for-defining-an-index-schema"></a>Leitfaden für ein IndexSchema definieren

Beim Entwerfen den Index nehmen Sie Ihre Zeit in der Planung Phase durch jeden Entscheidung zu berücksichtigen. Es ist wichtig, dass Sie beachten Sie Ihre Suche Benutzer Erfahrung und Business-Anforderungen beim Ihrer Index zu entwerfen, wie jedes Feld die [richtigen Attribute](https://msdn.microsoft.com/library/azure/dn798941.aspx)zugewiesen werden muss. Ändern eines Indexes aus, nach der Bereitstellung ist umfasst Neuerstellen und erneutes Laden der Daten.


Wenn Sie Speicherplatz für Daten im Laufe der Zeit ändern möchten, können Sie vergrößern oder Verkleinern von Kapazität durch Hinzufügen oder Entfernen von Partitionen. Details finden Sie unter [Verwalten der Suchdiensts in Azure](search-manage.md) oder [Beschränkungen Service](search-limits-quotas-capacity.md).
