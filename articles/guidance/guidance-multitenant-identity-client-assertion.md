<properties
   pageTitle="Verwendung von Client Assertion zum Abrufen von Access Token aus Azure AD | Microsoft Azure"
   description="Wie Assertion Client um Access Token von Azure AD zu gelangen."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="using-client-assertion-to-get-access-tokens-from-azure-ad"></a>Verwenden Client Assertion zum Access Token aus Azure AD abgerufen werden.

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Es gibt auch eine vollständige [Beispiel-Anwendung] , die dieser Reihe begleitet.

## <a name="background"></a>Hintergrund

Bei Verwendung von Autorisierung Code Fluss oder Hybrid Fluss in OpenID verbinden austauscht Client einen Autorisierungscode für eine Access-Token. In diesem Schritt muss der Client sich auf dem Server zu authentifizieren.

![Geheim Client](media/guidance-multitenant-identity/client-secret.png)

Eine Möglichkeit zum Authentifizieren des Clients ist mithilfe eines Client geheim. Der wie die [Tailspin Umfragen] [ Surveys] Anwendung ist standardmäßig so konfiguriert.

Hier ist eine Beispiel für eine Anforderung vom Client an den IDP, eine Access-Token anfordert. Hinweis Die `client_secret` Parameter.

```
POST https://login.microsoftonline.com/b9bd2162xxx/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded

resource=https://tailspin.onmicrosoft.com/surveys.webapi
  &client_id=87df91dc-63de-4765-8701-b59cc8bd9e11
  &client_secret=i3Bf12Dn...
  &grant_type=authorization_code
  &code=PG8wJG6Y...
```

Das Kennwort ist nur eine Zeichenfolge, so dass Sie nicht den Wert verloren gehen sicherstellen. Die bewährte Methode ist im Client geheim außerhalb des Datenquellen-Steuerelements beibehalten. Wenn Sie Azure bereitstellen, speichert Sie ihn in einer [app-Einstellung][configure-web-app].

Jede Person mit Zugriff auf das Abonnement Azure kann jedoch die Appeinstellungen anzeigen. Darüber hinaus ist immer eine Versuchung, checken Sie Kennwörter in Datenquellen-Steuerelements (z. B. in Bereitstellungsskripts), per e-Mail frei usw..

Zur Erhöhung der Sicherheit können Sie statt einer Client geheim [Client Assertion] verwenden. Mit Client Assertion verwendet der Client eine x 509-Zertifikat um nachzuweisen, dass der Client die Anfrage token stammen. Das Clientzertifikat wird auf dem Webserver installiert. Im Allgemeinen wird es einfacher sein zum Einschränken des Zugriffs auf das Zertifikat, als um sicherzustellen, dass niemand versehentlich ein Geheimnis Client werden. Weitere Informationen zum Konfigurieren von Zertifikaten in einer Web app finden Sie unter [Verwenden von Zertifikaten in Azure Websites Clientanwendungen][using-certs-in-websites]

Hier ist eine token Anforderung Client Assertion verwenden:

```
POST https://login.microsoftonline.com/b9bd2162xxx/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded

resource=https://tailspin.onmicrosoft.com/surveys.webapi
  &client_id=87df91dc-63de-4765-8701-b59cc8bd9e11
  &client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer
  &client_assertion=eyJhbGci...
  &grant_type=authorization_code
  &code= PG8wJG6Y...
```

Beachten Sie, dass die `client_secret` Parameter wird nicht mehr verwendet. In diesem Fall die `client_assertion` Parameter enthält ein JWT Token, die mit dem Client-Zertifikat signiert wurde. Die `client_assertion_type` Parameter bestimmt den Typ der Assertion &mdash; in diesem Fall JWT Token. Der Server überprüft das JWT Token. Wenn das Token JWT ungültig ist, wird die Anfrage token ein Fehler zurückgegeben.

> [AZURE.NOTE] X. 509-Zertifikate sind nicht die einzige Form der Client Assertion. Wir möchten sie hier, da es von Azure AD unterstützt wird.

## <a name="using-client-assertion-in-the-surveys-application"></a>Verwenden von Client Assertion in der Anwendung Umfragen

In diesem Abschnitt wird gezeigt, wie die Anwendung Tailspin Umfragen mit Client Assertion konfigurieren. In den folgenden Schritten generiert ein selbst signiertes Zertifikat, das für die Entwicklung geeignet ist, aber nicht für die Herstellung verwenden.

1. Ausführen der PowerShell-Skript [/Scripts/Setup-KeyVault.ps1] [ Setup-KeyVault] wie folgt:

    ```
    .\Setup-KeyVault.ps -Subject [subject]
    ```

    Für die `Subject` Parameter, geben Sie einen beliebigen Namen ein, beispielsweise "Surveysapp". Das Skript ein selbst signiertes Zertifikat generiert und im Zertifikatspeicher "Aktueller Benutzer/eigene" gespeichert wird.

