<properties
    pageTitle="Aktivieren Sie gedehnt Datenbank für eine Tabelle | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren einer Tabelle für gedehnt Datenbank."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-table"></a>Aktivieren Sie gedehnt Datenbank für eine Tabelle

Wählen Sie zum Konfigurieren einer Tabelle für gedehnt Datenbank **gedehnt | Aktivieren von** für eine Tabelle in SQL Server Management Studio, um die **Tabelle gedehnt aktivieren** -Assistenten zu öffnen. Sie können auch die Transact\-SQL aktivieren gedehnt Datenbank auf einer vorhandenen Tabelle oder zum Erstellen einer neuen Tabelle mit gedehnt Datenbank aktiviert.

-   Wenn Sie kalte Daten in einer separaten Tabelle speichern möchten, können Sie die gesamte Tabelle migrieren.

-   Wenn die Tabelle wichtiges und kalte Daten enthält, können Sie eine Filterfunktion, markieren Sie die Zeilen migrieren angeben.

**Erforderliche Komponenten**. Wenn Sie **gedehnt auswählen | Aktivieren von** für eine Tabelle, und Sie noch keine Strecken Datenbank für die Datenbank aktiviert haben, konfiguriert der Assistent die Datenbank zuerst für gedehnt Datenbank. Führen Sie die Schritte in [Erste Schritte, indem Sie die Datenbank aktivieren gedehnt-Assistenten ausführen](sql-server-stretch-database-wizard.md) anstelle der Schritte in diesem Thema.

**Berechtigungen**. Aktivieren der gedehnt Datenbank anhand einer Datenbank oder einer Tabelle Db erfordert\_die Berechtigungen eines Websitebesitzers. Aktivieren der gedehnt Datenbank für eine Tabelle erfordert auch ALTER-Berechtigungen für die Tabelle aus.

 >   [AZURE.NOTE] Später, wenn Sie gedehnt Datenbank deaktivieren, beachten Sie, dass für eine Tabelle oder für eine Datenbank gedehnt Datenbank deaktivieren das remote-Objekt nicht gelöscht. Wenn Sie die entfernte Tabelle oder die Remotedatenbank löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Der remote-Objekte weiterhin Azure Kosten entstehen, bis Sie manuell löschen.
 
## <a name="a-nameenablewizardtableause-the-wizard-to-enable-stretch-database-on-a-table"></a><a name="EnableWizardTable"></a>Verwenden des Assistenten zum Aktivieren von gedehnt Datenbank in einer Tabelle
**Starten Sie den Assistenten**

1.  Wählen Sie die Tabelle, die auf der Sie gedehnt aktivieren möchten, in SQL Server Management Studio im Objekt-Explorer.

2.  Rechts\-klicken Sie auf **gedehnt**wählen Sie aus, und wählen Sie dann auf **Aktivieren** , um den Assistenten zu starten.

**Einführung**

Überprüfen Sie den Zweck des Assistenten und die erforderlichen Komponenten.

**Select-Datenbanktabellen**

Bestätigen Sie, dass die Tabelle, die Sie aktivieren möchten angezeigt und ausgewählt ist.

Sie können eine gesamte Tabelle migrieren, oder Sie können eine einfache Filterfunktion Geben Sie im Assistenten. Wenn Sie einen anderen Typ von Filter-Funktion verwenden Sie zum Auswählen von Zeilen migrieren möchten, führen Sie eine der folgenden Aktionen aus.

-   Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung gedehnt für die Tabelle zu aktivieren und eine Filterfunktion anzugeben.

