<properties 
    pageTitle="Azure RemoteApp häufig gestellte Fragen zu | Microsoft Azure" 
    description="Erfahren Sie Antworten auf häufig gestellte Fragen zur Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Azure RemoteApp häufig gestellte Fragen

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wir haben die folgenden Fragen zur Azure RemoteApp gehört. Haben Sie andere? Besuchen Sie die [Foren RemoteApp](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) , und lassen Sie uns wissen Sie, was müssen Sie wissen, oder einen Kommentar Dropdown-Liste unter.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Gefunden nicht, wonach Sie suchen? Haben Sie eine Frage, die wir beantworten haben?
Wenn Sie die Informationen nicht finden können Sie benötigen, oder Sie haben eine zusätzliche Frage, die wir hier nicht behandelt sind, wechseln Sie zur [Azure RemoteApp-Forum](http://aka.ms/araforum) und Ihre Frage es. Wir können jederzeit weitere Antworten finden Sie hier hinzufügen.

## <a name="what-is-azure-remoteapp"></a>Was ist Azure RemoteApp? ##


- **Was ist Azure RemoteApp?** RemoteApp ist, dass ein Azure Service hilft Sie remote-sicheren Zugriff auf Applications aus vielen verschiedenen Benutzer Geräten bereit. Weitere Informationen zum [Azure RemoteApp](remoteapp-whatis.md).
- **Was sind die Bereitstellungsoptionen?** Es gibt zwei Arten von RemoteApp Websitesammlungen: Cloud und Hybrid. Sie müssen eine hängt eine Reihe von Faktoren, wie die benötigten beitreten zu einer Domäne aus. Wir über alle diese Entscheidungen sprechen [hier](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Tipps zur Verwendung von Azure RemoteApp ##
- **Wie lange, bis ich getrennt bin? Wie lange können ich im Leerlauf sein, bevor Sie mir die Boot mitteilen?** 4 Stunden. Wenn Sie oder ein Benutzer ist für 4 Stunden im Leerlauf, werden Sie automatisch aus Azure RemoteApp angemeldet sein. Schauen Sie sich die anderen Standardeinstellungen in [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md).
- **Kann ich diesen Dienst kostenlos ausprobieren?** Ja. Es ist eine kostenlose Testversion für 30 Tage verfügbar. Nach der Enden zum Testen, können Sie die Umstellung auf ein kostenpflichtiges Konto (das in der Herstellung verwendet werden können) oder mithilfe des Diensts beenden. Wechseln zu [portal.azure.com](http://portal.azure.com) Sie zunächst Ihre kostenlose Testversion – erstellen Sie eine neue Instanz der RemoteApp. Mit dem kostenlosen Testversion können Sie 2 Instanzen von RemoteApp mit 10 Benutzer pro Instanz erstellen. Denken Sie daran, dass diese Testversion nur 30 Tage lang verfügbar ist.
## <a name="azure-remoteapp-subscription-details"></a>Azure RemoteApp Abonnementdetails ##

- **Was sind die Grenzwerte Service?** Sie können die Standardeinstellungen und Dienst Beschränkungen des Azure RemoteApp in [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md)kennen. Lassen Sie uns wissen Sie, wenn Sie weitere Fragen haben.
- **Wie viele Benutzer muss ich mich haben?** Es ist mindestens 20 Benutzer aus. Wiederholen Sie zum super deutlich zu sagen, die ich mich – das MINIMUM ist 20. Ihnen werden 20 berechnet. 
- **Was kostet RemoteApp Kosten?** Schauen Sie sich [Azure RemoteApp Preise Details ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Kostet eine Art der Auflistung mehr als eine andere?** Ja, können sie, je nach Ihren Anforderungen für die Websitesammlung. Eine Auflistung Hybrid erfordert eine Verbindung mit Ihrem lokalen Netzwerk Azure RemoteApp. Wenn Sie eine vorhandene VNET/Express-Routing verwenden, ist es ohne zusätzliche Kosten. Aber wenn Sie eine neue Azure VNET und ein Gateway oder Express-Routing verwenden, werden Sie für die [Option VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) oder [Express-Routing](https://azure.microsoft.com/pricing/details/expressroute/)Rechnung. Diese Kosten (unter den Links im Detail) wird auf Ihre monatliche Azure RemoteApp Kosten.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Websitesammlungen – was unterstützt wird, welche sollten Sie verwenden, und andere Personen
- **Werden benutzerdefinierte Linie-von-branchenspezifische Applikationen werden unterstützt?** Ja. Verwenden einer benutzerdefinierten Anwendungs in Azure RemoteApp, Erstellen eines [benutzerdefinierten Vorlagenbild](remoteapp-create-custom-image.md), und klicken Sie dann auf die Sammlung RemoteApp hochladen.
- **Funktionieren meine benutzerdefinierten LOB-Anwendung in Azure RemoteApp?** Die beste Methode zum Abbildung, die sich besteht darin, ihn zu testen. Schauen Sie sich das [RD Compatibility Center](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Welches Verfahren Bereitstellung (Cloud oder Hybrid) für meine Organisation am besten geeignet ist?** Hybrid Websitesammlungen erstellen, die am häufigsten abgeschlossen Oberfläche gewünschte vollständige Integration mit einmaliges Anmelden (SSO) und sichere lokalen Netzwerkkonnektivität. Cloud-Sammlungen bieten eine agile und einfache Möglichkeit zum die Bereitstellung über mehrere Authentifizierungsmethoden isolieren. Weitere Informationen zu den [Optionen für die Bereitstellung](remoteapp-whatis.md).
- **Wir haben SQL oder einer anderen Datenbank entweder lokal oder in Azure. Welche Art von Bereitstellung sollen wir verwenden?** Die hängt davon ab, wo finde ich die SQL oder Back-End-Datenbank. Wenn die Datenbank in ein privates Netzwerk ist, verwenden Sie die Hybrid-Auflistung. Wenn die Datenbank im Internet offen gelegt wird und Client Verbindungen ermöglicht, um eine Verbindung herstellen, können Sie die Cloud-Sammlung.
- **Wissenswertes zu Zuordnung zu Laufwerk, USB und seriell, gemeinsame Nutzung der Zwischenablage und Drucker Umleitung** Alle diese Features werden in Azure RemoteApp unterstützt. Freigeben und Drucker Umleitung der Zwischenablage ist standardmäßig aktiviert. Weitere Informationen finden Sie Informationen zu Umleitung [hier](remoteapp-redirection.md). 


## <a name="template-images"></a>Vorlage Bilder
- **Kann ich eine Cloud oder vorhandenen virtuellen Computern wie die Vorlage für meine RemoteApp Websitesammlung verwenden?** Ja! Sie können ein Bild basierend auf einer Azure-virtuellen Computer erstellen, verwenden eines der Bilder in Ihrem Abonnement enthalten oder erstellen ein benutzerdefiniertes Bild. Schauen Sie sich die [RemoteApp Bildoptionen](remoteapp-imageoptions.md).


## <a name="network-options"></a>Netzwerkoptionen
- **Die Sammlung Hybrid erfordert eine VNET. Können wir unsere vorhandene VNET verwenden?** Sie können die vorhandenen VNET ist eine VNET Azure. Finden Sie unter "Schritt 1: Einrichten Ihres Netzwerks virtuelle" in den [Hybrid Websitesammlung Anweisungen](remoteapp-create-hybrid-deployment.md) für Weitere Informationen.
- **Kann ich eine VNET mit einer Websitesammlung Cloud verwenden?** Tatsächlich um können Sie aus. Schauen Sie sich das [Erstellen einer Websitesammlung Cloud](remoteapp-create-cloud-deployment.md), besonders Schritt 1 für Weitere Informationen.

## <a name="authentication-options"></a>Authentifizierungsoptionen



- **Wie sieht Authentifizierung? Welche Methoden werden unterstützt?** Die Cloud-Sammlung unterstützt das Microsoft-Konten und Azure Active Directory-Konten, die ebenfalls Office 365-Konten sind. Die Sammlung Hybrid unterstützt nur Azure Active Directory-Konten, die wurden (mit einem Tool wie [Azure Active Directory-Synchronisierung](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) synchronisiert aus einer Windows Server Active Directory-Bereitstellung; insbesondere entweder mit der Option für die Synchronisierung von Kennwörtern synchronisiert oder mit Active Directory Federation Services (AD FS) Föderation konfiguriert synchronisiert. Sie können auch [Mehrstufige Authentifizierung (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/)konfigurieren.

>[AZURE.NOTE]Der Azure-Active Directory-Benutzer muss aus den Mandanten, die Ihr Abonnement zugeordnet ist. (Sie können anzeigen und Ändern Ihres Abonnements auf der Registerkarte **Einstellungen** im Portal. Informationen finden Sie unter [Ändern der Azure-Active Directory-Mandanten untersuchten RemoteApp](remoteapp-changetenant.md) Weitere.)

- **Warum gewähren kann nicht ich meine Azure Active Directory-Konto-Zugriff?** Der Azure-Active Directory-Benutzer muss aus dem Verzeichnis, die Ihr Abonnement zugeordnet ist. Sie können anzeigen oder Ändern dieses Verzeichnis auf der Registerkarte Einstellungen im Portal. Weitere Informationen finden Sie unter [Ändern der Azure-Active Directory-Mandanten von RemoteApp verwendet](remoteapp-changetenant.md) .

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Kunden - welches Gerät kann ich mithilfe von Azure RemoteApp zugreifen?
Sie können gute Clientinformationen, einschließlich der Schritte zur Installation von den verschiedenen Clients am [Zugriff auf Ihre apps im Azure RemoteApp](remoteapp-clients.md)suchen.

- **Welche Geräte und Betriebssysteme unterstützen die Clientanwendungen?**
Erste Computern und Tablets: 
    - Windows-10 (Client Preview)
    - Windows 8.1 und Windows 8
    - Windows 7 Servicepack 1
    - Mac OS X
    - Windows RT
    - Android-tablets
    - iPads und die Telefone:
    - iPhone
    - Android-Smartphone
    - Windows Phone
 
    [Herunterladen](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) einer RemoteApp Client jetzt aus.
- **Unterstützt Azure RemoteApp dünnen Clients?** Ja, werden die folgenden Windows Embedded dünnen Clients unterstützt:
    - Windows Embedded Standard 7
    - Windows Embedded-8 Standard
    - Windows Embedded 8.1 Industry Pro
    - Windows 10 IoT Enterprise

- **Welche Version von Windows Server ist für den Remote Desktop Session Host (RDSH) unterstützt?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Support und feedback


- **Was ist der Support-Plan für RemoteApp?** Support für abrechnungs- und Abonnementsupport Management ist kostenlos zur Verfügung gestellt. Technischen Support ist der [Azure Service-Pläne](https://azure.microsoft.com/support/plans/)erhältlich. Sie können auch durch unsere [Azure Diskussionsforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)kostenlose Community Support abrufen. 
- **Wie senden ich Feedback?** Besuchen Sie das [Forum Feedback](https://feedback.azure.com/forums/247748-azure-remoteapp/)ein.
- **Wer kann ich sprechen, um weitere Informationen zur Azure RemoteApp?** Zusätzlich zu unseren [Diskussionsforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), also gut, Fragen stellen, können Sie die wöchentlichen [Fragen der Experten Webinar](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html)verknüpfen, wo wir alles, was RemoteApp sprechen.
- **Wissenswertes zu RemoteApp Dokumentation** Danke so, dass Sie aufgefordert werden. Zusätzlich zu den Hilfeinhalt in den Einzug Portal-Hilfe (klicken Sie einfach auf die **?** Klicken Sie auf einer beliebigen Seite im Portal) die folgenden Artikeln stehen Sie vermitteln Akzente RemoteApp:
    - **Erste Schritte:**
        - [Was ist RemoteApp?](remoteapp-whatis.md)
        - [Was ist RemoteApp Vorlage Bilder?](remoteapp-images.md)
        - [Wie funktioniert die Lizenzierung?](remoteapp-licensing.md)
        - [Wie funktionieren RemoteApp und Office zusammen?](remoteapp-o365.md)
        - [Wie funktioniert die Umleitung in RemoteApp](remoteapp-redirection.md)?
    - **Bereitstellen:**
        - [Erstellen einer benutzerdefinierten Vorlage Bilder](remoteapp-create-custom-image.md)
        - [Erstellen einer Websitesammlung hybrid](remoteapp-create-hybrid-deployment.md)
        - [Erstellen einer Websitesammlung cloud](remoteapp-create-cloud-deployment.md)
        - [Konfigurieren von Azure-Active Directory für RemoteApp](remoteapp-ad.md)
        - [Veröffentlichen einer app in RemoteApp](remoteapp-publish.md)
    - **Verwalten:**
        - [Hinzufügen von Benutzern](remoteapp-user.md)
        - [Bewährte Methoden zum Konfigurieren und Verwenden von RemoteApp](remoteapp-bestpractices.md)  

    Videos! Wir haben auch eine Reihe von Videos über RemoteApp. Einige stellen Einführung ([Einführung in Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), während andere Personen Sie Bereitstellung ([Bereitstellung Cloud](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) und [hybridbereitstellung](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)) durchzuführen. Probieren Sie es einfach!

 
### <a name="help-us-help-you"></a>Helfen Sie uns Ihnen helfen 
Wussten Sie schon, dass zusätzlich zur Bewertung der in diesem Artikel und Erstellen von Kommentaren nach unten unter, Sie im Artikel selbst ändern können? Fehlt etwas? Ein Problem? Schreiben ich etwas, die nur verwirrend ist? Bildlauf nach oben, und klicken Sie auf **Bearbeiten, klicken Sie auf GitHub** , um die Änderungen vornehmen – die werden zu uns kommen, zur Überprüfung, und klicken Sie dann, sobald wir sie melden Sie sich auf deaktivieren, sehen Sie Ihre Änderungen und direkt hier Verbesserungen.
