<properties
    pageTitle="Verwalten von und Problembehandlung bei gedehnt Datenbank | Microsoft Azure"
    description="Informationen Sie zum Verwalten von und Problembehandlung bei gedehnt Datenbank."
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
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Verwalten von und Problembehandlung bei gedehnt Datenbank

Zum Verwalten und Problembehandlung bei gedehnt Datenbank, verwenden Sie die Tools und Methoden in diesem Artikel beschrieben.

## <a name="manage-local-data"></a>Verwalten von lokalen Daten

### <a name="a-namelocalinfoaget-info-about-local-databases-and-tables-enabled-for-stretch-database"></a><a name="LocalInfo"></a>Erhalten von Informationen zu lokalen Datenbanken und Tabellen, die für die Datenbank gedehnt aktiviert
Öffnen Sie den Katalog Ansichten **sys.databases** und **' sys.Tables '** , um Informationen zu gedehnt finden Sie unter\-SQL Server-Datenbanken und Tabellen aktiviert. Weitere Informationen finden Sie unter [sys.databases (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) und [' sys.Tables ' (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Wie viel Speicherplatz ein gedehnt sehen\-aktiviert Tabelle ist in SQL Server, führen Sie folgende Anweisung verwenden.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Verwalten der Migration von Daten

### <a name="check-the-filter-function-applied-to-a-table"></a>Aktivieren Sie das zu einer Tabelle angewendete Filterfunktion
Öffnen Sie die Katalogansicht **sys.remote\_Daten\_Archiv\_Tabellen** und überprüfen Sie den Wert von der **Filter\_Prädikat** Spalte, um die Funktion zu identifizieren, die Datenbank gedehnt zum Auswählen von Zeilen migrieren verwendet wird. Wenn der Wert null ist, wird die gesamte Tabelle berechtigt migriert werden. Weitere Informationen finden Sie unter [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) , und [Wählen Sie Zeilen mithilfe einer Filterfunktion migrieren](sql-server-stretch-database-predicate-function.md).

### <a name="a-namemigrationacheck-the-status-of-data-migration"></a><a name="Migration"></a>Überprüfen Sie den Status der Datenmigration
Wählen Sie Vorgänge **| Dehnen | Monitor** für eine Datenbank in SQL Server Management Studio zum Überwachen der Migration von Daten in die Datenbank Monitor gedehnt. Weitere Informationen finden Sie unter [Überwachen und Problembehandlung bei der Migration von Daten (gedehnt Datenbank)](sql-server-stretch-database-monitor.md).

Oder öffnen Sie die Ansicht dynamische Management **sys.dm\_Db\_rda\_Migration\_Status** sehen, wie viele Blattnamen und Datenzeilen migriert wurden.

### <a name="a-namefirewallatroubleshoot-data-migration"></a><a name="Firewall"></a>Behandeln von Problemen mit der Migration von Daten
Vorschläge zur Problembehandlung, finden Sie unter [Überwachen und Problembehandlung bei der Migration von Daten (gedehnt Datenbank)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Verwalten von remote-Daten

### <a name="a-nameremoteinfoaget-info-about-remote-databases-and-tables-used-by-stretch-database"></a><a name="RemoteInfo"></a>Erhalten von Informationen zu entfernten Datenbanken und Tabellen, die von gedehnt Datenbank verwendet
Öffnen Sie die Katalogsansichten **sys.remote\_Daten\_Archiv\_Datenbanken** und **sys.remote\_Daten\_Archiv\_Tabellen** zu finden Sie unter Informationen zu den entfernten Datenbanken und Tabellen, in dem migrierte Daten gespeichert ist. Weitere Informationen finden Sie unter [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) und [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

Um wie viel Speicherplatz ist eine Tabelle gedehnt aktiviert in Azure, führen Sie folgende Anweisung verwenden.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Migrierte Daten löschen  
Wenn Sie Daten, die bereits zu Azure migriert wurde, löschen möchten, führen Sie die in [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx)beschriebenen Schritte.

## <a name="manage-table-schema"></a>Tabellenschema verwalten

### <a name="dont-change-the-schema-of-the-remote-table"></a>Das Schema der entfernten Tabelle nicht ändern
Ändern Sie nicht das Schema einer remote Azure-Tabelle, die eine SQL Server-Tabelle, die so konfiguriert, dass für gedehnt Datenbank zugeordnet ist. Insbesondere ändern Sie nicht den Namen oder den Datentyp einer Spalte. Das Feature gedehnt Datenbank Annahmen aus verschiedenen über das Schema der entfernten Tabelle in Bezug auf das Schema der SQL Server-Tabelle. Wenn Sie das entfernte Schema ändern, funktioniert gedehnt Datenbank nicht mehr für die geänderte Tabelle.

### <a name="reconcile-table-columns"></a>Stimmen Sie Tabellenspalten ab.  
Wenn Sie Spalten aus der Tabelle remote versehentlich gelöscht haben, führen Sie **Sp_rda_reconcile_columns** zum Hinzufügen von Spalten zu der remote-Tabelle, die in der Strecken vorhanden sind\-SQL Server-Tabelle aktiviert, jedoch nicht in der entfernten Tabelle. Weitere Informationen finden Sie unter [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx).  

  > [!IMPORTANT] Wenn **Sp_rda_reconcile_columns** Spalten neu, die Sie aus der entfernten Tabelle versehentlich gelöscht haben erstellt, werden sie nicht die Daten wiederhergestellt, die zuvor in die gelöschten Spalten wurde.

**Sp_rda_reconcile_columns** Löscht Spalten aus der remote-Tabelle, die in der entfernten Tabelle aber nicht in der Strecken vorhanden sind nicht\-SQL Server-Tabelle aktiviert. Wenn es gibt Spalten in der entfernten Azure-Tabelle, die in der Strecken nicht mehr vorhanden sind\-aktiviert SQL Server Tabelle diese zusätzlichen Spalten kann ich nicht verhindern gedehnt Datenbank funktioniert normal. Sie können die zusätzlichen Spalten manuell entfernen.  

## <a name="manage-performance-and-costs"></a>Verwalten der Leistung und Kosten  

### <a name="troubleshoot-query-performance"></a>Behandeln von Problemen mit der Leistung von Abfragen
Abfragen, in denen gedehnt\-aktivierte Tabellen sind die verlorenen langsamer als diese hat, bevor die Tabellen gedehnt aktiviert worden. Wenn die abfrageleistung erheblich nimmt ab, überprüfen Sie folgende mögliche Probleme.

-   Ist der Azure-Server in einem anderen geografische Region als SQL Server? Konfigurieren der Azure benutzerspezifisch in derselben geografische Region wie SQL Server, Netzwerkwartezeit zu verringern.

-   Ihre netzwerkbedingungen möglicherweise heruntergestuft haben. Informationen zum zuletzt verwendete Probleme oder Ausfall wenden Sie sich an Ihren Netzwerkadministrator.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Vergrößern von Azure Leistungsstufe für Ressourcen\-stark JOIN-Operationen wie Indizierung
Wenn Sie erstellen, neu zu erstellen oder Neuorganisieren ein Indexes auf einer großen Tabelle, die für die Datenbank gedehnt konfiguriert ist, und Sie erwarten beanspruchen Abfragen migrierte Daten in Azure dieses Zeitraums, erwägen Sie die Leistungsstufe der entsprechenden Azure Remotedatenbank für die Dauer des Vorgangs zu erhöhen. Weitere Informationen über die Leistung und Preise finden Sie unter [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Sie können den SQL Server-gedehnt Datenbank-Dienst auf Azure nicht anhalten  
 Stellen Sie sicher, dass Sie die entsprechenden Leistung und Preise Ebene auswählen. Wenn Sie die Leistungsstufe vorübergehend für eine Ressource erhöhen\-stark Operation wiederherstellen, auf dem vorherigen Niveau, nach Abschluss des Vorgangs. Weitere Informationen über die Leistung und Preise finden Sie unter [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  

## <a name="change-the-scope-of-queries"></a>Ändern des Gültigkeitsbereichs von Abfragen  
 Abfragen für gedehnt\-aktivierte Tabellen lokale und remote-Daten standardmäßig zurückgeben. Sie können den Umfang der Abfragen für alle Abfragen von allen Benutzern oder nur für eine einzelne Abfrage von einem Administrator ändern.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Ändern des Gültigkeitsbereichs von Abfragen für alle Abfragen von allen Benutzern  
 Führen Sie zum Ändern des Gültigkeitsbereichs von alle Abfragen von allen Benutzern die gespeicherte Prozedur **sys.sp_rda_set_query_mode**aus. Sie können den Bereich um Abfragen nur lokale Daten, deaktivieren alle Abfragen oder die Standardeinstellung wiederherstellen reduzieren. Weitere Informationen finden Sie unter [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx).  

### <a name="a-namequeryhintsachange-the-scope-of-queries-for-a-single-query-by-an-administrator"></a><a name="queryHints"></a>Ändern des Gültigkeitsbereichs von Abfragen für eine einzelne Abfrage von einem administrator  
 Hinzufügen, um den Bereich einer Abfrage von einem Mitglied der Rolle Db_owner Ändern der * *mit \( REMOTE_DATA_ARCHIVE_OVERRIDE = *Wert* \)** Abfrage Hinweistext in der SELECT-Anweisung. Der Abfrage-Hinweis REMOTE_DATA_ARCHIVE_OVERRIDE kann die folgenden Werte aufweisen.  
 -   **LOCAL_ONLY**. Abfrage nur lokale Daten.  

 -   **REMOTE_ONLY**. Abfrage remote-Daten.  

 -   **STAGE_ONLY**. Abfrage nur die Daten in der Tabelle, wo gedehnt Datenbank Phasen Zeilen für die Migration in Frage kommenden und behält migrierte Zeilen für die angegebene Periode nach der Migration. Dieser Abfragehinweis besteht die einzige Möglichkeit zum Abfragen der staging Tabelle aus.  

Die folgende Abfrage gibt beispielsweise nur lokale Ergebnisse zurück.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="a-nameadminhintsamake-administrative-updates-and-deletes"></a><a name="adminHints"></a>Stellen Sie administrative aktualisieren und löschen  
 Standardmäßig kann nicht aktualisiert werden, oder Löschen von Zeilen, die für die Migration berechtigt sind, oder Zeilen, die bereits, in einem gedehnt migriert wurden\-Tabelle aktiviert. Wenn Sie ein Problem zu beheben haben, kann ein Mitglied der Rolle Db_owner durch Hinzufügen ein Vorgangs aktualisieren oder löschen Ausführen der * *mit \( REMOTE_DATA_ARCHIVE_OVERRIDE = *Wert* \)** Abfragehinweis für die Anweisung. Der Abfrage-Hinweis REMOTE_DATA_ARCHIVE_OVERRIDE kann die folgenden Werte aufweisen.  
 -   **LOCAL_ONLY**. Aktualisieren oder nur lokale Daten löschen.  

 -   **REMOTE_ONLY**. Aktualisieren oder Löschen von remote-Daten.  

 -   **STAGE_ONLY**. Aktualisieren oder nur die Daten in der Tabelle, wobei gedehnt Datenbank Phasen Zeilen für die Migration berechtigt und behält migrierte Zeilen für die angegebene Periode nach der Migration, löschen.  

## <a name="see-also"></a>Siehe auch

[Dehnen Monitor-Datenbank](sql-server-stretch-database-monitor.md)

[Zusätzliche gedehnt aktiviert Datenbanken](sql-server-stretch-database-backup.md)

[Wiederherstellen der Datenbanken gedehnt aktiviert](sql-server-stretch-database-restore.md)
