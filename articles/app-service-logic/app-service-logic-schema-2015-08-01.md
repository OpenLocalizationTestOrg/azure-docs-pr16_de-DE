<properties 
    pageTitle="Neues Schema Version 2015-08-01-Vorschau" 
    description="Erfahren Sie, wie die JSON-Definition für die neueste Version von apps Logik schreiben" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Neues Schema Version 2015-08-01-Vorschau

Das neue Schema und API Version für Logik apps bietet eine Reihe von zur Verbesserung der die Zuverlässigkeit zu verbessern und Center für erleichterte Bedienung von Logik apps. Es gibt 4 wichtigsten Unterschiede:

1. Der Aktionstyp **APIApp** wurde in einen anderen Typ von **APIConnection** Aktion aktualisiert.
2. **Wiederholen Sie die** **Foreach**wurde umbenannt in.
3. Die **Zuhörer HTTP** -API app ist nicht mehr erforderlich.
4. Aufrufen von untergeordneten Workflows verwendet wird ein neues Schema aus.

## <a name="1-moving-to-api-connections"></a>1. verschieben API Verbindungen

Die größte Änderung ist, dass Sie nicht mehr bereitstellen API apps in Ihr Abonnement Azure-APIs verwenden müssen. Es gibt 2 Verwendungsmöglichkeiten APIs aus:
* Verwaltete API
* Ihre benutzerdefinierten Web-API

Jede dieser wird etwas anders behandelt, da deren Management und Hostinganbieter Modelle unterscheiden. Ein Vorteil dieses Modells ist, dass Sie nicht mehr eingeschränkt sind zu Ressourcen, die bereitgestellt werden in der Ressourcengruppe. 

### <a name="managed-apis"></a>Verwaltete APIs

Es gibt eine Reihe von API, die von Microsoft, in Ihrem Auftrag, wie Office 365, Vertrieb, Twitter, FTP usw. verwaltet werden ein... Einige dieser verwalteten API als verwendet werden kann – ist sein können, wie z. B. Bing übersetzen, während andere Konfiguration erforderlich. Diese Konfiguration wird als eine *Verbindung*bezeichnet.

Wenn Sie Office 365 verwenden, müssen Sie beispielsweise eine Verbindung zu erstellen, die Ihr Office 365-Anmeldung Token enthält. Dieses Token werden sicher gespeichert und aktualisiert, damit Ihre app Logik immer die Office 365-API aufrufen kann. Wenn Sie zu Ihrem SQL oder FTP-Server herstellen möchten, müssen Sie Sie können auch eine Verbindung zu erstellen, die die Verbindungszeichenfolge enthält. 

Diese Aktionen werden innerhalb der Definition bezeichnet `APIConnection`. Hier ist ein Beispiel für eine Verbindung, die Anrufe von Office 365, um eine e-Mail zu senden:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Wird der Anteil der Eingaben, die eindeutige API-Verbindungen ist die `host` Objekt. Dies besteht aus zwei Teilen: `api` und `connection`.

