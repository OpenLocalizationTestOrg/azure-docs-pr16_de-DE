<properties 
   pageTitle="Behandeln von Problemen bei der Bereitstellung von StorSimple | Microsoft Azure"
   description="Beschreibt, wie diagnostizieren und Beheben von Fehlern, die beim Bereitstellen von StorSimple auftreten."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Behandeln von Problemen bei der Bereitstellung von StorSimple-Gerät

## <a name="overview"></a>(Übersicht)

Dieser Artikel enthält nützliche Leitfaden für die Problembehandlung für die Bereitstellung von Microsoft Azure StorSimple. Es werden häufige Probleme, zu den möglichen Ursachen und Schritte empfohlen, mit denen Sie Probleme beheben, die auftreten können, wenn Sie StorSimple konfigurieren. Diese Informationen gelten für das StorSimple lokalen physischen Gerät und das StorSimple virtuelle Gerät.

> [AZURE.NOTE] Gerät Konfigurations-bezogene Probleme, die Sie möglicherweise stoßen können auftreten, wenn Sie das Gerät zum ersten Mal bereitstellen, oder sie später auftreten können, wenn das Gerät funktionsfähig ist. Dieser Artikel befasst sich zur Behebung von Problemen bei der erstmaligen Bereitstellung. Wechseln Sie zum Behandeln einer Betrieb Gerät zum [Behandeln von Betrieb Gerät Probleme](storsimple-troubleshoot-operational-device.md).

Dieser Artikel beschreibt die Tools zur Behandlung dieses Problems StorSimple Bereitstellungen auch und bietet ein schrittweises Beispiel für die Problembehandlung.

## <a name="first-time-deployment-issues"></a>Probleme bei der erstmaligen Bereitstellung

Wenn Sie ein Problem auftreten, wenn Sie Ihr Gerät zum ersten Mal bereitstellen, sollten beachten Sie Folgendes:

