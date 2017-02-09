## <a name="repeatability-during-copy"></a>Wiederholbarkeit während kopieren

Beim Kopieren von Daten auf Azure SQL-SQL Server von anderen Daten speichert muss eine zu Wiederholbarkeit beachten um unbeabsichtigte Ergebnisse zu vermeiden. 

Beim Kopieren von Daten in Azure SQL-SQL Server-Datenbank wird kopieren Aktivität standardmäßig ANFÜGEN Datenmenge der Empfänger Tabelle standardmäßig. Beispielsweise beim Kopieren von Daten aus einer CSV (durch Trennzeichen getrennte Werte) Datei Datenquelle mit zwei Datensätzen in Azure SQL-SQL Server-Datenbank, ist dies die Tabelle aussieht:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Angenommen Sie, Sie Fehler in Quelldatei gefunden, und die Menge der unten Glas von 2 bis 4 in der Quelldatei aktualisiert. Wenn Sie das Segment Daten für den betreffenden Zeitraum erneut ausführen, finden Sie zwei neue Datensätze mit Azure SQL-SQL Server-Datenbank angefügt. Die nachstehend wird davon ausgegangen, keine der Spalten in der Tabelle den Primärschlüssel haben.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Um dies zu vermeiden, müssen Sie UPSERT Semantik angeben, indem Sie eine der Nutzung der unter 2 Verfahren unter angegeben ist.

> [AZURE.NOTE] Ein Segment kann automatisch in Azure Data Factory gemäß der angegebenen "Wiederholen" Richtlinie erneut ausgeführt werden.

### <a name="mechanism-1"></a>Verfahren 1

Sie können nutzen **SqlWriterCleanupScript** Eigenschaft Aufräumen Aktion zuerst ausgeführt wird, wenn ein Segment ausgeführt wird. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Das Skript zum Aufräumen ausgeführt ersten während kopieren für ein angegebenes Segment werden die Daten aus der SQL-Tabelle, die diesem Segment entspricht gelöscht würden. Die Aktivität wird anschließend die Daten in der SQL-Tabelle einfügen. 

Wenn das Segment ist jetzt erneut ausführen, können Sie ermitteln, dass die Menge aktualisiert wird, als gewünscht.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Nehmen Sie an, dass der flachen Wasch-Datensatz aus der ursprünglichen Csv entfernt wird. Klicken Sie dann erneut das Segment ausführen erzeugt das folgende Ergebnis: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Keine neuen hatten durchgeführt werden. Die Aktivität kopieren ist das Skript zum Aufräumen, um die entsprechenden Daten für die Segment zu löschen. Und dann die Eingabe von im CSV-Format lesen (die dann nur 1 Datensatz enthalten), und es in die Tabelle eingefügt. 

### <a name="mechanism-2"></a>Verfahren 2
> [AZURE.IMPORTANT] SliceIdentifierColumnName wird zu diesem Zeitpunkt nicht Azure SQL-Data Warehouse unterstützt. 

Ein weiteres Verfahren erzielen Wiederholbarkeit ist eine dedizierte Spalte (**SliceIdentifierColumnName**) in der Zielliste Tabelle Probleme. In dieser Spalte würde von Azure Daten Factory verwendet werden, um sicherzustellen, dass die Quell- und Zielliste stets synchronisiert. Dieser Ansatz funktioniert, wenn es Flexibilität in ändern oder das Ziel SQL-Tabelle Schema definiert ist. 

In dieser Spalte würde von Azure Daten Factory für Wiederholbarkeit Zwecke verwendet werden, und im Prozess Azure Data Factory wird nicht Schema Ändern der Tabelle. Die Möglichkeit, diese Vorgehensweise zu verwenden:

1.  Definieren Sie eine Spalte vom Typ binäre (32) in das Ziel SQL-Tabelle. Keine Einschränkungen anhand dieser Spalte sollten. Wir nennen Sie diese Spalte als 'ColumnForADFuseOnly' für dieses Beispiel.
2.  Verwenden sie in der Kopie Aktivität wie folgt:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure Data Factory füllen Sie diese Spalte gemäß deren Stellen Sie sicher, dass die Quell- und Zielliste synchronisierten bleiben müssen. Die Werte in dieser Spalte sollte innerhalb dieses vom Benutzer nicht verwendet werden. 

Verfahren 1 ähnlich, Aktivität kopieren, werden automatisch bereinigen Sie zuerst die Daten für das angegebene Segment vom Ziel SQL-Tabelle, und führen Sie dann die Aktivität kopieren normal, um die Daten aus der Quelle zum Ziel für die Segment einfügen. 
