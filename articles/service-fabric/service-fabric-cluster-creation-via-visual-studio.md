<properties
   pageTitle="Einrichten von einem Dienst Fabric Cluster mit Visual Studio | Microsoft Azure"
   description="Beschreibt, wie Sie einen Dienst Fabric Cluster einrichten, mithilfe einer Ressourcengruppe Azure-Projekt in Visual Studio erstellte Ressourcenmanager Azure-Vorlage"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Einrichten von einem Dienst Fabric Cluster mit Visual Studio
Dieser Artikel beschreibt, wie eine Azure Service Fabric Cluster mithilfe von Visual Studio und eine Vorlage Azure Ressourcenmanager eingerichtet. Wir verwenden einem Gruppenprojekt für Visual Studio Azure Ressourcen, um die Vorlage zu erstellen. Nachdem die Vorlage erstellt wurde, können sie in Visual Studio direkt auf Azure bereitgestellt werden. Sie können auch mit einem Skript oder als Teil der Einrichtung fortlaufende Integration (CI) verwendet werden.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Erstellen Sie Dienst Fabric Cluster Vorlage, indem Sie mit einem Gruppenprojekt Azure Ressource
Um anzufangen, öffnen Sie Visual Studio und erstellen Sie eine Ressource Azure Gruppenprojekt (es steht im Ordner " **Cloud** "):

![Dialogfeld "Neues Projekt" Ressourcengruppe Azure-Projekt aus][1]

Sie können eine neue Visual Studio-Lösung für dieses Projekt erstellen oder fügen Sie es in eine vorhandene Lösung.

