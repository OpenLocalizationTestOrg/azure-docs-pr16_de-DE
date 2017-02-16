<properties
   pageTitle="PowerShell-Cmdlets für Azure SQL-Data Warehouse"
   description="Finden Sie die obersten PowerShell-Cmdlets für Azure SQL-Data Warehouse einschließlich anhalten und Fortsetzen einer Datenbank ein."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse

Viele SQL Data Warehouse Verwaltungsaufgaben können mit Azure PowerShell-Cmdlets oder REST APIs verwaltet werden.  Nachfolgend finden Sie einige Beispiele zum PowerShell-Befehle verwenden, um allgemeine Aufgaben in der SQL-Data Warehouse automatisieren.  Einige Beispiele für gute REST finden Sie im Artikel [Verwalten Skalierbarkeit mit weiteren][].

> [AZURE.NOTE]  Mit SQL Data Warehouse Azure PowerShell verwenden möchten, benötigen Sie Azure PowerShell Version 1.0.3 oder höher.  Überprüfen Sie die Version, indem Sie ausführen **Get-Modul - ListAvailable-Namen Azure**.  Die neueste Version kann von [Microsoft Web Platform Installer][]installiert werden.  Weitere Informationen zum Installieren der neuesten Version von Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Erste Schritte mit Azure PowerShell-cmdlets

1. Windows PowerShell zu öffnen. 
2. PowerShell dazu aufgefordert werden, führen Sie diese Befehle zum Azure-Manager anmelden, und wählen Sie Ihr Abonnement.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Anhalten SQL Datawarehouse Beispiel

Zeigen Sie eine Datenbank mit dem Namen "Database02" gehostet auf einem Server mit dem Namen "Server01."  Der Server ist in einer Azure Ressourcengruppe mit dem Namen "ResourceGroup1". 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Eine Variation, leitet in diesem Beispiel wird das abgerufene Objekt [Standby-AzureRmSqlDatabase][]an.  Daher ist die Datenbank angehalten. Der Befehl abgeschlossen zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Starten Sie SQL Datawarehouse Beispiel

Wiederaufnahme des Betriebs einer Datenbank mit dem Namen "Database02" gehostet auf einem Server mit dem Namen "Server01." Der Server ist in einer Ressourcengruppe mit dem Namen "ResourceGroup1." enthalten.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Eine Variation, ruft in diesem Beispiel wird eine Datenbank namens "Database02" von einem Server mit dem Namen "Server01", die in einer Ressourcengruppe mit dem Namen "ResourceGroup1." enthalten sind Es leitet das abgerufene Objekt [Lebenslauf-AzureRmSqlDatabase][].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Notiz, die Ihr Server foo.database.windows.net, ist "Foo" als – ServerName in der PowerShell-Cmdlets verwenden.

## <a name="frequently-used-powershell-cmdlets"></a>Häufig verwendete PowerShell-cmdlets

Diese PowerShell-Cmdlets werden häufig mit Azure SQL-Data Warehouse verwendet.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Neue AzureRmSqlDatabase][]
- [Entfernen-AzureRmSqlDatabase][]
- [Wiederherstellen-AzureRmSqlDatabase][] 
- [Lebenslauf-AzureRmSqlDatabase][]
- [Wählen Sie-AzureRmSubscription][]
- [Set-AzureRmSqlDatabase][]
- [Anhalten AzureRmSqlDatabase][]

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Beispiele für PowerShell finden Sie unter:

- [Erstellen einer SQL Data Warehouse mithilfe der PowerShell][]
- [Wiederherstellen der Datenbank][]

Eine Liste aller Aufgaben, die mit PowerShell automatisierte werden können, finden Sie unter [Azure SQL-Datenbank-Cmdlets][].  Eine Liste der Aufgaben, die mit weiteren automatisierte werden können, finden Sie unter [für Azure SQL-Datenbanken][].

<!--Image references-->

<!--Article references-->
[So installieren und Konfigurieren von Azure PowerShell]: ./powershell-install-configure.md
[Erstellen einer SQL Data Warehouse mithilfe der PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Wiederherstellen der Datenbank]: ./sql-data-warehouse-restore-database-powershell.md
[Verwalten von Skalierbarkeit mit weiteren]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Cmdlets für SQL Azure-Datenbank]: https://msdn.microsoft.com/library/mt574084.aspx
[Für SQL Azure-Datenbanken]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Neue AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Entfernen-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Wiederherstellen-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Lebenslauf-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Wählen Sie-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Anhalten AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
