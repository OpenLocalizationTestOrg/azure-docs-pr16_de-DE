<properties
    pageTitle="Fortlaufende Integration in im Vergleich mit einer Team Services mit Azure Ressourcengruppe Projekte | Microsoft Azure"
    description="Beschreibt, wie mithilfe von Azure Ressourcengruppe Bereitstellungsprojekte in Visual Studio fortlaufende Integration in Visual Studio Team Services einrichten."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Fortlaufende Integration in Visual Studio Team Services mit Azure Ressourcengruppe Bereitstellungsprojekte

Um eine Vorlage Azure bereitstellen zu können, müssen Sie Aufgaben in den einzelnen Phasen durchlaufen ausführen: erstellen, testen, Kopieren in Azure (auch als "Staging" bezeichnet), und die Vorlage bereitstellen.  Es gibt zwei verschiedene Arten Zweck in Visual Studio Team Services (im Vergleich mit einer Team Services). Beide Methoden bieten die gleichen Ergebnisse, und wählen Sie also das Schema, das den Workflow am besten entspricht.

-   Hinzufügen von einem einzigen Schritt zu der Definition Ihrer erstellen, die das Skript PowerShell ausgeführt wird, das in das Projekt Ressourcengruppe Azure-Bereitstellung (Bereitstellen-AzureResourceGroup.ps1) enthalten ist. Das Skript Elemente kopiert, und klicken Sie dann die Vorlage bereitstellt.
-   Hinzufügen von mehreren im Vergleich mit einer Team Services Schritte, können Sie jeweils eine Phase Aufgabe erstellen.

Dieser Artikel beschreibt, wie Sie die erste Option (verwenden Sie eine eigene Definition zum Ausführen des PowerShell-Skripts) verwenden. Ein Vorteil dieser Option besteht darin, das Skript untersuchten Entwickler in Visual Studio das gleiche Skript aus, das im Vergleich mit einer Team Services verwendet wird. Dieses Verfahren setzt voraus, dass Sie bereits ein Visual Studio-Bereitstellungsprojekt in im Vergleich mit einer Team Services eingecheckt haben.

## <a name="copy-artifacts-to-azure"></a>Kopieren Sie die Elemente in Azure 

Unabhängig von dem Szenario Wenn Sie alle Elemente verfügen, die für die Bereitstellung der Vorlage, benötigt werden müssen Sie Azure Ressourcenmanager Zugriff darauf zu gewähren. Diese Elemente können Dateien umfassen:

-   Verschachtelte Vorlagen
-   Konfigurationsskripts und DSC Skripts
-   Anwendungsbinärdateien

