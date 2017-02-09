Die meisten Fehler Authentifizierung Zeit als Ergebnis falsch oder inkonsistent Konfigurations-Einstellungen. Hier sind einige Vorschläge, bestimmte Dinge zu überprüfen.

* Stellen Sie sicher, dass Sie an einer beliebigen Stelle Schaltfläche **Speichern** verpasst haben. Dies ist oft einfach zu tun ist, und das Ergebnis ist, auf einer Portalseite auf die richtigen Werte gefunden werden wird, aber noch nicht tatsächlich in der Azure-Umgebung oder Azure AD-Anwendung gespeichert wurden.
* Einstellungen **Anwendungseinstellungen** Falz Azure-Portal Berechtigungen konfiguriert sind Vergewissern Sie sich, dass die richtige API app oder Web app ausgewählt wurde, wenn die Einstellungen eingegeben wurden.  Außerdem sicherstellen Sie, dass die Einstellungen als **App-Einstellungen** und keine **Verbindungszeichenfolgen**eingegeben wurden, wie das Format der beiden Abschnitte ähnlich ist.
* Für die Authentifizierung ein JavaScript-front-End, laden Sie das Manifest erneut, um sicherzustellen, dass `oauth2AllowImplicitFlow` wurde erfolgreich geändert in `true`.
* Stellen Sie sicher, dass Sie HTTPS verwendet, wo Sie URLs konfiguriert:

    * Project-Code
    * In CORS
    * In Azure-Umgebung, die App-Einstellungen für jede API-app und Web app
    * In den Einstellungen für Azure AD-Anwendung.
    
    Beachten Sie, dass wenn Sie eine API-app-URL aus dem Portal kopieren, es häufig ist `http://` und manuell zu ändern, müssen Sie `https://`.

* Stellen Sie sicher, dass alle Änderungen Code erfolgreich bereitgestellt wurden. Beispielsweise kann in einer Lösung mehrere Projekte Codes für ein Projekt ändern, und wählen einen der anderen versehentlich, wenn Sie die Änderung bereitstellen möchten.
* Stellen Sie sicher, dass Sie in Ihrem Browser nicht HTTP-URLs auf HTTPS-URLs abgelegt werden. Standardmäßig erstellt Visual Studio Profile mit HTTP-URLs veröffentlichen, und das ist, was im Browser geöffnet wird, nachdem Sie ein Projekt bereitstellen.
* Stellen Sie sicher, dass CORS richtig API-App konfiguriert ist, die der JavaScript-Code ruft Aktionen aus, für die Authentifizierung ein JavaScript-front-End. Im Zweifelsfall darüber, ob das Problem CORS verknüpft ist, versuchen Sie "*" als die zulässige Origin-URL. 
* Ein JavaScript-front-End Öffnen Ihres Browsers Developer Tools Console Registerkarte, um weitere Informationen zu erhalten, und Untersuchen von HTTP-Anfragen im Netzwerk. Fehlermeldungen Console können jedoch irreführend sein. Wenn eine Meldung mit einen CORS Fehler ausgegeben wird, kann das Problem reale Authentifizierung sein. Sie können prüfen, ob dies der Fall ist, indem Sie die app mit Authentifizierung vorübergehend vorübergehend deaktiviert ausführen.
* Sicherzustellen Sie zu einer app .NET API, dass Sie so viele Informationen in Fehlermeldungen wie möglich abrufen durch Festlegen [CustomErrors Modus aus](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Starten Sie für eine app .NET API eine [Remotedebugsitzung](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), und überprüfen Sie die Werte der Variablen, die auf Code, der ADAL wird verwendet, um eine Person Token erfassen oder Code, Beschwerden gegen die wichtigsten erwartete Service-ID überprüft, übergeben werden Beachten Sie, dass Code, sodass es möglich ist, finden Sie auf diese Weise Auflösung von Konfigurationswerten aus vielen unterschiedlichen Quellen auswählen kann. Beispielsweise, wenn falsch eingegeben `ida:ClientId` als `ida:ClientID` beim Konfigurieren der App-Verwaltungsdienst Azure-Umgebung, die Einstellungen des Codes erhalten möglicherweise die `ida:ClientId` Wert, der aus der Datei Web.config, ignorieren die Einstellung für die App-Verwaltungsdienst Azure-Datei gesucht. 
* Wenn Elemente nicht in einem normalen Internet Explorer-Fenster arbeiten möchten, möglicherweise eine vorhandene anmelden stören; Wiederholen Sie den InPrivate, und versuchen Sie es Chrome oder Firefox aus.
