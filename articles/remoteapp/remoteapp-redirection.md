<properties
    pageTitle="Verwenden von Umleitung in Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren und verwenden Sie die Umleitung in RemoteApp"
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

# <a name="using-redirection-in-azure-remoteapp"></a>Verwenden von Umleitung in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Umleitung kann Benutzer mit remote-apps mit ihren lokalen Computer, Smartphone oder Tablet verbundenen Geräte mit interagieren. Wenn Sie Skype über Azure RemoteApp angegeben haben, benötigt der Benutzer beispielsweise die Kamera ihres PCs für die Arbeit mit Skype installiert. Dies gilt auch für Drucker, Lautsprecher, Monitore und einen USB-Bereich verbundenen Peripheriegeräte.

RemoteApp nutzt die Bereitstellung der Umleitung (Remotedesktopprotokoll) und RemoteFX.

## <a name="what-redirection-is-enabled-by-default"></a>Welche Umleitung ist standardmäßig aktiviert?
Wenn Sie RemoteApp verwenden, sind die folgenden Umleitung standardmäßig aktiviert. Die Informationen in Klammern angezeigt, die RDP-Einstellung.

- Automatische Wiedergabe von Sounds auf dem lokalen Computer (**Wiedergabe auf diesem Computer**). (Audiomode:i:0)
- Erfassen von Audio aus dem lokalen Computer, und senden Sie mit dem Remotecomputer (**Datensatz von diesem Computer**). (Audiocapturemode:i:1)
- Drucken Sie auf lokale Drucker (Redirectprinters:i:1)
- Com-Ports (Redirectcomports:i:1)
- Smartcard-Gerät (Redirectsmartcards:i:1)
- Zwischenablage (Möglichkeit zum Kopieren und Einfügen) (Redirectclipboard:i:1)
- Deaktivieren Sie Typ Schriftart Glätten (Schriftart Glätten zulassen: I:1)
- Leiten Sie alle unterstützten Plug Play-Geräte. (Devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Welche anderen Umleitung verfügbar ist?
Zwei Umleitungsoptionen sind standardmäßig deaktiviert:

- Laufwerk Umleitung (Laufwerk Zuordnung): Konvertierung in Ihrem lokalen Computer Laufwerke zugeordnete Laufwerke in der remote-Sitzung. Auf diese Weise können Sie speichern oder Öffnen von Dateien aus Ihren lokalen Laufwerken zu reduzieren, während Sie in der remote-Sitzung arbeiten.
- USB-Umleitung: können Sie das USB-Geräte an Ihrem lokalen Computer innerhalb der remote-Sitzung verwenden.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Ändern der Einstellungen für die Umleitung in RemoteApp
Sie können die Umleitung des Audiogeräts für eine Websitesammlung mithilfe der PowerShell Microsoft Azure SDK ändern. Nachdem Sie die neue PowerShell und SDK installiert haben, zuerst konfigurieren Sie, um Ihr Abonnement zu verwalten, wie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)beschrieben.

Verwenden Sie dann einen Befehl wie den folgenden, um die benutzerdefinierten RDP Eigenschaften festlegen:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Beachten Sie, dass die *' n* als ein Trennzeichen zwischen den einzelnen Eigenschaften verwendet wird.)

Führen Sie das folgende Cmdlet aus, um eine Liste der konfigurierten welche benutzerdefinierten Eigenschaften, die RDP zu erhalten. Beachten Sie, dass nur die benutzerdefinierten Eigenschaften, die als Ergebnis Ergebnisse und nicht die Standardeigenschaften dargestellt werden:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Wenn Sie benutzerdefinierte Eigenschaften festlegen, müssen Sie alle benutzerdefinierte Eigenschaften, die jedes Mal angeben. Andernfalls wird die Einstellung auf deaktiviert zurückgesetzt.   

