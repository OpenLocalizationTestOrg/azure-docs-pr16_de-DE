
<properties
    pageTitle="Schützen von apps und Ressourcen Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie Sie apps und Ressourcen Azure RemoteApp Sperren"
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



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Schützen von apps und Ressourcen Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp bietet Benutzern Zugriff auf zentral verwaltete Windows-apps zu dem können Sie steuern, welche Inhalte der Benutzer können nicht an.  Dies ist besonders hilfreich, wenn der Benutzer eine Verbindung aus einem nicht verwalteten Gerät (wie ihre persönlichen Macbook herstellt) und Steuern des Benutzerzugriffs oder auftreten soll.

Beispielsweise können Wenn Active Directory für die Benutzerauthentifizierung verwenden, und Sie, um zu verhindern, dass Ihre Benutzer kopieren von Daten aus einer app möchten, Sie eine Remote Desktop-Gruppenrichtlinien für Benutzer blockieren konfigurieren Kopieren von Daten.

Ein weiteres Beispiel ist, wenn Sie Zugriff auf das Internet zu einer bestimmten app in Ihrer Websitesammlung blockieren möchten. Sie können eine Windows-Firewall-Regel erstellen, die die Access blockiert werden, wenn Sie das Bild für Ihre Websitesammlung erstellen.

## <a name="implementation-options"></a>Implementierungsoptionen

  Hier sind die wichtigsten Implementierungsoptionen, einzeln oder gemeinsam verwendet werden können, je nach Bedarf aus:

1.  Wenn Ihre Sammlung RemoteApp Domäne verbunden ist, können Sie alle [Gruppenrichtlinien](https://technet.microsoft.com/library/cc725828.aspx) (mit Ausnahme der Leerlauf und trennen Timeout Richtlinien beschrieben [hier](../azure-subscription-service-limits.md)) erzwingen.
2.  Alternativ zum Gruppenrichtlinien (wenn der Websitesammlung nicht-Domäne hinzugefügt oder verfügen Sie nicht über die entsprechenden Berechtigungen in Active Directory) können Sie in Ihrer Vorlagenbild [Lokale Richtlinien](https://technet.microsoft.com/library/cc775702.aspx) konfigurieren.  Beachten Sie, dass dieser Gruppe Lokale Richtlinien Trumpf Richtlinien, wenn ein Konflikt vorliegt.
3.  Einige OS/app-Einstellungen können nicht über die Richtlinie konfiguriert werden, aber über Registrierungsschlüssel verwenden das [Programm "RegEdit"](./remoteapp-hybridtrouble.md) beim Konfigurieren Ihrer Vorlage Bilds werden können.
4.  [Windows-Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) können Sie steuern den Netzwerkzugriff zu und vom Computer aus, wenn die app ausgeführt wird. Denken Sie aber daran, dass Sie die URLs und hier definierten Ports blockieren nicht.
5.  Sie können [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) zum Steuerelement die Applikationen und Dateien Benutzer ausgeführt werden können. Beispielsweise können Benutzer ermitteln Ausführen von Applications, dass Sie nicht veröffentlicht werden, aber, die zur Verfügung, in dem Bild, das Sie zum Erstellen der Websitesammlung verwendet werden – AppLocker kann diese Sperren.

## <a name="detailed-information"></a>Ausführliche Informationen

- Die folgenden Richtlinien für RDS können besonders hilfreich sein:
    - [Gerät und Ressourcen Umleitung](https://technet.microsoft.com/library/ee791794.aspx)
    - [Drucker Umleitung](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profile](https://technet.microsoft.com/library/ee791865.aspx).
- Beachten Sie, dass konfigurieren Umleitung über die RemoteApp PowerShell Modul (als Sicht- [hier](./remoteapp-redirection.md)) auf dem Clientcomputer basiert zu die Richtlinie erzwingen, damit die Sicherheit ist die primäre Zielsetzung Sie werden die Richtlinie über die Vorlage Bild lokale Richtlinie oder über Gruppenrichtlinien erzwingen möchten.
- [Windows Server 2012 R2 Richtlinien](https://technet.microsoft.com/library/hh831791.aspx).
- [Office 2013-Richtlinien](https://technet.microsoft.com/library/cc178969.aspx) (einschließlich [zum Anpassen der Office-Symbolleiste](https://technet.microsoft.com/library/cc179143.aspx)).
