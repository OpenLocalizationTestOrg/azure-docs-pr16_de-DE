<properties
    pageTitle="Verwenden Azure Key Tresor aus einer Webanwendung | Microsoft Azure"
    description="Mithilfe dieses Lernprogramms können Sie Informationen zum Verwenden von Azure-Taste Tresor aus einer Webanwendung."
    services="key-vault"
    documentationCenter=""
    authors="adhurwit"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-azure-key-vault-from-a-web-application"></a>Verwenden von Azure Key Tresor aus einer Webanwendung #

## <a name="introduction"></a>Einführung  
Mithilfe dieses Lernprogramms können Sie Informationen zum Verwenden von Azure-Taste Tresor aus einer Webanwendung in Azure. Es führt Sie durch das Verfahren zum Zugreifen auf ein Geheimnis aus Tresor ein Azure-Taste, so, dass sie in der Webanwendung verwendet werden kann.

**Geschätzte Dauer:** 15 Minuten


Überblick über die Taste Tresor Azure erhalten finden Sie unter [Neuigkeiten Azure-Taste Tresor?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramms abgeschlossen haben, müssen Sie Folgendes:

- Ein URI einen geheimen in einer Azure-Taste Tresor
- Eine Client-ID und ein Geheimnis Client für eine Webanwendung registriert Azure Active Directory, die auf Ihrem Tresor Schlüssel zugreifen kann
- Eine Webanwendung. Wir werden die Schritte für eine ASP.NET-MVC-Anwendung bereitgestellt in Azure als Web App angezeigt werden.

> [AZURE.NOTE]  Es ist wichtig, dass Sie die Schritte in diesem Lernprogramm in [Erste Schritte mit Azure Schlüssel Tresor](key-vault-get-started.md) aufgeführt, damit Sie den URI zu einer geheim und den Client-ID und den Client Schlüssel für eine Webanwendung haben beendet haben.

Die Webanwendung, die die Taste Tresor zugreifen wird, die in Azure Active Directory registriert ist, und hat Zugriff auf den Schlüssel Tresor gewährt wurde. Wenn dies nicht der Fall ist, kehren Sie zum Registrieren einer Anwendungs in das erste Schritte-Lernprogramm, und wiederholen Sie die Schritte aufgelistet.

In diesem Lernprogramm dient für Web-Entwickler, bei denen die Grundlagen zum Erstellen von Webanwendungen auf Azure zu verstehen. Weitere Informationen zu Azure Web Apps finden Sie unter [Übersicht über die Web Apps](../app-service-web/app-service-web-overview.md).



## <a name="a-idpackagesaadd-nuget-packages"></a><a id="packages"></a>Hinzufügen von Nuget-Paketen ##
Es gibt zwei Pakete, die die Webanwendung muss installiert haben.

- Active Directory-Authentifizierung Library - enthält Methoden für die Interaktion mit Azure Active Directory und Verwalten von der Identität des Benutzers
- Azure Schlüssel Tresor Bibliothek - enthält Methoden für die Interaktion mit Azure-Taste Tresor


Beide der folgenden Pakete können mithilfe der Paket-Manager-Konsole mit dem Befehl Installationspaket installiert sein.

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a name="a-idwebconfigamodify-webconfig"></a><a id="webconfig"></a>Web.Config ändern ##
Es gibt drei Anwendungseinstellungen, die die Datei web.config wie folgt hinzugefügt werden müssen.

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Wenn Sie nicht zum Hosten der Anwendung als eine Azure Web App abgelegt werden, sollten Sie der tatsächlichen ClientId, Client geheim und geheim URI-Werte der Web.config-Datei hinzufügen. Lassen Sie andernfalls diese fiktiven Werten auf, da wir der tatsächlichen Werte im Portal Azure für eine weitere Sicherheitsebene hinzufügen.


## <a name="a-idgettokenaadd-method-to-get-an-access-token"></a><a id="gettoken"></a>Add-Methode, um eine Access Token erhalten ##
Verwenden Sie die Taste Tresor-API benötigen Sie eine Access-Token. Der Schlüssel Tresor-Client übernimmt die Taste Tresor-API aufgerufen, aber müssen Sie es mit einer Funktion angeben, die das Access-Token erhält.  

Es folgt der Code eine Access von Azure Active Directory token zu gelangen. In diesem Code kann an einer beliebigen Stelle in der Anwendung wechseln. Kann ich möchten eine Klasse Utils oder EncryptionHelper hinzufügen.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [AZURE.NOTE] 
> Mit einer Client-ID und Client geheim ist die einfachste Möglichkeit zur Authentifizierung einer Azure AD-Anwendung. Und verwenden sie in der Webanwendung ermöglicht eine Trennung der Aufgaben und mehr Kontrolle über Ihre Key-Verwaltung. Doch es beruht auf mit der Client geheim in Ihrer Konfiguration-Einstellung, die für einige als mit der geheim, die Sie schützen, in der Konfiguration Einstellungen möchten wie riskant sein können. Nachfolgend finden Sie eine Diskussion zum eine Client-ID und das Zertifikat statt Client-ID und Client geheim zu verwenden, um die Azure AD-Anwendung zu authentifizieren.



## <a name="a-idappstartaretrieve-the-secret-on-application-start"></a><a id="appstart"></a>Rufen Sie das Geheimnis auf Anwendung starten ##
Nun benötigen wir Code die Taste Tresor-API aufrufen, und rufen Sie das Geheimnis aus. Mit dem folgende Code kann an einer beliebigen Stelle abgelegt werden, solange sie aufgerufen wird, bevor Sie es verwenden müssen. Ich haben diesen Code in das Ereignis Anwendung zu starten, in der Datei Global.asax setzen, damit es beim Start einmal ausgeführt wird und das Kennwort für die Anwendung bereitgestellt.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]).Result.Value;

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec;



