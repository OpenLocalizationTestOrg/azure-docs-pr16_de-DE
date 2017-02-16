<properties
 pageTitle="Scheduler PowerShell-Cmdlets Bezug"
 description="Scheduler PowerShell-Cmdlets Bezug"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Scheduler PowerShell-Cmdlets Bezug

In der folgenden Tabelle beschrieben, und links zur Verweisseite der einzelnen in Azure Scheduler die wichtigsten Cmdlets.

Zum Azure PowerShell installieren und ordnen Sie sie Ihr Abonnement Azure finden Sie unter [Informationen zum Installieren und konfigurieren Sie Azure PowerShell](../powershell-install-configure.md). 

Weitere Informationen zu [Cmdlets Azure Ressourcenmanager](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx)finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md).

|Cmdlet|Dieses Cmdlet der Beschreibung|
|---|---|
[Deaktivieren-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Wird eine Position Websitesammlung deaktiviert. 
[Aktivieren-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Ermöglicht eine Auflistung Position.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Ruft Scheduler Aufträge ab.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Ruft Auftrag Websitesammlungen.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Ruft Auftrag Verlauf.
[Neue AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Erstellt eine HTTP-Position.
[Neue AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Erstellt eine Auflistung Position.
[Neue AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Erstellt einen Bus Warteschlange Auftrag an.
[Neue AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Erstellt einen Thema Auftrag Bus an.
[Neue AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Erstellt einen Speicher Warteschlange Auftrag an. 
[Entfernen-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Entfernt einen Scheduler Auftrag.  
[Entfernen-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Entfernt eine Sammlung Position. 
[Set-AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Ändert ein Scheduler HTTP-Projekt.
[Set-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Ändert eine Auflistung Position. 
[Set-AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Ändert einen Bus Warteschlange Auftrag an.  
[Set-AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Ändert einen Thema Auftrag Bus an. 
[Set-AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Ändert die Position eines Speicher Warteschlange an.   

Ausführlichere Informationen können Sie eine der folgenden Cmdlets ausführen: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Siehe auch


 [Was ist der Scheduler?](scheduler-intro.md)

 [Azure Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Grenzwerte für Azure Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
