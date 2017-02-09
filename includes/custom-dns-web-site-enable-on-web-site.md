Nachdem die Einträge für Ihren Domänennamen verteilt haben, müssen Sie diese Web-App zuordnen. Verwenden Sie die folgenden Schritte aus, um den Domänennamen mit dem Webbrowser aktivieren.

> [AZURE.NOTE] Es kann einige TXT-Einträge in den vorherigen Schritten im DNS-System weitergegeben erstellt dauern. Sie können keine Web app den Domänennamen des hinzugefügt, bis der TXT-Eintrag verteilt wurde. Bei Verwendung ein A-Datensatzes können nicht Sie Web app den A-Datensatz Domänennamen hinzufügen, bis der TXT-Eintrag im vorherigen Schritt erstellte verteilt wurde.
>
> Einen Dienst, wie z. B. <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der TXT-Eintrag verfügbar ist.

1. Öffnen Sie in Ihrem Browser das [Azure-Portal](https://portal.azure.com)aus.

2. Klicken Sie auf den Namen der Web app auf der Registerkarte **Web Apps** , und wählen Sie dann auf **benutzerdefinierte Domänen**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. Klicken Sie in das Blade **benutzerdefinierte Domänen** auf **Hostname hinzufügen**.
    
4. Verwenden Sie die Textfelder **Hostname** beim Eingeben der Domänennamen, mit dieser Web app zugeordnet werden soll.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Klicken Sie auf **Überprüfen**.

7.  Beim Klicken auf die **Gültigkeit** Azure wird deaktivieren Domäne Überprüfung Workflow Starten eines. Hiermit wird für Besitzer der Domäne als auch Hostname Verfügbarkeit und Bericht Erfolg oder detaillierte Fehler mit Nachschlagewerke Guidence zum Beheben des Fehlers überprüft.    

An diesem Punkt sollte es möglich sein, geben den Namen der benutzerdefinierten Domäne in Ihrem Browser, und sehen, dass Sie dieses erfolgreich Web app hat.
