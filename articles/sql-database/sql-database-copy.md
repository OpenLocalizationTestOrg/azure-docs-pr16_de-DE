<properties
    pageTitle="Kopieren eine SQL Azure-Datenbank | Microsoft Azure"
    description="Erstellen einer Kopie einer SQL Azure-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Kopieren einer SQL Azure-Datenbank

> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Die Azure [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md) können Sie um eine Kopie Ihrer SQL-Datenbank zu erstellen. Die Datenbankkopie verwendet die gleiche Technologie wie das Feature "Geo-Replikation". Aber im Gegensatz zu Geo-Replikation es die Replikation Verknüpfung als beendet, sobald die Seedrouting konfigurierten Phase abgeschlossen ist. Daher ist die Datenbank kopieren eine Momentaufnahme der Quelldatenbank zum Zeitpunkt der Erstellung der Anfrage kopieren.  
Sie können die Datenbankkopie auf dem gleichen Server oder einem anderen Server erstellen. Die Ebene und Leistung Dienstalter (Preisgestaltung Stufe) der Datenbankkopie sind standardmäßig der Quelldatenbank identisch. Bei der Verwendung der API können Sie einen anderen Leistungsstufe innerhalb der gleichen Dienst Schicht (Edition) auswählen. Nachdem das Kopieren abgeschlossen ist, wird die Kopie einer Datenbank voll funktionsfähige, unabhängig an. Sie können zu diesem Zeitpunkt aktualisieren oder sollten sie eine beliebige Edition. Die Benutzernamen, Benutzer und Berechtigungen können unabhängig voneinander verwaltet werden.  

Beim Kopieren einer Datenbank auf dem gleichen logischen Server können die gleichen Benutzernamen auf beide Datenbanken verwendet werden. Die wichtigsten können Sie die Datenbank kopieren Sicherheit wird den Datenbankbesitzer (DBO) für die neue Datenbank. Alle Datenbankbenutzer, deren Berechtigungen und ihren Sicherheits-IDs (SIDs) werden in der Datenbankkopie kopiert.  

Beim Kopieren einer Datenbank auf einem anderen logischen Server wird die Sicherheit auf dem neuen Server Hauptbenutzer den Datenbankbesitzer für die neue Datenbank ein. Wenn Sie für [Datenbankbenutzer enthaltenen](sql-database-manage-logins.md) verwenden Zugriff auf Daten ist sichergestellt, dass primäre und sekundäre Datenbanken immer die gleichen Anmeldeinformationen verfügen, damit nach Abschluss der kopieren Sie sofort es mit den gleichen Anmeldeinformationen zugreifen können. Wenn Sie [Azure Active Directory](../active-directory/active-directory-whatis.md)verwenden, können Sie vollständig für die Verwaltung von Anmeldeinformationen in der Kopie erforderlich. Beim Kopieren der Datenbank auf einem neuen Server funktioniert Zugriff Login basierend im Allgemeinen nicht jedoch, da die Benutzernamen auf dem neuen Server nicht vorhanden sind. Finden Sie unter [Sicherheit von SQL Azure-Datenbank nach der Wiederherstellung Verwalten von](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzernamen beim Kopieren einer Datenbank auf einem anderen logischen Server. 

Wenn Sie eine SQL-Datenbank kopieren möchten, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Abonnement Azure lediglich **Kostenlose Testversion** am oberen Rand dieser Seite klicken Sie auf, und klicken Sie dann wieder zum Ende der in diesem Artikel.
- Einer SQL-Datenbank zu kopieren. Wenn Sie nicht mit eine SQL-Datenbank verfügen, erstellen Sie eine folgen die Schritten in diesem Artikel: [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).

## <a name="next-steps"></a>Nächste Schritte

- Kopieren eine Datenbank mit dem Azure-Portal [kopieren eine Azure SQL-Datenbank mit dem Azure-Portal](sql-database-copy-portal.md) finden Sie unter.
- Kopieren eine Datenbank mithilfe der PowerShell [Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell](sql-database-copy-powershell.md) finden Sie unter.
- [Kopieren einer SQL Azure-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md) kopieren eine Datenbank mithilfe von Transact-SQL finden Sie unter.
- Erfahren Sie, [wie SQL Azure-Datenbank Sicherheit nach der Wiederherstellung verwalten](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzern und Benutzernamen, beim Kopieren einer Datenbank auf einem anderen logischen Server.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Verwalten von Benutzernamen](sql-database-manage-logins.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md)
- [Exportieren Sie die Datenbank in einer BACPAC](sql-database-export.md)
- [Business Continuity (Übersicht)](sql-database-business-continuity.md)
- [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)
