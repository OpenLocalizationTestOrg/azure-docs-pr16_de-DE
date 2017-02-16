<properties
    pageTitle="Was ist Azure RemoteApp Vorlage Bilder? | Microsoft Azure"
    description="Lernen Sie die Vorlage Bilder Azure RemoteApp enthaltenen aus."
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

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Was ist Azure RemoteApp Vorlage Bilder?

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Ihr Abonnement Azure RemoteApp umfasst drei Vorlage Bilder:


- WindowsServer 2012
- Microsoft Office 365 ProPlus (Office 365-Abonnement erforderlich)
- Microsoft Office 2013 Professional Plus (nur Testversion)

> [AZURE.IMPORTANT]Ihr Abonnement Azure RemoteApp gewährt, dass Sie den Zugriff auf die Software in die Bilder, mit Ausnahme von Office 365 ProPlus, die ein gesondertes Abonnement erforderlich ist, und Office 2013, die in der Herstellung verwendet werden kann. Dies bedeutet, dass Sie die Programme oder apps auf die Vorlage Bilder für Ihre Benutzer freigeben können. Wenn Sie eine Websitesammlung, die das Windows Server 2012 R2-Bild wird verwendet erstellen, können Sie beispielsweise System Center Endpunkt Protection für Benutzer für den Zugriff über RemoteApp auf veröffentlichen.
>
> Schauen Sie sich für Weitere Informationen [zur Lizenzierung RemoteApp-Details](remoteapp-licensing.md) . Und [Verwenden von Office mit Azure RemoteApp](remoteapp-o365.md) für die Office-Lizenzierung Informationen.

Lesen Sie weiter Details auf was jedes Bild enthält.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("der herkömmlichen Image")
Diese Abbildung basiert auf das Betriebssystem von Microsoft Windows Server 2012 R2 Datacenter und weist die folgenden Rollen und Features installiert sein, damit die Azure RemoteApp Vorlage Bilder erfüllen:


- .NET Framework 4.5, 3.5.1, 3.5
- Desktop-Benutzeroberfläche
- Freihand und Handschrift Services
- Media Foundation
- Remote Desktop Session Host
- Windows PowerShell 4.0
- Windows PowerShell ISE
- WoW64-Support

Diese Abbildung weist auch die folgenden Programme installiert ist:

- Adobe Flash Player
- Microsoft Silverlight
- Microsoft System Center 2012-Endpunkt Protection
- Microsoft Windows MediaPlayer


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (Abonnement erforderlich)
Office 365 ist der Anwendung, die am häufigsten angeforderten, damit wir ein "benutzerdefiniertes" Bild für die Arbeit mit erstellt haben.

Diese Abbildung ist eine Erweiterung des herkömmlichen Bilds und weist die folgenden Komponenten von Microsoft Office 365 ProPlus-installiert zusätzlich zu den Komponenten des Bilds Windows Server 2012 R2 beschrieben:


- Access
- Excel
- Lync
- OneNote
- OneDrive for Business (Beachten Sie, dass der Sync-Agent für die Verwendung mit Azure RemoteApp nicht unterstützt wird)
- Outlook
- PowerPoint
- Word
- Microsoft Office Online

Das Bild enthält darüber hinaus Visio Pro und Project Pro.

Und die folgenden Programme auch:

- SQL Native client
- ODBC-Treiber
- SQL Server Data Mining-client
- MasterDataServices client
- Microsoft Publisher
- PowerQuery
- PowerMap


Vollständige Funktionalität von Office 365 ProPlus apps steht nur für Benutzer, die einen Office 365 ProPlus-Plan verfügen. Weitere Informationen zu Office 365-Abonnementpläne finden Sie unter [Office 365-Service-Pläne](http://technet.microsoft.com/library/office-365-plan-options.aspx). Haben Sie noch Fragen? Schauen Sie sich die [Office 365 + RemoteApp](remoteapp-o365.md) -Informationen. Lesen Sie auch im neuen Artikel, [wie Sie Ihre Office 365-Abonnement mit Azure RemoteApp verwenden](remoteapp-officesubscription.md).

Beachten Sie, dass Sie Office 365 ProPlus, Visio Pro und Project Pro separat lizenzieren müssen – sie jeweils über eigene Lizenz verfügen.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (nur Testversion)
Während des Testzeitraums kostenlosen können Sie den Dienst, mit dem Office 2013-Bild testen.

Diese Abbildung ist eine Erweiterung des Bilds herkömmlichen und weist die folgenden Komponenten von Microsoft Office 2013 Professional Plus installiert zusätzlich zu den Komponenten des Bilds Windows Server 2012 R2 beschrieben:


- Access
- Excel
- Lync
- OneNote
- OneDrive for Business (Beachten Sie, dass der Sync-Agent für die Verwendung mit Azure RemoteApp nicht unterstützt wird)
- Outlook
- PowerPoint
- Project
- Visio
- Word
- Microsoft Office Online

> [AZURE.IMPORTANT]**Rechtliche Hinweise:** Diese Abbildung enthält keiner Microsoft Office-Lizenz und *kann nicht für die Herstellung verwendet werden*. Die Office 2013 Professional Plus Bild ist nur für Testversion vorgesehen. Wenn Sie Office-apps in Azure RemoteApp für Herstellung verwenden möchten, müssen Sie das Office 365 ProPlus Bild verwenden. Informationen zur Lizenzierung von Office finden Sie unter [Verwenden von Office 365 mit Azure RemoteApp](remoteapp-o365.md)
