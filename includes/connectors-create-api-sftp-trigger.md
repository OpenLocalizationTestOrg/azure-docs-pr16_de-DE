Wir fügen Sie einen Trigger hinzu.

1. Geben Sie in das Suchfeld *Sftp* auf der Logik apps Designer und wählen Sie dann den Trigger **SFTP – Wenn eine Datei hinzugefügt oder geändert wird**   
![SFTP auslösenden Bild 1](./media/connectors-create-api-sftp/trigger-1.png)  
- Das Steuerelement **, wenn eine Datei hinzugefügt oder geändert wird** angezeigt wird  
![SFTP auslösenden Bild 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Wählen Sie die **...** befindet sich auf der rechten Seite des Steuerelements. Daraufhin wird das Ordner Datumsauswahl-Steuerelement  
![SFTP auslösenden Bild 3](./media/connectors-create-api-sftp/action-1.png)  
- Wählen Sie die **SFTP** auf den Stammordner als den Ordner, um neue oder geänderte Dateien überwachen auswählen aus. Beachten Sie, dass der Stammordner des Steuerelements **Ordner** jetzt angezeigt wird.  
![SFTP auslösenden Bild 4](./media/connectors-create-api-sftp/action-2.png)   

An diesem Punkt wurde die app Logik zu einem Trigger konfiguriert, die in eine Abfolge von anderen Trigger und Aktionen in dem Workflow beginnen, wenn eine Datei geändert oder in einem Ordner Wahl SFTP erstellt wird. 

>[AZURE.NOTE]Für eine app Logik funktioniert muss es mindestens ein Trigger und eine Aktion enthalten. Führen Sie die Schritte im nächsten Abschnitt, um eine Aktion hinzuzufügen.  