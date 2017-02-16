<properties
    pageTitle="Importieren von Daten in Azure-Suche verwenden von Indexern im Portal Azure | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Verwenden Sie die Azure suchen Datenimport-Assistenten im Portal Azure Durchforstung Daten aus Azure BLOB-Speicher, weiteres Tabelle, SQL-Datenbank und SQL Server auf Azure-virtuellen Computern."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Importieren von Daten in Azure suchen, die mit dem portal

Azure-Portal stellt den Assistenten **Importieren von Daten** auf dem Dashboard Azure-Suche für das Laden von Daten in einen Index bereit. 

  ![Importieren von Daten auf der Befehlsleiste][1]

Intern, der Assistent konfiguriert und ruft einen *Indexer*, mehrere Maßnahmen, die von der Indizierung zu automatisieren: 

- Verbinden Sie mit einer externen Datenquelle im aktuellen Azure-Abonnement
- AutoGenerate ein IndexSchema auf Grundlage der Datenstruktur Quelle
- Erstellen Sie Dokumente basierend auf einem Rowset aus der Datenquelle abgerufen werden
- Hochladen von Dokumenten auf den Index in der Suchdienst

Sie können diesen Workflow Verwenden von Beispieldaten in DocumentDB ausprobieren. Anweisungen finden Sie unter [Erste Schritte mit Azure suchen im Portal Azure](search-get-started-portal.md) .

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Vom Import-Assistenten unterstützte Datenquellen

Der Import-Assistent unterstützt die folgenden Datenquellen: 

- SQL Azure-Datenbank
- Relationale SQL Server-Daten eine Azure-virtuellen Computers
- Azure DocumentDB
- Azure Blob-Speicher (in der Vorschau)
- Azure Table Storage (in der Vorschau)

Eine flache Dataset ist eine erforderliche Eingabe. Sie können nur aus einer einzelnen Tabelle, die Datenbankansicht oder die entsprechende Datenstruktur importieren. Sie sollten diese Datenstruktur vor dem Ausführen des Assistenten erstellen.

Beachten Sie, dass einige der die Indexer immer noch in der Seitenansicht sind, was bedeutet, dass die Indexer Definition von der Preview-Version der API unterstützt wird. Weitere Informationen und Links finden Sie unter [Übersicht über die Indexer](search-indexer-overview.md) .

## <a name="connect-to-your-data"></a>Herstellen einer Verbindung von Daten mit