## <a name="a-idportalsettingsaadd-app-settings-in-the-azure-portal-optional"></a><a id="portalsettings"></a>Hinzufügen von App-Einstellungen im Azure-Portal (optional) ##
Wenn Sie einer Azure Web App haben, können Sie jetzt der tatsächlichen Werte für die AppSettings Azure-Portal hinzufügen. Auf diese Weise die ist-Werte werden nicht in der Datei web.config jedoch geschützt über das Portal, wo Sie Steuerelement-Funktionen eigenen Zugriff haben. Diese Werte werden für die Werte ersetzt, die Sie in web.config eingegeben haben. Stellen Sie sicher, dass die Namen übereinstimmen.

![Anwendungseinstellungen Azure-Portal angezeigt][1]


## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Mit einem Zertifikat anstelle eines Client geheim authentifizieren
Eine weitere Möglichkeit zur Authentifizierung einer Azure AD-Anwendung wird mithilfe einer Client-ID und ein Zertifikat anstelle eines Client-ID und Client geheim. Im folgenden werden die Schritte in einer Azure Web App ein Zertifikat verwenden:

1. Erhalten Sie, oder erstellen Sie ein Zertifikat
2. Ordnen Sie das Zertifikat mit einer Azure AD-Anwendung
3. Hinzufügen von Code zu Web App das Zertifikat verwenden
4. Hinzufügen eines Zertifikats zur Web App


**Erhalten, oder erstellen Sie ein Zertifikat** Für unsere Zwecke werden wir ein Testzertifikat vornehmen. Hier sind ein paar Befehle, die Sie zum Erstellen eines Zertifikats ein Eingabeaufforderungsfenster Entwicklertools verwenden können. Wechseln Sie zu der Stelle, an der die Zertifikat-Dateien erstellt werden sollen.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 07/31/2015 -e 07/31/2016 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Notieren Sie sich das Enddatum und das Kennwort für die PFX-Datei (in diesem Beispiel: 07/31/2016 und test123). Sie benötigen können unten.

