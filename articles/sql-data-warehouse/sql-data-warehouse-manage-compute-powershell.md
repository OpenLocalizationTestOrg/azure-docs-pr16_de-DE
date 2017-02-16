<properties
   pageTitle="Verwalten von berechnen Power in Azure SQL-Data Warehouse (PowerShell) | Microsoft Azure"
   description="PowerShell-Aufgaben zum Verwalten von Power zu berechnen. Maßstab berechnen Ressourcen durch DWUs anpassen. Oder anhalten und fortsetzen Ressourcen zum Speichern von Kosten zu berechnen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Verwalten von berechnen Power in Azure SQL-Data Warehouse (PowerShell)

> [AZURE.SELECTOR]
- [(Übersicht)](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skalierung der Leistung durch Skalierung berechnen, Ressourcen und Arbeitsspeicher, um die Notwendigkeit, Ändern der Arbeitsbelastung entsprechen. Speichern von Kosten durch Skalierung zurück Ressourcen während nicht Höchstwert Zeiten oder Anhalten berechnen ganz. 

Diese Sammlung von Aufgaben verwendet im Azure-Portal an:

- Maßstab berechnen
- Anhalten berechnen
- Berechnen des Lebenslaufs

Informationen hierzu finden Sie Informationen finden Sie unter [Übersicht über verwalten zu berechnen][].


## <a name="before-you-begin"></a>Vorbemerkung

### <a name="install-the-latest-version-of-azure-powershell"></a>Installieren der neuesten Version von Azure PowerShell

> [AZURE.NOTE]  Wenn mit SQL Data Warehouse Azure PowerShell verwenden möchten, benötigen Sie Azure PowerShell Version 1.0.3 oder höher.  Zur Überprüfung Ihrer aktuellen Version führen Sie den Befehl **Get-Modul - ListAvailable-Namen Azure**. Sie können die neueste Version von [Microsoft Web Platform Installer][]installieren.  Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Erste Schritte mit Azure PowerShell-cmdlets

Erste Schritte:

1. Azure PowerShell zu öffnen. 
2. PowerShell dazu aufgefordert werden, führen Sie diese Befehle zum Azure-Manager anmelden, und wählen Sie Ihr Abonnement.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Maßstab berechnen power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Um die DWUs zu ändern, verwenden Sie das [Set-AzureRmSqlDatabase][] PowerShell-Cmdlet ein. Im folgende Beispiel: Legt das Ebene Ziel Dienst auf DW1000 für die Datenbank MySQLDW, die auf dem Server MeinServer gehostet wird. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Anhalten berechnen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Um eine Datenbank zu anzuhalten, verwenden Sie das Cmdlet [Standby-AzureRmSqlDatabase][] . Im folgende Beispiel wird eine Datenbank auf einem Server mit dem Namen Server01 gehostet Database02 namens angehalten. Der Server ist in einer Azure Ressourcengruppe mit dem Namen ResourceGroup1. 

> [AZURE.NOTE] Notiz, die Ihr Server foo.database.windows.net, ist "Foo" als – ServerName in der PowerShell-Cmdlets verwenden.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Eine Variation, ruft in diesem Beispiel nächste die Datenbank in das Objekt $database ab. Klicken Sie dann leitet sie das Objekt [Standby-AzureRmSqlDatabase][]. Die Ergebnisse werden in das Objekt ResultDatabase gespeichert. Der Befehl abgeschlossen zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Berechnen des Lebenslaufs

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie zum Starten einer Datenbank das Cmdlet [Lebenslauf-AzureRmSqlDatabase][] ein. Im folgende Beispiel wird eine Datenbank auf einem Server mit dem Namen Server01 gehostet Database02 namens gestartet. Der Server ist in einer Azure Ressourcengruppe mit dem Namen ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Eine Variation, ruft in diesem Beispiel nächste die Datenbank in das Objekt $database ab. Klicken Sie dann das Objekt [Lebenslauf-AzureRmSqlDatabase][] pipes, und speichert die Ergebnisse in $resultDatabase. Der Befehl abgeschlossen zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Übersicht über die Verwaltung][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Verwaltung (Übersicht)]: ./sql-data-warehouse-overview-manage.md
[So installieren und Konfigurieren von Azure PowerShell]: ./powershell-install-configure.md
[Verwalten von berechnen (Übersicht)]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Lebenslauf-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Anhalten AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
