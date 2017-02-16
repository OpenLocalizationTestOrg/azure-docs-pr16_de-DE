<properties
   pageTitle="Problembehandlung bei häufigen Azure-Bereitstellung | Microsoft Azure"
   description="Beschreibt, wie häufig auftretender Fehler zu beheben, wenn Sie Ressourcen mithilfe von Azure Ressourcenmanager Azure bereitstellen."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="Fehler bei der Bereitstellung, Azure-Bereitstellung in Azure bereitstellen"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Problembehandlung bei häufigen Azure-Bereitstellung Azure Ressourcenmanager

Dieser Artikel wird beschrieben, wie Sie einige häufige auflösen können treten möglicherweise Fehler bei der Azure-Bereitstellung. Wenn Sie weitere Informationen zur Ursache für die Bereitstellung des Problems benötigen, finden Sie zuerst [die Bereitstellungsvorgänge anzeigen](resource-manager-troubleshoot-deployments-portal.md) , und klicken Sie dann wieder zur in diesem Artikel Hilfe bei der Behebung des Fehlers. In den Abschnitten in diesem Thema Liste den Fehlercode, den die angezeigt werden.

## <a name="invalid-template"></a>Ungültige Vorlage

Dieser Fehler kann von verschiedenen Typen von Fehlern führen. 

### <a name="syntax-error"></a>Syntaxfehler

Wenn Sie eine Fehlermeldung, die die Vorlage konnte nicht Validierung angibt erhalten, verfügen Sie in der Vorlage ein Problem Syntax. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Dieser Fehler wird gemacht werden, da der Vorlagenausdrücke komplizierte werden können. Die folgenden Namen Zuordnung für ein Speicherkonto enthält beispielsweise eine Reihe von Klammern, drei Funktionen, drei Sätze von Klammern, eine Reihe von einfachen Anführungszeichen und eine Eigenschaft:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Wenn Sie nicht die passende Syntax angeben, werden die Vorlage einen Wert, der sich Ihre Absicht unterscheidet.

Wenn Sie diese Art von Fehler angezeigt wird, überprüfen Sie sorgfältig die Ausdruckssyntax. Erwägen Sie einen JSON-Editor wie [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) oder [Visual Studio-Code](resource-manager-vs-code.md), den Sie zur Syntax auf Fehler warnen können. 

### <a name="incorrect-segment-lengths"></a>Falsche Segment Länge

Eine andere ungültige Vorlage-Fehler auftritt, wenn Sie der Namen der Ressource nicht im richtigen Format ist.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Ressource für einen Stamm benötigen in den Namen als in den Ressourcentyp eine weniger Segment. Jedes Segment wird durch einen Schrägstrich unterschieden. Im folgenden Beispiel der Typ weist zwei Segmente und der Namen weist eines einzelnen Segments aus, sodass er einen **gültigen Namen**ist.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Aber im nächste Beispiel ist **kein gültiger Name** aus, da sie die gleiche Anzahl von Segmenten als Typ hat.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Haben für untergeordnete Ressourcen den Typ und den Namen die gleiche Anzahl von Segmenten. Diese Anzahl Segmente ist sinnvoll, da der vollständige Name und Typ für die untergeordneten umfasst übergeordneten Name und Typ. Daher enthält der vollständige Namen immer noch ein kleiner Segment als den vollständigen Typ aus. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Die Segmente rechts erste kann knifflig mit Ressourcenmanager Typen sein, die über Ressourcenanbieter angewendet werden. Anwenden einer Ressource Sperren zu einer Website erfordert beispielsweise einen Typ mit vier Abschnitten. Daher ist der Name drei Segmente:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Kopieren von Index wird nicht erwartet.

