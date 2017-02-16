<properties
    pageTitle="Azure RemoteApp Lizenzierung | Microsoft Azure"
    description="Erfahren Sie, wie die Lizenzierung in Azure RemoteApp funktioniert."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Wie funktioniert Lizenzierung in Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Damit Sie Ihre Azure RemoteApp Dienst Ihre Vorlagen, erstellt eingerichtet haben und zum Veröffentlichen von apps für Ihre Benutzer bereit sind. Aber dennoch eine Sache ist zu ermitteln: Lizenzierung. Nur funktioniert wie zur Lizenzierung RemoteApp und von den apps, die über RemoteApp freigegeben werden?

RemoteApp sind keine Windows-Lizenzen oder Remote Desktop-CAL erforderlich. Ihr Abonnement erledigt der RemoteApp Seite selbst. (Schauen Sie sich die Details über die [Preise Pläne](https://azure.microsoft.com/pricing/details/remoteapp)).

Wenn Sie eines der Bilder, die in Ihrem Abonnement enthalten ist verwenden, können Sie eine der apps auf, die diesem Bild installiert werden, ohne eine separate Lizenz freigeben. Wenn Sie das Windows Server 2012 R2 Vorlagenbild verwenden, um Ihre Sammlung zu erstellen, können Sie beispielsweise System Center Endpunkt Protection für Ihre Benutzer freigeben. Die einzigen Ausnahmen für diese Regel sind Office 365 ProPlus, die ein gesondertes Abonnement erforderlich ist, und Office 2013, die in einer Websitesammlung Herstellung gemeinsam genutzt werden kann.

Wenn Sie das Bild der Office 365-Vorlage verwenden, das im Lieferumfang RemoteApp möchten, müssen Sie einen *vorhandenen* Office 365 ProPlus-Plan verfügen. Dasselbe gilt für alle Office 365-app, die Sie veröffentlichen mithilfe einer benutzerdefinierten Vorlage. Sie müssen die apps mit Ihrem eigenen-Abonnement zu aktivieren. Dies gilt für zum Testen und bezahlte Abonnements. Wechseln Sie zur Seite Office 365 bei einem Testabonnement [Anmelden](https://go.microsoft.com/fwlink/p/?LinkID=403802) , wenn Sie das Bild der Office 365-Vorlage während der Testversion, *und Sie nicht bereits über ein Abonnement verfügen*, verwenden möchten. Finden Sie unter [wie RemoteApp und Office funktionieren zusammen?](remoteapp-o365.md) für Weitere Informationen.

Wenn während des Testzeitraums, Sie möchten nicht erhalten ein Office 365-Testabonnement, verwenden Sie die Office 2013 Professional Plus Vorlage Bild, das im Lieferumfang von RemoteApp. Diese Abbildung Vorlage kann nur 30 Tage lang verwendet werden und nicht in ein kostenpflichtiges Sammlung konvertiert werden.

Für andere apps müssen Sie sicherstellen, dass Sie die Lizenz für die app freigeben verfügen.

Auch sinnvoll, rechts? Sie können eine beliebige app, die Sie freigeben gesetzlich berechtigt sind, veröffentlichen. Und Sie benötigen, um sicherzustellen, dass Sie wirklich berechtigt sind, Ihre Programme freigeben.

Bitte beachten Sie, dass einen CAL oder Volumenlizenz Vertrag in einen Cloud-Websitesammlung verwendet werden kann. Sie *können* verwenden einen Volumenlizenz Vertrag Applications in Ihrer Websitesammlung Hybrid (mit Ausnahme von Office) aktivieren. Sie müssen nur diese auf Ihrer Vorlagenbild aus der Volumenlizenz Medien zu installieren. Führen Sie die Informationen aus den Hersteller der Anwendung, Lizenzen in einer Remote Desktop-Umgebung zu installieren.
