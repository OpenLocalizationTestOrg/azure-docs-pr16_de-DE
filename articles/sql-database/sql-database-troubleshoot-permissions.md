<properties
    pageTitle="So Administratoraufgaben führen, z. B. zurücksetzen Administratorkennworts | Microsoft Azure"
    description="Beschreibt, wie häufig durchgeführte Verwaltungsaufgaben in SQL-Datenbank. Beispielsweise zurücksetzen Administratorkennworts, gewähren und Entfernen von Access aus."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="Zurücksetzen des Administratorkennworts"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Wie häufig durchgeführte Verwaltungsaufgaben z. B. Zurücksetzen von Administratorkennworts in Azure SQL-Datenbank
Verwenden Sie dieses Thema für QuickSteps gewähren und Entfernen von Access mit einer SQL Azure-Datenbank. Ausführlichere Informationen finden Sie unter:

- [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md)
- [Sichern Ihrer SQL-Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbank-Engine und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Zurücksetzen des Administratorkennworts für einen logischen server

- Im [Portal Azure](https://portal.azure.com) klicken Sie auf **SQL Server**, wählen Sie den Server aus der Liste aus, und klicken Sie dann auf **Kennwort zurücksetzen**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Um Hilfe, stellen Sie sicher, dass nur autorisierte-IP-Adressen dürfen Zugriff auf den server
- Finden Sie unter [wie: Konfigurieren der Firewall-Einstellungen auf SQL-Datenbank](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>In der enthaltenen Datenbankbenutzer in der Datenbank erstellen
- Verwenden Sie die Anweisung [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) und [Enthaltenen Datenbankbenutzer - ausführenden Ihrer Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx)angezeigt.

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Für die Benutzerauthentifizierung mithilfe Ihrer Azure Active Directory enthaltenen Datenbank
- Finden Sie unter [Herstellen einer Verbindung mit einer SQL-Datenbank mithilfe von Azure-Active Directory-Authentifizierung](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>So erstellen Sie zusätzliche Benutzernamen für Benutzer mit hoher Berechtigung in der virtuelle master-Datenbank
- Verwenden Sie die [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx) -Anweisung, und finden Sie im Abschnitt Verwalten von Benutzernamen der [Verwaltung von Datenbanken und Benutzernamen in SQL Azure-Datenbank](sql-database-manage-logins.md) für mehr Details.
