<properties 
    pageTitle="Erste Schritte mit Azure suchen | Microsoft Azure | DocumentDB | Search-Cloud-Dienst" 
    description="Informationen Sie zum Erstellen Ihrer ersten Azure Suchindex dieses Lernprogramms Exemplarische Vorgehensweise und DocumentDB Beispieldaten verwenden. In dieser Übung Portal-basierten, codefreien verwendet der Assistent importieren." 
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
    ms.topic="hero-article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/03/2016" 
    ms.author="heidist"/>

# <a name="get-started-with-azure-search-in-the-portal"></a>Erste Schritte mit Azure suchen im Portal

Diese codefreien Einführung wird Ihnen den Einstieg in Microsoft Azure Suche mit Funktionen, die direkt in das Portal. 

Das Lernprogramm wird davon ausgegangen, eine [Azure DocumentDB Beispieldatenbank](#apdx-sampledata) Einsteiger-bis zum Erstellen unsere Daten und Anweisungen verwenden, aber Sie können auch Schritte anpassen, mit Ihrer vorhandenen Daten entweder DocumentDB oder SQL-Datenbank.

> [AZURE.NOTE] In diesem Lernprogramm Erste Schritte erfordert ein [Azure-Abonnement](/pricing/free-trial/?WT.mc_id=A261C142F) und einem [Azure Suchdienst](search-create-service-portal.md). 
 
## <a name="find-your-service"></a>Suchen nach dem Dienst

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.

2. Öffnen des Dienst Dashboards Ihrer Azure Suchdiensts. Hier sind einige Möglichkeiten, zu das Dashboard finden.
    - Klicken Sie in der Jumpbar auf **Dienste suchen**. Der Jumpbar listet jeden Dienst nach der Bereitstellung im Rahmen Ihres Abonnements. Wenn ein Suchdienst definiert wurde, wird in der Liste **Suchdienste** .
    - Klicken Sie auf **Durchsuchen** , und geben Sie "Suchen" in das Suchfeld ein, um eine Liste aller erstellt in Ihrer Abonnements Suchdienste zu erstellen, in der Jumpbar.

## <a name="check-for-space"></a>Prüfen von Speicherplatz

Viele Kunden beginnen Sie mit dem kostenlosen Dienst. Diese Version ist auf drei Indizes, drei Datenquellen und drei Indexer beschränkt. Stellen Sie sicher, dass Sie Platz für zusätzliche Elemente haben, bevor Sie beginnen. Diese exemplarische Vorgehensweise erstellt eine der einzelnen Objekte.

## <a name="create-an-index-and-load-data"></a>Erstellen Sie einen Index und Laden von Daten

Suchabfragen Durchlaufen eines *Index* mit Daten durchsucht, Metadaten und Konstrukte zum Optimieren von bestimmter Verhaltensweisen suchen aus. Als ersten Schritt definieren und einen Index zu füllen.

Es gibt mehrere Methoden zum Erstellen eines Indexes. Ist Ihre Daten in einem Speicher, den Suche Azure durchforsten können – wie etwa SQL Azure-Datenbank, SQL Server auf einem Azure-virtuellen Computer oder DocumentDB - können Sie erstellen und füllen ein Indexes sehr einfach mit einem *Indexer*.

Um diese Aufgabe Portal-basierten beibehalten möchten, verwenden wir die Daten aus DocumentDB, die mit einem Indexer über den Assistenten zum **Importieren von Daten** durchforstet werden kann. 

Erstellen Sie eine [DocumentDB Beispieldatenbank](#apdx-sampledata) zum Verwenden mit diesem Lernprogramm und zurückzukehren, in diesem Abschnitt, führen Sie die folgenden Schritte aus, bevor Sie fortfahren.

<a id="defineDS"></a>
#### <a name="step-1-define-the-data-source"></a>Schritt 1: Definieren der Datenquelle

1. Klicken Sie auf Dienst Dashboard des Azure Suchen in der Befehlsleiste, um einen Assistenten zu starten, der sowohl erstellt und ein Indexes füllt auf **Daten importieren** .

    ![][7]

2. Klicken Sie im Assistenten auf **Datenquelle** > **DocumentDB** > **Name**, geben Sie einen Namen für die Datenquelle. Eine Datenquelle ist ein Connection-Objekt in Azure suchen, die mit anderen Indexer verwendet werden kann. Nachdem Sie es erstellt haben, wird es als einer "vorhandenen Datenquelle" in Ihrem Dienst verfügbar.

3. Wählen Sie aus Ihrem vorhandenen DocumentDB-Konto, und die Datenbank und die Websitesammlung. Wenn Sie die Beispieldaten, die wir bereitstellen verwenden, sieht die DSN-Datei wie folgt aus:

    ![][2]

Beachten Sie, dass wir die Abfrage überspringen sind. Dies ist, da wir keinen dieser Zeit ungefähr änderungsnachverfolgung in unser Dataset implementieren sind. Wenn das Dataset ein Feld, das verfolgt enthält, wenn ein Datensatz aktualisiert wird, können Sie einen Indexer Azure suchen zum Änderungsprotokoll selektiven Updates auf Ihren Index verwenden konfigurieren.

Klicken Sie auf **OK** , um diesen Schritt des Assistenten abgeschlossen haben.

#### <a name="step-2-define-the-index"></a>Schritt 2: Definieren des Indexes

Klicken Sie im Assistenten klicken Sie auf **Index** , und schauen Sie sich die Entwurfsoberfläche verwendet, um eine Azure Suchindex zu erstellen. Ein Index erfordert minimal, einen Namen und eine Auflistung Felder mit einem einzigen Feld markiert die Dokument-Taste. Da wir ein Dataset DocumentDB verwenden, Felder werden vom Assistenten automatisch erkannt und der Index mit Feldern und Daten Typ Zuordnungen vorinstalliert ist. 

  ![][3]

Obwohl die Felder und Datentypen konfiguriert haben, müssen Sie Attribute zuweisen. Die Kontrollkästchen am oberen Rand der Feldliste sind *Indexattributen* , die steuern, wie das Feld verwendet wird. 

- **Retrievable** bedeutet, dass sie in der Liste mit Suchergebnissen wird. Sie können einzelne Felder markieren, wie deaktivieren Grenzwerte für Suchergebnisse, indem Sie dieses Kontrollkästchen, beispielsweise wenn Sie nur in Filterausdrücken Felder verwendet. 
- **Filterable**, **sortierbar**und **Facetable** bestimmen, ob ein Feld in einen Filter, Sortierung oder eine Navigationsstruktur Vertriebsstrategie verwendet werden kann. 
- **Durchsuchbar** bedeutet, dass ein Feld in der vollständigen Textsuche enthalten ist. Zeichenfolgen werden in der Regel durchsucht. Numerische Felder und boolesche Felder werden häufig als nicht durchsuchbar markiert. 

Bevor Sie diese Seite verlassen, markieren Sie die Felder in Ihrem Index verwenden Sie die folgenden Optionen (Retrievable, durchsuchbar usw.). Die meisten Felder sind Retrievable. Die meisten Zeichenfolgenfelder sind durchsuchbar (nicht müssen Sie nach dem Schlüssel gesucht werden). Einige Felder wie Genre, OrderableOnline, Bewertung und Tags auch sind gefiltert und sortierbaren und Facetable. 
    
Feld | Typ | Optionen |
------|------|---------|
ID | Edm.String | |
albumTitle | Edm.String | Einer abrufbaren, durchsucht |
albumUrl | Edm.String | Einer abrufbaren, durchsucht |
Genre | Edm.String | Einer abrufbaren, durchsucht, gefiltert und sortierbaren, Facetable |
genreDescription | Edm.String | Einer abrufbaren, durchsucht |
artistName | Edm.String | Einer abrufbaren, durchsucht |
orderableOnline | Edm.Boolean | Einer abrufbaren, gefiltert, sortierbar, Facetable |
Kategorien | Collection(EDM.String) | Einer abrufbaren, gefiltert, Facetable |
Kurs | Edm.Double | Einer abrufbaren, gefiltert, Facetable |
Seitenrand | Edm.Int32 | |
Bewertung | Edm.Int32 | Einer abrufbaren, gefiltert, sortierbar, Facetable |
Inventory | Edm.Int32 | Einer abrufbaren |
lastUpdated | Edm.DateTimeOffset | |

Als Ausgangspunkt Vergleich ist das folgende Bildschirmabbild eine Abbildung eines Indexes der Spezifikation in der obigen Tabelle erstellt.

 ![][4]

Klicken Sie auf **OK** , um diesen Schritt des Assistenten abgeschlossen haben.

#### <a name="step-3-define-the-indexer"></a>Schritt 3: Definieren des Indexers

Klicken Sie im Assistenten **Importieren** , klicken Sie auf **Indexer** > **Name**, geben Sie einen Namen für den Indexer, und verwenden Sie die Standardeinstellungen für alle anderen Werte. Dieses Objekt definiert einen ausführbaren Prozess. Nachdem Sie es erstellt haben, platzieren auf periodischer Zeitplan, aber für die Verwendung der jetzt die Standardoption Indexer einmal ausführen, sofort, wenn Sie auf **OK**klicken. 

Die Dateneingaben importieren sollte alle in und einsatzbereit gefüllt werden.

  ![][5]

Um den Assistenten auszuführen, klicken Sie auf **OK** , um den Importvorgang zu starten und den Assistenten zu schließen.

## <a name="check-progress"></a>Überprüfen des Fortschritts

Um den Fortschritt überprüfen möchten, wechseln Sie zurück zu dem Dienst Dashboard einen Bildlauf nach unten, und doppelklicken Sie auf die Kachel **Indexer** zum Öffnen der Liste Indexer aus. Sollte den soeben in der Liste erstellte Indexer angezeigt werden und Sie auftreten, der angibt, "in Bearbeitung" Status oder Erfolg, sowie die Anzahl der Dokumente indiziert in Azure suchen.

  ![][6]

## <a name="query-the-index"></a>Abfrage des Indexes

Sie verfügen nun über eine Suchindex, der Abfragen bereit ist. 

**Search Explorer** ist eine Abfragetools im Portal integriert. Es bietet ein Suchfeld, sodass Sie können überprüfen, dass eine Suche Eingabemethoden die Daten zurückgibt, die Sie erwartet. 

1. Klicken Sie auf der Befehlsleiste auf **Explorer suchen** .
2. Beachten Sie, welcher Index aktiv ist. Wenn sie den soeben erstellten nicht ist, klicken Sie auf der Befehlsleiste in den gewünschten auswählen **Ändern Index** .
2. Lassen Sie das Suchfeld leer, und klicken Sie dann auf die Schaltfläche **Suchen** , um eine Suche Platzhalterzeichen ausführen möchten, die alle Dokumente zurückgibt.
3. Geben Sie ein paar voll-Textsuche Abfragen. Sie können die Ergebnisse aus Ihrer Suche Platzhalterzeichen zum Interpreten, Alben und Genres Abfragen kennenzulernen zu überprüfen.
4. Versuchen Sie andere Abfragesyntax mithilfe der [Beispiele finden Sie am Ende dieses Artikels](https://msdn.microsoft.com/library/azure/dn798927.aspx) für Ideen, ändern Ihre Abfrage zum von Suchzeichenfolgen verwenden, die sich in Ihrem Index gefunden werden können.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Assistenten auszuführen, Sie können zurückgehen und anzeigen oder ändern einzelne Komponenten: Index, Indexer oder einer Datenquelle. Einige Bearbeitungen, wie etwa das Ändern des Datentyp des Felds für den Index sind nicht zulässig, aber die meisten Eigenschaften und Einstellungen können geändert werden. Um die einzelnen Komponenten anzuzeigen, klicken Sie auf den **Index**, **Indexer**oder **Datenquellen** Kacheln auf des Dashboards, um eine Liste der vorhandenen Objekte anzeigen.

Weitere Informationen zu anderen in diesem Artikel genannten Features finden Sie auf folgenden Links:

- [Indexer](search-indexer-overview.md)
- [Erstellen von Index (umfasst eine ausführliche Erläuterung der Indexattribute)](https://msdn.microsoft.com/library/azure/dn798941.aspx)
- [Search-Explorer](search-explorer.md)
- [Suchen von Dokumenten (einschließlich Beispiele Abfragesyntax)](https://msdn.microsoft.com/library/azure/dn798927.aspx)

Sie können diese desselben Workflows, versuchen, mit dem Import-Assistenten bei anderen Datenquellen wie SQL Azure-Datenbank oder SQL Server auf Azure-virtuellen Computern.

> [AZURE.NOTE] Indexer-Unterstützung für die Durchforstung Azure BLOB-Speicher angekündigt neu ist, aber dieses Feature ist in der Vorschau und noch nicht über die Option Portal. Wenn Sie die Indexer versuchen, müssen Sie zum Schreiben von Code. Weitere Informationen finden Sie unter [Indizierung Azure Blob-Speicher in Azure-Suche](search-howto-indexing-azure-blob-storage.md) .
<a id="apdx-sampledata"></a>


## <a name="appendix-create-sample-data-in-documentdb"></a>Anlage: Erstellen von Beispieldaten in DocumentDB

In diesem Abschnitt erstellt eine kleine Datenbank in DocumentDB, die zum Abschließen der Vorgänge in diesem Lernprogramm verwendet werden kann.

Die folgenden Anweisungen bieten Ihnen Faustregel, jedoch sind nicht vollständig. Wenn Sie weitere Hilfe zum DocumentDB Portal Navigation oder Aufgaben benötigen, können Sie auf DocumentDB Dokumentation verweisen, aber die meisten benötigte Befehle sind in der Befehlsleiste Dienst am oberen Rand des Dashboards oder in der Datenbank Blade. 

  ![][1]

### <a name="create-musicstoredb-for-this-tutorial"></a>Musicstoredb in diesem Lernprogramm erstellen

1. [Klicken Sie hier](https://github.com/HeidiSteen/azure-search-get-started-sample-data) , eine ZIP-Datei, die mit Musik Speicherdateien JSON herunterladen. Wir erläutern die notwendigen 246 JSON-Dokumente für diesen Dataset.
2. Ihr Abonnement DocumentDB hinzu, und öffnen Sie dann auf das Dashboard Dienst.
2. Klicken Sie auf **Datenbank hinzufügen** , um eine neue Datenbank erstellen, mit der Id `musicstoredb`. Es wird angezeigt, in der Datenbank-Kachel weiter unten auf der Seite, sobald sie erstellt wurde.
2. Klicken Sie auf den Datenbanknamen, um das Blade Datenbank zu öffnen.
3. Klicken Sie auf **Sammlung hinzufügen** zum Erstellen einer Websitesammlungs mit der Id `musicstorecoll`.
3. Klicken Sie auf **Dokumentexplorer**.
4. Klicken Sie auf **Hochladen**.
5. Navigieren Sie im **Dokument hochladen**zu dem lokalen Ordner, der JSON-Dateien enthält, die Sie zuvor heruntergeladen haben. Auswählen von JSON-Dateien in Stapeln von 100 oder weniger.
    - 386. json
    - 387. json
    - . . .
    - 486. json
6. Wiederholen Sie die zum Abrufen der nächsten Gruppe von Dateien, bis Sie den letzten eine 669.json hochgeladen haben.
7. Klicken Sie auf **Abfrage-Explorer** zum Überprüfen, dass die Daten hochgeladen werden, um von Dokument-Explorer hochladen erfüllen.

Eine einfache Möglichkeit, die hierfür besteht darin, die Standardabfrage verwenden, aber Sie können auch ändern der Standardabfrage, damit er wählt im oberen Bereich 300 (Es stehen weniger als 300 Elemente in diesem Dataset).

Sie sollten ausgeben, JSON Dokument Zahl 386, und zwar angefangen, die mit dem Dokument 669 zurückzukehren. Nachdem Sie die Daten geladen haben, können Sie [zurück zu den Schritten in diesem Exemplarische Vorgehensweise](#defineDS) zum Erstellen eines Indexes mit dem **Datenimport-Assistenten**.


<!--Image references-->
[1]: ./media/search-get-started-portal/AzureSearch-GetStart-Docdbmenu1.png
[2]: ./media/search-get-started-portal/AzureSearch-GetStart-DataSource.png
[3]: ./media/search-get-started-portal/AzureSearch-GetStart-DefaultIndex.png
[4]: ./media/search-get-started-portal/AzureSearch-GetStart-FinishedIndex.png
[5]: ./media/search-get-started-portal/AzureSearch-GetStart-ImportReady.png
[6]: ./media/search-get-started-portal/AzureSearch-GetStart-IndexerList.png
[7]: ./media/search-get-started-portal/search-data-import-wiz-btn.png
