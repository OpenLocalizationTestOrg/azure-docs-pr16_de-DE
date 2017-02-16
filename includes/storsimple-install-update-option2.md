<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>So installieren Sie Update 1.2 vom klassischen Azure-portal

1. Im Portal Azure klassischen wechseln Sie zur Seite **Geräte** , und wählen Sie Ihr Gerät.

2. Navigieren Sie zu **Geräte** > **Konfigurieren**.

3. **Netzwerk-Schnittstellen**zuerst überprüfen Sie unter aktuell mindestens ein Netzwerk-Benutzeroberfläche, die iSCSI aktiviert ist. Suchen Sie dann die Schnittstelle (ohne Daten 0), die einen Gateway zugewiesen hat.

4. Deaktivieren der Schnittstelle, die einen zugeordneten Gateway enthält, und speichern Sie die geänderte Konfiguration. Beachten Sie die Einstellungen im Netzwerk Schnittstelle verbleiben, und daher müssen, wenn Sie diese Netzwerkschnittstelle später wieder aktivieren, im Portal wird zurückgesetzt an den ursprünglichen Einstellungen.

7. Sie können jetzt [Verwenden des Azure klassischen-Portals um Update 1.2 zu installieren](#install-update-12-via-the-azure-classic-portal). Anweisungen Sie beginnend mit Schritt 3 fort. Nachdem Sie alle Updates installiert haben, können Sie die Netzwerk-Benutzeroberfläche wieder aktivieren, die Sie deaktiviert.
