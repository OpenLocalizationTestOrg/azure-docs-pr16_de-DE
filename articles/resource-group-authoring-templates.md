<properties
   pageTitle="Authoring Azure Ressourcenmanager Vorlagen | Microsoft Azure"
   description="Mit JSON-Deklarationssyntax Bereitstellen von Applications in Azure Ressourcenmanager Azure-Vorlagen erstellen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Authoring Ressourcenmanager Azure-Vorlagen

In diesem Thema werden die Struktur einer Ressourcenmanager Azure-Vorlage. Es stellt die verschiedenen Abschnitten einer Vorlage und Eigenschaften, die in diese Abschnitte zur Verfügung stehen. Die Vorlage besteht aus JSON und Ausdrücke, die Sie verwenden können, um Werte für die Bereitstellung zu erstellen. 

Um die Vorlage für Ressourcen anzuzeigen, die Sie bereits bereitgestellt haben, finden Sie unter [Exportieren eine Ressourcenmanager Azure-Vorlage von vorhandenen Ressourcen](resource-manager-export-template.md). Leitfaden für die zum Erstellen einer Vorlage finden Sie unter [Exemplarische Vorgehensweise Ressourcenmanager-Vorlage](resource-manager-template-walkthrough.md). Empfehlungen zum Erstellen von Vorlagen finden Sie unter [bewährte Methoden zum Erstellen von Azure Ressourcenmanager Vorlagen](resource-manager-template-best-practices.md).

Ein guter JSON-Editor kann den Vorgang des Erstellens von Vorlagen vereinfachen. Informationen zur Verwendung von Visual Studio mit Ihrem Vorlagen finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Informationen zur Verwendung im Vergleich mit einer Code finden Sie unter [Arbeiten mit Azure Ressourcenmanager Vorlagen in Visual Studio-Code](resource-manager-vs-code.md).

Beschränken Sie der Größe der Vorlage 1 MB und jede Parameterdatei 64 KB sein. Die Beschränkung von 1 MB gilt der endgültige Status der Vorlage aus, nachdem es mit iterative Ressourcendefinitionen und Werte für Variablen und Parameter erweitert wurde. 

## <a name="template-format"></a>Vorlagenformat

In ihrer einfachsten Struktur enthält eine Vorlage für die folgenden Elemente:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Name des Elements   | Erforderlich | Beschreibung
| :------------: | :------: | :----------
| $schema        |   Ja    | Speicherort der JSON-Schemadatei, die die Version der Vorlagensprache beschreibt. Verwenden Sie die URL, die im vorherigen Beispiel dargestellt.
| contentVersion |   Ja    | Version der Vorlage (z. B. 1.0.0.0). Sie können einen beliebigen Wert für dieses Element bereitstellen. Beim Bereitstellen von Ressourcen mithilfe der Vorlage kann dieser Wert verwendet werden, um sicherzustellen, dass die richtige Vorlage verwendet wird.
| Parameter     |   Nein     | Werte, die bereitgestellt werden, wenn die Bereitstellung ausgeführt wird, um die Bereitstellung der Ressource anpassen.
| Variablen      |   Nein     | Werte, die als JSON-Fragmente in der Vorlage verwendet werden, um die Vorlage Sprache Ausdrücken zu vereinfachen.
| Ressourcen      |   Ja    | Ressourcentypen, die bereitgestellt oder in einer Ressourcengruppe aktualisiert werden.
| Ausgaben        |   Nein     | Werte, die nach der Bereitstellung zurückgegeben werden.

In den Abschnitten der Vorlage weiter unten in diesem Thema ausführlicher untersucht. Jetzt werden wir einige der Syntax überprüfen, aus denen die Vorlage besteht.

## <a name="expressions-and-functions"></a>Ausdrücken und Funktionen

