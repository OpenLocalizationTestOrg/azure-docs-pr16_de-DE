<properties
   pageTitle="Zum Ermitteln von Datenquellen | Microsoft Azure"
   description="Gewusst wie-Artikel zum Ermitteln eingetragene Datenbestände mit Azure Datenkatalog, einschließlich suchen und Filtern und die Verwendung des Funktionen des Portals Azure Datenkatalog Hervorhebung Treffer hervorheben."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Zum Ermitteln von Datenquellen

## <a name="introduction"></a>Einführung
**Microsoft Azure Datenkatalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und der Suche für Enterprise-Datenquellen System dient. Kurzum, geht **Azure Datenkatalog** beiträgt ermitteln, verstehen und Verwenden von Datenquellen und Hilfe Organisationen, um größeren Nutzen aus ihrer vorhandenen Daten zu ziehen. Nachdem Sie eine Datenquelle mit **Azure Datenkatalog**registriert wurde, wird ihre Metadaten vom Dienst, indiziert, sodass die Benutzer einfach durchsuchen können, um die Daten zu entdecken benötigten.

## <a name="searching-and-filtering"></a>Suchen und Filtern

Suche in **Azure Datenkatalog** verwendet zwei primäre Methoden: Suchen und filtern.

Suche bietet die intuitive und leistungsfähige – standardmäßig, Suchbegriffe mit jeder Eigenschaft im Katalog, einschließlich Benutzer bereitgestellte Anmerkungen abgeglichen werden.

Filtern wird als Ergänzung suchen. Benutzer können Besonderheiten wie Experten, Datenquellentyp, Objekttyp und Tags, zum Anzeigen von nur übereinstimmende Datenbestände und zum Einschränken der Suchergebnisse in Passende Posten ebenfalls auswählen.

Mithilfe einer Kombination von Suchen und Filtern können Benutzer schnell die Datenquellen navigieren, die mit **Azure Datenkatalog** zum Ermitteln von der Datenquellen benötigten erfasst wurden.

## <a name="search-syntax"></a>Die Syntax der Suche

Zwar kostenlosen Textsuche Standard einfache und intuitive, können Benutzer auch **Azure Datenkatalog**Suche Syntax verwenden, haben Sie mehr Kontrolle über die Suchergebnisse. **Datenkatalog Azure** -Suche unterstützt die folgenden Aktionen:

| Methode                 | Verwendung                                                                                                                                     | Beispiel                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Einfache Suche              | Einfache Suche einen oder mehrere Suchbegriffe verwenden. Ergebnisse werden alle Elemente, die in einer beliebigen Eigenschaft mit einem oder mehreren von den angegebenen Bedingungen entsprechen. | Verkaufsdaten                                                |
| Bereichsdefinition Eigenschaft          | Datenquellen, die der Suchbegriff mit der angegebenen Eigenschaft verglichen wird, nur zurück                                                   | Name: Finanzen                                              |
| Boolesche Operatoren         | Erweitern Sie oder schränken Sie ein, eine Suche mit booleschen Vorgänge                                                                                     | Finanzen nicht im Unternehmen                                     |
| Gruppieren von mit Klammer | Mithilfe von Klammern Gruppe Teile der Abfrage um logische Isolation, vor allem in Verbindung mit booleschen Operatoren zu erzielen.              | Name: Finanzen und (Kategorien: Q1 oder Kategorien: Q2) |
| Vergleichsoperatoren      | Verwenden Sie Vergleiche anderen Kriteriums als für Eigenschaften mit numerischen und Datum-Datentypen                                                | ModifiedTime > "11/05/2014"                                 |

Weitere Informationen zum **Azure Datenkatalog** suchen finden Sie unter [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Drücken Sie die Hervorhebung
Beim Anzeigen von Suchergebnissen, werden alle angezeigten Eigenschaften, die der angegebenen Suchbegriffe – wie etwa die Anlage Data source Name, Beschreibung und Tags – entsprechen hervorgehoben um, damit es einfacher zu erkennen, warum eine Anlage angegebenen Daten von einer angegebenen Suche zurückgegeben wurde.

> [AZURE.NOTE] Benutzer können Treffer Hervorhebung deaktivieren, falls gewünscht, verwenden den Schalter "Hervorheben" im **Datenkatalog Azure** -Portal, aktivieren.

Beim Anzeigen von Suchergebnissen immer offensichtlich möglicherweise nicht, warum eine Anlage Daten enthalten, ist es selbst bei Treffer hervorheben aktiviert. Da alle Eigenschaften standardmäßig durchsucht werden, kann eine Anlage Daten aufgrund einer Übereinstimmung auf eine Spalte-Level-Eigenschaft zurückgegeben werden. Und da registrierten Datenbestände über eigene Kategorien und Beschreibungen Anmerkungen mehrere Benutzer hinzufügen, möglicherweise nicht alle Metadaten in der Liste der Suchergebnisse angezeigt werden.

In der Standard-Ansicht "Nebeneinander", jeder Kachel in den Suchergebnissen angezeigt wird ein Symbol "Ansicht Suchbegriff entspricht", der dem Benutzer, um schnell die Anzahl der Treffer und die Standortinformationen anzuzeigen und zu springen, falls gewünscht ermöglicht enthalten.

 ![Drücken Sie die Hervorhebung und Suchergebnisse im Datenkatalog Azure-portal](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Zusammenfassung
Eine Datenquelle mit **Azure Datenkatalog** registrieren, sind die Datenquelle leichter zu ermitteln und zu verstehen, indem Sie strukturelle und beschreibenden Metadaten aus der Datenquelle in der Katalog-Dienst kopieren. Nachdem eine Datenquelle registriert wurde, können Benutzer mithilfe von Filtern und Suchen von innerhalb des Portals **Azure Datenkatalog** ermitteln.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Azure Datenkatalog](data-catalog-get-started.md) Lernprogramm schrittweise ausführliche Informationen zum Ermitteln von Datenquellen.
