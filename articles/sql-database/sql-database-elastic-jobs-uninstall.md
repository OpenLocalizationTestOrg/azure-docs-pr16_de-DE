<properties 
    pageTitle="So deinstallieren Sie flexible Aufträge Datenbanktool" 
    description="So deinstallieren Sie das flexible Aufträge Datenbanktool" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

#<a name="uninstall-elastic-database-jobs-components"></a>Flexible Aufträge Datenbankkomponenten deinstallieren
**Flexible Datenbank Aufträge** Komponenten können mit dem Portal oder PowerShell deinstalliert werden.

##<a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Deinstallieren Sie flexible Aufträge Datenbankkomponenten über das Azure-portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/)an.
2. Navigieren Sie zu des Abonnements, die enthält **flexible Datenbank Aufträge** Komponenten, nämlich des Abonnements, in dem flexible Aufträge Datenbankkomponenten installiert wurden.
3. Klicken Sie auf **Durchsuchen** , und klicken Sie auf **Ressourcengruppen**.
4. Wählen Sie aus der Ressourcengruppe mit dem Namen "__ElasticDatabaseJob".
5. Löschen Sie die Ressourcengruppe aus.

##<a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Deinstallieren Sie flexible Aufträge Datenbankkomponenten mithilfe der PowerShell

1.  Starten ein Microsoft Azure PowerShell-Befehl-Fensters, und navigieren Sie zu der untergeordnete Verzeichnis von Tools unter dem Ordner Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: Geben Sie ein **cd-Tools**.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2.  Führen Sie die.\UninstallElasticDatabaseJobs.ps1 PowerShell-Skript ein.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

Oder einfach, führen Sie Folgendes Skript, Standard Voraussetzung Werte, wobei die Installation Komponenten verwendet:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager
        
        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }
        
        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Nächste Schritte

Um flexible Datenbank Aufträge erneut zu installieren, finden Sie unter [Installieren des flexible Datenbank Auftrag-Diensts](sql-database-elastic-jobs-service-installation.md)

Eine Übersicht über flexible Datenbank Einzelvorgänge finden Sie unter [flexible Datenbank Aufträge Übersicht](sql-database-elastic-jobs-overview.md).

<!--Image references-->

 
