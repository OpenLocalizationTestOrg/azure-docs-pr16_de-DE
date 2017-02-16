<properties
    pageTitle="So Hintergrundinstallation der Azure AD-Anwendung Proxy Verbinder | Microsoft Azure"
    description="Erläutert, wie eine Hintergrundinstallation Azure AD-Anwendung Proxy Verbinders sicheren Remotezugriff auf Ihrem lokalen apps bereitstellen ausführen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-silently-install-the-azure-ad-application-proxy-connector"></a>So Hintergrundinstallation der Azure AD-Anwendung Proxy-Verbinder

Möchten Sie in der Lage, senden eine Installationsskript mehrere Windows Server oder Windows-Servern, die keine Benutzeroberfläche aktiviert haben. In diesem Thema wird erläutert, wie ein Windows PowerShell-Skript zu erstellen, können unbeaufsichtigten Installation installieren und den Azure AD-Anwendung Proxy Verbinder registrieren.

## <a name="enabling-access"></a>Aktivieren des Zugriffs
Anwendungsproxy funktioniert nach der Installation von einem schlanken Windows Server-Dienst bezeichnet den Verbinder in Ihrem Netzwerk. Für die Anwendung Proxy Verbinder entwickelt er muss bei Ihrem Azure AD-Verzeichnis mit einem globalen Administrator und das Kennwort registriert sein. Normalerweise ist dies während der Installation der Verbinder in einem Popup im Dialogfeld eingegeben. Alternativ können Sie mithilfe von Windows PowerShell zum Erstellen eines Objekts Anmeldeinformationen, um Ihre Registrierungsinformationen eingeben, oder Sie können eigene Token erstellen und verwenden sie Ihre Registrierungsinformationen eingeben.

## <a name="step-1--install-the-connector-without-registration"></a>Schritt 1: Installieren des Verbinders ohne Registrierung


Installieren Sie die Verbinder MSIs ohne Registrieren des Connectors wie folgt:


1. Öffnen Sie ein Eingabeaufforderungsfenster.
2. Führen Sie den folgenden Befehl in dem/q automatische Installation - bedeutet fordert die Installation nicht Sie auf den Endbenutzer-Lizenzvertrag anzunehmen.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="step-2-register-the-connector-with-azure-active-directory"></a>Schritt 2: Registrieren des Verbinders Azure-Active Directory
Dies kann erreicht werden, verwenden eine der folgenden Methoden:


- Registrieren des Verbinders mit einem Windows PowerShell-Anmeldeinformationen-Objekt
- Registrieren Sie den Verbinder mit einem Token offline erstellte

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Registrieren des Verbinders mit einem Windows PowerShell-Anmeldeinformationen-Objekt


1. Das Windows PowerShell-Anmeldeinformationen Objekt durch Ausführen der Folgendes erstellen, in dem "<username>"und"<password>" mit den Benutzernamen und das Kennwort für Ihr Verzeichnis ersetzt werden soll:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword

2. Wechseln Sie zu **C:\Programme c:\Programme\Microsoft AAD App Proxy Verbinder** , und führen Sie das Skript, die mithilfe des Objekts der PowerShell-Anmeldeinformationen, die Sie erstellt haben, darin $cred auf den Namen des PowerShell Objekt erstellten Anmeldeinformationen:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred


### <a name="register-the-connector-using-a-token-created-offline"></a>Registrieren Sie den Verbinder mit einem Token offline erstellte

1. Erstellen einer offline Token mithilfe der AuthenticationContext-Klasse mit den Werten in der Codeausschnitt:


        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.windows.net/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }





2. Nachdem Sie das Erstellen eines mit dem Token SecureString Token haben: <br>
`$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`
3. Führen Sie den folgenden Windows PowerShell-Befehl, wobei SecureToken ist der Name der Token, die, dem Sie soeben erstellt haben, und TenantID GUID des Mandanten: <br>
`RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`



## <a name="see-also"></a>Siehe auch

- [Aktivieren Sie die Anwendungsproxy für Azure-Active Directory](active-directory-application-proxy-enable.md)
- [Veröffentlichen von Applications, die Ihren eigenen Domänennamen verwenden](active-directory-application-proxy-custom-domains.md)
- [Aktivieren Sie auf einmalige Anmelden](active-directory-application-proxy-sso-using-kcd.md)
- [Behandeln von Problemen, die Sie mit der Anwendungsproxy haben](active-directory-application-proxy-troubleshoot.md)

Sehen Sie für die neuesten Informationen und Updates sich die [Anwendungsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
