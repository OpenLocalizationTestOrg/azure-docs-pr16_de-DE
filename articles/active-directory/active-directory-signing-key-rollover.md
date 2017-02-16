<properties
    pageTitle="Azure AD Key Rollover anmelden | Microsoft Azure"
    description="Dieser Artikel erläutert die signierende Key Rollover bewährte Methoden für Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Bei der Anmeldung Key Rollover in Azure Active Directory

In diesem Thema wird erläutert, Sie müssen die öffentlichen Schlüssel kennen, die sich Sicherheitstokens in Azure Active Directory (Azure AD) verwendet werden. Es ist wichtig, beachten Sie, dass diese Tasten Rollover in regelmäßigen Abständen und in dringenden, konnte über unmittelbar bereitgestellt werden. Alle Programme, die Azure AD verwenden sollten möglicherweise programmgesteuert verarbeitet den wichtigsten Rollover Prozess oder Festlegen eines Prozesses periodisch manuelle Rollover. Um zu verstehen, wie die Tasten arbeiten, lesen Sie weiter, wie Sie die Auswirkung der der bewegen Sie die Maus Ihrer Anwendung beurteilen und so aktualisieren Sie die Anwendung oder Festlegen eines Prozesses periodisch manuellen Rollover zum Behandeln von Key Rollover bei Bedarf.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Übersicht über Azure AD Tasten anmelden

