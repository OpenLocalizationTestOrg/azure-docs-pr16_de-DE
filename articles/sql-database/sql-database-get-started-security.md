<properties
    pageTitle="SQL-Datenbank-Lernprogramm: Erste Schritte mit der Sicherheit"
    description="Informationen Sie zum Erstellen von Benutzerkonten für den Zugriff auf und zum Verwalten einer Datenbank."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>SQL-Datenbank-Lernprogramm: Erstellen von SQL-Datenbank-Benutzerkonten zugreifen und Verwalten einer Datenbank


> [AZURE.SELECTOR]
- [Erste Schritte-Lernprogramm](sql-database-get-started-security.md)
- [Gewähren des Zugriffs](sql-database-manage-logins.md)

In diesem Lernprogramm erfahren Sie, wie Sie SQL Server Management Studio (SSMS) zu verwenden:

- Melden Sie sich mit SQL-Datenbank mit einem Server Ebene Hauptbenutzer Login.
- Erstellen eines Benutzerkontos SQL-Datenbank.
- Erteilen Sie einer SQL-Datenbank von [Db_owner-Berechtigungen](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0).
- Verbinden Sie mit einer SQL-Datenbank mit einem Benutzerkonto, das nicht auf einem Server Ebene der Tilgungsanteile ist.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie haben dieses Lernprogramms SQL-Datenbank abgeschlossen und Berechtigungen der Benutzer Konto Dbo und ein Benutzerkonto erstellt haben, können Sie weitere Informationen zur [Sicherheit von SQL-Datenbank](sql-database-manage-logins.md).


