Nachdem die Einträge für Ihren Domänennamen verteilt haben, sollten Sie nicht mehr zur Verwendung Ihres Browsers, um sicherzustellen, dass Ihr benutzerdefinierter Domänenname verwendet werden kann, auf die Web-app im App-Verwaltungsdienst Azure zugreifen.

> [AZURE.NOTE] Es dauert einige Zeit für Ihre CNAME im DNS-System übertragen. Einen Dienst, wie z. B. <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der CNAME-Eintrag verfügbar ist.

Wenn Sie Web app als einen Endpunkt Datenverkehr Manager noch nicht hinzugefügt haben, müssen Sie dies tun, bevor Sie mit einer namensauflösung von verwendet werden kann, als der benutzerdefinierten Domäne Namen leitet in den Datenverkehr-Manager. Datenverkehr-Manager und dann leitet bei der Web-app. Verwenden Sie die Informationen in [Hinzufügen oder Löschen von Endpunkten](../articles/traffic-manager/traffic-manager-endpoints.md) Web app als einen Endpunkt in Ihrem Profil Datenverkehr Manager hinzufügen.

> [AZURE.NOTE] Wenn beim Hinzufügen von außen liegenden Tabellenblättern Web app nicht aufgeführt ist, stellen Sie sicher, dass sie für **Standard** -App-Verwaltungsdienst Plan Modus konfiguriert ist. Sie müssen **Standardmodus für Ihre Web app** verwenden, um mit den Datenverkehr Manager arbeiten.

1. Öffnen Sie in Ihrem Browser das [Azure-Portal](https://portal.azure.com)aus.

1. Klicken Sie auf der Registerkarte **Web Apps** auf den Namen der Web app, wählen Sie **Einstellungen**aus, und wählen Sie dann auf **benutzerdefinierte Domänen**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. Klicken Sie in das Blade **benutzerdefinierte Domänen** auf **Hostname hinzufügen**.
    
1. Verwenden Sie die **Hostname** Textfelder, um den Datenverkehr Manager Domänennamen zuordnen? diese Web app eingeben.

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Klicken Sie auf **Überprüfen** , um die Konfiguration der Domäne Namen speichern.

7.  Beim Klicken auf die **Gültigkeit** Azure wird deaktivieren Domäne Überprüfung Workflow Starten eines. Hiermit wird für Besitzer der Domäne als auch Hostname Verfügbarkeit und Bericht Erfolg oder detaillierte Fehler mit Nachschlagewerke Guidence zum Beheben des Fehlers überprüft.    

8.  Bei einer erfolgreichen Validierung **Hinzufügen Hostname** Schaltfläche wird aktiv und Sie können die Hostname zuweisen. Navigieren Sie nun zu Ihren benutzerdefinierten Domänennamen in einem Browser. Ihre app-Ausführung mit Ihren benutzerdefinierten Domänennamen sollte jetzt angezeigt werden. 

    Nach Abschluss der Konfiguration wird der benutzerdefinierten Domänennamen im Abschnitt **Domänennamen** der Web app aufgeführt sein.

An diesem Punkt sollte es möglich sein, geben den Datenverkehr Manager Namen Domänennamen in Ihrem Browser, und sehen, dass Sie dieses erfolgreich Web app hat.
