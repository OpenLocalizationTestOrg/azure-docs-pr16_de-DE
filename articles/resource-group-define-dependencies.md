<properties
   pageTitle="Abhängigkeiten in Ressourcenmanager Vorlagen | Microsoft Azure"
   description="Beschreibt, wie eine Ressource als hängt von einer anderen Ressource festgelegt, während der Bereitstellung, um sicherzustellen, dass die Ressourcen in der richtigen Reihenfolge bereitgestellt werden."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Definieren von Abhängigkeiten in Azure Ressourcenmanager Vorlagen

Für eine bestimmte Ressource werden weitere Ressourcen, die vorhanden sein müssen, bevor die Ressourcen bereitgestellt wird. Beispielsweise muss ein SQLServer vorhanden sein, bevor Sie versuchen, eine SQL-Datenbank bereitstellen. Sie definieren diese Beziehung, indem Sie eine Ressource als die anderen Ressource abhängig. In der Regel, Sie definieren einer Beziehung mit dem Element **DependsOn** , aber Sie können Sie auch über die **Verweis** -Funktion definieren. 

Ressourcenmanager wertet die Abhängigkeiten zwischen Ressourcen und diese in deren abhängigen Reihenfolge bereitstellt. Wenn die Ressourcen nicht voneinander abhängig sind, werden Sie von Ressourcenmanager parallel bereitstellt.

## <a name="dependson"></a>dependsOn

Das Element DependsOn ermöglicht innerhalb der Vorlage eine Ressource als abhängige auf eine oder mehrere Ressourcen definieren. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen sein. 

Im folgenden Beispiel wird eine virtuellen Computern Skalieren festlegen, das hängt ein Lastenausgleich, virtuelle Netzwerk- und endlos wiedergegeben wird, die mehrere Speicherkonten erstellt. Diese anderen Ressourcen werden im folgenden Beispiel nicht angezeigt, aber an anderer Stelle in der Vorlage vorhanden sein müssen.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Zum Definieren einer Beziehung zwischen einer Ressourcen- und Ressourcen, die durch die Erstellung einer Kopie Schleife erstellt wurden, legen Sie das Element DependsOn auf Name der Schleife aus. Ein Beispiel finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Ressourcenmanager](resource-group-create-multiple.md).

Während Sie DependsOn Beziehungen zwischen Ressourcen zuordnen verwenden neigen, ist es wichtig zu verstehen, Gründe haben, da sie die Leistung der Bereitstellung auswirken kann. Um das Dokument, wie Ressourcen miteinander verbunden sind, ist DependsOn beispielsweise nicht die richtige Ansatz. Sie können keine Abfragen, welche Ressourcen nach der Bereitstellung im DependsOn-Element definiert wurden. Mithilfe von DependsOn beeinflussen Sie potenziell Zeitpunkt der Bereitstellung, da Ressourcenmanager bereitstellen nicht in parallelen zwei Ressourcen, die abhängig sind. Stattdessen verwenden [Ressource verknüpfen](resource-group-link-resources.md), um Beziehungen zwischen Ressourcen Dokument.

## <a name="child-resources"></a>Untergeordnete Ressourcen

Die Eigenschaft Ressourcen können Sie untergeordneten Ressourcen angeben, die für die Ressource definiert wird verwandt sind. Untergeordnete Ressourcen können nur definierten fünf Ebenen sein. Es ist wichtig, beachten Sie, dass eine implizite Abhängigkeit nicht zwischen einer Ressource untergeordnete und die übergeordnete Ressource erstellt wird. Wenn Sie die Ressource untergeordneten bereitgestellt werden, nachdem die übergeordnete Ressource benötigen, müssen Sie diese Abhängigkeit mit der Eigenschaft DependsOn explizit anzugeben. 

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordneten Ressourcen. Die zulässigen Ressourcentypen werden in der [Vorlage Schema](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben. Der Namen der untergeordneten Ressourcenart enthält den Namen des übergeordneten Ressource Typs wie **Microsoft.Web/sites/config** und **Microsoft.Web/sites/extensions** beide untergeordneten Ressourcen für die **Microsoft.Web/sites**sind.

Im folgenden Beispiel wird eine SQLServer und SQL-Datenbank. Beachten Sie, dass explizite Abhängigkeit zwischen den SQL-Datenbank und SQLServer, definiert ist, obwohl die Datenbank ein untergeordnetes Element des Servers ist.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>Verweis (Funktion)

Die [Verweis-Funktion](resource-group-template-functions.md#reference) ermöglicht einen Ausdruck, dessen Wert von anderen JSON-Namen und Wertepaare oder Laufzeitressourcen abgeleitet werden. Verweis Ausdrücke deklarieren implizit an, dass eine Ressource von einem anderen abhängt. 

    reference('resourceName').propertyPath

Sie können entweder dieses Element oder das Element DependsOn Abhängigkeiten angeben, aber nicht beide für dieselbe abhängige Ressource verwenden müssen. Verwenden Sie nach Möglichkeit einen impliziten Bezug um zu vermeiden Sie versehentlich eine unnötige Abhängigkeit hinzufügen.

Weitere Informationen finden Sie unter [Verweis (Funktion)](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen von Azure Ressourcenmanager Vorlagen, finden Sie unter [Authoring-Vorlagen](resource-group-authoring-templates.md). 
- Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Vorlagenfunktionen](resource-group-template-functions.md).

