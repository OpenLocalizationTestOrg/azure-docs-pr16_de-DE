<properties
    pageTitle="Anhalten und Fortsetzen der Migration von Daten (gedehnt Datenbank) | Microsoft Azure"
    description="Informationen Sie zum Anhalten oder Fortsetzen der Migration von Daten in Azure."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Anhalten Sie und fortsetzen Sie der Migration von Daten (gedehnt Datenbank)

Wenn anhalten oder Fortsetzen der Migration von Daten in Azure, wählen Sie für eine Tabelle in SQL Server Management Studio **gedehnt** aus, und wählen Sie dann Datenmigration angehalten **Anhalten** oder **Fortsetzen** , zum Fortsetzen der Migration von Daten. Sie können auch die Transact\-SQL zum Anhalten oder Fortsetzen der Migration von Daten.

Anhalten Migration von Daten auf einzelne Tabellen, wenn Sie für die Problembehandlung auf dem lokalen Server oder die verfügbaren Bandbreite maximieren möchten.

## <a name="pause-data-migration"></a>Anhalten der Migration von Daten

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Verwenden von SQL Server Management Studio angehalten Migration von Daten

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer den gedehnt\-aktiviert Tabelle Datenmigration angehalten werden soll.

2.  Rechts\-klicken Sie auf **gedehnt**wählen Sie aus, und wählen Sie dann auf **Anhalten**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Verwenden Sie Transact\-SQL angehalten Migration von Daten
Führen Sie den folgenden Befehl ein.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Fortsetzen der Migration von Daten

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Verwenden von SQL Server Management Studio zum Fortsetzen der Migration von Daten

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer den gedehnt\-aktiviert die Tabelle, für die Sie Datenmigration fortsetzen möchten.

2.  Rechts\-klicken Sie auf **gedehnt**wählen Sie aus, und wählen Sie dann auf **Lebenslauf**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Verwenden Sie Transact\-SQL zum Fortsetzen der Migration von Daten
Führen Sie den folgenden Befehl ein.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Überprüfen Sie, ob die Migration aktiv oder angehalten ist.

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Verwenden von SQL Server Management Studio prüfen, ob die Migration aktiv oder angehalten ist.
Klicken Sie in SQL Server Management Studio öffnen Sie **Gedehnt Datenbank Monitor** , und überprüfen Sie den Wert der Spalte **Status der Migration** . Weitere Informationen finden Sie unter [Überwachen und Problembehandlung bei der Migration von Daten](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Verwenden von Transact-SQL prüfen, ob die Migration aktiv oder angehalten ist.
Abfragen Sie der Katalog Ansicht **sys.remote_data_archive_tables** , und überprüfen Sie den Wert der Spalte **Is_migration_paused** . Weitere Informationen finden Sie unter [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Siehe auch

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[Überwachen und Problembehandlung bei der Migration von Daten](sql-server-stretch-database-monitor.md)
