<properties
    pageTitle="Linux-Knoten in Azure Stapel Pools | Microsoft Azure"
    description="Erfahren Sie, wie Ihre parallele berechnen Auslastung auf Speicherpools in Stapel Azure-virtuellen Computern Linux Verarbeitungszeit."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Bereitstellung von Linux Datenverarbeitungsknoten in Azure Stapel pools

Azure Stapel können Sie parallele berechnen Auslastung auf Linux und Windows-virtuellen Computern ausgeführt werden. In diesem Artikel ausführlich erläutert, wie Sie mithilfe von sowohl den [Stapel Python] Speicherpools Linux Datenverarbeitungsknoten im Dienst Stapel zu erstellen[ py_batch_package] und [Stapel .NET] [ api_net] Client-Bibliotheken.

> [AZURE.NOTE] [Anwendungspakete](batch-application-packages.md) werden derzeit nicht unterstützt auf Datenverarbeitungsknoten Linux.

## <a name="virtual-machine-configuration"></a>Konfiguration des virtuellen Computers

Wenn Sie einen Pool von Computeknoten in Stapel erstellen, stehen zwei Optionen aus der Knotengröße und Betriebssystem ausgewählt: Cloud Services-Konfiguration und Konfiguration des virtuellen Computers.

**Cloud Services-Konfiguration** bietet Windows Knoten *nur*zu berechnen. Verfügbare berechnen Knoten Größen sind in [Größen für Cloud Services](../cloud-services/cloud-services-sizes-specs.md), und [Azure Gast-BS-Versionen und SDK Kompatibilitätsmatrix](../cloud-services/cloud-services-guestos-update-matrix.md)verfügbaren Betriebssysteme aufgelistet. Wenn Sie einem Ressourcenpool, die Azure Cloud Services-Knoten enthält erstellen, müssen Sie nur die Knotengröße und "OS Eigentums," die in den oben erwähnten Artikeln zu finden sind. Für Windows Speicherpools Hierarchieknoten zu berechnen, ist die Cloud Services am häufigsten verwendet.

**Konfiguration des virtuellen Computers** stellt Linux und Windows-Bilder für das Berechnen von Knoten. Verfügbare berechnen Knoten Größen werden in [Größen für virtuellen Computern in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) und [Größen für virtuellen Computern in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) aufgelistet. Wenn Sie einem Ressourcenpool, die Konfiguration des virtuellen Computers Knoten enthält erstellen, müssen Sie die Größe des Knoten, den Bezug des virtuellen Computers Bild und den Stapel Knoten Agent SKU auf den Knoten installiert werden angeben.

### <a name="virtual-machine-image-reference"></a>Bild-Referenz virtuellen Computern

Der Dienst Stapel verwendet [virtuellen Computern Maßstab legt fest](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) , um Linux Datenverarbeitungsknoten zu ermöglichen. Betriebssystemabbilder für diesen virtuellen Computern sind nach dem [Azure Marketplace]bereitgestellten[vm_marketplace]. Wenn Sie einen virtuellen Computern Bildverweis konfigurieren, geben Sie die Eigenschaften eines Bilds virtuellen Computern Marketplace. Die folgenden Eigenschaften sind erforderlich, wenn Sie einen virtuellen Computern Bildverweis erstellen:

| **Bezug von Bildeigenschaften** | **Beispiel** |
| ----------------- | ------------------------ |
| Publisher         | Kanonische                |
| Angebot             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| Version           | neueste                   |

