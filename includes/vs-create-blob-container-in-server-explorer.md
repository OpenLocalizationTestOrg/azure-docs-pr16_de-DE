Sie können mithilfe von Visual Studio- **Server-Explorer**Azure-Speicher Warteschlangen erstellen.

![Server-Explorer Blobs][Image1]

1. Wählen Sie **Server-Explorer**im Menü **Ansicht** aus.
2. Im Server-Explorer erweitern Sie den Knoten **Azure** für Ihr Abonnement, erweitern Sie den **Speicher** und die Knoten für das Speicherkonto, die, das Sie in der Azure verbunden Speicherdienst angegeben haben.
3. Wählen Sie den Knoten **Warteschlangen** aus, und wählen Sie aus dem Kontextmenü **Warteschlange erstellen** aus.
4. Geben Sie einen Namen für den Container, und wählen Sie **OK**aus.   

Standardmäßig der neue Container ist privat, und geben Sie Ihre Speicher Zugriffstaste zum Herunterladen von Blobs aus diesem Container. Wenn Sie die Dateien im Container öffentlich machen möchten, wählen Sie den Container in **Server-Explorer** , und drücken Sie die `F4` in das Fenster **Eigenschaften** anzuzeigen. Legen Sie den **öffentlichen Lesezugriff** **Blob**fest. Jeder im Internet kann Blobs in einem öffentlichen Container sehen, jedoch können Sie ändern oder löschen sie nur, wenn Sie über die entsprechenden Zugriffstaste verfügen.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png