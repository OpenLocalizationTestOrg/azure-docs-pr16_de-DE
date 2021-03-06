<!--author=alkohli last changed: 9/17/15-->

#### <a name="to-complete-the-minimum-storsimple-device-setup"></a>Zum Abschließen der minimalen StorSimple Gerät

1. Klicken Sie auf der Seite **Geräte** wählen Sie das Gerät, klicken Sie auf den Pfeil neben dem Gerätenamen, um zu der Seite auf bestimmte Geräte zu wechseln. 

    ![Seite Geräte mit Gerät online](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png) 

2. Klicken Sie auf Schnellstart-Symbol ![Symbolleiste starten Symbol](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) auf die Seite Gerät Schnellstart zuzugreifen. Klicken Sie auf die **vollständige Einrichtung einrichten** , um den Assistenten zum **Konfigurieren von Geräten** zu starten.

    ![Schnellstart-Seite auf Geräte](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)

2. Klicken Sie auf der Einstellungsseite **Grundlegende** folgendermaßen Sie vor:
  1. Geben Sie einen **Anzeigenamen** für Ihr Gerät. Der Standardnamen Gerät widerspiegeln Informationen, wie etwa die Gerätemodell und die fortlaufende Zahl. Sie können einen Anzeigenamen von bis zu 64 Zeichen zum Verwalten von Ihrem Geräts zuweisen.
  2. Festlegen der **Zeitzone** basierend auf die geografische Position in der das Gerät bereitgestellt wird. Ihr Gerät wird dieser Zeitzone für alle geplanten Vorgängen verwendet werden.
  3. Klicken Sie unter **DNS-Einstellungen**Geben Sie eine Adresse für Ihren **Sekundären DNS-Server**aus. Wenn Sie IPv6 verwenden, wird das Feld basierend auf dem IPv6-Präfix in der Windows PowerShell-Schnittstelle bereitgestellt ausgefüllt werden. 
  Wenn der sekundäre DNS-Server konfiguriert ist, werden Sie zum Speichern Ihrer Konfigurations Gerät nicht zulässig.
  4. Aktivieren Sie unter aktiviert iSCSI-Schnittstellen mindestens ein Netzwerk für iSCSI aus. Mindestens ein Netzwerk-Benutzeroberfläche in der Cloud aktiviert sein muss, und eine Benutzeroberfläche iSCSI aktiviert werden muss. Daten 0 ist automatisch Cloud-aktiviert.
 
      ![StorSimple minimalen Setup grundlegende des Audiogeräts](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)

3. Klicken Sie auf das Pfeilsymbol. ![Pfeilsymbol StorSimple](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)

4. Klicken Sie auf der Seite **Netzwerk-Schnittstellen** Erläutern Sie die festen IP-Adressen für Controller 0 und 1 Controller aus. Wenn die Daten 0 Benutzeroberfläche für IPv4 konfiguriert wurde, die festen IP-Adressen im IPv4-Format bereitgestellt werden müssen. Wenn Sie ein Präfix für IPv6-Konfiguration erhalten haben, werden automatisch die festen IP-Adressen in diese Felder ausgefüllt werden.


    > [AZURE.NOTE] 
    > 
    > - Der Controller feste IP-Adressen kostenlosen IP-Adressen im Subnetz zugegriffen werden, indem Sie die IP-Adresse des Geräts sein müssen.
    > - Die festen IP-Adressen für den Controller sind für die Wartung die Updates für das Gerät verwendet und daher die feste IP-Adressen muss geroutet und eine Verbindung mit dem Internet herstellen.

    ![Netzwerk-Schnittstellen für StorSimple minimalen Gerät einrichten](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

5. Klicken Sie auf das Symbol Kontrollkästchen ![StorSimple Kontrollkästchen Symbol](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
  Sie zu der Seite auf Geräte **Schnellstart** zurückgegeben werden kann.

 > [AZURE.NOTE] Sie können alle anderen Gerät-Einstellungen zu einem beliebigen Zeitpunkt ändern, durch den Zugriff auf der Seite **Konfigurieren** .

![Video verfügbar](./media/storsimple-complete-minimum-device-setup/Video_icon.png) **Video verfügbar**

Wenn Sie ein Video zur Verfügung, die veranschaulicht, wie Sie das minimale Gerät Setup abschließen, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/).