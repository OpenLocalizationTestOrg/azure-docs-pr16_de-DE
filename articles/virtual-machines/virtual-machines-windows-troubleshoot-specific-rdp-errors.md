<properties
    pageTitle="Bestimmte RDP Fehlermeldungen für Azure-virtuellen Computern | Microsoft Azure"
    description="Grundlegendes zu, dass bestimmte Fehlermeldungen, die möglicherweise angezeigt wird, wenn Sie versuchen, in Azure Remote Desktop-Verbindung zu einem Windows-Computer verwenden"
    keywords="Remote desktop zurück, Remotedesktop-Verbindung zurück, keine Verbindung zu virtuellen Computer Remotedesktop Problembehandlung"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Problembehandlung bei bestimmten RDP Fehlermeldungen an einen virtuellen Computer Windows Azure
Sie möglicherweise eine bestimmte Fehlermeldung erhalten, wenn Remote Desktop-Verbindung zu einem Windows-Computer (virtueller Computer) in Azure verwenden. In diesem Artikel sind einige der häufigeren Fehlermeldungen aufgetreten sind, sowie die Problembehandlung vor, um diese zu beheben. Wenn Sie Probleme beim Herstellen einer Verbindung mit Ihrem virtuellen Computer mit RDP aber keine bestimmte Fehlermeldung auftreten, finden Sie unter die [zur Problembehandlung bei Remotedesktop](virtual-machines-windows-troubleshoot-rdp-connection.md).

Informationen zu bestimmten Fehlermeldungen finden Sie unter Folgendes:

