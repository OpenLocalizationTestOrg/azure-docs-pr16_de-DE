<properties
   pageTitle="Verwenden von PowerShell-Cmdlets mit Azure RemoteApp | Microsoft Azure"
   description="Informationen Sie zum Verwenden von Windows PowerShell-Cmdlets in Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Verwenden von Windows PowerShell-Cmdlets mit Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

 Sie können die Azure RemoteApp PowerShell-Cmdlets zum Verwalten Ihrer Websitesammlungen verwenden. Verwenden Sie die folgende Informationen, um anzufangen.

## <a name="get-the-cmdlets"></a>Die Cmdlets abrufen 
-------------
Zuerst herunterladen der Azure-Powershell-Cmdlets [hier](http://go.microsoft.com/?linkid=9811175)die RemoteApp Cmdlets darin enthalten sind. 

Schauen Sie sich der [Azure RemoteApp Cmdlet-Hilfe](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Konfigurieren von Azure Cmdlets, um Ihr Abonnement zu verwenden.
------------------
Folgen Sie [diesem Handbuch](../powershell-install-configure.md) , sodass Sie die Cmdlets für Ihr Abonnement Azure verwenden können.

Diese Schritte können Sie um schnell zu beginnen:

1.  Herunterladen Sie und installieren Sie der [Azure-PowerShell-Cmdlets](http://go.microsoft.com/?linkid=9811175).
2.  Starten Sie Microsoft Azure PowerShell.
3.  Führen Sie die **Hinzufügen-AzureAccount** mit Ihrem Abonnement Azure authentifiziert. Wenn Sie dazu aufgefordert werden, geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei Azure-Portal.  
4.  Führen Sie **Get-AzureSubscription** , um die Liste der Abonnements Ihr Benutzerkonto zugeordnet. 
5.  Führen Sie **Wählen Sie-AzureSubscription aus** , und geben Sie den Namen des Abonnements oder -ID, die in der PowerShell-Konsole verwenden.

Herzlichen Glückwunsch, Ihre Azure PowerShell-Konsole ist, konfiguriert und zur Verwendung bereit. Beachten Sie, dass auf Filtereinträge Schritte 2 bis 5 jedes Mal müssen Sie Sie Starten der Azure-PowerShell-Konsole.  

## <a name="create-a-cloud-collection"></a>Erstellen einer Websitesammlung cloud
--------------------
Es ist einfach, mit dem folgenden Befehl ausführen:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Der obige Befehl veröffentlicht automatisch Microsoft Office 365-Anwendungen (Excel, OneNote, Outlook, PowerPoint, Visio und Word).

Erstellen von Sammlungen kann 30 Minuten dauern, oder mehr durchführen. Dieser Befehl gibt daher eine Verlauf-ID, die Sie wie folgt verwenden können:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Nach Abschluss die Sammlung können Sie Benutzer hinzufügen, der Auflistung mit den folgenden Befehl aus:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

Und das war's schon! Diesen Benutzer sollten eine Verbindung herstellen mit der Anwendung mithilfe des Azure RemoteApp Clients gefunden werden [können](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Verfügbaren cmdlets
Es gibt zahlreiche andere Befehle, die wir haben, die Dokumentation für diese kommt in Kürze:

Cmdlets für grundlegende RemoteApp Websitesammlung: 

- Neue AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Set-AzureRemoteAppCollection
- Update-AzureRemoteAppCollection
- Entfernen-AzureRemoteAppCollection
- Hinzufügen von AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Entfernen-AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Trennen AzureRemoteAppSession
- Rufen Sie AzureRemoteAppSessionLogoff
- Senden-AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Veröffentlichen AzureRemoteAppProgram
- Aufheben der Veröffentlichung AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

Cmdlets für RemoteApp virtuelles Netzwerk:

- Neue AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Set-AzureRemoteAppVNet
- Entfernen-AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get – AzureRemoteAppVpnDeviceConfigScript
- Zurücksetzen-AzureRemoteAppVpnSharedKey

Cmdlets für RemoteApp Vorlage Bild:

- Neue AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Umbenennen-AzureRemoteAppTemplateImage
- Entfernen-AzureRemoteAppTemplateImage

Andere Cmdlets RemoteApp:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Set-AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
