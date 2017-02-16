<properties
    pageTitle="So erstellen Sie eine benutzerdefinierte Vorlagenbild für Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Erstellen eines benutzerdefinierten Vorlage Bilds für Azure RemoteApp. Sie können mithilfe dieser Vorlage mit einem Hybriden oder Cloud-Websitesammlung."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>So erstellen Sie eine benutzerdefinierte Vorlagenbild für Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp verwendet ein Windows Server 2012 R2 Vorlagenbild, um alle Programme zu hosten, die Sie für Ihre Benutzer freigeben möchten. Um ein benutzerdefiniertes Bild der RemoteApp Vorlage erstellen, können Sie mit einem vorhandenen Bild beginnen oder einen neuen erstellen. 


> [AZURE.TIP] Wussten Sie schon, dass Sie ein Bild aus einer Azure-virtuellen Computer erstellen können? WAHR Geschichte, woraufhin die Übernahme erheblich reduziert die Zeitdauer es hat, um das Bild zu importieren. Schauen Sie sich die Schritte [hier](remoteapp-image-on-azurevm.md).

Sind die Anforderungen für das Bild, das für die Verwendung mit Azure RemoteApp hochgeladen werden kann:


- Die Bildgröße sollte ein Vielfaches der MB sein. Wenn Sie versuchen, ein Bild hochladen, die ein Vielfaches nicht ist, tritt ein Fehler des Uploads.
- Die Bildgröße muss 127 GB oder zu verkleinern.
- Es muss eine virtuelle Festplatte-Datei (VHDX Dateien [Hyper-V virtuelle Festplatten] werden derzeit nicht unterstützt).
- Die virtuelle Festplatte darf eine Generation 2 virtuellen Computern.
- Die virtuelle Festplatte möglich fester Größe oder Dyn. Eine dynamisch erweiterte virtuelle Festplatte wird empfohlen, weil sie weniger Zeit als eine feste Größe virtuelle Festplatte Datei in Azure hochladen annimmt.
- Der Datenträger muss mit der Master Boot Record (MBR) formatiert werden: Initialisierung werden. Die Tabelle (GPT)-Partitionsstil GUID wird nicht unterstützt.
- Die virtuelle Festplatte muss mit eine einzige Installation von Windows Server 2012 R2 enthalten. Sie können mehrere Datenmengen, aber nur ein, die eine Installation von Windows enthält enthalten.
- Die Rolle des Remote Desktop Session Host (RDSH) und das Feature für die Desktopdarstellung müssen installiert sein.
- Die Rolle des Remotedesktop-Verbindungsbroker muss *nicht* installiert werden.
- Die Datei System VERSCHLÜSSELNDE muss deaktiviert werden.
- Das Bild muss mit den Parametern SYSPREPed **/oobe / generalize/shutdown** (nicht den **/mode:vm** Parameter verwenden).
- Hochladen der virtuellen Festplatte aus einer Momentaufnahmekette wird nicht unterstützt.


**Vorbemerkung**

Sie müssen vor dem Erstellen des Diensts die folgenden Schritte durchführen:

