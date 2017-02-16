## <a name="what-is-the-table-service"></a>Was ist der Tabelle Service

Der Speicherdienst Azure Tabelle speichert große Mengen von strukturierten Daten. Der Dienst ist, dass die akzeptiert NoSQL Datenspeicher Anrufe von innerhalb und außerhalb der Azure Cloud authentifiziert. Azure-Tabellen eignen sich zum Speichern von strukturierten, nicht relationalen Daten. Gemeinsame Verwendung des Diensts Tabelle umfassen:

-   Speichern von TB strukturierte Daten Bedienung von Webanwendungen-Skala
-   Speichern von Datasets, die nicht erforderlich komplexer Verknüpfungen, Fremdschlüssel oder gespeicherten Prozeduren und für einen schnellen Zugriff denormalisiert werden können
-   Schnell Abfragen von Daten mithilfe eines gruppierten Indexes
-   Zugreifen auf Daten mithilfe der OData-Protokoll und LINQ-Abfragen mit WCF Data Service .NET Bibliotheken

Sie können den Dienst Tabelle zum Speichern und Abfrage große Mengen strukturierten, nicht relationaler Daten und Tabellen werden bei Bedarf zunehmender skalieren.

## <a name="table-service-concepts"></a>Tabelle Dienst Konzepte

Der Dienst Tabelle enthält die folgenden Komponenten:

![Tabelle1][Table1]

-   **URL-Format:** Code Adressen Tabellen in einem Konto mithilfe dieses Adressformat an:   
    http://`<storage account>`.table.core.windows.net/`<table>`  
      
    Sie können Azure Tabellen verwenden diese Adresse direkt mit dem OData-Protokoll adressieren. Weitere Informationen finden Sie unter [OData.org][]

-   **Speicherkonto:** Alle Zugriff auf Azure-Speicher erfolgt über ein Speicherkonto. Details zum Konto Speicherkapazität finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md) .

-   **Tabelle**: eine Tabelle ist eine Auflistung von Elementen. Tabellen erzwingen nicht über ein Schema auf Personen, was bedeutet, dass eine einzelne Tabelle Elemente enthalten kann, die unterschiedliche Optionssätze Eigenschaften aufweisen. Die Anzahl der Tabellen, die ein Speicherkonto enthalten kann ist nur durch das Konto Kapazität Speichergrenzwert begrenzt.

-   **Entität**: eine Entität ist eine Reihe von Eigenschaften, die ähnliche Zeile in einer Datenbank. Eine Entität kann bis zu 1 MB groß sein.

-   **Eigenschaften**: eine Eigenschaft ist ein paar Wert für Name. Jede Entität kann bis zu 252 Eigenschaften zum Speichern von Daten enthalten. Jede Entität besitzt auch 3 Systemeigenschaften, die Partitionsschlüssel, einen Zeilenschlüssel und einen Zeitstempel angeben. Personen mit der gleichen Partitionsschlüssel können schneller abgefragt und in atomare Vorgänge eingefügt/aktualisiert werden. Einer Entität Zeilenschlüssel ist eindeutigen Bezeichner innerhalb einer Partition.

Details zur Benennung von Tabellen und Eigenschaften finden Sie unter [Grundlegendes zu den Dienst Daten Tabellenmodell](https://msdn.microsoft.com/library/azure/dd179338.aspx).
  
  [Table1]: ./media/storage-table-concepts-include/table1.png
  [OData.org]: http://www.odata.org/
