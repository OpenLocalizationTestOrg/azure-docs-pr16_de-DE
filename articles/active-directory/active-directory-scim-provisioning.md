<properties
    pageTitle="Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory Clientanwendungen mithilfe von SCIM | Microsoft Azure"
    description="Azure-Active Directory können automatisch Bereitstellung von Benutzern und Gruppen, um eine Anwendung oder Identität Store, die von einem Webdienst mit der Benutzeroberfläche in der SCIM Protokollspezifikation definierten Produktkatalogsystem ist"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory Applications mithilfe von SCIM

##<a name="overview"></a>(Übersicht)

Azure-Active Directory können automatisch Bereitstellung von Benutzern und Gruppen, um eine Anwendung oder Identität Store, die von einem Webdienst mit der Benutzeroberfläche in der [SCIM 2.0-Protokoll-Spezifikation](https://tools.ietf.org/html/draft-ietf-scim-api-19)definierten Produktkatalogsystem ist. Azure-Active Directory können Anfragen zu erstellen, ändern und Löschen von zugeordneten Benutzer und Gruppen, um diesen Webdienst, die dann diese Anfragen in Vorgängen für das Ziel Identität Store übersetzen können senden. 

![][1]
*Abbildung: Bereitstellung von Azure Active Directory eine Identität Store über einen Webdienst*

Diese Funktion kann verwendet werden in Verbindung mit der Funktion "[bringen Sie Ihre eigenen app](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" in Azure AD aktivieren einmaliges Anmelden und automatische Benutzer für Applikationen, die bereitstellen oder von einem Webdienst SCIM Produktkatalogsystem werden bereitgestellt.

Es gibt zwei Fällen wird SCIM in Azure Active Directory aus:

* **Benutzer und Gruppen auf Anwendungen, die SCIM unterstützen, provisioning** - Programmen, die SCIM 2.0 unterstützen, und verwenden für die Authentifizierung OAuth Person Token funktionieren mit Azure AD neben dem Feld.

* **Erstellen Ihrer eigenen provisioning Lösung für Applikationen, die andere API-basiertes provisioning unterstützen** - For Applications nicht SCIM, Sie können erstellen Sie einen SCIM Endpunkt zum Übersetzen zwischen Azure AD-SCIM Endpunkt und welchen API der Anwendung unterstützt für Benutzer bereitgestellt.  Bei der Entwicklung von einem SCIM Endpunkt zur Makrofunktion, stellen wir CLI-Bibliotheken zusammen mit Codebeispielen, die Ihnen zeigen, wie einen SCIM Endpunkt angeben und übersetzen SCIM Nachrichten.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Bereitstellung von Benutzern und Gruppen auf Anwendungen, die SCIM unterstützen

Azure-Active Directory können konfiguriert werden, automatisch zugewiesen Bereitstellen von Benutzern und Gruppen auf Anwendungen, die eine [System zum Domain-übergreifende Identitätsmanagement 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) Web implementieren service und OAuth Person Token für die Authentifizierung annehmen. Innerhalb der SCIM 2.0-Spezifikation müssen Applikationen diese Anforderungen entsprechen:

* Unterstützt Benutzer und/oder Gruppen gemäß Abschnitt 3.3, der das Protokoll SCIM erstellen.  

* Unterstützt Benutzer und/oder Gruppen mit Patch Anforderungen gemäß Abschnitt 3.5.2, der das Protokoll SCIM ändern.  

* Abrufen einer bekannten Ressource gemäß Abschnitt 3.4.1 der SCIM-Protokoll unterstützt.  

*  Unterstützt Benutzer und/oder Gruppen gemäß Abschnitt 3.4.2, der das Protokoll SCIM Abfragen.  Standardmäßig Benutzer abgefragt werden, indem Sie ExternalId und Gruppen von DisplayName abgefragt werden.  

* Unterstützt die Benutzer-ID und durch Manager gemäß Abschnitt 3.4.2, der das Protokoll SCIM Abfragen.  

* Unterstützt die Gruppen, indem Sie-ID und Mitglied gemäß Abschnitt 3.4.2, der das Protokoll SCIM Abfragen.  

* Akzeptiert OAuth Person Token für die Autorisierung gemäß Abschnitt 2.1, der das Protokoll SCIM an.

Mit dem Anbieter der Anwendung oder der Dokumentation Ihres Anwendungsanbieters für Kompatibilität mit den folgenden Anforderungen Anweisungen sollten.
 
###<a name="getting-started"></a>Erste Schritte

Programme, die das SCIM Profil oben beschriebenen unterstützen können mit Azure Active Directory mithilfe des Features "Benutzerdefiniert" app in der Galerie Azure AD-verbunden sein. Nachdem die Verbindung hergestellt wurde, wird Azure AD eine Synchronisierung alle 5 Minuten, in dem sie Abfragen der Anwendung SCIM Endpunkt für zugeordneten Benutzer und Gruppen, und erstellt oder ändert diese entsprechend der Aufgabendetails, ausgeführt.

**Um eine Anwendung zu verbinden unterstützt, SCIM:**

1.  Starten Sie den Azure-Verwaltungsportal am https://manage.windowsazure.com in einem Webbrowser.
2.  Navigieren Sie zu **Active Directory > Verzeichnis > [Ihr Verzeichnis] > Applications**, und wählen Sie **Hinzufügen > Hinzufügen einer App aus dem Katalog**.
3.  Wählen Sie die Registerkarte **Benutzerdefiniert** auf der linken Seite aus, geben Sie einen Namen für die Anwendung, und klicken Sie auf das Häkchensymbol, um ein Objekt app zu erstellen.

![][2]

4.  Wählen Sie in die resultierende Bildschirm die zweite Schaltfläche **Konfigurieren Konto bereitgestellt** .
5.  Geben Sie im Feld **Provisioning Endpunkt-URL** die URL der Anwendung SCIM Endpunkt aus.
6.  Wenn für der Endpunkt SCIM ein OAuth Person Token aus ein Herausgeber als Azure AD erforderlich ist, kopieren Sie das erforderliche OAuth Person Token in das Feld **Authentication Token (optional)** ein. Ist dieses Feld leer ist, und klicken Sie dann Azure AD wird ein OAuth Person Token ausgestellt von Azure AD mit jeder Anforderung enthalten. Apps, die Azure AD verwenden, wie ein Idenity Anbieter dieses Azure AD überprüfen kann-Token ausgestellt.
7.  Klicken Sie auf **Weiter**, und klicken Sie auf die Schaltfläche **Test starten** Azure Active Directory versuchen, eine Verbindung mit den Endpunkt SCIM haben. Wenn die Versuche fehlschlagen, wird Diagnoseinformationen angezeigt.  
8.  Wenn versucht, die Verbindung zu der Anwendung erfolgreich, klicken Sie dann auf **Weiter** der verbleibenden Bildschirme, und klicken Sie auf **abgeschlossen** , um das Dialogfeld zu beenden.
9.  Wählen Sie im Bildschirm resultierende dritte Schaltfläche **Konten zuweisen** . Weisen Sie die resultierende Abschnitt Benutzer und Gruppen die Benutzer oder Gruppen, die gewünschte bereitstellen, mit der Anwendung.
10. Nachdem die Benutzer und Gruppen zugewiesen sind, klicken Sie auf die Registerkarte **Konfigurieren** im oberen Bereich des Bildschirms.
11. Klicken Sie unter **Konto bereitgestellt**zu bestätigen, dass der Status festgelegt wurde. 
12. Klicken Sie unter **Tools**klicken Sie zum Starten der Bereitstellung Prozess auf **neu starten Konto bereitgestellt** .

Beachten Sie, dass 5 bis 10 Minuten vor der Bereitstellung beginnen wird für Anfragen an den Endpunkt SCIM verstreichen kann.  Eine Zusammenfassung der versucht, eine Verbindung für Dashboard-Registerkarte der Anwendung bereitgestellt wird, und sowohl einen Bericht zur Bereitstellung Aktivität und Fehlern provisioning des Verzeichnisses Berichte Registerkarte heruntergeladen werden können.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Erstellen Ihrer eigenen Lösung für jede Anwendung bereitgestellt

Durch eine SCIM-Webdienst, der Schnittstellen mit Azure Active Directory erstellt haben, können Sie die einzelnen Benutzer für einmaliges Anmelden und die automatische für nahezu jeder Anwendung, die einen REST oder SOAP-Benutzer bereitgestellt API bietet bereitgestellt aktivieren.

Und so funktioniert es:

1.  Azure AD bietet eine allgemeine Sprache Infrastruktur-Bibliothek mit dem Namen [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systemintegratoren und Entwickler können mithilfe dieser Bibliothek erstellen und Bereitstellen eines SCIM-basierten Webdienst-Endpunkts in Azure AD mit einem beliebigen Anwendungsspeicher Identität zu verbinden.
2.  Zuordnungen werden im Webdienst ordnen das Benutzer standardisierten Schema zu der Benutzerschema und Protokoll, die von der Anwendung benötigte implementiert.
3.  Die Endpunkt-URL ist in Azure AD als Teil einer benutzerdefinierten Anwendung in der Galerie registriert.
4.  Benutzer und Gruppen werden diese Anwendung in Azure AD zugewiesen. Bei der Zuordnung werden diese in eine Warteschlange mit der Zielanwendung synchronisiert werden aufgenommen. Die Synchronisierung die Warteschlange Behandlung wird 5 Minuten ausgeführt.

###<a name="code-samples"></a>Codebeispielen

Um dieses Verfahren zu erleichtern, sind eine Reihe von [Beispielen Fehlercode](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) vorausgesetzt, erstellen einen Webdienst-Endpunkt SCIM, und führen Sie die automatische Bereitstellung vor. Eine Stichprobe darstellen eines Anbieters, die eine Datei mit Zeilen mit durch Kommas getrennten Werten, die Benutzer und Gruppen darstellt unterhält.  Das andere ist für einen Anbieter, der für den Dienst Amazon Web Services Identität und Access-Verwaltung ausgeführt wird.  

**Erforderliche Komponenten**

* Visual Studio 2013 oder höher
* [Azure SDK für .NET](https://azure.microsoft.com/downloads/)
* Windows-Computer, die das ASP.NET-Framework 4.5 unterstützt als den Endpunkt SCIM verwendet werden soll. Dieser Computer muss aus der Cloud zugegriffen werden.
* [Ein Azure mit einer Testversion oder lizenzierte Version von Azure AD Premium-Abonnement](https://azure.microsoft.com/services/active-directory/)
* Im Beispiel Amazon AWS erfordert Bibliotheken aus dem [AWS-Toolkit für Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Lesen Sie die im Beispiel für weitere Details enthaltene Infodatei

###<a name="getting-started"></a>Erste Schritte

Die einfachste Möglichkeit, einen Endpunkt SCIM implementieren, der Bereitstellung Azure AD-Anfragen akzeptieren ist das Erstellen und Bereitstellen der Code Stichprobe, die der bereitgestellten Benutzer in eine Datei durch Trennzeichen getrennte Werte (CSV) gibt.

**So erstellen Sie einen Stichprobe SCIM Endpunkt**

1.  Im Beispielpaket [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) Code herunterladen
2.  Das Paket Entzippen Sie ihn, und legen Sie es auf Ihrem Windows-Computer an einem Speicherort, z. B. C:\AzureAD-BYOA-Provisioning-Samples\.
3.  Starten Sie in diesem Ordner die FileProvisioningAgent-Lösung in Visual Studio.
4.  Wählen Sie **Tools > Bibliothek Paket-Manager > Paket-Manager-Konsole**, und führen Sie die Befehle unter für das Projekt FileProvisioningAgent, um die Lösung Bezüge zu beheben:

    Installationspaket Installationspaket Microsoft.SystemForCrossDomainIdentityManagement Microsoft.IdentityModel.Clients.ActiveDirectory Installationspaket Microsoft.Owin.Diagnostics Installationspaket Microsoft.Owin.Host.SystemWeb

5.  Erstellen Sie das Projekt FileProvisioningAgent.
6.  Starten Sie die Anwendung Eingabeaufforderungsfenster in Windows (als Administrator), und verwenden Sie den Befehl **cd** Verzeichnis in den Ordner **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** ändern.
7.  Führen Sie den Befehl unter < Ip-Adresse > namens IP oder Domänennamen des Computers Windows ersetzt.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  Unter Windows **Windows-Einstellungen > Netzwerk und Internet Einstellungen**auf der **Windows-Firewall > Erweiterte Einstellungen**, und erstellen Sie eine **Eingehende Regel** , die eingehenden Zugriff auf Anschluss 9000 ermöglicht.
9.  Ist der Windows-Computer hinter einem Router, wird der Router konfiguriert sein, um Netzwerk Access Übersetzung zwischen das Anschluss 9000, die mit dem Internet verbunden ist, und den Port 9000 auf dem Windows-Computer ausführen müssen. Dies ist erforderlich, damit Azure AD diesen Endpunkt in der Cloud zugreifen können.


**In der Stichprobe SCIM Endpunkt in Azure AD zu registrieren:**

1.  Starten Sie den Azure-Verwaltungsportal am https://manage.windowsazure.com in einem Webbrowser.
2.  Navigieren Sie zu **Active Directory > Verzeichnis > [Ihr Verzeichnis] > Applications**, und wählen Sie **Hinzufügen > Hinzufügen einer App aus dem Katalog**.
3.  Wählen Sie die Registerkarte **Benutzerdefiniert** auf der linken Seite aus, geben Sie einen Namen ein, beispielsweise "SCIM Test App", und klicken Sie auf das Häkchensymbol, um ein Objekt app zu erstellen. Beachten Sie, dass das Anwendungsobjekt erstellt die Ziel-app Provisioning bis hin und implementieren einmaliges Anmelden für und nicht nur den Endpunkt des SCIM werden würde darstellen möchten.

![][2]

4.  Wählen Sie in die resultierende Bildschirm die zweite Schaltfläche **Konfigurieren Konto bereitgestellt** .
5.  Geben Sie im Dialogfeld Internet bereitgestellt URL und Port Ihres SCIM Endpunkts an. Dies wäre etwas wie IP Http://testmachine.contoso.com:9000 oder http://<ip-address>:9000/, wobei < Ip-Adresse > im Internet ist verfügbar gemacht Adresse.  
6.  Klicken Sie auf **Weiter**, und klicken Sie auf die Schaltfläche **Test starten** Azure Active Directory versuchen, eine Verbindung mit den Endpunkt SCIM haben. Wenn die Versuche fehlschlagen, wird Diagnoseinformationen angezeigt.  
7.  Wenn versucht, die Verbindung zu Ihren Webdienst erfolgreich sind, klicken Sie dann klicken Sie auf die übrigen Bildschirme auf **Weiter** , und klicken Sie auf **abgeschlossen** , um das Dialogfeld zu beenden.
8.  Wählen Sie im Bildschirm resultierende dritte Schaltfläche **Konten zuweisen** . Weisen Sie die resultierende Abschnitt Benutzer und Gruppen die Benutzer oder Gruppen, die gewünschte bereitstellen, mit der Anwendung.
9.  Nachdem die Benutzer und Gruppen zugewiesen sind, klicken Sie auf die Registerkarte **Konfigurieren** im oberen Bereich des Bildschirms.
10. Klicken Sie unter **Konto bereitgestellt**zu bestätigen, dass der Status festgelegt wurde. 
11. Klicken Sie unter **Tools**klicken Sie zum Starten der Bereitstellung Prozess auf **neu starten Konto bereitgestellt** .

Beachten Sie, dass 5 bis 10 Minuten vor der Bereitstellung beginnen wird für Anfragen an den Endpunkt SCIM verstreichen kann.  Eine Zusammenfassung der versucht, eine Verbindung für Dashboard-Registerkarte der Anwendung bereitgestellt wird, und sowohl einen Bericht zur Bereitstellung Aktivität und Fehlern provisioning des Verzeichnisses Berichte Registerkarte heruntergeladen werden können.

Der letzte Schritt in der Stichprobe Überprüfung ist die Datei TargetFile.csv im Ordner \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug auf Ihrem Windows-Computer zu öffnen. Sobald der Bereitstellung Prozess ausgeführt wird, zeigt diese Datei die Details der alle zugewiesen und nach der Bereitstellung Benutzer und Gruppen.

###<a name="development-libraries"></a>Von Entwicklungsbibliotheken

Bei der Entwicklung, der der Spezifikation SCIM entspricht Ihren eigenen Webdienst zuerst Vertrautmachen Sie mit den folgenden Bibliotheken von Microsoft unterstützen die Entwicklung zu beschleunigen bereitgestellt: 

**1:**  Allgemeine Language Infrastructure Bibliotheken sind für die Verwendung mit basierend auf diesen Infrastruktur, wie z. B. C#-Sprachen erhältlich.  Eine dieser Bibliotheken, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklariert Schnittstelle Microsoft.SystemForCrossDomainIdentityManagement.IProvider, in der folgenden Abbildung gezeigt.  Ein Entwickler mithilfe der Bibliotheken würde die Schnittstelle mit einer Klasse implementieren, die, Allgemein, als Anbieter bezeichnet werden kann.  Die Bibliotheken Aktivieren des Entwicklertools einfach Bereitstellen von einem Webdienst, der entspricht der Spezifikation SCIM, entweder in Internet Information Services oder alle ausführbare Common Language Infrastructure Assemblys gehostet.  Anfragen an diesen Webdienst werden in Aufrufen des Anbieters Methoden, umgewandelt werden, die vom Entwickler programmiert werden möchten, um einige Store Identität ausgeführt werden.    

![][3]

**2:** [Express-Routing Ereignishandler](http://expressjs.com/guide/routing.html) stehen für node.js Anforderungsobjekte zu analysieren einer node.js Webdienst vorgenommen, Anrufe (gemäß der Spezifikation SCIM), darstellt.     

###<a name="building-a-custom-scim-endpoint"></a>Erstellen eines benutzerdefinierten SCIM Endpunkts

Verwenden die oben beschriebenen Bibliotheken, können Entwickler, die mit diesen Bibliotheken ihre Dienste in alle ausführbare Common Language Infrastructure Assemblys, oder klicken Sie in Internet Information Services hosten.  So sieht Beispielcode für eine Hostingdienst innerhalb einer ausführbare Assembly, unter der Adresse http://localhost: 9000 aus: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Es ist wichtig, beachten Sie, dass dieser Dienst ein Zertifikat HTTP-Adresse und Server-Authentifizierung benötigen, von denen die Stammzertifizierungsstelle eine der folgenden ist: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Ein-Authentifizierung Serverzertifikat an einen Anschluss gebunden werden kann, auf einem Windows-Host mithilfe der Shell-Netzwerkkonfiguration, wie hier: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Hier ist der Wert für das Argument Certhash angegebene der Fingerabdruck des Zertifikats, während der Wert für das Argument Appid bereitgestellt zufälliger global eindeutigen Bezeichner.  

Wenn Sie um den Dienst in Internet Information Services zu hosten, würde ein Entwickler eine Common Language Infrastructure Library Codeassembly mit einer Klasse namens Start in der Standard-Namespace der Assembly erstellen.  Hier ist ein Beispiel einer solchen Klasse ein: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Behandeln von Endpunktauthentifizierung

Azure Active Directory-Anfragen gehören ein OAuth 2.0 Person Token.   Jeder Dienst empfangen die Anforderung sollte der Herausgeber als Azure Active Directory für den erwarteten Azure Active Directory-Mandanten, für den Zugriff auf Graph-Webdienst Azure Active Directory authentifizieren.  Im Token, wird der Herausgeber durch eine Iss anfordern, wie etwa "Iss" identifiziert: "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  In diesem Beispiel die Basisadresse des Werts anfordern, https://sts.windows.net, identifiziert Azure Active Directory als den Herausgeber, während das relative Adresssegment cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, ist ein eindeutiger Bezeichner für den Azure Active Directory-Mandanten, für die das Token ausgestellt wurde.  Wenn das Token ausgestellt wurde, für den Zugriff auf die Azure Active Directory Graph-Webdienst, sollte der Bezeichner dieses Diensts, 00000002-0000-0000-c000-000000000000, über das Token des also Inanspruchnahme Wert sein.  

Entwickler, die die Common Language Infrastructure Bibliotheken von Microsoft bereitgestellt werden, zum Erstellen eines SCIM-Diensts verwenden, können Anfragen Azure Active Directory mithilfe des Microsoft.Owin.Security.ActiveDirectory-Pakets mit folgenden Schritten authentifizieren: 

**1:**  Implementieren Sie die Eigenschaft Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior in einem Anbieter indem Sie ihm zurückkehren eine Methode aufgerufen werden, wenn der Dienst gestartet wird: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Fügen Sie den folgenden Code für diese Methode, um eine Anforderung an allen Endpunkte des Diensts, die als ein Token ausgestellt von Azure Active Directory für einen angegebenen Mandanten, für den Zugriff auf Azure Active Directory Graph-Webdiensts tragen authentifiziert haben: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Benutzer- und Gruppe Schemas

Azure-Active Directory können zwei Arten von Ressourcen SCIM Webdienste bereitstellen.  Diese Ressourcentypen können Benutzer und Gruppen angezeigt werden.  

Benutzerressourcen mit der Schema-ID, Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User, die in dieser Protokollspezifikation gehört identifiziert werden: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Die Standard-Zuordnung der Attribute von Benutzern in Azure Active Directory, um die Attribute der Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User Ressourcen werden in der Tabelle 1, unter bereitgestellt.  

Gruppieren von Ressourcen werden durch die Schema-ID identifiziert http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabelle 2, unten, zeigt die Zuordnung der Attribute von Gruppen in Azure Active Directory an die Attribute der http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group Ressourcen.  

###<a name="table-1-default-user-attribute-mapping"></a>Tabelle 1: Standardmäßige Benutzer-Attribut-Zuordnung

| Azure Active Directory-Benutzer | Urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktive |
| displayName | displayName |
| Faksimile-TelephoneNumber | Wert PhoneNumbers [Typ Eq "Fax"] |
| Vorname | name.givenName |
| Position | Titel |
| e-Mail-Nachrichten | Wert der e-Mails [Typ Eq "Arbeit"] |
| mailNickname | externalId |
| Manager | Manager |
| Mobile | Wert PhoneNumbers [Typ Eq "Mobil"] |
| objectId | ID |
| Postleitzahl | .postalCode Adressen [Typ Eq "Arbeit"] |
| Proxy-Adressen | e-Mails [Geben Sie Eq "andere"]. Wert |
| physische-Übermittlung-OfficeName | Adressen [Geben Sie Eq "andere"]. Formatiert |
| streetAddress | .streetAddress Adressen [Typ Eq "Arbeit"] |
| Nachname | name.familyName |
| Telefonnummer | Wert PhoneNumbers [Typ Eq "Arbeit"] |
| Benutzer-' PrincipalName ' | Benutzername |


###<a name="table-2-default-group-attribute-mapping"></a>Tabelle 2: Standard Gruppe Attribut Zuordnung

| Azure Active Directory-Gruppe | http://Schemas.Microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| displayName | externalId |
| e-Mail-Nachrichten | Wert der e-Mails [Typ Eq "Arbeit"] |
| mailNickname | displayName |
| Mitglieder | Mitglieder |
| objectId | ID |
| proxyAddresses | e-Mails [Geben Sie Eq "andere"]. Wert |


##<a name="user-provisioning-and-de-provisioning"></a>Benutzer erteilen und entziehen

Die folgende Abbildung zeigt, dass der Azure Active Directory zum Verwalten des Lebenszyklus von einem Benutzer in einer anderen Identität zu einem SCIM Dienst sendet Nachrichten gespeichert werden.  Das Diagramm zeigt auch an, wie ein SCIM Dienst mithilfe der Common Language Infrastructure Bibliotheken zum Erstellen solcher Dienste von Microsoft bereitgestellten implementiert diese Anfragen in Aufrufe der Methoden eines Anbieters übersetzen.  

![][4]
*Abbildung: Benutzer bereitgestellt und Aufheben der Bereitstellung Sequenz*

**1:**  Azure-Active Directory wird den Dienst für einen Benutzer mit einem ExternalId Attributwert, der dem MailNickname Attribut Wert eines Benutzers Nachschlagen in Azure Active Directory Abfragen.  Die Abfrage wird als eine Anforderung Hypertext Transfer Protocol wie diesen Termin ausgedrückt werden, in dem Jyoung eine Stichprobe aus einer MailNickname eines Benutzers Nachschlagen in Azure Active Directory sind: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Wenn der Dienst mithilfe der Common Language Infrastructure Bibliotheken zum Implementieren SCIM Dienste von Microsoft bereitgestellten erstellt wurde, wird die Anfrage in einen Anruf an die Query-Methode des Diensts Anbieter übersetzt werden.  Hier ist die Signatur dieser Methode: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Hier ist die Definition der Schnittstelle Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters ein: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

Bei dem vorstehenden Beispiel einer Abfrage für einen Benutzer mit einem bestimmten Wert für das Attribut ExternalId werden Werte in der Abfrage-Methode übergebenen Argumente wie folgt: 

* Parameter. AlternateFilters.Count: 1
* Parameter. AlternateFilters.ElementAt(0). AttributePath: "ExternalId"
* Parameter. AlternateFilters.ElementAt(0). Vergleichsoperator: ComparisonOperator.Equals
* Parameter. AlternateFilter.ElementAt(0). ComparisonValue: "Jyoung"
* CorrelationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. RequestId"] 

**2:**  Wenn die Antwort auf eine Abfrage an den Dienst für einen Benutzer mit einem ExternalId Attributwert, der dem MailNickname Attribut Wert eines Benutzers Nachschlagen in Azure Active Directory keine Benutzer zurückgibt, wird Azure Active Directory anfordern, dass der Dienst einen Benutzer entsprechend dem bereits im Azure Active Directory bereitstellen.  Hier ist ein Beispiel für einen solchen Antrag ein: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Die Common Language Infrastructure Bibliotheken zum Implementieren SCIM Dienste von Microsoft bereitgestellt würde die Anfrage in einen Anruf an die Create-Methode der Anbieter des Diensts übersetzen.  Die Methode weist diese Signatur: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Im Falle einer Anforderung zur Bereitstellung von eines Benutzers wird der Wert des Arguments Ressource eine Instanz der Microsoft.SystemForCrossDomainIdentityManagement sein. Core2EnterpriseUser-Klasse, in der Bibliothek Microsoft.SystemForCrossDomainIdentityManagement.Schemas definiert.  Bereitstellen den Benutzer die Anfrage erfolgreich ist, und die Implementierung der Methode wird eine Instanz der zurückzugebenden erwartet die der Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser-Klasse, mit dem Wert der Eigenschaft Bezeichner legen die eindeutigen Bezeichner des Benutzers neu bereitgestellt.  

**3:**  Klicken Sie zum Aktualisieren eines Benutzers bekannt, dass eine Identität Store nach einer SCIM Produktkatalogsystem wird Azure Active Directory fortgesetzt, per im aktuellen Zustand des Benutzers aus der Dienst mit einer Anforderung wie diese: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

In einem Dienst, die für die Durchführung von SCIM Dienste von Microsoft bereitgestellten Common Language Infrastructure-Bibliotheken mit erstellt wird die Anfrage in einen Anruf an die Methode Abrufen des Anbieters des Diensts umgewandelt werden.  So sieht die Signatur der Methode abrufen aus: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

Bei der vorstehenden Beispiel für eine Anforderung zum Abrufen des aktuellen Status eines Benutzers werden die Werte der Eigenschaften für das Objekt bereitgestellt wird als Wert des Arguments Parameter wie folgt: 

* Bezeichner: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Wenn ein Bezug Attribut ist aktualisiert werden, und klicken Sie dann Azure Active Directory den ab, um festzustellen, ob der aktuelle Wert für das Attribut Bezug im Identitätsspeicher bereits vom Dienst Produktkatalogsystem oder nicht Abfragen entspricht den Wert für das Attribut unter Azure Active Directory.  Wenn der Benutzer ist das einzige Attribut, von dem der aktuelle Wert auf diese Weise abgefragt werden, das Attribut Manager.  Hier ist ein Beispiel für eine Besprechungsanfrage zu bestimmen, ob das Attribut Manager eines bestimmten Benutzers Objekts aktuell einen bestimmten Wert verfügt: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Der Wert des Parameters Attribute Abfrage-Id gibt an, dass, wenn ein Benutzerobjekt vorhanden ist, das den Ausdruck als Wert des der Filters Abfrageparameter bereitgestellten erfüllt, und klicken Sie dann der Dienst wird mit einer Ressource Urn: Ietf:params:scim:schemas:core:2.0:User oder Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User, einschließlich nur den Wert für die Ressource-Id-Attribut reagiert erwartet.  Der Wert, der das ID-Attribut ist natürlich bekannt, dass das jeweilige – es befindet sich auf den Wert von der Filters Abfrageparameter; der Zweck des dafür Gruppenmitglieder ist tatsächlich eine minimale Darstellung einer Ressource anfordern, die Eigenschaften anzugeben, und zwar unabhängig davon, ob alle solches Objekt vorhanden ist, dass den Filterausdruck entsprechen.   

Wenn der Dienst mithilfe der Common Language Infrastructure Bibliotheken zum Implementieren SCIM Dienste von Microsoft bereitgestellten erstellt wurde, wird die Anfrage in einen Anruf an die Query-Methode des Diensts Anbieter übersetzt werden.  Der Wert der Eigenschaften des Objekts als Wert des Arguments Parameter bereitgestellt werden wie folgt: 

* Parameter. AlternateFilters.Count: 2
* Parameter. AlternateFilters.ElementAt(x). AttributePath: "Id"
* Parameter. AlternateFilters.ElementAt(x). Vergleichsoperator: ComparisonOperator.Equals
* Parameter. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* Parameter. AlternateFilters.ElementAt(y). AttributePath: "Manager"
* Parameter. AlternateFilters.ElementAt(y). Vergleichsoperator: ComparisonOperator.Equals
* Parameter. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* Parameter. RequestedAttributePaths.ElementAt(0): "Id"
* Parameter. SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"

Hier der Wert des Indexes X möglicherweise 0 und der Wert des Indexes y möglicherweise 1, oder der Wert von x möglicherweise 1 und der Wert von y möglicherweise 0, je nach der Reihenfolge der Ausdrücke des der Filters Abfrageparameter.   

**5:**  Hier ist ein Beispiel für eine Anforderung von Azure Active Directory eine SCIM Dienst aktualisieren ein Benutzers: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Die Microsoft Common Language Infrastructure Bibliotheken für die Implementierung SCIM Services würde die Anfrage in einen Anruf an die Update-Methode des Diensts Anbieter übersetzen.  Hier ist die Signatur dieser Methode: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



Wenn im vorstehenden Beispiel einer Anforderung ein Benutzers zu aktualisieren wird das Objekt bereitgestellt wird als Wert des Arguments Patch diese Eigenschaftswerte haben: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest als PatchRequest2). Operations.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "Manager"
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referenz: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Wert: 2819c223-7f76-453a-919d-413861904646

**6:**  Um einen Benutzer aus einer Identität Store Produktkatalogsystem von einem Dienst SCIM heben bereitzustellen, sendet Azure Active Directory eine Anforderung wie diese: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Wenn der Dienst mithilfe der Common Language Infrastructure Bibliotheken zum Implementieren SCIM Dienste von Microsoft bereitgestellten erstellt wurde, wird die Anfrage in einen Anruf an die Delete-Methode des Diensts Anbieter übersetzt werden.   Diese Methode weist diese Signatur: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Das Objekt, die als Wert des Arguments ResourceIdentifier bereitgestellt hat diese Eigenschaftswerte im Falle im vorstehenden Beispiel zur Aufforderung zur Bereitstellung von eines Benutzers aufheben: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "Urn: Ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Gruppe erteilen und entziehen

Die folgende Abbildung zeigt, dass der Azure Active Directory zum Verwalten des Lebenszyklus von einer Gruppe in einer anderen Identität zu einem SCIM Dienst sendet Nachrichten gespeichert werden.  Diese Nachrichten unterscheiden sich von der Nachrichten, die Benutzer auf drei Arten betreffen: 

* Das Schema einer Gruppe Ressource wird als http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group identifiziert werden.  
* Anfragen zum Abrufen von Gruppen werden vorgesehen werden, dass das Attribut für die Mitglieder alle Ressourcen bereitgestellt, die in der Antwort auf die Anforderung ausgeschlossen werden müssen.  
* Besprechungsanfragen, um festzustellen, ob ein Bezug Attribut einen bestimmten Wert weist werden Anfragen über das Attribut Mitglieder.  

![][5]
*Abbildung: Gruppieren Sie erteilen und entziehen Sequenz*

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Automatisieren von Benutzer Bereitstellung/aufheben zu SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Anpassen von Attribut Zuordnungen für Benutzer bereitgestellt](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Zuordnungen Attribut](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsdefinition Filter für Benutzer bereitgestellt](active-directory-saas-scoping-filters.md)
- [Bereitstellung von Benachrichtigungen zu berücksichtigen](active-directory-saas-account-provisioning-notifications.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
