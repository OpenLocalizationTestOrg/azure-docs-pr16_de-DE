Jetzt, da Sie einen Trigger hinzugefügt haben, interessante es ist Zeit für einen Vorgang ausführen mit den Daten, die vom Trigger generiert wird. Führen Sie diese Schritte zum Hinzufügen eines der Aktion **SharePoint Online - Datei zu erstellen** . Diese Aktion erstellt eine Datei in SharePoint Online jedes Mal der neuen Element Trigger ausgelöst wird. 

So konfigurieren Sie diese Aktion, müssen die folgende Informationen bereitzustellen. Sie können feststellen, dass es benutzerfreundliche Daten als Eingabe für einige der Eigenschaften für die neue Datei vom Trigger generiert:

|Erstellen von Dateieigenschaft|Beschreibung|
|---|---|
|Website-URL|Dies ist die URL der SharePoint Online-Website, wo Sie die neue Datei erstellen möchten. Wählen Sie die Website aus der angezeigten Liste aus.|
|Ordnerpfad|Hierbei handelt es sich um den Ordner (bei den Website-URL), in dem die neue Datei platziert wird. Suchen Sie nach, und wählen Sie den Ordner aus.|
|Dateiname|Dies ist der Name der Datei erstellt wird.|
|Der Inhalt der Datei|Der Inhalte, die in die Datei geschrieben werden.|

1. Wählen Sie **+ neuen Schritt** die Aktion hinzufügen.  
![](./media/connectors-create-api-sharepointonline/action-1.png)  
- Wählen Sie Hyperlink **Hinzufügen eine Aktion** aus. Dieses wird geöffnet, die im Suchfeld, in dem Sie für jede Aktion suchen können, ausführen möchten. In diesem Beispiel sind SharePoint-Aktionen von Interesse.    
![](./media/connectors-create-api-sharepointonline/action-2.png)    
- Geben Sie *Sharepoint* zu suchenden Aktionen im Zusammenhang mit SharePoint aus.
- Wählen Sie als die auszuführende Aktion auf **SharePoint Online - Datei zu erstellen** .   **Hinweis**: werden Sie aufgefordert, autorisieren die Logik app zu Ihrer SharePoint-Konto zugreifen, wenn Sie nicht bereits getan haben.    
![](./media/connectors-create-api-sharepointonline/action-3.png)    
- Das Steuerelement **Erstellen-Datei** wird geöffnet.   
![](./media/connectors-create-api-sharepointonline/action-4.png)     
- Wählen Sie **Website-URL** ein, und wechseln Sie zu der Website, in dem Sie die Datei erstellen möchten.     
![](./media/connectors-create-api-sharepointonline/action-5.png)  
- Wählen Sie **Ordnerpfad** , und suchen Sie nach dem Ordner, in dem die neue Datei platziert wird.  
![](./media/connectors-create-api-sharepointonline/action-6.png)  
- Wählen Sie das Steuerelement **Dateinamen ein** , und geben Sie den Namen der Datei, die Sie erstellen möchten. Beachten Sie, dass Sie die Eigenschaften von der Trigger, die Sie zuvor erstellt haben verwenden können, indem Sie es einfach aus der angezeigten Liste auswählen, für den Dateinamen.     
![](./media/connectors-create-api-sharepointonline/action-7.png)  
- Wählen Sie das Steuerelement **der Inhalt der Datei** , und geben Sie den Inhalt, der die Datei geschrieben werden sollen, die erstellt werden soll. Beachten Sie für den Dateiinhalt, dass Sie die Eigenschaften von der Trigger verwenden können, die Sie zuvor erstellt haben. Wählen Sie einfach die Eigenschaften aus der angezeigten Liste aus. Alternativ können Sie den **Inhalt der Datei** Text direkt in das Steuerelement eingeben. In diesem Beispiel kann ich einige Eigenschaften ausgewählt und Leerzeichen und ein Bindestrich zwischen jede Eigenschaft hinzugefügt.        
![](./media/connectors-create-api-sharepointonline/action-8.png)  
- Speichern der Änderungen auf den workflow  
