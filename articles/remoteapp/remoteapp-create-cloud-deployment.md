<properties 
    pageTitle="So erstellen Sie eine Cloud-Sammlung von Azure RemoteApp | Microsoft Azure" 
    description="Erfahren Sie, wie eine Bereitstellung von Azure RemoteApp zu erstellen, die Daten in der Cloud Azure speichert." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>So erstellen Sie eine Cloud-Sammlung von Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Es gibt zwei Arten von [Azure RemoteApp Websitesammlungen](remoteapp-collections.md)aus: 

- Cloud: befindet sich vollständig in Azure. Sie können auch alle Daten in der Cloud speichern (, damit eine Auflistung Cloud nur) oder zum Herstellen einer Verbindung eine VNET mit Ihrer Websitesammlung und Speichern von Daten vorhanden.   
- Hybrid: enthält ein virtuelles Netzwerk für lokale Access - dies erfordert die Verwendung von Azure AD- und einer lokalen Active Directory-Umgebung.

In diesem Lernprogramm führt Sie durch den Vorgang des Erstellens einer Cloud-Websitesammlungs. Es gibt vier Schritte aus: 

1.  Erstellen einer Websitesammlungs Azure RemoteApp.
2.  Konfigurieren Sie optional Verzeichnissynchronisation. Bei Verwendung von Azure AD + Active Directory, müssen Sie Benutzer, Kontakte und Kennwörter aus Ihrem lokalen Active Directory in Ihrem Azure AD-Mandanten zu synchronisieren.
5.  Veröffentlichen von apps.
6.  Konfigurieren des Benutzerzugriffs.


**Vorbemerkung**

Sie müssen vor dem Erstellen der Auflistung die folgenden Schritte durchführen:

- [Registrieren](https://azure.microsoft.com/services/remoteapp/) für Azure RemoteApp an. 
- Sammeln Sie Informationen zu den Benutzern, denen Sie Zugriff gewähren möchten. Dies kann entweder Microsoft-Kontoinformationen oder Active Directory-Konto Arbeitsinformationen für Benutzer sein.
- Dieses Verfahren setzt voraus, dass Sie entweder gezeigt, verwenden Sie eines der Bilder Vorlage liegen als Teil Ihres Abonnements oder werden, dass Sie bereits das Vorlagenbild hochgeladen, die, das Sie verwenden möchten. Wenn Sie eine andere Vorlagenbild hochladen müssen, können Sie das von der Seite Vorlage Bilder erreichen. Nur klicken Sie auf **eine Vorlagenbild hochladen** , und führen Sie die Schritte im Assistenten. 
- Möchten Sie das Office 365 ProPlus Bild verwenden? Schauen Sie sich Informationen [hier](remoteapp-officesubscription.md).
- Möchten Sie benutzerdefinierte apps bereitstellen oder diese LOB-Programme? Erstellen Sie ein neues [Bild](remoteapp-imageoptions.md) , und verwenden Sie sie in der Cloud-Websitesammlung.
- Ermitteln Sie, ob Sie die Verbindung zu einer VNET herstellen müssen. Wenn Sie die Verbindung zu einem VNET auswählen, vergewissern Sie sich diese entsprechen den [Richtlinien des Ziehpunkts](remoteapp-vnetsizing.md) und die It [können eine Verbindung mit RemoteApp](remoteapp-vnet.md). Schauen Sie sich im [Planungshandbuch VNET ](remoteapp-planvnet.md)für Weitere Informationen.
- Wenn Sie eine VNET verwenden, entscheiden Sie, ob Sie es mit Ihrem lokalen Active Directory-Domäne verknüpfen möchten.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Schritt 1: Erstellen einer Websitesammlungs Cloud - mit oder ohne ein VNET##


Gehen Sie zum **Erstellen einer Websitesammlung Cloud nur**folgendermaßen vor:

1. Im Verwaltungsportal wechseln Sie zu der Seite RemoteApp.
2. Klicken Sie auf **Neu > Quick erstellen**.
3. Geben Sie einen Namen für die Auflistung, und wählen Sie Ihre Region.
4. Wählen Sie den Plan, dass Sie - standard oder grundlegende verwenden möchten.
5. Wählen Sie die Vorlage für diese Websitesammlung verwendet werden soll. 

    **Tipp:** Ihr Abonnement für RemoteApp im Lieferumfang von [Vorlage Bildern](remoteapp-images.md) mit Office 365 oder Office 2013 (für verwenden Sie zum Testen) Programme, einige veröffentlicht (beispielsweise Word) und andere veröffentlichen möchten. Sie können auch ein neues [Bild](remoteapp-imageoptions.md) erstellen und verwenden es in der Cloud-Websitesammlung.


1. Klicken Sie auf die **Auflistung RemoteApp erstellen**.
    
    **Wichtige:** Sie können in Ihrer Websitesammlung Bereitstellung von bis zu 30 Minuten dauern.

Nachdem Sie Ihre Azure RemoteApp Sammlung erstellt wurde, doppelklicken Sie auf den Namen der Sammlung. Die von der Seite **Schnellstart** bringt – Dies ist die Stelle, an der Sie die Websitesammlung konfiguriert haben.

Verwenden Sie die folgenden Schritte aus, um eine **Cloud + VNET Websitesammlung**erstellen:

1. Im Verwaltungsportal wechseln Sie zu der Seite Azure RemoteApp.
2. Klicken Sie auf **neue** > **mit VNET erstellen**.
3. Geben Sie einen Namen für Ihre Websitesammlung.
4. Wählen Sie den Plan, dass Sie - standard oder grundlegende verwenden möchten.
5. Wählen Sie die VNET, die Sie bereits erstellt haben. Sie wissen nicht wie erledigen? Jetzt sind die Schritte im Thema [Hybrid](remoteapp-create-hybrid-deployment.md) aus.
6. Entscheiden Sie, ob Sie Ihre Sammlung an Ihre Domäne teilnehmen möchten. Wenn Ja, müssen Sie AD verbinden verwenden Azure AD integriert werden soll, und Ihre Active Directory-Umgebung. Wird unterhalb in **Schritt2**behandelt.
6. Klicken Sie auf die **Auflistung RemoteApp erstellen**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Schritt 2: Konfigurieren von Active Directory-Verzeichnissynchronisation (optional) ##

Wenn Sie Active Directory verwenden möchten, erfordert Azure RemoteApp Verzeichnissynchronisation zwischen Azure Active Directory und der lokalen Active Directory-Benutzer, Kontakte und Kennwörter zu Ihrem Mandanten Azure Active Directory synchronisiert. Finden Sie unter [Konfigurieren von Active Directory für Azure RemoteApp](remoteapp-ad.md) für die Planung von Informationen ein. Sie können auch direkt zu [AD verbinden](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) Informationen wechseln.

## <a name="step-3-publish-apps"></a>Schritt 3: Veröffentlichen von apps ##

Eine app Azure RemoteApp ist der app oder Programm, die für Ihre Benutzer bereitgestellt. Es befindet sich das Vorlagenbild, die Sie für die Websitesammlung hochgeladen. Wenn ein Benutzer eine app greift auf, die app in ihrer lokalen Umgebung ausgeführt wird, aber es wirklich in einem virtuellen Computer in Azure ausgeführt wird. 

Damit die Benutzer apps zugreifen können, müssen Sie diese veröffentlicht – veröffentlichen apps ermöglicht die Benutzer der apps über Remote Desktop-Client zugreifen.
 
Sie können mehrere apps für Ihre Sammlung Azure RemoteApp veröffentlichen. Klicken Sie aus der Veröffentlichungsseite auf **Veröffentlichen** , um ein Programm hinzuzufügen. Sie können entweder über **das Startmenü des Bilds Vorlage oder die Pfadangabe auf das Vorlagenbild für die app** veröffentlichen. Wenn Sie über **das Startmenü** hinzufügen, wählen Sie die app zu veröffentlichen. Wenn Sie den Pfad zu der app bereitstellen auch, geben Sie einen Namen für die app und den Pfad zu dem sie auf das Vorlagenbild installiert ist.

## <a name="step-4-configure-user-access"></a>Schritt 4: Konfigurieren des Benutzerzugriffs ##

Jetzt, da Sie Ihre Sammlung erstellt haben, müssen Sie die Benutzer hinzufügen, die Sie wäre remote Ressourcen verwenden möchten. Wenn Sie Active Directory verwenden, zum Benutzer Bereitstellen von Access, um in den Active Directory-Mandanten mit dem Abonnement verknüpft ist vorhanden sein, müssen Sie diese Sammlung erstellen.

1.  Klicken Sie auf der Seite Schnellstart **Konfigurieren des Benutzerzugriffs**. 
2.  Geben Sie das Konto Arbeit (aus dem Active Directory) oder Microsoft-Konto, dem Sie Zugriff gewähren möchten.

    **Hinweise:** 

    Stellen Sie sicher, dass Sie verwenden die “user@domain.com” Format.

    Wenn Sie Office 365 ProPlus in Ihrer Websitesammlung verwenden, müssen Sie die Active Directory-Identitäten für Ihre Benutzer verwenden. Auf diese Weise können Lizenzierung überprüfen. 

3.  Nachdem die Benutzer überprüft werden, klicken Sie auf **Speichern**.


## <a name="next-steps"></a>Nächste Schritte ##

Das war's auch – erfolgreich erstellt und Ihre RemoteApp Azure-Cloud-Sammlung bereitgestellt haben. Im nächsten Schritt wird, dass Ihre Benutzer herunterladen und Installieren des Remote Desktop-Clients. Sie können die URL für den Client auf der Seite Azure RemoteApp Schnellstart suchen. Klicken Sie dann Benutzern die Anmeldung in den Client und den Zugriff der apps, die Sie veröffentlicht.

### <a name="help-us-help-you"></a>Helfen Sie uns Ihnen helfen 
Wussten Sie schon, dass zusätzlich zur Bewertung der in diesem Artikel und Erstellen von Kommentaren nach unten unter, Sie im Artikel selbst ändern können? Fehlt etwas? Ein Problem? Schreiben ich etwas, die nur verwirrend ist? Bildlauf nach oben, und klicken Sie auf **Bearbeiten, klicken Sie auf GitHub** , um die Änderungen vornehmen – die werden zu uns kommen, zur Überprüfung, und klicken Sie dann, sobald wir sie melden Sie sich auf deaktivieren, sehen Sie Ihre Änderungen und direkt hier Verbesserungen.