Weitere Informationen zum Erstellen eines Testzertifikats finden Sie unter [So: Erstellen Ihrer eigenen Testzertifikat](https://msdn.microsoft.com/library/ff699202.aspx)


**Ordnen Sie das Zertifikat mit einer Azure AD-Anwendung** Jetzt, da Sie ein Zertifikat haben, müssen Sie diese Anwendung Azure AD-zugeordnet werden. Aber der Azure-Verwaltungsportal unterstützt dies nicht sofort. Stattdessen müssen Sie Powershell verwenden möchten. Im folgenden sind die Befehle, die Sie ausführen müssen:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2

    PS C:\> $x509.Import("C:\data\KVWebApp.cer")

    PS C:\> $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    PS C:\> $now = [System.DateTime]::Now

    # this is where the end date from the cert above is used
    PS C:\> $yearfromnow = [System.DateTime]::Parse("2016-07-31")

    PS C:\> $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -KeyValue $credValue -KeyType "AsymmetricX509Cert" -KeyUsage "Verify" -StartDate $now -EndDate $yearfromnow

    PS C:\> $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    PS C:\>$x509.Thumbprint

Nachdem Sie diese Befehle ausgeführt haben, können Sie die Anwendung in Azure AD anzeigen. Wenn Sie anstelle von "verwendet Applikationen wurde von meiner Firma auf" die Anwendung, sich zuerst Suchen nach "Applications wurde von meiner Firma zugegriffen" angezeigt werden.

Wenn Sie weitere Informationen zur Azure AD-Anwendung und ServicePrincipal Objekte finden Sie unter [Anwendung und Dienst Tilgungsanteile Objekte](../active-directory/active-directory-application-objects.md)



**Code Web App verwenden Sie das Zertifikat hinzufügen** Wir werden nun Code der Web-App zum Zugriff auf das Zertifikat, und verwenden sie für die Authentifizierung hinzufügen.

Es ist zuerst Code zum Zugreifen auf des Zertifikats.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Beachten Sie, dass die StoreLocation CurrentUser statt LocalMachine ist. Und, dass wir false, wenn die Find-Methode angeben, da wir ein Zertifikat Test verwendet wird.


Als Nächstes wird Code, der die CertificateHelper verwendet und erstellt eine ClientAssertionCertificate, die für die Authentifizierung erforderlich ist.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Hier ist der neue Code das Access-Token abrufen. Hierdurch wird die oben genannten GetToken-Methode ersetzt. Ich haben sie einen anderen Namen für Komfort gewährt.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Ich haben gesamte Code in meinem Web App-Projekts Utils Klasse für erleichterte Bedienung setzen.

Die letzte Änderung des Codes ist in der Application_Start-Methode. Zuerst müssen wir rufen Sie die GetCert()-Methode, um die ClientAssertionCertificate zu laden. Und ändern wir die Rückrufmethode, die wir beim Erstellen eines neuen KeyVaultClient angeben. Beachten Sie, dass dies der Code, dem wir ersetzt über aufwiesen.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Hinzufügen ein Zertifikats, das Web App über das Azure-Portal** Hinzufügen eines Zertifikats zum Web App ist einfachen Schritten. Zunächst Azure-Portal wechseln Sie, und navigieren Sie zu Ihrem Web App. Klicken Sie in den Einstellungen Blade für Ihre Web-App auf den Eintrag für "benutzerdefinierte Domänen und SSL". Klicken Sie auf das Blade, das Sie geöffnet werden hochladen das Zertifikat, das Sie soeben KVWebApp.pfx erstellt haben, stellen Sie sicher, dass Sie das Kennwort für die Pfx erinnern können.

![Hinzufügen eines Zertifikats zu einer Web App im Azure-Portal][2]


Zuletzt ausgeführt, die Sie tun müssen, besteht darin, eine Anwendungseinstellung Web App hinzufügen, die Name WEBSITE ist\_laden\_Zertifikate und der Wert *. Dadurch wird sichergestellt, dass alle Zertifikate geladen sind. Wenn Sie nur die Zertifikate zu laden, die Sie hochgeladen haben beispielsweise, können Sie eine durch Trennzeichen getrennte Liste von deren Fingerabdrücke eingeben.

Weitere Informationen zum Hinzufügen eines Zertifikats zu einer Web App finden Sie unter [Verwenden von Zertifikaten in Azure Websites Applications](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)


**Hinzufügen eines Zertifikats zum Schlüssel Tresor als Geheimnis** Statt Hochladen des Zertifikats direkt an den Web App-Dienst, können Sie in Schlüssel Tresor als Geheimnis speichern und Bereitstellen von dort aus. Dies umfasst zwei Schritte, die in den folgenden Blogbeitrag, [Bereitstellen von Azure Web App Zertifikat durch die Taste Tresor](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) hervorgehoben wird



## <a name="a-idnextanext-steps"></a><a id="next"></a>Nächste Schritte ##


Verweise für die Programmierung, finden Sie unter [Azure Schlüssel Tresor C#-Client-API-Referenz](https://msdn.microsoft.com/library/azure/dn903628.aspx).


<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
