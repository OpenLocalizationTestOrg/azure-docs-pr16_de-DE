 <properties
    pageTitle="So Batchverarbeitung verwenden, um die Leistung der Anwendung Azure SQL-Datenbank zu verbessern."
    description="Das Thema enthält beweisen die Batchverarbeitung Datenbankvorgänge erheblich Imroves der Geschwindigkeit und Skalierbarkeit Anwendungen Azure SQL-Datenbank. Obwohl diese Batchverarbeitung Techniken für jede SQL Server-Datenbank arbeiten, ist der Fokus des Artikels auf Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>So Batchverarbeitung verwenden, um die Leistung der SQL-Datenbank-Anwendung zu verbessern.

Batchvorgängen mit Azure SQL-Datenbank erheblich verbessert die Leistung und Skalierbarkeit Anwendungen. Um die Vorteile zu verstehen, behandelt im erste Teil dieses Artikels einige Stichprobe Testergebnisse, die sequenzielle und gespeicherten Anfragen in einer SQL-Datenbank zu vergleichen. Der Rest des Artikels zeigt die Techniken, Szenarien und Aspekte, damit Sie erfolgreich in Ihrer Azure Applications Batchverarbeitung verwenden können.

**Autoren**: Jason größere, Silvano Coriani, Trent Swanson (vollständige Maßstab 180 Inc.)

**Bearbeiter**: Conor Cunningham, Michael Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Warum wichtig für SQL-Datenbank Batchverarbeitung?
Anrufe an einen Remotedienst Batchverarbeitung ist eine bekannte Strategie zum Erhöhen der Leistung und Skalierbarkeit. Es sind Verarbeitungskosten aus, um alle Interaktionen mit einen Remotedienst, z. B. Serialisierung, Netzwerk-Transfer und Deserialisierung fest. Verpacken viele separate Transaktionen in einem einzigen Stapel minimiert diese Kosten.

In diesem Dokument möchten untersuchen Batchverarbeitung Strategien und Szenarien verschiedene SQL-Datenbank. Obwohl diese Strategien auch für lokale Applikationen wichtig, die SQL Server verwenden sind, gibt es verschiedene Gründe für die Verwendung der Batchverarbeitung für SQL-Datenbank hervorheben:

- Es gibt potenziell größer Netzwerk Wartezeit für den Zugriff auf SQL-Datenbank, insbesondere dann, wenn Sie außerhalb der gleichen Datencenter Microsoft Azure SQL-Datenbank aus zugreifen.
- Die mandantenfähigen Merkmale der SQL-Datenbank bedeutet, dass die Effizienz der Daten Access Layer entspricht der gesamten Skalierbarkeit der Datenbank. SQL-Datenbank müssen verhindern, dass alle Mandanten/Einzelbenutzer stark auszulasten Datenbankressourcen Schaden einer anderen Mandanten. Reaktion Einsatz mehr als vordefinierte Kontingente können SQL-Datenbank Durchsatz verringern oder Antworten mit begrenzungsebene Ausnahmen. Effizientes, z. B. Batchverarbeitung, ermöglicht Ihnen mehr Arbeit auf SQL-Datenbank, bevor Sie diese Grenzwerte erreicht. 
- Batchverarbeitung ist auch geeignet, um Architekturen, die mehrere Datenbanken (Sharding) verwenden. Die Effizienz für die Interaktion mit der Datenbank ein Stück ist immer noch ein wichtiger Faktor in der gesamten Skalierbarkeit aus. 

Einer der Vorteile der Verwendung von SQL-Datenbank ist, dass Sie nicht die Server verwalten, die die Datenbank zu hosten. Diese verwalteten Infrastruktur bedeutet jedoch auch, dass Sie die Datenbank Optimierungen anders anzustellen haben. Sie können nicht mehr zur Verbesserung der Datenbank Hardware oder Netzwerk-Infrastruktur suchen. Microsoft Azure steuert dieser Umgebungen. Im Hauptfenster Bereich, den Sie steuern können, ist Interaktion zwischen Ihrer Anwendung und SQL-Datenbank. Batchverarbeitung ist eine der folgenden Optimierungen an. 

Im erste Teil des Papiers untersucht verschiedene Batchverarbeitung Techniken für .NET Applications, die SQL-Datenbank zu verwenden. Die letzten beiden Abschnitte hervorgehen Batchverarbeitung Richtlinien und Szenarien.

## <a name="batching-strategies"></a>Strategien Batchverarbeitung

### <a name="note-about-timing-results-in-this-topic"></a>Beachten Sie Anzeigedauer Ergebnisse in diesem Thema
>[AZURE.NOTE] Ergebnisse sind nicht Benchmarks, aber Anzeigen der **relativen Leistung**vorgesehen sind. Anzeigedauer basieren auf einen Mittelwert von mindestens 10 Test ausgeführt. Vorgänge werden in eine leere Tabelle einfügen. Diese Tests waren gemessen Pre-V12 und diese entsprechen nicht unbedingt auf den Durchsatz, die in einer V12-Datenbank mithilfe der neuen [Ebenen Dienst](sql-database-service-tiers.md)auftreten können. Der relative Vorteil der Batchverarbeitung Technik sollte ähnlich sein.

