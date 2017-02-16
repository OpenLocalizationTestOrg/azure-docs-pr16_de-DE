<properties
    pageTitle="Aktivieren RAS für Azure-Bereitstellungen in "Ellipse""
    description="Informationen Sie zum Remotezugriff für Azure-Bereitstellungen mit dem Azure-Toolkit für "Ellipse" zu aktivieren."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Aktivieren RAS für Azure-Bereitstellungen in "Ellipse"

Um Ihre Bereitstellungen zu beheben, möglicherweise aktiviert ist und RAS Verbindung zu des virtuellen Computers die Bereitstellung hosten. Die Remote Access-Funktionalität beruht auf den (Remotedesktopprotokoll). Sie können für die Bereitstellung RAS konfigurieren, nachdem Sie es in Azure veröffentlicht haben, oder, wenn Sie mit einem Windows-Betriebssystem "Ellipse" verwenden, Sie Remote Access konfigurieren können, bevor Sie Azure veröffentlichen. Beachten Sie, dass Sie einen remote-Desktopclient, der mit Ihrem Betriebssystem kompatibel ist benötigt werden und mit der Bereitstellung des virtuellen Computers in Azure herstellen.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>So Remotezugriff aktivieren, bevor Sie auf Azure bereitstellen

> [AZURE.NOTE] Um RAS aktivieren, bevor Sie die Anwendung in Azure bereitstellen, müssen Sie "Ellipse" unter Windows ausgeführt werden.

Die folgende Abbildung zeigt die Dialogfeld verwendet, um den Remotezugriff aktivieren **RAS** -Eigenschaften.

![][ic719494]

Es gibt zwei Möglichkeiten, um das Dialogfeld **RAS** Eigenschaften anzeigen:

* Klicken Sie auf den Link **Erweitert** im Abschnitt **RAS** Rand des Dialogfelds **in Azure veröffentlichen** .
* Öffnen Sie im Dialogfeld **Eigenschaften** von Azure Projekt.

Beim Erstellen eines neuen Projekts von Azure-Bereitstellung, kann das Projekt nicht Remote zugreifen, die standardmäßig aktiviert sind. Allerdings können Sie problemlos RAS durch Angeben von Benutzername und Kennwort in das Dialogfeld **Veröffentlichen in Azure** aktivieren. Kennwort des RAS ist verschlüsselt x. 509-Zertifikate verwenden. Wenn Sie nicht verwenden, geben Sie Ihr eigenes Zertifikat, die Verschlüsselung ist ein selbst signiertes Zertifikat, das im Lieferumfang der Azure-Plug-In für "Ellipse" abhängig. Dieses selbst signierte Zertifikat befindet sich im Ordner **Zertifikat** des Projekts Azure, sowohl als einer öffentlichen Zertifikatsdatei (SampleRemoteAccessPublic.cer) eine persönliche Informationen Exchange (PFX) Zertifikatsdatei (SampleRemoteAccessPrivate.pfx) gespeichert. Letztere enthält den privaten Schlüssel für das Zertifikat, und es wurde ein Standardkennwort **Kennwort1**. Da dieses Kennwort bekannt gegeben ist, sollte jedoch das Standardzertifikat verwendet werden nur für learning Zwecke, nicht für die Herstellung Bereitstellung. Daher als für learning Zwecke, sollten wenn remote Sitzungen für die Bereitstellung aktiviert werden soll Sie den Link " **Erweitert** " im Dialogfeld **Veröffentlichen in Azure** , um Ihr eigenes Zertifikat anzugeben klicken. Beachten Sie, dass müssen Sie hochladen die PFX-Version des Zertifikats mit Ihrem gehosteten Dienst innerhalb der Azure-Verwaltungsportal, damit diese Azure das Benutzerkennwort entschlüsseln kann.

