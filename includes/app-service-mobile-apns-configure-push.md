

1. Starten Sie auf Ihrem Mac **Schlüsselbund**ein. Öffnen Sie auf der linken Navigationn Leiste **Eigene Zertifikate** unter **Kategorie** ein. Suchen Sie das SSL-Zertifikat, das Sie im vorherigen Abschnitt heruntergeladen haben und Zwecken Sie deren Inhalt. Wählen Sie nur das Zertifikat (Markieren Sie dabei nicht den privaten Schlüssel), und [es exportieren](https://support.apple.com/kb/PH20122?locale=en_US).

2. Klicken Sie im [Azure-Portal](https://portal.azure.com/)auf **Alle durchsuchen** > **App Services** > Ihre Mobile-App Back-End. Klicken Sie unter **Einstellungen**auf **App Dienst Pushbenachrichtigungen**klicken Sie auf die Namen der Benachrichtigung Hub. Wechseln Sie zu **Apple Pushbenachrichtigungen Benachrichtigungsdienst** > **Zertifikat hochladen**. Hochladen der P12-Datei, die Auswahl des richtigen **Modus** (je nachdem, ob Ihr Client SSL Zertifikat aus einer früheren Version ist Herstellung oder Sandkasten-). Alle Änderungen zu speichern.

Der Dienst ist jetzt Zusammenarbeit mit Pushbenachrichtigungen unter iOS konfiguriert!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
