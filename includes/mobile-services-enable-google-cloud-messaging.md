
1. Navigieren Sie zu den [Google-Cloud-Verwaltungskonsole](https://console.developers.google.com/project), melden Sie sich mit Ihrer Gmail-Kontoanmeldeinformationen. 
 
2. Klicken Sie auf **Projekt erstellen**, geben Sie den Namen eines Projekts, und klicken Sie auf **Erstellen**. Wenn Sie aufgefordert werden, führen Sie die SMS-Überprüfung, und klicken Sie erneut auf **Erstellen** .

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   

     Geben Sie in Ihrem neuen **Projektnamen ein** , und klicken Sie auf **Projekt erstellen**.

3. Klicken Sie dann auf **Projektinformationen**, und klicken Sie auf die Schaltfläche **Dienstprogramme und vieles mehr** . Notieren Sie die **Projektnummer**an. Sie müssen diesen Wert als Festlegen der `SenderId` Variable in der Client-app.

    ![](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)


4. Klicken Sie im Projektdashboard unter **Mobile-APIs**auf **Google Cloud Messaging**, klicken Sie auf der nächsten Seite auf **API aktivieren** und annehmen der Vertragsbedingungen. 

    ![Aktivieren der GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)

    ![Aktivieren der GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 

5. Klicken Sie in dem Projektdashboard auf **Anmeldeinformationen** > **Create Credential** > **API-Schlüssel**. 

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)

6. **Erstellen Sie einen neuen Product Key**klicken Sie auf **Server-Taste**, geben Sie einen Namen für den Key, klicken Sie auf **Erstellen**.

7. Notieren Sie den Wert **API-Taste** .

    Sie verwenden diese API Schlüsselwert Azure mit GCM authentifizieren und Senden von Pushbenachrichtigungen für Ihre app zu aktivieren.

