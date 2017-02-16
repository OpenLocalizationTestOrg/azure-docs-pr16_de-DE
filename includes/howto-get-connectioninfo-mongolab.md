Wenn Sie eine Datenbank MongoLab bereitstellen, übermittelt MongoLab eine Verbindung zu Azure URI in das Format des MongoDB standard Verbindungszeichenfolge aus. Dieser Wert wird zum Einleiten einer Verbindung MongoDB über Ihre Auswahl der MongoDB Treiber verwendet. Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter mongodb.org [Verbindungen](http://www.mongodb.org/display/DOCS/Connections) .

**Dieser URI enthält Ihrer Datenbank-Benutzernamen und Ihr Kennwort ein.  Als vertrauliche Informationen behandelt und nicht freigeben.**

Sie können diesen URI im Azure-Portal mit den folgenden Schritten abrufen:

1. Wählen Sie die **Add-ons**aus.  
![AddonsButton][button-addons]
1. Suchen nach dem MongoLab-Dienst in der Liste der Add-Ons.  
![MongolabEntry][entry-mongolabaddon]
1. Klicken Sie auf den Namen des Add-Ons, um die Seite Add-On angezeigt wird.
1. Klicken Sie auf **Verbindungsinformationen**.  
![ConnectionInfoButton][button-connectioninfo]  
Ihre MongoLab URI wird angezeigt:  
![ConnectionInfoScreen][screen-connectioninfo]  
1.  Klicken Sie auf die Schaltfläche "Zwischenablage" rechts neben den MONGOLAB_URI Wert den vollständigen Wert in die Zwischenablage zu kopieren.

[entry-mongolabaddon]: ./media/howto-get-connectioninfo-mongolab/entry-mongolabaddon.png
[button-connectioninfo]: ./media/howto-get-connectioninfo-mongolab/button-connectioninfo.png
[screen-connectioninfo]: ./media/howto-get-connectioninfo-mongolab/dialog-mongolab_connectioninfo.png
[button-addons]: ./media/howto-get-connectioninfo-mongolab/button-addons.png
