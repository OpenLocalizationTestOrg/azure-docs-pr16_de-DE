<properties
    pageTitle="Rufen Sie eine benutzerdefinierte API Logik Apps"
    description="Verwenden Ihre benutzerdefinierten API auf App-Verwaltungsdienst mit apps Logik gehostet"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Verwenden Ihre benutzerdefinierten API auf App-Verwaltungsdienst mit apps Logik gehostet

Zwar Logik apps verfügt über eine Vielzahl von 40 + Connectors für eine Vielzahl von Diensten, empfiehlt es sich bei Ihrer eigenen benutzerdefinierten API einwählen, die von eigenem Code ausgeführt werden kann. Die einfachste und am besten skalierbare Möglichkeiten eigene benutzerdefinierte Web-APIs hosten besteht darin, App-Dienst verwenden. In diesem Artikel beschrieben, wie Sie bei einem beliebigen Web-API in einer App-Service-API app Web app oder mobile-app gehostet einwählen.

Informationen darüber, wie die APIs als Trigger oder Aktion innerhalb der Logik apps erstellen lesen Sie [in diesem Artikel](app-service-logic-create-api-app.md)aus.

## <a name="deploy-your-web-app"></a>Bereitstellen von Web App

Zuerst müssen Sie Ihre API als Web App im App-Dienst bereitstellen. Hier die Anleitung behandelt grundlegende Bereitstellung: [Erstellen einer ASP.NET Web app](../app-service-web/web-sites-dotnet-get-started.md).  Gedrückt, während Sie in einer beliebigen-API aus einer app Logik aufrufen können, fügen Sie für optimale Ergebnisse, Sie sollten, Swagger Metadaten einfach Steuerelementaktionen apps Logik integriert werden soll.  Sie können Details zum [Hinzufügen von Swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)suchen.

### <a name="api-settings"></a>API-Einstellungen

