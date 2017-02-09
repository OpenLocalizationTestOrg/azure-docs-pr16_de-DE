Jetzt, da Sie einen Trigger hinzugefügt haben, interessante es ist Zeit für einen Vorgang ausführen mit den Daten, die vom Trigger generiert wird. Wie folgt vor, um die **SharePoint Online - Datei erstellen** Aktion hinzuzufügen. Diese Aktion erstellt eine Datei in SharePoint Online jedes Mal der neuen Element Trigger ausgelöst wird. 

So konfigurieren Sie diese Aktion, müssen die folgende Informationen bereitzustellen. Wie Sie diese Informationen angeben, werden Sie feststellen, dass es benutzerfreundliche Daten als Eingabe für einige der Eigenschaften für die neue Datei vom Trigger generiert wird:

|Erstellen von Dateieigenschaft|Beschreibung|
|---|---|
|Website-URL|Dies ist die URL der SharePoint Online-Website, wo Sie die neue Datei erstellen möchten. Wählen Sie die Website aus der angezeigten Liste aus.|
|Ordnerpfad|Dies ist der Ordner (bei den Website-URL, die im vorherigen Schritt ausgewählte), in die neue Datei kopiert werden. Suchen Sie nach, und wählen Sie den Ordner aus.|
|Dateiname|Dies ist der Name der Datei erstellt wird.|
|Der Inhalt der Datei|Der Inhalte, die in die Datei geschrieben werden.|

1. Wählen Sie **+ neuen Schritt** die Aktion hinzufügen.  
![SharePoint online Aktion Bild 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Wählen Sie den Link zum **Hinzufügen einer Aktion** aus. Dieses wird geöffnet, die im Suchfeld, in dem Sie für jede Aktion suchen können, ausführen möchten. In diesem Beispiel sind SharePoint-Aktionen von Interesse.    
![SharePoint online Aktion Bild 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- Geben Sie *Sharepoint* zu suchenden Aktionen im Zusammenhang mit SharePoint aus.
- Wählen Sie als die auszuführende Aktion auf **SharePoint Online - Datei zu erstellen** .   **Hinweis**: werden Sie aufgefordert, autorisieren die Logik app zu Ihrer SharePoint-Konto zugreifen, wenn Sie eine Verbindung mit SharePoint Online nicht zuvor erstellt haben.    
![SharePoint online Aktion Bild 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Das Steuerelement **Erstellen-Datei** wird geöffnet.   
![SharePoint online Aktion Bild 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Wählen Sie **Website-URL** ein, und wechseln Sie zu der Website, in dem Sie die Datei erstellen möchten.     
![SharePoint online Aktion Bild 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Wählen Sie **Ordnerpfad** , und suchen Sie nach dem Ordner, in dem die neue Datei platziert wird.  
![SharePoint online Aktion Bild 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Wählen Sie das Steuerelement **Dateinamen ein** , und geben Sie den Namen der Datei, die Sie erstellen möchten. Hier, Sie können entweder den Dateinamen direkt eingeben oder Sie können eine der Eigenschaften aus den zuvor erstellten Trigger verwenden. Aktion auswählen von Eigenschaften in der Liste der **Ausgaben aus, wenn ein neues Element erstellt wird**. Diese Liste ist nur anzeigen, nachdem Sie das **Dateinamen ein** Steuerelement auswählen. In diesem Walkthough ausgewählt ich ID (die ID des neuen Listenelements) als Namen für die Datei, die von der **SharePoint Online - Datei erstellen** Aktion erstellt wird.    
![SharePoint online Aktion Bild 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Wählen Sie das Steuerelement **der Inhalt der Datei** , und geben Sie den Inhalt, der die Datei geschrieben werden sollen, die erstellt werden soll. Beachten Sie für den Dateiinhalt, dass Sie die Eigenschaften von der Trigger verwenden können, die Sie zuvor erstellt haben. Wählen Sie einfach die Eigenschaften aus der angezeigten Liste aus. Alternativ können Sie den **Inhalt der Datei** Text direkt in das Steuerelement eingeben. In diesem Beispiel kann ich einige Eigenschaften ausgewählt und Leerzeichen und ein Bindestrich zwischen jede Eigenschaft hinzugefügt.        
![SharePoint online Aktion Bild 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Speichern der Änderungen auf den workflow  
- Herzlichen Glückwunsch, Sie verfügen nun über eine voll funktionsfähige Logik-app, die ausgelöst wird, wenn ein neues Element mit einer SharePoint Online-Liste hinzugefügt wird. Die app wird dann mit einigen der Eigenschaften von neuen Listenelements eine Datei erstellen.  Sie können es jetzt testen, durch Erstellen eines neuen Elements in der SharePoint-Liste. 
