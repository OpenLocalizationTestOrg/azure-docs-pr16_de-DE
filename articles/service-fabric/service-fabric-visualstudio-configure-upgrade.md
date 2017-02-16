<properties
   pageTitle="Konfigurieren des Upgrades einer Fabric Service-Anwendung | Microsoft Azure"
   description="Erfahren Sie, wie die Einstellungen für das Upgrade einer Fabric Service-Anwendungs mithilfe von Microsoft Visual Studio konfigurieren."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Konfigurieren Sie das Upgrade einer Fabric Service-Anwendung in Visual Studio

Visual Studio-Tools für Azure Service Fabric bieten Upgrade Unterstützung für die Veröffentlichung auf lokale oder remote-Cluster an. Es gibt zwei Vorteile für ein Upgrade Ihrer Anwendung auf eine neuere Version anstelle die Anwendung während testen und Debuggen ersetzen möchten:

- Anwendungsdaten wird nicht bei der Aktualisierung verloren.
- Verfügbarkeit bleiben hoch, sodass es können dienstunterbrechung während des Upgrades nicht vorhanden sind, dass genügend Dienstinstanzen auf Upgrade Domänen verteilt.

Tests können für eine Anwendung ausgeführt werden, während sie aktualisiert wird.

## <a name="parameters-needed-to-upgrade"></a>Parameter zum upgrade erforderlich sind

Sie können zwei Arten von Bereitstellung auswählen: regulären oder Upgrade. Eine normale Bereitstellung löscht alle vorherigen Informationen zur Bereitstellung und Daten auf dem Cluster, während eine Upgrade Bereitstellung beibehalten. Wenn Sie eine Fabric Service-Anwendung in Visual Studio aktualisieren, müssen Sie angeben, dass die Anwendung Upgrade Parameter und Gesundheit Richtlinien überprüfen. Upgrade Anwendungsparameter kontrollieren die Aktualisierung während Gesundheit Kontrollkästchen Richtlinien bestimmen, ob die Aktualisierung erfolgreich war. Finden Sie unter [Fabric Service-Anwendungsupgrade: Aktualisieren der Parameter](service-fabric-application-upgrade-parameters.md) für weitere Details.

Es gibt drei Upgrade Modi: *überwacht*, *UnmonitoredAuto*und *UnmonitoredManual*.

  - Ein Upgrade überwacht Automatisierung des Upgrades und integritätsprüfung der Anwendung.

  - Ein Upgrade UnmonitoredAuto Automatisierung des Upgrades, aber die Anwendung integritätsprüfung überspringt.

  - Wenn Sie ein Upgrade UnmonitoredManual ausführen, müssen Sie jede Upgrade Domäne manuell aktualisieren.

Jeder Upgrade Modus erfordert unterschiedliche Optionssätze Parameter. Finden Sie unter [upgrade Anwendung Parameter](service-fabric-application-upgrade-parameters.md) , erfahren Sie mehr über die verfügbaren Upgrade-Optionen.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Aktualisieren einer Fabric Service-Anwendung in Visual Studio

Wenn Sie die Tools für Visual Studio-Dienst Fabric, ein upgrade von einer Fabric Service-Anwendung zu verwenden, können Sie einen Veröffentlichungsprozess benutzerspezifisch Aktualisierung statt eine normale Bereitstellung durch Aktivieren des Kontrollkästchens **Aktualisieren Sie die Anwendung** angeben.

### <a name="to-configure-the-upgrade-parameters"></a>So konfigurieren Sie die Upgrade-Parameter

1. Klicken Sie auf die Schaltfläche " **Einstellungen** " neben dem Kontrollkästchen. Das Dialogfeld **Upgrade Parameter bearbeiten** wird angezeigt. Das Dialogfeld **Bearbeiten Upgrade Parameter** unterstützt das Upgrade Modi überwacht, UnmonitoredAuto und UnmonitoredManual.

2. Wählen Sie das Upgrade Modus, den Sie verwenden möchten und füllen Sie dann das Parameterraster aus.

    Jeder Parameter hat Standardwerten. Der optionale Parameter *DefaultServiceTypeHealthPolicy* nimmt die Eingabe eines Hash-Tabelle. Hier ist ein Beispiel für die Eingabewerte Tabelle Hashformat für *DefaultServiceTypeHealthPolicy*ein:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* ist ein anderes optionalen Parameter, der Eingabe eines Hash Tabelle in folgendem Format annimmt:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    So sieht ein Beispiel Praxis aus:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Wenn Sie Upgrade UnmonitoredManual-Modus aktivieren, müssen Sie manuell starten eine PowerShell-Konsole um fortzusetzen und den Upgradevorgang abzuschließen. Verweisen auf [Fabric Service-Anwendungsupgrade: Erweiterte Themen](service-fabric-application-upgrade-advanced.md) zu erfahren Sie, wie die manuelle Aktualisierung funktioniert.

## <a name="upgrade-an-application-by-using-powershell"></a>Aktualisieren einer Anwendung mithilfe der PowerShell

PowerShell-Cmdlets können Sie eine Fabric Service-Anwendung aktualisieren. Ausführliche Informationen finden Sie unter [upgrade Lernprogramm Fabric Service-Anwendung](service-fabric-application-upgrade-tutorial.md) und [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) .

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Angeben einer Gesundheit Kontrollkästchen Richtlinie in der Anwendungsmanifestdatei

Jeder Dienst in einer Fabric Service-Anwendung kann eigene Gesundheit Richtlinienparameter haben, die die Standardwerte außer Kraft setzen. Sie können die folgenden Parameterwerte in der Anwendungsmanifestdatei bereitstellen.

Im folgenden Beispiel wird gezeigt, wie Sie keine eindeutige Gesundheit Kontrollkästchen Richtlinien für jeden Dienst im Anwendungsmanifest anwenden.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Bereitstellen von Anwendung finden Sie unter [Bereitstellen einer vorhandenen Anwendung in Azure Service Architektur](service-fabric-deploy-existing-app.md).
