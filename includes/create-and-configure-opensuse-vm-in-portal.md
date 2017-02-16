<properties writer="cynthn" editor="tysonn" manager="timlt" />

1. Melden Sie sich zum [Azure klassischen Portal](http://manage.windowsazure.com)aus.  

2. Klicken Sie auf der Befehlsleiste am unteren Rand des Fensters auf **neu**.

3. Klicken Sie unter **zu berechnen**klicken Sie auf **virtuellen Computern**, und klicken Sie dann auf **Aus Galerie**.

    ![Erstellen eines neuen virtuellen Computers][Image1]

4. Klicken Sie unter der Gruppe **SUSE** wählen Sie OpenSUSE virtuellen Computerabbild aus, und klicken Sie dann auf den Pfeil, um den Vorgang fortzusetzen.

5. Klicken Sie auf der ersten Seite der **Konfiguration des virtuellen Computers** :

    - Geben Sie einen **Name des virtuellen Computers**, wie etwa "Testlinuxvm". Der Name muss zwischen 3 und 15 Zeichen enthalten, können nur Buchstaben, Zahlen und Bindestriche, enthalten und muss mit einem Buchstaben beginnen und enden mit einen Buchstaben oder eine Zahl.

    - Überprüfen Sie die **Ebene** aus, und wählen Sie eine **Größe**aus. Die Ebene bestimmt, die Größen, die, denen Sie auswählen können. Die Größe wirkt sich auf die Kosten der Verwendung, sowie Konfigurationsoptionen wie wie viele Daten Datenträger Sie anfügen können. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](../articles/virtual-machines-linux-sizes.md).
    - Geben Sie einen **Neuen Benutzernamen ein**, oder übernehmen Sie den Standardnamen **Azureuser**. Dieser Name wird auf die Datei Sudoers hinzugefügt.
    - Entscheiden Sie, welche Art von **Authentifizierung** verwendet. Allgemeine Kennwortrichtlinien finden Sie unter [sicherer Kennwörter](http://msdn.microsoft.com/library/ms161962.aspx).

6. Klicken Sie auf der nächsten Seite der **Konfiguration des virtuellen Computers** :

    - Verwenden Sie die Standardeinstellung **Erstellen einer neuen Cloud-Dienst**an.
    - Namen Sie im **DNS-Name** einen eindeutigen DNS als Teil der Adresse, beispielsweise "Testlinuxvm" verwendet.
    - Wählen Sie im Feld **Region Zugehörigkeit/Gruppe/virtuellen Netzwerk** einen Bereich, in dem diese Abbildung virtuellen gehostet werden soll.
    - Lassen Sie den Endpunkt SSH unter **Endpunkte**. Sie können andere nun hinzufügen oder hinzufügen, ändern oder löschen sie nach der Erstellung des virtuellen Computers.

    >[AZURE.NOTE] Wenn Sie eine virtuellen Computern ein virtuelles Netzwerk verwenden möchten, angeben **müssen** Sie das virtuelle Netzwerk, beim Erstellen des virtuellen Computers. Sie können kein virtuellen Computers nach dem Erstellen des virtuellen Computers zu einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Übersicht über Virtual Netzwerk](virtual-networks-overview.md).

7.  Klicken Sie auf der letzten Seite **Konfiguration des virtuellen Computers** behalten Sie die Standardeinstellungen, und klicken Sie dann auf das Häkchen auf Fertig stellen.

Im Portal Listet die neuen virtuellen Computern unter **virtuelle Computer**an. Während der Status als **(Provisioning)**angezeigt wird, wird der virtuellen Computern eingerichtet wird. Wenn der Status als **ausgeführt**angezeigt wird, können Sie mit dem nächsten Schritt verschieben.

##<a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung des virtuellen Computers mit

Sie verwenden SSH oder kitten Verbindung zum des virtuellen Computers, je nach Betriebssystem auf dem Computer, die Sie aus eine Verbindung herstellen können:

- Verwenden Sie von einem Computer unter Linux SSH aus. Geben Sie an der Befehlszeile ein:

    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`

    Geben Sie das Kennwort des Benutzers ein.

- Verwenden Sie von einem Computer unter Windows kitten aus. Wenn nicht installiert haben, können Sie ihn von der [Downloadseite kitten][PuTTYDownload].

    Speichern Sie **putty.exe** in ein Verzeichnis auf Ihrem Computer. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu diesem Ordner führen Sie **putty.exe aus**.

    Geben Sie den Hostnamen ein, beispielsweise "testlinuxvm.cloudapp.net", und geben Sie für den **Port**"22".

    ![Bildschirm puTTY][Image6]  

##<a name="update-the-virtual-machine-optional"></a>Aktualisieren des virtuellen Computers (optional)

1. Nachdem Sie mit der virtuellen Computern verbunden sind, können Sie optional System-Updates und Patches installieren. Wenn das Update ausführen möchten, geben Sie Folgendes ein:

    `$ sudo zypper update`

2. Wählen Sie **Software**und dann auf **Online aktualisieren** , um die Liste der verfügbaren Updates. Wählen Sie **akzeptieren** , um die Installation zu starten und Patch alle verfügbaren (mit Ausnahme der optionalen Netzwerken) aus.

3. Nach der Installation abgeschlossen ist, wählen Sie **Ende**aus.  Ihr System ist jetzt auf dem neuesten Stand.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
