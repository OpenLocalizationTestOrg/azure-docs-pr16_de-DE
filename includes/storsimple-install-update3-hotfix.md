<!--author=alkohli last changed: 09/01/16-->

#### <a name="to-download-hotfixes"></a>Herunterladen von Updates

Führen Sie die folgenden Schritte aus, um Softwareupdates von Microsoft Update-Katalog herunterladen.

1. Starten Sie Internet Explorer, und navigieren Sie zu [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Ist dies zum ersten Mal Microsoft Update-Katalog auf diesem Computer verwenden, klicken Sie auf **Installieren** , wenn Sie aufgefordert werden, das Add-on Microsoft Update-Katalog zu installieren.
    ![Installieren der Katalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. Geben Sie in das Suchfeld der Microsoft Update-Katalog die Anzahl der Knowledge Base (KB) des Updates, beispielsweise **3186843**, heruntergeladen werden sollen, und klicken Sie dann auf **Suchen**.

    Die Update-Auflistung angezeigt wird, beispielsweise **Kumulierte Software Paket Update 3.0 8000 ohne Finanzierungskosten oder StorSimple**.

    ![Search-Katalog](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)

4. Klicken Sie auf **Hinzufügen**. Das Update wird der Warenkorb hinzugefügt.

5. Suchen nach einem beliebigen zusätzlichen Updates aufgeführt, die in der Tabelle oben (**3186859**), und fügen jeder zum Warenkorb.

5. Klicken Sie auf **Auswahlkorb anzeigen**.

6. Klicken Sie auf **herunterladen**. Geben Sie oder **Navigieren** Sie zu einem lokalen Speicherort, in dem die Downloads angezeigt werden soll. Die Updates werden in der angegebenen Position heruntergeladen und mit demselben Namen wie die Aktualisierung in einem Unterordner platziert. Der Ordner kann auch auf eine Netzwerkfreigabe kopiert werden, die vom Gerät erreichbar ist.

>   [AZURE.NOTE]
Müssen beide Controller mögliche Fehlermeldungen vom Peer-Controller erkennen verfügbare Updates.

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Installieren und normalen Modus Updates überprüfen

Führen Sie die folgenden Schritte aus, zum Installieren und normale-Modus Updates überprüfen. Wenn Sie bereits über das Azure-Portal installiert haben, fahren Sie mit [Installieren und Wartung Modus Updates überprüfen](#to-install-and-verify-maintenance-mode-hotfixes).

1. Zugreifen Sie auf der Windows PowerShell-Benutzeroberfläche an Ihre StorSimple Gerät seriellen Konsole zum Installieren von Updates. Anweisungen Sie die ausführliche in [Kitten verwenden, mit der seriellen Konsole verbinden](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Drücken Sie die **EINGABETASTE**, an der Befehlszeile.

4. Wählen Sie **Option 1** , um das Gerät mit Vollzugriff sich anzumelden. Es empfiehlt sich, dass Sie das Update auf dem passiven Controller installieren.

5. Um das Update, an der Befehlszeile zu installieren, geben Sie Folgendes ein:

    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`

    Verwenden Sie IP-anstelle von DNS-Einträge in den Pfad für die Freigabe im obigen Befehl ein. Credential-Parameter dient nur, wenn Sie eine authentifizierte Freigabe zugreifen.

    Es empfiehlt sich, dass Sie den Credential-Parameter verwenden, um Freigaben zugreifen. Gerade Freigaben, die auf "Jeder" geöffnet sind, sind nicht in der Regel für nicht authentifizierte Benutzer geöffnet.

    Geben Sie das Kennwort ein.

    Nachfolgend finden Sie eine Beispiel für die Ausgabe.

        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John

        Confirm

        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y

        ````

6. Geben Sie **Y** , um die Update-Installation zu bestätigen.

7. Überwachen die Aktualisierung mithilfe der `Get-HcsUpdateStatus` Cmdlet. Die Aktualisierung wird auf dem passiven Controller zuerst abzuschließen. Nachdem der passive Controller aktualisiert wird, werden ein Failover und das Update wird dann auf dem anderen Controller angewendet abrufen. Die Aktualisierung ist abgeschlossen, wenn beide der Controller aktualisiert werden.

    Folgender Ausgabe zeigt das Update wird ausgeführt. Die `RunInprogress` werden `True` bei die Aktualisierung wird ausgeführt.

        ````
        Controller0>Get-HcsUpdateStatus
        RunInprogress       : True
        LastHotfixTimestamp :
        LastUpdateTimestamp : 8/29/2016 2:04:02 AM
        Controller0Events   :
        Controller1Events   :

        ````

     Der folgende Beispielausgabe zeigt an, dass die Aktualisierung abgeschlossen ist. Die `RunInProgress` werden `False` Wenn die Aktualisierung abgeschlossen ist.

        ````
        Controller0>Get-HcsUpdateStatus
        RunInprogress       : False
        LastHotfixTimestamp : 8/30/2016 9:15:55 AM
        LastUpdateTimestamp : 8/30/2016 9:06:07 AM
        Controller0Events   :
        Controller1Events   :


        ````

    > [AZURE.NOTE] Gelegentlich, die Cmdlet-Berichte `False` Wenn das Update noch in Bearbeitung ist. Um sicherzustellen, dass das Update abgeschlossen ist, warten Sie einige Minuten, führen Sie diesen Befehl erneut aus, und überprüfen Sie, ob die `RunInProgress` ist `False`. Es ist, hat das Update abgeschlossen.

8. Nach der Software Aktualisierung abgeschlossen ist, überprüfen die System-Software-Versionen. Type:

    `Get-HcsSystem`

    Sie sollten die folgenden Versionen finden Sie unter:

    - `HcsSoftwareVersion: 6.3.9600.17759`
    - `CisAgentVersion:  1.0.9343.0`
    - `MdsAgentVersion: 30.0.4698.16`

    Wenn die Versionsnummern nach dem Anwenden der Aktualisierung nicht ändern, gibt es an, dass das Update fehlgeschlagen ist, angewendet. Sie sollten dies mit dem [Microsoft-Support](storsimple-contact-microsoft-support.md) wenden Sie sich wegen weiterer Unterstützung an sehen.
    
    > [AZURE.IMPORTANT] Sie müssen über den aktiven Controller neu starten, die `Restart-HcsController` Cmdlet vor Anwenden der verbleibenden aktualisiert. 

9. Wiederholen Sie die Schritte 3 bis 5, um die LSI-Treiber und Firmware Update **KB3186859**installiert haben. Nachdem das Update installiert ist, verwenden Sie die `Get-HcsSystem` Cmdlet. Die Version LSI sollten:

    - `Lsisas2Version: 2.0.78.00`
    
10. Wiederholen Sie die Schritte 3 bis 5, um das Update Storport und Spaceport **KB3121261**zu installieren.


11. Wenn Sie die Aktualisierung von Update 2 oder frühere Version durchführen, müssen Sie auch herunterladen:

    - iSCSI Fix KB3146621 verwenden

    - WMI beheben KB3103616 verwenden



#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Installieren und Wartung Modus Updates überprüfen

Verwenden von KB3121899 Datenträger Firmwareupdates installieren. Diese Unterbrechung Updates werden und in Anspruch nehmen ungefähr 30 Minuten. Sie können diese Elemente in einem Fenster der geplanten Wartung durch Herstellen einer Verbindung mit der seriellen Gerät-Konsole installieren.

Beachten Sie, dass ist die Datenträger Firmware bereits auf dem neuesten Stand, Sie nicht benötigen diese Updates zu installieren. Führen Sie die `Get-HcsUpdateAvailability` Cmdlet aus der Gerät seriellen Konsole prüfen, ob Updates verfügbar sind, und gibt an, ob die Updates Unterbrechung sind (Wartungsmodus) oder Updates ohne Unterbrechung (normalen Modus).

Gehen Sie wie folgt vor um den Datenträger Firmwareupdates installieren.

1. Platzieren Sie das Gerät im Modus Wartung. Beachten Sie, dass Sie Windows PowerShell Remote nicht verwenden sollten, bei der Verbindung mit einem Gerät im Modus Wartung. Führen Sie stattdessen mit diesem Cmdlet auf dem Gerätecontroller Wenn über das Gerät serielle Konsole verbunden. Type:

    `Enter-HcsMaintenanceMode`

    Nachfolgend finden Sie eine Beispiel für die Ausgabe.

        Controller0>Enter-HcsMaintenanceMode
        Checking device state...

        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y

        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------

        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>

    Sowohl die Controller starten in Wartungsmodus klicken Sie dann erneut.

3. Um die Aktualisierung der Firmware zu installieren, geben Sie Folgendes ein:

    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`

    Nachfolgend finden Sie eine Beispiel für die Ausgabe.

        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.

1.  Überwachen der Installation des Vorgangsfortschritts mit `Get-HcsUpdateStatus` Befehl. Die Aktualisierung wurde abgeschlossen, wann die `RunInProgress` ändert sich in `False`.

2.  Nach Abschluss die Installation installiert der Controller, auf dem das Wartung Modus Update wurde, neu gestartet wurde. Melden Sie sich als Option 1 mit Vollzugriff und überprüfen Sie die Datenträger Firmwareversion. Type:

    `Get-HcsFirmwareVersion`

    Die erwarteten Datenträger Firmwareversionen sind:

    `XMGG, XGEG, KZ50, F6C2, VR08`

    Nachfolgend finden Sie eine Beispiel für die Ausgabe.

        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17705
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller1
        ---------------------------------------------------------------

        Controller1>Get-HcsFirmwareVersion

        Controller0 : TalladegaFirmware
          ActiveBIOS:0.45.0006
          BackupBIOS:0.45.0008
          MainCPLD:17.0.0005
          ActiveBMCRoot:2.0.000E
          BackupBMCRoot:2.0.000E
          BMCBoot:2.0.0001
          LsiFirmware:19.00.00.00
          LsiBios:07.37.00.00
          Battery1Firmware:06.29
          Battery2Firmware:06.29
          DomFirmware:X231600
          CanisterFirmware:3.5.0.32
          CanisterBootloader:5.03
          CanisterConfigCRC:0xD1B030A4
          CanisterVPDStructure:0x06
          CanisterGEMCPLD:0x17
          CanisterVPDCRC:0xEE3504B4
          MidplaneVPDStructure:0x0C
          MidplaneVPDCRC:0xA6BD4F64
          MidplaneCPLD:0x10
          PCM1Firmware:1.00|1.05
          PCM1VPDStructure:0x05
          PCM1VPDCRC:0x41BEF99C
          PCM2Firmware:1.00|1.05
          PCM2VPDStructure:0x05
          PCM2VPDCRC:0x41BEF99C

          DisksFirmware
          SEAGATE:ST400FM0073:XGEG
          SEAGATE:ST400FM0073:XGEG
          SEAGATE:ST400FM0073:XGEG
          SEAGATE:ST400FM0073:XGEG
          SEAGATE:ST4000NM0023:XMGG
          SEAGATE:ST4000NM0023:XMGG
          SEAGATE:ST4000NM0023:XMGG
          SEAGATE:ST4000NM0023:XMGG
          SEAGATE:ST4000NM0023:XMGG
          SEAGATE:ST4000NM0023:XMGG
          SEAGATE:ST4000NM0023:XMGG
          SEAGATE:ST4000NM0023:XMGG

     Führen Sie die `Get-HcsFirmwareVersion` Befehl auf dem zweiten Controller zu überprüfen, ob die Softwareversion aktualisiert wurde. Sie können den Wartungsmodus beenden. Geben Sie hierzu den folgenden Befehl für jeden Gerätecontroller aus:

    `Exit-HcsMaintenanceMode`

1. Der Controller neu zu starten, wenn Sie Wartungsmodus zu beenden. Nachdem die Festplatten-Software Updates erfolgreich angewendet, und das Gerät Wartungsmodus klassischen Azure-Portal zurück beendet wurde. Beachten Sie, dass im Portal möglicherweise nicht anzeigen, dass Sie die Wartung Modus Updates für 24 Stunden installiert haben.