Der Rest des Lernprogramms wird gezeigt, wie Remotezugriff für ein Projekt Azure-Bereitstellung zu aktivieren, die mit dem Remotezugriff deaktiviert ursprünglich erstellt wurde. Im Rahmen dieses Lernprogramms erstellen wir ein neues selbst signiertes Zertifikat, und deren PFX-Datei wird ein Kennwort Ihrer Wahl haben. Sie haben auch die Möglichkeit, mit einem Zertifikat, das von einer Zertifizierungsstelle ausgestellt.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>So Remotezugriff aktivieren, nachdem Sie in Azure bereitgestellt haben

Wenn Sie Remotezugriff aktivieren, nachdem Sie in Azure bereitgestellt haben, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der mit Ihrem Konto Azure Azure Verwaltungsportal
1. Wählen Sie in der Liste der **Cloud Services**aus dem bereitgestellten Clouddienst
1. Klicken Sie in der Cloud-Service-Webseite auf den Link **Konfigurieren**
1. Klicken Sie am unteren Rand der Konfigurationsseite auf den **Remote** -link
1. Wenn das Popup-Dialogfeld angezeigt wird:
    * Geben Sie die Rolle, die Sie für denen Sie sollen Remotezugriff aktivieren
    * Klicken Sie auf das Kontrollkästchen **Remotedesktop aktivieren**
    * Geben Sie einen Benutzernamen und Ihr Kennwort ein, die Sie für den Remotezugriff verwenden möchten.
    * Wählen Sie das zu verwendende Zertifikat aus.
1. Klicken Sie auf **OK** 

Sie sehen eine Meldung, dass die Änderungen Konfiguration bearbeitet wird das wenige Minuten in Anspruch nehmen kann. Nach der Änderung der Konfiguration abgeschlossen ist, führen Sie die Schritte im Abschnitt **zum Remote melden Sie sich** später in diesem Artikel aus.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Wie Sie in Ihrem Paket Remotezugriff aktivieren

1. Innerhalb des "Ellipse" im Bereich Projekt-Explorer mit der rechten Maustaste Azure Projekt, und klicken Sie auf **Eigenschaften**.

1. Klicken Sie im Dialogfeld **Eigenschaften** erweitern Sie **Azure** im linken Bereich, und klicken Sie auf **Remote-Zugriff**.

1. Stellen Sie sicher, dass **alle Rollen Remote Desktop-Verbindungen mit diesen Anmeldeinformationen annehmen aktivieren** aktiviert ist, klicken Sie im Dialogfeld **RAS** .

1. Geben Sie einen Benutzernamen für die Verbindung Remotedesktop.

1. Geben Sie ein, und bestätigen Sie das Kennwort für den Benutzer. Die Benutzer Namen und das Kennwort Werte in diesem Dialogfeld festlegen werden beim Herstellen einer Verbindung Remotedesktop verwendet. (Beachten Sie, dass es sich um ein separates Kennwort Ihr Kennwort PFX handelt).

1. Geben Sie das Ablaufdatum für das Benutzerkonto an.