- [Die remote-Sitzung getrennt wurde, da es sind keine Remote Desktop Lizenzserver eine Lizenz verfügbar sind](#rdplicense).
- [Remote Desktop des Computers "Name" kann nicht gefunden werden](#rdpname).
- [Eine Authentifizierung Fehler ist aufgetreten. Die lokale Sicherheit Zertifizierungsstelle kann keine Verbindung hergestellt werden](#rdpauth).
- [Windows-Sicherheit zurück: funktionieren Ihre Anmeldeinformationen nicht](#wincred).
- [Diesem Computer kann keine Verbindung mit dem Remotecomputer herstellen](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Die remote-Sitzung wurde getrennt, da es sind keine Remote Desktop-Lizenzserver eine Lizenz verfügbar sind.

Ursache: Der Nachfrist 120 Tage zur Lizenzierung für die Funktion Remote Desktop-Servers ist abgelaufen, und Lizenzen installieren müssen.

Problem zu umgehen speichern Sie eine lokale Kopie der Datei RDP aus dem Portal, und führen Sie diesen Befehl über die Befehlszeile PowerShell Verbindung. Dieser Schritt deaktiviert nur diese Verbindung Lizenzierung:

        mstsc <File name>.RDP /admin

Wenn Sie mehr als zwei gleichzeitige Remote Desktop-Verbindungen mit dem virtuellen Computer tatsächlich benötigen, können Server-Manager Sie um die Rolle des Remote Desktop-Server zu entfernen.

Weitere Informationen finden Sie im Blogbeitrag [Azure-virtuellen Computer schlägt fehl mit "Kein Remote Desktop-Lizenzserver verfügbar"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Remotedesktop kann den Computer "Name" nicht finden.

Ursache: Den Namen des Computers in den Einstellungen von der RDP-Datei kann nicht auf Ihrem Computer Remotedesktop-Clients aufgelöst werden.

Lösungsvorschläge:

- Wenn Sie Intranet einer Organisation angezeigt wird, stellen Sie sicher, dass Ihr Computer Zugriff auf dem Proxyserver und kann HTTPS-Verkehr zu senden.

- Wenn Sie eine lokal gespeicherte RDP-Datei verwenden, versuchen Sie es mit der Phase, die vom Portal generiert wird. Dieser Schritt stellen Sie sicher, dass Sie den richtigen DNS-Namen für den virtuellen Computer, oder Cloud-Dienst und den Endpunktport, der den virtuellen Computer haben. Hier ist eine Stichprobe RDP-Datei, die vom Portal generierte aus:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Der Adressteil dieses RDP-Datei wurde:
- Den vollqualifizierten Domänennamen des Cloud-Dienst, der den virtuellen Computer ("Tailspin-azdatatier.cloudapp.net" in diesem Beispiel) enthält.

- Der externe TCP-des Endpunkts für Remotedesktop Datenverkehr (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Ein Authentifizierungsfehler ist aufgetreten. Die lokale Sicherheit Zertifizierungsstelle kann keine Verbindung hergestellt werden.

Ursache: Das Ziel virtueller Computer kann nicht die Sicherheit Zertifizierungsstelle in den Benutzer Namensteil Ihrer Anmeldeinformationen gesucht werden soll.

Wenn Ihr Benutzername ist im Formular *SecurityAuthority*\\*Benutzernamen* (Beispiel: CORP\User1), der *SecurityAuthority* Anteil ist entweder Computernamen (für die lokale Sicherheit Zertifizierungsstelle) des virtuellen Computers oder einem Active Directory-Domänennamen.

Lösungsvorschläge:

- Wenn das Konto für den virtuellen Computer lokal ist, stellen Sie sicher, dass der Name des virtuellen Computers richtig geschrieben ist.

- Wenn das Konto in einer Active Directory-Domäne ist, überprüfen Sie die Schreibweise des Domänennamens.

- Wenn sie eine Domäne Active Directory-Konto ist und der Domänenname richtig geschrieben ist, stellen Sie sicher, dass ein Domänencontroller in dieser Domäne verfügbar ist. Es ist eine allgemeine Problem in Azure virtuelle Netzwerke, die Domänencontroller enthalten, dass ein Domänencontroller nicht verfügbar ist, da es noch nicht gestartet wurde. Problem zu umgehen können Sie ein lokales Administratorkonto anstelle einer Domänenkonto verwenden.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows-Sicherheit zurück: Ihre Anmeldeinformationen nicht funktionieren.

Ursache: Das Ziel virtueller Computer kann nicht Ihren Kontonamen und Ihr Kennwort überprüfen.

Ein Windows-Computer kann die Anmeldeinformationen des entweder eine lokale oder ein Benutzerkonto Domäne überprüfen.

- Verwenden Sie für lokale Konten *ComputerName*\\Syntax für*Benutzernamen* (Beispiel: SQL1\Admin4798).
- Verwenden Sie für Domänenkonten der *Domänenname*\\Syntax für*Benutzernamen* (Beispiel: CONTOSO\peterodman).

Wenn Sie Ihre virtuellen Computer mit einem Domänencontroller in einer neuen Active Directory höhergestuft haben, wird das lokale Administratorkonto, dem mit dem Sie angemeldet in entsprechende Konto mit demselben Kennwort in der neuen Gesamtstruktur und Domäne konvertiert. Das lokale Konto wird gelöscht.

Beispielsweise wenn Sie mit dem lokalen DC1\DCAdmin-Konto angemeldet, und dann des virtuellen Computers als Domänencontroller in einer neuen Gesamtstruktur für die Domäne corp.contoso.com heraufgestufte, das lokale DC1\DCAdmin-Konto wird gelöscht, und ein neue Domänenkonto (CORP\DCAdmin) wird erstellt, mit demselben Kennwort.

Stellen Sie sicher, dass der Kontoname einen Namen ist, den als ein gültiges Konto des virtuellen Computers überprüfen können, und das Kennwort richtig ist.

Wenn Sie das Kennwort für das lokale Administratorkonto ändern müssen, finden Sie unter [Zurücksetzen eines Kennworts oder der Remote Desktop-Dienst für Windows-virtuellen Computern](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Diesem Computer kann keine Verbindung mit dem Remotecomputer herstellen.

Ursache: Das Konto, das zum Herstellen der Verbindung verfügt nicht Remotedesktop Anmeldung Rechte.

Jeder Windows-Computer verfügt über eine Remotedesktop lokalen Benutzergruppe, enthält die Konten und Gruppen aus, die darin Remote anmelden können. Mitglieder der Gruppe der lokalen Administratoren können zudem zugreifen, obwohl diese Konten in der lokalen Benutzergruppe Remotedesktop nicht aufgelistet werden. Für Domänenverbund Maschinen enthält der lokalen Administratorgruppe auch die Domänenadministratoren für die Domäne aus.

Stellen Sie sicher, dass das Konto, das Sie verwenden, um das Herstellen von Verbindungen mit Remotedesktop Anmeldung Rechte verfügt. Problem zu umgehen verwenden Sie eine Domäne oder eine lokale Administratorkonto Herstellen einer Verbindung über Remote Desktop aus. Um das gewünschte Konto der lokalen Remotedesktop Benutzergruppe hinzuzufügen, verwenden Sie das Microsoft Management Console-Snap-in (**Systemprogramme > Lokale Benutzer und Gruppen > Gruppen > Remote Desktop-Benutzer**).


## <a name="next-steps"></a>Nächste Schritte
Wenn keiner dieser Fehler aufgetreten ist, und Sie haben ein unbekanntes Problem beim Herstellen einer Verbindung mit RDP, finden Sie unter [Problembehandlung bei der Remotedesktop](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure IaaS (Windows) Diagnose-Paket](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Zur Behandlung dieses Problems Schritte beim Zugriff auf die Anwendung, die auf einem virtuellen Computer ausgeführt wird, finden Sie unter [Behandeln von Zugriff auf eine Anwendung einer Azure-virtuellen Computers ausgeführt](virtual-machines-linux-troubleshoot-app-connection.md).
- Wenn Sie mithilfe von Secure Shell (SSH) Verbindung zu einem Linux VM in Azure Probleme auftreten, finden Sie unter [Behandeln von Problemen mit SSH Verbindungen mit eines Linux virtuellen Computers in Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).