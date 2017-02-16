<properties
    pageTitle="Erstellen einer Linux VM mithilfe einer Vorlage Azure | Microsoft Azure"
    description="Erstellen einer Linux VM auf Azure mithilfe einer Vorlage Azure Ressourcenmanager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Erstellen einer Linux VM mithilfe einer Vorlage Azure

In diesem Artikel wird gezeigt, wie schnell eine Linux virtuellen Computern auf Azure bereitstellen mithilfe einer Vorlage Azure.  Im Artikel erfordert:

- ein Azure-Konto ([kostenlose Testversion erhalten](https://azure.microsoft.com/pricing/free-trial/)).

- die [Azure CLI](../xplat-cli-install.md) in angemeldet `azure login`.

- Azure CLI _muss_ Ressourcenmanager Azure-Modus `azure config mode arm`.

Sie können auch schnell Linux VM Vorlage mithilfe der [Azure-Portal](virtual-machines-linux-quick-create-portal.md)bereitstellen.

## <a name="quick-command-summary"></a>Zusammenfassung der Symbolleiste Befehle

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Vorlagen können Sie zum Erstellen von virtuellen Computern auf Azure mit Einstellungen, die Sie beim Start anpassen möchten, wie Benutzernamen und Hostnamen Einstellungen. In diesem Artikel werden wir mit Anschluss 22 öffnen eine Azure-Vorlage mit einer Ubuntu VM zusammen mit einer Netzwerksicherheitsgruppe (NSG) für SSH starten.

Azure Ressourcenmanager Vorlagen sind JSON-Dateien, die für einfache einmaligen Aufgaben wie das Starten einer Ubuntu VM, als erledigt in diesem Artikel verwendet werden können.  Azure Vorlagen können auch verwendet werden, um komplexe Azure Konfigurationen für gesamte Umgebungen wie ein testen, Entwickler oder Herstellung Bereitstellung Stapel zu erstellen.

## <a name="create-the-linux-vm"></a>Erstellen Sie den virtuellen Linux Computer

Im folgenden Code wird gezeigt, wie Nummer `azure group create` eine Ressourcengruppe erstellen und Bereitstellen einer SSH-gesicherte Linux VM mithilfe [dieser Vorlage Azure Ressourcenmanager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)gleichzeitig. Denken Sie daran, die in Ihrem Beispiel benötigten Namen verwendet, die in Ihrer Umgebung eindeutig sind. In diesem Beispiel wird `myResourceGroup` als den Namen der Ressource Gruppe, und `myVM` als den Namen des virtuellen Computers.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Die Ausgabe sollte wie den folgenden Ausgabeblock aussehen:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Dieses Beispiel bereitgestellt, einem virtuellen Computer mithilfe der `--template-uri` Parameter.  Sie können auch herunterladen, oder erstellen Sie eine Vorlage lokal, und übergeben die Vorlage mit den `--template-file` Parameter mit einem Pfad zu der Vorlagendatei als Argument. Die CLI Azure fordert Sie für die Parameter, die durch die Vorlage erforderlich.

## <a name="next-steps"></a>Nächste Schritte

Suchen Sie im [Vorlagenkatalog](https://azure.microsoft.com/documentation/templates/) zum erfahren Sie, welche app Framework weiter bereitstellen.