- Wenn Sie ein Gerät physisches Probleme haben, stellen Sie sicher, dass die Hardware installiert und konfiguriert, wie in [Ihrem Gerät StorSimple 8100 installieren Sie](storsimple-8100-hardware-installation.md) oder [Ihr Gerät StorSimple 8600](storsimple-8600-hardware-installation.md)beschrieben ist wurde.
- Aktivieren Sie erforderliche Komponenten für die Bereitstellung. Stellen Sie sicher, dass Sie alle Informationen in der [Checkliste für die Bereitstellung](storsimple-deployment-walkthrough.md#deployment-configuration-checklist)beschriebenen haben.
- Überprüfen Sie die Versionsinformationen StorSimple, um festzustellen, ob das Problem beschrieben wird. Problemumgehung für bekannte Installationsprobleme sind die Hinweise enthalten. 

Während der Bereitstellung Gerät Probleme der am häufigsten verwendeten, die von Benutzern Smiley auftreten, wenn sie den Setup-Assistenten ausführen, und wenn sie das Gerät über Windows PowerShell für StorSimple registrieren. (Sie mithilfe von Windows PowerShell für StorSimple registriert und Konfigurieren von Ihrem Gerät StorSimple. Weitere Informationen für die Registrierung Gerät finden Sie unter [Schritt 3: konfigurieren, und melden Sie sich Ihr Gerät über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

In den folgenden Abschnitten helfen Ihnen die Probleme zu beheben, die auftreten, wenn Sie das Gerät StorSimple zum ersten Mal konfigurieren.

## <a name="first-time-setup-wizard-process"></a>Erstmalige Setupprozesses-Assistenten

Die folgenden Schritte Zusammenfassen des Setupprozesses-Assistenten. Ausführliche Setupinformationen finden Sie unter [Bereitstellen von Ihrem lokalen StorSimple Gerät](storsimple-deployment-walkthrough.md).

1. Führen Sie das Cmdlet [HcsSetupWizard aufrufen](https://technet.microsoft.com/library/dn688135.aspx) , um den Setup-Assistenten zu starten, der Sie durch die restlichen Schritte führt. 
2. Konfigurieren des Netzwerks: der Setup-Assistenten können Sie die Einstellungen für die Daten 0 Netzwerk-Schnittstelle im Netzwerk auf Ihrem Gerät StorSimple konfigurieren. Diese Einstellungen umfassen Folgendes:
  - Virtuelle IP-Adresse (VIP), Subnetz-Maske und Gateway – das Cmdlet " [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) " im Hintergrund ausgeführt wird. Es wird die IP-Adresse, Subnetz-Maske und Gateway für die Schnittstelle 0 Netzwerk auf Ihrem Gerät StorSimple konfiguriert.
  - Primärer DNS-Server – das Cmdlet " [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) " wird im Hintergrund ausgeführt. Die DNS-Einstellungen für Ihre Lösung StorSimple konfiguriert.
  - NTP-Server – das Cmdlet " [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) " wird im Hintergrund ausgeführt. Es wird die NTP servereinstellungen für Ihre Lösung StorSimple konfiguriert.
  - Optionale Webproxy – das Cmdlet " [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) " wird im Hintergrund ausgeführt. Legt fest, und ermöglicht die Web-Proxy-Konfiguration für Ihre StorSimple-Lösung.
3. Richten Sie die Kennwörter: im nächsten Schritt wird Gerät Unternehmensadministrator "und" StorSimple Snapshot-Manager Kennwörter einrichten. Wenn Sie das Update 1 ausgeführt werden, klicken Sie dann nicht müssen Sie das Kennwort StorSimple Snapshot-Manager einrichten.
  - Das Gerät Administratorkennwort dient zum Anmelden bei Ihrem Geräts. Das Kennwort für das Standard-Gerät ist **Kennwort1**.
  - Das Kennwort StorSimple Snapshot-Manager ist erforderlich, wenn Sie ein Gerät verwenden StorSimple Snapshot-Manager konfigurieren. Sie müssen zuerst das Kennwort im Setup-Assistenten festlegen, und klicken Sie dann Sie festlegen können, und ändern Sie ihn von der StorSimple Manager-Dienst. Dieses Kennwort authentifiziert das Gerät mit StorSimple Snapshot-Manager.
 
    > [AZURE.IMPORTANT] Kennwörter werden vor der Registrierung erfasst, aber erst Sie das Gerät erfolgreich registrieren. Ist ein Fehler beim Anwenden eines Kennworts, werden Sie aufgefordert, das Kennwort erneut eingeben, bis die erforderlichen Kennwörter (die Komplexität erfüllen) erfasst wurden.

4. Das Gerät zu registrieren: der letzte Schritt ist das Gerät bei der Ausführung im Microsoft Azure Verwaltungsdienst für StorSimple registrieren. Die Registrierung erfordert, dass Sie zum [Abrufen der Dienst Registrierungsschlüssel](storsimple-manage-service.md#get-the-service-registration-key) vom klassischen Azure-Portal, und geben sie im Setup-Assistenten. Nachdem Sie das Gerät erfolgreich registriert ist, wird Sie ein Verschlüsselungsschlüssels für Dienst Daten bereitgestellt. Achten Sie darauf, dass diese Verschlüsselungsschlüssels an einem sicheren Ort belassen, da es werden alle nachfolgenden Geräte mit dem Dienst erfassen müssen.

## <a name="common-errors-during-device-deployment"></a>Häufige Fehler bei der Bereitstellung von Gerät

In den folgenden Tabellen Liste die häufigen, wann auftreten können Sie:

- Konfigurieren Sie die erforderlichen Netzwerk-Einstellungen.
- Konfigurieren Sie optionale Web-Proxyeinstellungen.
- Richten Sie das Geräteadministrator und Kennwörter StorSimple Snapshot-Manager. 
- Das Gerät zu registrieren. 

## <a name="errors-during-the-required-network-settings"></a>Fehler, während die erforderlichen Netzwerk-Einstellungen

| Nein.| Fehlermeldung | Zu den möglichen Ursachen | Empfohlene Aktion |
| ---| ------------- | --------------- | ------------------ |
| 1  | Rufen Sie HcsSetupWizard: Dieser Befehl kann nur auf dem aktiven Controller ausgeführt werden. | Konfiguration wurde für den passiven Controller ausgeführt wird.| Führen Sie diesen Befehl aus dem aktiven Controller aus. Weitere Informationen finden Sie unter [Identifizieren einer aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Rufen Sie HcsSetupWizard: Gerät nicht bereit. | Es gibt Probleme mit der Netzwerkkonnektivität auf Daten 0 ein.| Überprüfen Sie die physische Netzwerkkonnektivität auf Daten 0 ein.|
| 3 | Aufrufen HcsSetupWizard: Es gibt eine IP-Adressenkonflikt mit einem anderen System im Netzwerk (Ausnahme: 0x80070263). | Die IP-Adresse für Daten 0 angegeben wurde bereits von einem anderen System. | Stellen Sie eine neue IP-Adresse, die nicht verwendet wird.|
| 4 | Rufen Sie HcsSetupWizard: Fehler bei eine Clusterressource (Ausnahme: 0x800713AE). | Doppelte VIP. Die angegebene IP-Adresse wird bereits verwendet.| Stellen Sie eine neue IP-Adresse, die nicht verwendet wird.|
| 5 | Rufen Sie HcsSetupWizard: Ungültige IPv4-Adresse. | Die IP-Adresse wird in einem falschen Format bereitgestellt.| Überprüfen Sie das Format, und geben Sie Ihre IP-Adresse erneut. Weitere Informationen finden Sie unter [Ipv4 adressieren][1]. |
| 6 | Rufen Sie HcsSetupWizard: Ungültige IPv6-Adresse. | Die IP-Adresse wird in einem falschen Format bereitgestellt.| Überprüfen Sie das Format, und geben Sie Ihre IP-Adresse erneut. Weitere Informationen finden Sie unter [Ipv6 adressieren][2].|
| 7 | Aufrufen HcsSetupWizard: Es werden keine weitere Endpunkte aus der Endpunkt-Zuordnung zur Verfügung. (Ausnahme: 0x800706D9) | Die Clusterfunktionalität funktioniert nicht. | [Microsoft-Support wenden](storsimple-contact-microsoft-support.md) für den nächsten Schritten fort.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Während die Proxyeinstellungen optionale Web-Fehler

| Nein.| Fehlermeldung | Zu den möglichen Ursachen | Empfohlene Aktion |
| ---| ------------- | --------------- | ------------------ |
| 1  | Rufen Sie HcsSetupWizard: Ungültiger Parameter (Ausnahme: 0 x 80070057) | Einer der Parameter für die Proxyeinstellungen bereitgestellt ist ungültig.| Der URI wird nicht im richtigen Format bereitgestellt. Verwenden Sie das folgende Format: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Rufen Sie HcsSetupWizard: RPC-Server nicht verfügbar (Ausnahme: 0x800706ba) | Die Ursache ist eine der folgenden Aktionen aus:<ol><li>Der Cluster ist nicht nach oben.</li><li>Der passive Controller kann nicht mit dem aktiven Controller kommunizieren, und der Befehl vom passiven Controller ausgeführt wird.</li></ol> | Je nach der Ursache:<ol><li>[Microsoft-Support wenden](storsimple-contact-microsoft-support.md) , um sicherzustellen, dass der Cluster aktiv ist.</li><li>Führen Sie den Befehl aus dem aktiven Controller aus. Wenn Sie den Befehl aus dem passiven Controller ausführen möchten, müssen Sie sicherstellen, dass der passive Controller mit dem aktiven Controller kommunizieren kann. Sie müssen den [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , wenn diese Konnektivität fehlerhaft ist.</li></ol> |
| 3 | Aufrufen HcsSetupWizard: Remoteprozeduraufruf fehlgeschlagen ist (Ausnahme: 0x800706be) | Cluster ist nach unten. | [Microsoft-Support wenden](storsimple-contact-microsoft-support.md) , um sicherzustellen, dass der Cluster aktiv ist.|
| 4 | Rufen Sie HcsSetupWizard: Cluster-Ressource wurde nicht gefunden (Ausnahme: 0x8007138f) | Die Ressource wurde nicht gefunden. Dies kann geschehen, wenn es sich bei die Installation nicht korrekt war. | Möglicherweise müssen Sie das Gerät auf die Standardeinstellungen Factory zurücksetzen. [Microsoft-Support wenden](storsimple-contact-microsoft-support.md) , eine Clusterressource erstellen.|
| 5 | Aufrufen HcsSetupWizard: Cluster-Ressource nicht online (Ausnahme: 0x8007138c)| Cluster-Ressourcen stehen nicht online. | [Microsoft-Support wenden](storsimple-contact-microsoft-support.md) für den nächsten Schritten fort.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Fehler im Zusammenhang mit dem Gerät Unternehmensadministrator "und" StorSimple Snapshot-Manager Kennwörter

Das standardmäßige Gerät Administratorkennwort ist **Kennwort1**. Dieses Kennwort läuft ab nach der ersten Anmeldung; Daher müssen Sie den Setup-Assistenten verwenden, um ihn zu ändern. Wenn Sie das Gerät zum ersten Mal registrieren, müssen Sie ein neues Gerät Administratorkennwort angeben. 

Wenn Sie die StorSimple Snapshot-Manager-Software auf dem Host von Windows Server ausgeführt verwenden, um das Gerät zu verwalten, müssen Sie auch ein Kennwort StorSimple Snapshot-Manager während der erstmaligen Registrierung angeben. 

Stellen Sie sicher, dass die Kennwörter der erfüllen:

- Ihr Gerät Administratorkennwort sollte 8 bis 15 Zeichen lang sein.
- Ihr Kennwort StorSimple Snapshot-Manager sollte 14 oder 15 Zeichen lang sein.
- Kennwörter sollte der folgenden 4 Zeichentypen 3 enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. 
- Ihr Kennwort darf nicht die letzten 24 Kennwörter identisch sein.

Lassen Sie darüber hinaus, denken Sie daran, die Kennwörter ablaufen jedes Jahr und können geändert werden, nachdem Sie das Gerät erfolgreich registrieren. Wenn die Registrierung aus irgendeinem Grund fehlschlägt, werden die Kennwörter nicht geändert werden. Weitere Informationen zum Geräteadministrator und Kennwörter StorSimple Snapshot-Manager wechseln Sie zu [Verwenden der Dienst StorSimple-Manager die StorSimple Kennwörter ändern](storsimple-change-passwords.md).

Sie können eine oder mehrere der folgenden Fehler beim Einrichten der Geräteadministrator und StorSimple Snapshot-Manager Kennwörter auftreten.

| Nein.| Fehlermeldung | Empfohlene Aktion |
| ---| ------------- | ------------------ | 
| 1  | Das Kennwort überschreitet die maximale Länge. |Verwenden Sie ein Kennwort, das diese Anforderungen erfüllt:<ul><li>Ihr Gerät Administratorkennwort muss 8 bis 15 Zeichen lang sein.</li><li>Ihr Kennwort StorSimple Snapshot-Manager muss 14 oder 15 Zeichen lang sein.</li></ul> | 
| 2 | Das Kennwort erfüllt nicht die erforderliche Länge. | Verwenden Sie ein Kennwort, das diese Anforderungen erfüllt:<ul><li>Ihr Gerät Administratorkennwort muss 8 bis 15 Zeichen lang sein.</li><li>Ihr Kennwort StorSimple Snapshot-Manager muss 14 oder 15 Zeichen lang sein.</lu></ul> |
| 3 | Das Kennwort muss Kleinbuchstaben enthalten. | Kennwörter müssen 3 der folgenden 4 Zeichentypen enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. Stellen Sie sicher, dass Ihr Kennwort diese Anforderungen erfüllt. |
| 4 | Das Kennwort darf numerische Zeichen enthalten. | Kennwörter müssen 3 der folgenden 4 Zeichentypen enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. Stellen Sie sicher, dass Ihr Kennwort diese Anforderungen erfüllt. |
| 5 | Das Kennwort muss Sonderzeichen enthalten. | Kennwörter müssen 3 der folgenden 4 Zeichentypen enthalten: Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen. Stellen Sie sicher, dass Ihr Kennwort diese Anforderungen erfüllt. |
| 6 | Das Kennwort muss 3 der folgenden 4 Zeichentypen enthalten: Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen. | Ihr Kennwort enthält nicht die erforderlichen Typen von Zeichen. Stellen Sie sicher, dass Ihr Kennwort diese Anforderungen erfüllt. |
| 7 | Parameter entspricht Bestätigung nicht. | Vergewissern Sie sich, dass alle erfüllt, Ihr Kennwort ein, dass Sie korrekt eingegeben haben. |
| 8 | Ihr Kennwort darf nicht die Standardeinstellung übereinstimmen. | Das standardmäßige Kennwort lautet *Kennwort1*. Sie müssen dieses Kennwort ändern, nachdem Sie sich zum ersten Mal anmelden. |
| 9 | Das eingegebene Kennwort entspricht nicht Kennwort für das Gerät. Geben Sie das Kennwort erneut ein. | Aktivieren Sie das Kennwort ein, und geben Sie ihn erneut. |

Kennwörter werden gesammelt, bevor das Gerät registriert ist, aber nur nach der erfolgreichen Registrierung angewendet werden. Kennwort Wiederherstellung Workflows erfordert das Gerät registriert werden. 

> [AZURE.IMPORTANT] Im Allgemeinen fällt Versuch, ein Kennwort zuweisen, versucht dann die Software wiederholt, das Kennwort zu erfassen, bis es erfolgreich ist. Gelegentlich kann das Kennwort angewendet werden. In diesem Fall können Sie das Gerät zu registrieren und Vorgang fortsetzen, jedoch die Kennwörter nicht geändert werden. Sie erhalten keine Angabe darüber, welche Kennwort nicht geändert wurde – das Gerät Administratorkennwort oder das Kennwort StorSimple Snapshot-Manager. Wenn diese Situation auftritt, empfehlen wir, dass Sie beide Kennwörter ändern.

Sie können die Kennwörter im klassischen Azure-Portal über den Dienst StorSimple Manager zurücksetzen. Weitere Informationen finden Sie auf: 

- [Ändern Sie das Kennwort des Administrators](storsimple-change-passwords.md#change-the-device-administrator-password).
- [Ändern des Kennworts StorSimple Snapshot-Manager](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Fehler bei der Registrierung Gerät

Verwenden Sie den StorSimple-Manager-Dienst in Microsoft Azure ausgeführt, um das Gerät zu registrieren. Sie können eine oder mehrere der folgenden Probleme während der Registrierung Gerät auftreten.

| Nein.| Fehlermeldung | Zu den möglichen Ursachen | Empfohlene Aktion |
| ---| ------------- | --------------- | ------------------ |
| 1  | : 350027 Fehler das Gerät mit dem StorSimple-Manager zu registrieren. |  | Warten Sie einige Minuten, und versuchen Sie es dann erneut. Wenn das Problem weiterhin besteht, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md).|
| 2  | Fehler 350013: Ein Fehler ist aufgetreten, in das Gerät zu registrieren. Ursache könnte Registrierungsschlüssel falsche Service hierfür sein. | | Registrieren Sie das Gerät erneut mit den richtigen Service-Registrierungsschlüssel. Weitere Informationen finden Sie unter [Abrufen der Dienst Registrierungsschlüssel.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | 350063 Authentifizierung StorSimple Verwaltungsdienst übergeben aber Registrierung Fehler. Wiederholen Sie den Vorgang nach einiger Zeit ein. | Dieser Fehler gibt an, dass der Register Anrufes zum Dienst ausgefallen Authentifizierung mit ACS abgelaufen ist. Dies kann ein Netzwerkfehler gelegentliche Ergebnis sein. | Wenn das Problem weiterhin besteht, wenden Sie sich an [Microsoft Support](storsimple-contact-microsoft-support.md). |
| 4 | Fehler 350049: Der Dienst konnte nicht während der Registrierung erreicht werden. | Wenn der Dienst aufgerufen wird, wird eine Webausnahme empfangen. In einigen Fällen dies möglicherweise behoben erhalten mit den Vorgang zu einem späteren Zeitpunkt wiederholen. | Überprüfen Sie Ihre IP-Adresse und DNS-Namen, und wiederholen Sie den Vorgang. Wenn das Problem weiterhin besteht, [wenden Sie sich an Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Fehler 350031: Das Gerät wurde bereits registriert. | | Keine Maßnahme erforderlich. |
| 6 | 350016 Gerät Registrierung Fehler. | |Stellen Sie sicher, dass der Registrierungsschlüssel korrekt ist. |
| 7 | Rufen Sie HcsSetupWizard: Fehler bei der Registrierung von Ihrem Geräts; Ursache könnte falsche IP-Adresse oder DNS-Name hierfür sein. Überprüfen Sie Ihre Netzwerkeinstellungen, und versuchen Sie es erneut. Wenn das Problem weiterhin besteht, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md). (Fehler 350050) | Stellen Sie sicher, dass Ihr Gerät mit das externe Netzwerk anpingen kann. Wenn Sie keine Verbindung zu externen Netzwerk verfügen, kann die Registrierung folgender Fehler fehl. Dieser Fehler möglicherweise eine Kombination aus einem oder mehreren der folgenden Aktionen aus:<ul><li>Falsche IP</li><li>Falsche Subnetz</li><li>Falsche gateway</li><li>Falsche DNS-Einstellungen</li></ul> | Informationen Sie zu den Schritten in den [schrittweisen zur Problembehandlung Beispiel](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Aufrufen HcsSetupWizard: Das aktuelle fehlgeschlagen aufgrund ein interner Dienstfehler [0x1FBE2]. Wiederholen Sie den Vorgang nach einer Weile ein. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support. | Dies ist ein allgemeiner Fehler in Dienst oder Agent für alle Benutzer unsichtbaren Fehler ausgelöst. Die häufigste Ursache möglicherweise, dass die ACS-Authentifizierung fehlgeschlagen ist. Eine mögliche Ursache für den Fehler ist, dass es Probleme mit der NTP-Server-Konfiguration liegen, und die Uhrzeit auf dem Gerät nicht ordnungsgemäß festgelegt wurden wird. | Korrigieren Sie die Zeit (falls es Probleme liegen), und wiederholen Sie dann den Registrierung Vorgang. Wenn Sie des Befehls Set-HcsSystem - Zeitzone mithilfe die Zeitzone anpassen, in der Zeitzone (beispielsweise "pazifischen Raum Standard Time") Wort großschreiben.  Wenn dieses Problem weiterhin besteht, Schritte [Kontakt Microsoft Support](storsimple-contact-microsoft-support.md) für weiter. |
| 9 | Warnung: Das Gerät konnte nicht aktiviert werden. Ihr Geräteadministrator und StorSimple Snapshot-Manager Kennwörter wurden nicht geändert. | Wenn die Registrierung fehlschlägt, werden das Gerät Unternehmensadministrator "und" StorSimple Snapshot-Manager Kennwörter nicht geändert. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Tools zur Behandlung dieses Problems StorSimple Bereitstellungen

StorSimple enthält verschiedene Tools, mit denen Sie Ihre Lösung StorSimple zu behandeln. Hierzu gehören:

- Support-Paketen und Geräte von Protokollen 
- Cmdlets, die speziell für die Problembehandlung 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Unterstützung für Pakete und Gerät Protokolle verfügbar zur Behandlung dieses Problems

Support-Paket enthält die relevanten Protokolle, die das Microsoft-Support-Team mit Gerät Problembehandlung helfen können. Windows PowerShell für StorSimple können Sie um eine verschlüsselte Support-Paket zu erzeugen, die Sie dann für Supportmitarbeiter freigeben können.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Die Protokolle oder den Inhalt des Pakets Support anzeigen

1. Mithilfe von Windows PowerShell für StorSimple Support-Paket generieren in beschriebenen [Erstellen und Verwalten von Support-Paket](storsimple-create-manage-support-package.md).

2. Laden Sie das [Skript entschlüsseln](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokal auf dem Clientcomputer.

3. Verwenden Sie dieses [schrittweise](storsimple-create-manage-support-package.md#edit-a-support-package) öffnen und Entschlüsseln Support-Paket ein.

4. Die Protokolle der entschlüsselt Support-Paket werden im Etw/Etvx-Format. Führen Sie die folgenden Schritte aus, um die Dateien in der Windows-Ereignisanzeige anzuzeigen:
  1. Führen Sie den Befehl **Eventvwr ein** , auf Ihrem Windows-Client. Dadurch wird die Ereignisanzeige gestartet.
  2. Im Bereich **Aktionen** klicken Sie auf **Gespeicherte Protokolldatei öffnen** , und zeigen Sie die Protokolldateien im Etvx/Etw-Format (das Supportpaket). Sie können nun die Datei anzeigen. Nachdem Sie die Datei öffnen, können Sie mit der rechten Maustaste, und speichern Sie die Datei als Text.
   
    > [AZURE.IMPORTANT] Das Cmdlet " **Get-WinEvent** " können Sie auch um diese Dateien in Windows PowerShell zu öffnen. Weitere Informationen finden Sie in der Dokumentation der Windows PowerShell-Cmdlet [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) .

5. Wenn die Protokolle in der Ereignisanzeige öffnen, suchen Sie nach der folgenden Protokolle, die Probleme im Zusammenhang mit der Gerätekonfiguration enthalten:

  - Log Hcs_pfconfig/Betrieb
  - Hcs_pfconfig/Config

6. Suchen Sie in den Protokolldateien nach Zeichenfolgen, die im Zusammenhang mit der Cmdlets aufgerufen, indem Sie den Setup-Assistenten. Eine Liste der dieser Cmdlets finden Sie unter [erstmalige Setup-Assistenten Prozess](#first-time-setup-wizard-process) . 

7. Wenn Sie nicht die Ursache des Problems ermitteln sind, können Sie für weitere Schritte [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) . Führen Sie die Schritte in [Erstellen Sie eine Supportanfrage](storsimple-contact-microsoft-support.md#create-a-support-request) , wenn Sie Microsoft-Support, um Unterstützung zu erhalten wenden.

## <a name="cmdlets-available-for-troubleshooting"></a>Cmdlets für die Problembehandlung zur Verfügung.

Verwenden Sie die folgenden Windows PowerShell-Cmdlets zum Ermitteln von Fehlern Connectivity.

- `Get-NetAdapter`: Verwenden Sie dieses Cmdlet, um die Integrität des Netzwerk-Schnittstellen erkennen. 

- `Test-Connection`: Verwenden Sie dieses Cmdlet die Netzwerkkonnektivität innerhalb und außerhalb des Netzwerks überprüfen.

- `Test-HcsmConnection`: Verwenden Sie dieses Cmdlet die Verbindung eines erfolgreich registrierten Geräts überprüft werden.

Wenn Sie auf Ihrem Gerät StorSimple Update 1 ausführen, sind die folgenden diagnostic Cmdlets auch verfügbar.

- `Sync-HcsTime`: Verwenden Sie dieses Cmdlet Gerät Uhrzeit anzeigen und eine Uhrzeit Synchronisierung mit dem Server NTP erzwingen.

- `Enable-HcsPing`und `Disable-HcsPing`: Verwenden dieser Cmdlets für die Hosts im Netzwerk-Schnittstellen auf Ihrem Gerät StorSimple Signal an zulassen. Standardmäßig Antworten die StorSimple Netzwerk-Schnittstellen nicht auf Ping-Abfragen.

- `Trace-HcsRoute`: Verwenden Sie dieses Cmdlet als ein Tool zum Routing verfolgen. Sendet Pakete zu jedem auf dem Weg zu einem Ziel über einen Zeitraum, und klicken Sie dann berechnet Ergebnisse auf Grundlage der Pakete aus jedem Abschnitt zurückgegeben. Da `Trace-HcsRoute` zeigt dem Grad der Pakete verloren gehen bei einem beliebigen angegebenen Router oder Link, können Sie feststellen, welche Router oder Links möglicherweise Netzwerkprobleme verursacht. 

- `Get-HcsRoutingTable`: Verwenden Sie dieses Cmdlet, um die lokale IP-routing-Tabelle anzuzeigen.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Behandeln von Problemen mit mit das Cmdlet "Get-NetAdapter"

Beim Netzwerk-Schnittstellen für ein Gerät erstmalige Bereitstellung konfigurieren, ist der Hardwarestatus nicht verfügbar im Dienst StorSimple Manager Benutzeroberfläche, da das Gerät noch nicht mit dem Dienst registriert ist. Darüber hinaus möglicherweise die Hardware Statusseite nicht immer ordnungsgemäß den Zustand des Geräts, wider insbesondere dann, wenn es liegen Probleme, die Synchronisierung Dienst beeinflussen. In diesen Fällen können Sie mithilfe der `Get-NetAdapter` -Cmdlet zum Bestimmen der Gesundheit und den Status Ihrer Netzwerkschnittstellen.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Um eine Liste aller Netzwerkadapter auf Ihrem Gerät anzuzeigen

1. Starten Sie Windows PowerShell für StorSimple, und geben Sie `Get-NetAdapter`. 

2. Verwenden Sie die Ausgabe der `Get-NetAdapter` Cmdlet und die folgenden Richtlinien, um den Status Ihrer Netzwerkschnittstelle zu verstehen.
  - Wenn die Benutzeroberfläche fehlerfrei und aktiviert ist, wird der Status **IfIndex** als **nach oben**angezeigt.
  - Wenn die Benutzeroberfläche fehlerfrei ist, aber nicht physisch (über ein Netzwerkkabel) verbunden ist, wird die **IfIndex** als **deaktiviert**angezeigt.
  - Wenn die Benutzeroberfläche fehlerfrei aber nicht aktiviert ist, wird der Status **IfIndex** als **NotPresent**angezeigt.
  - Wenn die Benutzeroberfläche nicht vorhanden ist, wird es in dieser Liste nicht angezeigt. Der Dienst StorSimple Manager-Benutzeroberfläche wird immer noch diese Schnittstelle ausgefallen angezeigt.

Weitere Informationen zum Verwenden dieser Cmdlets zu wechseln [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) in der Windows PowerShell-Cmdlet Bezug 

Die folgenden Abschnitte zeigen Beispiele der Ausgabe von der `Get-NetAdapter` Cmdlet. 

 In diesen Beispielen Controller 0 der passiven Controller wurde, und wie folgt konfiguriert wurde:

- Daten 0, Daten 1, Daten 2 und 3 von Daten Netzwerk Schnittstellen auf dem Gerät vorhanden.
- Daten 4 und 5 Daten Netzwerk Benutzeroberflächen-Karten wurden nicht vorhanden; Daher sind nicht in der Ausgabe aufgeführt.
- Daten 0 aktiviert wurde.

Controller 1 der aktiven Controller wurde, und wie folgt konfiguriert wurde:

- Daten 0, Daten 1, Daten 2, Daten 3, Daten 4 und 5 Daten Netzwerk Schnittstellen auf dem Gerät vorhanden.
- Daten 0 aktiviert wurde.

**Beispiel für die Ausgabe – Controller 0**

So sieht das Ergebnis vom Controller 0 (passive Controller). Daten 1, Daten 2 und 3 von Daten sind nicht verbunden. Daten 4 und 5 Daten werden nicht aufgeführt, da sie nicht auf dem Gerät vorhanden sind. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Beispiel für die Ausgabe – Controller 1**

So sieht das Ergebnis vom Controller 1 (der aktiven Controller). Nur die Daten 0-Schnittstelle auf dem Gerät konfiguriert ist und funktioniert.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Behandeln von Problemen mit mit das Cmdlet "Verbindung testen"

Sie können die `Test-Connection` Cmdlet, um festzustellen, ob Ihr Gerät StorSimple mit dem externen Netzwerk herstellen kann. Wenn alle Netzwerke Parameter, einschließlich der DNS-Einträge, im Setup-Assistenten ordnungsgemäß konfiguriert sind, können Sie die `Test-Connection` Cmdlet Signal an eine bekannte Adresse außerhalb des Netzwerks, z. B. outlook.com. 

Aktivieren Sie Ping zum Behandeln von Verbindungsproblemen mit diesem Cmdlet Wenn Ping deaktiviert ist.

Finden Sie in den folgenden Beispielen der Ausgabe von der `Test-Connection` Cmdlet. 

> [AZURE.NOTE] Im ersten Beispiel ist das Gerät mit einer falschen DNS konfiguriert. Klicken Sie in das zweite Beispiel ist die DNS richtig.
 
**Beispiel für die Ausgabe – falsche DNS-Einträge**

Im folgenden Beispiel gibt es keine Ausgabe für IPV4 und IPV6-Adressen, die angibt, dass die DNS-Einträge nicht behoben wurde. Dies bedeutet, dass es keine Verbindung mit dem externen Netzwerk besteht und eine richtige DNS angegeben werden muss. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Ein Beispiel einer Ausgabe – korrigieren Sie die DNS-Einträge**

Im folgenden Beispiel gibt die DNS-Einträge die IPv4-Adresse, die angibt, dass DNS ordnungsgemäß konfiguriert ist. Dies bestätigt, dass eine Verbindung mit dem externen Netzwerk. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Behandeln von Problemen mit mit dem Test-HcsmConnection-cmdlet

Verwenden der `Test-HcsmConnection` -Cmdlet für ein Gerät, das bereits mit verbunden und mit Ihrem Dienst StorSimple Manager registriert ist. Mit diesem Cmdlet können Sie die Verbindung zwischen einem Gerät registriert und den entsprechenden StorSimple Manager-Dienst zu überprüfen. Sie können diesen Befehl auf Windows PowerShell für StorSimple ausgeführt werden. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Zum Ausführen des Cmdlets Test-HcsmConnection

1. Stellen Sie sicher, dass das Gerät registriert ist.

2. Überprüfen des Gerätestatus. Wenn das Gerät im Wartungsmodus oder offline, deaktiviert wird möglicherweise der folgende Fehler angezeigt: 

   - ErrorCode.CiSDeviceDecommissioned – gibt an, dass das Gerät deaktiviert ist.
   - ErrorCode.DeviceNotReady – gibt an, dass das Gerät Wartungsmodus ist.
   - ErrorCode.DeviceNotReady – gibt an, dass das Gerät nicht online ist.

3. Stellen Sie sicher, dass der StorSimple Manager-Dienst ausgeführt wird (verwenden Sie das Cmdlet " [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) "). Wenn der Dienst nicht ausgeführt wird, wird möglicherweise der folgende Fehler angezeigt:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – gibt an, dass eine Ausnahme aufgetreten, wenn Sie Get-ClusterResource ausgeführt haben.

4. Aktivieren Sie das Token Access Control Service (ACS). Wenn es ein Web-Ausnahme möglicherweise als Ergebnis eines Problems Gateway, eine fehlende Proxy-Authentifizierung, eine falsche DNS- oder eine fehlgeschlagene Authentifizierung. Der folgende Fehler möglicherweise wird angezeigt:

   - ErrorCode.CiSApplianceGateway – Dies weist darauf hin eine Ausnahme HttpStatusCode.BadGateway: der Namensauflösungsdienst konnte nicht den Hostnamen aufgelöst werden. 
   - ErrorCode.CiSApplianceProxy – Dies weist darauf hin eine Ausnahme HttpStatusCode.ProxyAuthenticationRequired (HTTP-Statuscode 407): der Client konnte nicht mit dem Proxyserver authentifizieren. 
   - ErrorCode.CiSApplianceDNSError – Dies weist darauf hin eine Ausnahme WebExceptionStatus.NameResolutionFailure: der Namensauflösungsdienst konnte nicht den Hostnamen aufgelöst werden.
   - ErrorCode.CiSApplianceACSError – gibt an, dass der Dienst hat einen Authentifizierungsfehler zurückgegeben, doch es Connectivity gibt.
   
    Wenn keine Webausnahme ausgelöst wird, prüfen Sie ErrorCode.CiSApplianceFailure aus. Dies zeigt an, dass das Gerät fehlgeschlagen ist.

5. Überprüfen Sie die Cloud Service-Konnektivität. Wenn der Dienst eine Web Ausnahme, wird möglicherweise der folgende Fehler angezeigt:

  - ErrorCode.CiSApplianceGateway – Dies weist darauf hin eine Ausnahme HttpStatusCode.BadGateway: ein mittlere Proxyserver ungültige Anforderung aus einem anderen Proxyserver oder dem ursprünglichen Server erhalten.
  - ErrorCode.CiSApplianceProxy – Dies weist darauf hin eine Ausnahme HttpStatusCode.ProxyAuthenticationRequired (HTTP-Statuscode 407): der Client konnte nicht mit dem Proxyserver authentifizieren. 
  - ErrorCode.CiSApplianceDNSError – Dies weist darauf hin eine Ausnahme WebExceptionStatus.NameResolutionFailure: der Namensauflösungsdienst konnte nicht den Hostnamen aufgelöst werden.
  - ErrorCode.CiSApplianceACSError – gibt an, dass der Dienst hat einen Authentifizierungsfehler zurückgegeben, doch es Connectivity gibt.
  
    Wenn keine Webausnahme ausgelöst wird, prüfen Sie ErrorCode.CiSApplianceSaasServiceError aus. Dies weist darauf hin, ein Problem mit dem Dienst StorSimple-Manager.
 
6. Überprüfen Sie die Verbindung Azure Service Bus. ErrorCode.CiSApplianceServiceBusError gibt an, dass das Gerät keine Verbindung mit dem Dienst herstellen kann.
 
Die Protokolldateien CiSCommandletLog0Curr.errlog und CiSAgentsvc0Curr.errlog verfügen über weitere Informationen, z. B. Ausnahmedetails. 

Weitere Informationen dazu, wie Sie das Cmdlet verwenden, wechseln Sie zu [Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) in der Windows PowerShell verwiesen Sie Dokumentation werden.

> [AZURE.IMPORTANT] Sie können mit diesem Cmdlet für das aktive und der passive Controller ausführen. 
 
Finden Sie in den folgenden Beispielen der Ausgabe von der `Test-HcsmConnection` Cmdlet. 

**Ein Beispiel einer Ausgabe – erfolgreich registrierten Gerät StorSimple Version ausgeführt (Juli 2014)**

Das erste Beispiel ist auf einem Gerät, das erfolgreich mit dem Dienst StorSimple Manager registriert ist und weist keine Netzwerkkonnektivitätsprobleme vor. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Beispiel für die Ausgabe – erfolgreich registrierten Gerät StorSimple Update 1 ausführen**

Wenn Sie auf Ihrem Gerät StorSimple Update 1 ausführen, müssen Sie nicht mit dem Schalter ausführlichen auszuführen.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Ein Beispiel einer Ausgabe – offline Gerät StorSimple Version ausgeführt (Juli 2014)**

In diesem Beispiel ist auf einem Gerät mit dem Status **Offline** im klassischen Azure-Portal.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Das Gerät konnte keine Verbindung mit der aktuellen Web Proxy-Konfiguration. Dies kann ein Problem mit der Web-Proxy-Konfiguration oder ein Netzwerk-Verbindungsproblem sein. In diesem Fall sollten Sie sicherstellen, dass Ihre Webproxyeinstellungen richtig sind und die Web-Proxy-Servern online und erreichbar sind. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Behandeln von Problemen mit mit dem Cmdlet HcsTime synchronisieren

Verwenden Sie dieses Cmdlet die Uhrzeit Gerät angezeigt wird. Wenn das Gerät Mal einen Versatz mit dem NTP-Server verfügt, klicken Sie dann können mit diesem Cmdlet Sie die Zeit mit dem NTP-Server sofort synchronisieren. Wenn der Abstand zwischen dem Gerät und NTP-Server größer als 5 Minuten ist, wird eine Warnung angezeigt. Der Offset 15 Minuten überschreitet, wird das Gerät offline gehen. Mit diesem Cmdlet können weiterhin eine Uhrzeit Synchronisierung erzwingen. Jedoch überschreitet der Versatz 15 Stunden, werden dann Sie kann nicht in Kraft synchron, die den Arbeitszeit- und eine Fehlermeldung angezeigt werden.

**Beispiel für die Ausgabe – erzwungene Uhrzeit synchronisieren synchronisieren-HcsTime verwenden**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Behandeln von Problemen mit mit den HcsPing-aktivieren und deaktivieren-HcsPing-cmdlets

Mithilfe dieser Cmdlets sicherstellen, dass die Netzwerk-Schnittstellen auf Ihrem Gerät auf ICMP Ping-Anfragen antworten. Standardmäßig Antworten die StorSimple Netzwerk-Schnittstellen nicht auf Ping-Abfragen. Mit diesem Cmdlet ist die einfachste Möglichkeit zum feststellen, ob Ihr Gerät online und erreichbar ist.  

**Beispiel für die Ausgabe –-HcsPing aktivieren und deaktivieren-HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Behandeln von Problemen mit mit dem Cmdlet Spur-HcsRoute

Verwenden Sie dieses Cmdlet als ein Tool zum Routing verfolgen. Sendet Pakete zu jedem auf dem Weg zu einem Ziel über einen Zeitraum, und klicken Sie dann berechnet Ergebnisse auf Grundlage der Pakete aus jedem Abschnitt zurückgegeben. Da das Cmdlet den Grad des Paketverlusts an jedem Router oder Link angezeigt wird, können Sie feststellen, welche Router oder Links möglicherweise Netzwerkprobleme verursacht.

**Beispiel für die Ausgabe zeigt, wie Sie die Übermittlung der ein Paket mit Spur-HcsRoute verfolgen**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Behandeln von Problemen mit mit das Cmdlet "Get-HcsRoutingTable"

Verwenden Sie dieses Cmdlet, um die routing-Tabelle für Ihr Gerät StorSimple anzuzeigen. Eine routing-Tabelle ist eine Reihe von Regeln, die Ihnen helfen können, die bestimmen, wo die Datenpakete unterwegs über ein Netzwerk IP (Internet Protocol) weitergeleitet werden sollen. 

Die routing-Tabelle zeigt die Schnittstellen und Gateways, die die Daten an den angegebenen Netzwerken weiterleitet. Darüber hinaus haben die Weiterleitung Metrik also Entscheidungsträger, ob jemand für den Pfad zu ein bestimmtes Ziel erreicht haben. Je niedriger der Weiterleitung Metrik, je höher die Einstellung. 

Beispielsweise, wenn Sie 2 Netzwerkschnittstellen verfügen, Daten 2 und 3 von Daten, mit dem Internet verbunden. Wenn die Weiterleitung Metrik für Daten 2 und 3 von Daten 15 und 261 Hilfethemas sind, ist Daten 2 mit der unteren Weiterleitung Metrik die bevorzugte Schnittstelle verwendet, die mit dem Internet herstellen.

Wenn Sie auf Ihrem Gerät StorSimple Update 1 ausgeführt werden, hat Ihre Daten 0 Netzwerk-Oberfläche die höchste Priorität für den Datenverkehr Cloud. Dies bedeutet, dass es sich bei der Cloud Datenverkehr, auch wenn andere Schnittstellen Cloud aktiviert sind, durch die Daten 0 weitergeleitet werden möchten. 

Wenn Sie beim Ausführen der `Get-HcsRoutingTable` Cmdlet ohne Angabe von Parametern verwenden (wie im folgenden Beispiel gezeigt), wird das Cmdlet IPv4 und IPv6-routing-Tabellen ausgeben. Alternativ können Sie angeben `Get-HcsRoutingTable -IPv4` oder `Get-HcsRoutingTable -IPv6` können Sie eine relevante routing-Tabelle zu gelangen.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Beispiel für schrittweise StorSimple zur Problembehandlung

Im folgenden Beispiel wird schrittweise Problembehandlung bei einer StorSimple Bereitstellung. Im Beispielszenario schlägt fehl Gerät Registration wird eine Fehlermeldung, dass die Einstellungen im Netzwerk oder der DNS-Name falsch ist.

Die Fehlermeldung zurückgegeben wird:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Der Fehler konnte eine der folgenden Ursachen haben:

- Falsche Hardware-installation
- Fehlerhafte Netzwerk-Schnittstellen
- Falsche IP-Adresse, Subnetz-Maske, Gateway, primären DNS-Server oder Web-proxy
- Falsche Registrierungsschlüssel
- Falsche Firewall-Einstellungen

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Suchen und Beheben des Problems auf Gerät Registrierung

1. Überprüfen Sie die Gerätekonfiguration: Führen Sie auf dem aktiven Controller, `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] Klicken Sie auf dem aktiven Controller muss der Setup-Assistenten ausführen. Um zu überprüfen, dass Sie mit dem aktiven Controller verbunden sind, sehen Sie sich im Banner in der seriellen Konsole angezeigt. Klicken Sie im Banner gibt an, ob Sie mit Controller 0 oder 1-Controller verbunden sind, und gibt an, ob der Controller wird. Wechseln Sie weitere Informationen zu [Identifizieren einer aktiven Controller auf Ihrem Gerät](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Stellen Sie sicher, dass das Gerät ordnungsgemäß verbunden ist: Überprüfen Sie das Kabel auf dem Gerät wieder Ebene Netzwerk. Das Kabel gilt nur für das Gerätemodell. Weitere Informationen finden Sie auf [Ihrem Gerät StorSimple 8100](storsimple-8100-hardware-installation.md) [Installieren](storsimple-8600-hardware-installation.md)oder Ihrem Gerät StorSimple 8600.

     > [AZURE.NOTE] Wenn Sie 10 GbE-Netzwerk-Ports verwenden, müssen Sie die bereitgestellten QSFP-SFP Netzwerkadapter und SFP Kabel verwendet werden. Weitere Informationen finden Sie unter der [Liste der Kabel, Schalter, und Transceiver vom OEM Lieferanten für Ports Mellanox empfohlen](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. Überprüfen Sie den Status der Netzwerkschnittstelle aus:

   - Verwenden Sie das Cmdlet "Get-NetAdapter", um die Integrität des Netzwerk-Schnittstellen für Daten 0 zu erkennen. 
   - Wenn der Link nicht funktioniert, wird der Status **Ifindex** kennzeichnen, die Benutzeroberfläche nach unten. Sie müssen die Netzwerkverbindung des Ports an die Einheit und dem Schalter überprüfen. Sie benötigen außerdem fehlerhafte Kabel auszuschließen. 
   - Wenn Sie davon ausgehen, dass die Daten 0 an port der aktive Controller ausgefallen, können Sie diese durch Herstellen einer Verbindung zu den Daten 0 Port auf Controller 1 bestätigen. Um dies zu überprüfen, ziehen Sie das Netzwerkkabel von der Rückseite des Geräts vom Controller 0, schließen Sie das Kabel an Controller 1, und führen Sie das Cmdlet "Get-NetAdapter" erneut aus. 
   Wenn die Daten 0 an port fehlschlägt ein Controller, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um weitere Schritte. Möglicherweise müssen Sie den Controller in Ihrem System zu ersetzen.
 
4. Überprüfen Sie die Verbindung zu den Schalter aus:
   - Stellen Sie sicher, dass Daten 0 Netzwerk-Schnittstellen 0 und 1-Controller in das mit der primären Einheit auf demselben Subnetz befinden. 
   - Aktivieren Sie Hub oder Router. In der Regel, sollten Sie beide Controller mit dem gleichen Hub oder Router verbinden. 
   - Stellen Sie sicher, dass die Schalter für die Verbindung verwendeten Daten 0 für beide Controller im gleichen LAN haben.
   
5. Entfernen von einem beliebigen Benutzerfehlern:

  - Führen Sie den Setup-Assistenten erneut (ausführen **Aufrufen-HcsSetupWizard**), und geben Sie die Werte erneut aus, um sicherzustellen, dass keine Fehler vorliegen. 
  - Überprüfen Sie die Registrierung Schlüssel verwendet. Der gleichen Registrierungsschlüssel kann Verbindung mehrere Geräte an einen StorSimple-Manager-Dienst verwendet werden. Gehen Sie vor in [der Registrierungsschlüssel Dienst erhalten](storsimple-manage-service.md#get-the-service-registration-key) , um sicherzustellen, dass Sie den richtigen Registrierungsschlüssel verwenden.

    > [AZURE.IMPORTANT] Wenn Sie mehrere Services ausgeführt haben, müssen Sie sicherstellen, dass der Registrierungsschlüssel für den entsprechenden Dienst verwendet wird, um das Gerät zu registrieren. Wenn Sie ein Gerät mit dem falschen StorSimple Manager-Dienst registriert haben, müssen Sie für weitere Schritte [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) . Möglicherweise müssen Sie eine Fabrik Zurücksetzen des Geräts (der Datenverlust führen kann) zum Ausführen, und klicken Sie dann verbinden Sie es mit den beabsichtigten Dienst.

6. Verwenden Sie das Cmdlet Verbindung testen, um sicherzustellen, dass Sie die Verbindung zu externen Netzwerk haben. Weitere Informationen zum [Behandeln von Problemen mit der Verbindung testen Cmdlet](#troubleshoot-with-the-test-connection-cmdlet)wechseln.

7. Firewall Durchdringung prüfen. Wenn Sie, die überprüft haben die virtuellen IP-Adresse (VIP), Subnetz, Gateway und DNS-Einstellungen sind alle korrekt, Verbindungsprobleme weiterhin angezeigt, und dann ist es möglich, dass Ihre Firewall Kommunikation zwischen Ihrem Gerät und dem externen Netzwerk blockiert werden. Sie müssen Sie sicherstellen, dass die Ports 80 und 443 auf Ihrem Gerät StorSimple für ausgehende Kommunikation zur Verfügung stehen. Weitere Informationen finden Sie unter [Networking Anforderungen für Ihr Gerät StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

8. Prüfen Sie die Protokolle aus. Wechseln Sie zum [Support-Paketen und Gerät Protokolle zur Behandlung dieses Problems zur Verfügung](#support-packages-and-device-logs-available-for-troubleshooting).

9. Wenn Sie die vorherigen Schritte nicht das Problem, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um Unterstützung gelöst werden.

## <a name="next-steps"></a>Nächste Schritte
Sie [erfahren, wie Sie ein Gerät Betrieb zu lösen](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
