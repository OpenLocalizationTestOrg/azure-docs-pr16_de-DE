
<properties
    pageTitle="Verwalten von Ressourcen mit der CLI Azure | Microsoft Azure"
    description="Verwenden Sie die Azure Line Interface (CLI) zum Verwalten von Azure Ressourcen und Gruppen"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Verwenden Sie die CLI Azure Azure Ressourcen und Ressourcengruppen verwalten


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST-API](resource-manager-rest-api.md)


Die Azure Line-Benutzeroberfläche (Azure CLI) ist der verschiedene Tools, mit denen, die Sie zum Bereitstellen und Verwalten von Ressourcen mit Ressourcenmanager verwenden können. In diesem Artikel werden allgemeine Methoden zum Verwalten von Azure Ressourcen und Ressourcengruppe mithilfe der Azure CLI im Modus Ressourcenmanager vorgestellt. Informationen zur Verwendung der CLI Ressourcen bereitstellen finden Sie unter [Bereitstellen von Ressourcen mit Ressourcenmanager Vorlagen und Azure CLI](resource-group-template-deploy-cli.md). Hintergrundinformationen zur Azure Ressourcen und Ressourcenmanager finden Sie auf der [Azure Ressourcenmanager Übersicht](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Zum Verwalten von Azure Ressourcen mit Azure CLI müssen Sie [Azure CLI installieren](xplat-cli-install.md), und [Melden Sie sich bei Azure](xplat-cli-connect.md) mithilfe der `azure login` Befehl. Vergewissern Sie sich das ist im Modus Ressourcenmanager (ausführen `azure config mode arm`). Wenn Sie die folgenden Punkte vorgenommen haben, können Sie erstellt haben.



## <a name="get-resource-groups-and-resources"></a>Ressourcen- und Ressourcen abrufen

### <a name="resource-groups"></a>Ressourcengruppen

Um eine Liste aller Ressourcen Gruppen in Ihrem Abonnement und deren Speicherorte zu erhalten, führen Sie diesen Befehl aus.

    azure group list
    

### <a name="resources"></a>Ressourcen
 Um eine Liste aller Ressourcen in einer Gruppe, wie z. B. mit Namen *TestRG*, verwenden Sie den folgenden Befehl ein.

    azure resource list testRG

Verwenden Sie eine bestimmte Ressource in der Gruppe, wie ein virtueller Computer mit der Bezeichnung *MyUbuntuVM*um anzuzeigen einen Befehl wie folgt aus.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Beachten Sie den **Microsoft.Compute/virtualMachines** Parameter ein. Dieser Parameter zeigt den Typ der Ressource, die, der Sie auf Informationen anfordern.
    
>[AZURE.NOTE]Wenn Sie die Befehle **Azure Ressource** als den Befehl **Liste** verwenden zu können, müssen Sie die API-Version der Ressource mit den **o -** Parameter angeben. Wenn Sie über die API-Version nicht sicher sind, wenden Sie sich an die Vorlagendatei, und suchen Sie das Feld ApiVersion für die Ressource. Weitere Informationen zu API Versionen in Ressourcenmanager finden Sie unter [Ressourcenmanager Anbieter, Regionen, API Versionen und Schemas](resource-manager-supported-services.md).

Bei der Anzeige von Details für eine Ressource ist es häufig sinnvoll, verwenden Sie die `--json` Parameter. Für diesen Parameter können die Ausgabe besser lesbar, da einige Werte geschachtelten Strukturen oder Websitesammlungen sind. Das folgende Beispiel veranschaulicht die Ergebnisse des Befehls **Anzeigen** als JSON-Dokument zurückgegeben.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Wenn Sie möchten, können Sie die JSON-Daten in Datei speichern, mithilfe der &gt; Zeichen die Ausgabe in Datei umleiten. Beispiel:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Kategorien

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Verwalten von Ressourcen


Um eine Ressourcengruppe eine Ressource, wie etwa ein Speicherkonto hinzugefügt haben, führen Sie einen Befehl ähnlich wie ein:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Zusätzlich zum Angeben der API Version der Ressource mit dem Parameter **-o** , verwenden Sie den Parameter **-p** , um eine JSON-formatierte Zeichenfolge mit alle erforderlichen oder zusätzlichen Eigenschaften zu übergeben.
    
    
Verwenden Sie zum Löschen einer vorhandenen Ressource wie eine Ressource virtuellen Computern wie den folgenden Befehl ein.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Verwenden Sie den Befehl **Azure Ressource verschieben** aus, um vorhandene Ressourcen auf ein anderes Ressourcengruppe oder Abonnement verschieben. Im folgenden Beispiel wird gezeigt, wie ein Redis Cache zu einer neuen Ressourcengruppe verschieben. Geben Sie in der **i -** Parameter an, dass eine kommagetrennte Liste der Ressource-Id ist um zu verschieben.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Steuern des Zugriffs auf Ressourcen

Sie können CLI Azure erstellen und Verwalten von Richtlinien zum Steuern des Zugriffs auf Azure Ressourcen verwenden. Hintergrundinformationen zur Richtliniendefinitionen und Zuweisen von Richtlinien für Ressourcen finden Sie unter [Richtlinien zum Verwalten von Ressourcen und Steuern des Zugriffs verwenden](resource-manager-policy.md).

Angenommen, definieren Sie die folgende Richtlinie, um alle Anfragen ablehnen, in dem Speicherort nicht Westen US oder US North Central ist, und klicken Sie auf die Richtlinie Definition Datei policy.json gespeichert:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Führen Sie den Befehl **Richtliniendefinition zu erstellen** :

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Dieser Befehl zeigt die Ausgabe ähnlich wie der folgende.

    + Erstellen der Richtliniendefinition von eigene Richtlinie Daten: PolicyName: von eigene Richtlinie Daten: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    Daten: PolicyType: benutzerdefinierte Daten: DisplayName: undefiniert Daten: Beschreibung: undefiniert Daten: PolicyRule: Feld = Speicherort, in [Westus Northcentralus], Effekt = verweigern

 Gehen Sie zum Zuweisen einer Richtlinie an den Bereich gewünschte verwenden Sie die **PolicyDefinitionId** aus dem vorherigen Befehl zurückgegeben. Im folgenden Beispiel dieser Bereich ist das Abonnement, aber Sie können einen Bereich für Ressourcengruppen oder einzelne Ressourcen:

    Azure Richtlinie Zuordnung erstellen MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy s - /subscriptions/###-###-###-###-### /

Sie können abrufen, ändern oder Entfernen der Richtliniendefinitionen mithilfe der Befehle **Richtliniendefinition anzeigen**, **Richtliniendefinition festzulegen**und **Richtliniendefinition löschen** .

Sie können auf ähnliche Weise abrufen, ändern oder Entfernen von zugewiesenen Richtlinien mithilfe der Befehle **Richtlinie Zuordnung anzeigen**, **Richtlinie Zuordnung festlegen**und **Richtlinie Zuordnung zu löschen** .


## <a name="export-a-resource-group-as-a-template"></a>Exportieren einer Ressourcengruppe als Vorlage

Für eine vorhandene Ressourcengruppe können Sie die Vorlage Ressourcenmanager für die Ressourcengruppe anzeigen. Exportieren der Vorlage bietet zwei Vorteile:

1. Sie können ganz einfach zukünftige Bereitstellungen der Lösung automatisieren, da alle die Infrastruktur in der Vorlage definiert ist.

2. Sie können mit vertraut Vorlagensyntax anhand der JSON, die Ihre Lösung darstellt.

Verwenden der CLI Azure, können Sie entweder exportieren eine Vorlage, die den aktuellen Status der Ressourcengruppe darstellt oder Herunterladen die Vorlage, die für eine bestimmte Bereitstellung verwendet wurde.

* **Exportieren Sie die Vorlage für eine Ressourcengruppe** – Dies ist hilfreich, wenn Sie Änderungen an einer Ressourcengruppe vorgenommen, und die JSON-Darstellung des aktuellen Zustand abgerufen werden müssen. Die generierte Vorlage wird jedoch nur eine minimale Anzahl von Parametern und keine Variablen enthält. Die meisten Werte in der Vorlage werden hartcodierte. Vor der Bereitstellung der Vorlage generierten, möchten Sie möglicherweise Konvertieren von mehreren Werten in Parameter, damit Sie die Bereitstellung für den verschiedenen Umgebungen anpassen können.

    Um die Vorlage für eine Ressourcengruppe in einem lokalen Verzeichnis zu exportieren, führen Sie die `azure group export` Befehl wie im folgenden Beispiel dargestellt. (Ersetzen durch einem lokalen Verzeichnis Ihrer Umgebung Betriebssystem).

        azure group export testRG ~/azure/templates/

* **Herunterladen die Vorlage für eine bestimmte Bereitstellung** – Dies ist hilfreich, wenn Sie müssen die ist-Vorlage anzeigen möchten, die zum Bereitstellen von Ressourcen verwendet wurde. Die Vorlage enthält alle Parameter und Variablen, die für die ursprüngliche Bereitstellung definiert. Wenn eine Person in Ihrer Organisation die Ressourcengruppe außerhalb der Definition in der Vorlage geändert haben, wird nicht diese Vorlage im aktuellen Zustand der Ressourcengruppe darstellen.

    Wenn die Vorlage für eine bestimmte Bereitstellung in einem lokalen Verzeichnis herunterladen möchten, führen Sie die `azure group deployment template download` Befehl. Beispiel:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Exportieren der Vorlage ist in der Vorschau und zurzeit nicht alle Ressourcentypen unterstützt Exportieren einer Vorlage. Wenn Sie versuchen, eine Vorlage exportieren, wird möglicherweise einen Fehler angezeigt, der besagt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf manuell definieren Sie diese Ressourcen in der Vorlage nach dem herunterladen.



## <a name="next-steps"></a>Nächste Schritte

* Um die Details der Bereitstellungsvorgänge und die Problembehandlung Bereitstellungsfehler mit Azure CLI, finden Sie unter [Bereitstellung-Vorgänge mit Azure CLI anzeigen](resource-manager-troubleshoot-deployments-cli.md).
* Wenn Sie die CLI zum Einrichten einer Anwendung oder das Skript Zugriff auf Ressourcen verwenden möchten, finden Sie unter [Verwenden von Azure CLI keinen Zugriff auf Ressourcen Hauptbenutzer Dienst erstellen](resource-group-authenticate-service-principal-cli.md).


