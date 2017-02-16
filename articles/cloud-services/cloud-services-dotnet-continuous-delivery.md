<properties
    pageTitle="Fortlaufender Übermittlung für Cloud services mit TFS in Azure | Microsoft Azure"
    description="Informationen Sie zum Einrichten der kontinuierlichen Bereitstellung für Azure-Cloud-apps. Codebeispielen für MSBuild Befehlszeile Anweisungen und PowerShell-Skripts."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Kontinuierlichen Bereitstellung von Cloud Services in Azure

Der in diesem Artikel beschriebenen Prozess wird gezeigt, wie kontinuierlichen Bereitstellung für Azure-Cloud-apps einrichten. Dieses Verfahren können Sie automatisch Pakete erstellen und Bereitstellen des Pakets in Azure nach jeder Code einchecken. Der in diesem Artikel beschriebenen Paket Build-Prozess entspricht der Befehl **Verpacken** in Visual Studio, und die Veröffentlichung Schritte sind gleichbedeutend mit dem Befehl **Veröffentlichen** in Visual Studio.
Der Artikel behandelt die Methoden, die Sie verwenden möchten, zu einen Server erstellen mit MSBuild Befehlszeile Anweisungen und Windows PowerShell-Skripts zu erstellen, und es auch veranschaulicht, wie Visual Studio Team Foundation Server - Team Build Definitionen und mit dem MSBuild Befehle PowerShell-Skripts optional konfigurieren. Der Vorgang ist für Ihre Umgebung erstellen und Azure Ziel Umgebungen anpassbare.

Sie können auch Visual Studio Team Services, eine Version von TFS verwenden, die in Azure Zweck einfacher gehostet wird. Weitere Informationen finden Sie unter [Kontinuierlichen Bereitstellung in Azure von Visual Studio Team Services verwenden][].

Bevor Sie beginnen, sollten Sie die Anwendung von Visual Studio veröffentlichen.
Dadurch wird sichergestellt, dass alle Ressourcen zur Verfügung und initialisierte werden, wenn Sie versuchen, die Publikation zu automatisieren.

## <a name="1-configure-the-build-server"></a>1: konfigurieren den Server erstellen

Bevor Sie ein Azure-Paket mithilfe von MSBuild erstellen können, müssen Sie die erforderlichen Software und Tools auf dem Server erstellen installieren.

Visual Studio muss nicht installiert werden, auf dem Server erstellen. Wenn Sie Team Foundation Build-Dienst verwenden, zum Verwalten Ihrer Servers erstellen möchten, gehen Sie in der Dokumentation [Team Foundation Build-Dienst][] .

