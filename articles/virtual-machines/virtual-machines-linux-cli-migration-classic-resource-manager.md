<properties
    pageTitle="Migrieren IaaS Ressourcen von klassischen Ressourcenmanager zu Azure mithilfe von Azure CLI | Microsoft Azure"
    description="In diesem Artikel durchläuft der Plattform unterstützt Migration von Ressourcen aus dem klassischen Ressourcenmanager zu Azure mithilfe von Azure CLI"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migrieren Sie IaaS Ressourcen von klassischen Ressourcenmanager zu Azure mithilfe von Azure CLI

Diese Schritte zeigen, wie mit Azure line Interface (CLI) Befehle Infrastruktur als eine Service (IaaS) Ressourcen aus dem Bereitstellungsmodell klassischen zum Bereitstellungsmodell Azure Ressourcenmanager migriert. Im Artikel erfordert die [Azure CLI](../xplat-cli-install.md).

>[AZURE.NOTE] Alle Vorgänge, die hier beschriebenen sind Idempotent. Wenn Sie ein Problem als ein nicht unterstütztes Feature oder einem Konfigurationsfehler verfügen, es empfiehlt sich, dass Sie zur Vorbereitung, wiederholen Sie Abbrechen oder Vorgang abzuschließen. Die Plattform versucht dann erneut.

## <a name="step-1-prepare-for-migration"></a>Schritt 1: Bereiten Sie für die Migration vor

Hier sind einige bewährte Methoden, die es empfiehlt sich, wie Sie migrieren von IaaS Ressourcen aus klassischen zu Ressourcenmanager ausgewertet werden soll:

- Lesen Sie durch die [Liste der Features oder nicht unterstützte Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). Wenn Sie virtuellen Computern, die nicht unterstützte Konfigurationen oder Features verwenden haben, wird empfohlen, für das Feature/Konfiguration bekannt gegeben zu warten. Alternativ können Sie diese Features entfernen oder Verschieben von dieser Konfiguration Migration zu aktivieren, wenn sie Ihren Bedürfnissen entspricht.
-   Wenn Sie Skripts, die Ihre Infrastruktur und Applikationen heute bereitstellen automatisch haben, versuchen Sie es so erstellen Sie ein ähnliche Test-Setup mithilfe dieser Skripts für die Migration. Alternativ können Sie Stichprobe Umgebungen einrichten, mithilfe des Azure-Portals.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Schritt 2: Legen Sie Ihr Abonnement und registrieren den Anbieter

Für Migrationsszenarien, müssen Sie Ihre Umgebung für beide klassischen einzurichten und Ressourcenmanager. [Azure CLI installieren](../xplat-cli-install.md) , und [Wählen Sie Ihr Abonnement](../xplat-cli-connect.md).

Anmelden Sie bei Ihrem Konto.
    
    azure login

Wählen Sie das Abonnement Azure mit dem folgenden Befehl aus.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Die Registrierung ist eine Uhrzeit Schritt, aber es einmal, bevor Sie versuchen, die Migration ausgeführt werden soll. Ohne Registrierung sehen Sie die folgende Fehlermeldung angezeigt 

>   *BadRequest: Abonnement ist für die Migration nicht registriert.* 

Registrieren Sie sich mit dem folgenden Befehl mit der Migrations-Anbieter für Ressourcen. Beachten Sie, dass in einigen Fällen wird dieser Befehl Timeout erreicht. Die Registrierung werden erfolgreich.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Warten Sie fünf Minuten für die Registrierung auf Fertig stellen. Sie können den Status der Genehmigung überprüfen, indem Sie mit dem folgenden Befehl. Stellen Sie sicher, dass RegistrationState ist `Registered` bevor Sie fortfahren.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Wechseln Sie nun CLI, um die `asm` Modus.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Schritt 3: Stellen Sie sicher, dass Sie der Azure Region Ihrer aktuellen Bereitstellung oder VNET genügend Kerne Azure Ressourcenmanager virtuellen Computern haben

Für diesen Schritt müssen Sie zum Umschalten auf `arm` Modus. Führen Sie diese mit den folgenden Befehl aus.

```
azure config mode arm
```