### <a name="nested-templates-and-configuration-scripts"></a>Geschachtelte Vorlagen und Konfigurationsskripts
Bei Verwendung von Visual Studio bereitgestellten Vorlagen (oder mit Visual Studio Codeausschnitte erstellt), das PowerShell-Skript Stufen nicht nur die Elemente, die sie auch den URI für die Ressourcen für die verschiedenen Bereitstellungen parametrisiert. Das Skript und dann Elemente zu einem sicheren Container in Azure kopiert, erstellt ein SaS Token für Container, und übergibt diese Informationen an die Bereitstellung der Vorlage. Finden Sie unter erfahren Sie mehr über geschachtelte Vorlagen [eine Vorlage Bereitstellung zu erstellen](https://msdn.microsoft.com/library/azure/dn790564.aspx) .

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Einrichten von kontinuierlichen Bereitstellung im Vergleich mit einer Teamwebsite-Dienste

Zum Aufrufen des PowerShell-Skripts im Vergleich mit einer Teamwebsite-Dienste müssen Sie Ihre eigene Definition zu aktualisieren. Kurz gesagt, werden die Schritte aus: 

1.  Bearbeiten der Definition erstellen.
1.  Einrichten von Azure Autorisierung im Vergleich mit einer Teamwebsite-Dienste.
1.  Hinzufügen eines Azure PowerShell Buildschritts, die im Projekt Azure Ressourcengruppe Bereitstellung der PowerShell-Skript verweist auf.
1.  Legen Sie den Wert des Parameters *- ArtifactsStagingDirectory* für die Arbeit mit einem Projekt, die sich im Vergleich mit einer Team Services.

### <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Die folgenden Schritte führt Sie durch die notwendigen Schritte zur Bereitstellung fortlaufender im Vergleich mit einer Team Services konfigurieren 

1.  Bearbeiten der Definition Ihrer im Vergleich mit einer Team Services erstellen und Hinzufügen eines Azure PowerShell-Schritts erstellen. Wählen Sie die Definition erstellen unter der Kategorie **Definitionen erstellen** aus, und wählen Sie dann auf den Link **Bearbeiten** .

    ![][0]

1.  Fügen Sie neuer **Azure PowerShell** Build Schritte zur Definition erstellen hinzu, und wählen Sie dann die **hinzufügen erstellen Schritt...** Schaltfläche.

    ![][1]

1.  Wählen Sie die Kategorie **Bereitstellen Vorgang** aus, wählen Sie die Aufgabe **Azure PowerShell** , und wählen Sie dann auf die Schaltfläche **Hinzufügen** .

    ![][2]

1.  Wählen Sie im Schritt **Azure PowerShell** erstellen aus, und füllen Sie dann die zugehörigen Werte.

    1.  Wenn Sie bereits einen Azure Service-Endpunkt im Vergleich mit einer Team Services hinzugefügt haben, wählen Sie das Abonnement im **Azure-Abonnement** Dropdown-Listenfeld, und klicken Sie dann mit dem nächsten Abschnitt überspringen. 

        Wenn Sie einen Endpunkt Azure Service im Vergleich mit einer Teamwebsite-Dienste haben, müssen Sie eine hinzufügen. Dieser Unterabschnitt führt Sie durch den Vorgang. Wenn Ihr Konto Azure ein Microsoft-Konto (wie Hotmail) verwendet, müssen Sie die folgenden Schritte zum Abrufen einer Service Tilgungsanteile Authentifizierung.

    1.  Wählen Sie der Link **Verwalten** neben dem **Azure-Abonnement** Dropdown-Listenfeld aus.

        ![][3]

    1. Wählen Sie in der **Neuen Service-Endpunkts an** Dropdown-Listenfeld **Azure** aus.

        ![][4]

    1.  Klicken Sie im Dialogfeld **Azure-Abonnement hinzufügen** die Option **Dienst Tilgungsanteile** aus.

        ![][5]

    1.  Hinzufügen von Informationen zu Ihrem Abonnement Azure zu im Dialogfeld **Azure-Abonnement hinzufügen** . Sie müssen die folgenden Elemente bereitzustellen:
        -   Abonnement-Id
        -   Namen des Abonnements.
        -   Dienst Tilgungsanteile-Id
        -   Dienst Tilgungsanteile Schlüssel
        -   Mandanten-Id

    1.  Fügen Sie einen Namen Ihrer Wahl, um das Namenfeld **Abonnement** . Dieser Wert wird weiter unten in der Dropdownliste **Azure-Abonnement** im Vergleich mit einer Teamwebsite-Dienste angezeigt. 

    1.  Wenn Sie nicht, dass Ihre Azure-Abonnement-ID wissen, können Sie einen der folgenden Befehle verwenden, wie Sie zu können.
        
        Verwenden Sie für PowerShell-Skripts:

        `Get-AzureRmSubscription`

        Verwenden Sie für Azure-CLI:

        `azure account show`
    

    1.  Wenn eine ID Tilgungsanteile Dienst erhalten möchten, gehen Sie wie Dienst Tilgungsanteile Taste gedrückt, und Mandanten-ID in [Active Directory erstellen Anwendung und Dienst Tilgungsanteile mithilfe von Portal](resource-group-create-service-principal-portal.md) oder [ein Dienst Tilgungsanteile Azure Ressourcenmanager authentifizieren](resource-group-authenticate-service-principal.md).

    1.  Das Dialogfeld **Azure-Abonnement hinzufügen** die Werte Tilgungsanteile Service-ID, Dienst Tilgungsanteile Schlüssel und Mandanten-ID hinzu, und wählen Sie dann auf die Schaltfläche **OK** .

        Sie verfügen jetzt über eine gültige Dienst Tilgungsanteile zu verwenden, um das Skript Azure PowerShell ausgeführt.

1.  Bearbeiten der Definition erstellen, und wählen Sie im Schritt **Azure PowerShell** erstellen. Wählen Sie das Abonnement im **Azure-Abonnement** Dropdown-Listenfeld aus. (Wenn das Abonnement nicht angezeigt wird, wählen Sie der Schaltfläche **Aktualisieren** als Nächstes auf den Link **Verwalten** .) 

    ![][8]

1.  Geben Sie einen Pfad zum Bereitstellen AzureResourceGroup.ps1 PowerShell-Skript ein. Dazu wählen Sie das Auslassungszeichen (...) neben dem Feld **Pfad Skript** , navigieren Sie zu der bereitstellen-AzureResourceGroup.ps1 PowerShell-Skript im Ordner **Skripts** Ihres Projekts, wählen Sie ihn aus, und wählen Sie dann auf die Schaltfläche **OK** . 

    ![][9]

1. Nachdem Sie das Skript ausgewählt haben, aktualisieren Sie den Pfad zu dem Skript, damit sie von der Build.StagingDirectory (das gleiche Verzeichnis, die *ArtifactsLocation* festgelegt wurde,) ausgeführt wird. Sie können diesem Zweck hinzufügen "$(Build.StagingDirectory)/"an den Anfang des Pfads Skript.

    ![][10]

1.  Geben Sie im Feld **Skriptargumente** die folgenden Parameter (in einer einzigen Zeile) ein. Wenn Sie das Skript in Visual Studio ausführen, können Sie sehen, wie im Vergleich mit einer der Parameter im **Ausgabefenster** verwendet. Sie können diese als Ausgangspunkt verwenden, zum Einrichten der Parameterwerte in Ihrem Erstellungsschritt.

  	| Parameter | Beschreibung|
  	|---|---|
  	| -ResourceGroupLocation           | Der geografischen Position-Wert, wo sich die Ressourcengruppe, z. B. **Eastus** oder **' ostasiatischen US '**befindet. (Fügen Sie einfache Anführungszeichen, ist es ein Leerzeichen im Namen.) Weitere Informationen finden Sie unter [Azure Regionen](https://azure.microsoft.com/en-us/regions/) .|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Der Name der Ressourcengruppe für diese Bereitstellung verwendet werden soll.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Für diesen Parameter, gibt an, sofern vorhanden, dass Elemente aus dem lokalen System in Azure hochgeladen werden müssen. Sie müssen diese Option festgelegt, wenn eine Vorlage Bereitstellung zusätzliche Elemente erforderlich ist, die Sie mithilfe des PowerShell-Skripts (z. B. Konfigurationsskripts oder verschachtelte Vorlagen) Phaseneigenschaften möchten.                                                                                                                                                                 |
  	| -StorageAccountName              | Der Name des Speicherkontos verwendet, um Elemente Phase für diese Bereitstellung. Für diesen Parameter ist erforderlich, nur, wenn Sie Elemente in Azure kopieren möchten. Dieses Speicherkonto nicht automatisch nach der Bereitstellung erstellt, es muss bereits vorhanden sein.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Der Name der Ressourcengruppe Speicher-Konto zugeordnet werden soll. Für diesen Parameter ist erforderlich, nur, wenn Sie Elemente in Azure kopieren möchten.|                                                                                                                                                                                                                                                                                                                               |
  	| TemplateFile-                    | Der Pfad der Vorlagendatei im Projekt Bereitstellung Azure Ressourcengruppe. Um die Flexibilität zu erhöhen, verwenden Sie einen Pfad für diesen Parameter, die relativ zum Speicherort des PowerShell Skripts statt ein absoluter Pfad ist.|
  	| -TemplateParametersFile          | Der Pfad zu der Parameterdatei im Projekt Bereitstellung Azure Ressourcengruppe. Um die Flexibilität zu erhöhen, verwenden Sie einen Pfad für diesen Parameter, die relativ zum Speicherort des PowerShell Skripts statt ein absoluter Pfad ist.|
  	| -ArtifactStagingDirectory        | Für diesen Parameter kann die PowerShell Skript kennen den Ordner aus, in dem die binäre Projektdateien kopiert werden sollen. Dieser Wert überschreibt den Standardwert von PowerShell-Skript verwendet. Legen Sie den Wert für im Vergleich mit einer Team Services verwenden, um: - ArtifactStagingDirectory $(Build.StagingDirectory)                                                                                                                                                                                              |

    Hier ist ein Skript Argumente Beispiel (Linie zur Verbesserung der Lesbarkeit fehlerhaft):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Wenn Sie fertig sind, sollte das **Skriptargumente** Feld wie folgt aus.

    ![][11]

1.  Nachdem Sie, dass alle erforderlichen Elemente in der PowerShell Azure Schritt erstellen hinzugefügt haben, wählen Sie die **Warteschlange** Generator-Schaltfläche, um das Projekt zu erstellen. Der Bildschirm **Erstellen** zeigt die Ausgabe der PowerShell-Skript.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Übersicht Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md) Weitere Informationen zur Azure Ressourcenmanager und Azure Ressourcengruppen.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
