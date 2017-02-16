<properties 
    pageTitle="Was ist Azure RemoteApp? | Microsoft Azure" 
    description="Erfahren Sie, wie apps und Ressourcen zu einem beliebigen Gerät über Azure RemoteApp gemeinsam nutzen." 
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
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Was ist Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp bringt die Funktionalität des Programms Microsoft RemoteApp lokal, Remote Desktop Services, indem Sie auf Azure gesichert wird. Azure RemoteApp hilft Ihnen, sicheren remote-Zugriff auf Applications aus vielen verschiedenen Benutzer Geräten bereit. Azure RemoteApp hostet im Grunde nicht beständige Terminal Server Sitzungen in der Cloud, und Sie erhalten sie und Ihre Benutzer freigeben.

Sie können mit Azure RemoteApp apps und Ressourcen für Benutzer auf nahezu jedem Gerät freigeben. Wir Hosten Ihrer apps in der Cloud, was bedeutet, dass wir die Hardware erledigen und dieselbe Skalierung, um Benutzer erweitert. Sie müssen lediglich Hochladen der apps, die Sie freigeben möchten, und klicken Sie dann die Benutzer diese apps verwenden. [Benutzer erhalten, um ihre eigenen Geräte](remoteapp-clients.md), während Sie alles über das Azure Portal verwalten. Sie haben auch die Möglichkeit, mit Ihren corporate Anmeldeinformationen, lassen Sie die Sicherheit von apps und Daten zu gewährleisten.

Klicken Sie auf Weitere Informationen zu Azure RemoteApp, lesen oder, wenn wir Sie, [Probieren Sie es einfach jetzt](https://azure.microsoft.com/services/remoteapp/)bereits überzeugt haben.

Haben Fragen zu Azure RemoteApp? Schauen Sie sich unsere [häufig gestellte Fragen](remoteapp-faq.md).

Azure RemoteApp ist Teil der [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Neu!** Möchten Sie weitere Informationen zur Azure RemoteApp? Oder bereit sind, Azure RemoteApp bei überprüfen? Teilnehmen an unseren Wöchentlicher [Fragen an Experten Webinar](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp Websitesammlungen
Es gibt zwei Arten von [Azure RemoteApp Websitesammlungen](remoteapp-collections.md)aus:


- Eine **Auflistung der Cloud** in gehostet wird und Daten für Programme in der Cloud gespeichert. Benutzer können apps durch Anmelden mit ihrem Microsoft-Konto oder corporate Anmeldeinformationen synchronisiert oder Verbund mit Azure Active Directory zugreifen.

    Wählen Sie einer Zusammenstellung der Cloud Wenn die Anwendung, die Sie freigeben möchten keine Verbindung zu einer beliebigen Ressource erfordert privates Netzwerk Ihres Unternehmens (z. B. über ein VPN-Gerät). Wenn die Anwendung auf das Internet, OneDrive oder Azure Ressourcen verwendet, wird eine Auflistung der Cloud für Ihre Arbeitsweise. Es ist auch die schnellste zu erstellen.

- Eine **Auflistung Hybrid** in gehostet wird, und speichert die Daten in der Cloud Azure aber auch ermöglicht Benutzern Zugriff auf Daten und Ressourcen, die in Ihrem lokalen Netzwerk gespeichert. Benutzer können apps durch Anmelden mit ihrer corporate synchronisiert oder Verbund mit Azure Active Directory-Anmeldeinformationen zugreifen.

    Wählen Sie eine Auflistung Hybrid, wenn Sie eine Verbindung zu Ressourcen auf privates Netzwerk Ihres Unternehmens erforderlich ist. Wenn die Anwendung Zugriff auf eine der folgenden benötigt beispielsweise:

    - Dateiserver, die sich auf Ihr Intranet befindet
    - Quicken
    - Datenbanken hinter einer firewall

    Dies ist im Allgemeinen hilfreicher in großen Unternehmen mit vielen von Ressourcen an ihre private Netzwerke, die in der Cloud verschoben werden können.

Die verschiedenen Websitesammlungen haben andere Optionen, einschließlich der Netzwerken, sodass Sie herausfinden, [welche Auflistung](remoteapp-collections.md) für Sie am besten geeignet. 


### <a name="updating-your-collection"></a>Aktualisieren Ihrer Websitesammlung
Einer der wichtigsten Unterschiede zwischen der Websitesammlungen Hybrid und Cloud besteht, wie Softwareupdates verarbeitet werden. Mit einer Cloud-Auflistung, die das vorinstallierte Office 365 ProPlus oder Office 2013-Bild wird verwendet, müssen Sie keine Updates kümmern. Der Dienst verwaltet selbst und stellt Updates kontinuierlichen Produktlizenzierung apps und das Betriebssystem.

Für Websitesammlungen Hybrid sowie Cloud-Sammlungen, die eine benutzerdefinierte Vorlagenbild verwenden, können Sie für das Bild und apps verwalten. Für die Domäne Bilder können Sie die Updates mithilfe von Tools wie Windows Update, Gruppenrichtlinien oder System Center steuern.

Nachdem Sie das Bild der benutzerdefinierten Vorlage aktualisieren, Sie das neue Bild in der Cloud Azure hochladen und aktualisieren Sie dann auf die Auflistung, um das neue Bild verwenden. (Sie können die Seite Azure RemoteApp **Schnellstart** oder dem Dashboard dazu.)

Weitere Informationen finden Sie unter [Aktualisieren der Websitesammlung](remoteapp-update.md) .

## <a name="supported-remoteapp-clients"></a>Unterstützte RemoteApp-clients
Azure RemoteApp wird auf der RemoteApp-Client-apps für Windows und Windows RT als auch die Microsoft Remote Desktop-apps für Mac, iOS und Android unterstützt. Ihre Benutzer können verwenden diese apps auf ihre Mobile oder Berechnen von Geräten, um die neue Azure RemoteApp-Programme zugreifen.

Weitere Informationen zu den Clients finden Sie unter [Zugreifen auf Ihre apps im Azure RemoteApp](remoteapp-clients.md) .

## <a name="next-steps"></a>Nächste Schritte
Wechseln Sie! Probieren Sie es aus! Die folgenden Artikel helfen Ihnen beim Einstieg mit Azure RemoteApp:

- [Welche Arten von Websitesammlungen benötigen Sie für Azure RemoteApp?](remoteapp-collections.md)
- [Erstellen Sie ein Bild Azure RemoteApp](remoteapp-imageoptions.md)
- [So erstellen Sie eine Cloud-Sammlung von Azure RemoteApp](remoteapp-create-cloud-deployment.md)
- [So erstellen Sie eine Auflistung Hybrid Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Wie funktioniert Lizenzierung in Azure RemoteApp?](remoteapp-licensing.md)
- [Bewährte Methoden für die Verwendung von Azure RemoteApp](remoteapp-bestpractices.md)
- [Azure RemoteApp häufig gestellte Fragen](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Helfen Sie uns Ihnen helfen 
Wussten Sie schon, dass zusätzlich zur Bewertung der in diesem Artikel und Erstellen von Kommentaren nach unten unter, Sie im Artikel selbst ändern können? Fehlt etwas? Ein Problem? Schreiben ich etwas, die nur verwirrend ist? Bildlauf nach oben, und klicken Sie auf **Bearbeiten, klicken Sie auf GitHub** oder **Bearbeiten** , um Änderungen vorzunehmen – die werden zu uns kommen, zur Überprüfung, und klicken Sie dann, sobald wir sie melden Sie sich auf deaktivieren, sehen Sie Ihre Änderungen und direkt hier Verbesserungen.