Die `api` weist die Laufzeit URL der Stelle, an der die API verwaltet gehostet wird. Sie können alle verfügbaren verwalteten APIs für Sie anzeigen, indem Sie das Anrufen von `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Wenn Sie eine API verwenden, möglicherweise oder möglicherweise keine **Verbindungsparameter** definiert. Wenn dies nicht der Fall ist keine **Verbindung** erforderlich. Wenn dies der Fall ist, müssen Sie eine Verbindung herzustellen. Wenn Sie diese Verbindung müssen sie den Namen, die Sie auswählen, erstellen und dann Sie verweisen, die in der `connection` Objekt innerhalb der `host` Objekt. Rufen Sie zum Erstellen einer Verbindungs in einer Ressourcengruppe:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Mit den folgenden Text:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Bereitstellen von verwalteten APIs in einer Ressource Azure-Manager-Vorlage

Sie können eine vollständige Anwendung in einen Cloud-Vorlage erstellen, solange sie interaktive Anmeldung erforderlich ist. Wenn sie Anmeldung erforderlich ist, Sie können vollständige Einrichtung mit der Cloud-Vorlage, aber verfügen weiterhin im Portal, um die Verbindungen zu autorisieren besuchen. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Sie können in diesem Beispiel sehen, dass die Verbindungen nur normalen Ressourcen sind, die in der Ressourcengruppe live. Diese verweisen auf die ManagedAPIs im Rahmen Ihres Abonnements zur Verfügung.

### <a name="your-custom-web-apis"></a>Ihre benutzerdefinierten Web-APIs

Wenn Sie Ihre eigenen API (insbesondere nicht von Microsoft verwaltet Regeln) verwenden, sollten Sie die integrierte **HTTP** -Aktion verwenden, um angerufen werden. Um eine ideale Erfahrung haben, sollten Sie einen Endpunkt Swagger für Ihre API verfügbar machen. Dadurch werden den Logik app-Designer die Eingaben und Ausgaben für Ihre API dargestellt. Ohne eine Swagger kann der Designer nur die Eingaben und Ausgaben als undurchsichtig JSON-Objekte angezeigt werden.

Hier ist ein Beispiel dafür, das neue `metadata.apiDefinitionUrl` Eigenschaft:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Wenn Sie Ihre API Web **App** -Diensts hosten wird dann es automatisch in der Liste der verfügbaren Aktionen im Designer angezeigt. Wenn dies nicht der Fall ist, dann müssen Sie direkt in die URL einzufügen. Der Endpunkt Swagger muss nicht authentifiziert sein, um innerhalb der Logik apps Designer verwendbar sein (obwohl Sie werden die API geschützt können für jeden Methoden der Swagger unterstützt werden).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Verwenden die bereits bereitgestellten API-apps mit 2015-08-01-Vorschau

Wenn Sie eine app API zuvor bereitgestellt haben, können Sie es über die **HTTP** -Aktion aufrufen.

Beispielsweise, wenn Sie in der Listendateien Dropbox verwenden, ungefähr wie folgt aus der Definition Ihrer **2014-12-01-Preview** -Version Schema möglicherweise:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Sie können die entsprechende HTTP-Aktion wie unter (im Abschnitt Parameter der unverändert bleibt die Logik app Definition) erstellen:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Durchgehen diese Eigenschaften nacheinander aus:

| Action-Eigenschaft |  Beschreibung |
| --------------- | -----------  |
| `type` | `Http`Statt`APIapp` |
| `metadata.apiDefinitionUrl` | Wenn Sie diese Aktion im Logik apps-Designer verwenden möchten, sollten Sie den Metadaten-Endpunkt aufnehmen möchten. Dies ist von zusammengesetzt:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Dies ist von zusammengesetzt:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Immer`POST` |
| `inputs.body` | Identisch mit der app-API-Parameter | 
| `inputs.authentication` | Identisch mit api app-Authentifizierung |

Dieser Ansatz sollte für alle Aktionen der API-app verwendet werden. Wählen Sie weiter, denken Sie daran, die diese vorherigen API apps werden nicht mehr unterstützt, und Sie sollten in eine der beiden anderen Optionen über (entweder eine verwaltete API oder hosting Ihrer benutzerdefinierten Web-API) verschieben.

## <a name="2-repeat-renamed-to-foreach"></a>2. Wiederholen in Foreach umbenannt

Für die vorherige Schemaversion wir zahlreiche Kundenfeedback erhalten, **Wiederholen Sie** verwirrend und wurde nicht ordnungsgemäß erfassen, dass sie wirklich wurde eine für jede Schleife. Daher müssen wir es in **Foreach**umbenannt. Beispiel:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Sollte jetzt folgendermaßen geschrieben werden:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Zuvor die Funktion `@repeatItem()` wurde verwendet, um das aktuelle Element, das durchlaufen verweisen. Dies wurde vereinfacht, um nur `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Verweisen auf die Ausgaben für die Foreach
Zur weiteren Vereinfachung werden die Ausgaben der **Foreach** Aktionen in einem Objekt namens **RepeatItems**nicht umbrochen werden. Dies bedeutet, während die Ausgaben für die oben genannten wiederholen wurden:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Jetzt werden:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Wenn diese Ausgaben verwiesen wird, um den Hauptteil der Aktion zu gelangen müssten Sie nutzen möchten:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Jetzt können Sie stattdessen ausführen:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Mit diesen Änderungen, die Funktionen `@repeatItem()`, `@repeatBody()` und `@repeatOutputs()` werden entfernt.

