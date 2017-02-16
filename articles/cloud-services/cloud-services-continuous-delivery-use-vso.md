<properties
    pageTitle="Fortlaufender Übermittlung mit Visual Studio Team Services in Azure | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren von der Visual Studio Team Services Teamprojekten automatisch erstellen und Bereitstellen des Web App-Features in Azure-App-Verwaltungsdienst oder Cloud Services."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Fortlaufender Übermittlung an Azure mit Visual Studio Team Services

Sie können Ihre Projekte Visual Studio Team Services Team, um automatisch erstellen und Bereitstellen für Azure Web apps oder cloud Services konfigurieren.  (Informationen zum Einrichten einer kontinuierlichen erstellen und Bereitstellen einer *lokalen* Team Foundation Server mit System finden Sie unter [Kontinuierlichen Bereitstellung von Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)

In diesem Lernprogramm wird davon ausgegangen, dass Sie Visual Studio 2013 und dem Azure SDK installiert haben. Wenn Sie noch keine Visual Studio 2013 haben, indem Sie auf den Link **kostenlos beginnen** bei [www.visualstudio.com](http://www.visualstudio.com)herunterladen. Installieren Sie das Azure SDK von [hier](http://go.microsoft.com/fwlink/?LinkId=239540)aus.

> [AZURE.NOTE]Benötigen Sie ein Konto Visual Studio Team Services zum Bearbeiten dieses Lernprogramms: Sie können [ein Konto Visual Studio Team Services kostenlos geöffnet](http://go.microsoft.com/fwlink/p/?LinkId=512979)ist.

Gehen Sie folgendermaßen vor, um einrichten auf einen Clouddienst automatisch erstellen und Bereitstellen für Azure mithilfe von Visual Studio Team Services.

## <a name="1-create-a-team-project"></a>1: Erstellen eines Teamprojekts

Folgen Sie den Anweisungen [hier](http://go.microsoft.com/fwlink/?LinkId=512980) erstellen Ihr Team Project und mit Visual Studio verknüpfen. Diese exemplarische Vorgehensweise wird vorausgesetzt, dass Sie als Ihre Quelle Steuerelement Lösung Team Foundation Version Steuerelement (TFVC) verwenden. Wenn Sie Git für die Versionskontrolle verwenden möchten, finden Sie unter [der Version Git diese Anleitung](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: Einchecken eines Projekts mit Datenquellen-Steuerelements

1. Öffnen Sie in Visual Studio die Lösung, die Sie bereitstellen möchten, oder Erstellen eines neuen Kontos.
Sie können eine Web app oder einen Cloud-Service (Anwendung Azure) bereitstellen, anhand der Schritte in dieser Anleitung erfahren.
Wenn Sie eine neue Lösung erstellen möchten, erstellen Sie ein neues Projekt mit Azure-Cloud-Dienst oder ein neues ASP.NET-MVC-Projekt. Stellen Sie sicher, dass das Projekt .NET Framework 4 oder 4.5 ausgerichtet ist, und wenn Sie ein Projekt Cloud-Dienst erstellen, fügen Sie eine ASP.NET-MVC Webrolle und eine Worker-Rolle hinzu, und wählen Sie Internet-Anwendung, die Rolle des Web. Wenn Sie dazu aufgefordert werden, wählen Sie **Internet-Anwendung**aus.
Wenn Sie eine Web app erstellen möchten, wählen Sie die Projektvorlage ASP.NET Web-Anwendung, und wählen Sie dann auf MVC. Finden Sie unter [Erstellen einer ASP.NET Web-app in Azure-App-Dienst](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services unterstützt nur CI-Bereitstellungen mit Visual Studio Web Applications zu diesem Zeitpunkt aus. Website-Projekten befinden außerhalb des Gültigkeitsbereichs.

1. Öffnen Sie das Kontextmenü für die Lösung, und wählen Sie **Lösung an Datenquellen-Steuerelement hinzufügen**.

    ![][5]

1. Annehmen oder Ändern der Standardeinstellungen, und wählen Sie die Schaltfläche **OK** aus. Nachdem der Vorgang abgeschlossen ist, werden die Quelle Steuerelement Symbole im **Lösung Explorer**angezeigt.

    ![][6]

1. Öffnen Sie das Kontextmenü für die Lösung, und wählen Sie **Einchecken**aus.

    ![][7]

1. Im Bereich **Änderungen ausstehend** **Team**Explorer Geben Sie einen Kommentar für das Einchecken, und wählen Sie die Schaltfläche **Einchecken** aus.

    ![][8]

    Beachten Sie die Optionen zum ein- oder Ausschließen von bestimmter Änderungen beim Einchecken. Wenn die gewünschte Änderungen ausgeschlossen werden, wählen Sie den Link **Alle einschließen** aus.

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: verbinden das Projekt und Azure

1. Jetzt, da Sie eine im Vergleich mit einer Team Services Team Project mit einigen Quellcode haben, sind Sie bereit sind, Ihr Teamprojekt mit Azure verbinden.  Wählen Sie im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)aus der Cloud-Dienst oder Web app oder einen neuen erstellen, indem Sie auf die **+** -Symbol in der unteren linken und **Cloud-Dienst** oder **Web App** und klicken Sie dann auf **Symbolleiste erstellen**auswählen. Wählen Sie den Link **einrichten mit Visual Studio Team Services veröffentlichen** .

    ![][10]

1. Geben Sie den Namen Ihres Visual Studio Team Services-Kontos in das Textfeld, und klicken Sie auf den Link **Jetzt autorisieren** , im Assistenten. Sie möglicherweise aufgefordert, melden Sie sich an.

    ![][11]

1. Wählen Sie im Popupmenü Dialogfeld **Verbindung anfordern** **akzeptieren** , um Azure zum Konfigurieren des Teamprojekts im Vergleich mit einer Teamwebsite-Dienste zu autorisieren ein.

    ![][12]

1. Wenn Autorisierung erfolgreich ist, wird eine Dropdown-Liste mit einer Liste der Projekte Team Visual Studio Team Services. Wählen Sie den Namen des Teamprojekt, das Sie in den vorherigen Schritten erstellt haben, und wählen Sie dann die Schaltfläche mit Häkchen des Assistenten.

    ![][13]

1. Nachdem Sie Ihr Projekt verknüpft ist, werden einige Anweisungen für das Projekt Team Visual Studio Team Services Änderungen einchecken angezeigt.  Klicken Sie auf der nächsten Einchecken wird Visual Studio Team Services erstellen und Bereitstellen von Ihrem Projekt in Azure.  Versuchen Sie dies nun durch Klicken auf den Link **Einchecken von Visual Studio** , und klicken Sie dann den Link **Visual Studio starten** (oder die entsprechende **Visual Studio** -Schaltfläche am unteren Rand des Bildschirms Portal).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: Auslösen eines neu erstellen und erneut bereitstellen des Projekts

1. Wählen Sie den Link **Source Control Explorer** in Visual Studio **Team Explorer**aus.

    ![][15]

1. Navigieren Sie zu Ihrer Lösung-Datei, und öffnen Sie sie.

    ![][16]

1. Im **Explorer Lösung**eine Datei zu öffnen Sie, und ändern Sie ihn. Ändern Sie beispielsweise die Datei `_Layout.cshtml` unter Ansichten\\freigegebene Ordner in einem MVC Webrolle.

    ![][17]

1. Bearbeiten Sie das Logo für die Website, und wählen Sie **STRG + S** , um die Datei zu speichern.

    ![][18]

1. Wählen Sie den Link **Ausstehende Änderungen** im **Team Explorer**aus.

    ![][19]

1. Geben Sie einen Kommentar ein, und wählen Sie dann auf die Schaltfläche **Einchecken** .

    ![][20]

1. Wählen Sie die **Start** -Schaltfläche, um zur Startseite **Team Explorer** zurückzukehren.

    ![][21]

1. Wählen Sie den Link **erstellt** , die Builds in den Fortschritt anzuzeigen.

    ![][22]

    **Team Explorer** zeigt, dass ein Build für Ihre Einchecken ausgelöst wurde.

    ![][23]

1. Doppelklicken Sie auf den Namen der Build wird ausgeführt, um eine detaillierte Protokollierung im Verlauf der Generator anzuzeigen.

    ![][24]

1. Während der Erstellung in Bearbeitung wird, sehen Sie sich die Definition von erstellen, die beim Verknüpfen TFS mit Azure mithilfe des Assistenten erstellt wurde.  Öffnen Sie das Kontextmenü für die Definition erstellen, und wählen Sie **Build Definition bearbeiten**.

    ![][25]

    Auf der Registerkarte **Trigger** sehen Sie sich, dass die Definition erstellen, die auf jedem Einchecken erstellen standardmäßig festgelegt ist.

    ![][26]

    Auf der Registerkarte **Prozess** können Sie sehen, dass die Umgebung für die Bereitstellung auf den Namen der Cloud-Dienst oder Web app festgelegt ist. Wenn Sie mit Web apps arbeiten, werden sich von den hier gezeigten die Eigenschaften, die angezeigt werden.

    ![][27]

1. Geben Sie Werte für die Eigenschaften aus, wenn Sie andere Werte als die Standardeinstellungen soll. Die Eigenschaften für die Veröffentlichung Azure sind im Abschnitt **Bereitstellung** .

    Die folgende Tabelle zeigt die verfügbaren Eigenschaften im Abschnitt **Bereitstellung** :

  	|Eigenschaft|Standardwert|
  	|---|---|
  	|Nicht vertrauenswürdige Zertifikate zulassen|Ist der Wert false ist, müssen SSL-Zertifikate von einer Stammzertifizierungsstelle angemeldet sein.|
  	|Upgrade zulassen|Ermöglicht die Bereitstellung aktualisieren eine vorhandene Bereitstellung statt einen neuen zu erstellen. Behält die IP-Adresse an.|
  	|Kann nicht löschen.|Wenn der Wert true, überschreiben eine vorhandene, nicht verknüpfte Bereitstellung nicht (Upgrade ist zulässig).|
  	|Pfad zur Bereitstellung Einstellungen|Der Pfad zur Datei .pubxml für eine Web-app, relativ zu den Stammordner des der Repo. Für Clouddienste ignoriert.|
  	|SharePoint-Bereitstellung-Umgebung|Der Dienstnamen identisch.|
  	|Azure-Bereitstellung-Umgebung|Der Web-app oder Cloud-Dienstname.|

1. Wenn Sie mehrere Dienstkonfigurationen (.cscfg-Dateien) verwenden, können Sie die gewünschte Dienstkonfiguration der Einstellung **erstellen, erweitert MSBuild-Argumente** angeben. Angenommen, um ServiceConfiguration.Test.cscfg verwenden zu können, legen Sie MSBuild-Argumente Zeilenoption `/p:TargetProfile=Test`.

    ![][38]

    Bis zu diesem Zeitpunkt sollte der Build erfolgreich abgeschlossen werden.

    ![][28]

1. Wenn Sie den Buildnamen doppelklicken, zeigt Visual Studio eine **Zusammenfassung erstellen**, einschließlich von alle Testergebnisse aus zugeordneten Einheit Testprojekte.

    ![][29]

1. Im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)können Sie die zugeordnete Bereitstellung auf der Registerkarte **Bereitstellungen** anzeigen, wenn die staging-Umgebung ausgewählt ist.

    ![][30]

1.  Navigieren Sie zu der Website-URL. Klicken Sie für eine Web-app auf die Schaltfläche **Durchsuchen** auf der Befehlsleiste. Wählen Sie die URL für einen Clouddienst im Abschnitt **Schnelle Daten eine Übersicht** der **Dashboard** -Seite, die die Staging-Umgebung für einen Clouddienst anzeigt. Bereitstellungen von fortlaufende Integration für Clouddienste werden standardmäßig in der Staging-Umgebung veröffentlicht. Sie können dies mithilfe der **Alternativen Cloud-Service-Umgebung** Eigenschaft zu **Herstellung**ändern. Dieses Screenshot zeigt an, wo die URL der Website auf die Cloud-Dienst Dashboardseite ist.

    ![][31]

    Eine neue Browserregisterkarte wird geöffnet, um Ihre laufenden Website anzuzeigen.

    ![][32]

    Für Cloud-Diensten Wenn Sie andere Änderungen an Ihrem Projekt möchten vornehmen Sie Trigger Weitere erstellt, und Sie mehrere Bereitstellungen werden beim Wiederholen. Die neuesten eine als aktiv gekennzeichnet ist.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: erneut bereitstellen einer früheren erstellen

Dieser Schritt bezieht sich auf Clouddienste und ist optional. Im Portal Azure klassischen wählen Sie aus einer früheren Bereitstellung, und wählen Sie dann auf die Schaltfläche **erneut bereitstellen** zu Ihrer Website zu einer früheren Eincheckvorgang zurückspulen.  Beachten Sie, dass dies die Erstellung ein neues in TFS auslösen und eines neuen Eintrags in Ihrem Verlauf der Bereitstellung erstellen.

![][34]

## <a name="6-change-the-production-deployment"></a>6: ändern die Herstellung-Bereitstellung

Dieser Schritt gilt nur für Cloud Services nicht Web apps. Wenn Sie bereit sind, können Sie die Staging-Umgebung, die in dieser Umgebung heraufstufen, indem Sie auf die Schaltfläche **austauschen** im klassischen Azure-Portal. Die neu bereitgestellte Staging-Umgebung ist für die Kundendaten in Herstellung und dieser vorherige Umgebung, sofern vorhanden, wird eine Staging-Umgebung. Die aktive Bereitstellung kann für die Herstellung und Staging-Umgebungen unterschiedlich sein, aber der Verlauf Bereitstellung der letzten Builds ist gleich, unabhängig von der Umgebung.

![][35]

## <a name="7-run-unit-tests"></a>7: Einheit Tests ausführen

Dieser Schritt gilt nur für web apps, die nicht cloud Services. Um eine Qualität Eingang auf Ihre Bereitstellung setzen, Einheit Tests ausgeführt werden kann, und wenn sie ein Fehler auftreten, können Sie die Bereitstellung beenden.

1.  Fügen Sie in Visual Studio ein Projekt Einheit hinzu.

    ![][39]

1.  Fügen Sie Projektverweise des Projekts, die Sie testen möchten.

    ![][40]

1.  Fügen Sie einige Tests Einheit hinzu. Um anzufangen, versuchen Sie, der immer übergibt Test-platzhalterprodukt aus.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Bearbeiten der Definition erstellen, wählen Sie aus der Registerkarte **Prozess** , und erweitern Sie den Knoten **Testen** .

1.  Setzen Sie den **Fail-Generator auf Fehler beim Test** auf True. Dies bedeutet, dass die Bereitstellung durchgeführt wird, wenn die Tests bewegen.

    ![][41]

1.  Neue Build in die Warteschlange.

    ![][42]

    ![][43]

1. Während der Erstellung fortgesetzt wird, aktivieren Sie auf deren Status.

    ![][44]

    ![][45]

1. Wenn der Build fertig ist, überprüfen Sie die Testergebnisse aus.

    ![][46]

    ![][47]

1.  Versuchen Sie, erstellen einen Testanruf, der fehlschlägt. Fügen Sie einen neuen Test durch Kopieren der ersten Phase, und benennen sie kommentieren Sie die Zeile des Codes, die besagt, dass NotImplementedException eine erwartete Ausnahme ist.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Aktivieren Sie im Feld ändern in Warteschlange einen neuen erstellen.

    ![][48]

1. Anzeigen der Testergebnisse zum Anzeigen von Details zu dem Fehler an.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu in Visual Studio Team Services Testen von Einheiten finden Sie unter [Unit Tests Ihre eigene ausführen](http://go.microsoft.com/fwlink/p/?LinkId=510474). Wenn Sie Git verwenden, finden Sie unter [Freigeben von Code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) und [kontinuierliche Bereitstellung Azure-App-Dienst](../app-service-web/app-service-continuous-deployment.md).  Weitere Informationen zu Visual Studio Team Services finden Sie unter [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
