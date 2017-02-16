
<properties
    pageTitle="Verwenden von Office mit Azure RemoteApp | Microsoft Azure" 
    description="Erfahren Sie, wie Office und Azure RemoteApp zusammenarbeiten"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-office-with-azure-remoteapp"></a>Verwenden von Office mit Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Sie haben zwei Optionen für das Hosten von Office-Clientanwendungen in Azure RemoteApp: Office 365 ProPlus oder Office 2013 Professional Plus-Testversion.

**Wussten Sie, dass wir haben neue Artikel besser, die bald dadurch ersetzt werden? Überprüfen Sie, [wie Ihr Office 365-Abonnement mit Azure RemoteApp verwendet](remoteapp-officesubscription.md). Es werden sämtliche Informationen, die, den Sie benötigen für die Verwendung von Office 365 + Azure RemoteApp, behandelt.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Sie können eine RemoteApp Websitesammlung mithilfe des Office 365 ProPlus Bilds der Vorlage erstellen. Mit dieser Option können Sie Ihre Office 365-Dienst zu RemoteApp zu erweitern. Benötigen Sie eine vorhandene Abonnementtyp und Ihre Benutzer für Office 365 ProPlus Dienst entweder eigenständig lizenziert werden müssen oder über den Office 365-Pläne service.

RemoteApp unterstützt freigegebene Computer Aktivierung von Office 365 an. Wenn Sie freigegebene Computer Aktivierung aktivieren, und verwenden Sie das [Office-Bereitstellungstools](http://www.microsoft.com/download/details.aspx?id=36778) für die Installation, Installationen ohne aktiviert worden zu Office 365 ProPlus. Wenn ein Benutzer Vorzeichen in einer Websitesammlung, die Office 365 enthält, überprüft Office um festzustellen, ob der Benutzer für Office 365 ProPlus bereitgestellt wurde. Wenn Ja, Office vorübergehend Office 365 ProPlus - aktiviert behält diese Aktivierung, bis die Benutzer Vorzeichen aus dem Dienst.

Um die Aktivierung von Office 365 freigegebene Computer verwenden zu können, müssen Sie eine [benutzerdefinierte Vorlage](remoteapp-create-custom-image.md) erstellen, und installieren Sie Office 365 ProPlus vorhanden, begonnen, [diese Anweisungen](https://technet.microsoft.com/library/dn782858.aspx).

Sie können Ihre Office 365 Benutzerlizenzen am im [Verwaltungsportal von Office 365](https://portal.office365.com/)verwalten. Lesen Sie weitere Informationen zu [Office 365-Service-Pläne](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Office 2013 Professional Plus-Testversion
Während einer 30 Tage-Testversion von RemoteApp werden soll können Sie das Bild des Office 2013 Professional Plus (Testversion) Vorlage zum Erstellen einer Sammlung RemoteApp. Sie können die Benutzer diese Testversion Auflistung über deren Azure Active Directory Arbeit Konten oder die Microsoft-Konten zuweisen. Es ist keine zusätzliche Abonnements erforderlich.

Dies ist eine gute Option zum Starten eines der Reifen und ein guter Gefühl für Office in RemoteApp abrufen. Ist jedoch diese Option für die Auswertung beabsichtigte und Testen nur. RemoteApp Websitesammlungen erstellt, mit dem Office 2013 Professional Plus (Testversion) Vorlagenbild können nicht Herstellung Modus gewechselt werden und werden am Ende des Testzeitraums deaktiviert.

## <a name="switching-from-trial-to-production"></a>Umsteigen von Testversion auf Herstellung
Wenn Sie Ihre kostenlose 30-Tage-Testversion beginnen, wird eine Notiz im Abschnitt RemoteApp des Portals angezeigt wie lange Sie in der Testversion verlassen haben, bevor Sie für den Übergang in ein kostenpflichtiges Konto benötigen. Sie können Ihr Konto aktivieren und in Herstellung-Modus verwenden den Link in dieser Notiz wechseln.

Wenn Sie Ihr Konto aktivieren, wirkt sich dies auf alle RemoteApp Websitesammlungen in Ihr Konto aus.

- Websitesammlungen, die mit der Windows Server 2012 R2 oder Office 365 ProPlus Vorlage Bilder ausführen werden Herstellung nahtlos Übergabe. Alle Benutzerdaten und Einstellungen, einschließlich der laufende Sitzungen, bleiben erhalten.
- Wenn Sie eine benutzerdefinierte Vorlage Bilder hochgeladen haben, werden mit diesen Bildern Websitesammlungen auch nahtlos Übergabe.
- Das Bild des Office 2013 Professional Plus (Testversion) Vorlage dient nur zu Testzwecken. Websitesammlungen mit dieser Vorlagenbild ausführen können nicht zum Herstellung umgestellt wurden werden. Sie werden in den Status "deaktiviert" gesetzt.


Wenn Sie nicht zu Herstellung Modus durch Ihre Testversion ablaufen übergehen, werden Ihre RemoteApp Sammlungen deaktiviert. Keine Sorge: Ihre Einstellungen und Benutzer Daten werden gespeichert für eine andere 90 Tage, damit Sie können weiterhin des Diensts aktivieren, und wechseln Sie zur Herstellung Modus ohne Datenverlust.
