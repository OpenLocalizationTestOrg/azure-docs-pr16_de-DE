<properties
    pageTitle="Legt automatisch skalieren Linux virtuellen Computern skalieren | Microsoft Azure"
    description="Richten Sie automatische Skalierung für eine Linux virtuellen Computern skalieren Set mithilfe von Azure CLI"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a>Skalieren Sie automatisch Linux-Computern in einer Gruppe von virtuellen Computern skalieren

Virtuellen Computern skalieren Sätze erleichtern das Bereitstellen und Verwalten von identischen virtuellen Computern als einen Satz. Maßstab Sätze Bereitstellen eines Layers hochgradig skalierbare und anpassbare berechnen für Applikationen Hyperscale und Windows-Plattform Bilder, Linux Plattform Bilder, benutzerdefinierte Bilder und Erweiterungen unterstützen. Weitere Informationen finden Sie unter [Übersicht des virtuellen Computern Umfang Sätze](virtual-machine-scale-sets-overview.md).

In diesem Lernprogramm erfahren Sie, wie zum Erstellen einer Skala Menge der virtuelle Linux-Computer die neueste Version von Ubuntu Linux verwenden. Das Lernprogramm erfahren Sie auch so skaliert automatisch die Computer festlegen. Sie erstellen die Skalierung festlegen und durch eine Ressourcenmanager Azure-Vorlage erstellen und Bereitstellen von Azure CLI mit Skalierung einrichten. Weitere Informationen zu Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](../resource-group-authoring-templates.md). Wenn Sie weitere Informationen zur automatischen Skalierung der Skalierung Mengen finden Sie unter [Automatische Skalierung und virtuellen Computern Maßstab legt fest](virtual-machine-scale-sets-autoscale-overview.md).

In diesem Lernprogramm stellen Sie die folgenden Ressourcen und Erweiterungen:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Weitere Informationen zu Ressourcen Ressourcenmanager finden Sie unter [Azure zu berechnen, Netzwerk und Speicherdienstanbieter unter Azure-Manager](../virtual-machines/virtual-machines-linux-compare-deployment-models.md).

Vor beginnen Sie mit den Schritten in diesem Lernprogramm [Azure CLI installieren](../xplat-cli-install.md).

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a>Schritt 1: Erstellen einer Ressourcengruppe und Speicher-Konto

