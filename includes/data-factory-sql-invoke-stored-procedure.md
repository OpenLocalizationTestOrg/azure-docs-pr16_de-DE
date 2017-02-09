## <a name="invoking-stored-procedure-for-sql-sink"></a>Aufrufen der gespeicherten Prozedur für SQL ignorieren

Beim Kopieren von Daten in SQL Server oder Azure SQL-SQL Server-Datenbank angegeben ein Benutzers gespeicherte Prozedur konfiguriert und mit zusätzlichen Parametern aufgerufen werden konnte. 

Wenn integrierten kopieren Verfahren nicht die Zweck erfüllen, kann eine gespeicherte Prozedur genutzt werden. Dies ist in der Regel genutzt, bei der Verarbeitung von zusätzliche (Zusammenführen von Spalten, suchen Sie zusätzliche Werte einfügen in mehreren Tabellen...) muss vor dem endgültigen Einfügen der Quelldaten in die Zieltabelle erfolgen. 

Sie können eine gespeicherte Prozedur Wahl aufrufen. Das folgende Beispiel zeigt, wie Sie eine gespeicherte Prozedur zum Führen Sie eine einfache Einfügemarke in einer Tabelle in der Datenbank. 

**Die Ausgabe dataset**

In diesem Beispiel ist Typ auf festgelegt: SqlServerTable. Legen Sie dafür den AzureSqlTable zur Verwendung mit einer SQL Azure-Datenbank. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Definieren Sie im Abschnitt SqlSink wie folgt in kopieren Aktivität JSON aus. Wenn Sie eine gespeicherte Prozedur beim Einfügen von Daten anrufen möchten, werden sowohl SqlWriterStoredProcedureName und SqlWriterTableType Eigenschaften benötigt.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

Definieren Sie in der Datenbank die gespeicherte Prozedur mit demselben Namen als SqlWriterStoredProcedureName ein. Ganzer eingegebenen Daten aus der angegebenen Quelle und Einfügen in der Ausgabetabelle. Beachten Sie, dass der Parametername der gespeicherten Prozedur identisch mit der Tabellenname in Tabelle JSON-Datei definiert sein soll.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

Definieren Sie in der Datenbank den Tabellentyp mit demselben Namen als SqlWriterTableType ein. Beachten Sie, dass das Schema des Tabellentyps das Schema der Eingabedaten zurückgegebene identisch sein soll.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Das Feature gespeicherte Prozedur nutzt [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).