<properties
    pageTitle="Verschieben von Datenbanken zwischen Servern, zwischen Abonnements sowie ein-und Azure."
    description="QuickSteps zu kopieren, verschieben und Migrieren von Daten und Datenbanken in SQL Azure-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Verschieben von Datenbanken zwischen Servern, zwischen Abonnements sowie ein-und Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Verschieben eine Datenbank auf einem anderen Server im selben-Abonnement
- Im [Portal Azure](https://portal.azure.com)-klicken Sie auf **SQL-Datenbanken**, wählen Sie eine Datenbank aus der Liste aus, und klicken Sie dann auf **Kopieren**. Weitere Details finden Sie unter [kopieren eine SQL Azure-Datenbank](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>Verschieben eine Datenbank zwischen Abonnements
- Im [Portal Azure](https://portal.azure.com)klicken Sie auf **SQL Server** , und wählen Sie dann auf den Server, der die Datenbank aus der Liste hostet. Klicken Sie auf **Verschieben**, und wählen Sie dann den Ressourcen verschieben und das Abonnement zu verschieben.

## <a name="to-migrate-a-sql-database-into-azure"></a>Eine SQL-Datenbank in Azure migrieren
- Ermitteln Sie die Datenbankkompatibilität, und wählen Sie dann die richtigen Migrationsmethode entsprechend Ihren Anforderungen. Führen Sie die Richtlinien und Optionen in [einer SQL Server-Datenbank migrieren](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>So erstellen eine Kopie einer Datenbank für die Verwendung von Azure
- [Exportieren einer BACPAC-Datei.](sql-database-export.md)