## <a name="3-native-http-listener"></a>3 einer systemeigenen HTTP Zuhörer 
Die Zuhörer HTTP-Funktionen sind jetzt integrierte, sodass Sie nicht mehr benötigen, eine app HTTP Zuhörer-API bereitstellen. Erfahren Sie mehr über [die vollständigen Details zu wie die hier vorgenommene Ihrer Logik app Endpunkt aufgerufen](app-service-logic-http-endpoint.md)werden soll. 

Mit diesen Änderungen, die Funktion `@accessKeys()` entfernt und wurde durch ersetzt die `@listCallbackURL()` Funktion im Hinblick auf erste den Endpunkt (wenn erforderlich). Darüber hinaus müssen Sie jetzt mindestens ein Trigger in Ihrer app Logik jetzt definieren. Wenn Sie möchten `/run` Workflows, Sie müssen eine der haben ein `manual`, `apiConnectionWebhook` oder `httpWebhook` Trigger. 

## <a name="4-calling-child-workflows"></a>4. die untergeordneten Workflows Aufrufen von

Aufrufen von untergeordneten Workflows erforderlich in früheren Versionen dieser Workflow vertraut, das Access-Token angezeigt wird und dann einfügen, die in der Definition der Logik app, die Sie dieser untergeordneten anrufen möchten. Durch die neue Schemaversion generiert die apps-Engine Logik automatisch einem SAS zur Laufzeit für den untergeordneten Workflow, d. h., Sie besitzen, um vertrauliche Daten in der Definition zu kopieren.  Hier ist ein Beispiel:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Eine zweite Verbesserung ist, dass wir die untergeordneten Workflows Vollzugriff auf die eingehende Anforderung zugewiesen werden wird. Dies bedeutet, dass Sie im Abschnitt *Abfragen* und in das Objekt *Kopfzeilen* Parameter übergeben können und können Sie den gesamten Text vollständig definieren.

Schließlich sind erforderliche Änderungen an den untergeordneten Workflow vorhanden. Während die vor dem Sie gerade untergeordneter Workflow direkt; Anruf konnte jetzt, müssen Sie einen Endpunkt Trigger im Workflow für die übergeordnete Nummer definieren. In der Regel ist, bedeutet dies Sie Hinzufügen eines Triggers der Typ **Manuelle** und verwenden, klicken Sie dann in der übergeordneten Definition. Beachten Sie, dass die `host` Eigenschaft speziell weist eine `triggerName`, da Sie immer welcher Trigger angeben müssen, sind Sie aufrufen.

## <a name="other-changes"></a>Andere Änderungen

### <a name="new-queries-property"></a>Neue Abfragen-Eigenschaft
Alle Aktionstypen unterstützen nun eine neue Eingabe **Abfragen**genannt. Dies kann ein strukturierten Objekt statt dass Sie die Zeichenfolge von hand zusammenstellen sein.

### <a name="parse-function-renamed"></a>umbenannt Parse()-Funktion
Wie wir bald über weitere Inhaltstypen hinzufügen der `parse()` Funktion wurde in umbenannt `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>In Kürze verfügbar: Integration-APIs Enterprise
Zu diesem Zeitpunkt keine noch verwalteten Versionen der Enterprise-Integration-APIs zur Verfügung (z. B. AS2) aufweisen. Diese kommt früh wie in der [Wegweiser](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/)behandelt werden. In der Zwischenzeit können Sie Ihre vorhandene bereitgestellten BizTalk-APIs über die HTTP-Aktion verwenden wie verdeckt über in "mithilfe Ihrer bereits bereitgestellten API apps".