### <a name="common-examples"></a>Allgemeine Beispiele
Verwenden Sie das folgende Cmdlet Laufwerk Umleitung zu aktivieren:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Dieses Cmdlet verwenden, aktivieren Sie sowohl USB-Laufwerk Umleitung:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Verwenden Sie dieses Cmdlet So deaktivieren Sie die gemeinsame Nutzung der Zwischenablage an:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Achten Sie darauf, melden Sie sich alle Benutzer in der Sammlung vollständig (und nicht nur trennen sie), bevor Sie die Änderung testen. Um sicherzustellen, dass Benutzer vollständig deaktivieren angemeldet sind, wechseln Sie zur Registerkarte **Sitzungen** in der Auflistung der Azure-Portal, und melden Sie sich ab allen Benutzern, die getrennt oder angemeldet sind. Manchmal kann es einige Sekunden für die lokale Laufwerke anzeigen in Explorer innerhalb der Sitzung dauern.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Ändern der USB-Umleitung Einstellungen auf Ihrem Windows-client

Wenn Sie ein USB-Umleitung auf einem Computer verwenden, die mit RemoteApp verbindet möchten, gibt es 2 Aktionen erfolgen. 1: der Administrator muss USB-Umleitung auf Ebene der Websitesammlung aktivieren, indem Sie mithilfe der PowerShell Azure. 2 - auf jedem Gerät, in dem Sie USB-Umleitung verwenden möchten, müssen Sie eine Gruppenrichtlinie zu aktivieren, die dies zulässt. Diesen Schritt müssen für jeden Benutzer erfolgen, die möchte USB-Umleitung verwenden.

> [AZURE.NOTE] USB-Umleitung mit Azure RemoteApp wird nur für Windows-Computer unterstützt.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Aktivieren Sie die USB-Umleitung für die Websitesammlung RemoteApp
Verwenden Sie das folgende Cmdlet USB-Umleitung auf Ebene der Websitesammlung aktivieren:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Aktivieren Sie die USB-Umleitung für den Clientcomputer

So konfigurieren Sie ein USB-Umleitung Einstellungen auf Ihrem computer

1. Öffnen Sie den Editor für lokale Gruppenrichtlinien (GPEDIT. MSC). (Gpedit.msc über eine Befehlszeile ausführen.)
2. **Computer Benutzerkonfiguration\Richtlinien\Administrative Vorlagen\Windows-Components\Remote Komponenten\Remotedesktopdienste\Remotedesktop Desktop Verbindung Client\RemoteFX USB-Gerät Umleitung**zu öffnen.
3. Doppelklicken Sie auf **Zulassen RDP Umleitung von anderen unterstützten RemoteFX USB-Geräten aus diesem Computer**.
4. Wählen Sie **aktiviert**aus, und wählen Sie dann auf **Administratoren und Benutzer in die RemoteFX USB-Umleitung Zugriffsrechte**.
5. Öffnen Sie ein Eingabeaufforderungsfenster mit Administratorberechtigungen, und führen Sie den folgenden Befehl aus:

        gpupdate /force
6. Starten Sie den Computer neu.

Sie können auch das Gruppenrichtlinien-Tool zum Erstellen und Anwenden der Richtlinie USB-Umleitung für alle Computer in Ihrer Domäne verwenden:

1. Melden Sie sich als Domänenadministrator in der Domänencontroller.
2. Öffnen Sie die Gruppenrichtlinien-Verwaltungskonsole. (Klicken Sie auf **Start > Verwaltung > Gruppieren Richtlinienmanagement**.)
3. Navigieren Sie zu der Domäne oder Organisationseinheit, für die Sie die Richtlinie erstellen möchten.
4. Mit der rechten Maustaste **Standardrichtlinie**, und klicken Sie dann auf **Bearbeiten**.
5. **Computer Benutzerkonfiguration\Richtlinien\Administrative Vorlagen\Windows-Components\Remote Komponenten\Remotedesktopdienste\Remotedesktop Desktop Verbindung Client\RemoteFX USB-Gerät Umleitung**zu öffnen.
6. Doppelklicken Sie auf **Zulassen RDP Umleitung von anderen unterstützten RemoteFX USB-Geräten aus diesem Computer**.
7. Wählen Sie **aktiviert**aus, und wählen Sie dann auf **Administratoren und Benutzer in die RemoteFX USB-Umleitung Zugriffsrechte**.
8. Klicken Sie auf **OK**.  
