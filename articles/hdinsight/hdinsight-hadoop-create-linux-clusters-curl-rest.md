<properties
    pageTitle="Hadoop, HBase oder Storm Cluster unter Linux in mit cURL und der REST-API Azure HDInsight erstellen | Microsoft Azure"
    description="Erfahren Sie, wie Linux-basierten HDInsight Cluster mit cURL, Azure Ressourcenmanager Vorlagen und die Azure REST-API erstellen. Sie können Geben Sie den Cluster (Hadoop, HBase oder Storm), oder Sie können mithilfe von Skripts angepassten Komponenten installieren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Erstellen von Linux-basierten Cluster in mit cURL und der REST-API Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Die REST-API von Azure ermöglicht Ihnen, Management Vorgänge in der Azure-Plattform, einschließlich der Erstellung von neuen Ressourcen wie HDInsight Linux-basierten Cluster gehostete Dienste ausführen. In diesem Dokument erfahren Sie, wie Sie Vorlagen für Ressourcenmanager Azure HDInsight Cluster und der zugehörige Speicher konfigurieren, und klicken Sie dann cURL verwenden, um die Vorlage für die Azure REST-API zum Erstellen eines neuen HDInsight Clusters bereitstellen erstellen.

> [AZURE.IMPORTANT] Die Schritte in diesem Dokument verwenden die Standardanzahl von Worker Knoten (4) für eine HDInsight Cluster an. Wenn Sie, klicken Sie auf mehr als 32 Worker am Cluster erstellen oder Knoten beabsichtigen durch Skalierung Cluster nach der Erstellung, müssen Sie eine Knotengröße am mit mindestens 8 Kernen und 14 GB Ram auswählen.
>
> Weitere Informationen zu Knoten Größen und anfallenden Kosten finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure CLI__. Azure CLI wird verwendet, um einen Dienst Hauptbenutzer, beträchtlich erhöhen, klicken Sie dann mit der Authentifizierungstoken für Anforderungen an die REST-API Azure generiert wird.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __Aufrollen__. Dieses Programm steht Ihrem Paket Management System oder von [http://curl.haxx.se/](http://curl.haxx.se/)heruntergeladen werden kann.

    > [AZURE.NOTE] Wenn Sie zum Ausführen der Befehle in diesem Dokument PowerShell verwenden, müssen Sie zuerst Entfernen der `curl` Alias, das standardmäßig erstellt wird. Dieser Alias verwendet aufrufen-WebRequest, PowerShell-Cmdlet, statt cURL bei der Verwendung von der `curl` Befehl aus einer PowerShell-Eingabeaufforderung und für viele der in diesem Dokument verwendete Befehle Fehler zurück.
    > 
    > Verwenden Sie zum Entfernen dieser Alias aus der PowerShell-Eingabeaufforderung folgende aus:
    >
    > `Remove-item alias:curl`
    >
    > Wenn Sie der Alias entfernt haben, sollten Sie möglicherweise die Version der cURL verwenden, die Sie auf Ihrem System installiert haben.

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Erstellen einer Vorlage

Azure Ressourcenverwaltung Vorlagen werden JSON-Standarddokumenten, die zur Beschreibung einer __Ressourcengruppe__ und alle Ressourcen darin (z. B. HDInsight.) Dieser Ansatz Vorlage basiert können Sie alle Ressourcen zu definieren, die Sie in eine Vorlage für HDInsight benötigen, und zum Verwalten von Änderungen an der Gruppe als Ganzes bis __Bereitstellungen__ , die in der Gruppe Änderungen zu übernehmen.

Vorlagen werden in der Regel in zwei Teilen bereitgestellt. die Vorlage selbst, und eine Parameterdatei, die Sie an der Konfiguration mit bestimmten Werten aufgefüllt. Für Exmaple, Clustername, Administratornamen und das Kennwort ein. Wenn Sie die REST-API direkt verwenden zu können, müssen Sie diese in einer Datei kombinieren. Das Format dieses Dokuments JSON lautet:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Die folgenden beträgt beispielsweise eine Beurteilung von Fusionen Vorlage und Parameter-Dateien aus [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), die einen Linux-basierten Cluster mit einem Kennwort das Benutzerkonto SSH gesichert erstellt.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

In diesem Beispiel wird in den Schritten in diesem Dokument verwendet werden. Sie müssen den Platzhalter _Werte_ im Abschnitt __Parameter__ am Ende des Dokuments mit den Werten ersetzen, die Sie für Ihren Cluster verwenden möchten.

##<a name="login-to-your-azure-subscription"></a>Melden Sie sich Ihr Abonnement Azure

Führen Sie die Schritte beschrieben, die in [Verbindung mit einer Azure-Abonnement aus der Azure Line Interface (CLI Azure) herstellen](../xplat-cli-connect.md) , und verbinden Sie Ihr Abonnement mit den `azure login` Befehl.

##<a name="create-a-service-principal"></a>Erstellen einer Dienst Tilgungsanteile

> [AZURE.NOTE] Diese Schritte sind eine gekürzte Version der die Angaben im Abschnitt _authentifizieren Service mit einem Kennwort - Azure CLI Hauptbenutzer_ des Dokuments [ein Dienst Tilgungsanteile Azure Ressourcenmanager authentifizieren](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) . Diesen Schritten erstellt eine neue Dienstleistung Hauptbenutzer, die die REST-API Anfragen verwendet, um Azure Ressourcen wie bei einer HDInsight Cluster erstellen Authentifizierung verwendet werden können.

1. Verwenden Sie den folgenden Befehl über die Befehlszeile, terminal Sitzung oder Shell in der Liste Ihrer Azure-Abonnements.

        azure account list
        
    Wählen Sie in der Liste der Abonnements, die Sie verwenden möchten und beachten Sie die __ID-__ Spalte. Dies ist die __Abonnement-ID__ und in den meisten die Schritte in diesem Dokument verwendet werden.

2. Erstellen einer neuen Anwendung in Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Ersetzen Sie die Werte für die `--name`, `--home-page`, und `--identifier-uris` mit Ihren eigenen Werten. Geben Sie ein Kennwort für den neuen Active Directory-Eintrag.
    
    > [AZURE.NOTE] Da Sie diese Anwendung für Authentifizierung über einen Dienst der Tilgungsanteile, erstellen die `--home-page` und `--identifier-uris` Werte brauchen verweisen auf einer Webseite gehostet wird, klicken Sie auf das Internet. Sie müssen nur eindeutige URIs angezeigt werden.
    
    Speichern Sie die Daten zurückgegeben __AppId__ -Wert ein.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Erstellen Sie einen Dienst, die wichtigsten unter Verwendung des __AppId__ Werts zuvor zurückgegeben.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Aus den zurückgegebenen Daten speichern Sie den __Objekt-Id__ -Wert.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Weisen Sie der __Besitzer__ Rolle den Dienst aus, die zuvor Hauptbenutzer mit dem __Objekt-ID__ -Wert zurückgegeben wird. Sie müssen außerdem die __Abonnement-ID__ verwenden, die Sie zuvor für Ihren Kunden.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Sobald dieser Befehl abgeschlossen ist, der Dienst Hauptbenutzer verfügt jetzt über Besitzer Zugriff auf die angegebene Abonnement-ID.

##<a name="get-an-authentication-token"></a>Abrufen einer Authentifizierungstoken

1. Verwenden Sie die folgenden, um die __Mandanten-ID__ für Ihr Abonnement zu finden.

        azure account show -s <subscription ID>
        
    Finden Sie aus den Daten, die zurückgegeben wird die __Mandanten-ID__ein.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Erstellen Sie ein neues Token mithilfe der Azure REST-API.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Ersetzen Sie mit den Werten, die für Ihren Kunden oder eine zuvor verwendete __TenantID__, __AppID__und __Ihr Kennwort ein__ .

    Wenn diese Anforderung erfolgreich ist, erhalten Sie Reaktionen 200 Serie, und der Hauptteil einer Antwort enthält ein JSON-Dokument.

    Das JSON-Dokument durch diese Anforderung zurückgegeben wird ein Element namens __Access_token__enthalten. der Wert für dieses Element ist das Access-Token, das Sie zum Authentifizierung die Anforderungen verwendet, die in den nächsten Abschnitten dieses Dokuments verwenden müssen.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Verwenden Sie zum Erstellen einer neuen Ressourcengruppe vor. Bevor Sie die Ressourcen wie HDInsight Cluster erstellen können, müssen Sie zuerst die Gruppe erstellen. 

* Ersetzen Sie __SubscriptionID__ durch die Abonnement-ID, die beim Erstellen der Tilgungsanteile Dienst erhalten.
* Ersetzen Sie __AccessToken__ durch das Zugriffstoken erhalten im vorherigen Schritt.
* Ersetzen Sie __DataCenterLocation__ durch das Data Center, die, dem Sie in der Ressourcengruppe und Ressourcen, erstellen möchten. Beispielsweise 'Süd zentralen US'. 
* Ersetzen Sie __ResourceGroupName__ durch den Namen, die, den Sie für diese Gruppe verwenden möchten:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Wenn diese Anforderung erfolgreich ist, erhalten Sie Reaktionen 200 Serie und der Hauptteil einer Antwort wird ein JSON-Dokument mit Informationen über die Gruppe enthalten. Die `"provisioningState"` Element enthalten einen Wert von `"Succeeded"`.

##<a name="create-a-deployment"></a>Erstellen einer bereitstellungs

Verwenden Sie die folgenden für die Bereitstellung der Cluster-Konfigurations (Vorlage und Parameter Werte,) der Ressourcengruppe.

* Ersetzen von __SubscriptionID__ und __AccessToken__ mit den Werten, die zuvor verwendet. 
* Ersetzen Sie __ResourceGroupName__ durch den Namen der Ressource-Gruppe, die, den Sie im vorherigen Abschnitt erstellt haben.
* Ersetzen Sie __DeploymentName__ durch den Namen, die, den Sie für diese Bereitstellung verwenden möchten.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Wenn Sie das JSON-Dokument mit der Vorlage und dem Parameter in eine Datei gespeichert haben, können Sie die folgenden anstelle von `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Wenn diese Anforderung erfolgreich ist, erhalten Sie Reaktionen 200 Serie, und der Hauptteil einer Antwort enthält ein JSON-Dokument mit Informationen über den Bereitstellungsvorgang.

> [AZURE.IMPORTANT] Beachten Sie, dass die Bereitstellung wurde übermittelt, aber noch nicht zu diesem Zeitpunkt abgeschlossen. Es dauert einige Minuten, in der Regel ungefähr 15, für die Bereitstellung ausführen zu können.

##<a name="check-the-status-of-a-deployment"></a>Überprüfen des Status einer Bereitstellung

Um den Status der Bereitstellung überprüfen möchten, verwenden Sie Folgendes ein:

* Ersetzen von __SubscriptionID__ und __AccessToken__ mit den Werten, die zuvor verwendet. 
* Ersetzen Sie __ResourceGroupName__ durch den Namen der Ressource-Gruppe, die, den Sie im vorherigen Abschnitt erstellt haben.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Dadurch wird die Informationen ein, die Informationen zum Bereitstellungsvorgang enthält JSON-Dokument zurückgegeben. Die `"provisioningState"` Element wird den Status der Bereitstellung; enthalten Wenn der Wert enthalten, der `"Succeeded"`, und klicken Sie dann die Bereitstellung erfolgreich abgeschlossen wurde. An diesem Punkt sollte Ihre Cluster zur Verwendung verfügbar sein.

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie einen HDInsight Cluster erfolgreich erstellt haben, verwenden Sie die folgenden Informationen zum Arbeiten mit Ihren Cluster. 

###<a name="hadoop-clusters"></a>Hadoop Cluster

* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)
* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase Cluster

* [Erste Schritte mit HBase auf HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-Anwendungen für HBase auf HDInsight entwickeln](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm Cluster

* [Entwickeln Sie Java Topologien für Storm auf HDInsight](hdinsight-storm-develop-java-topology.md)
* [Verwenden Sie Python Komponenten in Storm auf HDInsight](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen Sie und überwachen Sie Topologien mit Storm auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
