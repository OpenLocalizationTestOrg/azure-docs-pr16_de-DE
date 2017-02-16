<properties
   pageTitle="Erstellen Sie ein SQL Datawarehouse mit TSQL | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer Azure SQL-Data Warehouse mit TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Erstellen Sie eine SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In diesem Artikel wird das Erstellen einer SQL Data Warehouse mithilfe von T-SQL veranschaulicht.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um anzufangen, müssen Sie folgende Aktionen ausführen: 

- **Azure-Konto**: Besuchen [Azure kostenlose Testversion][] oder [MSDN Azure Gutschriften][] ein Konto zu erstellen.
- **SQL Azure-Server**: [Erstellen einer logischen Azure SQL-Datenbank-Server mit dem Portal Azure][] oder [Erstellen einer logischen Azure SQL-Datenbank-Server mit PowerShell][] finden Sie weitere Details.
- **Ressourcengruppe**: verwenden die gleichen Ressourcengruppe als dem SQL Azure-Server oder finden Sie unter [erstellen eine Ressourcengruppe][].
- **Zur Ausführung von T-SQL-Umgebung**: Sie können [Visual Studio][Installing Visual Studio and SSDT], [Sqlcmd][]oder [SSMS][] auszuführende T-SQL.

> [AZURE.NOTE] Erstellen einer SQL Data Warehouse kann dazu führen, dass eine neue berechenbaren Dienstleistung.  Informationen zur Preisgestaltung finden Sie unter [SQL Data Warehouse Preise][] .

## <a name="create-a-database-with-visual-studio"></a>Erstellen einer Datenbank mit Visual Studio

Wenn Sie mit Visual Studio vertraut sind, finden Sie im Artikel [Abfrage Azure SQL Data Warehouse (Visual Studio)][].  Um zu beginnen, öffnen Sie SQL Server-Objekt-Explorer in Visual Studio und Verbinden mit dem Server, der Data Warehouse SQL-Datenbank gespeichert wird.  Nachdem die Verbindung hergestellt wurde, können Sie eine SQL Data Warehouse durch Ausführen des folgenden SQL-Befehls für die **master** Datenbank erstellen.  Dieser Befehl erstellt die Datenbank MySqlDwDb mit einem Dienst Ziel DW400 und die Datenbank auf eine maximale Größe von 10 TB vergrößert zulassen.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Erstellen einer Datenbank mit sqlcmd

Alternativ können Sie denselben Befehl mit Sqlcmd ausführen, indem Sie die folgenden über die Befehlszeile ausführen.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Die Standardsortierreihenfolge Wenn nicht angegeben ist, SQL_Latin1_General_CP1_CI_AS sortiert werden sollen.  Die `MAXSIZE` zwischen 250 GB und 240 TB werden können.  Die `SERVICE_OBJECTIVE` kann zwischen DW100 und DW2000 [DWU][].  Eine Liste aller gültigen Werte finden Sie unter MSDN-Dokumentation für die [Datenbank erstellen][].  Die MAXSIZE und die SERVICE_OBJECTIVE können mit einem T-SQL-Befehl [ALTER DATABASE][] geändert werden.  Die Sortierung einer Datenbank kann nach der Erstellung geändert werden.   Ändern der SERVICE_OBJECTIVE als veränderbare DWU verursacht einen Neustart von Diensten, die alle Abfragen in Flight bricht ab, sollte vorsichtig verwendet werden.  Ändern der MAXSIZE werden nicht Dienste neu gestartet ungeändert einen einfachen Metadatenvorgang.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Ihr SQL Data Warehouse wurde bereitgestellt, Sie können [Laden Beispieldaten][] oder Auschecken wie [entwickeln][], [Laden][]oder [Migrieren][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Abfrage SQL Azure Datawarehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[Migrieren von]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Beim Laden]: sql-data-warehouse-overview-load.md
[Laden von Beispieldaten]: sql-data-warehouse-load-sample-databases.md
[Erstellen Sie einen logischen Azure SQL-Datenbankserver mit dem Azure-Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Erstellen Sie einen logischen Azure SQL-Datenbankserver mit PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[So erstellen Sie eine Ressourcengruppe]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[ERSTELLEN DER DATENBANK]: https://msdn.microsoft.com/library/mt204021.aspx
[ÄNDERN DER DATENBANK]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure kostenlose Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