2. Die Ausgabe des Skripts ist ein JSON-Fragment. Fügen Sie diese wie folgt auf das Anwendungsmanifest des Web-app:

    1. Melden Sie sich bei der [Azure-Verwaltungsportal] [ azure-management-portal] und navigieren Sie zu Ihrem Verzeichnis Azure AD-.

    2. Klicken Sie auf **Anwendungen**.

    3. Wählen Sie die Anwendung Umfragen.

    4.  Klicken Sie auf **Manifest verwalten** , und wählen Sie die **Manifest herunterladen**.

    5.  Öffnen Sie die JSON-Manifestdatei in einem Text-Editor ein. Fügen Sie das Skript in die Ausgabe der `keyCredentials` Eigenschaft. Es sollte etwa wie folgt aussehen:

        ```    
        "keyCredentials": [
            {
              "type": "AsymmetricX509Cert",
              "usage": "Verify",
              "keyId": "29d4f7db-0539-455e-b708-....",
              "customKeyIdentifier": "ZEPpP/+KJe2fVDBNaPNOTDoJMac=",
              "value": "MIIDAjCCAeqgAwIBAgIQFxeRiU59eL.....
            }
          ],
         ```

    6.  Speichern der Änderungen auf die JSON-Datei.

    7.  Wechseln Sie wieder zum Portal. Klicken Sie auf **Verwalten Manifest** > **Manifest hochladen** und Hochladen der JSON-Datei.

3. Führen Sie den folgenden Befehl aus, um den Fingerabdruck des Zertifikats erhalten.

    ```
    certutil -store -user my [subject]
    ```

    wo `[subject]` ist der Wert, den Sie in der PowerShell-Skript zum Thema angegeben. Der Fingerabdruck ist unter "Zertifikat Hash(SHA1):" aufgeführt. Entfernen Sie die Leerzeichen zwischen den hexadezimalen Zahlen.

4. Aktualisieren Sie Ihre app vertraulichen Daten. Klicken Sie im Explorer-Lösung mit der rechten Maustaste in des Projekts Tailspin.Surveys.Web, und wählen Sie **Vertrauliche von Benutzerdaten verwalten**. Hinzufügen eines Eintrags für "Asymmetrischen" unter "AzureAd", ein, wie unten dargestellt:

    ```
    {
      "AzureAd": {
        "ClientId": "[Surveys application client ID]",
        // "ClientSecret": "[client secret]",  << Delete this entry
        "PostLogoutRedirectUri": "https://localhost:44300/",
        "WebApiResourceId": "[App ID URI of your Survey.WebAPI application]",
        // new:
        "Asymmetric": {
          "CertificateThumbprint": "[certificate thumbprint]",  // Example: "105b2ff3bc842c53582661716db1b7cdc6b43ec9"
          "StoreName": "My",
          "StoreLocation": "CurrentUser",
          "ValidationRequired": "false"
        }
      },
      "Redis": {
        "Configuration": "[Redis connection string]"
      }
    }
    ```

    Sie müssen festlegen `ValidationRequired` auf false, da das Zertifikat nicht von einem Stamm-CA Zertifizierungsstelle signiert wurde. Verwenden Sie ein Zertifikat, das von einer Zertifizierungsstelle Authority angemeldet ist, und legen Sie in der Herstellung, `ValidationRequired` true.

    Löschen Sie auch den Eintrag für `ClientSecret`, da sie nicht mit Client Assertion benötigt wird.

5. Suchen Sie den Code, der registriert Startup.cs, die `ICredentialService`. Kommentieren Sie die Zeile, die verwendet `CertificateCredentialService`, und kommentieren Sie die Zeile, die verwendet `ClientCredentialService`:

    ```csharp
    // Uncomment this:
    services.AddSingleton<ICredentialService, CertificateCredentialService>();
    // Comment out this:
    //services.AddSingleton<ICredentialService, ClientCredentialService>();
    ```

Zur Laufzeit liest die Webanwendung das Zertifikat aus dem Zertifikat Store. Das Zertifikat muss auf demselben Computer wie das Web app installiert sein.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den nächsten Artikel in dieser Reihe: [Mithilfe von Azure Schlüssel Tresor Anwendung Kennwörter schützen][key vault]


<!-- Links -->
[configure-web-app]: ../app-service-web/web-sites-configure.md
[azure-management-portal]: https://manage.windowsazure.com
[Client-assertion]: https://tools.ietf.org/html/rfc7521
[key vault]: guidance-multitenant-identity-keyvault.md
[Setup-KeyVault]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/scripts/Setup-KeyVault.ps1
[Surveys]: guidance-multitenant-identity-tailspin.md
[using-certs-in-websites]: https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/
[Teil einer Serie]: guidance-multitenant-identity.md
[Beispiel-Anwendung]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
