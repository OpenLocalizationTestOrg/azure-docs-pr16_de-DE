
<properties
    pageTitle="Behandeln von Problemen mit RemoteApp Cloud Websitesammlungen - Erstellung | Microsoft Azure"
    description="Informationen Sie zum Behandeln von Problemen mit der RemoteApp Cloud Websitesammlung Erstellung Fehlern"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Behandeln von Problemen mit erstellen RemoteApp Cloud-Sammlungen

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wenn Sie beim Erstellen einer Websitesammlungs Cloud Probleme auftreten, lesen Sie die folgenden Informationen.

## <a name="your-image-is-invalid"></a>Das Bild ist ungültig ##
Wenn eine Meldung wie "GoldImageInvalid" wird angezeigt, wenn Sie zur Azure zur Bereitstellung Ihrer Websitesammlung anstehen, bedeutet Vorlagenbild im [Bild Anforderungen definiert](remoteapp-imagereqs.md)nicht entsprechen. Wechseln Sie Ja, lesen Sie diese [Anforderungen](remoteapp-imagereqs.md), beheben das Bild, und versuchen Sie erneut eine Auflistung erstellen.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Allgemeiner Fehler im Verwaltungsportal Azure angezeigt

    DNS server could not be reached
    ProvisioningTimeout

Cloud-Sammlungen häufig Fail während der Erstellung, da Sie benutzerdefinierte Bilder verwenden.  Wenn Sie finden Sie unter einer der oben genannten Fehler, und Sie ein benutzerdefiniertes Bild verwenden sind, um die Auflistung zu erstellen, überprüfen Sie die folgenden Elemente:

- Stellen Sie sicher, dass das benutzerdefinierte Bild hochgeladenen Bild erfüllt.
- In den meisten Fällen ist das häufige auftretendes Problem, dass das Bild nicht ordnungsgemäß Syspreped war.  
- Überprüfen Sie das Bild in Hyper-V starten kann, oder versuchen, eine IAAS VM direkt in Ihrem Azure-Abonnement verwenden das Bild zu erstellen. Wenn der virtuellen Computer nicht starten und nicht gestartet werden kann, gibt dies normalerweise, dass das benutzerdefinierte Bild nicht ordnungsgemäß vorbereitet wurde.  Überprüfen Sie, ob das benutzerdefinierte Bild erstellt wurde, die wie folgt zum Erstellen eines benutzerdefinierten Vorlage Bilds für RemoteApp

Wenn Sie eines der Bilder Microsoft Lieferumfang Ihres Abonnements verwenden, versuchen Sie, die Sammlung erneut zu erstellen. Wenn das Problem weiterhin besteht dann wenden Sie sich an Microsoft Support.

    PlatformImageTrialModeOnly

Wenn dieser Fehler sehen kann dies in der Regel, die Sie in ein kostenpflichtiges Konto aktualisiert wurde, aber Sie versuchen, verwenden Sie ein Bild von Microsoft bereitgestellt, die nur während der Modus zum Testen des Diensts gültig ist. In diesem Fall versuchen Sie, Ihre Cloud-Sammlung erneut erstellen, aber Achten Sie darauf, um das richtige Bild anzugeben.