Den folgenden CLI-Befehl können den aktuellen Betrag der Kerne zu überprüfen, die Sie in Azure Ressourcenmanager haben. Weitere Informationen zum Core Kontingenten finden Sie unter [Grenzwerte und Azure Ressourcenmanager](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Sobald Sie fertig sind dieses Schritts überprüft haben, können Sie wieder zu wechseln `asm` Modus.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Schritt 4: Option 1 - Migration virtueller Computer in einen Cloud-Dienst 

Abrufen der Liste der Cloud-Diensten mit den folgenden Befehl aus, und wählen Sie dann in des Cloud-Diensts, den Sie migrieren möchten. Beachten Sie, dass, wenn Sie die virtuellen Computern in der Cloud-Dienst in einem virtuellen Netzwerk sind, oder wenn sie Web/Worker-Rollen aufweisen, Sie eine Fehlermeldung angezeigt erhalten.

    azure service list

Führen Sie den folgenden Befehl Sie den Namen für den Clouddienst in die ausführliche abrufen. In den meisten Fällen ist der Bereitstellung-Name den Namen der Cloud-Dienst entspricht.

    azure service show <serviceName> -vv

Bereiten Sie den virtuellen Computern in der Cloud-Dienst für die Migration vor. Sie haben zwei Optionen zur Auswahl.

Wenn Sie die virtuellen Computern zu einem Plattform erstellten virtuellen Netzwerk migrieren möchten, verwenden Sie den folgenden Befehl aus.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Wenn Sie zu einem vorhandenen virtuellen Netzwerk im Bereitstellungsmodell Ressourcenmanager migrieren möchten, verwenden Sie den folgenden Befehl aus.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Nachdem die vorbereiten erfolgreich ist, suchen Sie über die ausführliche erhalten den Migrationsstatus von der virtuellen Maschinen, und stellen Sie sicher, dass sie in sind die `Prepared` Zustand.

    azure vm show <vmName> -vv

Überprüfen Sie die Konfiguration für die vorbereiteten Ressourcen mithilfe von CLI oder Azure-Portal an. Wenn Sie nicht für die Migration noch – und Sie in der alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl aus.

    azure service deployment abort-migration <serviceName> <deploymentName>

Wenn die vorbereitete Konfiguration zusagt, können Sie nach vorne verschieben und die Ressourcen mithilfe des folgenden Befehls abzuschließen.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Schritt 4: Option 2 - virtuellen Computern in einem Netzwerk virtuelle migrieren

Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. Beachten Sie, dass das virtuelle Netzwerk Web/Worker-Rollen oder virtuellen Computern mit nicht unterstützten Konfigurationen enthält, Sie eine Fehlermeldung bei Überprüfung erhalten.

Erhalten Sie alle virtuellen Netzwerke in das Abonnement mit dem folgenden Befehl ein.

    azure network vnet list
    
Die Ausgabe sieht ungefähr wie folgt aus:

![Screenshot der Befehlszeile mit dem gesamten virtuellen Netzwerknamen hervorgehoben.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Im obigen Beispiel ist die **VirtualNetworkName** der gesamte Name **"Gruppe classicubuntu16 classicubuntu16"**.

Bereiten Sie das virtuelle Netzwerk Ihrer Wahl für die Migration mit dem folgenden Befehl ein.

    azure network vnet prepare-migration <virtualNetworkName>

Überprüfen Sie die Konfiguration für die vorbereiteter virtuellen Computer mithilfe von CLI oder Azure-Portal an. Wenn Sie nicht für die Migration noch – und Sie in der alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl aus.

    azure network vnet abort-migration <virtualNetworkName>

Wenn die vorbereitete Konfiguration zusagt, können Sie nach vorne verschieben und die Ressourcen mithilfe des folgenden Befehls abzuschließen.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Schritt 5: Migrieren einer Speicher-Konto

Sobald Sie fertig sind migrieren den virtuellen Computern, wir empfehlen Sie Speicher-Konto migrieren.

Vorbereiten des Speicherkontos für die Migration mit dem folgenden Befehl

    azure storage account prepare-migration <storageAccountName>

Überprüfen Sie die Konfiguration für das Speicherkonto kann begonnen mit CLI oder Azure-Portal an. Wenn Sie nicht für die Migration noch – und Sie in der alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl aus.

    azure storage account abort-migration <storageAccountName>

Wenn die vorbereitete Konfiguration zusagt, können Sie nach vorne verschieben und die Ressourcen mithilfe des folgenden Befehls abzuschließen.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Nächste Schritte

- [Plattform unterstützt Migration IaaS Ressourcen aus klassischen zu Ressourcenmanager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Technische aneignen auf Plattform unterstützt Migration von Classic zu Ressourcenmanager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
