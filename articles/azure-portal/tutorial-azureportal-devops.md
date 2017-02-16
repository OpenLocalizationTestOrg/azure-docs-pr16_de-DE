<properties
    pageTitle="Lernprogramm: DevOps mit dem Portal Azure | Microsoft Azure"
    description="Erfahren Sie die verschiedenen DevOps Workflows Azure-Portal an."
    services="azure-portal"
    documentationCenter=""
    authors="mlearned"
    manager="douge"
    editor="mlearned"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/05/2016"
    ms.author="mlearned"/>

# <a name="tutorial-devops-with-the-azure-portal"></a>Lernprogramm: DevOps mit dem Azure-Portal

Die Azure-Plattform enthält viele flexible DevOps Workflows. In diesem Lernprogramm erfahren Sie, wie Sie die Nutzung der Funktionen des Portals Azure Entwickeln, testen, bereitstellen, Problembehandlung bei, überwachen und Verwalten von Applications laufenden. In diesem Lernprogramm Schwerpunkt auf Folgendes:

1.  Erstellen einer Web app und Aktivieren der kontinuierlichen Bereitstellung

2.  Entwickeln und Testen einer app

3.  Für die Überwachung und Problembehandlung bei einer app

4.  Allgemeine Aufgaben bei der Verwaltung

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a>Erstellen einer Web app und Aktivieren der kontinuierlichen Bereitstellung

