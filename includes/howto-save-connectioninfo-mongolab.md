Während ein MongoLab URI in den Code eingefügt werden kann, wird empfohlen, konfigurieren, dass er in der Umgebung zum Steigern der Verwaltung. Auf diese Weise, wenn der URI verwandelt hat, können Sie ihn über das Azure-Portal aktualisieren ohne zu den Code zu wechseln.


1. Wählen Sie im Portal Azure **Web Apps**.
1. Klicken Sie auf den Namen der Web-app in der Web Apps-Liste.  
![WebAppEntry][entry-website]  
Das Web App-Dashboard wird angezeigt.

1. Klicken Sie in der Menüleiste auf " **Konfigurieren** ".  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Führen Sie einen Bildlauf nach unten bis zum Abschnitt Verbindungszeichenfolgen.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Geben Sie unter **Name**MONGOLAB_URI.
1. Fügen Sie die Verbindungszeichenfolge, die im vorherigen Abschnitt erzielten **Wert**.
1. Wählen Sie in der Dropdownliste **Typ** (anstelle des Standardwerts **SQLAzure**) **Benutzerdefiniert** .
1. Klicken Sie auf der Symbolleiste auf **Speichern** .  
![SaveWebApp][button-website-save]

**Hinweis:** Azure fügt die **CUSTOMCONNSTR\_ ** Präfix auf diese Variable, weshalb den Code über Verweise **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