> [AZURE.TIP] Sie erhalten weitere Informationen zu diesen Eigenschaften und wie Marketplace Bilder in [Navigieren und select Linux virtuellen Computern Bilder in Azure mit CLI oder PowerShell](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md)aufgelistet. Beachten Sie, dass nicht alle Marketplace Bilder mit Stapel aktuell kompatibel sind. Weitere Informationen finden Sie unter [Knoten Agent SKU](#node-agent-sku).

### <a name="node-agent-sku"></a>Knoten Agent SKU

Der Stapel-Knoten-Agent ist ein Programm, das auf den einzelnen Knoten im Pool ausgeführt wird und stellt die Benutzeroberfläche Befehl and Control zwischen dem Knoten und den Stapel-Dienst. Es gibt verschiedene Implementierungen des Knoten Agents, bekannt als Lagerhaltungsdaten, für verschiedene Betriebssysteme. Beim Erstellen einer Konfiguration des virtuellen Computers im Wesentlichen Sie zuerst den virtuellen Computern Bild Bezug angeben, und geben Sie dann den Knoten Agent, klicken Sie auf das Bild zu installieren. Normalerweise ist jeder Knoten Agent SKU mit mehreren virtuellen Computern Bildern kompatibel. Hier sind einige Beispiele für Knoten Agent SKUs:

* Batch.Node.Ubuntu 14.04
* Batch.Node.CentOS 7
* Batch.Node.Windows amd64

> [AZURE.IMPORTANT] Nicht alle Bilder von virtuellen Computern, die auf dem Markt verfügbar sind, sind mit den aktuell verfügbaren Stapel Knoten Agents kompatibel. Sie müssen den Stapel SDKs verwenden, um die Liste der verfügbaren Knoten Agent SKUs und die virtuellen Computern Bilder, mit denen kompatibel sind. Die [Liste der virtuellen Computern Bilder](#list-of-virtual-machine-images) später in diesem Artikel Weitere Informationen finden Sie unter.

## <a name="create-a-linux-pool-batch-python"></a>Erstellen einer Linux Ressourcenpool: Stapel Python

Der folgende Codeausschnitt zeigt ein Beispiel für die Verwendung der [Microsoft Azure Stapel Clientbibliothek für Python] [ py_batch_package] zu einem Ressourcenpool Ubuntu Servers berechnen Hierarchieknoten zu erstellen. Dokumentation zur für das Modul Stapel Python finden Sie unter [azure.batch-Paket] [py_batch_docs] beim Lesen der Dokumente.

Dieser Codeausschnitt erstellt ein [ImageReference] [ py_imagereference] explizit und gibt die einzelnen Verbindungseigenschaften (Herausgeber, Angebot, SKU, Version). In Herstellung Code, jedoch, empfiehlt es sich, dass Sie die [List_node_agent_skus] verwenden[ py_list_skus] Methode, um zu ermitteln, und wählen Sie aus den verfügbaren Bild- und Knoten Agent SKU Kombinationen zur Laufzeit.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Wie zuvor schon erwähnt, wir empfehlen, die anstelle der [ImageReference] [ py_imagereference] explizit, verwenden Sie die [List_node_agent_skus] [ py_list_skus] Methode dynamisch aus der aktuell unterstützten Knoten Agent/Marketplace Bild Kombinationen auswählen. Im folgende Python Codeausschnitt zeigt die Verwendung dieser Methode.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Erstellen einer Linux Ressourcenpool: Stapel .NET

Der folgende Codeausschnitt zeigt ein Beispiel für so verwenden Sie den [Stapel .NET] [ nuget_batch_net] Clientbibliothek mit einem Ressourcenpool Ubuntu Servers berechnen Hierarchieknoten zu erstellen. Finden Sie die [Dokumentation zur Stapel .NET] [ api_net] auf MSDN.

Im folgenden Codeausschnitt verwendet die [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] Methode zum Wählen Sie in der Liste der aktuell unterstützt Marketplace Bild- und Knoten Agent SKU Kombinationen. Diese Methode empfiehlt sich, da die Liste der unterstützten Kombinationen von Zeit zu Zeit ändern kann. In den meisten Fällen sind die unterstützte Kombinationen hinzugefügt.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Obwohl in der vorherige Ausschnitt der [PoolOperations]verwendet[net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] Methode dynamisch Liste, und wählen Sie aus unterstützten Bild- und Knoten Agent SKU Kombinationen (empfohlen), Sie können auch eine [ImageReference] konfigurieren[ net_imagereference] explizit:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Liste der Bilder virtuellen Computern

Die folgende Tabelle enthält die Bilder der Marketplace-virtuellen Computern, die mit den verfügbaren Stapel Knoten Agents kompatibel sind, wenn Sie in diesem Artikel zuletzt aktualisiert wurde. Es ist wichtig, beachten Sie, dass diese Liste nicht endgültigen ist, da Bilder und Knoten Agents hinzugefügt oder zu einem beliebigen Zeitpunkt entfernt werden können. Wir empfehlen, Ihre Stapel Anwendungen und Dienste immer [List_node_agent_skus] mit[ py_list_skus] (Python) und [ListNodeAgentSkus] [ net_list_skus] (Stapel .NET) zu ermitteln, und wählen Sie aus der aktuell verfügbaren SKUs.

> [AZURE.WARNING] Die folgende Liste möglicherweise zu einem beliebigen Zeitpunkt ändern. Verwenden Sie die **Liste Knoten Agent SKU** Methoden zur Verfügung, in den Stapel APIs immer Liste, und wählen Sie dann aus dem kompatibel virtuellen Computern und Knoten Agent SKUs, wenn Sie Ihre Stapelverarbeitung.

| **Publisher** | **Angebot** | **Abbildung SKU** | **Version** | **Knoten Agent SKU-ID** |
| ------- | ------- | ------- | ------- | ------- |
| Kanonische | UbuntuServer | 14.04.0-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.1-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.2-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.3-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.4-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 14.04.5-LTS | neueste | Batch.Node.Ubuntu 14.04 |
| Kanonische | UbuntuServer | 16.04.0-LTS | neueste | Batch.Node.Ubuntu 16.04 |
| Credativ | Debian | 8 | neueste | Batch.Node.Debian 8 |
| OpenLogic | CentOS | 7.0 | neueste | Batch.Node.CentOS 7 |
| OpenLogic | CentOS | 7.1 | neueste | Batch.Node.CentOS 7 |
| OpenLogic | CentOS-HPC | 7.1 | neueste | Batch.Node.CentOS 7 |
| OpenLogic | CentOS | 7.2 | neueste | Batch.Node.CentOS 7 |
| Oracle | Oracle-Linux | 7.0 | neueste | Batch.Node.CentOS 7 |
| SUSE | openSUSE | 13.2 | neueste | Batch.Node.opensuse 13.2 |
| SUSE | OpenSUSE-Sprung | 42.1 | neueste | Batch.Node.opensuse 42.1 |
| SUSE | SLES-HPC | 12 | neueste | Batch.Node.opensuse 42.1 |
| SUSE | SLES | 12-SP1 | neueste | Batch.Node.opensuse 42.1 |
| Microsoft-anzeigen | Standard Daten Wissenschaft virtueller Computer | Standard Daten Wissenschaft virtueller Computer | neueste | Batch.Node.Windows amd64 |
| Microsoft-anzeigen | Linux Daten Wissenschaft virtueller Computer | linuxdsvm | neueste | Batch.Node.CentOS 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | neueste | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | neueste | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | neueste | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows-Server-technischen-Vorschau | neueste | Batch.Node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Verbinden mit Linux Knoten

Während der Entwicklung oder bei der Problembehandlung können Sie es erforderlich, melden Sie sich zu den Knoten in der Ressourcenpool finden. Im Gegensatz zu Windows berechnen Knoten können Sie mit Linux Knoten verbunden (Remotedesktopprotokoll) nutzen. Stattdessen kann der Stapel Dienst SSH-Zugriff auf den einzelnen Knoten für remote-Verbindung.

Im folgende Python Codeausschnitt erstellt einen Benutzer auf den einzelnen Knoten in einem Ressourcenpool, die für den remote-Verbindung erforderlich ist. Die Verbindungsinformationen secure Shell (SSH) für die einzelnen Knoten gedruckt.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Hier ist die Ausgabe für den vorherigen Code für einen Pool, der vier Linux Knoten enthält:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Beachten Sie, dass anstelle eines Kennworts, Sie beim Erstellen eines Benutzers auf einem Knoten einen SSH öffentlichen Schlüssel angeben können. In der SDK Python dazu verwenden den Parameter **Ssh_public_key** auf [ComputeNodeUser][py_computenodeuser]. In .NET geschieht dies mithilfe der [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] Eigenschaft.

## <a name="pricing"></a>Preise

Azure Stapel basiert auf Azure Cloud Services und Azure-virtuellen Computern Technologie. Der Stapel-Dienst selbst wird kostenlos, angeboten, was, dass Sie nur für die Ressourcen berechnen belastet werden bedeutet, dass Ihre Stapel Lösungen nutzen. Wenn Sie **Cloud Services-Konfiguration**auswählen, Sie werden in Rechnung gestellt basierend auf die [Cloud Services Preise] [ cloud_services_pricing] Struktur. Wenn Sie die **Konfiguration des virtuellen Computers**auswählen, Sie werden in Rechnung gestellt basierend auf den [virtuellen Computern Preise] [ vm_pricing] Struktur.

## <a name="next-steps"></a>Nächste Schritte

### <a name="batch-python-tutorial"></a>Stapel Python Lernprogramm

Ein umfassender Lernprogramm dazu, wie Sie in Stapel mit Python arbeiten checken Sie [Erste Schritte mit der Azure Stapel Python-Client aus](batch-python-tutorial.md). Seine Companion [Code Stichprobe] [ github_samples_pyclient] enthält eine Funktion Helper `get_vm_config_for_distro`, zeigt, dass ein anderes Verfahren zum Konfiguration des virtuellen Computers zu erhalten.

### <a name="batch-python-code-samples"></a>Stapel Python Codebeispielen

Schauen Sie sich die anderen [Python Fehlercode Beispiele] [ github_samples_py] [Azure-Stapel-Beispiele] [ github_samples] Repository auf GitHub für mehrere Skripts, die zeigen, wie Sie allgemeine Stapel Operationen wie Ressourcenpool, Position und Erstellen von Tasks. Die [Infos zu] [ github_py_readme] , die die Python begleitet Beispiele enthält ausführliche Informationen zum Installieren von die erforderlichen Pakete.

### <a name="batch-forum"></a>Stapel-forum

Das [Azure Stapel Forum] [ forum] auf MSDN finden Sie hervorragende Stapel diskutieren, und stellen Sie Fragen zu dem Dienst. Lesen Sie Beiträge hilfreich "Stickied", und stellen Sie Ihre Fragen auftretende, während Sie Ihre Lösungen Stapel erstellen.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