1. **Melden Sie sich bei Microsoft Azure** - Ihrer Line-Benutzeroberfläche (Bash, Terminal, Befehl ' auffordern), wechseln Sie zur Ressourcenmanager Modus, und klicken Sie dann [Melden Sie sich mit Ihrer Arbeit oder Schule Id](../xplat-cli-connect.md#use-the-log-in-method). Folgen Sie den Anweisungen für eine interaktive Anmeldung Erfahrung mit Ihrem Azure-Konto an.

        azure config mode arm

        azure login

    > [AZURE.NOTE] Wenn Sie verfügen über eine Arbeit oder Schule ID, und Sie nicht zweifaktorielle Varianzanalyse Authentifizierung aktiviert haben, verwenden Sie `azure login -u` die ID, ohne eine interaktive Sitzung anmelden. Wenn Sie keine ID Schule oder eine geschäftlichen haben, können Sie [Erstellen einer Arbeit oder Schule Id aus Ihrem persönlichen Microsoft-Konto](../virtual-machines/virtual-machines-linux-create-aad-work-id.md)an.

2. **Erstellen einer Ressourcengruppe** – alle Ressourcen einer Ressourcengruppe bereitgestellt werden müssen. In diesem Lernprogramm benennen Sie die Ressource Gruppe **vmsstest1**.

        azure group create vmsstestrg1 centralus

3. **Bereitstellen eines Kontos Speicher in die neue Ressourcengruppe** – dieses Speicherkonto ist die Vorlage gespeichert ist. Erstellen eines Namens **Vmsstestsa**Speicher-Konto an.

        azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa

## <a name="step-2-create-the-template"></a>Schritt 2: Erstellen der Vorlage
Eine Vorlage Ressourcenmanager Azure ermöglicht es Ihnen das Bereitstellen und verwalten Azure Ressourcen zusammen mithilfe einer JSON-Beschreibung der Ressourcen und zugeordneten Bereitstellungsparameter.

1. Erstellen Sie in Ihrem bevorzugten Editor die Datei VMSSTemplate.json, und fügen Sie die ursprüngliche JSON-Struktur zur Unterstützung der Vorlage.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Parameter sind nicht immer erforderlich, aber sie bieten eine Möglichkeit, um Werte einzugeben, wenn die Vorlage bereitgestellt wird. Fügen Sie diese Parameter unter der Parameter übergeordnetes Element, das Sie der Vorlage hinzugefügt.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Legen Sie ein Namen für die separaten virtuellen Computern, die zum Zugreifen auf der Computern in den Maßstab verwendet wird.
    - Einen Namen für das Speicherkonto, in dem die Vorlage gespeichert ist.
    - Die Anzahl der Instanzen des virtuellen Computern So erstellen Sie zunächst in den Maßstab festlegen.
    - Einen Namen und das Kennwort für das Administratorkonto auf den virtuellen Computern.
    - Legen Sie ein Namenspräfix für die Ressourcen, die erstellt werden, um die Skalierung zu unterstützen.

3. In einer Vorlage können Variablen verwendet werden, um anzugeben, Werte, die häufig geändert werden können oder Werte, die aus einer Kombination von Parameterwerte erstellt werden müssen. Fügen Sie diese Variablen unter der Variablen übergeordnetes Element, das Sie der Vorlage hinzugefügt.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
        "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

    - DNS-Namen, die durch die Netzwerk-Schnittstellen verwendet werden.
    - Die Namen der IP-Adresse und Präfixe für die virtuelle Netzwerk und Subnetze.
    - Laden die Namen und Bezeichner des virtuellen Netzwerks, Lastenausgleich und Netzwerkschnittstellen.
    - Festlegen von Speicher Kontonamen für die Konten, auf den Computern in den Maßstab zugeordnet.
    - Einstellungen für die Diagnose Erweiterung, die auf den virtuellen Computern installiert ist. Weitere Informationen über die Erweiterung Diagnose finden Sie unter [Erstellen eines Windows virtuelle Computer mit für die Überwachung und Diagnose Azure Ressourcenmanager Vorlage verwenden](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).

4. Fügen Sie Speicher Konto Ressource unter der Ressourcen übergeordnetes Element, das Sie der Vorlage hinzugefügt. Diese Vorlage verwendet eine Schleife, die empfohlenen fünf Speicherkonten erstellen, in dem die Betriebssystem-Datenträger und diagnostic Daten gespeichert. Mit dieser Gruppe von Konten kann bis zu 100 virtuellen Computern in einer Gruppe von Farben-Skala unterstützen, also das aktuelle Maximum. Jedes Storage-Konto heißt mit einem Buchstaben, die in den Variablen kombiniert mit dem Suffix, das Sie in den Parametern für die Vorlage bereitzustellen definiert wurde.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Fügen Sie die Ressource virtuelles Netzwerk hinzu. Weitere Informationen finden Sie unter [Netzwerk-Anbieter für Ressourcen](../virtual-network/resource-groups-networking.md).

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Fügen Sie die öffentlichen IP-Adressenressourcen, die von den Lastenausgleich und Netzwerk-Benutzeroberfläche verwendet werden.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. Fügen Sie laden Lastenausgleich Ressource, die von der Skalierung festlegen verwendet wird. Weitere Informationen finden Sie unter [Azure Ressourcenmanager Unterstützung für Lastenausgleich](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 22
                }
              }
            ]
          }
        },

8. Hinzufügen der Netzwerk Benutzeroberflächen-Ressource, die von der separaten virtuellen Computern verwendet wird. Da Computern in einer Gruppe von Farben-Skala nicht über eine öffentliche IP-Adresse zugänglich sind, wird eine separate virtuellen Computern die Computer in der gleichen virtuellen Netzwerk remote Access erstellt.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Hinzufügen der separaten virtuellen Computern im gleichen Netzwerk als Maßstab festlegen.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "14.04.4-LTS",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                }
              ]
            }
          }
        },

