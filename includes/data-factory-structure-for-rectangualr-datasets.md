## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Angeben der Strukturdefinition für rechteckige datasets
Im Abschnitt Struktur in die Datasets JSON ist ein **Optionaler** Abschnitt für rechteckige Tabellen (mit Zeilen und Spalten) und eine Auflistung von Spalten für die Tabelle enthält. Verwenden Sie im Abschnitt Struktur für entweder mit Typinformationen für Konvertieren des Datentyps oder spaltenzuordnungen ausführen. In den folgenden Abschnitten werden diese Features ausführlich beschrieben. 

Jede Spalte enthält die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Namen | Name der Spalte. | Ja |
| Typ | Der Datentyp der Spalte. Finden Sie unter Typ Konvertierungen unten im Abschnitt Weitere details entschieden wird, wann Sie Informationen zum angeben sollten | Nein |
| Gebietsschema | .NET basiert Kultur beim Typ angegeben und .NET Typ Datetime oder Datetimeoffset verwendet werden soll. Standardwert ist "En-us".  | Nein |
| Format | Formatieren Sie die Zeichenfolge beim Typ angegeben und .NET Typ Datetime oder Datetimeoffset verwendet werden soll. | Nein |

Das folgende Beispiel zeigt den Struktur Abschnitt JSON für eine Tabelle mit drei Spalten Benutzer-ID, Name und Lastlogindate aus.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Verwenden Sie die folgenden Richtlinien für den Einsatz von "" Strukturinformationen aufzunehmen und was im Abschnitt **Struktur** enthalten sein soll.

- Führen Sie **strukturierte Datenquellen** , in denen gespeichert Daten Schema und Typ zusammen mit den Daten selbst (Datenquellen wie SQL Server, Oracle, Azure Table usw.) ist, sollten Sie im Abschnitt "Struktur" angeben, nur, wenn Sie möchten Spalte Zuordnung von bestimmter Quellspalten auf bestimmte Spalten in Empfänger und deren Namen stimmen nicht überein (Siehe Details in der Spalte Zuordnungsabschnitt unten). 

    Wie zuvor erwähnt, ist die Informationen im Abschnitt "Struktur" optional. Für strukturierte Datenquellen, geben Sie Informationen ist bereits verfügbar als Bestandteil der Dataset Definition im Datenspeicher, damit Sie sollten nicht Typinformationen enthalten beim führen Sie im Abschnitt "Struktur" aufnehmen.
- **Für das Schema auf gelesen Datenquellen (insbesondere Azure Blob)** , die Sie auswählen können, um Daten zu speichern, ohne dass alle Schema, oder geben Informationen mit den Daten gespeichert. Für diese Typen von Datenquellen sollten Sie in folgenden Fällen 2 "Struktur" einbeziehen:
    - Führen Sie die Spalte Zuordnung werden soll.
    - Wenn das Dataset einer Quelle in einer Aktivität kopieren ist, können Sie angeben, geben Sie Informationen in "Struktur" und Daten Factory wird diese Informationen für die Konvertierung zu systemeigenen Typen für den Empfänger verwendet. [Verschieben von Daten zwischen Azure Blob](../articles/data-factory/data-factory-azure-blob-connector.md) -Artikel für Weitere Informationen finden Sie unter.

### <a name="supported-net-based-types"></a>Unterstützt. Netz-basierte Typen 
Daten Factory unterstützt die folgenden CLS kompatiblen .NET Framework-basierte Typwerte für die Bereitstellung von Informationen in "Struktur" für das Schema auf gelesen Datenquellen wie Azure Blob.

- Int16
- Int32 
- Int64
- Einzelne
- Double
- Dezimalzahl
- Byte]
- Bool
- Zeichenfolge 
- GUID
- "DateTime"
- DateTimeOffset
- TimeSpan 

Für "DateTime" & Datetimeoffset können Sie optional auch "Kultur" und "format" eine Zeichenfolge, die zu erleichtern, analysieren Ihre benutzerdefinierten Datetime-Zeichenfolge angeben. Finden Sie im Beispiel unten Typumwandlung.

