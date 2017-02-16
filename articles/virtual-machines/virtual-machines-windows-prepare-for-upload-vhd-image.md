<properties
    pageTitle="Vorbereiten einer virtuellen Windows Azure hochladen | Microsoft Azure"
    description="Empfohlene Vorgehensweisen bei der Vorbereitung einer virtuellen Windows vor dem Hochladen auf Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Vorbereiten einer virtuellen Windows Azure hochladen
Zum Hochladen eines Windows virtuellen Computers aus lokalen in Azure, müssen Sie die virtuelle Festplatte (virtuelle Festplatte) ordnungsgemäß vorbereiten. Es gibt mehrere empfohlene Maßnahmen, die für Sie abgeschlossen werden, bevor Sie eine virtuelle Festplatte in Azure hochladen aus. In diesem Artikel wird gezeigt, wie eine Windows virtuelle Festplatte Hochladen in Microsoft Azure vorbereiten und außerdem erfahren Sie, [wann und wie Sie Sysprep verwenden](#step23).

## <a name="prepare-the-virtual-disk"></a>Vorbereiten des virtuellen Laufwerks

>[AZURE.NOTE] 
> Azure unterstützt nur [Generation 1 virtuellen Computern](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) , die im Dateiformat virtuelle Festplatte sind. Das neuere VHDX-Format wird in Azure nicht unterstützt. 
>
> Die virtuelle Festplatte muss eine feste Größe, nicht dynamische. Bei Bedarf Einzelheitenarten aufgeführten Schritte ausführen von VHDX oder dynamische Festplatten konvertieren. Die maximale zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB.


Stellen Sie sicher, dass die Windows-virtuelle Festplatte auf dem lokalen Server ordnungsgemäß arbeitet. Beheben von Fehlern in den virtuellen Computer selbst bevor Sie versuchen, konvertieren oder in Azure hochladen.

Wenn Sie Ihre virtuellen Datenträger in das erforderliche Format für Azure konvertieren müssen, verwenden Sie eine der Methoden in den folgenden Abschnitten notiert haben. Sichern Sie den virtuellen Computer aus, bevor Sie eine beliebige virtuelle Laufwerk Konvertierung zu oder Sysprep ausführen.

### <a name="convert-using-hyper-v-manager"></a>Die Konvertierung mit Hyper-V-Manager
- Öffnen Sie Hyper-V-Manager zu, und wählen Sie aus Ihrem lokalen Computer auf der linken Seite. Klicken Sie auf **Vorgang**, klicken Sie im Menü darüber > **Datenträger bearbeiten**.
    - Klicken Sie auf dem Bildschirm **virtuelle Festplatte suchen** navigieren Sie zu, und wählen Sie das Laufwerk.
    - Wählen Sie auf dem nächsten Bildschirm **Konvertieren**
        - Wenn Sie von VHDX konvertieren müssen, wählen Sie **virtuelle Festplatte** aus und klicken Sie auf **Weiter**
        - Wenn Sie vom dynamische Datenträger konvertieren müssen, wählen Sie **feste Größe** aus, und klicken Sie auf **Weiter**

    - Navigieren Sie zu, und wählen Sie **Pfad für die neue virtuelle Festplatte Datei**.
    - Klicken Sie auf **Fertig stellen** , um zu schließen.

### <a name="convert-using-powershell"></a>Konvertieren Sie mithilfe der PowerShell
Sie können eine virtuelle Festplatte mithilfe des [Konvertieren-virtuelle Festplatte PowerShell-Cmdlet](http://technet.microsoft.com/library/hh848454.aspx)konvertieren. Im folgenden Beispiel werden wir aus einer VHDX in virtuelle Festplatte konvertieren und aus einem dynamischen in feste Typ zu konvertieren:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Konvertieren von VMware VMDK Datenträger formatieren
Wenn Sie ein Bild virtuellen Windows-Computer im [VMDK-Dateiformat](https://en.wikipedia.org/wiki/VMDK)verfügen, konvertieren Sie sie in eine virtuelle Festplatte mit dem [Microsoft-virtuellen Computern Konverter](https://www.microsoft.com/download/details.aspx?id=42497). Blog [zum Konvertieren einer VMware VMDK zu Hyper-V virtuelle Festplatte](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) für Weitere Informationen zu lesen.

## <a name="prepare-windows-configuration-for-upload"></a>Vorbereiten der Windows-Konfiguration für hochladen

> [AZURE.NOTE] Führen Sie alle folgenden Befehle mit [Administratorrechten](https://technet.microsoft.com/library/cc947813.aspx)an.

1. Entfernen Sie alle statischen beständigen Routing für die Weiterleitung Tabelle an:

    - Führen Sie zum Anzeigen der Routingtabelle `route print`.
    - Aktivieren Sie in den Abschnitten **Beibehaltung weitergeleitet** . Ist ein beständiger weiterleiten, verwenden Sie [Routing löschen](https://technet.microsoft.com/library/cc739598.apx) um zu entfernen.

2. Entfernen des WinHTTP Proxys an:

    ```
    netsh winhttp reset proxy
    ```

3. Konfigurieren Sie die Festplatte SAN Richtlinie [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

    ```
    diskpart san policy=onlineall
    ```

4. Verwenden Sie Coordinated Universal Time (UTC) für Windows, und legen Sie den Starttyp des Diensts Windows-Zeitgeber (w32time) auf **automatisch**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Konfigurieren von Windows-Diensten
5. Stellen Sie sicher, dass die folgenden Windows-Dienste die **Standardwerte für Windows**festgelegt ist. Sie werden mit den starteinstellungen in der nachstehenden Liste erwähnt konfiguriert. Sie können diese Befehle zum Zurücksetzen der starteinstellungen ausführen:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Konfigurieren von Remote Desktop-Konfiguration
6. Wenn alle selbstsignierten Zertifikaten, um die Zuhörer (Remotedesktopprotokoll) verknüpft sind, entfernen Sie sie aus:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Weitere Informationen zum Konfigurieren von Zertifikaten für RDP Zuhörer finden Sie unter [Zuhörer Zertifikat Konfigurationen in Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. Konfigurieren Sie die [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) Werte für RDP-Dienst:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Konfigurieren Sie den Authentifizierungsmodus für den Dienst RDP an:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Aktivieren Sie RDP-Dienst, indem Sie die folgenden Unterschlüssel zur Registrierung hinzufügen:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Konfigurieren der Windows-Firewall-Regeln
10. Zulassen, dass WinRM über die drei Firewallprofile (Domäne, privat und öffentlich), und aktivieren Sie PowerShell Remote-Dienst:

    ```
    Enable-PSRemoting -force
    ```

11. Stellen Sie sicher, dass die folgenden Gast Betriebssystem Firewallregeln vorhanden sind:

    - Eingehende

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Eingehende und ausgehende

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Ausgehende

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Zusätzliche Konfigurationsschritte für Windows
12. Führen Sie `winmgmt /verifyrepository` um zu bestätigen, dass das Windows Management Verwaltungsinstrumentation Repository konsistent ist. Wenn Repository beschädigt ist, lesen Sie [diesen Blogbeitrag](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not)aus.

13. Stellen Sie sicher, dass die Boot Konfiguration Daten (BCD)-Einstellungen die folgenden Kriterien entsprechen:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Entfernen Sie zusätzliche Transport Treiber Interface Filter, wie z. B. Software, die analysiert TCP-Pakete.
15. Um sicherzustellen, dass der Datenträger ist fehlerfrei und konsistente, führen Sie die `CHKDSK /f` Befehl.
16. Deinstallieren Sie alle anderen Drittanbieter-Software und im Zusammenhang mit physischen Komponenten oder einer anderen Virtualisierungstechnologie-Treiber.
17. Stellen Sie sicher, dass eine Drittanbieter-Anwendung nicht Port 3389 verwendet wird. Dieser Port ist für den Dienst RDP in Azure verwendet. Sie können die `netstat -anob` Befehl aus, um die Ports zu überprüfen, die von den Clientanwendungen verwenden.
18. Ist die Windows-virtuelle Festplatte, die Sie hochladen möchten eine Domänencontroller, führen Sie [zusätzliche Schritte](https://support.microsoft.com/kb/2904015) , um das Laufwerk vorbereiten.
19. Neustart den virtuellen Computer, um sicherzustellen, dass Windows noch vorhanden ist, kann mithilfe von RDP-Verbindung erreicht werden.
20. Zurücksetzen Sie des aktuellen lokalen Administratorkennworts, und stellen Sie sicher, dass Sie dieses Konto verwenden können, bis die RDP-Verbindung bei Windows anmelden.  Diese Zugriffsberechtigung wird durch die Richtlinie "Anmelden über Remote Desktop Services zulassen" Objekt gesteuert. Dieses Objekt befindet sich unter "Computer-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten."


## <a name="install-windows-updates"></a>Installieren von Windows-Updates
22. Installieren Sie die neuesten Updates für Windows. Wenn das nicht möglich ist, stellen Sie sicher, dass die folgenden Updates installiert sind:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure-virtuellen Computern nicht über ein Netzwerkausfall wiederherstellen und Probleme mit beschädigten Daten auftreten.

    - [KB3115224](https://support.microsoft.com/kb/3115224) Verbesserte Zuverlässigkeit für virtuellen Computern, die auf einem Windows Server 2012 R2 oder Windows Server 2012-Server ausgeführt werden

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows eine Adresse Erhöhung von Berechtigungen: 8 März 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Viele ID 129 Ereignisse sind beim Ausführen von Windows Server 2012 R2 virtuellen Computers in Microsoft Azure angemeldet

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure-virtuellen Computern nicht über ein Netzwerkausfall wiederherstellen und Probleme mit beschädigten Daten auftreten.

    - [KB3114025](https://support.microsoft.com/kb/3114025) Beim Zugriff auf Dateien Azure-Speicher von Windows 8.1 oder Server 2012 R2 langsam

    - [KB3033930](https://support.microsoft.com/kb/3033930) Update erhöht die Grenze von 64 KB für RIO Puffer pro Prozess für Windows Azure service

    - [KB3004545](https://support.microsoft.com/kb/3004545) Sie können nicht virtuellen Computern zugreifen, die auf über ein VPN-Verbindung in Windows Azure Hostingdienste gehostet werden

    - [KB3082343](https://support.microsoft.com/kb/3082343) Cross lokale VPN-Konnektivität geht verloren, wenn Azure Standort-zu-Standort VPN-Tunnel Windows Server 2012 R2 RRAS verwenden

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows eine Adresse Erhöhung von Berechtigungen: 8 März 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Beschreibung des Sicherheitsupdates für CSRSS: 12 April 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) System stürzt ab, während die Festplatte in Windows<a id="step23"></a>
23. Wenn Sie ein Bild, um es mehreren Computern bereitstellen erstellen möchten, müssen Sie das Bild durch Ausführen generalize `sysprep` , bevor Sie die virtuelle Festplatte auf Azure hochladen. Sie müssen nicht ausführen `sysprep` für eine spezialisierte virtuelle Festplatte verwenden. Weitere Informationen zum Erstellen eines Bildes GRG finden Sie unter den folgenden Artikeln:

    - [Erstellen Sie ein Bild virtueller Computer aus einer vorhandenen Azure virtueller Computer mit dem Modell zur Bereitstellung von Ressourcenmanager](virtual-machines-windows-create-vm-generalized.md)
    - [Erstellen Sie ein Bild virtueller Computer aus einer vorhandenen Azure virtueller Computer mit dem klassischen Bereitstellung](virtual-machines-windows-classic-capture-image.md)
    - [Sysprep-Unterstützung für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Zusätzliche Vorschläge zu Konfigurationen

Die folgenden Einstellungen beeinflussen nicht virtuelle Festplatte hochladen. Jedoch wird dringend empfohlen, dass Sie diese konfiguriert haben.

- Installieren Sie den [Agent Azure-virtuellen Computern](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)an. Nach der Installation des Agents können Sie die Erweiterungen virtueller Computer aktivieren. Die virtuellen Computer Erweiterungen implementieren die meisten kritischen Funktionen, die Sie mit Ihrem virtuellen Computern wie das Zurücksetzen von Kennwörtern, RDP konfigurieren verwenden möchten, und viele andere.

- Das Protokoll Abbild kann in Behandeln von Problemen mit Windows Absturz hilfreich sein. Aktivieren Sie das Abbild Log-Websitesammlung:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Nach der Erstellung des virtuellen Computer in Azure konfigurieren Sie die Systemauslagerungsdatei für definierte Größe auf Laufwerk d:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Nächste Schritte

- [Hochladen eines Bilds Windows virtueller Computer in Azure für Ressourcenmanager Bereitstellungen](virtual-machines-windows-upload-image.md)