1.  Installieren Sie [.NET Framework 4.5.2][], wozu auch MSBuild auf dem Server erstellen.
2.  Installieren Sie die neuesten [Azure Authoring-Tools für .NET](https://azure.microsoft.com/develop/net/).
3.  Installieren Sie die [Azure Bibliotheken für .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Kopieren Sie die Datei Microsoft.WebApplication.targets aus einer Visual Studio-Installation auf dem Server erstellen.

    Auf einem Computer mit Visual Studio installiert haben, befindet sich diese Datei im Verzeichnis C:\\Programm Files(x86)\\MSBuild\\Microsoft\\Visual Studio\\v14.0\\WebApplications. Sie sollten sie im selben Verzeichnis auf dem Server erstellen kopieren.
5.  Installieren Sie die [Azure Tools für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: erstellen ein Pakets mit MSBuild-Befehle

In diesem Abschnitt beschrieben, wie Sie einen MSBuild-Befehl zu erstellen, der ein Azure-Paket erstellt wird. Führen Sie diesen Schritt auf dem Server erstellen, um sicherzustellen, dass alles richtig konfiguriert ist und der Befehl MSBuild hat, was sie tun möchten. Sie können vorhandene Buildskripts auf dem Server erstellen entweder diese Befehlszeile hinzufügen, oder Sie können die Befehlszeile in einer TFS Build Definition verwenden, wie im nächsten Abschnitt beschrieben. Weitere Informationen zu Befehlszeilenparameter und MSBuild finden Sie unter [MSBuild Befehlszeile verweisen](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1.  Wenn auf dem Server erstellen Visual Studio installiert ist, suchen Sie, und wählen Sie in den Ordner für **Visual Studio-Tools** in Windows **Visual Studio Befehl auffordern** aus.

    Wenn Visual Studio auf die Generator-Server nicht installiert ist, öffnen Sie ein Eingabeaufforderungsfenster, und stellen Sie sicher, dass MSBuild.exe auf den Pfad zugegriffen werden. MSBuild mit .NET Framework im Ordner Pfad % installiert wird\\Microsoft.NET\\Framework\\*Version*. Geben Sie beispielsweise um MSBuild.exe der Pfad Umgebung Variablen hinzuzufügen, wenn Sie .NET Framework 4 installiert haben, mit dem folgenden Befehl in die Befehlszeile eingeben:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  Navigieren Sie zu dem Ordner mit der Azure Projektdatei, die Sie erstellen möchten, an der Eingabeaufforderung.

3.  Führen Sie MSBuild mit der/target: Veröffentlichen der Option wie im folgenden Beispiel gezeigt:

        MSBuild /target:Publish

    Diese Option als/t Abkürzung werden kann: veröffentlichen. Die Option /t:Publish in MSBuild darf nicht mit in Visual Studio verfügbaren Befehle veröffentlichen verwechselt werden, wenn Sie die Azure SDK installiert haben. / T: Veröffentlichen der Option nur Builds Azure-Paketen. Es wird nicht die Pakete bereitgestellt, so, wie die Befehle Veröffentlichen in Visual Studio.

    Optional können Sie den Namen des Projekts als MSBuild-Parameter angeben. Wenn nicht angegeben, wird das aktuelle Verzeichnis verwendet. Weitere Informationen zu MSBuild-Befehlszeilenoptionen finden Sie unter [MSBuild Befehlszeile verweisen](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

4.  Suchen Sie die Ausgabe an. Dieser Befehl erstellt standardmäßig ein Verzeichnis in Bezug auf den Stammordner für das Projekt, z. B. *ProjectDir*\\Papierkorb\\*Konfiguration*\\app.publish\\. Wenn Sie ein Azure-Projekt erstellen, generieren Sie zwei Dateien, die Paketdatei selbst und der zugehörigen Konfigurationsdatei:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Standardmäßig jedes Azure Projekt umfasst eine Konfiguration Dienstdatei (.cscfg-Datei) für lokale (Debuggen) Builds sowie eine weitere Builds Cloud (Staging oder Fertigung), aber Sie können Dateien hinzufügen oder entfernen Dienst Konfiguration nach Bedarf. Wenn Sie ein Paket in Visual Studio erstellen, werden Sie aufgefordert, die Konfiguration Dienstdatei entlang des Pakets aufnehmen möchten werden.

5.  Geben Sie an der Konfiguration Dienstdatei. Wenn Sie ein Paket mithilfe von MSBuild erstellen, ist standardmäßig die lokale Konfiguration Dienstdatei enthalten. Um eine andere Konfiguration Dienstdatei zu einzufügen, legen Sie die Eigenschaft TargetProfile des Befehls MSBuild wie im folgenden Beispiel:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Geben Sie den Speicherort für die Ausgabe an. Legen Sie den Pfad mithilfe der /p:PublishDir =*Verzeichnis* \\ Option, einschließlich das nachfolgende umgekehrter Schrägstrich Trennzeichen, wie im folgenden Beispiel gezeigt:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Nachdem Sie eine entsprechende MSBuild Befehlszeile um Ihre Projekte erstellen und kombinieren Sie sie in ein Azure-Paket getestet und erstellt haben, können Sie Ihre eigene Skripts dieser Befehlszeile hinzufügen. Wenn der Server Erstellen benutzerdefinierter Skripts verwendet, wird dieses Verfahren die Einzelheiten Ihrer benutzerdefinierten Buildprozess abhängig sind. Wenn Sie TFS als Build-Umgebung verwenden, können Sie die Anweisungen im nächsten Schritt zum Hinzufügen des Azure-Paket Builds Prozesses erstellen folgen.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: erstellen ein Pakets mit TFS Team Build

Wenn Sie haben als TFS Buildcomputer einrichten Team Foundation Server (TFS) richten Sie als einen Controller erstellen und auf dem Server erstellen, und dann können Sie optional Einrichten eines automatisierten Builds für Ihr Azure-Paket. Informationen zum Einrichten und Verwenden von Team Foundation Server als System erstellen finden Sie unter [skalieren, Ihrem System erstellen][]. Insbesondere wird das folgende Verfahren davon ausgegangen, dass Sie Ihre eigene Server konfiguriert haben, wie unter [Bereitstellen und Konfigurieren eines Servers Build][], Projekt einen Cloud-Dienst im Teamprojekt erstellt, und dass Sie ein Teamprojekt erstellt haben.

Führen Sie die folgenden Schritte aus, TFS Azure-Paketen erstellen um zu konfigurieren:

1.  Klicken Sie in Visual Studio auf Ihrem Entwicklungscomputer im Menü Ansicht **Team Explorer**auswählen, oder wählen Sie STRG +\\, STRG + M. Klicken Sie im Team Explorer-Fenster erweitern Sie den Knoten **erstellt** oder wählen Sie das Zeichenblatt **erstellt** , und wählen Sie **Neue Build Definition**.

    ![Neue Build Definition-option][0]

2.  Wählen Sie die Registerkarte **Trigger** , und die Geben Sie gewünschten Optionen für an, wenn Sie das Paket erstellt werden soll. Geben Sie beispielsweise **Fortlaufende Integration** das Paket erstellen bei jedem Quelle Steuerelement Einchecken eintreten an.

3.  Wählen Sie die Registerkarte **Quelle Einstellungen** aus, und stellen Sie sicher, Ihre Projektordner in der Spalte **Quelle Steuerelement Ordner** aufgeführt ist, und der Status ist **aktiv**.

4.  Wählen Sie die Registerkarte **Standardwerte erstellen** aus, und klicken Sie unter Controller erstellen, überprüfen Sie den Namen des Servers erstellen.  Auch, wählen Sie die Option **Kopie erstellen Ausgabe in den folgenden Dropdown-Ordner** aus, und geben Sie den Speicherort der gewünschten ablegen.

5.  Wählen Sie aus der Registerkarte **Prozess** . Klicken Sie auf der Registerkarte Prozess wählen Sie die Standardvorlage, wählen Sie unter **Erstellen**, und wählen Sie das Projekt aus, sofern dies noch nicht ausgewählt ist, und erweitern Sie den Abschnitt **Erweitert** im Abschnitt **Erstellen** des Rasters.

6.  Wählen Sie **MSBuild-Argumente**, und legen Sie die entsprechenden MSBuild Befehlszeilenargumente in Schritt2 oben beschriebenen. Geben Sie zum Beispiel **/t: Veröffentlichen /p:PublishDir =\\\\MeinServer\\löscht\\ ** erstellen ein Paket und kopieren die Paketdateien an die Position \\ \\MeinServer\\löscht\\:

    ![MSBuild-Argumente][2]

    **Hinweis:** Kopieren die Dateien in einem freigegebenen Verzeichnis vereinfacht die Pakete von Ihrem Entwicklungscomputer manuell bereitstellen.

5.  Testen Sie den Erfolg Ihrer Erstellungsschritt, indem Sie eine Änderung an Ihrem Projekt einchecken oder einen neuen Build eine Warteschlange. Wenn Sie einen neuen Build im Team-Explorer in die Warteschlange mit der rechten Maustaste **Alle Definitionen erstellen,** und wählen Sie dann auf **Neuen Build in Warteschlange**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: veröffentlichen ein Pakets mithilfe der PowerShell-Skript

In diesem Abschnitt beschrieben, wie ein Windows PowerShell-Skript zu erstellen, die Ausgabe der Cloud-app-Paket die optionale Parameter mit Azure veröffentlicht werden. Dieses Skript kann aufgerufen werden, nachdem der Build Schritt in Ihre eigene benutzerdefinierte Automatisierung. Sie können auch von Process Template Workflowaktivitäten in Visual Studio TFS Team Build aufgerufen werden.

1.  Installieren der [Azure-PowerShell-Cmdlets][] (v0.6.1 oder höher).
    Während der Phase der Installation Cmdlet wählen Sie aus, die als Snap-in installieren. Beachten Sie, dass dieser formal unterstützte Version, die ältere Version angeboten CodePlex, ersetzt jedoch früheren Versionen 2.x.x nummeriert wurden.

2.  Beginnen Sie Azure PowerShell mithilfe des Startmenüs oder Seite. Wenn Sie auf diese Weise starten, werden die Azure PowerShell-Cmdlets geladen.

3.  PowerShell dazu aufgefordert werden, stellen Sie sicher, dass die PowerShell-Cmdlets geladen sind durch Eingabe des Befehls teilweise `Get-Azure` und drücken die Tab-Taste für den Abschluss der Anweisung.

    Wenn Sie die Tab-Taste wiederholt drücken, sollte verschiedener Azure PowerShell Befehle angezeigt werden.

4.  Stellen Sie sicher, dass Sie Ihr Abonnement Azure durch Importieren von Informationen zu Ihrem Abonnement aus der Datei publishsettings verbinden können.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Geben Sie den Befehl

    `Get-AzureSubscription`

    Zeigt Informationen über Ihr Abonnement. Stellen Sie sicher, dass alles richtig ist.

4.  Speichern Sie die Vorlage für ein Skript finden Sie am Ende dieses Artikels in den Skriptordner als c:\\Skripts\\WindowsAzure\\**PublishCloudService.ps1**.

5.  Überprüfen Sie im Abschnitt Parameter des Skripts ein. Hinzufügen oder Ändern von Standardwerten. Diese Werte können durch die Übergabe explizite Parameter immer überschrieben werden.

6.  Sicherstellen Sie gültiger Cloud-Dienst und Speicher Konten in Ihr Abonnement erstellt werden, die das Skript Veröffentlichen verwendet werden kann. Hochladen und die Bereitstellung Paket und Config-Datei vorübergehend zu speichern, während der Erstellung der bereitstellungs wird das Speicherkonto (BLOB-Speicher) verwendet.

    -   Zum Erstellen eines neuen Cloud-Diensts können Sie dieses Skript anrufen oder das [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)verwenden. Der Name der Cloud-Dienst als Präfix in einen vollqualifizierten Domänennamen verwendet werden und muss daher eindeutig sein.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   Zum Erstellen eines neuen Kontos mit Speicher können Sie dieses Skript anrufen oder das [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)verwenden. Den Namen des Kontos Speicher als Präfix in einen vollqualifizierten Domänennamen verwendet werden und muss daher eindeutig sein. Versuchen Sie, ob Sie denselben Namen wie des Cloud-Diensts verwenden.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Rufen Sie das Skript direkt Azure PowerShell oder Verbinden Sie dieses Skript zu Ihrer Host erstellen Automatisierung nach der Erstellung Paket ausgeführt.

    >[AZURE.IMPORTANT] Das Skript wird immer löschen oder Bereitstellung Ihrer vorhandenen standardmäßig ersetzen, wenn sie erkannt werden. Dies ist erforderlich, vor aktivieren kontinuierlichen Bereitstellung von Automatisierung darin keine Aufforderung des Benutzers möglich.

    **Beispielszenario 1:** kontinuierlichen Bereitstellung auf das staging-Umgebung eines Diensts:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    Dies in der Regel Testlaufs Überprüfung und eine VIP austauschen folgt nach oben. Die VIP austauschen kann über das [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885) oder mithilfe des Cmdlets verschieben-Bereitstellung vorgenommen werden.

    **Beispielszenario 2:** kontinuierlichen Bereitstellung zu dieser Umgebung von einem Dienst zum Testen der dedizierten

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Remotedesktop:**

    Wenn Remotedesktop im Projekt Azure aktiviert ist, müssen Sie zusätzliche einmalige Schritte, um sicherzustellen, dass das richtige Zertifikat des Cloud-Dienst auf alle Cloud-Diensten, die von diesem Skript gerichtet hochgeladen wird ausführen.

    Suchen Sie die Werte des Zertifikats Fingerabdruck durch Ihre Rollen erwartet. Die Fingerabdruck-Werte werden im Abschnitt Zertifikate der Cloud Konfigurationsdatei (d. h. ServiceConfiguration.Cloud.cscfg) angezeigt. Es wird auch in Visual Studio im Dialogfeld Konfiguration des Remotedesktop angezeigt, wenn Sie die Anzeige von Optionen und Anzeigen des ausgewählten Zertifikats.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Laden Sie Remotedesktop Zertifikate als einmalige Einrichtung Schritt verwenden das folgende Cmdlet Skript aus:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Beispiel:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    Alternativ können Sie die Zertifikatsdatei PFX mit privaten Schlüssel exportieren und Zertifikate auf jedes Ziel Cloud-Dienst verwenden des [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen. 
    Lesen Sie den folgenden Artikel Weitere Informationen: [http://msdn.microsoft.com/library/windowsazure/gg443832.aspx] [].

    **Bereitstellung der Bereitstellung im Vergleich zu löschen - Upgrade\> neue Bereitstellung**

    Das Skript wird standardmäßig Ausführen einer Bereitstellung Upgrade ($enableDeploymentUpgrade = 1) bei ist kein Parameter in übergeben oder der Wert 1 wird explizit übergeben. Für einzelne Instanzen hat dies den Vorteil weniger Zeit als eine vollständige Bereitstellung aufzeichnen. Für Instanzen, die eine hohen Verfügbarkeit, den dies auch den Vorteil erfordern von einigen Fällen ausgeführt werden, während andere verlassen hat (durchlaufen Ihre Domäne aktualisieren) aktualisiert, sowie der VIP nicht gelöscht werden wird.

    Upgrade Bereitstellung in das Skript deaktiviert werden kann ($enableDeploymentUpgrade = 0) oder durch übergeben *- EnableDeploymentUpgrade 0* als Parameter, die das Skriptverhalten zu löschen Sie zunächst alle vorhandenen Bereitstellung und erstellen Sie eine neue Bereitstellung ändert.

    >[AZURE.IMPORTANT] Das Skript wird immer löschen oder Bereitstellung Ihrer vorhandenen standardmäßig ersetzen, wenn sie erkannt werden. Dies ist erforderlich, vor aktivieren kontinuierlichen Bereitstellung von Automatisierung darin keine Aufforderung des Benutzers/Operator möglich.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: veröffentlichen ein Pakets mithilfe von TFS Team Build

Mit diesem optionale Schritt verbindet TFS Team Build an das Skript in Schritt 4 erstellt die Veröffentlichung von das Paket erstellen Azure behandelt. Dies umfasst das Ändern der Prozessvorlage, mithilfe der Definition Ihrer erstellen, damit sie eine veröffentlichen Aktivität am Ende der Workflow ausgeführt wird. Die Aktivität veröffentlichen, wird den PowerShell-Befehl übergeben Parameter aus dem Build ausgeführt. Ausgabe der MSBuild ausgerichtet und Veröffentlichen von Skript wird in der Buildausgabe standard geleitet werden.

1.  Bearbeiten der Definition erstellen verantwortlich fortlaufender bereitstellen.

2.  Wählen Sie aus der Registerkarte **Prozess** .

3.  Führen Sie [diese Anweisungen](http://msdn.microsoft.com/library/dd647551.aspx) zum Hinzufügen eines Projekts Aktivität für die Vorlage erstellen, laden Sie die Standardvorlage, dem Projekt hinzuzufügen und checken Sie es aus. Geben Sie der Vorlage erstellen einen neuen Namen ein, beispielsweise AzureBuildProcessTemplate.

3.  Kehren Sie zu der Registerkarte **Prozess** zurück, und verwenden Sie **Details anzeigen** , um eine Liste der verfügbaren Build Prozessvorlagen anzuzeigen. Wählen Sie die Schaltfläche **neu...** , und navigieren Sie zu des Projekts, die Sie soeben hinzugefügt und eingecheckt. Suchen Sie die Vorlage, die Sie soeben erstellte, und wählen Sie **OK**aus.

4.  Die ausgewählte Prozessvorlage zur Bearbeitung zu öffnen. Sie können direkt im Workflow-Designer oder in der XML-Editor für die Arbeit mit XAML zu öffnen.

5.  Fügen Sie die folgende Liste der neuen Argumente als separate Zeile Elemente auf der Registerkarte Argumente im Workflowdesigner hinzu. Alle Argumente sollten Richtung haben = In, und geben Sie = Zeichenfolge. Diese werden zum Fluss Parameter aus der Definition erstellen in den Workflow, die dann zum Aufrufen der veröffentlichen gewöhnen.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Liste mit Argumenten zurück][3]

    Das entsprechende XAML sieht wie folgt aus:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Fügen Sie eine neue Sequenz am Ende des ausführen auf Agent hinzu:

    1.  Durch Hinzufügen einer Wenn-Anweisung Aktivität zu prüfen, ob eine gültige Skriptdatei beginnen. Festlegen Sie die Bedingung auf diesen Wert:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  Dann Groß-/Kleinschreibung der Wenn-Anweisung, fügen Sie eine neue Sequenz Aktivität hinzu. Festlegen des Anzeigenamens 'Starten veröffentlichen'

    3.  Veröffentlichen Sie bei markiertem Sequenz mit dem Anfang, fügen Sie die folgende Liste neue Variablen als separate Zeile Elemente auf der Registerkarte Variablen im Workflowdesigner hinzu. Alle Variablen müssen Variable vom Typ = Zeichenfolge und Umfang = Anfang veröffentlichen. Diese werden zum Fluss Parameter aus der Definition erstellen in den Workflow, die dann zum Aufrufen der veröffentlichen gewöhnen.

        -   SubscriptionDataFilePath, vom Typ String

        -   PublishScriptFilePath, vom Typ String

            ![Neue Variablen][4]

    4.  Wenn Sie TFS 2012 oder einer früheren Version verwenden, fügen Sie eine ConvertWorkspaceItem-Aktivität am Anfang der neuen Sequenz. Wenn Sie TFS 2013 oder höher verwenden, fügen Sie eine GetLocalPath Aktivität am Anfang der neuen Sequenz. Für eine ConvertWorkspaceItem, legen Sie die Eigenschaften wie folgt: Richtung = ServerToLocal, DisplayName = 'Konvertieren veröffentlichen Skript Filename' Eingabe = PublishScriptLocation, Ergebnis = PublishScriptFilePath, Arbeitsbereich = "Arbeitsbereich". Legen Sie für eine Aktivität GetLocalPath Eigenschaft IncomingPath auf 'PublishScriptLocation', und das Ergebnis, um 'PublishScriptFilePath' ein. Diese Aktivität konvertiert den Pfad zum Veröffentlichen Skript TFS Server Orten (falls zutreffend) zu einem standard lokalen Festplattenpfad ein.

    5.  Wenn Sie TFS 2012 oder einer früheren Version verwenden, fügen Sie eine andere ConvertWorkspaceItem Aktivität am Ende der neuen Sequenz. Richtung = ServerToLocal, DisplayName = 'Konvertieren Abonnement Filename', Eingabe = SubscriptionDataFileLocation, Ergebnis = SubscriptionDataFilePath, Arbeitsbereich = "Arbeitsbereich". Wenn Sie TFS 2013 oder höher verwenden, fügen Sie eine andere GetLocalPath. IncomingPath = 'SubscriptionDataFileLocation', und das Ergebnis = 'SubscriptionDataFilePath'.

    6.  Hinzufügen einer Aktivität InvokeProcess am Ende der neuen Sequenz.
        Diese Aktivität Anrufe PowerShell.exe mit den Argumenten übergebener der Definition erstellen.

        1.  Argumente = String.Format ("-Datei""{0}" "- ServiceName {1} - StorageAccountName {2} - PackageLocation""{3}" "- CloudConfigLocation""{4}" "- SubscriptionDataFile""{5}" "- SelectedSubscription {6}-Übermittlungsstatus-Umgebung""{7}" "", PublishScriptFilePath ServiceName StorageAccountName PackageLocation CloudConfigLocation SubscriptionDataFilePath SubscriptionName, Umgebung)

        2.  DisplayName = Ausführung Skript veröffentlichen

        3.  FileName = "PowerShell" (einschließlich die Anführungszeichen)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  Klicken Sie im Abschnitt **Standardausgabe behandeln** von der InvokeProcess Textfeld 'Daten' den Wert Textfeld festlegen. Dies ist eine Variable zum Speichern der Daten für die standardmäßige Ausgabe.

    8.  Fügen Sie einer Aktivität WriteBuildMessage direkt unter dem Abschnitt **Standardausgabe behandeln** . Legen Sie die Wichtigkeit = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' und die Nachricht = 'Daten'. Dadurch wird sichergestellt, die Ausgabe erstellen die standardmäßige Ausgabe der das Skript geschrieben erhalten sollen.

    9.  Klicken Sie im Abschnitt **Fehler ausgegeben verarbeitet wurden** von der InvokeProcess Textfeld 'Daten' den Wert Textfeld festlegen. Dies ist eine Variable zum Speichern der Daten Standardfehler.

    10. Fügen Sie einer Aktivität WriteBuildError direkt unter dem Abschnitt **Fehler ausgegeben verarbeitet wurden** . Legen Sie die Nachricht = 'Daten'. Dadurch wird sichergestellt, dass die Ausgabe der Build Fehler der Standardfehler des Skripts geschrieben erhalten sollen.

    11. Beheben Sie alle Fehler, die blauen Ausrufezeichen erkennbar. Zeigen Sie auf das Ausrufezeichen, um einen Hinweis zu dem Fehler zu erhalten. Speichern Sie den Workflow, um Fehler zu löschen.

    Die Workflowaktivitäten veröffentlichen das endgültige Ergebnis wird im Designer wie folgt aus:

    ![Workflowaktivitäten][5]

    Die Workflowaktivitäten veröffentlichen das endgültige Ergebnis wird in XAML wie folgt aussehen:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Speichern Sie der Workflow des Vorlage erstellen und Einchecken dieser Datei.

8.  Bearbeiten die Definition erstellen (Schließen Sie es, wenn sie bereits geöffnet ist), und wählen Sie die Schaltfläche " **neu** " aus, wenn die neue Vorlage in der Liste der Vorlagen Prozess noch nicht angezeigt wird.

9.  Legen Sie den Parameter Immobilienwerte, die im Abschnitt Sonstiges wie folgt:

    1.  CloudConfigLocation = "c:\\löscht\\app.publish\\ServiceConfiguration.Cloud.cscfg' *dieser Wert wird von abgeleitet: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation = ' c:\\löscht\\app.publish\\ContactManager.Azure.cspkg' *dieser Wert wird von abgeleitet: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = "c:\\Skripts\\WindowsAzure\\PublishCloudService.ps1'

    4.  ServiceName = 'Mycloudservicename' *Verwenden Sie den entsprechenden Cloud-Dienstnamen hier*

    5.  Umgebung = 'Staging'

    6.  StorageAccountName = 'Mystorageaccountname' *verwenden den entsprechenden Speicher-Kontonamen*

    7.  SubscriptionDataFileLocation = "c:\\Skripts\\WindowsAzure\\Subscription.xml'

    8.  SubscriptionName = 'Standard'

    ![Parameterwerte-Eigenschaft][6]

10. Speichern der Änderungen auf die Definition erstellen.

11. Warteschlange einen Build ausführen sowohl das Paket erstellen und veröffentlichen. Wenn Sie einen Trigger fortlaufende Integration festgelegt haben, wird dieses Verhalten auf jedem Einchecken ausgeführt werden.

### <a name="publishcloudserviceps1-script-template"></a>Vorlage für ein PublishCloudService.ps1 Skript

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Nächste Schritte

Bei Verwendung von kontinuierlichen Bereitstellung remote Debuggen aktivieren, finden Sie unter [Aktivieren remote Debuggen bei Verwendung von kontinuierlichen Bereitstellung Azure veröffentlichen](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

  [Fortlaufender Übermittlung an Azure mithilfe von Visual Studio Team Services]: cloud-services-continuous-delivery-use-vso.md  
  [Team Foundation Build-Diensts]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [Skalieren Sie auf Ihrem System erstellen]: https://msdn.microsoft.com/library/dd793166.aspx
  [Bereitstellen und Konfigurieren von einem Server erstellen]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure PowerShell-cmdlets]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
