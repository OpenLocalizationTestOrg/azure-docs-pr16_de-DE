<properties
    pageTitle="Synchronisieren den Inhalt eines Ordners Cloud auf Azure-App-Verwaltungsdienst"
    description="Erfahren Sie, wie Ihre app zu Azure-App-Verwaltungsdienst über Inhalt synchronisieren aus einem Cloud-Ordner bereitstellen."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronisieren den Inhalt eines Ordners Cloud auf Azure-App-Verwaltungsdienst

In diesem Lernprogramm erfahren Sie, wie Sie für [App-Verwaltungsdienst Azure](http://go.microsoft.com/fwlink/?LinkId=529714) bereitstellen, indem Sie die Synchronisierung Ihrer Inhalte von beliebte Cloud-Speicher-Diensten wie Dropbox und OneDrive. 

## <a name="a-nameoverviewaoverview-of-content-sync-deployment"></a><a name="overview"></a>Übersicht über die Bereitstellung von Inhalten synchronisieren

Die Bereitstellung bei Bedarf Inhalt synchronisieren wird durch die [Bereitstellung-Engine Kudu](https://github.com/projectkudu/kudu/wiki) mit App-Dienst integriert betrieben. Im [Portal Azure](https://portal.azure.com)-können Sie bestimmen Sie einen Ordner in der Cloud-Speicher, arbeiten mit Ihrem app-Codes und den Inhalt des Ordners und App-Dienst durch Klicken auf eine Schaltfläche synchronisieren. Synchronisieren von Inhalten verwendet Kudu Aktualisierungsprozess für das Erstellen und bereitstellen. 
    
## <a name="a-namecontentsyncahow-to-enable-content-sync-deployment"></a><a name="contentsync"></a>Zum Synchronisieren von Inhalten Bereitstellung aktivieren
Gehen Sie folgendermaßen vor, um Inhalte synchronisieren aus dem [Azure-Portal](https://portal.azure.com)zu aktivieren:

1. Klicken Sie in Ihrer app-vorher in Azure-Portal, auf **Einstellungen** > **Bereitstellung Quelle**. Klicken Sie auf **Quelle auswählen**, und wählen Sie dann **OneDrive** oder **Dropbox** als Quelle für die Bereitstellung. 

    ![Synchronisieren von Inhalten](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Aufgrund der zugrunde liegenden Unterschiede in den APIs wird die **OneDrive for Business** zurzeit nicht unterstützt. 

2. Führen Sie den Autorisierung Workflow zum Aktivieren der App-Verwaltungsdienst zum Zugriff auf einen bestimmten vordefinierte vorgesehenen Pfad für OneDrive oder Dropbox, alle Ihre App-Dienst Inhalt gespeichert wird.  
    Nach Genehmigung der App-Verwaltungsdienst erhalten Plattform die Option zum Erstellen von Inhalten Ordnern unter der vorgesehenen Pfad oder wählen Sie einen vorhandenen Inhalten Ordner unter dieser vorgesehenen Inhalt Pfad Sie. Die vorgesehenen Inhalt Pfade unter Ihre Cloud-Speicher-Konten für die Synchronisierungs-App-Dienst verwendet gehören die folgenden:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. Nach der anfänglichen Inhalt synchronisieren kann Inhalte synchronisieren bei Bedarf vom Azure-Portal eingeleitet werden. Bereitstellung Verlauf steht Falz **Bereitstellungen** mit.

    ![Bereitstellung Verlauf](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Weitere Informationen zur Bereitstellung von Dropbox ist unter [Bereitstellen von Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx)verfügbar. 