### <a name="transactions"></a>Transaktionen.
Anscheinend ungewöhnliches einen Überblick über die Batchverarbeitung mit einer Diskussion über Transaktionen beginnen soll. Jedoch die Verwendung von clientseitige Transaktionen wirkt sich raffinierten serverseitigen Batchverarbeitung aus, die Leistung zu verbessern. Und Transaktionen, damit sie eine schnelle Möglichkeit zum Verbessern der Leistung von sequenziellen Vorgängen bieten mit nur wenigen Codezeilen, hinzugefügt werden.

Betrachten Sie den folgenden C#-Code, der eine Abfolge von einfügen enthält und aktualisieren Sie Vorgänge in einer einfachen Tabelle.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Der folgende Code ADO.NET führt sequenziell dieser Vorgänge.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Die beste Methode zum Optimieren von diesem Code besteht darin eine Form von clientseitige Batchverarbeitung dieser Anrufe implementieren. Es gibt jedoch eine einfache Möglichkeit, um die Leistung dieses Codes erhöhen, indem Sie einfach die Reihenfolge der Aufrufe in eine Transaktion einbinden. Hier wird der gleiche Code, der eine Transaktion verwendet.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

Transaktionen sind tatsächlich in beiden Beispielen verwendet werden. Im ersten Beispiel wird jede einzelne Anruf eine implizite Transaktion. Im zweiten Beispiel umschließt eine explizite Transaktion alle Anrufe an. Pro der für die- [Schreiben während der Transaktionslog](https://msdn.microsoft.com/library/ms186259.aspx)-Dokumentation werden Protokolldatensätze auf den Datenträger geleert, wenn die Transaktion übermittelt wird. Damit kann einschließlich weiterer Anrufe in einer Transaktion, indem Sie das Schreiben in das Transaktionsprotokoll verzögern bis Transaktion. Aktivieren Sie für die schreibt in des Servers Transaktionsprotokoll Batchverarbeitung.

Die folgende Tabelle enthält einige Ad-hoc-Testergebnisse. Die Tests durchgeführt vom gleichen sequenziellen eingefügte mit und ohne Transaktionen. Für weitere Perspektive ist der erste Satz von Tests Remote von einem Laptop aus in die Microsoft Azure-Datenbank. Die zweite Reihe von Tests ist in einen Cloud-Dienst und die Datenbank, die beide innerhalb der gleichen Microsoft Azure Datencenter (Westen USA) befand. Die folgende Tabelle zeigt die Dauer in Millisekunden von sequenziellen fügt mit und ohne Transaktionen.

**Lokal in Azure**:

| Vorgänge | Keine Transaktion (ms) | Transaktion (ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000 | 128852 | 102917 |


**Azure zu Azure (demselben Datencenter)**:

| Vorgänge | Keine Transaktion (ms) | Transaktion (ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000 | 21479 | 2756 |

>[AZURE.NOTE] Die Ergebnisse sind nicht Benchmarks. Finden Sie im [Hinweis zur Anzeigedauer Ergebnisse in diesem Artikel](#note-about-timing-results-in-this-topic)aus.

Basierend auf der vorherigen Testergebnisse, verkleinert Umbrechen von einem Vorgang in einer Transaktion tatsächlich Leistung. Aber wenn Sie die Anzahl der Vorgänge in einer einzigen Transaktion erhöhen, wird die Leistung Verbesserung wird mehr markiert. Der Leistungsunterschied ist auch noch deutlicher zu sehen, wenn alle Vorgänge innerhalb der Microsoft Azure Datencenters auftreten. Die höhere Wartezeit der Verwendung von außerhalb des Datencenters Microsoft Azure SQL-Datenbank aus den Hauptdokuments Leistungsgewinn oder der Verwendung von Transaktionen.

Obwohl die Verwendung von Transaktionen Leistung zu verbessern kann, weiterhin [bewährte Verfahren für Transaktionen und Verbindungen zu beobachten](https://msdn.microsoft.com/library/ms187484.aspx). Beibehalten der Transaktions so kurz wie möglich, und die datenbankverbindung zu schließen, nachdem Sie seine Arbeit beendet hat. Die mit-Anweisung im vorherigen Beispiel wird sichergestellt, dass die Verbindung geschlossen wird, wenn der nachfolgenden Codeblock abgeschlossen ist.

Im vorherige Beispiel wird veranschaulicht, dass Sie eine lokale Transaktion ADO.NET-Code mit zwei Zeilen hinzufügen können. Transaktionen bieten eine schnelle Möglichkeit zum Steigern der Leistung des Codes, der sequenziellen einfügen, aktualisieren und Löschen von Vorgängen ist. Jedoch für die schnellste Leistung, sollten Sie den Code um nutzen, um clientseitige Batchverarbeitung, beispielsweise Parameter Tabellenwertfunktionen ändern.

Weitere Informationen zu Transaktionen in ADO.NET finden Sie unter [Lokale Transaktionen in ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Tabellenwertfunktionen Parameter
Tabellenwertfunktionen Parameter unterstützen benutzerdefinierte Tabelle als Parameter in Transact-SQL-Anweisungen, gespeicherten Prozeduren und Funktionen. Diese clientseitige Batchverarbeitung Methode können Sie mehrere Zeilen von Daten innerhalb des Parameters Tabellenwertfunktionen senden. Um Tabellenwertfunktionen Parameter verwenden zu können, müssen Sie zuerst definieren Sie einen Tabellentyp. Die folgende Transact-SQL-Anweisung erstellt eine benannte **MyTableType**Table-Datentyp.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

Code erstellen Sie eine **Datentabelle** mit den genauen denselben Namen und Typen vom Tabellentyp ein. Übergeben dieser **Datentabelle** in einem Parameter in einer Textabfrage oder remote Procedure Call gespeichert. Im folgenden Beispiel wird diese Methode:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

Im vorherigen Beispiel, fügt das **SqlCommand** -Objekt Zeilen aus einer Parameter Tabellenwertfunktionen **@TestTvp**. Das zuvor erstellte **Datentabelle** Objekt wird mit der Methode **SqlCommand.Parameters.Add** für diesen Parameter zugewiesen. Die Fügt einen Anruf erheblich Batchverarbeitung steigert die Leistung über sequenziellen eingefügt.

Zur Verbesserung der weiteren im vorherige Beispiel, verwenden Sie eine gespeicherte Prozedur anstelle eines Befehls textbasierten aus. Der folgende Transact-SQL-Befehl erstellt eine gespeicherte Prozedur, die den **SimpleTestTableType** Tabellenwertfunktionen Parameter akzeptiert.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Ändern Sie dann die Deklaration der **SqlCommand** im vorherigen Beispiel der folgende aus.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

In den meisten Fällen müssen Tabellenwertfunktionen Parameter entspricht oder eine bessere Leistung als andere Batchverarbeitung Techniken an. Tabellenwertfunktionen Parameter sind oft gewünscht, da die flexibler als die anderen Optionen. Andere Techniken, wie etwa SQL Massenkopieren, wird beispielsweise nur das Einfügen von neuen Zeilen zulassen. Aber mit Tabellenwertfunktionen Parametern können Sie Logik in der gespeicherten Prozedur verwenden, um festzustellen, welche Zeilen Aktualisierungen sind, und fügt die sind. Die Table-Datentyp kann auch um eine Spalte "Der Vorgang" enthalten, die angibt, ob die angegebene Zeile eingefügt, aktualisiert oder gelöscht werden soll, geändert werden.

Die folgende Tabelle zeigt Ad-hoc-Testergebnisse für die Verwendung von Parametern Tabellenwertfunktionen in Millisekunden an.

| Vorgänge | Lokal in Azure (ms)  | Azure demselben Datencenter (ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Die Ergebnisse sind nicht Benchmarks. Finden Sie im [Hinweis zur Anzeigedauer Ergebnisse in diesem Artikel](#note-about-timing-results-in-this-topic)aus.

Die Leistungsgewinn aus Batchverarbeitung fällt sofort ins Auge. In der vorherigen sequenziellen Test habe 1000 Vorgänge 129 Sekunden außerhalb des Datencenters und 21 Sekunden aus innerhalb des Datencenters. Aber mit Tabellenwertfunktionen Parameter 1000 Vorgänge nur 2.6 Sekunden außerhalb des Datencenters und 0,4 Sekunden innerhalb des Datencenters dauern.

Weitere Informationen zu Parametern Tabellenwertfunktionen finden Sie unter [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>Kopieren des SQL-großer Datenmengen
Kopieren des SQL-großer Datenmengen ist eine weitere Möglichkeit zum Einfügen von großer Mengen von Daten in einer Zieldatenbank. **SqlBulkCopy** -Klasse können .NET Applications Massen Einfügevorgänge ausführen. **SqlBulkCopy** ähnelt (Funktion), die Befehlszeile Tool **Bcp.exe**oder die Transact-SQL-Anweisung **Massen einfügen**. Im folgenden Code wird veranschaulicht gruppenweise kopieren die Zeilen in der Quelle **Datentabelle**, Tabelle, in die Zieltabelle in SQL Server, MyTable.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Es gibt einige Fälle, in denen Kopieren großer Datenmengen bevorzugte über Tabellenwertfunktionen Parameter aus. Finden Sie unter der Vergleichstabelle Parametertyp im Vergleich zu Massen einfügen Vorgänge unter dem Thema [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx)Tabellenwertfunktionen.

Die folgenden Ad-hoc-Testergebnisse zeigen die Leistung von Batchverarbeitung mit **SqlBulkCopy** in Millisekunden an.

| Vorgänge | Lokal in Azure (ms)  | Azure demselben Datencenter (ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Die Ergebnisse sind nicht Benchmarks. Finden Sie im [Hinweis zur Anzeigedauer Ergebnisse in diesem Artikel](#note-about-timing-results-in-this-topic)aus.

In kleinere Stapelgrößen übertroffen die Verwenden von Parametern Tabellenwertfunktionen **SqlBulkCopy** -Klasse. Jedoch ausgeführt **SqlBulkCopy** 12-31 % schneller als Parameter Tabellenwertfunktionen für die Tests von 1.000 und 10.000 Zeilen ein. Wie Tabellenwertfunktionen Parameter ist der **SqlBulkCopy** eine gute Option für gespeicherten Einfüge-, besonders, wenn Sie mit der Leistung von Vorgängen nicht zusammengefasst verglichen werden.

Weitere Informationen zum Kopieren großer Datenmengen in ADO.NET finden Sie unter [Massen Kopiervorgänge in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Mehrzeiliges parametrisierte einfügen Anweisungen
Eine Alternative für kleine Stapel ist, eine große parametrisierte einfügen-Anweisung zu erstellen, die mit mehreren Zeilen eingefügt. Im folgenden Code wird veranschaulicht, dieses Verfahren wird.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

In diesem Beispiel wird das grundlegende Konzept anzeigen soll. Ein weitere realistisches Szenario würde durchlaufen und die erforderlichen Einheiten der Abfragezeichenfolge und der Befehlsparameter gleichzeitig zu erstellen. Sie sind auf insgesamt 2100 Abfrageparameter, beschränkt, wodurch dies die Gesamtzahl der Zeilen eingeschränkt, die auf diese Weise verarbeitet werden können.

Die folgenden Ad-hoc-Testergebnisse zeigen die Leistung dieses Typs Insert-Anweisung in Millisekunden an.

| Vorgänge | Tabellenwertfunktionen Parameter (ms) | Einzelner Anweisung einfügen (ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Die Ergebnisse sind nicht Benchmarks. Finden Sie im [Hinweis zur Anzeigedauer Ergebnisse in diesem Artikel](#note-about-timing-results-in-this-topic)aus.

Dieser Ansatz kann für Stapel etwas schneller sein, die weniger als 100 Zeilen sind. Auch wenn die Verbesserung klein ist, ist diese Methode eine weitere Möglichkeit, die auch in Ihrem Anwendungsszenario bestimmten funktionieren möglicherweise aus.

### <a name="dataadapter"></a>DataAdapter
Die **DataAdapter** -Klasse können Sie ein **DataSet** -Objekt ändern, und übermitteln Sie die Änderungen wie einfügen, aktualisieren und Löschen von Vorgängen. Wenn Sie auf diese Weise **DataAdapter** verwenden, ist es wichtig zu beachten, dass separate Anrufe für jeden Vorgang distinct vorgenommen werden. Verwenden Sie die **UpdateBatchSize** -Eigenschaft auf die Anzahl der Vorgänge, die jeweils zusammengefasst werden sollen, um die Leistung zu verbessern. Weitere Informationen finden Sie unter [Stapel Vorgänge mithilfe von DataAdapters ausführen](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entität framework
Entität Framework unterstützt derzeit Batchverarbeitung nicht. Andere Entwickler in der Community haben versucht, um zu veranschaulichen, Lösungsmöglichkeiten, wie z. B. außer Kraft setzen die **SaveChanges** -Methode. Aber die Solutions sind in der Regel komplexer und mit der Anwendung und des Datenmodells angepasste. Dieses Feature Anforderung weist Entität Framework Codeplex Projekt aktuell eine Diskussionsseite. Um diese Diskussion anzeigen zu können, finden Sie unter [Entwurf Besprechungsnotizen – 2 August 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Vollständigkeit Meinung sind, es wichtig ist, über XML als Batchverarbeitung Strategie sprechen. Die Verwendung von XML hat jedoch keine über andere Methoden und mehrere Nachteile. Der Ansatz ähnelt dem Parameter Tabellenwertfunktionen, aber eine XML-Datei oder eine Zeichenfolge wird an eine gespeicherte Prozedur anstelle einer Tabelle User defined übergeben. Die gespeicherte Prozedur analysiert die Befehle in der gespeicherten Prozedur.

Es gibt mehrere Nachteile bei dieser Vorgehensweise:

- Arbeiten mit XML kann schwierig sein, und.
- Analysieren die XML-Daten in der Datenbank kann die CPU-Auslastung sein.
- In den meisten Fällen ist diese Methode langsamer als Parameter Tabellenwertfunktionen.

Aus diesen Gründen empfiehlt sich die Verwendung von XML für Stapel Abfragen nicht.

## <a name="batching-considerations"></a>Batchverarbeitung Aspekte
Die folgenden Abschnitte enthalten weitere Unterstützung für die Verwendung der Batchverarbeitung in Clientanwendungen SQL-Datenbank.

### <a name="tradeoffs"></a>Kompromisse
Abhängig von der Architektur kann Batchverarbeitung einen Kompromiss zwischen Leistung und Stabilität (Variablen) haben. Angenommen Sie, das Szenario, in dem Ihre Rolle unerwartet fällt aus. Wenn Sie eine Zeile mit Daten verlieren, ist die Auswirkung kleiner als die Wirkung einen großen Stapel nicht übermittelte Zeilen zu verlieren. Es besteht das Risiko größer, wenn Sie Zeilen Puffer, bevor sie die Datenbank in einem angegebenen Zeitfenster gesendet werden.

Da diese Abwägung Bewerten der Art der Vorgänge Sie Stapel. Stapelverarbeitung stärker voranzutreiben (größere Blattnamen und längere Zeit Zeitfenster) mit den Daten, die weniger wichtig ist.

### <a name="batch-size"></a>Stapelgröße
In unseren Tests wurde in der Regel kein Vorteil der Bruchfestigkeit großer Stapel in kleinere Einheiten. Tatsächlich geführt hat dieses Teilbereich häufig Leistung als einen einzelnen großen Stapel senden ein. Betrachten Sie beispielsweise ein Szenario, in dem Sie 1000 Zeilen einfügen möchten. Die folgende Tabelle zeigt, wie lange dauert Tabellenwertfunktionen Parameter verwenden Sie zum Einfügen von 1000 Zeilen beim in kleineren Stapeln unterteilt.

| Stapelgröße | Iterationen | Tabellenwertfunktionen Parameter (ms) |
| -------- | --- | --- |
| 1000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Die Ergebnisse sind nicht Benchmarks. Finden Sie im [Hinweis zur Anzeigedauer Ergebnisse in diesem Artikel](#note-about-timing-results-in-this-topic)aus.

Sie können sehen, dass die optimale Leistung für 1000 Zeilen besteht darin, sie alle gleichzeitig zu übermitteln. In anderen Tests (hier nicht dargestellt) ist eine kleine Performance-Steigerung in einem Stapel 10000 Zeile in zwei Stapel von 5000 Teile aufzuteilen. Aber das Schema für diese Tests ist relativ einfach, sodass Sie Tests, auf dem bestimmte Daten und Stapelgrößen durchführen sollten, um diese Ergebnisse zu überprüfen.

Ein weiterer Faktor zu berücksichtigen ist, dass das gesamte Blatt zu groß wird, SQL-Datenbank möglicherweise schränken Sie ablehnen, um den Stapel abzuschließen. Die besten Ergebnisse erzielen Sie testen Sie Ihr spezielles Szenario, um festzustellen, ob eine Stapelgröße eignet vorhanden ist. Stellen Sie die Stapelgröße konfigurierbare zur Laufzeit schnelle Anpassungen basierend auf der Leistung oder Fehler zu aktivieren.

Schließlich gegeneinander Wägen Sie die Größe des Stapels mit Risiko im Zusammenhang mit Batchverarbeitung ein ab. Wenn vorübergehende Fehler vorliegen, oder die Rolle schlägt fehl, erwägen Sie die folgen den Vorgang wiederholen oder der Verlust von Daten in den Stapel.

### <a name="parallel-processing"></a>Parallele Verarbeitung
Was passiert, wenn Sie erstellen den Ansatz der Stapel zu verkleinern, aber verwendet mehrere Threads zum Ausführen der Arbeit? Erneut gezeigt unserer Tests, dass mehrere kleinere multithreaded Blätter in der Regel als einem einzigen größeren Stapel Verschlechterung durchgeführt. Der folgende Test versucht, 1000 Zeilen in ein oder mehrere parallele Stapel einfügen. Dieser Test zeigt an, wie viele zusätzliche gleichzeitigen Blattnamen tatsächlich Leistung geringere.

| Stapelgröße [Iterationen] | Zwei Threads (ms) | Vier Threads (ms) | Sechs Threads (ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Die Ergebnisse sind nicht Benchmarks. Finden Sie im [Hinweis zur Anzeigedauer Ergebnisse in diesem Artikel](#note-about-timing-results-in-this-topic)aus.

Es gibt mehrere mögliche Gründe für die Leistungsabfall aufgrund Parallelism aus:

- Es gibt mehrere Gleichzeitiges Netzwerk Anrufe statt eine ein.
- Mehrere Operationen auf eine einzelne Tabelle können Konflikte und Blockieren führen.
- Es gibt Aufwand zugeordnet multithreading.
- Die Ausgaben für mehrere Verbindungen öffnen aufwiegt die Vorteile der parallelen Verarbeitung.

Wenn Sie andere Tabellen oder Datenbanken Zielgruppen, ist es möglich, finden Sie einige Leistung bei dieser Vorgehensweise zu erhalten. Datenbank Sharding oder Verbünden wäre ein Szenario für diesen Ansatz. Sharding verwendet mehrere Datenbanken und andere Daten in die einzelnen Datenbanken weiterleitet. Wenn jede kleine Stapel zu einer anderen Datenbank vertraut ist, kann anschließend ausführen, die die Vorgänge parallel effizienter sein. Die Leistung Gain ist jedoch nicht signifikant genug ist, verwenden Sie als Grundlage für eine Entscheidung zur Verwendung der Datenbank Sharding in Ihre Lösung.

In einigen Entwürfen kann parallele Ausführung von kleineren Stapeln verbesserten Durchsatz von Besprechungsanfragen in einem System Auslastung führen. In diesem Fall, obwohl sie einen einzigen größeren Stapel Verarbeitungszeit schneller ist, möglicherweise mehrere Blattnamen parallel Verarbeitung effizienter sein.

Wenn Sie parallele Ausführung verwenden, sollten Sie die maximale Anzahl von Arbeitsthreads steuern. Eine kleinere Zahl möglicherweise weniger Konflikte und eine schnellere Ausführung. Erwägen Sie auch die zusätzliche Last, die diese auf die Zieldatenbank sowohl in Verbindungen und Transaktionen platziert wurde.

### <a name="related-performance-factors"></a>Verwandte Leistungsfaktoren
Typische Anleitungen Datenbank-Performance gilt auch für Batchverarbeitung. Legen Sie beispielsweise für Tabellen mit großen Primärschlüssel oder viele nicht gruppierte Indizes die Leistung reduziert wird.

Wenn eine gespeicherte Prozedur Tabellenwertfunktionen Parameter verwenden möchten, können Sie den Befehl **SET NOCOUNT ON** am Anfang der Prozedur verwenden. Diese Anweisung unterdrückt die Rückgabe von der Anzahl der betroffenen Zeilen in der Prozedur an. Jedoch in unseren Tests, die Verwendung von **SET NOCOUNT ON** entweder hatte keine Auswirkung oder geringere Leistung. Die Test gespeicherte Prozedur wurde mit einer einzelnen **Einfügen** einfachen Befehl aus dem Parameter Tabellenwertfunktionen. Es ist möglich, dass diese Anweisung komplexere gespeicherte Prozeduren nutzen möchten. Aber nicht wird davon ausgegangen, dass die Leistung **NOCOUNT aktivieren** automatisch zur gespeicherten Prozedur hinzufügen verbessert werden. Um den Effekt zu verstehen, testen Sie Ihre gespeicherte Prozedur mit und ohne die **NOCOUNT aktivieren** -Anweisung.

## <a name="batching-scenarios"></a>Batchverarbeitung Szenarien
In den folgenden Abschnitten wird beschrieben, wie Parameter Tabellenwertfunktionen in drei Anwendungsszenarien verwenden. Im erste Szenario wird gezeigt, wie Puffer und Batchverarbeitung zusammenarbeiten können. Das zweite Szenario verbessert die Leistung von Master / Detail-Operationen ausführen, die eine einzelne gespeicherte Prozedur Anruf. Das letzte Szenario wird gezeigt, wie Parameter Tabellenwertfunktionen einen Vorgang "UPSERT" verwenden.

### <a name="buffering"></a>Pufferung
Obwohl es einige Szenarien, die offensichtlich Kandidat gibt für die Batchverarbeitung sind, sind viele Szenarien, die durch die verzögerte Verarbeitung Batchverarbeitung nutzen können. Verzögerte Verarbeitung führt jedoch auch ein höheres Risiko, dass die Daten verloren gehen, wenn ein unerwarteter Fehler. Es ist wichtig, dieses Risiko verstehen und die folgen zu berücksichtigen sind.

Angenommen Sie, eine Anwendung, die den Navigationsbereich Verlauf jeder Benutzer nachverfolgt. Klicken Sie auf jeder Seite Anforderung konnte die Anwendung eine Datenbank aufrufen, um die Ansicht des Benutzers aufzeichnen machen. Aber bessere Leistung und Skalierbarkeit durch Puffer von Benutzeraktivitäten Navigation, und klicken Sie dann an die Datenbank in Stapeln senden diese Daten erreicht werden können. Sie können das Datenbank aktualisieren von der verstrichenen Zeit und/oder Puffergröße auslösen. Beispielsweise konnte eine Regel angeben, dass der Stapel nach 20 Sekunden oder wenn Puffer 1000 Elemente erreicht verarbeitet werden sollen.

Im folgenden Code wird verwendet [Reaktivieren Extensions - Rx](https://msdn.microsoft.com/data/gg577609) zwischengespeicherten durch eine Überwachung Klasse ausgelöste Ereignisse zu verarbeiten. Wenn Puffer voll oder Timeout erreicht ist, der Stapel von Benutzerdaten in die Datenbank mit einem Parameter Tabellenwertfunktionen gesendet wird.

Die folgende NavHistoryData Klasse Modelle die Navigation Benutzerdetails. Sie enthält grundlegenden Informationen, wie die Benutzer-ID, die URL zugegriffen und die Uhrzeit von Access.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

Die NavHistoryDataMonitor-Klasse ist für die Navigation Benutzerdaten in der Datenbank Pufferung verantwortlich. Er enthält eine Methode, RecordUserNavigationEntry, die durch das Auslösen eines Ereignisses **OnAdded** reagiert. Der folgende Code zeigt die Konstruktorlogik, die Rx wird verwendet, um eine sichtbare Auflistung basierend auf das Ereignis zu erstellen. Klicken Sie dann abonniert diese sichtbare Sammlung mit der Methode Puffer. Die Überladung gibt an, dass der Puffer alle 20 Sekunden oder 1000 Einträge gesendet werden soll.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Der Ereignishandler wandelt alle zwischengespeicherten Elemente in ein anderes Tabellenwertfunktionen und übergibt diese Art an eine gespeicherte Prozedur, die den Stapel verarbeitet. Der folgende Code zeigt die vollständige Definition für die NavHistoryDataEventArgs und die NavHistoryDataMonitor Klassen.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Um diese Zwischenspeichern Klasse verwenden zu können, erstellt die Anwendung ein statisches NavHistoryDataMonitor Objekt. Jedes Mal ein Benutzer auf einer Seite greift auf Ruft die Anwendung die Methode NavHistoryDataMonitor.RecordUserNavigationEntry. Die Zwischenspeichern Logik verläuft zu erledigen diese Einträge an die Datenbank in Stapeln zu senden.

### <a name="master-detail"></a>Ausführliche Master
Tabellenwertfunktionen Parameter sind nützlich für einfache Szenarien mit einfügen. Es kann jedoch schwieriger Stapel fügt, die mehr als eine Tabelle beinhalten. Das "hierarchischen" Szenario ist ein guter Beispiel. Die Mastertabelle kennzeichnet die primäre Einheit. Mindestens ein Detailtabellen speichern weitere Daten zur Person an. In diesem Szenario erzwingen fremdschlüsselbeziehungen das Verhältnis von Details zu einer eindeutigen master Person an. Erwägen Sie eine vereinfachte Version einer Bestellung Tabelle und der zugehörigen OrderDetail Tabelle ein. Die folgende Transact-SQL-Anweisung erstellt die Bestellung-Tabelle mit vier Spalten: Bestell-Nr, OrderDate, CustomerID und Status.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Jede Bestellung enthält mindestens ein Erwerb von Produkten. Diese Informationen werden in der Tabelle PurchaseOrderDetail erfasst. Die folgende Transact-SQL-Anweisung erstellt die PurchaseOrderDetail-Tabelle mit fünf Spalten: Bestell-Nr, OrderDetailID, ProductID, Einzelpreis und OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

Die Spalte "OrderID" in der Tabelle PurchaseOrderDetail muss Ordnung aus der Tabelle Bestellung verwiesen werden. Die folgende Definition von einen Fremdschlüssel erzwingt diese Einschränkung.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Tabellenwertfunktionen Parameter verwenden möchten, müssen Sie eine benutzerdefinierte Table-Datentyp für jeden Zieltabelle verfügen.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Definieren Sie dann eine gespeicherte Prozedur, die Tabellen dieser Typen akzeptiert. Mit diesem Verfahren können eine Anwendung an die eine Reihe von Bestellungen und Bestelldetails einen einzelnen Anruf lokal Stapelverarbeitung. Die folgende Transact-SQL stellt die vollständige gespeicherte Prozedurdeklaration beispielsweise Reihenfolge dieser erwerben.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

In diesem Beispiel die lokal definierte @IdentityLink Tabelle speichert die tatsächliche Bestell-Nr-Werte aus der neu eingefügten Zeilen angewendet werden. Diese Reihenfolgebezeichner unterscheiden sich von der temporären OrderID Werte in den @orders und @details Tabellenwertfunktionen Parameter. Aus diesem Grund der @IdentityLink Tabelle dann eine Verbindung herstellt die Bestell-Nr Werte aus der @orders Parameter real OrderID Werte für die neuen Zeilen in der Tabelle Bestellung. Nach diesem Schritt der @IdentityLink Tabelle erleichtern wird die Bestelldetails mit der tatsächlichen Bestell-Nr, das die Fremdschlüssel-Einschränkung erfüllt einfügen.

Diese gespeicherte Prozedur kann von Code oder andere Transact-SQL-Anrufe verwendet werden. Finden Sie im Abschnitt Parameter Tabellenwertfunktionen dieses Dokuments für eines Beispiels aus. Die folgende Transact-SQL-Anweisung wird gezeigt, wie die Sp_InsertOrdersBatch anrufen.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Mit dieser Lösung können jeder Stapel, der eine Wertemenge OrderID verwenden, die mit 1 zu beginnen. Diese temporäre OrderID Werte beschreiben die Beziehungen in den Stapel, aber der tatsächlichen OrderID Werte zum Zeitpunkt der Einfügevorgang bestimmt werden. Können Sie wiederholt führen Sie die gleichen Anweisungen im vorherigen Beispiel und Generieren eindeutige Orders in der Datenbank. Erwägen Sie daher mehr Code oder Datenbank Logik, die doppelte Bestellungen wird verhindert, dass bei Verwendung dieser Methode Batchverarbeitung hinzufügen aus.

Dieses Beispiel zeigt, dass auch komplexere Datenbankvorgänge, wie z. B. Master / Detail-Vorgänge, Tabellenwertfunktionen Parameter zusammengefasst werden können.

### <a name="upsert"></a>UPSERT
Ein anderes Batchverarbeitungsszenario umfasst das vorhandene Zeilen und Einfügen von neuen Zeilen gleichzeitig zu aktualisieren. Dieser Vorgang wird manchmal als "UPSERT" (Update + EINFG) Vorgang bezeichnet. Anstatt separate Anrufe zum Einfügen und aktualisieren, ist die SERIENDRUCK-Anweisung für diese Aufgabe am besten geeignet. SERIENDRUCK-Anweisung kann ausführen beide einfügen und Aktualisieren von Operationen in einer einzelnen Anruf.

Tabellenwertfunktionen Parameter können mit der SERIENDRUCKFUNKTION-Anweisung zum Ausführen von Updates und fügt verwendet werden. Angenommen, Sie verfügen über eine vereinfachte Mitarbeitertabelle, die die folgenden Spalten enthält: EmployeeID, Vorname, Nachname, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
In diesem Beispiel können Sie die Fakultät, dass die SocialSecurityNumber zum Ausführen des SERIENDRUCKS mehrerer Mitarbeiter eindeutig ist. Erstellen Sie zuerst den benutzerdefinierter Tabellentyp:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Als Nächstes erstellen Sie eine gespeicherte Prozedur oder Schreiben von Code, die die SERIENDRUCK-Anweisung verwendet, führen Sie die Aktualisierung und einfügen. Im folgenden Beispiel wird die SERIENDRUCK-Anweisung für einen Parameter Tabellenwertfunktionen @employees, vom Typ EmployeeTableType. Den Inhalt der @employees Tabelle hier nicht angezeigt.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Weitere Informationen finden Sie unter der Dokumentation und Beispiele für den SERIENDRUCK-Anweisung. Obwohl die gleiche Arbeit in einer gespeicherten mehrstufige ausgeführt werden konnte mit Remote Procedure Call trennen einfügen und UPDATE-Vorgänge, die SERIENDRUCK-Anweisung ist effizienter. Datenbankcode kann auch Transact-SQL-Anrufe erstellen, mit denen die SERIENDRUCK-Anweisung direkt, ohne dass zwei Datenbank Anrufe für einfügen und aktualisieren.

## <a name="recommendation-summary"></a>Empfehlungen Zusammenfassung

Die folgende Liste enthält eine Zusammenfassung der in diesem Thema erläuterten Batchverarbeitung Empfehlungen:

- Verwenden Sie Puffer und Batchverarbeitung zum Steigern der Leistung und Skalierbarkeit von Applications SQL-Datenbank.
- Kompromisse zwischen Batchverarbeitung/Puffer und Stabilität. Während eines Fehlers Rolle möglicherweise das Risiko einen nicht verarbeiteten Stapel von Unternehmensdaten zu verlieren die Leistungsvorteile der Batchverarbeitung übersteigen.
- Versuchen Sie, bleiben alle Anrufe an die Datenbank in einem einzigen Rechenzentrum Wartezeit zu verringern.
- Wenn Sie eine Batchverarbeitung Technik auswählen, bieten Parameter Tabellenwertfunktionen an die optimale Leistung und Flexibilität.
- Führen Sie für die schnellste Leistung mit einfügen die folgenden allgemeinen Richtlinien, aber Testen von Ihrem Szenario:
    - Verwenden Sie für < 100 Zeilen einen einzelnen parametrisierten Einfügebefehl ein.
    - Verwenden Sie für < 1000 Zeilen Tabellenwertfunktionen Parameter ein.
    - Für > = 1000 Zeilen verwenden SqlBulkCopy.
- Für aktualisieren und Löschen von Vorgängen, verwenden Sie Parameter Tabellenwertfunktionen gespeicherte Prozedur Logik, die den richtigen Vorgang für jede Zeile in der Tabelle Parameter bestimmt.
- Stapel Größe Richtlinien:
    - Verwenden Sie die größten Stapelgrößen, die für die Anwendung und geschäftliche Anforderungen sinnvoll ein.
    - Verteilen der Leistungsgewinn der großer Stapel mit Risiken der temporären oder schwerwiegenden Fehlern. Was ist die Folge Wiederholungsversuche oder Verlust der Daten in den Stapel? 
    - Testen der größte Stapel, um sicherzustellen, dass die SQL-Datenbank nicht zurückgewiesen werden.
    - Erstellen Sie Konfiguration Einstellungen dieser Steuerelement Batchverarbeitung, wie die Stapelgröße oder des Zeitfensters Zwischenspeichern. Diese Einstellungen bieten Flexibilität. Sie können die Stapelverarbeitungsverhalten produziert ändern, ohne erneutes Cloud-Dienst bereitstellen.
- Vermeiden Sie parallele Ausführung von Stapeln, die auf eine einzelne Tabelle in einer Datenbank ausgeführt werden. Wenn Sie in einem einzigen Stapel über mehrere Arbeitsthreads Dividieren auswählen, führen Sie Tests durch, um die ideale Anzahl der Threads zu bestimmen. Nach dem Schwellenwert für ein nicht angegebener werden mehr Threads verkleinern Leistung statt erhöhen es.
- Erwägen Sie die Pufferung auf Größe und der Uhrzeit als eine Möglichkeit für Weitere Szenarien Batchverarbeitung zu implementieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel dienten wie Datenbankentwurf und verstehen und anwenden, die im Zusammenhang mit Batchverarbeitung Ihrer Anwendung Leistung und Skalierbarkeit verbessern können. Dies ist nur eine Faktor für insgesamt strategische jedoch. Weitere Informationen zum Verbessern der Leistung und Skalierbarkeit finden Sie unter [für einzelne Datenbanken Leitfaden zur Azure SQL-Datenbank Leistung](sql-database-performance-guidance.md) und [Preis und Leistung für ein Ressourcenpool flexible Datenbank](sql-database-elastic-pool-guidance.md).
