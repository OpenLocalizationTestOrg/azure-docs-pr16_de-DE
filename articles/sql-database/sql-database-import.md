<properties
    pageTitle="Eine BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank importieren | Microsoft Azure"
    description="Erstellen einer SQL Azure-Datenbank durch Importieren einer vorhandenen BACPAC Datei an."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/31/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="import-a-bacpac-file-to-create-an-azure-sql-database"></a>Importieren einer BACPAC-Datei zum Erstellen einer SQL Azure-Datenbank


**Einzelne Datenbank**

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Dieser Artikel enthält Anweisungen zum Erstellen einer SQL Azure-Datenbank aus einer BACPAC-Datei, die mit der [Azure-Portal](https://portal.azure.com)an.

Eine BACPAC ist eine .bacpac-Datei, die ein Datenbankschema und die Daten enthält. Die Datenbank wird aus einer BACPAC importiert aus einem Speicher Azure Blob-Container erstellt. Wenn Sie eine Datei .bacpac in Azure-Speicher besitzen, können Sie eine gemäß die Anweisungen in [Erstellen und Exportieren eines BACPAC einer Azure SQL-Datenbank](sql-database-export.md)erstellen.


> [AZURE.NOTE] Azure SQL-Datenbank automatisch erstellt und verwaltet Sicherungskopien für jeden Benutzerdatenbank, die Sie wiederherstellen können. Details finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md).


Zum Importieren einer SQL­Datenbank aus einer .bacpac benötigen Sie Folgendes:

- Ein Azure-Abonnement. 
- Ein Azure SQL-Datenbank V12-Server. Wenn Sie nicht mit einen V12 Server verfügen, erstellen Sie eine folgen die Schritten in diesem Artikel: [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).
- Eine Datei .bacpac der Datenbank, die Sie in einem Blob-Container [(standard) Speicher Azure-Konto](../storage/storage-create-storage-account.md) importieren möchten.

> [AZURE.IMPORTANT] Verwenden Sie beim Importieren einer BACPAC aus Azure Blob-Speicher standard-Speicher. Importieren einer BACPAC von Premium Speicher wird nicht unterstützt.


## <a name="select-the-server-to-host-the-database"></a>Wählen Sie den Server zum Hosten der Datenbank

Öffnen Sie das SQL Server-Blade:

1.  Wechseln Sie zum [Azure-Portal](https://portal.azure.com)an.
2.  Klicken Sie auf **SQL Server**.
3.  Klicken Sie auf dem Server zum Wiederherstellen der Datenbank in.
4.  Klicken Sie in der SQL Server-Blade auf **Datenbank importieren** , um das Blade **Datenbank importieren** zu öffnen:

    ![-Datenbank importieren][1]

1.  Klicken Sie auf **Speichern** und wählen Sie Ihre Speicher-Konto, Blob Container und .bacpac Datei klicken Sie auf **OK**.

    ![Konfigurieren Sie Speicheroptionen][2]

1.  Wählen Sie die Preisgestaltung Ebene für die neue Datenbank aus, und klicken Sie auf **auswählen**. Importieren einer Datenbank direkt in eine flexible Ressourcenpool wird nicht unterstützt, jedoch können Sie zunächst in einer einzelnen Datenbank importieren und dann die Datenbank in einem Ressourcenpool verschieben.

    ![Wählen Sie Preisgestaltung Ebene][3]

1.  Geben Sie einen **Namen der Datenbank** für die Datenbank, die Sie aus der Datei BACPAC erstellen.
2.  Wählen Sie den Authentifizierungstyp aus, und geben Sie die Authentifizierungsinformationen für den Server. 
3.  Klicken Sie auf **Erstellen** , um die Datenbank aus der BACPAC zu erstellen.

    ![Erstellen der Datenbank][4]

**Erstellen** auf sendet die Anforderung einer importieren Datenbank an den Dienst. Abhängig von der Größe der Datenbank möglicherweise der Importvorgang einige Zeit in Anspruch nehmen.

## <a name="monitor-the-progress-of-the-import-operation"></a>Überwachen Sie den Fortschritt des Importvorgangs

1.  Klicken Sie auf **SQL Server**.
2.  Klicken Sie auf dem Server, die, dem Sie auf wiederherstellen.
3.  Klicken Sie auf **Import/Export-Verlauf**, in der SQL Server-vorher, in dem Bereich Vorgänge:

    ![Importieren von exportieren Verlauf][5]
    ![exportieren Verlauf importieren][6]





## <a name="verify-the-database-is-live-on-the-server"></a>Stellen Sie sicher, dass die Datenbank auf dem Server live ist

1.  Klicken Sie auf **SQL-Datenbanken** , und überprüfen Sie, ob die Datenbank **Online**ist.



## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Herstellen einer Verbindung mit und einer importierten SQL-Datenbank Abfragen finden Sie unter [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md)


<!--Image references-->
[1]: ./media/sql-database-import/import-database.png
[2]: ./media/sql-database-import/storage-options.png
[3]: ./media/sql-database-import/pricing-tier.png
[4]: ./media/sql-database-import/create.png
[5]: ./media/sql-database-import/import-history.png
[6]: ./media/sql-database-import/import-status.png
