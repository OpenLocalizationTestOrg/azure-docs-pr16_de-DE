<properties
    pageTitle="Veröffentlichen eine app in Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Veröffentlichen von Applications und Ressourcen in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>So veröffentlichen Sie eine app in RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Nachdem Sie Ihre Sammlung RemoteApp erstellt haben, müssen Sie veröffentlichen die apps oder Ressourcen, die Sie für Ihre Benutzer zur Verfügung stellen möchten. Die Vorlage Bilder in Ihrem Abonnement inbegriffen nur ein paar apps standardmäßig - zum Freigeben von anderen apps veröffentlicht haben, müssen Sie diese veröffentlichen.

> [AZURE.NOTE] Benötigen Sie eine app aktualisieren? Sie müssen [das Bild](remoteapp-update.md) aktualisieren zuerst.

Klicken Sie auf der Registerkarte **Veröffentlichen** im Portal auf **Veröffentlichen**. Sie können Hinzufügen einer app aus dem **Startmenü des Bilds Ihrer Vorlage** , oder geben Sie den Pfad zu der Stelle, an der die app, klicken Sie auf das Vorlagenbild installiert wurde. Wenn Sie über **das Startmenü** hinzufügen, wählen Sie die app aus der Liste zu veröffentlichen. Wenn Sie den Pfad zu der app bereitstellen auch, geben Sie einen Namen für die app und den Pfad zu der app. Verwenden von Variablen in den Pfad – beispielsweise "Systemlaufwerk %" statt "c:\".

> [AZURE.NOTE] Wenn Sie Ihre app aus dem Menü **Start** hinzufügen möchten, müssen Sie haben *diese app hinzugefügt der * *Starten* * Menü auf dem Vorlagenbild.* Klicken Sie andernfalls RemoteApp werden nur feststellen, welche *ist* im Menü **Start** , und Sie werden verwechselt werden. 

>Um sicherzustellen, dass Ihre app in **das Startmenü** ist, setzen Sie eine Verknüpfungsdatei - **.lnk** - innerhalb der %systemdrive%\ProgramData\Microsoft\Windows\Start im Ordner aus.

> Wenn Sie vergessen haben, die app in **das Startmenü** hinzufügen, wenn Sie die Vorlage erstellt haben, wählen Sie den Pfad zu der app hinzufügen. (Oder das Vorlagenbild neu zu erstellen, aber dies ist ganz etwas mehr Arbeit.)


 