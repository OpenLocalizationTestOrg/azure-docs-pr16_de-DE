<properties
    pageTitle="Wiederherstellen der Datenbanken gedehnt aktiviert | Microsoft Azure"
    description="Informationen zum Wiederherstellen von gedehnt\-Datenbanken aktiviert."
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
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Wiederherstellen der Datenbanken gedehnt aktiviert

Stellen Sie eine gesicherte Datenbank wiederherstellen aus vielen Arten von Fehlern, Fehlern und Schäden im Bedarfsfall wieder her.

Weitere Informationen zu sichern finden Sie unter [Datenbanken Sicherung gedehnt aktiviert](sql-server-stretch-database-backup.md).

>   [AZURE.NOTE] Sicherung ist nur einen Teil einer abgeschlossen hohen Verfügbarkeit und Business Continuity-Lösung. Weitere Informationen zur hohen Verfügbarkeit finden Sie unter [Hohen Verfügbarkeit von Lösungen](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>Wiederherstellen von SQL Server-Daten
Zum Wiederherstellen von beschädigt oder Hardware Wiederherstellen der Strecken\-SQL Server-Datenbank aus einer Sicherung aktiviert. Sie können weiterhin die SQL Server Wiederherstellungsmethoden verwenden, die Sie zurzeit verwenden. Weitere Informationen finden Sie unter [Wiederherstellen und Wiederherstellung (Übersicht)](https://msdn.microsoft.com/library/ms191253.aspx).

Nachdem Sie die SQL Server-Datenbank wiederherstellen, müssen Sie die gespeicherte Prozedur **sys.sp_rda_reauthorize_db** zum Herstellen einer Verbindung zwischen der Strecken erneut ausführen\-SQL Server-Datenbank und die Remotedatenbank Azure aktiviert. Weitere Informationen finden Sie unter [die Verbindung zwischen der SQL Server-Datenbank und der entfernten Azure-Datenbank wiederherstellen](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Wiederherstellen von remote Azure-Daten

### <a name="recover-a-live-azure-database"></a>Wiederherstellen einer live Azure-Datenbank
Der SQL Server-Datenbank gedehnt service alle Livedaten mindestens alle 8 Stunden, die mithilfe von Momentaufnahmen der Azure-Speicher auf Azure Momentaufnahmen. Diese Momentaufnahmen werden für sieben Tage beibehalten. So können Sie die Daten auf einem mindestens 21 Endpunkte Zeitpunkt innerhalb der letzten 7 Tage bis zum Zeitpunkt wiederherstellen wann die letzte Momentaufnahme erstellt wurde.

Gehen Sie folgendermaßen vor, um eine live Azure-Datenbank zu einem früheren Zeitpunkt wiederherstellen mithilfe des Azure-Portals.

1. Melden Sie sich des Azure-Portals an.
2. Wählen Sie **Durchsuchen** , und wählen Sie dann auf **SQL-Datenbanken**, klicken Sie auf der linken Seite des Bildschirms.
3. Navigieren Sie zu der Datenbank, und wählen Sie ihn aus.
4. Am oberen Rand der Datenbank Blade klicken Sie auf **Wiederherstellen**.
5. Geben Sie einen neuen **Namen der Datenbank**, wählen Sie einen **Punkt wiederherstellen** und dann auf **Erstellen**.
6. Der Datenbank Wiederherstellungsprozess beginnt und mit **BENACHRICHTIGUNGEN**überwacht werden kann.

### <a name="recover-a-deleted-azure-database"></a>Wiederherstellen einer gelöschten Azure-Datenbank
Der Dienst SQL Server gedehnt Datenbank auf Azure wird eine Datenbank Momentaufnahme aus, bevor eine Datenbank gelöscht wird und sie 7 Tage lang behält. Nachdem Sie in diesem Fall wird nicht mehr Momentaufnahmen aus der Datenbank live beibehalten. Auf diese Weise können Sie dem Punkt eine gelöschte Datenbank wiederherstellen, wenn sie gelöscht wurde.

Zum Wiederherstellen einer gelöschten Azure-Datenbank dem Punkt an, wenn es mithilfe des Azure-Portals gelöscht wurde, führen Sie die folgenden Elemente aus.

1. Melden Sie sich des Azure-Portals an.
2. Klicken Sie auf der linken Seite des Bildschirms Wählen Sie **Durchsuchen** , und wählen Sie dann auf **SQL Server**.
3. Navigieren Sie zu dem Server, und wählen Sie ihn aus.
4. Führen Sie einen Bildlauf nach unten bis zum Vorgänge des Servers Blade, klicken Sie auf die Kachel **Datenbanken gelöscht** .
5. Wählen Sie die gelöschte Datenbank, die Sie wiederherstellen möchten.
5. Geben Sie einen neuen **Namen der Datenbank** , und klicken Sie auf **Erstellen**.
6. Der Datenbank Wiederherstellungsprozess beginnt und mit **BENACHRICHTIGUNGEN**überwacht werden kann.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Stellen Sie die Verbindung zwischen der SQL Server-Datenbank und die Remotedatenbank Azure wieder her.

1.  Wenn Sie die Verbindung zu einer wiederhergestellten Azure-Datenbank mit einem anderen Namen oder in einem anderen Bereich vertraut sind, führen Sie die gespeicherte Prozedur [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) aus der vorherigen Azure-Datenbank zu trennen.  

2.  Führen Sie die gespeicherte Prozedur [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) , um die Verbindung der lokalen Strecken wiederherzustellen\-Datenbank in der Azure-Datenbank aktiviert.  

    -   Die vorhandene Datenbank ausgelegte Anmeldeinformationen als eine Sysname oder eine Varchar bieten\(128\) Wert. \(Verwenden Sie Varchar nicht\(max\).\) Sie können den Namen der Anmeldeinformationen in der Ansicht Nachschlagen **sys.database\_ausgelegte\_Anmeldeinformationen**.  

    -   Geben Sie an, ob eine Kopie der remote-Daten, und verbinden Sie die Kopie (empfohlen).  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Siehe auch

[Verwalten von und Problembehandlung bei gedehnt Datenbank](sql-server-stretch-database-manage.md)

[Sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sichern und Wiederherstellen von SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx)
