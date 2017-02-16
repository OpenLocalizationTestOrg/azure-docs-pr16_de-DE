<properties
   pageTitle="Den ACS-Cluster mit Azure CLI skalieren | Microsoft Azure"
   description="Verfahren zum Skalieren Ihrer Azure Container Dienst Clusters CLI Azure verwenden."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Container, Micro-Dienste Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Skalieren eines Containers Azure-Diensts

Sie können sich die Anzahl der Knoten skalieren Azure Container Service (ACS) wird mit dem Tool Azure CLI. Wenn Sie die CLI Azure skalieren verwenden, gibt das Tool Sie eine neue Konfigurationsdatei, die die Änderung angewendet werden, um den Container darstellt.

## <a name="about-the-command"></a>Informationen zu den Befehl

Die CLI Azure muss im Ressourcenmanager Azure-Modus für die Interaktion mit Azure Container. Sie können zum Ressourcenmanager Modus wechseln, indem Sie aufrufen `azure config mode arm`. Die `acs` Befehl befindet sich mit dem Namen der untergeordneten-Befehl `scale` , die die Skalierung Vorgänge für einen Container Dienst unterstützt. Gelangen Sie Hilfe zu den verschiedenen Parametern in den Befehl Skalierung durch die Ausführung verwendete `azure acs scale --help`, welche gibt etwa so:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Verwenden Sie den Befehl zum Skalieren

Um einen Container Dienst skalieren zu können, müssen Sie zuerst wissen, dass der **Ressourcengruppe** und den **Namen der Azure Container Service (ACS)**, und geben Sie auch die neue Anzahl von Agents. Sie können mithilfe einer Menge kleineren oder höheren skalieren Hilfethemas oben oder unten.

Möglicherweise möchten welche die aktuelle Anzahl von Agents für einen Container Dienst kennen, bevor Sie skalieren. Verwenden der `azure acs show <resource group> <ACS name>` Befehl aus, um den ACS-Config zurückzukehren. Beachten Sie das Ergebnis <mark>zählen</mark> .

#### <a name="see-current-count"></a>Finden Sie unter aktuelle zählen

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Neue zählen skalieren

Wie es wahrscheinlich bereits klar ist, können Sie den Container Dienst durch Aufrufen skalieren `azure acs scale` und **Ressourcengruppe**, **ACS-Name**und **Agent zählen**angeben. Wenn Sie einen Container Dienst skalieren, gibt Azure CLI eine JSON-Zeichenfolge, die die neue Konfiguration des Diensts Container, einschließlich der Anzahl der neuen Agent darstellt.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen eines Clusters](container-service-deployment.md)