10. Fügen Sie hinzu, dass die Skalierung virtuellen Computern Ressource festgelegt, und geben Sie die Diagnose-Erweiterung, die auf allen virtuellen Computern Skalieren festlegen installiert ist. Viele der Einstellungen für diese Ressource sind vergleichbar mit virtuellen Computern Ressource. Die wichtigsten Unterschiede sind die Element Kapazität, das angibt, die Anzahl der virtuellen Computern Skalieren festlegen und UpgradePolicy, die angibt, wie Updates auf virtuellen Computern ausgeführt werden. Skalierung festlegen wird nicht erstellt, bis alle Speicherkonten erstellt werden mit dem Element DependsOn wie angegeben.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "Canonical",
                      "offer": "UbuntuServer",
                      "sku": "14.04.4-LTS",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name":"LinuxDiagnostic",
                        "properties": {
                          "publisher":"Microsoft.OSTCExtensions",
                          "type":"LinuxDiagnostic",
                          "typeHandlerVersion":"2.1",
                          "autoUpgradeMinorVersion":false,
                          "settings": {
                            "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint":"https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Hinzufügen der AutoscaleSettings Ressourcen, die definiert, wie die Skalierung Menge basierend auf Prozessorverwendung auf den Computern in der Gruppe passt.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor\\PercentProcessorTime",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    In diesem Lernprogramm werden diese Werte:

    - **MetricName** - dieser Wert entspricht der Leistung Zähler, den wir in der Variablen Wadperfcounter definiert ist. Verwenden diese Variable, sammelt die Erweiterung Diagnose den Zähler **Processor\PercentProcessorTime** aus.
    - **MetricResourceUri** - dieser Wert wird den Resource Identifier des virtuellen Computers Maßstab festlegen.
    - **TimeGrain** – ist dieser Wert die Genauigkeit der die Metrik, die erfasst wurden. In dieser Vorlage wird es in eine Minute um festgelegt.
    - **Statistik** – dieser Wert bestimmt, wie der Metrik, die kombiniert werden, um die automatische Skalierung Aktion aufnehmen zu können. Mögliche Werte sind: Mittelwert, Min, Max. In dieser Vorlage werden die durchschnittliche total CPU-Auslastung des den virtuellen Computern erfasst.
    - **Zeitfenster** – ist dieser Wert den Zellbereich, der Uhrzeit in dem Instanzdaten erfasst werden. Zwischen 5 Minuten und 12 Stunden Wert muss liegen.
    - **TimeAggregation** – dieser Wert bestimmt, wie die gesammelten Daten über einen Zeitraum kombiniert werden sollen. Der Standardwert ist Mittelwert. Mögliche Werte sind: Mittelwert, Minimum, Maximum, letzten, Summe, Anzahl.
    - **Operator** – ist dieses Werts den Operator, mit dem die metrischen Daten und den Schwellenwert für den Vergleich. Mögliche Werte sind: gleich, NotEquals, größer als, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **Schwellenwert** – löst diesen Wert eine Aktion skalieren. In dieser Vorlage werden Autos Maßstab festlegen, wenn die durchschnittliche CPU-Auslastung zwischen Computern festlegen 50 % überschreitet hinzugefügt.
    - **Richtung** – bestimmt dieser Wert die Aktion, die ausgeführt wird, wenn der Schwellenwert erreicht ist. Die möglichen Werte sind erhöhen oder verringern. In dieser Vorlage wird die Anzahl von virtuellen Computern Skalieren festlegen erhöht, wenn der Schwellenwert 50 % in das festgelegte Zeitfenster überschreitet.
    - **Typ** – ist dieser Wert den Typ der Aktion, die erfolgen sollte und muss auf ChangeCount festgelegt sein.
    - **Wert** – ist dieser Wert die Anzahl der virtuellen Computern, die hinzugefügt oder aus den Maßstab entfernt werden. Dieser Wert muss 1 oder größer sein. Der Standardwert ist 1. In dieser Vorlage festlegen die Anzahl der Computer in die Skalierung erhöht von 1 an, wenn der Schwellenwert erfüllt ist.
    - **Cooldown** – ist dieser Wert die Zeitspanne seit der letzten Anpassungsbereich für Aktion warten, bevor die nächste Aktion auftritt. Dieser Wert muss zwischen einer Minute und eine Woche liegen.

12. Speichern Sie die Vorlagendatei an.    

## <a name="step-3-upload-the-template-to-storage"></a>Schritt 3: Hochladen der Vorlage in Speicher

Die Vorlage kann hochgeladen werden, solange Sie kennen, den Namen und den Primärschlüssel der Speicher Firma, die Sie in Schritt 1 erstellt haben.

1. In der Benutzeroberfläche Ihrer Line (Bash, Terminal, Befehlszeile), führen Sie diese Befehle zum Festlegen der Umgebungsvariablen zum Zugreifen auf des Speicherkontos erforderlich sind:

        export AZURE_STORAGE_ACCOUNT={account_name}
        export AZURE_STORAGE_ACCESS_KEY={key}

    Sie können die Taste durch Klicken auf das Symbol Key beim Anzeigen der Ressource Konto Speicher Azure-Portal erhalten. Wenn Sie eine Windows-Befehlszeile verwenden möchten, geben Sie **festlegen** , statt exportieren aus.

2. Erstellen Sie den Container für die Vorlage speichern.

        azure storage container create -p Blob templates

3. Hochladen der neuen Container.

        azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json

## <a name="step-4-deploy-the-template"></a>Schritt 4: Bereitstellen der Vorlage

Jetzt, da Sie die Vorlage erstellt haben, können Sie beginnen, die Ressourcen bereitstellen. Verwenden Sie diesen Befehl zum Starten des Prozesses ein:

    azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1

Wenn Sie drücken eingeben, Sie werden aufgefordert, die Werte für die Variablen bereitzustellen, die Ihnen zugewiesen. Geben Sie diese Werte an:

    vmName: vmsstestvm1
    vmSSName: vmsstest1
    instanceCount: 5
    adminUserName: vmadmin1
    adminPassword: VMpass1
    resourcePrefix: vmsstest

Es dauert etwa 15 Minuten für alle Ressourcen in erfolgreich bereitgestellt werden.

>[AZURE.NOTE]Möglichkeit des Portals können Sie auch die Ressourcen bereitgestellt. Verwenden Sie diesen Link: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>

## <a name="step-5-monitor-resources"></a>Schritt 5: Überwachen von Ressourcen

Sie können einige Informationen zu virtuellen Computern skalieren Datensätze mit diesen Methoden erhalten:

 - Azure-Portal - können Sie aktuell eine begrenzte Menge von Informationen mit dem Portal erhalten.
 - Der [Azure Ressource Explorer](https://resources.azure.com/) - ist dieses Tool am besten für untersuchen den aktuellen Status der Skalierung zurück. Folgen Sie diesem Pfad, und Sie auftreten der Instanzansicht Maßstab festlegen, die Sie erstellt haben:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure CLI - verwenden Sie diesen Befehl, um einige Informationen zu erhalten:

        azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1

 - Herstellen einer Verbindung mit Jumpbox virtuellen Computers so, wie Sie möchten, dass alle anderen Computer und dann Sie den virtuellen Computern in den Maßstab festlegen, um einzelne Prozesse überwachen Remote zugreifen können.

>[AZURE.NOTE]Eine vollständige REST-API zum Abrufen von Informationen zu skalieren Sätze finden Sie im [Virtuellen Computern Maßstab legt fest](https://msdn.microsoft.com/library/mt589023.aspx).

## <a name="step-6-remove-the-resources"></a>Schritt 6: Entfernen der Ressourcen

Da Ihnen für Ressourcen, die in Azure berechnet werden, ist es immer empfiehlt sich das Löschen von Ressourcen, die nicht mehr benötigt werden. Sie müssen nicht jede Ressource aus einer Ressourcengruppe separat zu löschen. Sie können die Ressourcengruppe löschen und alle zugeordneten Ressourcen werden automatisch gelöscht.

        azure group delete vmsstestrg1

## <a name="next-steps"></a>Nächste Schritte

- Finden Sie Beispiele für Azure Monitor Überwachungsfeatures in [Azure Monitor Plattformen CLI Symbolleiste Starten von Beispielen](../monitoring-and-diagnostics/insights-cli-samples.md)
- Erfahren Sie mehr über Features der Benachrichtigung in [automatisch skalieren Aktionen zum Senden von e-Mail- und Webhook-Benachrichtigung in Azure Monitor verwenden](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
- Erfahren Sie, wie Sie [Überwachungsprotokolle zum Senden von e-Mail- und Webhook-Benachrichtigung in Azure Monitor verwenden](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Schauen Sie sich die [automatisch skalieren virtueller Computer Skalierung festlegen Ausführen einer app Ubuntu/Apache/PHP](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) -Vorlage, die von einem Stapel LAMPE zum Ausführen die automatische Skalierung Funktionen von virtuellen Computern Maßstab legt fest legt.
