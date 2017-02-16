<properties
    pageTitle="Behandlung von Bundesstaat in Ressourcenmanager Vorlagen | Microsoft Azure"
    description="Empfohlene Vorgehensweisen für den Einsatz von komplexer Objekten Zustandsdaten mit Azure Ressourcenmanager und verknüpfte Vorlagen gemeinsam zeigt"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Freigabe von Status in Azure Ressourcenmanager Vorlagen

In diesem Thema werden bewährte Methoden für die Verwaltung und Freigabe von Status in Vorlagen. Die Parameter und Variablen angezeigt, die in diesem Thema werden Beispiele für den Typ der Objekte, die Sie definieren können Ihren Anforderungen Bereitstellung bequem zu organisieren. In diesen Beispielen können Sie Ihre eigenen Objekte mit Immobilienwerte implementieren, die für Ihre Umgebung sinnvoll.

Dieses Thema ist Teil einer größeren Whitepaper. Um die vollständige Papier zu lesen, herunterladen Sie [World Class Ressource Manager Vorlagen Aspekte und bewährte Methoden](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Bereitstellen von standard-Konfigurations-Einstellungen

Statt eine Vorlage anbieten, die gesamte Flexibilität und viele Variationen bereitstellt, ist ein übliches Schema zu zur Auswahl der bekannten Konfigurationen. Benutzer können, z. B. Sandkasten-, kleine, mittlere und große standard T-shirt-Größen auswählen. Weitere Beispiele für T-shirt-Größen sind Produktangebote, wie z. B. Community Edition oder Enterprise Edition. In anderen Fällen es möglicherweise Arbeitsbelastung-spezifische Konfigurationen einer Technologie – wie Karte verringern oder keine Sql.

Bei komplexen Objekten können Variablen, die Datenmengen, auch als "Eigenschaftensammlungen" bezeichnet aufzubewahren erstellen und diese Daten verwenden, um die Ressource Deklaration in der Vorlage zu fördern. Dieser Ansatz bietet gute, bekannte Konfigurationen mit unterschiedlichen Größen, die vorkonfiguriert sind für Kunden. Ohne bekannte möchten müssen Benutzer der Vorlage Cluster Ziehpunkt auf eigene ermitteln, Faktor in Plattform Ressource Einschränkungen und führen Sie mathematische zum Identifizieren der resultierende Aufteilen von Speicherkonten und anderen Ressourcen (aufgrund Cluster Größe und Ressourcen Einschränkungen). Einige bekannte Konfigurationen sind leichter zu machen, optimal für den Kunden, hilft Ihnen der Dichtefunktion eine höhere Ebene vorführen und unterstützt wird.

Im folgenden Beispiel wird veranschaulicht, wie Variablen zu definieren, die komplexe Objekte für die Darstellung von Sammlungen Daten enthalten. Die Sammlungen definieren Werte, die für die Größe des virtuellen Computers, deren Einstellungen im Netzwerk, Einstellungen für das Betriebssystem und Verfügbarkeit Einstellungen verwendet werden.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Beachten Sie, dass die Variable **TshirtSize** verkettet T-shirt-Größe, die Sie über einen Parameter (**klein**, **Mittel**, **Groß**) bereitgestellt, die Text **TshirtSize**. Sie verwenden diese Variable, um die zugehörigen komplexe Objektvariable für diese T-shirt Größe abzurufen.

Sie können dann diese Variablen weiter unten in der Vorlage verweisen. Die Möglichkeit zum Verweisen auf benannte Variablen und ihre Eigenschaften vereinfacht die Vorlagensyntax und erleichtert das Kontext zu verstehen. Im folgenden Beispiel wird definiert, eine Ressource zum Bereitstellen von mithilfe der Objekte, die zuvor zum Festlegen von Werten angezeigt wird. Angenommen, die Größe des virtuellen Computer durch Abrufen des Werts für festgelegt ist `variables('tshirtSize').vmSize` während er sich den Wert für die Größe des entnommen `variables('tshirtSize').diskSize`. Darüber hinaus der URI für verknüpfte Vorlage festgelegt ist, mit dem Wert für `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Übergeben Status zu einer Vorlage

Sie freigeben Zustand in eine Vorlage über Parameter, die Sie während der Bereitstellung bereitstellen.

Die folgende Tabelle enthält häufig verwendete Parameter in Vorlagen.

Namen | Wert | Beschreibung
---- | ----- | -----------
Speicherort    | Eine Zeichenfolge aus einer eingeschränkten Liste Azure Regionen   | Die Position, wo die Ressourcen bereitgestellt werden.
storageAccountNamePrefix    | Zeichenfolge    | Eindeutigen DNS-Namen für das Speicher-Konto, bei dem des virtuellen Computers Datenträger platziert werden
Domänenname  | Zeichenfolge    | Domänennamen von der öffentlich zugänglichen Jumpbox virtueller Computer im Format: **{Domänenname}. {} Speicherort}.cloudapp.com** zum Beispiel: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Zeichenfolge    | Der Benutzername für den virtuellen Computern
adminPassword   | Zeichenfolge    | Kennwort für die virtuellen Computern
tshirtSize  | Zeichenfolge aus einer eingeschränkten Liste von Angeboten T-shirt-Größen   | Benannte Maßstab Maßeinheit für den bereitstellen. Beispielsweise "Klein", "Mittel", "Groß"
virtualNetworkName  | Zeichenfolge    | Name des virtuellen Netzwerks, die der Consumer verwenden möchte.
enableJumpbox   | Eine Zeichenfolge aus einer eingeschränkten Liste (aktiviert/deaktiviert) | Parameter, die angibt, ob eine Jumpbox für die Umgebung aktivieren. Werte: "aktiviert", "deaktiviert"

Der im vorherigen Abschnitt verwendete **TshirtSize** Parameter ist wie folgt definiert:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Übergeben Status an verknüpfte Vorlagen

Bei der Verbindung mit verknüpften Vorlagen Sie häufig verwenden eine Kombination aus statischen und Variablen generiert.

### <a name="static-variables"></a>Statische Variablen

Statische Variablen werden häufig verwendet, um Basis Werte, wie etwa URLs, bereitzustellen, die überall in einer Vorlage verwendet werden.

In der folgenden Vorlage Ausschnitt `templateBaseUrl` gibt die Quadratwurzel Speicherort für die Vorlage in GitHub an. Die nächste Zeile erstellt eine neue Variable `sharedTemplateUrl` , die mit den bekannten Namen der Ressourcenvorlage freigegebenen die base-URL verkettet. Eine komplexe Objektvariable wird unterhalb dieser Zeile, eine Größe T-shirt gespeichert, wobei die base-URL zum Speicherort bekannten Konfiguration-Vorlage verkettet und gehörende Kehrmatrix verwendet die `vmTemplate` Eigenschaft.

Die Vorteile von diesem Ansatz ist, wenn sich der Speicherort für die Vorlage ändert, benötigen Sie nur so ändern Sie die statische Variable an einem Ort, die es in allen verknüpften Vorlagen übergibt.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Generierte Variablen

Zusätzlich zu statische Variablen werden mehrere Variablen dynamisch generiert. In diesem Abschnitt werden einige allgemeinen Typen von generierten Variablen.

#### <a name="tshirtsize"></a>tshirtSize

Sie können aus den oben aufgeführten Beispielen dieses generierte Variable vertraut.

#### <a name="networksettings"></a>networkSettings

In einer Kapazität, Videofunktionen oder Suchbegriffs End-to-End-Lösungsvorlage erstellen die verknüpften Vorlagen in der Regel Ressourcen, die vorhanden sind in einem Netzwerk. Eine einfache Möglichkeit besteht darin, verwenden ein komplexes Objekt Netzwerkeinstellungen speichern und zu verknüpften Vorlagen zu übergeben.

Ein Beispiel für die Kommunikation Einstellungen im Netzwerk angezeigt.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Ressourcen in verknüpften Vorlagen erstellt werden häufig in einem Satz Verfügbarkeit platziert. Im folgenden Beispiel wird die Verfügbarkeit Setname angegeben und auch die Fehlerstrukturanalyse-Domäne und eine Domäne aktualisieren zählen, um zu verwenden.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Wenn Sie mehrere Verfügbarkeit Sätze (beispielsweise eine master-Knoten) und einen anderen für Datenknoten benötigen, können Sie einen Namen als Präfix verwenden, mehrere Verfügbarkeit Sätze angeben oder führen Sie das Modell, das weiter oben für das Erstellen einer Variablen für eine bestimmte T-shirt Größe angezeigt.

#### <a name="storagesettings"></a>storageSettings

Speicherdetails werden häufig mit verknüpften Vorlagen freigegeben. Im folgenden Beispiel stellt ein Objekt *StorageSettings* Details über die Speicher-Konto und Container.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Mit verknüpften Vorlagen müssen Sie möglicherweise Betriebssystem Einstellungen auf verschiedene Arten von Knoten über andere bekannte Konfigurationstypen übergeben. Ein komplexes Objekt ist eine einfache Möglichkeit zum Speichern und Freigeben von Informationen zum Betriebssystem und auch gliederungsabfolge vereinfacht das zur Unterstützung von Betriebssystem Mehrfachauswahl für die Bereitstellung.

Im folgenden Beispiel wird ein Objekt für *OsSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Eine generierte Variable ist *MachineSettings* ein komplexes Objekt, das eine Mischung aus Core Variablen zum Erstellen eines virtuellen Computers enthält. Die Variablen umfassen Administrator-Benutzernamen und das Kennwort, ein Präfix für die Namen virtueller Computer und ein Betriebssystem Bild Bezug.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Beachten Sie, dass die *OsImageReference* die Werte aus der in der Vorlage Hauptfenster definierten *OsSettings* Variable abruft. Das bedeutet, dass Sie können ganz einfach das Betriebssystem, das für einen virtuellen ändern – vollständig oder basierend auf der Vorlage Consumer Wünsche.

#### <a name="vmscripts"></a>vmScripts

Das *VmScripts* -Objekt enthält Details zu den Skripts zum Herunterladen und ausführen, klicken Sie auf eine Instanz der virtueller Computer, einschließlich außerhalb und innerhalb Verweise. Außerhalb schließen Verweise die Infrastruktur. Innen Verweise umfassen die installierte Software installiert und die Konfiguration.

Verwenden Sie die Eigenschaft *ScriptsToDownload* Listen Sie die Skripts auf den virtuellen Computer herunterzuladen. Dieses Objekt enthält auch Verweise auf Befehlszeilenargumente für unterschiedliche Arten von Aktionen. Diese Aktionen umfassen Ausführen der Standardinstallation für die einzelnen Knoten, eine Installation, die ausgeführt wird, nachdem alle Knoten bereitgestellt werden und zusätzliche Skripts, die möglicherweise bestimmte einer bestimmten Vorlage.

In diesem Beispiel wird aus einer Vorlage zur Bereitstellung von MongoDB, Vermittler hohen Verfügbarkeit vorführen einzugeben. Die *ArbiterNodeInstallCommand* wurde *VmScripts* , installieren die Arbiter hinzugefügt.

Im Variablenabschnitt ist, finden Sie die Variablen, die den spezifischen Text zum Ausführen des Skripts mit den richtigen Werten definieren.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Zurücksetzen Zustand aus einer Vorlage

Nicht nur können Sie übergeben Daten in eine Vorlage verwenden, können Sie auch Daten wieder in der Vorlage einen freigeben. Im Abschnitt **gibt** eine verknüpfte Vorlage können Sie Schlüssel/Wert-Paare bereitstellen, die von der Quellvorlage genutzt werden können.

Im folgenden Beispiel wird gezeigt, wie die private IP-Adresse in einer verknüpften Vorlage generiert übergeben.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Im Hauptfenster Vorlage können Sie die Daten mit der folgenden Syntax verwenden:

    "[reference('master-node').outputs.masterip.value]"

Sie können diesen Ausdruck entweder im Abschnitt Ausgaben oder Ressourcenabschnitt der Hauptfenster Vorlage verwenden. Sie können nicht den Ausdruck im Variablenabschnitt verwenden, da er von der Laufzeitzustand abhängt. Um diesen Wert aus dem Hauptfenster Vorlage zurückzugeben, verwenden:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Ein Beispiel für den Abschnitt Ausgaben einer verknüpften Vorlage zurückzugebenden Daten Datenträger für einen virtuellen Computer verwenden finden Sie unter [mehrere Daten Datenträger für einen virtuellen Computer erstellen](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Definieren der Authentifizierungseinstellungen für virtuellen Computern

Sie können das gleiche Muster zuvor der Einstellungen für Konfiguration dargestellt verwenden, um die Authentifizierungseinstellungen für einen virtuellen Computer anzugeben. Erstellen Sie einen Parameter für die Übergabe in die Art der Authentifizierung.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Hinzufügen von Variablen für die verschiedenen Authentifizierungsarten und eine Variable zum Speichern des Typs wird für diese Bereitstellung basierend auf dem Wert des Parameters verwendet.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Wenn des virtuellen Computers zu definieren, legen Sie die **OsProfile** auf die Variable, die Sie erstellt haben.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu den Abschnitten der Vorlage finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](resource-group-authoring-templates.md)
- Die Funktionen, die innerhalb einer Vorlage zur Verfügung stehen, finden Sie unter [Azure Ressourcenmanager Vorlage Funktionen](resource-group-template-functions.md)

