## <a name="repeatability-during-copy"></a>Wiederholbarkeit während kopieren

Beim Kopieren von Daten aus und relationalen speichern, müssen Sie zu Wiederholbarkeit beachten um unbeabsichtigte Ergebnisse zu vermeiden. 

Ein Segment kann automatisch in Azure Data Factory gemäß der angegebenen "Wiederholen" Richtlinie erneut ausgeführt werden. Es empfiehlt sich, dass Sie eine Richtlinie "Wiederholen", um vorübergehende Fehler vorzubeugen einrichten. Wiederholbarkeit ist daher ein wichtiger Aspekt beim Verschieben von Daten erledigen. 

**Als Quelle:**

> [AZURE.NOTE] In den folgenden Beispielen für SQL Azure sind aber gelten für alle Datenspeicher, die rechteckige Datasets unterstützt. Möglicherweise müssen Sie den **Typ** der Quell- und die Eigenschaft **Abfrage** anpassen (zum Beispiel: Abfrage statt SqlReaderQuery) für die Daten zu speichern.   

Beim Lesen aus relationalen speichern, sollten Sie normalerweise, lesen Sie nur die Daten, die diesem Segment entspricht. Eine Möglichkeit, hierzu wäre Azure Data Factory mithilfe der verfügbaren Variablen WindowStart und WindowEnd. Informationen Sie zu der Variablen und Funktionen in Azure Data Factory hier im Artikel [Planung und Ausführung](../articles/data-factory/data-factory-scheduling-and-execution.md) . Beispiel: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Diese Abfrage liest Daten aus 'MyTable', der im Bereich Dauer Segment liegt. Dieses Verhalten wird erneut ausführen dieses Segments auch immer sichergestellt werden. 

In anderen Fällen möchten Sie möglicherweise die gesamte Tabelle lesen (Nehmen Sie an für einmaliges verschieben nur) und der SqlReaderQuery kann wie folgt definiert:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