Die grundlegende Syntax der Vorlage ist JSON. Jedoch erweitern Ausdrücken und Funktionen auf die JSON, die in der Vorlage zur Verfügung. Mit Ausdrücken erstellen Sie die Werte, die nicht nur literalen Werten sind. Ausdrücke in Klammern gesetzt werden [und] und ausgewertet werden, wenn die Vorlage bereitgestellt wird. Ausdrücke können an beliebiger Stelle in einer JSON-Zeichenfolgenwert und immer eine andere JSON-Wert zurückgegeben. Wenn Sie eine literale Zeichenfolge verwenden, die mit einer Klammer beginnt müssen [, verwenden Sie zwei Klammern [[.

Verwenden Sie in der Regel Ausdrücke mit Funktionen zum Ausführen von Vorgängen zum Konfigurieren der bereitstellungs. Wie werden in JavaScript Funktion Anrufe als **functionName(arg1,arg2,arg3)**formatiert. Mit den Punkt und [Index] Operatoren verweisen Sie Eigenschaften.

Im folgenden Beispiel wird gezeigt, wie verschiedene Funktionen verwendet, wenn Werte bauen:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Die vollständige Liste der Vorlagenfunktionen finden Sie unter [Azure Ressourcenmanager Vorlagenfunktionen](resource-group-template-functions.md). 


## <a name="parameters"></a>Parameter

Im Abschnitt Parameter der Vorlage Geben Sie die Werte eingegeben werden können, wenn Sie die Ressourcen bereitstellen. Diese Parameterwerte ermöglichen Ihnen die Bereitstellung können, indem Sie die Werte, die zugeschnitten sind für eine bestimmte Umgebung (wie etwa Entwickler, testen und Fertigung) anpassen. Sie müssen keinen Parameter in der Vorlage angeben, aber ohne Parameter sollte Ihre Vorlage immer die dieselben Ressourcen mit demselben Namen, Speicherorte und Eigenschaften bereitstellen.

Diese Parameterwerte überall auf der Vorlage können zum Festlegen von Werten für die Ressourcen bereitgestellten. Nur Parameter, die im Abschnitt Parameter deklariert sind, können in anderen Abschnitten der Vorlage verwendet werden.

Definieren Sie Parameter mit der folgenden Struktur:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Name des Elements   | Erforderlich | Beschreibung
| :------------: | :------: | :----------
| parameterName  |   Ja    | Der Name des Parameters. Sie müssen ein gültiger JavaScript-Bezeichner.
| Typ           |   Ja    | Typ des Parameterwerts. Finden Sie in der Liste unterhalb der zulässigen Inhaltstypen.
| Standardwert   |   Nein     | Der Standardwert für den Parameter, wenn kein Wert für den Parameter bereitgestellt wird.
| allowedValues  |   Nein     | Array der zulässigen Werte für den Parameter aus, um sicherzustellen, dass der richtige Wert angegeben wird.
| minValue       |   Nein     | Den kleinsten Wert für Int Typparameter, ist dieser Wert (jeweils einschließlich).
| maxValue       |   Nein     | Der maximale Wert für Int Typparameter, ist dieser Wert (jeweils einschließlich).
| minLength      |   Nein     | Die Mindestlänge für Zeichenfolge, SecureString und Array Typparameter, ist dieser Wert (jeweils einschließlich).
| maxLength      |   Nein     | Die maximale Länge für Zeichenfolge, SecureString und Parameter der Matrix eingeben, wird dieser Wert (jeweils einschließlich).
| Beschreibung    |   Nein     | Beschreibung des Parameters an, die Benutzer der Vorlage durch die benutzerdefinierte Vorlage Veröffentlichungsportal Benutzeroberfläche angezeigt wird.

Der zulässige Typen und Werte sind:

- **Zeichenfolge**
- **secureString**
- **Ganzzahl**
- **bool**
- **Objekt** 
- **secureObject**
- **Matrix**

Geben Sie zum Angeben eines Parameters als optional einen Standardwert (kann eine leere Zeichenfolge sein). 

Wenn Sie einen Parameternamen, der einer der Parameter in den Befehl angeben, um die Vorlage bereitstellen entspricht, werden Sie aufgefordert, einen Wert für einen Parameter mit dem Suffix **FromTemplate**bereitzustellen. Wenn Sie einen Parameter namens **ResourceGroupName** in Ihre Vorlage einbeziehen, die beträgt beispielsweise identisch mit den Parameter **ResourceGroupName** in der [Neu-AzureRmResourceGroupDeployment] [ deployment2cmdlet] Cmdlet, Sie werden aufgefordert, einen Wert für **ResourceGroupNameFromTemplate**angeben. Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie nicht Benennen von Parametern mit demselben Namen als Parameter für Bereitstellung Operationen verwendet.

>[AZURE.NOTE] Alle Kennwörter, Schlüssel und anderen vertraulichen sollte der **SecureString** verwenden. Mit dem Typ SecureString Vorlagenparameter können nach der Bereitstellung von Ressourcen gelesen werden. 

Im folgende Beispiel wird gezeigt, wie Parameter definiert wird:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Informationen über das zum Eingeben der Parameterwerte während der Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit Ressourcenmanager Azure-Vorlage](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Variablen

Im Abschnitt Variablen erstellen Sie Werte, die verwendet werden können, in der Vorlage. In der Regel Variablen basiert auf Werte aus der Parameter liegen. Sie müssen nicht Variablen definieren, aber sie häufig Ihre Vorlage vereinfachen, indem Sie komplexe Ausdrücken zu verringern.

Definieren Sie Variablen mit der folgenden Struktur:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Im folgenden Beispiel wird gezeigt, wie eine Variable zu definieren, die aus zwei Parameterwerte erstellt wird:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

Im nächste Beispiel zeigt eine Variable, die eine komplexe JSON-Typ ist und Variablen, die von anderen Variablen erstellt werden:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Ressourcen

Im Abschnitt Ressourcen definieren Sie die Ressourcen, die bereitgestellt oder aktualisiert werden. In diesem Abschnitt kann komplizierte abgerufen werden, da Sie die Typen kennen müssen, die Sie bereitstellen möchten, um die richtigen Werte. 

Definieren Sie Ressourcen mit der folgenden Struktur:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Name des Elements             | Erforderlich | Beschreibung
| :----------------------: | :------: | :----------
| apiVersion               |   Ja    | Version der REST API für die Ressource zu erstellen.
| Typ                     |   Ja    | Typ der Ressource. Dieser Wert ist eine Kombination aus den Namespace des Ressourcenanbieters und die Ressourcenart (z. B. **Microsoft.Storage/storageAccounts**).
| Namen                     |   Ja    | Name der Ressource. Der Name muss ein URI-Komponente Einschränkungen in RFC3986 definiert. Darüber hinaus Azure Services, die den Namen der Ressource außerhalb Parteien überprüft den Namen, um sicherzustellen, dass es verfügbar machen ist nicht auf den Versuch zu einer anderen Identität Spoofing. Erfahren Sie [Ressourcenname überprüfen](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| Speicherort                 |   Variiert  | Unterstützt Geo-Speicherorte der bereitgestellten Ressource an. Wählen Sie eine der verfügbaren Speicherorte aus, aber in der Regel ist es sinnvoll, eine auswählen, die in der Nähe der Benutzer befindet. In der Regel ist es sinnvoll, um Ressourcen zu platzieren, die in der gleichen Region miteinander interagieren. Die meisten Ressourcentypen erfordern einen Speicherort, aber bestimmte Typen (beispielsweise eine rollenzuweisung) einen Speicherort nicht erforderlich.
| Kategorien                     |   Nein     | Kategorien, die der Ressource zugeordnet sind.
| Kommentare                 |   Nein     | Ihre Notizen für die Ressourcen in der Vorlage dokumentieren
| dependsOn                |   Nein     | Ressourcen, von denen die Ressource definiert wird abhängig. Die Abhängigkeiten zwischen Ressourcen ausgewertet und Ressourcen in deren abhängigen Reihenfolge bereitgestellt werden. Wenn die Ressourcen nicht voneinander abhängig sind, werden sie parallel bereitgestellt. Der Wert kann eine durch Trennzeichen getrennte Liste einer Ressource sein Namen oder Ressource eindeutigen Bezeichner.
| Eigenschaften               |   Nein     | Ressourcen-spezifische Konfiguration Einstellungen. Die Werte für die Eigenschaften werden die Werte, die Sie für den Vorgang die REST-API (sich Methode) zum Erstellen der Ressource im Hauptteil Anforderung bereitstellen entspricht. Links zu Ressourcen Schemadokumentation oder REST-API sind finden Sie unter [Ressourcenmanager Anbieter, Regionen, API Versionen und Schemas](resource-manager-supported-services.md).
| Kopieren                     |   Nein     | Wenn mehr als eine Instanz benötigt wird, die Anzahl der Ressourcen zu erstellen. Weitere Informationen finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Ressourcenmanager](resource-group-create-multiple.md). |
| Ressourcen                |   Nein     | Untergeordnete Ressourcen, die der Ressource definiert wird abhängig sind. Sie können nur die Ressourcentypen bereitstellen, die durch das Schema der übergeordneten Ressource zulässig sind. Der vollqualifizierte Namen des Typs Ressource untergeordneten enthält die übergeordnete Ressourcenart, z. B. **Microsoft.Web/sites/extensions**. Abhängigkeit von der übergeordneten Ressource keinen impliziert; Sie müssen diese Abhängigkeit explizit definieren. 

Wissen, welche Werte für **ApiVersion**angeben, ist **Typ**und **Speicherort** nicht sofort offensichtlich. Glücklicherweise können Sie diese Werte mithilfe von Azure PowerShell oder Azure CLI ermitteln.

Verwenden Sie alle Ressourcenanbieter mit **PowerShell**, um:

    Get-AzureRmResourceProvider -ListAvailable

Suchen Sie in der Liste zurückgegebenen für die Ressourcenanbieter, die, denen Sie interessiert sind. Verwenden Sie die Ressourcentypen für eine Ressourcenanbieter (z. B. Speicher), um:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Verwenden Sie die Versionen der-API für einen Ressourcentyp (solche Speicherkonten), um:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Verwenden Sie unterstützte Speicherorte für einen Ressourcentyp, um:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Verwenden Sie alle Ressourcenanbieter mit **Azure CLI**, um:

    azure provider list

Suchen Sie in der Liste zurückgegebenen für die Ressourcenanbieter, die, denen Sie interessiert sind. Verwenden Sie die Ressourcentypen für eine Ressourcenanbieter (z. B. Speicher), um:

    azure provider show Microsoft.Storage

Verwenden Sie unterstützte Speicherorte und API-Versionen, um:

    azure provider show Microsoft.Storage --details --json

Wenn Sie weitere Informationen zur Ressourcenanbieter finden Sie unter [Ressourcenmanager Anbieter, Regionen, API Versionen und Schemas](resource-manager-supported-services.md).

Im Ressourcenabschnitt enthält ein Array von den Ressourcen bereitstellen. Innerhalb jeder Ressource können Sie auch ein Array von untergeordneten Ressourcen definieren. Daher denkbar Ressourcenabschnitt eine Struktur wie:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


Im folgenden Beispiel wird eine **Microsoft.Web/serverfarms** und eine **Microsoft.Web/sites** Ressource mit einem untergeordneten **Erweiterungen** Ressource an. Beachten Sie, dass die Website als abhängig von der Serverfarm markiert ist, da die Serverfarm vorhanden sein muss, bevor die Website bereitgestellt werden kann. Beachten Sie auch, dass die Ressource **Erweiterungen** untergeordnetes Element der Website ist.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Ausgaben

Im Abschnitt Ausgaben Geben Sie die Werte, die von der Bereitstellung zurückgegeben werden. Beispielsweise könnten Sie URI um eine bereitgestellte Ressource zuzugreifen zurück.

Im folgenden Beispiel wird die Struktur der Definition eines Ausgabe an:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Name des Elements   | Erforderlich | Beschreibung
| :------------: | :------: | :----------
| outputName     |   Ja    | Name des den Ausgabewert. Sie müssen ein gültiger JavaScript-Bezeichner.
| Typ           |   Ja    | Typ des den Ausgabewert. Ausgabewerte unterstützen dieselben Typen als Vorlage Eingabeparameter an.
| Wert          |   Ja    | Vorlage Sprache-Ausdruck, der ausgewertet und als Ausgabewert zurückgegeben wird.


Im folgenden Beispiel wird einen Wert, der im Abschnitt Ausgaben zurückgegeben wird.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Weitere Informationen zum Arbeiten mit Ausgabe finden Sie unter [Freigabe-Zustand in Azure Ressourcenmanager Vorlagen](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Nächste Schritte
- Zum Anzeigen der vollständige Vorlagen für viele verschiedene Typen von Lösungen [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/)anzuzeigen.
- Weitere Informationen zu den Funktionen, die Sie innerhalb einer Vorlage aus verwenden können, finden Sie unter [Azure Ressourcenmanager Vorlage Funktionen](resource-group-template-functions.md).
- Um mehrere Vorlagen während der Bereitstellung kombinieren möchten, finden Sie unter [Verwendung von verknüpften Vorlagen Azure Ressourcenmanager](resource-group-linked-templates.md).
- Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe vorhanden sind. Dieses Szenario wird häufig, bei der Arbeit mit Speicher Firmen oder virtuelle Netzwerke, die von mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [ResourceId (Funktion)](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
