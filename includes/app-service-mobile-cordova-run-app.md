
1. Besuchen Sie das [Azure-Portal]aus. Klicken Sie auf **Alle durchsuchen** > **Mobile-Apps** > die Back-End-, die Sie soeben erstellt haben. Klicken Sie in der mobilen app-Einstellungen auf **Schnellstart** > **Cordova**. Klicken Sie unter **Konfigurieren der Clientanwendung**wählen Sie **erstellen eine neue App**, und klicken Sie auf **herunterladen**. Diese downloads ein vollständiges Cordova Projekt für eine app, die vorab so konfiguriert, dass Ihre Back-End-Verbindung.

2. Entpacken Sie heruntergeladene ZIP-Datei in ein Verzeichnis auf Ihrer Festplatte, navigieren Sie zu der Lösungsdatei (.sln), und öffnen Sie sie in Visual Studio.

5. Wählen Sie in Visual Studio die Lösung-Plattform (Android, iOS oder Windows) aus den Dropdown-Pfeil neben den Start-Pfeil, und wählen Sie dann eine bestimmte Bereitstellungsgerät oder einen Emulator durch Klicken auf den Dropdown-Pfeil auf den grünen Pfeil. Beachten Sie, dass Sie die Android Standard-Plattform und die Welle Emulator verwenden können. Erweiterte Lernprogramme müssen Sie einen unterstützten Gerät oder Emulator auswählen. 

6. Drücken Sie F5, oder klicken Sie auf den grünen Pfeil, um zu erstellen und, und führen Sie die app Cordova. Wenn Sie ein Sicherheitsdialog in der Emulator anfordern des Zugriffs auf das Netzwerk angezeigt wird, akzeptieren Sie es.   

7. Nach der die app gestartet wird auf dem Gerät oder Emulator, geben Sie **neuen Text eingeben**, z. B. _vollständig des Lernprogramms_ aussagekräftigen Text, und klicken Sie dann auf die Schaltfläche **Hinzufügen** .  
Sendet eine POST-Anforderung an das Azure Back-End-, die, das Sie zuvor bereitgestellt. Die Back-End-fügt Daten aus der Anforderung werden in der Tabelle TodoItem in der SQL-Datenbank, und gibt Informationen über die neu gespeicherten Elemente wieder zur mobile-app. Die mobile-app zeigt diese Daten in der Liste an.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Wiederholen Sie die vorherigen drei Schritte für jedes Gerät Plattform integriert, die unterstützt werden soll.

[Azure-Portal]: https://portal.azure.com/
