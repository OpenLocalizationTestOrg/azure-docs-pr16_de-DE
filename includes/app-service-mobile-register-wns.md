
1. In Visual Studio-Lösung Explorer Maustaste auf das Windows Store-app-Projekt, und klicken Sie auf **Store** > **App mit dem Store... zugeordnet werden soll**.

    ![Verknüpfen mit Windows Store-app](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. Klicken Sie im Assistenten klicken Sie auf **Weitersuchen**, melden Sie sich mit Ihrem Microsoft-Konto, geben Sie einen Namen für Ihre app in **Reservieren einen neuen Namen für die app**, klicken Sie auf **Reservieren**.

3. Nach die app-Registrierung erfolgreich erstellt wurde, wählen Sie den neuen Namen für die app aus, klicken Sie auf **Weiter**, und klicken Sie dann auf **Verbinden**. Dadurch wird die erforderliche Windows Store-Registrierungsinformationen Anwendungsmanifest hinzugefügt.

7. Wiederholen Sie die Schritte 1 und 3 für Windows Phone Store-app-Projekt mit den gleichen Registrierungs-zuvor erstellten für Windows Store-app.  

7. Navigieren Sie zum [Windows Developer Center](https://dev.windows.com/en-us/overview), mit Ihrem Microsoft-Konto anmelden, klicken Sie auf die neue app-Registrierung in **Meine apps**, und klicken Sie dann erweitern Sie **Dienste** > **Pushbenachrichtigungen**.

8. Auf der Seite **Pushbenachrichtigungen** klicken Sie auf **Live Services-Website** unter **Windows Pushbenachrichtigungen Benachrichtigung Services (WNS) und Microsoft Azure Mobile-Apps**, und notieren Sie die Werte für das **Paket SID** und des *aktuellen* Werts in der **Anwendung geheim**. 

    ![App-Einstellung im Developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Die Anwendung geheim und Paket SID sind wichtige Sicherheitsanmeldeinformationen. Teilen Sie diese Werte mit jedem oder verteilen Sie diese App nicht.
