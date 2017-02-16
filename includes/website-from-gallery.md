Die Azure Marketplace stellt eine Vielzahl von gängige Web apps, entwickelt von Microsoft, Drittanbieter Unternehmen und open-Source-Software Initiativen zur Verfügung. Installieren von Software, außer den Webbrowser zum Verbinden mit der [Preview-Portal Azure](http://go.microsoft.com/fwlink/?LinkId=529715)erforderlich Web apps erstellt aus dem Azure Marketplace nicht. 

In diesem Lernprogramm erfahren Sie:

- So erstellen Sie eine neue Web-app durch die Azure Marketplace.

- Informationen zum Bereitstellen des Web app über das Azure Preview-Portal.
 
Sie erhalten einen WordPress Blog erstellen, der eine Standardvorlage verwendet. Die folgende Abbildung zeigt die fertige Anwendung:


![WordPress-blog][13]

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="create-a-web-app-in-the-portal"></a>Erstellen einer Web app im portal

1. Melden Sie sich mit dem Portal Azure Vorschau.

2. Öffnen Sie die Azure Marketplace entweder durch Klicken auf das Symbol **Marketplace** :

    ![Marketplace-Symbol][marketplace]

    Oder indem Sie auf das **neue** Symbol in der oberen rechten Ecke des Dashboards und **Marketplace** bei der Bottow der Liste auswählen.
    
    ![Neu erstellen][5]
    
3. Wählen Sie **Web + Mobile**. Suchen Sie nach **WordPress** , und klicken Sie auf das Symbol **WordPress** .

    ![WordPress aus der Liste][7]
    
5. Wählen Sie nach dem Lesen der Beschreibung der app WordPress, **Erstellen**.

6. Klicken Sie auf **Web app**und Erläutern Sie die notwendigen erforderlichen Werte für Web app konfigurieren.
    
    ![Konfigurieren der app][8]

7. Klicken Sie auf die **Datenbank**, und geben Sie die erforderlichen Werte für das Konfigurieren der MySQL-Datenbank. 

    ![Konfigurieren der Datenbank][database]

8. Geben Sie einen Namen für eine neue Ressourcengruppe ein.

    ![Festlegen von Ressourcengruppe][groupname]

8. Falls erforderlich, klicken Sie auf **Abonnement**, und geben Sie das Abonnement verwenden. 

7. Wenn Sie die Web app definieren abgeschlossen haben, klicken Sie auf **Erstellen**, und warten Sie, während das neue Web app erstellt wird.

   Wenn die app erstellt wurde, wird die Ressourcengruppe Web app und die Datenbank, angezeigt.

   ![Gruppe ' anzeigen '][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten und Verwalten von WordPress Web app
    
1. Klicken Sie auf Ihre neue Web app finden Sie Details zu der app.

    ![Starten Sie dashboard][10]

2. Klicken Sie auf der Seite **Essentials** unter **Url** zum Öffnen der Web-app-Homepage auf **Durchsuchen** oder den Link.

    ![Website-URL][browse]

3. WordPress nicht installiert haben, geben Sie die entsprechenden Informationen, WordPress erforderlich machen, und klicken Sie auf **Installieren WordPress** zum Fertigstellen Konfiguration, und öffnen Sie die Web-app-Anmeldeseite.

4. Geben Sie Ihre Anmeldeinformationen ein, und klicken Sie auf **Login** .  

5. Sie müssen eine neue WordPress Web app, die bei der Web-app unten ähnlich aussieht.    

    ![Ihre Website WordPress][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