Dieser **InvalidTemplate** Fehler auftreten, wenn Sie das Element **Kopieren** zu einem Teil der Vorlage angewendet haben, die dieses Element nicht unterstützt. Sie können nur das Element kopieren in einen Ressourcentyp anwenden. Sie können keine Kopie auf eine Eigenschaft in einen Ressourcentyp anwenden. Beispielsweise eines virtuellen Computers kopieren zuweisen, aber Sie können sie auf die OS Datenträger für einen virtuellen Computer anwenden. In einigen Fällen können Sie eine untergeordnete Ressource an eine Ressource übergeordneten zum Erstellen einer Kopie Schleife konvertieren. Weitere Informationen zur Verwendung von kopieren finden Sie unter [mehrere Instanzen von Ressourcen in Azure Ressourcenmanager erstellen](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Der Parameter ist ungültig

Wenn die Vorlage gibt zulässigen Werte für einen Parameter an, und Sie geben Sie einen Wert, der nicht eine dieser Werte, erhalten Sie eine Meldung ähnlich wie der folgende Fehler auf:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Überprüfen Sie die zulässigen Werte in der Vorlage, und bieten Sie eine während der Bereitstellung.

## <a name="not-found-or-resource-not-found"></a>Nicht gefunden (oder Ressource nicht gefunden)

Wenn Sie Ihre Vorlage den Namen einer Ressource enthält, die aufgelöst werden kann, erhalten Sie eine ähnliche Fehlermeldung:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Wenn Sie versuchen, die fehlende Ressource in der Vorlage bereitstellen, überprüfen Sie, ob Sie eine Abhängigkeit hinzufügen müssen. Ressourcenmanager optimiert die Bereitstellung durch das Erstellen von Ressourcen parallel, falls möglich. Wenn eine Ressource nach einem anderen Ressourcen bereitgestellt werden muss, müssen Sie das Element **DependsOn** in Ihre Vorlage verwenden, um eine Abhängigkeit auf die anderen Ressource zu erstellen. Wenn Sie eine Web app bereitstellen zu können, muss beispielsweise der App-Serviceplan vorhanden. Wenn Sie nicht angegeben haben, dass das Web-app von der App-Serviceplan abhängt, erstellt Ressourcenmanager beide Ressourcen zur gleichen Zeit aus. Sie erhalten eine Fehlermeldung, dass die App-Dienst Plan Ressource gefunden werden kann, da es nicht noch vorhanden ist, wenn Sie versuchen, eine Eigenschaft für das Web app festzulegen. Sie können diesen Fehler durch Festlegen der Abhängigkeit in der Web-app verhindern.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Dieser Fehler wird auch angezeigt, wenn die Ressource in einer anderen Ressourcengruppe zu bereitgestellt wird als dem vorhanden ist. Verwenden Sie in diesem Fall die [ResourceId-Funktion](resource-group-template-functions.md#resourceid) können Sie um den vollqualifizierten Namen der Ressource zu gelangen.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Wenn Sie versuchen, die [Bezug](resource-group-template-functions.md#referenc) oder [ListKeys](resource-group-template-functions.md#listkeys) Funktionen mit einer Ressource zu verwenden, die nicht aufgelöst werden kann, erhalten Sie folgende Fehlermeldung:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Suchen Sie nach ein Ausdruck, der die **Verweis** -Funktion enthält. Überprüfen Sie, dass die gewünschten Parameterwerte richtig sind.

## <a name="storage-account-already-exists-or-already-taken"></a>Speicherkonto bereits vorhanden ist (oder bereits geöffnet)

Für Speicherkonten müssen Sie einen Namen für die Ressource angeben, die Azure eindeutig ist. Wenn Sie einen eindeutigen Namen keine bieten, erhalten Sie eine Fehlermeldung wie:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Sie können einen eindeutigen Namen durch Verketten Ihrer Benennungskonvention mit dem Ergebnis der Funktion [UniqueString](resource-group-template-functions.md#uniquestring) erstellen.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Wenn Sie ein Speicherkonto mit demselben Namen wie ein vorhandenes Speicherkonto in Ihrem Abonnement bereitstellen, aber einen anderen Speicherort bereitstellen, erhalten Sie eine Fehlermeldung, die angibt, dass das Speicherkonto bereits in einem anderen Speicherort vorhanden ist. Löschen Sie das vorhandene Speicherkonto, oder geben Sie am selben Speicherort wie das vorhandene Speicherkonto.

## <a name="account-name-invalid"></a>Ungültige Kontonamen

Den Fehler **AccountNameInvalid** zu sehen, wenn bei dem Versuch, ein Speicherkonto einen Namen zu geben, der enthält Zeichen nicht zulässig. Kontonamen Speicher müssen zwischen 3 und 24 Zeichen lang sein und Zahlen und nur Kleinbuchstaben verwenden.

## <a name="no-registered-provider-found"></a>Keine registrierten Anbieter gefunden

Beim Bereitstellen von Ressourcen möglicherweise die folgenden Fehlercode und eine Fehlermeldung angezeigt:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Dieser Fehler wird für eine der drei Gründe:

1. Speicherort für die Ressourcenart nicht unterstützt.
2. API-Version für den Ressourcentyp nicht unterstützt.
3. Der Anbieter für Ressourcen wurde für Ihr Abonnement nicht registriert

Die Fehlermeldung erhalten Sie Vorschläge für die unterstützten Speicherorte und API Versionen. Sie können eine der vorgeschlagenen Werte Ihrer Vorlage ändern. Die meisten Anbieter sind eingetragene automatisch nach dem Azure-Portal oder die Line-Benutzeroberfläche, die Sie verwenden, aber nicht alle. Wenn Sie einen bestimmte Ressource-Anbieter, bevor nicht verwendet haben, müssen Sie diese Anbieter zu registrieren. Sie können weitere Informationen zu Ressourcenanbieter über PowerShell oder Azure CLI ermitteln.

### <a name="powershell"></a>PowerShell

Wenn Ihr Registrierungsstatus anzeigen möchten, verwenden Sie **Get-AzureRmResourceProvider**aus.

    Get-AzureRmResourceProvider -ListAvailable

Um einen Anbieter zu registrieren, verwenden Sie **Register-AzureRmResourceProvider** , und geben Sie den Namen des Ressourcenanbieters, die Sie erfassen möchten.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Verwenden Sie die unterstützten Speicherorte für einen bestimmten Typ von Ressourcen, um:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Verwenden Sie die unterstützten API Versionen für einen bestimmten Typ von Ressourcen, um:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure CLI

Verwenden, um festzustellen, ob der Anbieter registriert ist die `azure provider list` Befehl.

    azure provider list

Um eine Ressourcenanbieter zu registrieren, verwenden Sie die `azure provider register` Befehl aus, und geben Sie den *Namespace* registrieren.

    azure provider register Microsoft.Cdn

Um die unterstützten Speicherorte und API Versionen für eine Ressourcenanbieter angezeigt wird, verwenden:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Vorgang nicht zulässig.

Bereitstellung Kontingent überschreitet die pro Ressourcengruppe, Abonnements, Konten und anderen Bereichen sein könnten, haben Sie möglicherweise Probleme. Beispielsweise kann Ihr Abonnement beschränken Sie die Anzahl der Kerne für eine Region konfiguriert sein. Wenn Sie versuchen, einen virtuellen Computer mit mehr Kerne als die zulässige Menge bereitstellen, erhalten Sie eine Fehlermeldung, die besagt, dass das Kontingent überschritten wurde.
Vollständige Kontingentinformationen finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente und Einschränkungen](azure-subscription-service-limits.md).

Um Ihr Abonnement des Kontingente für Kerne untersuchen, können Sie die `azure vm list-usage` in der CLI Azure Befehl. Im folgenden Beispiel wird veranschaulicht, dass das Kontingent Core für ein kostenlosen Testkonto 4 ist:

    azure vm list-usage

Diese Funktion gibt zurück:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Wenn Sie eine Vorlage, die mehr als vier Kerne in der Region Westen US erstellt wird bereitstellen, erhalten Sie einen Bereitstellungsfehler, der aussieht:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Oder in PowerShell, können Sie das Cmdlet " **Get-AzureRmVMUsage** " verwenden.

    Get-AzureRmVMUsage

Diese Funktion gibt zurück:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

In diesen Fällen sollten Sie wechseln Sie zu dem Portal und Ablegen ein Problem Support, um das Kontingent für die Region auszulösen, in die Sie bereitstellen möchten.

> [AZURE.NOTE] Denken Sie daran, dass für die Ressourcengruppen, das Kontingent für jede einzelne Region, nicht für das gesamte Abonnement ist. Wenn Sie 30 Kernen Westen US bereitstellen müssen, müssen Sie für 30 Ressourcenmanager Kernen Westen US Fragen. Wenn Sie müssen 30 Kerne bereitstellen dazu die Regionen, dem Sie Zugriff haben, sollten Sie für alle Regionen 30 Ressourcenmanager Kernen auffordern.

## <a name="invalid-content-link"></a>Ungültige Inhalts-link

Wenn Sie die Fehlermeldung angezeigt:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Sie haben wahrscheinlich versucht, zu einer Vorlage geschachtelte zu verknüpfen, die nicht verfügbar ist. Überprüfen Sie den URI, die Sie für die verschachtelte Vorlage bereitgestellt. Wenn die Vorlage in einem Speicherkonto vorhanden ist, stellen Sie sicher, dass der URI zugegriffen werden. Möglicherweise müssen Sie ein SAS Token übergeben. Weitere Informationen finden Sie unter [Verwendung von verknüpften Vorlagen Azure Ressourcenmanager](resource-group-linked-templates.md).

## <a name="authorization-failed"></a>Fehler bei der Autorisierung

Sie können während der Bereitstellung ein Fehler auftreten, da die Firma oder den Dienst Hauptbenutzer versuchen, die Ressourcen bereitgestellt Ausführen dieser Aktionen keinen Zugriff. Azure-Active Directory ermöglicht es Ihnen, oder der Administrator steuern, welche Identitäten welche Ressourcen mit einem attraktiven Maß Genauigkeit zugreifen kann. Angenommen, Ihr Konto die Rolle Leser zugewiesen ist, können Sie nicht um Ressourcen zu erstellen. In diesem Fall wird eine Fehlermeldung, dass die Autorisierung fehlgeschlagen ist.

Weitere Informationen zu Access rollenbasierte Steuerelement finden Sie unter [Azure_Role-Based Access Control](./active-directory/role-based-access-control-configure.md).

Zusätzlich zu rollenbasierte Access-Steuerelement möglicherweise Aktionen Bereitstellung von Richtlinien für das Abonnement beschränkt. Durch Richtlinien kann der Administrator Konventionen für alle Ressourcen bereitgestellt, in dem Abonnement erzwingen. Beispielsweise kann ein Administrator erfordern, dass ein bestimmtes Tag-Wert für einen Ressourcentyp bereitgestellt wird. Wenn Sie nicht die Richtlinie Vorschriften erfüllen, erhalten Sie eine Fehlermeldung während der Bereitstellung. Weitere Informationen zu den Richtlinien finden Sie unter [Verwenden von Richtlinien zum Verwalten von Ressourcen und Steuern des Zugriffs](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Problembehandlung bei virtuellen Computern

| Fehler | Artikel |
| -------- | ----------- |
| Benutzerdefinierte Erweiterung Skriptfehler | [Virtueller Windows-Computer Erweiterung Fehlern](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />oder<br />[Linux VM Erweiterung Fehlern](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Fehler bei der Bereitstellung OS-Bild | [Fehler bei der neuen virtuellen Windows-Computer](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />oder<br />[Neue Linux VM Fehler](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Zuteilung Fehlern | [Virtueller Windows-Computer Zuteilung Fehlern](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />oder<br />[Linux VM Zuteilung Fehlern](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Secure Shell (SSH) Fehler beim Herstellen einer Verbindung | [Secure Shell-Verbindungen mit Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Herstellen einer Verbindung mit virtuellen Computers ausgeführte Anwendung Fehler | [Windows virtuellen Computers ausgeführte Anwendung](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />oder<br />[Anwendung auf einer Linux VM ausführen](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Fehler bei der Remotedesktop-Verbindung | [Remote Desktop-Verbindungen mit virtuellen Windows-Computer](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Fehler bei der Verbindung durch die erneute Bereitstellung gelöst | [Stellen Sie erneut bereit virtuellen Computers zu neuen Azure-Knoten](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Fehler bei der Cloud-Dienst | [Probleme mit Cloud-Dienst Bereitstellung](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Problembehandlung bei anderen Diensten

In der folgenden Tabelle ist es sich nicht um eine vollständige Liste der Problembehandlung für Azure. In diesem Fall Schwerpunkt es Probleme bei der bereitstellen oder Konfigurieren von Ressourcen. Wenn Sie Hilfe zur Problembehandlung zur Laufzeit Probleme mit einer Ressource benötigen, finden Sie in der Dokumentation für diesen Dienst Azure.

| Dienst | Artikel |
| -------- | -------- |
| Automatisierung | [Tipps zur Problembehandlung für häufige Fehler bei der Azure-Automatisierung](./automation/automation-troubleshooting-automation-errors.md) |
| Azure Stapel | [Problembehandlung von Microsoft Azure Stapel](./azure-stack/azure-stack-troubleshooting.md) |
| Azure Stapel | [Web Apps und Azure Stapel](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Daten Factory | [Behandeln von Problemen mit Daten Factory](./data-factory/data-factory-troubleshoot.md) |
| Dienst Fabric | [Problembehandlung bei häufige Probleme bei der Bereitstellung von Dienstleistungen auf Azure Service Fabric](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Website-Wiederherstellung | [Überwachen Sie und Behandeln von Problemen mit Schutz für virtuellen Computern und physische Server](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Speicher | [Überwachen, diagnose und Problembehandlung bei Microsoft Azure-Speicher](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Behandeln von Problemen bei der Bereitstellung von StorSimple-Gerät](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-Datenbank | [Behandeln von Verbindungsproblemen mit Azure SQL-Datenbank](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL Datawarehouse | [Problembehandlung bei SQL Azure Datawarehouse](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Zu verstehen, wenn eine Bereitstellung bereit ist.

Azure Ressourcenmanager Berichte Erfolg auf eine Bereitstellung, wenn alle Anbieter erfolgreich aus Bereitstellung zurückgegeben. Jedoch bedeutet diese Nachricht nicht unbedingt, dass Ihre Ressourcengruppe "aktiv und für Ihre Benutzer bereit." ist. Beispielsweise möglicherweise eine Bereitstellung von Upgrades herunterladen, warten auf Ressourcen ohne Vorlage oder komplexe Schriftzeichen installieren müssen. Ressourcenmanager weiß nicht, wenn diese Aufgaben abgeschlossen haben, weil sie kein Anbieter verfolgt Aktivitäten sind. In diesen Fällen kann es einige Zeit, bevor Sie Ihre Ressourcen praktisches einsatzbereit sind sein. Daher sollten Sie erwarten, dass der Bereitstellungsstatus erfolgreich ist, einige Zeit vor die Bereitstellung verwendet werden kann.

Sie können verhindern, dass Azure Erfolgsmeldung Bereitstellung, jedoch, indem Sie ein benutzerdefiniertes Skript für Ihre benutzerdefinierte Vorlage erstellen. Das Skript weiß, wie Sie die gesamte Bereitstellung für das ganze System Readiness überwachen und gibt erfolgreich nur, wenn Benutzer die gesamte Bereitstellung interagieren können. Wenn Sie, um sicherzustellen, dass möchten ist die Erweiterung der letzten ausführen, verwenden Sie die Eigenschaft **DependsOn** in der Vorlage ein.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Überwachung von Aktionen, finden Sie unter [Audit Vorgänge mit Ressourcen-Manager](resource-group-audit.md).
- Weitere Informationen zu Aktionen zum Bestimmen der Fehler während der Bereitstellung finden Sie unter [die Bereitstellungsvorgänge anzeigen](resource-manager-troubleshoot-deployments-portal.md).