1. Klicken Sie auf **neu** , um ein neues selbst signiertes Zertifikat erstellen. (Alternativ ein Zertifikat konnte von Ihrem System Arbeitsbereich oder einer Datei über die Schaltflächen **Arbeitsbereich** oder **Dateisystem** Hilfethemas auswählen, aber für Rahmen dieses Lernprogramms wir erstellen ein neues Zertifikat.)

    * Klicken Sie im Dialogfeld **Neues Zertifikat** Geben Sie an, und bestätigen Sie das Kennwort ein, die, das Sie für die PFX-Datei verwenden möchten.

    * Akzeptieren Sie den Wert für **Name (CN)**bereitgestellt, oder verwenden Sie einen benutzerdefinierten Namen.

    * Geben Sie den Pfad und Dateinamen ein, die, in dem das neue Zertifikat, CER-Formular gespeichert werden. Für diesen Schritt und dem nächsten Schritt fort könnten Sie den Ordner **Zertifikat** Ihres Projekts Azure verwenden, aber Sie können einen anderen Speicherort auszuwählen. Aus Gründen der in diesem Lernprogramm verwenden wir **c:\mycert\mycert.cer**. (Erstellen Sie den Ordner **c:\mycert** , indem Sie den Vorgang fortsetzen, oder verwenden Sie einen vorhandenen Ordner aus, falls gewünscht.)

    * Geben Sie den Pfad und Dateinamen ein, die Stelle, an der das neue Zertifikat und deren privater Schlüssel, klicken Sie im Formular PFX-Datei gespeichert werden. Aus Gründen der in diesem Lernprogramm verwenden wir **c:\mycert\mycert.pfx**. Ihr **Neues Zertifikat** Dialogfeld sollte folgendermaßen aussehen (die Ordnerpfade aktualisieren, wenn Sie keinen **c:\mycert**verwendet haben) folgende:

        ![][ic712275]

    * Klicken Sie auf **OK** , um das Dialogfeld **Neues Zertifikat** zu schließen.

1. Der **Remote Access** -Dialogfeld sollte etwa wie folgt aussehen:</p>

    ![][ic719495]

1. Klicken Sie auf **OK** , um das Dialogfeld **RAS** zu schließen.
    
Erneutes Erstellen der Anwendung, mit dem Build für die Bereitstellung in die cloud festlegen aus.

## <a name="to-log-in-remotely"></a>Remote anmelden

Wenn Ihre Rolleninstanz bereit ist, können die virtuellen Computern Remotezugriff auf, die eine Anwendung hostet.

* Wenn "Ellipse" auf verwenden Windows und ausgewählt **remote Desktop Start bereitstellen** während der bereitstellungs in Azure haben, Ihnen den Anmeldebildschirm Remote Desktop-Verbindung angezeigt, wenn Ihre Bereitstellung wird gestartet. Wenn Sie für den Benutzernamen und Kennwort aufgefordert werden, geben Sie die Werte, die Sie für den entfernten Benutzer angegeben haben, und wird in der Lage, melden Sie sich.

* Alternativ können Sie Remote melden Sie sich lautet über das <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure-Verwaltungsportal</a>

    * Klicken Sie in der **Cloud Services** Ansicht der Azure-Verwaltungsportal auf dem Clouddienst, klicken Sie auf **Instanzen**, klicken Sie auf eine bestimmte Instanz, und klicken Sie dann auf die Schaltfläche **Verbinden** . Die Schaltfläche **Verbinden** wird als in der Befehlsleiste in der folgenden angezeigt:

        ![][ic659273]

    * Nach dem Klicken auf die Schaltfläche **Verbinden** , werden Sie aufgefordert, eine RDP-Datei zu öffnen. Öffnen Sie die Datei, und folgen Sie den Anweisungen. (Sie konnte diese Datei auch mit Ihrem lokalen Computer speichern, und führen Sie die Datei durch Doppelklicken auf remote-Protokoll in Ihrem virtuellen Computern ohne zuerst Verwaltungsportal wechseln.)

    * Wenn Sie für den Benutzernamen und Kennwort aufgefordert werden, geben Sie die Werte, die Sie für den entfernten Benutzer angegeben haben, und wird in der Lage, melden Sie sich.

> [AZURE.NOTE] Wenn Sie bei einem nicht-Windows-Betriebssystem sind, müssen Sie verwenden einen Remote Desktop-Client, der mit Ihrem Betriebssystem kompatibel ist, und folgen den Schritten zum Konfigurieren von diesem Clients mit den Einstellungen in der RDP-Datei, die Sie heruntergeladen haben.

## <a name="see-also"></a>Siehe auch

[Azure-Toolkit für "Ellipse"][]

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"][]

[Installieren des Azure-Toolkits für "Ellipse"][] 

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