>[AZURE.NOTE] Wenn Sie die Ressource Azure Gruppenprojekt unter dem Cloud-Knoten nicht angezeigt werden, müssen Sie nicht das Azure SDK installiert. Starten Sie Web Platform Installer ([es jetzt installieren](http://www.microsoft.com/web/downloads/platform.aspx) , wenn Sie noch nicht getan haben), und suchen Sie nach "Azure SDK für .NET", und installieren Sie die Version, die mit Ihrer Version von Visual Studio kompatibel ist.

Nachdem Sie die Schaltfläche OK getroffen haben, werden Visual Studio gebeten, die Ressourcenmanager Vorlage auszuwählen, die Sie erstellen möchten:

![Wählen Sie Vorlage Azure-Dialogfeld mit ausgewählten Dienst Fabric Cluster-Vorlage][2]

Wählen Sie den Dienst Fabric Cluster Vorlage aus, und drücken Sie erneut auf die Schaltfläche OK. Das Projekt und die Vorlage Ressourcenmanager haben nun erstellt wurde.

## <a name="prepare-the-template-for-deployment"></a>Vorbereiten der Vorlage für Bereitstellung
Bevor Sie die Vorlage zum Erstellen des Clusters bereitgestellt wird, müssen Sie die Werte für die erforderlichen Vorlagenparameter angeben. Diese Parameterwerte gelesen werden, aus der `ServiceFabricCluster.parameters.json` -Datei, die in der `Templates` Ordner des Projekts Gruppe Ressourcen. Öffnen Sie die Datei, und geben Sie die folgenden Werte:

|Parameternamen           |Beschreibung|
|-----------------------  |--------------------------|
|adminUserName            |Der Name des Administratorkontos für Dienst Fabric Maschinen (Knoten).|
|certificateThumbprint    |Der Fingerabdruck des Zertifikats, das den Cluster sichert.|
|sourceVaultResourceId    |Die *Ressourcen-ID* des die wichtigsten Tresor das Zertifikat, das den Cluster sichert gespeichert ist.|
|certificateUrlValue      |Die URL des Zertifikats Cluster Sicherheit.|

Die Vorlage Ressourcenmanager für Visual Studio Service Fabric erstellt einen sicheren Cluster, der durch ein Zertifikat geschützt ist. Dieses Zertifikat wird durch die letzten drei Vorlagenparameter identifiziert (`certificateThumbprint`, `sourceVaultValue`, und `certificateUrlValue`), und er muss in einer **Azure Schlüssel Tresor**vorhanden. Weitere Informationen zum Erstellen von des Sicherheitszertifikats Cluster [Service Fabric cluster Sicherheitsszenarien](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) finden Sie im Artikel.

## <a name="optional-change-the-cluster-name"></a>Optional: Ändern des Clusternamens
Jeder Dienst Fabric Cluster hat einen Namen. Wenn ein Fabric Cluster in Azure erstellt wird, bestimmt Clustername (zusammen mit der Azure Region) den Namen (DNS = Domain Name System) für den Cluster. Wenn Sie Ihren Cluster benennen, z. B. `myBigCluster`, und der Speicherort (Azure Region) der Ressourcengruppe, die den neuen Cluster hostet ostasiatischen US ist, der DNS-Name der Cluster `myBigCluster.eastus.cloudapp.azure.com`.

Standardmäßig ist der Clustername automatisch generiert und vorgenommen eindeutige durch eine zufällige Suffix "Cluster" Präfix anfügen. Dies erleichtert die Vorlage als Teil eines Systems **fortlaufende Integration** (CI) verwendet. Wenn Sie einen bestimmten Namen für Ihren Cluster verwenden möchten, die Sie von Bedeutung ist legen Sie den Wert von der `clusterName` Variable in die Vorlagendatei Ressourcenmanager (`ServiceFabricCluster.json`) auf der ausgewählten Namen. Die erste Variable in dieser Datei definiert ist.

## <a name="optional-add-public-application-ports"></a>Optional: Hinzufügen von öffentlichen Anwendungsports
Sie möchten möglicherweise auch ändern die öffentliche Anwendungsports für den Cluster vor der Bereitstellung. Die Vorlage wird standardmäßig nur zwei öffentlichen TCP-(80 und 8081) geöffnet. Wenn Sie weitere für die Anwendung benötigen, ändern Sie die Definition Azure Lastenausgleich in der Vorlage. Die Definition befindet sich an das Hauptfenster Vorlagendatei (`ServiceFabricCluster.json`). Öffnen Sie die Datei, und suchen Sie nach `loadBalancedAppPort`. Jeder Port gibt es drei Elemente:

1. Eine Vorlage-Variable, die den Wert TCP Port für den Port definiert:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. Einen *Prüfpunkt* , die definiert, wie häufig und wie lange der Azure Auslastung Lastenausgleich versucht, einen bestimmten Dienst Fabric Knoten vor einem über zu einem anderen Platzhalter verwenden. Die Prüfpunkte sind Teil der Lastenausgleich Ressource an. So sieht die Prüfpunkt Definition für den ersten Anwendung Standardport aus:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. Eine *Regel für den Lastenausgleich* , die den Port als auch der Prüfpunkt, wodurch Lastenausgleich über einen Satz von Dienst Fabric Clusterknoten ausgedrückt:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Wenn die Anwendung, die Sie zum Cluster bereitstellen möchten mehr Ports benötigen, können Sie diese durch Erstellen zusätzlicher Prüfpunkt und den Lastenausgleich Regeldefinitionen hinzufügen. Weitere Informationen zum Arbeiten mit Azure Lastenausgleich durch Ressourcenmanager Vorlagen finden Sie unter [Erste Schritte beim Erstellen eines internen Lastenausgleich mithilfe einer Vorlage](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Bereitstellen Sie die Vorlage mit Visual Studio
Nachdem Sie alle erforderlichen Parameterwerte in gespeichert haben die`ServiceFabricCluster.param.dev.json` Datei, sind Sie bereit sind, die Vorlage bereitstellen und Ihrem Dienst Fabric Cluster erstellen. Mit der rechten Maustaste in die Ressource Gruppenprojekt Explorer für Visual Studio-Lösung, und wählen Sie **Bereitstellen | Neue Bereitstellung...**. Falls erforderlich, zeigt Visual Studio im Dialogfeld **Bereitstellen Ressourcengruppe** mit der Frage, Azure authentifizieren:

![Bereitstellen Sie für Dialogfeld Ressourcengruppe][3]

Das Dialogfeld können Sie eine vorhandene Ressourcenmanager Ressourcengruppe für den Cluster auswählen und bietet Ihnen die Möglichkeit zum Erstellen eines neuen Kontos. Es sinnvoll normalerweise eine separate Ressourcengruppe für einen Dienst Fabric Cluster verwenden.

Nachdem Sie die Schaltfläche bereitstellen getroffen haben, werden Sie von Visual Studio aufgefordert, die Vorlage Parameterwerte zu bestätigen. Drücken Sie die Schaltfläche **Speichern** . Einen Parameter hat keinen gespeicherten Wert: das administrative Kontokennwort für den Cluster. Sie müssen ein Kennwortwert eingegeben werden, wenn Visual Studio für eine aufgefordert werden.

>[AZURE.NOTE] Beginnend mit Azure SDK 2.9, unterstützt Visual Studio Lesebereich Kennwörter aus **Azure Schlüssel Tresor** während der Bereitstellung. Klicken Sie im Dialogfeld Vorlage Parameter Beachten Sie, dass die `adminPassword` Parameter-Textfeld enthält ein kleines "Key"-Symbol auf der rechten Seite. Dieses Symbol können Sie eine vorhandene Key Tresor geheim als das Administratorkennwort für den Cluster auswählen. Stellen Sie sicher nur zum ersten Aktivieren Ressourcenmanager Azure Access für die Bereitstellung der Vorlage in die erweiterte Richtlinien von Ihrem Key Tresor. 

Sie können den Fortschritt des Bereitstellungsprozesses im Ausgabefenster Visual Studio überwachen. Sobald die Vorlage Bereitstellung abgeschlossen ist, ist Ihre neue Cluster bereit sind, verwenden Sie!

>[AZURE.NOTE] Wenn PowerShell nie verwendet wurde, Azure vom Computer aus zu verwalten, die Sie verwenden, müssen Sie etwas Wartungsaufgaben zu erledigen.
>1. Aktivieren Sie das PowerShell scripting durch Ausführen der [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) Befehl. Für die Entwicklung Maschinen ist "uneingeschränkte" Richtlinie normalerweise ausreichend.
>2. Entscheiden, ob diagnostic Datensammlung Azure PowerShell-Befehle zulassen, und führen Sie [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) oder [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) nach Bedarf. Dies wird nicht benötigte Anweisungen während der Bereitstellung der Vorlage vermeiden.

Wenn Fehler vorhanden sind, wechseln Sie zum [Azure-Portal](https://portal.azure.com/) , und öffnen Sie die Ressourcengruppe aus, der Sie in bereitgestellt. Klicken Sie auf **Alle Einstellungen**und dann die Einstellungen Blade **Bereitstellungen** auf. Fehler bei der Ressourcengruppe Bereitstellung belässt ausführlichen Diagnoseinformationen vorhanden.

>[AZURE.NOTE] Dienst Fabric Cluster erfordern eine bestimmte Anzahl von Knoten von werden Verfügbarkeit und Beibehalten des Status - genannt "Verwalten Quorum". Daher ist es nicht sicher ist, fahren Sie alle Computer im Cluster zu herunter, es sei denn, Sie zuerst eine [vollständige Sicherung Ihres](service-fabric-reliable-services-backup-restore.md)durchgeführt haben.

## <a name="next-steps"></a>Nächste Schritte
- [Informationen Sie zum Einrichten der Dienst Fabric Cluster über das Azure-portal](service-fabric-cluster-creation-via-portal.md)
- [Informationen Sie zum Verwalten und Dienst Fabric Applikationen mit Visual Studio bereitstellen](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