-   Führen Sie die ALTER TABLE-Anweisung, um eine Filterfunktion anzugeben, nachdem Sie den Assistenten zu beenden. Die erforderlichen Schritte finden Sie unter [Hinzufügen einer Filterfunktion nach dem Ausführen des Assistenten](sql-server-stretch-database-predicate-function.md#addafterwiz).

Die Syntax der ALTER TABLE wird später in diesem Artikel beschrieben.

**Zusammenfassung**

Überprüfen Sie die Werte, die Sie eingegeben haben und die Optionen, die Sie im Assistenten ausgewählt haben. Wählen Sie dann aktivieren gedehnt **Fertig stellen** .

**Ergebnisse**

Überprüfen Sie die Ergebnisse an.

## <a name="a-nameenabletsqltableause-transact-sql-to-enable-stretch-database-on-a-table"></a><a name="EnableTSQLTable"></a>Verwenden Sie Transact\-SQL gedehnt Datenbank für eine Tabelle aktivieren
Sie können gedehnt Datenbank für eine vorhandene Tabelle aktivieren oder erstellen eine neue Tabelle mit gedehnt Datenbank mithilfe von Transact-SQL aktiviert.

### <a name="options"></a>Optionen
Verwenden Sie die folgenden Optionen beim Ausführen von CREATE TABLE oder ALTER TABLE gedehnt Datenbank auf einer Tabelle zu aktivieren.

-   Verwenden Sie optional die `FILTER_PREDICATE = <function>` -Klausel, um die Angabe einer Funktion zum Auswählen von Zeilen zu migrieren, wenn die Tabelle wichtiges und kalte Daten enthält. Das Prädikat muss eine Tabelle Inline Aufrufen\-bewertet (Funktion). Weitere Informationen finden Sie unter [Wählen Sie Zeilen mithilfe einer Filterfunktion migrieren](sql-server-stretch-database-predicate-function.md). Wenn Sie eine Filter-Funktion nicht angeben, wird die gesamte Tabelle migriert.

    >   [AZURE.NOTE] Wenn Sie eine Filterfunktion schlecht ausführt bereitstellen, führt Datenmigration schlecht. Dehnen Datenbank wendet die Filterfunktion in die Tabelle mit dem Operator CROSS anwenden.

-   Geben Sie `MIGRATION_STATE = OUTBOUND` sofort mit der Migration von Daten oder `MIGRATION_STATE = PAUSED` zu Beginn der Migration von Daten zu verschieben.

### <a name="enable-stretch-database-for-an-existing-table"></a>Aktivieren Sie gedehnt Datenbank für eine vorhandene Tabelle
Führen Sie zum Konfigurieren einer vorhandenen Tabelle für gedehnt Datenbank Befehl ALTER TABLE aus.

Hier ist ein Beispiel, migriert werden die gesamte Tabelle und Migration von Daten sofort beginnt.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```
Hier ein Beispiel, die nur die Zeilen identifizierten migriert werden die `dbo.fn_stretchpredicate` Inline-Tabelle\-bewertet (Funktion) und verschiebt Migration von Daten. Weitere Informationen zu den Filter-Funktion finden Sie unter [Wählen Sie Zeilen mithilfe einer Filterfunktion migrieren](sql-server-stretch-database-predicate-function.md).

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
 GO
```

Weitere Informationen finden Sie unter [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).

### <a name="create-a-new-table-with-stretch-database-enabled"></a>Erstellen einer neuen Tabelle mit gedehnt Datenbank aktiviert
Führen Sie zum Erstellen einer neuen Tabelle mit gedehnt Datenbank aktiviert, den Befehl Tabelle erstellen aus.

Hier ist ein Beispiel, migriert werden die gesamte Tabelle und Migration von Daten sofort beginnt.

```tsql
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```

Hier ein Beispiel, die nur die Zeilen identifizierten migriert werden die `dbo.fn_stretchpredicate` Inline-Tabelle\-bewertet (Funktion) und verschiebt Migration von Daten. Weitere Informationen über die Filterfunktion finden Sie unter [Wählen Sie Zeilen mithilfe einer Filterfunktion migrieren](sql-server-stretch-database-predicate-function.md).

```tsql
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
GO  
```

Weitere Informationen finden Sie unter [CREATE TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms174979.aspx).


## <a name="see-also"></a>Siehe auch

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)

[Erstellen Sie die Tabelle (Transact-SQL)](https://msdn.microsoft.com/library/ms174979.aspx)
