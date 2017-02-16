<properties
   pageTitle="Erstellen Sie mithilfe der PowerShell SQL Data Warehouse | Microsoft Azure"
   description="Erstellen Sie mithilfe der PowerShell SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Erstellen von SQL Data Warehouse mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In diesem Artikel wird gezeigt, wie ein SQL Data Warehouse mithilfe der PowerShell erstellt.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um anzufangen, müssen Sie folgende Aktionen ausführen:

- **Azure-Konto**: Besuchen [Azure kostenlose Testversion][] oder [MSDN Azure Gutschriften][] ein Konto zu erstellen.
- **SQL Azure-Server**: [Erstellen einer logischen Azure SQL-Datenbank-Server mit dem Portal Azure][] oder [Erstellen einer logischen Azure SQL-Datenbank-Server mit PowerShell][] finden Sie weitere Details.
- **Ressourcengruppe**: verwenden die gleichen Ressourcengruppe als dem SQL Azure-Server oder finden Sie unter [erstellen eine Ressourcengruppe][].
- **PowerShell Version 1.0.3 oder größer**: Überprüfen Sie die Version, indem Sie ausführen **Get-Modul - ListAvailable-Namen Azure**.  Die neueste Version kann von [Microsoft Web Platform Installer][]installiert werden.  Weitere Informationen zum Installieren der neuesten Version von Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell][].

> [AZURE.NOTE] Erstellen einer SQL Data Warehouse kann dazu führen, dass eine neue berechenbaren Dienstleistung.  Informationen zur Preisgestaltung finden Sie unter [SQL Data Warehouse Preise][] .

## <a name="create-a-sql-data-warehouse"></a>Erstellen einer SQL Datawarehouse

1. Windows PowerShell zu öffnen.
2. Führen Sie dieses Cmdlet für die Anmeldung an Azure-Ressourcen-Manager.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Wählen Sie das Abonnement, das Sie für die aktuelle Sitzung verwenden möchten.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Erstellen Sie die Datenbank. In diesem Beispiel wird eine Datenbank namens "Mynewsqldw", mit dem Ziel Dienstalter "DW400", auf dem Server mit dem Namen "sqldwserver1", in der Ressourcengruppe mit dem Namen "mywesteuroperesgp1" wird erstellt.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Parameter erforderlich sind:

- **RequestedServiceObjectiveName**: die Menge des [DWU][] , die Sie anfordern.  Unterstützte Werte sind: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 und DW6000.
- **Datenbankname**: der Name des SQL Data Warehouse, die Sie erstellen.
- **ServerName**: den Namen des Servers, mit dem Sie für die Erstellung (muss V12).
- **ResourceGroupName**: Ressourcengruppe verwenden.  So finden Sie die verfügbaren Ressourcen mithilfe Gruppen in Ihrem Abonnement Get-AzureResource.
- **Edition**: "Data Warehouse" muss ein SQL Data Warehouse erstellen.

Optionale Parameter werden:

- **CollationName**: die Standardsortierreihenfolge nicht angegeben ist, SQL_Latin1_General_CP1_CI_AS.  Sortierung kann nicht auf eine Datenbank nicht geändert werden.
- **MaxSizeBytes**: die Standardeinstellung für die maximale Größe einer Datenbank ist 10 GB.


Weitere Informationen zu den Parameteroptionen finden Sie unter [Neu-AzureRmSqlDatabase][] und [Create Database (Azure SQL-Data Warehouse)][].

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss der SQL Data Warehouse provisioning sollten Sie versuchen, [Laden Sie die Beispieldaten][] oder Auschecken wie [entwickeln][], [Laden][]oder [Migrieren][].

Wenn Sie weitere Informationen zum Verwalten von SQL Data Warehouse programmgesteuert interessiert sind, schauen Sie sich unsere Artikel zum [PowerShell-Cmdlets und REST-APIs][]verwenden.

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Migrieren von]: ./sql-data-warehouse-overview-migrate.md
[Entwickeln]: ./sql-data-warehouse-overview-develop.md
[Beim Laden]: ./sql-data-warehouse-load-with-bcp.md
[Laden die Beispieldaten]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell-Cmdlets und REST-APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[So installieren und Konfigurieren von Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Erstellen Sie einen logischen Azure SQL-Datenbankserver mit dem Azure-Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Erstellen Sie einen logischen Azure SQL-Datenbankserver mit PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[So erstellen Sie eine Ressourcengruppe]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Neue AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Erstellen Sie die Datenbank (SQL Azure Datawarehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure kostenlose Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
