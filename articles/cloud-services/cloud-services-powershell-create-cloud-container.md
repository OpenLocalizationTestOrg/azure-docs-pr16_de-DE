<properties
   pageTitle="Erstellen Sie einen Cloud-Dienstcontainer mit PowerShell | Microsoft Azure"
   description="In diesem Artikel wird erläutert, wie einen Cloud-Dienstcontainer mit PowerShell zu erstellen. Der Container hostet Web- und Worker Rollen."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Verwenden eines Azure PowerShell-Befehls zum Erstellen eines leeren Cloud-Dienst Containers
In diesem Artikel wird erläutert, wie Sie schnell einen Cloud Services-Container mit Azure PowerShell-Cmdlets erstellen. Führen Sie die folgenden Schritte aus:

1. Installieren Sie das Microsoft Azure PowerShell-Cmdlet aus [Azure PowerShell](http://aka.ms/webpi-azps) -Downloadseite.
2. Öffnen Sie die PowerShell-Eingabeaufforderung.
3. Verwenden der [Hinzufügen-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) anmelden.

    > [AZURE.NOTE] Weitere Anweisung Installieren des Azure PowerShell-Cmdlets und Herstellen einer Verbindung mit Ihrem Abonnement Azure finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

4. Verwenden Sie das Cmdlet " **New-AzureService** ", um einer leeren Azure-Cloud-Dienstcontainer erstellen.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Führen Sie in diesem Beispiel wird zum Aufrufen des Cmdlets aus:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Weitere Informationen zum Erstellen von Azure-Cloud-Dienst führen Sie zu können aus:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Nächste Schritte

 * Zum Verwalten der bereitstellungs der Cloud-Dienst finden Sie in die Befehle [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Entfernen-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)und [AzureService festlegen](https://msdn.microsoft.com/library/azure/dn495242.aspx) . Sie können auch auf [wie Cloud Services konfiguriert](cloud-services-how-to-configure.md) Weitere Informationen verweisen.

 * Um Ihr Projekt Cloud-Dienst auf Azure veröffentlichen möchten, lesen Sie in im Beispiel **PublishCloudService.ps1** [kontinuierlichen Bereitstellung für in Azure-Cloud-Dienst](cloud-services-dotnet-continuous-delivery.md).
