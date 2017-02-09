<properties
    pageTitle="Einrichten von staging-Umgebungen von Web apps in Azure-App-Verwaltungsdienst"
    description="Erfahren Sie, wie bereitgestellte für die Veröffentlichung von Web apps in Azure-App-Dienst verwenden."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Einrichten von staging-Umgebungen von Web apps in Azure-App-Verwaltungsdienst
<a name="Overview"></a>

Wenn Sie die Web app [App-Dienst](http://go.microsoft.com/fwlink/?LinkId=529714)bereitstellen, können Sie auf einem separaten Bereitstellung Slot anstelle der standardmäßigen Herstellung Slot bereitstellen, wenn im Plan **Standard** oder **Premium** App-Verwaltungsdienst-Modus ausgeführt. Bereitstellung Steckplätze sind tatsächlich live-Web-apps mit ihrer eigenen Hostnamen. Web app Inhalt und Konfigurationen Elemente können zwischen zwei Bereitstellung Steckplätzen, einschließlich den Herstellung Slot ausgetauscht werden. Bereitstellen der Anwendung auf ein Slot Bereitstellung weist die folgenden Vorteile:

- Sie können Web app Änderungen in einem staging Bereitstellung Slot überprüfen, bevor Sie es mit der Herstellung Slot austauschen.

- Bereitstellen einer Web app auf einem Slot zuerst und es in Betrieb austauschen: Damit ist sichergestellt, dass alle Instanzen von den Slot vor, die in Betrieb vertauscht Standardserverhardware sind. Dadurch werden Ausfallzeiten, wenn Sie die Web-app bereitstellen. Eine Umleitung des Datenverkehrs nahtlose ist und keine Anfragen aus austauschen Vorgängen, die gelöscht werden. Diesen Workflow gesamte kann automatisierte werden, durch Konfigurieren [Automatisch austauschen](#configure-auto-swap-for-your-web-app) , wenn vor dem Austauschen Überprüfung nicht erforderlich ist.

- Nach einem Austausch hat der Slot mit zuvor bereitgestellte Web app jetzt des vorherigen Herstellung Web app an. Wenn die Änderungen in der Herstellung Slot ausgetauscht werden nicht wie erwartet, können Sie die gleichen austauschen sofort, um Ihre "letzten bekannten guten Seite" im Handumdrehen ausführen zurück.

Jede App Dienst Plan Modus unterstützt unterschiedlich viele Bereitstellung Steckplätze. Um herauszufinden, die Anzahl der Steckplätze der Web-app-Modus unterstützt, finden Sie unter [App Preisen](/pricing/details/app-service/).

- Wenn Ihre Web app mehrere Steckplätze enthält, können Sie den Modus nicht ändern.

- Skalierung ist nicht verfügbar für nicht Herstellung Steckplätze.

- Verknüpfte Ressourcenmanagement wird nicht Herstellung Steckplätze nicht unterstützt. Im [Portal Azure](http://go.microsoft.com/fwlink/?LinkId=529715) nur können Sie diese mögliche Auswirkung auf ein Slot Herstellung vermeiden, indem Sie vorübergehend den Slot nicht Herstellung in einen anderen Plan Modus für App-Dienst verschieben. Beachten Sie, dass nicht Herstellung Slot erneut freigeben muss den gleichen Modus mit der Herstellung Slot, bevor Sie die beiden Steckplätze austauschen können.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Fügen Sie einen Bereitstellung Slot mit einem Web-app hinzu ##

Die Web-app muss im Modus **Standard** oder **Premium** in Reihenfolge, damit Sie mehrere Bereitstellung Steckplätze aktivieren ausgeführt werden.

1. Öffnen Sie im [Portal Azure](https://portal.azure.com/)-Web app Blade aus.
2. Klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Bereitstellung Steckplätze**. Klicken Sie dann in das Blade **Bereitstellung Steckplätze** auf **Slot hinzufügen**.

    ![Fügen Sie einen neuen Bereitstellung Slot hinzu][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Wenn das Web app im Modus **Standard** oder **Premium** noch nicht vorhanden ist, erhalten Sie eine Meldung mit den unterstützten Modi für das Aktivieren der Veröffentlichung bereitgestellte. An diesem Punkt müssen Sie die Option zum auswählen, **Aktualisieren** , und navigieren Sie zur Registerkarte **Skalierung** der Web app, bevor Sie fortfahren.

2. Klicken Sie in das Blade **Hinzufügen ein Slot** Geben Sie Slot einen Namen, und wählen Sie, ob Sie die Konfiguration des Web-app aus einer anderen vorhandenen Bereitstellung Slot klonen. Klicken Sie auf das Häkchen, um den Vorgang fortzusetzen.

    ![Konfigurationsquelle][ConfigurationSource1]

    Fügen Sie einen Slot, stehen Ihnen zwei Optionen nur beim ersten: Klonen Konfiguration aus dem Slot Standard Herstellung oder überhaupt nicht.

    Nachdem Sie mehrere Steckplätze erstellt haben, werden Sie Konfiguration von ein Slot als das im Herstellung Klonen sein:

    ![Von Konfigurationsquellen][MultipleConfigurationSources]

5. Klicken Sie in das Blade **Bereitstellung Steckplätze** auf der Bereitstellung Slot um ein Blade für den Slot, mit einer Reihe von Kennzahlen und Konfiguration wie eine beliebige andere Web app öffnen. **your-Web-App-Name-Deployment-Slot-Name** wird am oberen Rand Blade Sie daran erinnern, dass Sie den Bereitstellung Slot anzeigen angezeigt.

    ![Bereitstellung Slot Titel][StagingTitle]

5. Klicken Sie auf die app-URL in den Slot des Blade. Beachten Sie der Bereitstellung Slot verfügt über eine eigene Hostname und ist ebenfalls eine live-app. Um öffentlichen Zugriff auf die Bereitstellung Slot beschränken, finden Sie unter [App Dienst Web App – Webzugriff auf nicht Herstellung Bereitstellung Slots blockieren](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Es ist kein Inhalt nach der Erstellung der Bereitstellung Slot. Sie können aus einem anderen Zweig oder ein ganz anderes Repository für den Slot bereitstellen. Sie können auch den Slot der Konfiguration ändern. Verwenden Sie die veröffentlichen Profil oder Bereitstellung Anmeldeinformationen der Bereitstellung Slot für Updates von Onlineinhalten zugeordnet.  Beispielsweise können Sie [in diesem Slot mit Git veröffentlichen](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Konfiguration für Bereitstellung Steckplätze ##
Wenn Sie die Konfiguration von einem anderen Bereitstellung Slot klonen, kann die duplizierte Konfiguration bearbeitet werden. Darüber hinaus werden einige Konfigurationselemente den Inhalt über eine austauschen (nicht Slot bestimmte) folgen, während andere Konfigurationselemente im selben Feld nach einer austauschen (Slot bestimmte) verbleibt. Die folgenden Listen anzeigen die Konfiguration, die geändert wird, wenn Sie Steckplätze austauschen.

**Einstellungen, die ausgetauscht werden**:

- Allgemeine Einstellungen – beispielsweise Framework Version, 32/64-Bit-Web sockets
- App-Einstellungen (kann konfiguriert werden an ein Slot halten)
- Verbindungszeichenfolgen (kann konfiguriert werden an ein Slot halten)
- Ereignishandler Zuordnungen
- Überwachung und zu Diagnosezwecken Einstellungen
- WebJobs Inhalt

**Einstellungen, die nicht ausgetauscht werden**:

- Endpunkte für die Veröffentlichung
- Benutzerdefinierten Domänennamen
- SSL-Zertifikate und Bindungen
- Skalierungseinstellungen
- Planer WebJobs

Eine app Einstellung oder Verbindung Zeichenfolge ein Slot (nicht vertauscht) verwenden um zu konfigurieren, Zugriff auf das Blade **Anwendungseinstellungen** für eine bestimmte Slot, und wählen Sie dann im Feld **Slot Einstellung** für die Elemente der Konfiguration, die den Slot für übernommen werden sollen. Beachten Sie die durch das Kennzeichnen eines Konfigurationselements als Slot bestimmte den Effekt der herstellen dieses Elements als nicht austauschbare auf alle die Bereitstellung der Web-app zugeordnet wurde.

![Slot-Einstellungen][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Bereitstellung Steckplätze vertauschen ##

>[AZURE.IMPORTANT] Bevor Sie eine Web-app aus einer Bereitstellung Slot in Betrieb austauschen, stellen Sie sicher, dass alle nicht-Slot bestimmte Einstellungen wie gewünscht, damit diese in der Zielliste austauschen konfiguriert sind.

1. Um Bereitstellung Steckplätze austauschen, klicken Sie auf die Schaltfläche **austauschen** in der Befehlsleiste in der Web-app oder in der Befehlsleiste in ein Slot Bereitstellung. Stellen Sie sicher, dass der austauschen Quell-als auch austauschen ordnungsgemäß festgelegt sind. In der Regel wäre das Ziel Austauschen der Herstellung Slot.  

    ![Schaltfläche "austauschen"][SwapButtonBar]

3. Klicken Sie auf **OK** , um den Vorgang abzuschließen. Wenn der Vorgang abgeschlossen ist, müssen die Bereitstellung Steckplätze vertauscht wurden.

## <a name="configure-auto-swap-for-your-web-app"></a>Konfigurieren von automatischen austauschen für Ihre Web app ##

Automatische austauschen Optimierung DevOps Szenarien kontinuierlich Web app mit 0 (null) kalt Start- und Ausfallzeiten für Ende Kunden des Web app bereitstellen möchten. Wenn ein Slot Bereitstellung für automatische austauschen in Betrieb, konfiguriert ist jedes Mal, wenn Sie Ihre Code Update zu diesem Slot Pushbenachrichtigungen, wird App-Dienst automatisch austauschen des Web app in Betrieb, nachdem es bereits im Slot Standardserverhardware hat.

>[AZURE.IMPORTANT] Wenn Sie automatische austauschen für ein Slot aktivieren, stellen Sie sicher, dass die Konfiguration Slot genau die Konfiguration für den Slot Ziel (normalerweise der Herstellung Slot) vorgesehen ist.

Konfigurieren von automatischen austauschen für ein Slot ist einfach. Führen Sie die folgenden Schritte aus:

1. Wählen Sie in der **Bereitstellung Steckplätze** Blade ein Slot nicht Herstellung aus, klicken Sie auf **Alle Einstellungen** für die Slot des Blade.  

    ![][Autoswap1]

2. Klicken Sie auf **ApplicationSettings**. Wählen Sie **auf** für **Automatische austauschen**, markieren Sie das gewünschte Ziel-Feld in der **Automatischen austauschen Slot**, und klicken Sie auf **Speichern** , in der Befehlsleiste. Stellen Sie sicher, dass die Konfiguration für den Slot genau die Konfiguration, die für den Target Slot vorgesehen ist.

    Die Registerkarte **Benachrichtigungen** blinkt grün **Erfolg** , nachdem der Vorgang abgeschlossen ist.

    ![][Autoswap2]

    >[AZURE.NOTE] Klicken Sie zum Testen von automatischen austauschen für Ihre Web app können Sie zuerst einen Slot nicht Herstellung Ziel im **Automatischen austauschen Slot** , um mit der Funktion vertraut auswählen.  

3. Führen Sie einen Code Pushbenachrichtigungen zu diesem Slot Bereitstellung. Automatische austauschen tritt nach einer kurzen Zeit und die Aktualisierung wird bei der Ziel-Slot URL übernommen werden.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Verwenden von mehreren Schritten austauschen für Web app ##

Multi-Phase austauschen steht zur Vereinfachung der Überprüfung im Zusammenhang mit der Konfigurationselemente um ein Slot z. B. Verbindungszeichenfolgen für übernommen werden sollen. In diesen Fällen kann es hilfreich sein, solche Konfigurationselemente aus dem Ziel austauschen auf die Quelle austauschen anzuwenden, und überprüfen, bevor austauschen tatsächlich wirksam wird. Sobald die verfügbaren Aktionen der austauschen durchführen oder Wiederherstellen der ursprünglichen Konfiguration für die Quelle austauschen, die auch das Verhalten der austauschen Abbrechen enthält austauschen Ziel Konfigurationselemente mit der Quelldatei austauschen angewendet werden. Beispiele für die Azure-PowerShell-Cmdlets zum Austauschen von mehreren Schritten verfügbar sind in der Azure-PowerShell-Cmdlets für die Bereitstellung Steckplätze Abschnitt enthalten.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Zum Zurücksetzen einer app Herstellung nach austauschen ##

Wenn Fehler bei Herstellung nach einem Slot austauschen identifiziert werden, setzen Sie die Steckplätze zurück in ihren Zustand vor dem austauschen, indem Sie die gleichen beiden Steckplätze sofort austauschen.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Benutzerdefinierte Aufwärmphase vor dem Austauschen ##

Einige apps möglicherweise benutzerdefinierte Aufwärmphase Aktionen erforderlich. Das ApplicationInitialization Konfigurationselement in web.config ermöglicht das benutzerdefinierte Initialisierungsaktionen ausgeführt werden, bevor eine Anforderung eingeht festlegen. Der Vorgang austauschen wartet dieser benutzerdefinierten Vorgeschmack auf ausführen. Hier ist ein Beispiel für web.config Fragment aus.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>So löschen Sie einen Slot Bereitstellung##

Klicken Sie in das Blade für ein Slot Bereitstellung in der Befehlsleiste auf **Löschen** .  

![Löschen Sie einen Slot Bereitstellung][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure PowerShell-Cmdlets für die Bereitstellung Steckplätze

Azure PowerShell ist ein Modul, Cmdlets zum Verwalten von Azure über Windows PowerShell, einschließlich der Unterstützung für die Verwaltung von Web app-Bereitstellung Steckplätze in Azure-App-Verwaltungsdienst bereitstellt.

- Informationen zum Installieren und Konfigurieren von Azure PowerShell, und klicken Sie auf Authentifizierung Azure PowerShell mit Ihrem Abonnement Azure finden Sie unter [So installieren und Konfigurieren von Microsoft Azure PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Erstellen von Web-app

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Erstellen Sie einen Slot Bereitstellung für eine Web app

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Einleiten Sie Multi-Phase austauschen und anwenden Ziel Slot Konfiguration auf Quelle slot

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Wiederherstellen der ersten Phase des Multi-Phase austauschen und Konfigurieren von Datenquellen Slot wiederherstellen

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Austauschen Bereitstellung Steckplätze

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Löschen der Bereitstellung slot

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure Line Interface (CLI Azure)-Befehle für die Bereitstellung Steckplätze

Die CLI Azure bietet Plattform-Befehle für die Arbeit mit Azure, einschließlich der Unterstützung für die Verwaltung von Web App-Bereitstellung Steckplätze.

- Anweisungen zum Installieren und Konfigurieren der Azure-CLI, einschließlich Informationen zu Ihrem Abonnement Azure Azure CLI Verbindung finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md).

-  Um die Liste der verfügbaren Befehle für App-Dienst in der CLI Azure Azure anrufen `azure site -h`.

----------
### <a name="azure-site-list"></a>Azure Websiteliste
Rufen Sie Informationen zu den Web apps in das aktuelle Abonnement, **Azure Websiteliste**, wie im folgenden Beispiel gezeigt.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Erstellen von Azure-Website
Zum Erstellen eines Bereitstellung Slot rufen Sie **Azure-Website zu erstellen an** , und geben Sie den Namen einer vorhandenen Web app und den Namen der Slot zu erstellen, wie im folgenden Beispiel.

`azure site create webappslotstest --slot staging`

Um Datenquellen-Steuerelements für den neuen Slot aktivieren möchten, verwenden Sie die Option **--Git** , wie im folgenden Beispiel aus.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Austauschen von Azure-Website
Um die aktualisierten Bereitstellung die Herstellung app slot, verwenden Sie den Befehl **austauschen Azure-Website** zum Ausführen eines Vorgangs austauschen wie im folgenden Beispiel zu gestalten. Die Herstellung app wird nicht ab dem Zeitpunkt auftreten, noch wird einen kalten Start unterliegen.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Azure-Website löschen
Um ein Slot Bereitstellung löschen, der nicht mehr benötigt wird, verwenden Sie den Befehl **Azure Website löschen** , wie im folgenden Beispiel aus.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="next-steps"></a>Nächste Schritte ##
[Azure App Service-Web App – Webzugriff auf nicht Herstellung Bereitstellung Slots blockieren](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Kostenlose Testversion von Microsoft Azure](/pricing/free-trial/)

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