1. Melden Sie sich zum Dashboard Service öffnen und [Azure-Portal](https://portal.azure.com) aus. Sie können in der Leiste Sprung zum Anzeigen der vorhandenen Services in das aktuelle Abonnement **Dienste suchen** klicken. 

2. Klicken Sie auf der Befehlsleiste zum Öffnen der Folie das Blade Daten importieren auf **Daten importieren** .  

3. Klicken Sie auf **Verbinden mit Daten** zum Angeben eines die DSN-Datei, die von einem Indexer verwendet. Für Datenquellen Abonnement innerhalb der Assistent normalerweise erkennen und Weitere Informationen zur Verbindung Anforderungen für insgesamt Konfiguration minimieren.

| | |
|--------|------------|
|**Vorhandenen Datenquelle** | Wenn Sie bereits Indexer Suchdienst definiert haben, können Sie eine vorhandene Datendefinitionsabfrage für eine andere importieren auswählen.|
|**SQL Azure-Datenbank** | Namen, die Anmeldeinformationen für einen Datenbankbenutzer mit Leseberechtigung und einen Datenbanknamen können auf der Seite oder über einer ADO.NET-Verbindungszeichenfolge angegeben werden muss. Wählen Sie die Option für die Verbindungszeichenfolge anzeigen oder Anpassen der Eigenschaften aus. <br/><br/>Die Tabelle oder die Ansicht, die das Rowset bereitstellt, muss auf der Seite angegeben werden. Diese Option wird angezeigt, nachdem die Verbindung hergestellt wurde, mit einem Dropdown-Listenfeld zugewiesen, damit Sie eine Auswahl treffen können.|
|**SQLServer Azure-virtuellen Computers** | Geben Sie einen vollqualifizierter Dienstname, Benutzer-ID und das Kennwort sowie Datenbank als Verbindungszeichenfolge aus. Wenn Sie diese Datenquelle verwenden zu können, muss zuvor ein Zertifikat im lokalen Speicher installiert sein, die die Verbindung verschlüsselt. <br/><br/>Die Tabelle oder die Ansicht, die das Rowset bereitstellt, muss auf der Seite angegeben werden. Diese Option wird angezeigt, nachdem die Verbindung hergestellt wurde, mit einem Dropdown-Listenfeld zugewiesen, damit Sie eine Auswahl treffen können.
|**DocumentDB** |Anforderungen beziehen Sie das Konto, Datenbank und Websitesammlung. Alle Dokumente in die Sammlung werden im Index enthalten sein. Sie können eine Abfrage zu reduzieren, oder als filter Rowset oder geänderte Dokumente für nachfolgende aktualisieren Datenoperationen erkennen definieren.|
|**Azure Blob-Speicher** | Das Konto Speicherplatz und eines Containers umfassen. Wenn eine virtuelle Benennungskonvention für die Gruppierung Blob Namen gehen Sie vor, können Sie optional den virtuelle Verzeichnis Teil des Namens als Ordner unter Container angeben. Weitere Informationen finden Sie unter [Indizierung BLOB-Speicher (Preview)](search-howto-indexing-azure-blob-storage.md) . |
|**Azure Table Storage** | Anforderungen gehören Speicher-Konto und einen Tabellennamen ein. Optional können Sie eine Abfrage, um eine Teilmenge der Tabellen abrufen angeben. Weitere Informationen finden Sie unter [Indizierung Tabellenspeicher (Preview)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Zielindex anpassen

Ein Index über der vorläufigen wird in der Regel aus dem Dataset abgeleitet. Hinzufügen, bearbeiten oder Löschen von Feldern zum Vervollständigen des Schemas. Legen Sie darüber hinaus Attribute Ebene der Feld seine nachfolgenden Suchen Verhalten zu bestimmen.

1. **Anpassen Zielindex**Geben Sie den Namen und einen **Schlüssel** verwendet, um jedes Dokument eindeutig identifizieren. Die Taste muss eine Zeichenfolge sein. Wenn Feldwerte Leerzeichen oder Striche enthalten müssen Sie unter **Importieren von Daten** wird die Überprüfung für diese Zeichen erweiterte Optionen festlegen.

2. Überprüfen Sie und Überarbeiten Sie die restlichen Felder. Feldname und Typ werden in der Regel für Sie ausgefüllt. Sie können den Datentyp ändern.

3. Indizieren von Attributen für jedes Feld fest

 - Einer abrufbaren gibt das Feld in den Suchergebnissen angezeigt.
 - Gefiltert können Sie das Feld in Filterausdrücke verwiesen werden.
 - Sortierbar können Sie das Feld, in die Sortierung verwendet werden soll.
 - Facetable wird das Feld für die facettierten Navigation aktiviert.
 - Durchsuchbare ermöglicht voll-Textsuche.
  
4. Wenn Sie eine Sprache Analyse Ebene der Feld angeben möchten, klicken Sie auf der Registerkarte **Analyzer** . Zurzeit können nur Sprache Analyzern angegeben werden muss. Verwenden einer benutzerdefinierten Analyse oder eine andere Sprache Analyse wie Schlüsselwort, Muster usw., wird Code erforderlich.

 - Klicken Sie auf **durchsuchbar** so bestimmen voll-Textsuche auf das Feld, und aktivieren Sie die Dropdownliste Analyzer.
 - Wählen Sie den gewünschte Analyzer. Details finden Sie unter [Erstellen eines Indexes für Dokumente in mehreren Sprachen](search-language-support.md) .

5. Klicken Sie auf die **Suggester** , um während der Eingabe Abfragevorschläge ausgewählten Felder zu aktivieren.


## <a name="import-your-data"></a>Importieren von Daten

1. Geben Sie unter **Importieren von Daten**einen Namen für den Indexer aus. Denken Sie daran, dass das Produkt des Import-Assistenten ein Indexer ist. Später, wenn Sie es anzeigen oder bearbeiten möchten, wählen Sie es aus dem Portal und nicht durch den Assistenten erneut aus. 

2. Geben Sie den Zeitplan, die auf die regionalen Zeitzone basiert, in dem der Dienst bereitgestellt wird.

3. Legen Sie erweiterte Optionen, Schwellenwerte anzugeben, ob Indizierung fortgesetzt werden kann, wenn ein Dokument gelöscht wird. Darüber hinaus können Sie angeben, ob **Schlüsselfelder** Leerzeichen und Schrägstrichen enthalten dürfen.  

## <a name="edit-an-existing-indexer"></a>Bearbeiten eines vorhandenen Indexers

Doppelklicken Sie im Dashboard Service auf die Kachel Indexer auf die Folie, die eine Liste aller Indexer für Ihr Abonnement erstellt. Doppelklicken Sie auf eine der die Indexer ausführen, bearbeiten oder löschen können. Sie können den Index durch einen anderen vorhandenen ersetzen, Ändern der Datenquelle und Festlegen von Optionen für Schwellenwerte für Fehler bei der Indizierung.

## <a name="edit-an-existing-index"></a>Bearbeiten eines vorhandenen Indexes

In Azure suchen benötigen strukturelle Aktualisierungen eines Indexes neu zu erstellen, Index besteht aus den Index löschen, den Index, neu zu erstellen und Neuladen Daten. Ändern des Datentyps und Umbenennen oder Löschen eines Felds vorgenommene strukturelle Aktualisierungen umfassen.

Änderungen, die eine Wiederherstellung erfordern nicht enthalten, Hinzufügen eines neuen Felds, Punktzahl Profile Suggesters ändern oder Ändern der Sprache Analyzern ändern. Weitere Informationen finden Sie unter [Index aktualisieren](https://msdn.microsoft.com/library/azure/dn800964.aspx) .

## <a name="next-step"></a>Als Nächstes

Überprüfen Sie die folgenden Links, um weitere Informationen zu Indexern finden:

- [Indizierung SQL Azure-Datenbank](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB Indizierung](../documentdb/documentdb-search-indexer.md)
- [Indizierung BLOB-Speicher (Preview)](search-howto-indexing-azure-blob-storage.md)
- [Indizierung Tabellenspeicher (Preview)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

