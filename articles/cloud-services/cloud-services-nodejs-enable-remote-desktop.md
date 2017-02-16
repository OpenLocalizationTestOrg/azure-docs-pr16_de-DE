<properties 
    pageTitle="Aktivieren von Remotedesktop für Clouddienste (Node.js)" 
    description="Informationen Sie zum Aktivieren des Remote Desktop-Zugriffs für den virtuellen Computern Ihrer Anwendung Azure Node.js hosten." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Aktivieren von Remotedesktop in Azure

Remotedesktop können Sie auf den Desktop des eine Instanz der Rolle in Azure ausgeführt zugreifen. Sie können eine Remotedesktop-Verbindung zum Konfigurieren des virtuellen Computers oder Beheben von Problemen mit der Anwendung.

> [AZURE.NOTE] In diesem Artikel gilt Node.js Applikationen als Azure-Cloud-Dienst aus.


## <a name="prerequisites"></a>Erforderliche Komponenten

- Installieren und Konfigurieren von [Azure Powershell](../powershell-install-configure.md).
- Bereitstellen einer app Node.js an einen Azure-Cloud-Dienst an. Weitere Informationen finden Sie unter [Erstellen und Bereitstellen eine Anwendung Node.js zu Azure-Cloud-Dienst](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Schritt 1: Verwenden von Azure PowerShell zur Konfiguration des Diensts für den Remote Desktop-Zugriff

Um Remotedesktop verwenden zu können, müssen Sie die Definition von Azure Service und Konfiguration mit einem Benutzernamen, Kennwort und Zertifikat aktualisieren. 

Führen Sie die folgenden Schritte aus, über einen Computer, der für Ihre app-Quelldateien enthält.

1. **Windows PowerShell** als Administrator ausführen. (Aus dem **Menü Start** oder **-Startbildschirm**, suchen Sie nach der **Windows PowerShell**.)

2.  Navigieren Sie zu dem Verzeichnis, das die Definition des Diensts (.csdef) und Service-Konfiguration (.cscfg) Dateien enthält.

3. Geben Sie das folgende PowerShell-Cmdlet aus:

        Enable-AzureServiceProjectRemoteDesktop

4. Geben Sie dazu aufgefordert werden einen Benutzernamen und ein Kennwort ein.

    ![Aktivieren-azureserviceprojectremotedesktop][enable-rdp]

3.  Geben Sie das folgende PowerShell-Cmdlet, um die Änderungen zu veröffentlichen:

        Publish-AzureServiceProject

    ![Veröffentlichen azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Schritt 2: Herstellen einer Verbindung die Instanz der Rolle mit

Nachdem Sie die Definition der Update-Dienst veröffentlicht haben, können Sie mit der Rolleninstanz verbinden.

1.  Im [Azure klassischen Portal]wählen Sie **Cloud Services aus** , und wählen Sie den Dienst.

    ![Azure klassischen-portal][cloud-services]

2.  Klicken Sie auf **Instanzen**, und klicken Sie dann auf **Herstellung** oder **Staging** , um die Instanzen des Diensts anzuzeigen. Wählen Sie eine Variante aus, und klicken Sie dann auf **Verbinden** am unteren Rand der Seite.

    ![Die Seite Instanzen][3]

2.  Wenn Sie eine **Verbindung herstellen**klicken, fordert der Webbrowser, eine RDP-Datei zu speichern. Öffnen Sie diese Datei. (Beispielsweise, wenn Sie Internet Explorer verwenden, klicken Sie auf **Öffnen**.)

    ![Aufforderung zum Öffnen oder speichern Sie die RDP-Datei][4]

3.  Wenn die Datei geöffnet ist, wird die folgenden Sicherheitshinweis angezeigt:

    ![Sicherheitshinweis für Windows][5]

4.  Klicken Sie auf **Verbinden**, und eine Sicherheitshinweis für die Eingabe von Anmeldeinformationen für den Zugriff auf die Instanz angezeigt. Geben Sie das Kennwort, das Sie erstellt, in [Schritt 1 haben] [Schritt 1: konfigurieren den Dienst für den Remote Desktop Zugriff mithilfe der PowerShell Azure], und klicken Sie dann auf **OK**.

    ![Benutzername und Kennwort auffordern][6]

Wenn die Verbindung hergestellt wurde, zeigt Remote Desktop-Verbindung den Desktop der Instanz in Azure. 

![Remotedesktop][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Schritt 3: Konfigurieren Sie den Dienst, um den Remotedesktop Zugriff deaktivieren 

Wenn Sie remote desktop-Verbindungen mit der Rolleninstanzen in der Cloud nicht mehr benötigen, deaktivieren Sie mithilfe der [PowerShell Azure]remote desktop-Zugriff.

1.  Geben Sie das folgende PowerShell-Cmdlet aus:

        Disable-AzureServiceProjectRemoteDesktop

2.  Geben Sie das folgende PowerShell-Cmdlet, um die Änderungen zu veröffentlichen:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Remotezugriff auf Rolleninstanzen in Azure] 
- [Verwenden von Remotedesktop mit Azure Rollen]
- [Node.js-Entwicklercenter](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure klassischen-portal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Remotezugriff auf Rolleninstanzen in Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Verwenden von Remotedesktop mit Azure Rollen]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 