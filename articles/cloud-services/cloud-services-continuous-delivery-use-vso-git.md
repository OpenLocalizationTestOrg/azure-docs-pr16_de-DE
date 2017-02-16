<properties
    pageTitle="Fortlaufender Übermittlung mit Git und Visual Studio Team Services in Azure | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren von der Visual Studio Team Services Teamprojekten Git automatisch erstellen und Bereitstellen für das Web App-Feature in Azure-App-Verwaltungsdienst oder Cloud Services verwenden."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Fortlaufender Übermittlung an Azure mithilfe von Visual Studio Team Services und Git

Visual Studio Team Services Teamprojekte können Sie ein Repository Git für Ihre Quellcode gehostet und automatisch erstellen und bei Azure Web apps bereitstellen oder cloud Services, wenn Sie einen Commit zum Repository Pushbenachrichtigungen.

Sie benötigen Visual Studio 2013 und dem Azure SDK installiert. Wenn Sie noch keine Visual Studio 2013 haben, indem Sie auf den Link **kostenlos beginnen** bei [www.visualstudio.com](http://www.visualstudio.com)herunterladen. Installieren Sie das Azure SDK von [hier](http://go.microsoft.com/fwlink/?LinkId=239540)aus.


> [AZURE.NOTE]Benötigen Sie ein Konto Visual Studio Team Services zum Bearbeiten dieses Lernprogramms: Sie können [ein Konto Visual Studio Team Services kostenlos geöffnet](http://go.microsoft.com/fwlink/p/?LinkId=512979)ist.

Gehen Sie folgendermaßen vor, um einrichten auf einen Clouddienst automatisch erstellen und Bereitstellen für Azure mithilfe von Visual Studio Team Services.

## <a name="1-create-a-git-repository"></a>1: Erstellen eines Repositorys Git

1. Wenn Sie bereits über ein Visual Studio Team Services-Konto besitzen, können Sie eine abrufen [können](http://go.microsoft.com/fwlink/?LinkId=397665). Wenn Sie Ihr Team Project erstellen, wählen Sie Git als auch das Steuerelement Quelle. Folgen Sie den Anweisungen in Verbindung mit Ihrem Team Project Visual Studio.

2. Wählen Sie in **Team Explorer**den Link **Klonen dieses Repository** ein.

    ![][3]

3. Geben Sie den Speicherort der lokalen Kopie, und wählen Sie dann auf die Schaltfläche **Klonen** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: Erstellen eines Projekts und es in das Repository zu übernehmen

1. Wählen Sie den Link " **neu** " zum Erstellen eines neuen Projekts im lokalen Repository in **Team Explorer**im Abschnitt **Lösungen** aus.

    ![][4]

2. Sie können eine Web app oder einen Cloud-Service (Anwendung Azure) bereitstellen, anhand der Schritte in dieser Anleitung erfahren. Erstellen eines neuen Projekts von Azure-Cloud-Dienst oder ein neues ASP.NET-MVC-Projekt. Stellen Sie sicher, dass das Projekt ausgerichtet ist .NET Framework 4 oder höher. Wenn Sie ein Projekt Cloud-Dienst erstellen, fügen Sie eine ASP.NET-MVC Webrolle und eine Worker-Rolle aus.
Wenn Sie eine Web app erstellen möchten, wählen Sie die Projektvorlage **ASP.NET Web-Anwendung** , und wählen Sie dann auf **MVC**. Weitere Informationen finden Sie unter [Erstellen einer ASP.NET Web-app in Azure-App-Dienst](../app-service-web/web-sites-dotnet-get-started.md) .

3. Öffnen Sie das Kontextmenü für die Lösung, und wählen Sie die **abzuschließen**.

    ![][7]

4. Wenn dies das erste Mal Sie Git in Visual Studio Team Services verwendet haben ist, müssen Sie einige Hinweise zur Identifikation Git. Geben Sie im Bereich **Änderungen ausstehend** **Team**Explorer Ihren Benutzernamen und e-Mail-Adresse ein. Geben Sie einen Kommentar für die Commit ausführen, und wählen Sie dann auf die Schaltfläche " **bestätigen** ".

    ![][8]

5. Beachten Sie die Optionen zum ein- oder Ausschließen von bestimmter Änderungen beim Einchecken. Wenn die gewünschten Änderungen ausgeschlossen werden, wählen Sie **Alle enthalten**.

6. Jetzt haben Sie die Änderungen in Ihrer lokalen Kopie des Repositorys zugesichert. Synchronisieren Sie diese Änderungen mit dem Server weiter, indem Sie auf den Link **Synchronisieren** .

## <a name="3-connect-the-project-to-azure"></a>3: verbinden das Projekt und Azure

1. Jetzt, da Sie ein Repository Git in Visual Studio Team Services mit entsprechendem Quellcode darin haben, sind Sie bereit sind, Ihre Git Repository Azure Verbindung.  Wählen Sie im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)aus der Cloud-Dienst oder Web app oder einen neuen erstellen, indem Sie auf das +-Symbol am unten links klicken und dann die **Cloud-Dienst** oder **Web App** und klicken Sie dann auf **Symbolleiste erstellen**auswählen.

    ![][9]

3. Wählen Sie den Link zum **Einrichten von mit Visual Studio Team Services veröffentlichen** für Clouddienste aus. Wählen Sie für Web apps den Link zum **Einrichten der Bereitstellung von Datenquellen-Steuerelement** aus.

    ![][10]

2. Geben Sie den Namen Ihres Visual Studio Team Services-Kontos in das Textfeld im Assistenten und wählen Sie den Link **Zu autorisieren jetzt** aus. Sie möglicherweise aufgefordert, melden Sie sich an.

    ![][11]

3. Wählen Sie im Popupmenü Dialogfeld **Verbindung anfordern** , **akzeptieren** Azure so konfigurieren Sie Ihr Team Project in Visual Studio Team Services autorisieren ein.

    ![][12]

4. Nachdem Autorisierung hergestellt wurde, wird eine Dropdownliste, die Ihre Visual Studio Team Services Teamprojekte enthält.  Wählen Sie den Namen des Projekts Teamwebsite, die Sie in den vorherigen Schritten erstellt haben, und wählen Sie die Schaltfläche mit Häkchen des Assistenten.

    ![][13]

    Das nächste Mal, das Sie an Ihrem Repository, einen Commit Pushbenachrichtigungen wird Visual Studio Team Services erstellen und Bereitstellen von Ihrem Projekt in Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: Auslösen eines neu erstellen und erneut bereitstellen des Projekts

1. Öffnen Sie in Visual Studio eine Datei aus, und ändern Sie ihn. Ändern Sie beispielsweise die Datei `_Layout.cshtml` unter Ansichten\\freigegebene Ordner in einem MVC Webrolle.

    ![][17]

2. Bearbeiten von Text in die Fußzeile für die Website, und speichern Sie die Datei.

    ![][18]

3. Öffnen Sie im **Explorer Lösung**das Kontextmenü für den Lösungsknoten, Projektknoten oder die Datei, die Sie geändert haben, und wählen Sie dann auf **Übernehmen**.

4. Geben Sie in einem Kommentar ein, und wählen Sie die **abzuschließen**.

    ![][20]

5. Wählen Sie den Link **Synchronisieren** aus.

    ![][38]

6. Wählen Sie den Link **Pushbenachrichtigungen** Ihrer Commit zum Repository in Visual Studio Team Services Pushbenachrichtigungen aus. (Sie können Ihre Commit in das Repository kopiert auch die Schaltfläche **Synchronisieren** . Der Unterschied ist, dass **Synchronisieren** auch die neuesten Änderungen aus dem Repository abruft.)

    ![][39]

7. Wählen Sie die **Start** -Schaltfläche, um zur Startseite **Team Explorer** zurückzukehren.

    ![][21]

8. Wählen Sie **erstellt** , die Builds in den Fortschritt anzuzeigen.

    ![][22]

    **Team Explorer** zeigt, dass ein Build für Ihre Einchecken ausgelöst wurde.

    ![][23]

9. Um eine detaillierte Protokollierung im Verlauf der Build anzeigen möchten, doppelklicken Sie auf den Namen der Build wird ausgeführt.

10. Während der Erstellung in Bearbeitung wird, sehen Sie sich die Definition von erstellen, die erstellt wurde, wenn Sie den Assistenten zum Verknüpfen mit Azure verwendet.  Öffnen Sie das Kontextmenü für die Definition erstellen, und wählen Sie **Build Definition bearbeiten**.

    ![][25]

11. Auf der Registerkarte **Trigger** sehen Sie sich, dass die Definition erstellen auf jedem Einchecken erstellen standardmäßig festgelegt ist. (Für einen Clouddienst, Visual Studio Team Services erstellt und gibt den master Zweig an das staging-Umgebung automatisch. Sie haben noch einen manuellen Schritt zu der Website, live bereitstellen möchten. Für eine Web-app, die staging Umgebung aufweist, wird die Folienmaster Verzweigung direkt auf der Website live bereitgestellt.

    ![][26]

1. Auf der Registerkarte **Prozess** können Sie sehen, dass die Umgebung für die Bereitstellung auf den Namen der Cloud-Dienst oder Web app festgelegt ist.

    ![][27]

1. Geben Sie Werte für die Eigenschaften aus, wenn Sie andere Werte als die Standardeinstellungen soll. Die Eigenschaften für die Veröffentlichung Azure werden im Abschnitt **Bereitstellung** , und Sie müssen möglicherweise auch MSBuild Parameter festlegen. Beispielsweise in der Cloud Service Project, um anzugeben, eine Dienstkonfiguration als "Cloud", legen Sie die MSbuild-Parameter auf `/p:TargetProfile=[YourProfile]` *[YourProfile]* entspricht, wo eine Konfiguration Dienstdatei mit einem Namen wie ServiceConfiguration. *YourProfile*.cscfg.

    Die folgende Tabelle zeigt die verfügbaren Eigenschaften im Abschnitt **Bereitstellung** :

  	|Eigenschaft|Standardwert|
  	|---|---|
  	|Nicht vertrauenswürdige Zertifikate zulassen|Ist der Wert false ist, müssen SSL-Zertifikate von einer Stammzertifizierungsstelle angemeldet sein.|
  	|Upgrade zulassen|Ermöglicht die Bereitstellung aktualisieren eine vorhandene Bereitstellung statt einen neuen zu erstellen. Behält die IP-Adresse an.|
  	|Kann nicht löschen.|Wenn der Wert true, überschreiben eine vorhandene, nicht verknüpfte Bereitstellung nicht (Upgrade ist zulässig).|
  	|Pfad zur Bereitstellung Einstellungen|Der Pfad zur Datei .pubxml für eine Web-app, relativ zu den Stammordner des der Repo. Für Clouddienste ignoriert.|
  	|SharePoint-Bereitstellung-Umgebung|Der Dienstnamen identisch.|
  	|Azure-Bereitstellung-Umgebung|Der Web-app oder Cloud-Dienstname.|

1. Bis zu diesem Zeitpunkt sollte der Build erfolgreich abgeschlossen werden.

    ![][28]

1. Wenn Sie den Buildnamen doppelklicken, zeigt Visual Studio eine **Zusammenfassung erstellen**, einschließlich von alle Testergebnisse aus zugeordneten Einheit Testprojekte.

    ![][29]

1. Im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)können Sie die zugeordnete Bereitstellung auf der Registerkarte **Bereitstellungen** anzeigen, wenn die staging-Umgebung ausgewählt ist.

    ![][30]

1.  Navigieren Sie zu der Website-URL. Wählen Sie für eine Web-app die Schaltfläche **Durchsuchen** nur im Portal aus. Wählen Sie die URL für einen Clouddienst im Abschnitt **Schnelle Daten eine Übersicht** der **Dashboard** -Seite, die die Umgebung Staging anzeigt.

    Bereitstellungen von fortlaufende Integration für Clouddienste werden standardmäßig in der Staging-Umgebung veröffentlicht. Sie können dies mithilfe der **Alternativen Cloud-Service-Umgebung** Eigenschaft zu **Herstellung**ändern. Hier ist die Cloud-Dienst Dashboardseite Stelle die Website-URL ist.

    ![][31]

    Eine neue Browserregisterkarte wird geöffnet, um Ihre laufenden Website anzuzeigen.

    ![][32]

1.  Wenn Sie andere Änderungen an Ihrem Projekt möchten vornehmen Sie Trigger Weitere erstellt und können die mehrere Bereitstellungen gesammelten Daten werden. Die neuesten eine wird als aktiv markiert.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: erneut bereitstellen einer früheren erstellen

Dieser Schritt ist optional. Klicken Sie im Portal Azure klassischen wählen Sie aus einer früheren Bereitstellung, und wählen Sie auf Ihrer Website einer früheren Eincheckvorgang zurückspulen **erneut bereitstellen** . Beachten Sie, dass dies die Erstellung ein neues in TFS auslösen und eines neuen Eintrags in Ihrem Verlauf der Bereitstellung erstellen.

![][34]

## <a name="6-change-the-production-deployment"></a>6: ändern die Herstellung-Bereitstellung

Wenn Sie bereit sind, können Sie die Staging-Umgebung, die in dieser Umgebung heraufstufen, in der klassischen Azure-Portal auswählen **austauschen** . Die neu bereitgestellte Staging-Umgebung ist für die Kundendaten in Herstellung und dieser vorherige Umgebung, sofern vorhanden, wird eine Staging-Umgebung. Die aktive Bereitstellung kann für die Herstellung und Staging-Umgebungen unterschiedlich sein, aber der Verlauf Bereitstellung der letzten Builds ist gleich, unabhängig von der Umgebung.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: Bereitstellen von einer Verzweigung arbeiten.

Wenn Sie Git verwenden, nehmen Sie Änderungen in einer Verzweigung arbeiten Sie normalerweise an und in der Gestaltungsvorlage Verzweigung integriert werden soll, wenn die Entwicklung einen fertigen Status erreicht. Möchten Sie während der Entwicklungsphase eines Projekts erstellen und die Arbeit mit Azure bereitstellen.

1. Wählen Sie im **Team Explorer**die Schaltfläche **Start** , und wählen Sie dann auf die Schaltfläche **Verzweigungen** .

    ![][40]

2. Wählen Sie den Link **Neue Verzweigung** aus.

    ![][41]

3. Geben Sie den Namen der Zweigstelle, wie etwa "arbeiten,", und wählen Sie **Verzweigung erstellen**. Dadurch wird eine neue lokale Verzweigung erstellt.

    ![][42]

4. Veröffentlichen der Verzweigung. Wählen Sie den Namen der Zweigstelle in **nicht veröffentlichten Verzweigungen**aus, und wählen Sie dann **Veröffentlichen**.

    ![][44]

6. Standardmäßig lösen nur Änderungen an der master Verzweigung einen fortlaufenden Build. Klicken Sie zum Einrichten von fortlaufenden Build für eine Verzweigung arbeiten, wählen Sie die Seite **erstellt** in **Team Explorer**aus, und wählen Sie **Build Definition bearbeiten**.

7. Öffnen Sie die Registerkarte **Quelle Settings** . Wählen Sie unter **überwacht eine Verzweigung für fortlaufende Integration und erstellen** **Klicken Sie hier, um eine neue Zeile hinzuzufügen**.

    ![][47]

8. Geben Sie den Zweig, die, den Sie erstellt, z. B. Verweise/Spitzen/arbeiten haben.

    ![][48]

9. Nehmen Sie eine Änderung in den Code, öffnen Sie das Kontextmenü für die geänderten Datei, und wählen Sie dann auf **Übernehmen**.

    ![][43]

10. Wählen Sie den Link **Unsynced Commit** aus, und wählen Sie die Schaltfläche " **Synchronisieren** " oder den Link **Pushbenachrichtigungen** , um die Änderungen auf die Kopie der Zweigstelle Arbeit im Visual Studio Team Services zu kopieren.

    ![][45]

11. Navigieren Sie zu der Ansicht **erstellt** und finden Sie für die Arbeit mit den erstellen, der nur ausgelöst haben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Tipps zur Verwendung von Git mit Visual Studio Team Services finden Sie unter [Entwicklung und Freigeben von Code in Visual Studio mit Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) und Informationen zur Verwendung einer Git Repository, die nicht von Visual Studio Team Services veröffentlichen in Azure verwaltet wird, finden Sie unter [Fortlaufender Bereitstellung Azure-App-Dienst](../app-service-web/app-service-continuous-deployment.md). Weitere Informationen zu Visual Studio Team Services finden Sie unter [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