Azure AD verwendet öffentlichen Schlüsseln Branchenstandards zum Herstellen von Vertrauensstellung zwischen sich selbst und die Programme, die sie verwenden. Praktisch dies funktioniert in folgender Weise: Azure AD verwendet einen signierenden Schlüssel, die aus einem öffentlichen und privaten Schlüssel Paar besteht. Wenn ein Benutzer zur Anwendung, die für die Authentifizierung Azure AD verwendet anmeldet, erstellt Azure AD ein Sicherheitstoken, die Informationen über den Benutzer enthält. Dieses Token ist signiert von Azure Active Directory mithilfe des privaten Schlüssels aus, bevor sie an die Anwendung gesendet wird. Zum Überprüfen, ob das Token gültig und stammt tatsächlich aus Azure AD ist, muss die Anwendung überprüft werden das Token der Signatur mit dem öffentlichen Schlüssel von Azure AD, die in des Mandantenverwaltungs [OpenID verbinden Discovery-Dokument](http://openid.net/specs/openid-connect-discovery-1_0.html) oder SAML/WS-eingezogen [Föderation Metadatendokument](active-directory-federation-metadata.md)enthalten sind verfügbar gemacht werden.

Aus Gründen der Sicherheit konnte Azure AD-Signieren Key Rollen in regelmäßigen Abständen und im Falle dringenden, zusammengefasst werden über sofort. Eine Anwendung, die mit Azure AD integriert soll den anderen Teilnehmern Behandeln eines Ereignisses Key Rollover unabhängig davon, wie häufig sie auftreten kann. Wenn dies nicht der Fall und Ihrer Anwendung versucht, einen nicht mehr gültige Schlüssel zum Überprüfen der Signatur ein Token verwenden, wird die Anforderung Anmeldung fehl.

Es gibt immer mehr als einen gültiger Key OpenID verbinden Discovery-Dokuments und das Dokument Föderation Metadaten zur Verfügung. Die Anwendung sollte bereiten Sie einen der Schlüssel der angegebenen im Dokument zu verwenden, da nacheinander bald rückgängig gemacht werden könnte, ein anderes möglicherweise werden dem neuen usw..

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>So bewerten, wenn eine Anwendung betroffen ist und was Sie dagegen tun

Wie eine Anwendung Key Rollover behandelt, hängt von Variablen wie den Typ der Anwendung oder welche Identität Protokoll und Bibliothek verwendet wurde. In den folgenden Abschnitten prüfen, ob die am häufigsten verwendeten Typen von Applications hängen davon ab, die wichtigsten Rollover und bieten einen Leitfaden zum Aktualisieren der Anwendungs unterstützen automatische Rollover oder manuell aktualisieren die-Taste.

* [Zugreifen auf Ressourcen systemeigenen Clientanwendungen](#nativeclient)
* [Web Applications / APIs den Zugriff auf Ressourcen](#webclient)
* [Web Applications / APIs Ressourcen schützen und mithilfe von Azure App Services erstellt](#appservices)
* [Web Applications / Schützen von Ressourcen mithilfe von .NET OWIN OpenID verbinden, WS-eingezogen oder WindowsAzureActiveDirectoryBearerAuthentication Middleware-APIs](#owin)
* [Web Applications / Schützen von Ressourcen mithilfe von .NET Core OpenID verbinden oder JwtBearerAuthentication Middleware-APIs](#owincore)
* [Web Applications / APIs Schützen von Ressourcen mithilfe von Node.js Passport-Azure-Active Directory-Modul](#passport)
* [Web Applications / APIs Ressourcen schützen und erstellt mit Visual Studio 2015](#vs2015)
* [Webanwendungen Ressourcen schützen und erstellt mit Visual Studio 2013](#vs2013)
* [Schützen von Ressourcen und mit Visual Studio 2013 erstellte Web-APIs](#vs2013_webapi)
* [Webanwendungen Ressourcen schützen und erstellt mit Visual Studio 2012](#vs2012)
* [Webanwendungen Ressourcen schützen und mit Visual Studio 2010, 2008 o mit Windows Identität Foundation erstellt](#vs2010)
* [Web Applications / APIs Schützen von Ressourcen an eine beliebige andere Bibliotheken oder manuelles Implementieren eines der unterstützten Protokolle](#other)

Dieser Leitfaden ist **nicht** verfügbar für:

* Applications aus Azure AD-Anwendung Katalog (einschließlich benutzerdefinierte) hinzugefügt haben, separaten Anleitungen im Hinblick auf Schlüssel signieren. [Weitere Informationen.](active-directory-sso-certs.md)
* Lokale Applikationen über einen Proxyserver Anwendung veröffentlicht zur Registrierung von Tasten sorgen besitzen.

### <a name="a-namenativeclientanative-client-applications-accessing-resources"></a><a name="nativeclient"></a>Zugreifen auf Ressourcen systemeigenen Clientanwendungen

Programme, die nur Ressourcen (z. B. zugreifen Microsoft Graph, KeyVault, Outlook-API und andere Microsoft-APIs) im Allgemeinen nur ein Token abrufen und geben Sie diese an dem Ressourcenbesitzer. Vorausgesetzt, dass diese Ressourcen nicht geschützt werden, sie überprüfen das Token nicht und können daher nicht mehr Stellen Sie sicher, dass diese ordnungsgemäß signiert ist.

Systemeigene-Clientanwendungen, ob desktop oder mobilen, fallen in diese Kategorie, und somit nicht durch das Rollover beeinträchtigt werden.

### <a name="a-namewebclientaweb-applications--apis-accessing-resources"></a><a name="webclient"></a>Web Applications / APIs den Zugriff auf Ressourcen

Programme, die nur Ressourcen (z. B. zugreifen Microsoft Graph, KeyVault, Outlook-API und andere Microsoft-APIs) im Allgemeinen nur ein Token abrufen und geben Sie diese an dem Ressourcenbesitzer. Vorausgesetzt, dass diese Ressourcen nicht geschützt werden, sie überprüfen das Token nicht und können daher nicht mehr Stellen Sie sicher, dass diese ordnungsgemäß signiert ist.

Web-Applikationen und web-APIs, die app nur illustrieren verwenden (Clientanmeldeinformationen / Client-Zertifikat), fallen in diese Kategorie und somit nicht durch das Rollover beeinträchtigt werden.

### <a name="a-nameappservicesaweb-applications--apis-protecting-resources-and-built-using-azure-app-services"></a><a name="appservices"></a>Web Applications / APIs Ressourcen schützen und mithilfe von Azure App Services erstellt

Azure App Services-Authentifizierung / Autorisierung (EasyAuth) Funktionalität weist bereits die notwendigen Logik Key Rollover automatisch zu behandeln.

### <a name="a-nameowinaweb-applications--apis-protecting-resources-using-net-owin-openid-connect-ws-fed-or-windowsazureactivedirectorybearerauthentication-middleware"></a><a name="owin"></a>Web Applications / Schützen von Ressourcen mithilfe von .NET OWIN OpenID verbinden, WS-eingezogen oder WindowsAzureActiveDirectoryBearerAuthentication Middleware-APIs

Wenn in Ihrer Anwendung die .NET OWIN OpenID verbinden, WS-eingezogen oder WindowsAzureActiveDirectoryBearerAuthentication Middleware, verwendet wird, muss es bereits die notwendigen Logik Key Rollover automatisch verarbeitet.

Sie können bestätigen, dass die Anwendung der durch Suchen Sie nach einer der folgenden Codeausschnitte in Ihrer Anwendungs Startup.cs oder Startup.Auth.cs verwendet wird

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="a-nameowincoreaweb-applications--apis-protecting-resources-using-net-core-openid-connect-or--jwtbearerauthentication-middleware"></a><a name="owincore"></a>Web Applications / Schützen von Ressourcen mithilfe von .NET Core OpenID verbinden oder JwtBearerAuthentication Middleware-APIs

Wenn in Ihrer Anwendung die Middleware .NET Core OWIN OpenID verbinden oder JwtBearerAuthentication verwendet wird, muss es bereits die notwendigen Logik Key Rollover automatisch zu behandeln.

Sie können bestätigen, dass die Anwendung der durch Suchen Sie nach einer der folgenden Codeausschnitte in Ihrer Anwendungs Startup.cs oder Startup.Auth.cs verwendet wird

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="a-namepassportaweb-applications--apis-protecting-resources-using-nodejs-passport-azure-ad-module"></a><a name="passport"></a>Web Applications / APIs Schützen von Ressourcen mithilfe von Node.js Passport-Azure-Active Directory-Modul

Wenn in Ihrer Anwendung das Node.js Passport-Ad-Modul verwendet wird, muss es bereits die notwendigen Logik Key Rollover automatisch zu behandeln.

Bestätigen Sie, dass Ihre Anwendung Passport-Anzeige durch Suchen nach den folgenden Codeausschnitt in Ihrer Anwendung app.js

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="a-namevs2015aweb-applications--apis-protecting-resources-and-created-with-visual-studio-2015"></a><a name="vs2015"></a>Web Applications / APIs Ressourcen schützen und erstellt mit Visual Studio 2015

Wenn eine Anwendung erstellt wurde, verwenden die Vorlage eine Web-Anwendung in Visual Studio 2015 und **Arbeit und Schule Konten** aus dem Menü **Ändern Authentifizierung** ausgewählt haben, muss es bereits die notwendigen Logik Key Rollover automatisch zu behandeln. Diese Logik, eingebettet in die Middleware OWIN OpenID verbinden Ruft die Tasten aus dem OpenID verbinden Discovery-Dokument und zwischengespeichert regelmäßig aktualisiert werden.

Wenn Sie Authentifizierung manuell zu Ihrer Lösung hinzugefügt, möglicherweise die Anwendung nicht die erforderlichen Key Rollover Logik verfügen. Sie müssen selbst schreiben, oder führen Sie die Schritte in [Web Applications / APIs eine beliebige andere Bibliotheken oder manuelles Implementieren eines der unterstützten Protokolle.](#other).

### <a name="a-namevs2013aweb-applications-protecting-resources-and-created-with-visual-studio-2013"></a><a name="vs2013"></a>Webanwendungen Ressourcen schützen und erstellt mit Visual Studio 2013

Wenn eine Anwendung erstellt wurde, verwenden die Vorlage eine Web-Anwendung in Visual Studio 2013 und **Organisationskonten** aus dem Menü **Ändern Authentifizierung** ausgewählt haben, muss es bereits die notwendigen Logik Key Rollover automatisch zu behandeln. Diese Logik speichert Ihrer Organisation eindeutigen Bezeichner sowie die signierende Informationen in zwei Tabellen mit dem Projekt verknüpft ist. Sie finden die Verbindungszeichenfolge für die Datenbank in der Datei Web.config des Projekts.

Wenn Sie Authentifizierung manuell zu Ihrer Lösung hinzugefügt, möglicherweise die Anwendung nicht die erforderlichen Key Rollover Logik verfügen. Sie müssen selbst schreiben, oder führen Sie die Schritte in [Web Applications / APIs eine beliebige andere Bibliotheken oder manuelles Implementieren eines der unterstützten Protokolle.](#other).

Die folgenden Schritte helfen Ihnen, stellen Sie sicher, dass die Logik in Ihrer Anwendung ordnungsgemäß arbeitet.

1. Öffnen Sie die Lösung in Visual Studio 2013, und klicken Sie dann auf die Registerkarte **Server-Explorer** , klicken Sie im rechten.
2. Erweitern Sie **Datenverbindungen**, **DefaultConnection**und klicken Sie dann auf **Tabellen**. Suchen Sie nach der Tabelle **IssuingAuthorityKeys** , der rechten Maustaste darauf, und klicken Sie dann auf **Tabellendaten anzeigen**.
3. In der Tabelle **IssuingAuthorityKeys** werden mindestens eine Zeile, die den Fingerabdruckwert für den Schlüssel entspricht. Löschen Sie alle Zeilen in der Tabelle ein.
4. Mit der rechten Maustaste in der Tabelle **Mandanten** , und klicken Sie dann auf **Tabellendaten anzeigen**.
5. In der Tabelle **Mandanten** werden mindestens eine Zeile, die ein eindeutiges Verzeichnis Mandanten Bezeichner entspricht. Löschen Sie alle Zeilen in der Tabelle ein. Wenn Sie die Zeilen in der Tabelle **Mandanten** und die **IssuingAuthorityKeys** Tabelle nicht löschen, erhalten Sie zur Laufzeit einen Fehler zurück.
6. Erstellen Sie, und führen Sie die Anwendung. Nachdem Sie sich bei Ihrem Konto angemeldet haben, können Sie die Anwendung beenden.
7. Zurück zu den **Server-Explorer** , und schauen Sie sich die Werte in der Tabelle **IssuingAuthorityKeys** und **Mandanten** . Sie sehen, dass er automatisch mit den entsprechenden Informationen aus dem Föderation Metadatendokument aufgefüllt wurde.

### <a name="a-namevs2013aweb-apis-protecting-resources-and-created-with-visual-studio-2013"></a><a name="vs2013"></a>Schützen von Ressourcen und mit Visual Studio 2013 erstellte Web-APIs

Wenn Sie eine Web-API-Anwendung in Visual Studio 2013 mithilfe der Web-API Vorlage erstellt und dann aus dem Menü **Ändern Authentifizierung** **Organisationskonten ausgewählt** , besitzen Sie bereits die notwendigen Logik in Ihrer Anwendung.

Wenn Sie manuell Authentifizierung konfiguriert haben, führen Sie die folgenden beschrieben vor, um Informationen zum Konfigurieren der Web-API, um Informationen für den Schlüssel automatisch zu aktualisieren.

Der folgende Codeausschnitt veranschaulicht, wie erhalten die neuesten Schlüssel aus dem Föderation Metadatendokument, und klicken Sie dann den [JWT Token Ereignishandler](https://msdn.microsoft.com/library/dn205065.aspx) verwenden, um das Token zu überprüfen. Der Codeausschnitt wird vorausgesetzt, dass es sich bei Ihrer eigenen Zwischenspeicherungsmechanismus vorgesehenen zum Beibehalten des Schlüssels aus Azure AD zukünftige Token überprüft sei es in einer Datenbank, Konfigurationsdatei oder an anderer Stelle.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="a-namevs2012aweb-applications-protecting-resources-and-created-with-visual-studio-2012"></a><a name="vs2012"></a>Webanwendungen Ressourcen schützen und erstellt mit Visual Studio 2012

Wenn die Anwendung in Visual Studio 2012 erstellt wurde, verwendet Sie wahrscheinlich die Identität und Access-Tool zum Konfigurieren Ihrer Anwendungs. Es ist es wahrscheinlich, dass Sie die [Überprüfung Herausgeber Namen Registrierung (VINR)](https://msdn.microsoft.com/library/dn205067.aspx)verwenden. Die VINR ist verantwortlich für die Verwaltung von Informationen zu vertrauenswürdigen Identitätsanbieter (Azure AD-) und die Tasten verwendet Token ausgestellt von ihnen zu bestätigen. Die VINR vereinfacht außerdem die Informationen in der Datei Web.config gespeichert, durch das neueste Ihres Verzeichnisses zugeordnete Föderation Metadatendokument herunterladen automatisch aktualisiert wird überprüft, ob die Konfiguration mit dem neuesten Dokument älter ist, und aktualisieren die Anwendung mit den neuen Schlüssel nach Bedarf.

Wenn Sie eine Anwendung verhindert das Codebeispielen oder Exemplarische Vorgehensweise Dokumentation von Microsoft bereitgestellt erstellt haben, ist die wichtigsten Rollover Logik bereits in Ihrem Projekt enthalten. Beachten Sie, dass der Code unten im Projekt bereits vorhanden ist. Wenn die Anwendung nicht diese Logik noch, folgen Sie den Schritten unten, um es hinzuzufügen oder zu überprüfen, ob es ordnungsgemäß funktioniert.

1. Fügen Sie im **Explorer Lösung**einen Verweis auf die Assembly **System.IdentityModel** für das entsprechende Projekt aus.
2. Öffnen Sie die Datei **Global.asax.cs** und fügen Sie die folgenden Richtlinien verwenden:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Fügen Sie die folgende Methode, um die Datei **Global.asax.cs** :
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Rufen Sie die Methode **RefreshValidationSettings()** in der **Application_Start()** -Methode in **Global.asax.cs** wie dargestellt:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Nachdem Sie diese Schritte ausgeführt haben, wird die Web.config Ihrer Anwendung mit den neuesten Informationen aus dem Dokument Föderation Metadaten, einschließlich der neuesten Schlüssel aktualisiert werden. Dieses Update tritt auf jedem Ihrer Anwendung Ressourcenpool in IIS-Freigabe; Standardmäßig ist IIS auf Papierkorb von Applications jeder 29 Stunden festgelegt.

Gehen Sie folgendermaßen vor, um sicherzustellen, dass die wichtigsten Rollover Logik arbeitet.

1. Nachdem Sie überprüft haben, dass der Anwendung den obigen Code verwendet wird, öffnen Sie die Datei **Web.config** , und navigieren Sie zu der **<issuerNameRegistry>** blockieren, insbesondere suchen Sie nach den folgenden Zeilen:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. In der **<add thumbprint=””>** festlegen, ändern Sie den Fingerabdruckwert, indem Sie ein Zeichen durch einen anderen ersetzen. Speichern Sie die Datei **Web.config** .

3. Erstellen Sie die Anwendung, und führen Sie es. Wenn Sie den Vorgang abschließen können, ist eine Anwendung die Taste erfolgreich aktualisieren, indem Sie die erforderliche Informationen aus Ihrem Verzeichnis des Föderation Metadatendokument herunterladen. Wenn Sie Probleme bei der Anmeldung haben, vergewissern Sie sich die Änderungen in Ihrer Anwendung korrekt sind, indem Sie im Thema [Hinzufügen melden Sie sich für den Zugriff auf Ihre Web-Anwendung verwenden Azure AD-](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) lesen oder herunterladen und prüfen im folgenden Beispiel: [Mehrere Mandanten Cloud-Anwendung für Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="a-namevs2010aweb-applications-protecting-resources-and-created-with-visual-studio-2008-or-2010-and-windows-identity-foundation-wif-v10-for-net-35"></a><a name="vs2010"></a>Webanwendungen Ressourcen schützen und mit Visual Studio 2008 oder 2010 erstellt und Windows Identität Foundation (WIF), Version 1.0 für .NET 3.5

Wenn Sie eine Anwendung auf WIF Version 1.0 erstellt, gibt es keine bereitgestellten Verfahren Anwendungskonfiguration um einen neuen Product Key zu verwenden automatisch zu aktualisieren.

- *Am einfachsten* Verwenden der FedUtil Werkzeuge im SDK WIF, die das neueste Metadatendokument abrufen und aktualisieren Sie Ihre Konfiguration können enthalten.
- Aktualisieren Sie Ihrer Anwendung auf .NET 4.5, wozu auch die neueste Version von WIF befindet sich im System-Namespace an. Die [Überprüfung Herausgeber Namen Registrierung (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) können dann durchführen von automatischen Updates für die Anwendungskonfiguration.
- Führen Sie eine manuelle Rollover gemäß den Anweisungen am Ende dieses Dokuments Anleitungen.

Anweisungen, um die FedUtil verwenden, um die Konfiguration zu aktualisieren:

1. Stellen Sie sicher, dass Sie die WIF Version 1.0 SDK auf Ihrem Entwicklungscomputer für Visual Studio 2008 oder 2010 installiert haben. Sie können [hier herunterladen](https://www.microsoft.com/en-us/download/details.aspx?id=4451) , wenn Sie nicht noch nicht installiert haben.
2. Öffnen Sie in Visual Studio die Lösung, mit der rechten Maustaste in des anwendbare Projekts, und wählen Sie **Föderation Metadaten zu aktualisieren**. Wenn diese Option nicht verfügbar ist, wurde FedUtil und/oder das WIF Version 1.0 SDK nicht installiert.
3. Wählen Sie dazu aufgefordert werden, **Update** zum Aktualisieren der Metadaten Ihrer Föderation zu beginnen. Wenn Sie Zugriff auf die Server-Umgebung haben, in dem die Anwendung gehostet wird, können Sie optional FedUtils [automatischen Metadaten Scheduler aktualisiert](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klicken Sie auf **Fertig stellen** , um den Aktualisierungsvorgang abzuschließen.

### <a name="a-nameotheraweb-applications--apis-protecting-resources-using-any-other-libraries-or-manually-implementing-any-of-the-supported-protocols"></a><a name="other"></a>Web Applications / APIs Schützen von Ressourcen an eine beliebige andere Bibliotheken oder manuelles Implementieren eines der unterstützten Protokolle

Wenn Sie eine der unterstützten Protokolle manuell implementiert oder eine anderen Bibliothek verwenden, müssen Sie die Bibliothek oder die Implementierung, um sicherzustellen, dass die Taste aus OpenID verbinden Discovery-Dokuments oder der Föderation Metadatendokument Abrufens überprüfen. Eine Methode, um dies zu überprüfen, ist eine Suche in Ihrem oder der Bibliothek den Code für alle aufrufen OpenID Discovery-Dokuments oder der Föderation Metadatendokument ab.

Wenn sie Key dort gespeichert wird oder hartcodierte in Ihrer Anwendung, Sie manuell können abgerufen Sie werden die Taste und aktualisieren sie entsprechend durch Ausführen ein manuelles Rollovers gemäß den Anweisungen am Ende dieses Dokuments Anleitungen. **Es wird dringend empfohlen, dass Sie Ihrer Anwendung zur Unterstützung von automatischen Rollover optimieren** verwenden eine der Vorgehensweisen in diesem Artikel zur Vermeidung von zukünftiger Ausbluten Gliedern und Aufwand, wenn Azure AD erhöht Rollover Trittfrequenz ist oder weist eine notrufdienste Out-of-Band-Rollover.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>So testen Sie die Anwendung, um festzustellen, ob sie betroffen sind

Sie können überprüfen, ob es sich bei Ihrer Anwendung automatische Key Rollover unterstützt, indem Sie die Skripts herunterladen und wie im folgenden beschrieben [dieses Repository GitHub.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Wie Sie eine manuelle Rollover ausführen, wenn Sie die Anwendung automatische Rollover nicht unterstützt

Wenn eine Anwendung **nicht** die automatische Rollover Unterstützung bedeutet, müssen Sie einen Prozess einzurichten, der regelmäßig überwacht Azure AD-signierenden Schlüssel und führt eine manuelle Rollover entsprechend. [Diese GitHub Repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) enthält Skripts und Anweisungen zum Zweck.
