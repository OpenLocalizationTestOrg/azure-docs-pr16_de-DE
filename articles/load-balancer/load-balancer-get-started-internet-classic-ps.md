<properties
   pageTitle="Erste Schritte beim Erstellen eines Internet gegenüberliegende Lastenausgleich im klassischen Modus mithilfe der PowerShell | Microsoft Azure"
   description="Informationen Sie zum Erstellen eines Internet gegenüberliegende Lastenausgleich im klassischen Modus mithilfe der PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Erste Schritte beim Erstellen eines Internet gegenüberliegende Lastenausgleich (klassisch) in PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen. Sie können auch an, [wie ein Lastenausgleich mit Azure Ressourcenmanager gegenüberliegende Internet erstellt werden](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Einrichten von Lastenausgleich mithilfe der PowerShell

Um ein Lastenausgleich mithilfe der Powershell eingerichtet haben, führen Sie die folgenden Schritte aus:

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../../articles/powershell-install-configure.md) , und folgen Sie den Anweisungen, ganz nach Ende melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.


2. PowerShell-Cmdlets können Sie nach dem Erstellen eines virtuellen Computers, ein Lastenausgleich eines virtuellen Computers innerhalb derselben Cloud-Dienst hinzufügen.

Fügen Sie im folgenden Beispiel, dass ein Lastenausgleich bezeichnet "Webfarm" in die cloud Service "Mytestcloud" (oder myctestcloud.cloudapp.net) hinzufügen die Endpunkte für den Lastenausgleich, die virtuellen Computern, die mit dem Namen "web1" und "web2" festgelegt. Lastenausgleich ein empfängt Netzwerkverkehr auf Port 80 und Laden von Salden zwischen den virtuellen Computern, die durch den lokalen Endpunkt (in diesem Fall Port 80) definiert TCP verwenden.


### <a name="step-1"></a>Schritt 1
Erstellen Sie einen Endpunkt Lastenausgleich für den ersten virtuellen Computer "web1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Schritt 2

Erstellen Sie einen anderen Endpunkt für den zweiten virtuellen Computer "web2" mit der gleichen laden Lastenausgleich SetName

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Entfernen eines virtuellen Computers aus einem Lastenausgleich

Entfernen-AzureEndpoint können Sie einen Endpunkt virtuellen Computern aus dem Lastenausgleich entfernen

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

Sie können auch [Erste Schritte beim Erstellen eines internen Lastenausgleich](load-balancer-get-started-ilb-classic-ps.md) und welche Art von [Verteilung Modus](load-balancer-distribution-mode.md) für ein Especific laden Lastenausgleich Netzwerk Datenverkehr Verhalten konfigurieren.

Wenn die Anwendung benötigt Verbindungen für Server hinter einem Lastenausgleich beibehalten werden soll, können Sie weitere Informationen zu [im Leerlauf TCP Timeouteinstellungen für ein Lastenausgleich](load-balancer-tcp-idle-timeout.md)verstehen. Es wird hilfreich Verbindung im Leerlauf Verhalten wissen möchten, wenn Sie Lastenausgleich Azure verwenden.