Erstellen Sie eine Web app mit [Azure-App-Dienst](https://azure.microsoft.com/services/app-service/), der Sie in der im weiteren Verlauf dieses Lernprogramms verwenden möchten. Sie werden zunächst fortlaufender Bereitstellung aus der Quelle Code Repository in unserer Einstieg Azure-Umgebung aktivieren.

1.  Melden Sie sich bei der Azure-Portal

2.  Wählen Sie die **App Services** &gt; **Symbol "hinzufügen"** , und geben Sie einen Namen ein, wählen Sie Ihr Abonnement und erstellen eine neuen Ressourcengruppe zu dienen als Container für den Dienst.

    Ressourcengruppen können Sie verschiedene Aspekte der Lösung wie Abrechnung, Bereitstellungen und alle als eine einzelne Gruppe über [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md)Überwachung verwalten.

    ![Image1][image1]


3.  Nach ein paar Augenblicke wird Ihre app-Verwaltungsdienst erstellt. Dauern Sie Überblick über die verschiedenen Menüoptionen für den Dienst im Portal ein paar Minuten.

    ![Bild2][image2]    

4.  Klicken Sie auf die URL. Beachten Sie die Vielzahl von Optionen für Tools und Repositorys verfügbar. Sie können auch die Sprachen und Ihrer Wahl, einschließlich Ruby, Java und .NET Framework verwenden.

    ![image3][image3]    

3.  Azure-Portal wird die kontinuierliche Bereitstellung einen einfachen Vorgang, bei dem nur wenigen einfachen Schritten. Wählen Sie im Portal Azure über das Symbol für die app-Verwaltungsdienst soeben erstellten Einstellungen aus.

    ![image4][image4]

    Aus dem Blade, das auf der rechten Seite angezeigt wird, führen Sie einen Bildlauf zum Abschnitt für die Veröffentlichung.

    ![image5][image5]

4.  Konfigurieren Sie anschließend einige Einstellungen, um kontinuierliche Bereitstellung für die app zu aktivieren. Klicken Sie auf Bereitstellung Quelle, und klicken Sie dann auf Quelle auswählen. Beachten Sie die Vielzahl von Optionen, die Sie für Repository Quellen haben.

    ![Image6][image6]

1.  Wählen Sie in diesem Beispiel GitHub aus. Optional Wählen Sie aus dem Repository Ihrer Wahl und die Anmeldeinformationen für die Autorisierung installiert werden.

    ![image7][image7]

2.  Nach Autorisierung an Ihrem Repository können Sie ein Projekt und Verzweigung, die Sie bereitstellen möchten. Es gibt mehrere unten aufgeführten fiktives Beispiel Beispiele aus.

    ![image8][image8]

3.  Nachdem Sie das Projekt und Verzweigung auswählen, klicken Sie auf ok. Sie sollten die Benachrichtigungen über eine Bereitstellung finden Sie unter beginnen.

    ![image9][image9]

4.  Navigieren Sie zurück zu Github, um die Webhook anzuzeigen, die erstellt wurde, um die Quelle Steuerelement Repo mit Azure zu integrieren. Im Portal Azure ermöglicht die Integration mit Github mit nur wenigen einfachen Schritten.

    ![image10][image10]

5.  Um kontinuierliche Bereitstellung zu veranschaulichen, fügen Sie schnell ein Teil des Inhalts in das Repository. Ein einfaches Beispiel: Hinzufügen einer Stichprobe Textdatei zu einer Github Repo. Sie sind kostenlos zur Verwendung von .NET, Ruby, Python oder eine andere Art von Anwendung mit App-Dienst. Eine Textdatei, ASP.NET-MVC, Java oder Ruby Anwendung, um die Repo Ihrer Wahl hinzufügen können.

    ![image11][image11]

6.  Nach dem Commit Änderungen an Ihrem Repository, Sie sehen eine neue Bereitstellung im Portal Benachrichtigungsbereich einleiten. Klicken Sie auf synchronisieren, wenn Sie nicht schnell Änderungen sehen nach der Übermittlung an Ihrem Repository.

    ![image12][image12]

7.  Wenn Sie versuchen, und Laden Sie die Seite für den app-Dienst, möglicherweise zu diesem Zeitpunkt ein 403-Fehler angezeigt. In diesem Beispiel ist es, da es keine normalerweise Dokument Einrichtung für die Seite beispielsweise eine Datei index.htm oder default.html ist. Sie können dies schnell mit den Tools auf das Portal Azure beheben.  Wählen Sie im Portal Azure Einstellungen aus &gt; Application Settings.

     ![image13][image13]

8.  Eine Blade wird für Anwendungseinstellungen geöffnet. Geben Sie den Namen der Seite "SamplePage.html", und klicken Sie auf Speichern. Durchsuchen die anderen Einstellungen ein paar Minuten dauern.

    ![image14][image14]

9.  Optional aktualisieren Sie Ihr Browser-URL, um sicherzustellen, dass die erwarteten Änderungen angezeigt. In diesem Fall befindet sich jetzt Auffüllen von der Seite einfachen Text. Jede weitere Änderung an Repository führt zu einer neuen automatische Bereitstellung.

    ![image15][image15]

    Aktivieren der kontinuierlichen Bereitstellung mit dem Portal Azure ist eine einfache. Sie können auch komplexere Version Pipelines erstellen und viele andere Techniken mit vorhandenen Datenquellen-Steuerelement verwenden und fortlaufende Integration Systeme in Azure, bereitgestellt werden, wie automatisierte Nutzung von erstellen, und lassen Sie wieder los Management-Systeme.

## <a name="develop-and-test-an-app"></a>Entwickeln und Testen einer app

Als Nächstes nehmen Sie bei einigen Änderungen an den Code Basis und implementieren Sie diese Änderungen schnell zu. Sie können auch einige Leistung bei Tests für das Web app einrichten.

1.  Im Portal Azure wählen Sie Services App aus dem Navigationsbereich aus, und suchen Sie Ihre App-Verwaltungsdienst.

    ![image16][image16]

2.  Klicken Sie auf Extras

    ![image17][image17]

3.  Beachten Sie die Entwicklung Kategorie klicken Sie unter Tools. Es gibt einige hilfreiche Tools hier, mit denen uns für die Arbeit mit apps, ohne das Azure-Portal verlassen zu müssen. Klicken Sie auf Konsole.

    ![image18][image18]

4.  Klicken Sie im Console können Sie live Befehle für Ihre app senden. Geben Sie den Befehl Verzeichnis ein, und geben Sie Treffer. Beachten Sie, dass Befehle mit Anforderung der erhöhten nicht funktionieren.

    ![image19][image19]

5.  Wechseln Sie wieder zur Entwicklung Kategorie, und wählen Sie Visual Studio Online. Hinweis: Visual Studio Online ist jetzt Visual Studio Team Services mit dem Namen.

    ![image20][image20]

6.  Schalten Sie die Bearbeitung im Browser-Oberfläche für Ihre App.

    ![image21][image21]

7.  Web-Erweiterung für Ihre app-Installationen. Erweiterungen Funktionsumfang schnell und einfach zu apps in Azure. Beachten Sie einige der anderen Erweiterungstypen in den folgenden Screenshot zur Verfügung.

    ![image22][image22]

8.  Sobald die Visual Studio Online-Erweiterung installiert wird, klicken Sie auf Gehe zu.

    ![image23][image23]

9.  Die Registerkarte eines Browsers wird geöffnet, in dem Sie eine Entwicklung IDE direkt im Browser angezeigt. Beachten Sie, dass der unten in Chrome verwendet wird.

    ![image24][image24]

10. Sie können mehrere Aktivitäten wie bearbeiten Dateien ausführen, Hinzufügen von Dateien und Ordnern und Inhalt der live-Website herunterladen. Stellen Sie eine schnelle bearbeiten, zu der Datei SamplePage.html.

    ![image25][image25]

11. In wenigen Minuten werden die Änderungen automatisch gespeichert. Wenn Sie wieder zu der Seite navigieren, können Sie die Änderungen angezeigt. Beibehalten in Meinung live Bearbeitungen wie diese höchstwahrscheinlich sind nicht für die Herstellung Umgebungen geeignet. Jedoch erleichtern die Tools sehr schnelle vorzunehmen, für Entwickler und Umgebungen testen.

    ![image26][image26]

    ![image27][image27]

12. Rückwärtsrichtung an die Blade Tools und unter der Kategorie Entwicklung, klicken Sie auf Leistung testen.

    ![image28][image28]

13. Sie müssen ein Team Services-Konto einrichten. Hier finden Sie weitere Informationen hierzu: [Erstellen eines Team Services-Kontos](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)

14. Klicken Sie auf neu, um einen Testanruf Leistung zu erstellen.

    ![image29][image29]

    Konfigurieren Sie die verschiedenen Werte, und klicken Sie auf am unteren Rand der Dialog Test ausführen, um eine Leistungstest zu starten.

    ![image30][image30]

    ![image31][image31]

1.  Nachdem der Test wird gestartet, können Sie den Status überwachen.

    ![image32][image32]

    Nachdem der Test abgeschlossen ist, klicken Sie auf das Ergebnis klicken, werden weitere Details wird.

    ![image33][image33]

2.  In diesem Beispiel haben Sie einen small Testlauf, damit gibt es begrenzte Daten zu analysieren, aber Sie können finden Sie unter verschiedene Kennzahlen als auch den Test in dieser Ansicht erneut ausführen, erstellt. Mit dem Azure-Portal kann erstellen, ausführen und Analysieren von Web Performance Tests einfacher Prozess. Die folgenden Screenshots zeigen die Leistungsdaten.

    ![image34][image34]

    ![image35][image35]

    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a>Für die Überwachung und Problembehandlung bei einer app

Azure bietet viele Funktionen für die Überwachung und Problembehandlung ausgeführt Applications.

1.  Wählen Sie im Portal Azure für unsere Web app Tools aus.

    ![image37][image37]

2.  Beachten Sie unter der Kategorie Behandeln von verschiedenen Optionen für die Verwendung von Tools zum potenzieller Probleme mit einer laufenden app zu behandeln. Sie können beispielsweise Live HTTP-Verkehr, Korrektur aktivieren, Protokolle anzeigen und überwachen ausführen.

    ![image38][image38]

3.  Wählen Sie die Website Kennzahlen, schnell einen Überblick über einige HTTP-Codes aus.

    ![image39][image39]

4.  Wählen Sie als Dienst Diagnose aus. Wählen Sie den Anwendungstyp, und wählen Sie dann ausführen.

    ![image40][image40]

    Eine Auflistung beginnt.

    ![image41][image41]

5.  Wählen Sie das entsprechende Protokoll potenzielle Probleme diagnostizieren. Sie müssen die Protokollierung aktivieren, um alle verfügbaren Datenoptionen wie HTTP-Protokolle angezeigt.

    ![image42][image42]

    Indem Sie auf die Sicherungsdatei Arbeitsspeicher können Sie herunterladen und Analysieren einer DebugDiag Analysebericht um potenzielle Probleme zu finden.

    ![image43][image43]

6.  Um weitere Daten anzeigen zu können, müssen Sie zusätzliche Protokollierung aktivieren. Im Portal Azure navigieren Sie zu der Web-app, und wählen Sie Einstellungen aus.

    ![image44][image44]

7.  Führen Sie einen Bildlauf nach unten zu den Features Kategorie, und wählen Sie Diagnoseprotokolle.

     ![image45][image45]

8.  Beachten Sie die verschiedenen Optionen für die Protokollierung. Schalten Sie Web-Server-Protokollierung, und klicken Sie auf Speichern.

    ![image46][image46]

9.  Wieder in den Bereich des Tools für die app verschieben und Diagnose als Dienst auswählen, und klicken Sie auf Ausführen, um die Sammlung von Daten erneut ausführen.

    ![image47][image47]

10. Die HTTP-Protokollierung Einstellung aktiviert ist, sehen Sie nun für HTTP-Protokollen gesammelte Daten.

    ![image48][image48]

11. Durch Klicken auf das Protokoll der HTML-Datei, erstellen Sie einen Rich-Browser-basierten Bericht zur weiteren Untersuchung an.

    ![image49][image49]

12. Wechseln Sie zum Abschnitt "Tools" Azure-Portal für die app. Führen Sie einen Bildlauf zum Abschnitt "Tools", und wählen Sie Process Explorer.

    ![image50][image50]

13. Prozess-Explorer die Option auswählen, können Sie die Details über das Ausführen von Prozessen anzeigen. Hinweis unter Sie können detailliert auswerten Prozesse und sogar Prozesse alle vom Azure-Portal beenden.

    ![image51][image51]

    ![image52][image52]

14. Wechseln Sie zum das Blade Einstellungen auf der linken Seite. Klicken Sie auf neue Unterstützung anfordern.

    ![image53][image53]

15. Aus dem Blade auf der rechten Seite können Sie füllen Sie die Details der Probleme, geben Sie Kontaktinformationen und sogar diagnostische Daten hochladen. Im Portal Azure ermöglicht das Arbeiten mit Microsoft-Support problemlos.

    ![image54][image54]

    ![image55][image55]

    Das Azure-Portal bietet leistungsfähige und vertraute Tools Erfahrung, um zu überwachen und Behandeln von Problemen mit unseren laufenden Applications. Sie können auch Aktion schnell zu nutzen, indem Sie beispielsweise Wiederverwendung von Prozessen, aktivieren und Deaktivieren von verschiedenen Daten Websitesammlungen und sogar Integration mit Microsoft professional Support Aufgaben ausführen.

## <a name="general-application-management"></a>Allgemeine Anwendungsverwaltung

Beim Verwalten von Applications, müssen Sie oft eine Vielzahl von Aktivitäten wie Strategien konfigurieren, implementieren und Verwalten von Identitätsanbieter und konfigurieren rollenbasierte Access-Steuerelements ausführen. Wie bei anderen DevOps Erfahrung integriert Azure-Plattform diese Aufgaben direkt in das Portal an.

1.  Um sicherzustellen, dass Sie das Web App Datenverlust sichere aufbewahrt werden, müssen Sie Sicherungskopien konfigurieren. Navigieren Sie zu dem Bereich der Einstellungen für Ihre Web app.

    ![image56][image56]

2.  Das Blade auf der rechten Seite einen Bildlauf nach unten zu den Features Kategorie.

     ![image57][image57]

1.  Wählen Sie Sicherungskopien; eine Blade wird geöffnet, auf der rechten Seite.

    ![image58][image58]

2.  Klicken Sie auf konfigurieren, und wählen Sie ein Speicherkonto aus dem Blade auf der rechten Seite.

    ![image59][image59]

3.  Jetzt erstellen Sie, und wählen Sie einen Speichercontainer, Ihre Sicherungskopien enthalten soll. Klicken Sie auf am unteren Rand der Blade erstellen. Wählen Sie dann den Container aus.

    ![image60][image60]

4.  Nachdem Sie den Container ausgewählt haben, können Sie Zeitpläne sowie Setup Sicherungskopien für Ihre Datenbanken konfigurieren. In diesem Szenario klicken Sie auf das Symbol.

     ![image61][image61]

5.  Nach dem Speichern, einen Bildlauf nach Sicherungskopien wieder an die Blade auf der linken Seite. Klicken Sie auf Jetzt sichern, um die Anwendung zurück.

     ![image62][image62]

6.  In wenigen Minuten wird eine Sicherungskopie erstellt. Beachten Sie die Option jetzt wiederherstellen, klicken Sie auf den Screenshot unten an.

     ![image63][image63]

7.  Klicken Sie auf Jetzt wiederherstellen, und überprüfen Sie die Optionen, um das Blade auf der rechten Seite. Wählen Sie eine geeignete Sicherung und in einer früheren Zustand nach Bedarf auf einfache Weise wiederherstellen. Azure-Portal hat uns einfachen Wiederherstellen nach Datenverlusten für die app einfach aktivieren beigetragen.

     ![image64][image64]

8.  Wechseln Sie zum das Blade Einstellungen auf der linken Seite, und klicken Sie unter Features, und wählen Sie Authentifizierung/Autorisierung.

     ![image65][image65]

9.  Wählen Sie in das Blade auf der rechten Seite App-Authentifizierung ein. Beachten Sie die Vielzahl von Optionen, die Sie mit populäre konfigurieren können.

     ![image66][image66]

10. Beachten Sie die Optionen für den Bereich, und wählen Sie den Anbieter Ihrer Wahl. Können Sie einen App-ID und App geheim und schnell Facebook-Authentifizierung für die app aktivieren. Im Portal Azure ermöglicht Authentifizierung als eine sofort einsatzbereite Lösung für apps.

     ![image67][image67]

11. In Rückwärtsrichtung an die Blade Einstellungen, und wählen Sie Benutzer unter der Kategorie Ressourcenmanagement.

     ![image68][image68]

12. Untersuchen Sie die verschiedenen Optionen zum Hinzufügen von Rollen und Benutzer in das Blade auf der rechten Seite. Das Azure-Portal können Sie ganz einfach RBAC (rollenbasierte Access-Steuerelement) für die Anwendung steuern.

     ![image69][image69]


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm, die Teil der Power mit der Azure-Plattform durch schnell aktivieren fortlaufender Bereitstellung für eine Web-app, Durchführung der verschiedenen Entwicklung aus, und Testen der Aktivitäten, die Überwachung und Problembehandlung bei einer live-app und schließlich Key Strategien wie Wiederherstellung, Identität und Steuerung des Benutzerzugriffs rollenbasierte Verwaltung verdeutlicht. Die Azure-Plattform ermöglicht eine integrierte Lösung für diese DevOps Workflows, und Sie können effizienter arbeiten, indem Sie im Kontext für die aktuelle Aufgabe zur Vermeidung.


## <a name="next-steps"></a>Nächste Schritte 

* Azure Ressourcenmanager ist wichtig für das DevOps auf der Azure-Plattform aktivieren.  Weitere Informationen finden Sie [Azure Ressourcenmanager Übersicht](../azure-resource-manager/resource-group-overview.md).

* Weitere Informationen finden Sie über die App-Verwaltungsdienst Azure-Bereitstellung [Bereitstellen Ihre app Azure-App-Verwaltungsdienst](../app-service-web/web-sites-deploy.md)


[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