In Reihenfolge für den Logik apps Designer, um Ihre Swagger zu analysieren ist es wichtig, CORS aktivieren und Festlegen der Eigenschaften APIDefinition der Web app.  Dies ist sehr einfach, innerhalb der Azure-Portal einzurichten.  Einfach Falz Einstellungen der Web-App zu öffnen und unter dem Abschnitt API Festlegen der 'API Definition' auf die URL Ihrer swagger.json-Datei (Dies ist in der Regel https://{name}.azurewebsites.net/swagger/docs/v1) und hinzufügen eine Richtlinie CORS für ' *' in der Designer-Logik apps-Anfragen zulässig ist.

## <a name="calling-into-the-api"></a>Aufrufen der API

Wenn Sie sich im Portal apps Logik, wenn Sie CORS festgelegt haben, und die Definition von API Eigenschaften Sie sollten möglicherweise einfach fügen Sie API benutzerdefinierte Aktionen innerhalb der Fluss hinzu.  Im Designer können Sie auswählen, Durchsuchen Sie Ihren Abonnement-Websites, um die Liste der Websites mit einer Swagger-URL definiert.  Sie können auch die HTTP + Swagger Aktion, zeigen Sie auf eine Swagger und die Liste der verfügbaren Aktionen und Eingaben.  Schließlich können Sie immer erstellen eine Anforderung mithilfe der Aktion HTTP jeder API, auch wenn diese Nummer, die nicht haben oder ein Dokument Swagger angezeigt.

Wenn Sie Ihre API sichern möchten, werden einige andere Methoden zum erledigen:

1. Keine Änderung des Codes erforderlich - Azure Active Directory kann Ihre API schützen, ohne Code Änderung oder erneute Bereitstellung verwendet werden.
1. Erzwingen Sie Standardauthentifizierung, AAD autorisierende oder Zertifikat autorisierende in den Code, der Ihre API aus.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Sichern von Anrufe an Ihre API ohne Änderung des Codes

In diesem Abschnitt erstellen Sie zwei Azure Active Directory-Applikationen – eine für Ihre app Logik und eine für Ihre Web App.  Sie können Anrufe an Ihre Web-App verwenden die Dienst der Tilgungsanteile (Client-Id und Schlüssel) der Anwendung AAD für Ihre app Logik zugeordneten authentifizieren. Darüber hinaus erhalten Sie die Anwendung enthalten IDs in der Definition Ihrer Logik app.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Teil 1: Einrichten einer Anwendungsidentität für Ihre app Logik

Dies ist, was die app Logik Authentifizierung für active Directory verwendet. Sie nur *müssen* dies einmal für Ihr Verzeichnis. Beispielsweise können Sie auswählen mit der gleichen Identität für alle Ihre apps Logik erstellen oder aber auch möglicherweise eindeutige Identitäten pro Logik app Wunsch. Sie können dazu in der Benutzeroberfläche oder PowerShell verwenden.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Erstellen Sie die Identität des mit dem klassischen Azure-portal

1. Navigieren Sie zu [Active Directory im klassischen Azure-Portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) , und wählen Sie aus dem Verzeichnis aus, das Sie für Ihre Web App verwenden
2. Klicken Sie auf die Registerkarte **Applications**
3. Klicken Sie auf **Hinzufügen** , in der Befehlsleiste am unteren Rand der Seite
4. Benennen Sie Ihre Identität zu verwenden, klicken Sie auf den Pfeil nach rechts
5. Setzen Sie in eine eindeutige Zeichenfolge als eine Domäne in die beiden Textfelder formatiert werden soll, und klicken Sie auf das Häkchen
6. Klicken Sie auf die Registerkarte " **Konfigurieren** " für diese Anwendung
7. Kopieren Sie die **Client-ID**, wird dies in Ihrer app Logik verwendet
8. Klicken Sie im Abschnitt **Tasten** auf **Dauer auswählen** , und wählen Sie entweder Jahr 1 oder 2 Jahre
9. Klicken Sie auf die Schaltfläche **Speichern** am unteren Rand des Bildschirms (Sie müssen möglicherweise warten Sie ein paar Sekunden)
10. Nun werden Sie sicher, dass die Taste in das Feld zu kopieren. Dies wird auch in der app Logik verwendet werden

#### <a name="create-the-application-identity-using-powershell"></a>Erstellen Sie die Identität des mithilfe der PowerShell
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Achten Sie darauf, kopieren Sie die **Mandanten-ID**, die **Anwendung-ID** und das Kennwort ein, das Sie verwendet haben

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>Teil 2: Web App mit einer AAD app Identität schützen

Wenn der Web-app bereits bereitgestellt ist können Sie es direkt im Portal aktivieren. Andernfalls können Sie vornehmen, Aktivieren der Autorisierung für einen Teil des Bereitstellung Ihrer Azure Ressource-Manager.

#### <a name="enable-authorization-in-the-azure-portal"></a>Aktivieren der Autorisierung Azure-Portal

1. Navigieren Sie zu der Web-app, und klicken Sie auf die **Einstellungen** in der Befehlsleiste.
2. Klicken Sie auf **Autorisierung/Authentifizierung**.
3. **Aktivieren Sie es.**

Eine Anwendung wird zu diesem Zeitpunkt automatisch für Sie erstellt. Sie benötigen diese Anwendungs Client-ID für Teil 3, daher Sie in müssen:

1. Wechseln Sie zu [Active Directory im klassischen Azure-Portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) an, und wählen Sie Ihr Verzeichnis.
2. Suchen Sie nach der app in das Suchfeld
3. Klicken Sie auf ihn in der Liste
4. Klicken Sie auf die Registerkarte **Konfigurieren**
5. Die **Client-ID** sollte angezeigt werden.

#### <a name="deploying-your-web-app-using-an-arm-template"></a>Bereitstellen von mithilfe einer Vorlage Cloud Web App

Zuerst müssen Sie eine Anwendung für Ihre Web-app erstellen. Diese sollte sich von der Anwendung unterscheiden, die für Ihre app Logik verwendet wird. Zunächst nach oben beschriebenen Schritte in Teil 1, aber jetzt für die **HomePage** und **IdentifierUris** Nutzung der tatsächlichen https://**URL** der Web app.

>[AZURE.NOTE]Wenn Sie die Anwendung für Ihre Web-app erstellen, müssen Sie die [Azure klassischen Portal Ansatz](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), verwenden, wie der PowerShell-Cmdlet nicht darauf, dass die erforderlichen Berechtigungen einrichten für den Benutzer bei einer Website anmelden.

Wenn Ihre Client-ID und Mandanten-ID, die folgenden Zeichen als eine Sub-Ressource des Web app in Ihrer Bereitstellungsvorlage eingebunden werden:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Um eine Bereitstellung ausführen automatisch, die eine leere Web app und Logik app nicht trennen, mit denen AAD bereitstellt, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Vollständige Vorlage finden Sie unter [Logik app Anrufe in eine benutzerdefinierte API auf App-Dienst gehostet und durch AAD geschützt ist](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>Teil 3: Füllen Sie im Abschnitt Autorisierung in der app Logik

Im Abschnitt **Autorisierung** der Aktion **HTTP** :`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Beschreibung |
|---------|-------------|
| Typ | Art der Authentifizierung. Für die ActiveDirectoryOAuth Authentifizierung ist der Wert ActiveDirectoryOAuth. |
| Mandanten | Die Mandanten Bezeichner verwendet, um den AD-Mandanten zu identifizieren. |
| Zielgruppe | Erforderlich. Die Ressource, die, der Sie eine Verbindung herstellen. |
| clientID | Die Client-ID für die Azure AD-Anwendung. |
| geheim | Erforderlich. Geheim des Clients, der das Token anfordert. |

Die vorstehenden Vorlage weist bereits dies einrichten, aber wenn Sie die app Logik direkt erstellen, müssen Sie den vollständige Autorisierung Abschnitt angezeigt werden.

## <a name="securing-your-api-in-code"></a>Schutz Ihrer API Code

### <a name="certificate-auth"></a>Zertifikat-Authentifizierung

Client-Zertifikate können auf eingehenden Anfragen zu Ihrem Web app überprüfen. Finden Sie unter [How To konfigurieren TLS gemeinsamen Authentication für Web App](../app-service-web/app-service-web-configure-tls-mutual-auth.md) für zum Einrichten von Codes.

Sie sollten im Abschnitt *Autorisierung* bereitstellen: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Element | Beschreibung |
|---------|-------------|
| Typ | Erforderlich. Art der Authentifizierung. Für SSL-Client-Zertifikate muss der Wert ClientCertificate sein. |
| PFX | Erforderlich. Base64-codierte Inhalt der PFX-Datei. |
| Kennwort | Erforderlich. Kennwort für die PFX-Datei zugreifen. |

### <a name="basic-auth"></a>Standardauthentifizierung

Standardauthentifizierung (z. B. Benutzername und Kennwort) können Sie die eingehenden Anfragen zu überprüfen. Standardauthentifizierung ist ein übliches Schema und können Sie dies tun, in einer beliebigen Sprache, die Sie in Ihrer app erstellen.

Sie sollten im Abschnitt *Autorisierung* bereitstellen: `{"type": "basic","username": "test","password": "test"}`.

| Element | Beschreibung |
|---------|-------------|
| Typ | Erforderlich. Art der Authentifizierung. Bei der Standardauthentifizierung muss der Wert Basic sein. |
| Benutzername | Erforderlich. Der Benutzername zum Authentifizieren. |
| Kennwort | Erforderlich. Kennwort zum Authentifizieren. |

### <a name="handle-aad-auth-in-code"></a>Ziehpunkt AAD autorisierende Code

Standardmäßig führt die Azure Active Directory-Authentifizierung, die Sie im Portal unterstützen keine abgestimmte Autorisierung aus. Es wird beispielsweise nicht Ihre API einen bestimmten Benutzer oder eine app, sondern nur einen bestimmten Mandanten gesperrt.

Wenn Sie die API mit nur der Logik, beispielsweise im Code einschränken möchten können Sie die Kopfzeile extrahieren enthält den JWT und prüfen, wer der Anrufer keine Anfragen zurückgewiesen wird, die nicht entsprechen.

Fortfahren, wenn Sie vollständig in Ihrem eigenen Code zu implementieren und nicht das Feature Portal nutzen möchten, können Sie in diesem Artikel lesen: [Verwenden Sie für die Authentifizierung in der App-Verwaltungsdienst Azure Active Directory](../app-service-web/web-sites-authentication-authorization.md).

Sie müssen immer noch führen Sie die obigen Schritte zum Erstellen einer Anwendungsidentität für Ihre app Logik und verwenden, die die API aufrufen.
