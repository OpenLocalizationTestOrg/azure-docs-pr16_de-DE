<properties
    pageTitle="Erstellen Sie ein Bild Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie mehr über die Optionen zum Erstellen von Bildern für Azure RemoteApp zur Verfügung."
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



# <a name="create-an-azure-remoteapp-image"></a>Erstellen Sie ein Bild Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp verwendet Bilder, um die apps zu halten, die Sie für Ihre Benutzer freizugeben. (Wir machen Sie das Bild und ihre Verwendung zum Erstellen von virtuellen Computern -, die welchen Benutzerzugriff ist, wenn sie nicht bei Azure RemoteApp anmelden). Zum Erstellen einer Azure RemoteApp-Auflistung mit Ihrer Wahl von Applications, sei es Cloud oder Hybrid, zunächst erstellen Sie ein Bild mit diesen Anwendungen installiert. Erstellen Sie dann eine Auflistung, die diesem Bild verwendet, Zuweisen von Benutzern zu der Websitesammlung und Veröffentlichen von apps für diesen Benutzer.

Sie haben mehrere Optionen zum Erstellen oder Verwenden von Bildern. Die grundlegende [Anforderung](remoteapp-imagereqs.md) für ein Bild ist, dass sie Windows Server 2012 R2 ausführen und die Rolle des Remote Desktop Session Host (RDSH) installiert haben. Wie erhalten, dass ist, in dem Elemente interessant zu werden.

Wenn es zu Bildern geht, stehen Ihnen die folgenden Optionen:

- Sie können importieren und einem [Bild auf Grundlage einer Azure-virtuellen Computern](remoteapp-image-on-azurevm.md)verwenden. Dies ist sinnvoll, für die Branchen apps, die benutzerdefinierte Einstellungen erfordern. Sie können das Bild für die app entwickelt anpassen.
- Sie können das [Erstellen und ein benutzerdefiniertes Bild hochladen](remoteapp-create-custom-image.md). Dies ist sinnvoll, wenn Sie bereits ein Bild haben, die Sie für die Bereitstellung Ihrer lokalen Remote Desktop Services verwenden.
- Sie können eine der [Vorlage Bilder](remoteapp-images.md) in Ihrem Abonnement RemoteApp enthalten. Diese Bilder werden erstellt und verwaltet durch das Team RemoteApp und enthalten einige standard-Anwendungen (wie die Office-Suite), die Sie Ihren Benutzern zur Verfügung stellen können. Beachten Sie, dass nur das Office 365 Pro Plus Bild in der Einstellung der Herstellung verwendet werden kann.

Unabhängig davon, wo Sie das Bild erhalten oder wie Sie sie erstellt haben sollten Sie sicherstellen, dass Sie wissen, dass die [app-Anforderungen](remoteapp-appreqs.md) , um sicherzustellen, dass Ihre app gut RemoteApp arbeitet. Klicken Sie dann besteht im nächste Schritt im Erstellen einer Websitesammlungs [Cloud](remoteapp-create-cloud-deployment.md) oder [Hybrid](remoteapp-create-hybrid-deployment.md) .
