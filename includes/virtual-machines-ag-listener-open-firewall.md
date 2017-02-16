In diesem Schritt erstellen Sie eine Firewall-Regel, um den Prüfpunkt Port für den Lastenausgleich Endpunkt (wie zuvor angegeben 59999) zu öffnen und eine weitere Regel, den Verfügbarkeit Gruppe Zuhörer Anschluss zu öffnen. Da Sie den Endpunkt mit Lastenausgleich auf den Azure-virtuellen Computern erstellt, die Verfügbarkeit Gruppe Replikate enthalten haben, müssen Sie den Port Prüfpunkt und den Port Zuhörer auf den entsprechenden Azure-virtuellen Computern geöffnet.

1. Starten Sie auf virtuellen Computern Hostinganbieter Replikations **Windows Firewall mit erweiterter Sicherheit**.

1. Mit der rechten Maustaste **Eingehende Regeln** , und klicken Sie auf **Neue Regel**.

1. Klicken Sie auf der Seite **Regeltyp** wählen Sie **Anschluss aus**und dann auf **Weiter**.

1. Klicken Sie auf der Seite **Protokolle und Ports** wählen Sie **TCP** , und geben Sie im Feld **bestimmte lokale Ports** **59999** . Klicken Sie dann auf **Weiter**.

1. Klicken Sie auf der Seite **Aktion** behalten Sie **Verbindung zulassen** ausgewählt bei, und klicken Sie auf **Weiter**.

1. **Die Profilseite** akzeptieren Sie die Standardeinstellungen und dann auf **Weiter**.

1. Auf der Seite **Namen** Geben Sie eine Regel an, wie z. B. **AlwaysOn Zuhörer Prüfpunkt Port** in das Textfeld **Name** , und klicken Sie auf **Fertig stellen**.

1. Wiederholen Sie die obigen Schritte für die Verfügbarkeit Gruppe Zuhörer Port (wie weiter oben in der $EndpointPort-Parameter des Skripts angegeben), und geben Sie einen entsprechenden Regel an, wie z. B. **AlwaysOn Zuhörer Port**.