- [Registrieren](https://azure.microsoft.com/services/remoteapp/) für RemoteApp an.
- Erstellen eines Benutzerkontos in Active Directory als Dienstkonto RemoteApp verwenden. Schränken Sie die Berechtigungen für dieses Konto ein, sodass sie nur Computer in der Domäne hinzufügen kann. Weitere Informationen finden Sie unter [Konfigurieren von Azure Active Directory für RemoteApp](remoteapp-ad.md) .
- Sammeln Sie Informationen zu Ihrem lokalen Netzwerk: IP-Adresse, Informationen und Details von VPN-Gerät.
- Installieren Sie das [Azure PowerShell](../powershell-install-configure.md) -Modul.
- Sammeln Sie Informationen zu den Benutzern, denen Sie Zugriff gewähren möchten. Dies kann entweder Microsoft-Kontoinformationen oder Active Directory-Konto Arbeitsinformationen für Benutzer sein.



## <a name="create-a-template-image"></a>Erstellen Sie ein Vorlagenbild ##

Hierbei handelt es sich um die auf hoher Ebene Schritte, um ein neues Vorlagenbild von Grund auf neu zu erstellen:

1.  Suchen Sie ein Windows Server 2012 R2 aktualisieren DVD oder ISO-Bild aus.
2.  Erstellen Sie eine virtuelle Festplatte Datei ein.
4.  Installieren von Windows Server 2012 R2.
5.  Installieren Sie die Rolle des Remote Desktop Session Host (RDSH) und die Desktop Experience-Funktion.
6.  Installieren Sie zusätzliche Features, die durch die Anwendung erforderlich.
7.  Installieren Sie und konfigurieren Sie einer Anwendung. Um die Freigabe apps zu erleichtern, fügen Sie alle apps oder Programme, die Sie dem Menü **Start** des Bilds, insbesondere in **%systemdrive%\ProgramData\Microsoft\Windows\Start \Programme freigeben möchten.
8.  Führen Sie eine zusätzlichen Windows Konfigurationen nach Ihrer Anwendung erforderlich.
9.  Deaktivieren Sie das Verschlüsselung File System (EFS).
10. **Erforderlich:** Wechseln Sie zur Windows Update, und installieren Sie alle wichtigen Updates.
9.  SYSPREP das Bild.

Die detaillierten Schritte zum Erstellen eines neuen Bilds sind:

1.  Suchen Sie ein Windows Server 2012 R2 aktualisieren DVD oder ISO-Bild aus.
2.  Erstellen Sie eine virtuelle Festplatte-Datei mithilfe der Datenträger Verwaltung.
    1.  Starten Sie Datenträger Verwaltung (diskmgmt.msc).
    2.  Erstellen Sie eine dynamisch erweiterte virtuelle Festplatte von mindestens 40 GB Größe ein. (Schätzen Sie wird der Abstand für Windows, Ihre Applikationen und Anpassungen erforderlich. Windows-Server mit der Rolle RDSH und Desktop Experience-Feature installiert wird ungefähr 10 GB Speicherplatz erforderlich).
        1.  Klicken Sie auf **Aktion > virtuelle Festplatte erstellt**.
        2.  Geben Sie den Speicherort, Größe und virtuelle Festplatte formatieren. Wählen Sie **dynamisch erweitern**, und klicken Sie dann auf **OK**.

            Dadurch wird für einige Sekunden ausgeführt. Klicken Sie nach Abschluss die Erstellung virtuelle Festplatte sollte einen neuen Datenträger, ohne Sie zu einem beliebigen Laufwerkbuchstaben und im Status "Nicht Initialisierung", in der Datenträger-Verwaltungskonsole angezeigt werden.

        - Mit der rechten Maustaste in des Datenträgers (nicht die Speicherplatz), und klicken Sie dann auf **Datenträger Initialisierung**. Wählen Sie als den Partitionsstil **MBR** (Master Boot Record) aus, und klicken Sie dann auf **OK**.
        - Erstellen Sie ein neues Volume: mit der rechten Maustaste in des verfügbaren Platz, und klicken Sie dann auf **Neues einfaches Volume**. Sie können akzeptieren Sie die Standardeinstellungen des Assistenten, aber Sie können sicherstellen, dass Sie einen Laufwerkbuchstaben, um potenzielle Probleme zu vermeiden, wenn Sie das Vorlagenbild hochladen zuweisen.
        - Mit der rechten Maustaste in des Datenträgers, und klicken Sie dann auf **Trennen virtuelle Festplatte**.





1. Installieren von Windows Server 2012 R2:
    1. Erstellen eines neuen virtuellen Computers. Verwenden Sie den Assistenten für neue virtuelle Computer in Hyper-V-Manager oder Client Hyper-V.
        1. Wählen Sie auf der Seite Specify Generation **Generation 1**ein.
        2. Klicken Sie auf der Seite virtuelle Festplatte verbinden wählen Sie **Verwenden einer vorhandenen virtuellen Festplatte**, und navigieren Sie zu der virtuellen Festplatte, die Sie im vorherigen Schritt erstellt haben.
        2. Klicken Sie auf der Seite Installationsoptionen wählen Sie **ein Betriebssystem aus einem Boot CD-ROM installieren**aus, und wählen Sie dann auf den Speicherort der Windows Server 2012 R2 Installationsmedien.
        3. Wählen Sie weitere Optionen im Assistenten erforderlich, Windows und Ihre Anwendungen zu installieren. Beenden Sie den Assistenten.
    2.  Nach Beendigung des Assistenten, bearbeiten Sie die Einstellungen des virtuellen Computers und machen Sie alle anderen Änderungen erforderlich, Windows und Ihre Programme, wie etwa die Anzahl der virtuellen Prozessoren zu installieren, und klicken Sie dann auf **OK**.
    4.  Herstellen einer Verbindung des virtuellen Computers mit, und installieren Sie Windows Server 2012 R2.
1. Installieren Sie die Rolle des Remote Desktop Session Host (RDSH) und die Desktop Experience-Funktion ein:
    1. Server-Manager zu starten.
    2. Klicken Sie im Bildschirm Willkommen den oder aus dem Menü **Verwalten** auf **Hinzufügen von Rollen und Features** .
    3. Klicken Sie auf der Seite Vorbereitung auf **Weiter** .
    4. Wählen Sie **rollenbasierte oder featurebasierten Installation**, und klicken Sie dann auf **Weiter**.
    5. Wählen Sie den lokalen Computer in der Liste aus, und klicken Sie dann auf **Weiter**.
    6. Wählen Sie **Remote Desktop Services**aus, und klicken Sie dann auf **Weiter**.
    7. Erweitern Sie **Benutzeroberflächen und Infrastruktur** zu, und wählen Sie die **Desktopdarstellung**.
    8. Klicken Sie auf **Features hinzufügen**, und klicken Sie dann auf **Weiter**.
    9. Klicken Sie auf der Seite Remote Desktop Services auf **Weiter**.
    10. Klicken Sie auf **Remote Desktop Session Host**.
    11. Klicken Sie auf **Features hinzufügen**, und klicken Sie dann auf **Weiter**.
    12. Klicken Sie auf der Seite Confirm Installation Auswahl Wählen Sie **den Zielserver neu starten, automatisch, falls erforderlich**, und klicken Sie dann auf **Ja** , wenn die Warnung neu starten.
    13. Klicken Sie auf **Installieren**. Der Computer wird gestartet.
1.  Installieren Sie zusätzliche Features, die Ihre, z. B. von .NET Framework 3.5 erforderlich. Um die Features zu installieren, führen Sie das Hinzufügen von Rollen und Features-Assistenten aus.
7.  Installieren Sie und konfigurieren Sie der Programme und Anwendungen, die Sie über RemoteApp veröffentlichen möchten.

>[AZURE.IMPORTANT]
>
>Installieren Sie die Rolle des RDSH vor der Installation von Applications, um sicherzustellen, dass die Anwendungskompatibilität Probleme erkannt werden, bevor das Bild auf RemoteApp hochgeladen wird.
>
>Stellen Sie sicher, dass eine Verknüpfung mit der Anwendung (**lnk** -Datei) in **das Startmenü für alle Benutzer (%systemdrive%\ProgramData\Microsoft\Windows\Start \Programme)** angezeigt wird. Auch sicherstellen Sie, dass das Symbol, das Sie im **Startmenü** finden Sie unter was Benutzern angezeigt werden sollen. Wenn dies nicht der Fall ist, ändern Sie ihn. (Dies nicht *haben* die Anwendung bis zum Anfang hinzufügen Menü, aber es vereinfacht viel in RemoteApp die Anwendung veröffentlichen. Andernfalls müssen Sie den Installationspfad der Anwendung bereitstellen, wenn Sie die app veröffentlichen.)


8.  Führen Sie eine zusätzlichen Windows Konfigurationen nach Ihrer Anwendung erforderlich.
9.  Deaktivieren Sie das Verschlüsselung File System (EFS). Führen Sie den folgenden Befehl in ein Befehlsfenster erhöhten aus:

        Fsutil behavior set disableencryption 1

    Alternativ können Sie festlegen oder fügen Sie den folgenden DWORD-Wert in der Registrierung hinzu:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Wenn Sie das Bild innerhalb einer Azure-virtuellen Computern erstellen, Umbenennen der ** \%windir%\Panther\Unattend.xml** ablegen, da hierdurch das Upload-Skript später von Arbeit verwendet verhindert werden. Ändern Sie den Namen dieser Datei zu Unattend.old, so dass Sie weiterhin die Datei werden für den Fall, dass Sie Ihre Bereitstellung wiederherstellen müssen.
10. Wechseln Sie zur Windows Update, und installieren Sie alle wichtigen Updates. Möglicherweise müssen Sie Windows Update ausführen mehrmals, um alle Updates zu erhalten. (Manchmal installieren Sie ein Update, und das Update selbst erfordert ein Update).
10. SYSPREP das Bild. Führen Sie bei ein erweitertes Eingabeaufforderungsfenster den folgenden Befehl aus:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe/Shutdown**

    **Hinweis:** Verwenden Sie den Schalter **/mode:vm** des Befehls SYSPREP nicht aus, obwohl dies eines virtuellen Computers ist.


## <a name="next-steps"></a>Nächste Schritte ##
Jetzt, da Sie eine benutzerdefinierte Vorlagenbild haben, müssen Sie dieses Bild in Ihre Sammlung RemoteApp hochladen. Verwenden Sie die Informationen in den folgenden Artikeln, um Ihre Sammlung zu erstellen:


- [So erstellen Sie eine Auflistung Hybrid RemoteApp](remoteapp-create-hybrid-deployment.md)
- [So erstellen Sie eine Cloud-Sammlung von RemoteApp](remoteapp-create-cloud-deployment.md)
 