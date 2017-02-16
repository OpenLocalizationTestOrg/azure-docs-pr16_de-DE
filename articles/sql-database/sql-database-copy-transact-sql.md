<properties 
    pageTitle="Kopieren eine mit Transact-SQL Azure SQL-Datenbank | Microsoft Azure" 
    description="Erstellen Sie Kopieren einer mit Transact-SQL Azure SQL-Datenbank" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopieren einer mit Transact-SQL Azure SQL-Datenbank


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Die folgenden Schritte gezeigt, wie in einer SQL-Datenbank mit Transact-SQL in der gleichen oder einer anderen Server zu kopieren. Beim Kopieren Datenbank verwendet die [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) -Anweisung.

Um die Schritte in diesem Artikel ausführen, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Abonnement Azure lediglich **Kostenlose Testversion** am oberen Rand dieser Seite klicken Sie auf, und klicken Sie dann wieder zum Ende der in diesem Artikel.
- Einer Azure-SQL­Datenbank. Wenn Sie nicht mit eine SQL-Datenbank verfügen, erstellen eine folgen die Schritten in diesem Artikel: [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Wenn Sie SSMS besitzen oder Features, die in diesem Artikel beschriebenen sind nicht verfügbar, [Laden Sie die aktuellste Version](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopieren Sie die SQL-Datenbank

Melden Sie sich bei der master-Datenbank mithilfe der Server-Ebene Hauptbenutzer Login oder der Benutzername, der die Datenbank erstellt, die Sie kopieren möchten. Mitglieder der Rolle Dbmanager muss Benutzernamen, die nicht die Server Ebene der Tilgungsanteile sind um Datenbanken zu kopieren. Weitere Informationen zu Benutzernamen und Herstellen einer Verbindung mit dem Server finden Sie unter [Verwalten von Benutzernamen](sql-database-manage-logins.md).

Kopieren der Quelldatenbank mit [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) -Anweisung zu starten. Diese Anweisung ausführen initiiert die Datenbank kopieren. Da Kopieren einer Datenbank einen asynchronen Prozess, gibt CREATE DATABASE-Anweisung vor die Datenbank kopieren abgeschlossen ist.


### <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf dem gleichen server

Melden Sie sich bei der master-Datenbank mithilfe der Server-Ebene Hauptbenutzer Login oder der Benutzername, der die Datenbank erstellt, die Sie kopieren möchten. Mitglieder der Rolle Dbmanager muss Benutzernamen, die nicht die Server Ebene der Tilgungsanteile sind um Datenbanken zu kopieren.

Dieser Befehl Kopien Database1 an eine neue Datenbank mit dem Namen Database2 auf demselben Server. Abhängig von der Größe der Datenbank kann der Kopiervorgang einige Zeit in Anspruch dauern.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einem anderen server

Melden Sie sich bei der master-Datenbank des Servers Ziel, die Stelle, an der die neue Datenbank wird erstellt werden Azure SQL-Datenbankserver an. Verwenden Sie eine Anmeldung mit demselben Namen und Kennwort als Datenbankbesitzer (DBO) der Quelldatenbank auf dem Quelle Azure SQL-Datenbankserver an. Der Benutzername auf die Zielserver muss der Server Ebene Hauptbenutzer Login auch ein Mitglied der Rolle Dbmanager oder sein.

Dieser Befehl kopiert Database1 auf server1 - in eine neue Datenbank mit dem Namen Database2 auf server2. Abhängig von der Größe der Datenbank kann der Kopiervorgang einige Zeit in Anspruch dauern.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>Überwachen des Fortschritts von der kopieren

Überwachen Sie den Kopiervorgang zu, indem Sie die Ansichten sys.databases und sys.dm_database_copies Abfragen. Während der kopieren ausgeführt wird, wird die Spalte State_desc der Ansicht sys.databases für die neue Datenbank auf Kopieren festgelegt.


- Wenn das Kopieren fehlschlägt, wird die Spalte State_desc der Ansicht sys.databases für die neue Datenbank auf VERDÄCHTIGEN festgelegt. In diesem Fall die DROP-Anweisung ausführen, klicken Sie auf die neue Datenbank, und versuchen Sie es später erneut.
- Wenn das Kopieren erfolgreich ist, wird die Spalte State_desc der Ansicht sys.databases für die neue Datenbank auf ONLINE festgelegt. In diesem Fall das Kopieren abgeschlossen ist, und die neue Datenbank ist eine reguläre Datenbank, unabhängig von der Quelldatenbank geändert werden.

> [AZURE.NOTE] – Wenn Sie sich entscheiden, um den Vorgang abzubrechen, das Kopieren, während sie ausgeführt wird, führen Sie die [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) -Anweisung für die neue Datenbank. Sie können auch bricht die DROP DATABASE-Anweisung für die Quelldatenbank ausführen, auch den Kopiervorgang zu ab.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Beheben von Benutzernamen nach Abschluss des Kopiervorgangs

Nachdem Sie die neue Datenbank auf dem Zielserver online ist, verwenden Sie [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) -Anweisung der Benutzer aus der neuen Datenbank Benutzernamen auf dem Zielserver neu zuordnen aus. Um verwaiste Benutzer zu beheben, finden Sie unter [Problembehandlung bei verwaisten Benutzern](https://msdn.microsoft.com/library/ms175475.aspx). Siehe auch [zum Verwalten der Sicherheit von SQL Azure-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).

Alle Benutzer in der neuen Datenbank verwalten die Berechtigungen, die sie in der Quelldatenbank hatten. Der Benutzer, der die Datenbankkopie initiiert der Datenbank Besitzer der neuen Datenbank und eine neue Sicherheits-ID (SID) zugeordnet ist. Nachdem das Kopieren hergestellt wurde und vor anderen Benutzern erneut zugeordnet sind, kann nur der Benutzername, der initiiert kopieren, den Datenbankbesitzer (DBO), in die neue Datenbank anmelden.


## <a name="next-steps"></a>Nächste Schritte

- Übersicht über das Kopieren einer Azure SQL-Datenbank finden Sie unter [kopieren eine SQL Azure-Datenbank](sql-database-copy.md) .
- Kopieren eine Datenbank mit dem Azure-Portal [kopieren eine Azure SQL-Datenbank mit dem Azure-Portal](sql-database-copy-portal.md) finden Sie unter.
- Kopieren eine Datenbank mithilfe der PowerShell [Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell](sql-database-copy-powershell.md) finden Sie unter.
- Informationen Sie, [wie zum Verwalten der Sicherheit von SQL Azure-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzern und Benutzernamen, beim Kopieren einer Datenbank auf einem anderen logischen Server.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Verwalten von Benutzernamen](sql-database-manage-logins.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md)
- [Exportieren Sie die Datenbank in einer BACPAC](sql-database-export.md)
- [Business Continuity (Übersicht)](sql-database-business-continuity.md)
- [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)


