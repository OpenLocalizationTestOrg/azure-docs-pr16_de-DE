<properties
    pageTitle="Azure App Service Web app erweiterte Config und Erweiterungen"
    description="Verwenden von XML-Dokument Transformation(XDT) Deklarationen zum Transformieren der Datei ApplicationHost.config in Ihrer App-Verwaltungsdienst Azure Web app und private Erweiterungen zum Aktivieren der Verwaltung von benutzerdefinierten Aktionen hinzuzufügen."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure App Service Web app erweiterte Config und Erweiterungen

Mithilfe von Deklarationen [Transformation von XML-Dokumenten](http://msdn.microsoft.com/library/dd465326.aspx) (XDT), können Sie die Datei [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) in die Web-app im App-Verwaltungsdienst Azure transformieren. XDT Deklarationen können Sie auch als "Privat" Erweiterungen um benutzerdefinierte Web app Administration Aktionen aktivieren hinzufügen. Dieser Artikel enthält eine Stichprobe Manager von PHP Web app-Erweiterung, die Verwaltung von PHP Einstellungen über eine Web-Oberfläche ermöglicht.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a name="a-idtransformaadvanced-configuration-through-applicationhostconfig"></a><a id="transform"></a>Erweiterte Konfiguration über ApplicationHost.config
Die App-Service-Plattform bietet Flexibilität und Kontrolle für Web app-Konfiguration an. Obwohl die standard IIS ApplicationHost.config Konfigurationsdatei nicht für die direkte Bearbeitung in der App-Dienst verfügbar ist, unterstützt die Plattform ein deklaratives ApplicationHost.config Transformation-Modell, basierend auf XML-Dokument Transformation (XDT).

Um diese Transformation Funktionen nutzen zu können, erstellen Sie eine Datei ApplicationHost.xdt mit XDT Inhalt und Ort, unter der Stammordner (d:\home\site) in der [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console)aus. Möglicherweise müssen Sie die Web App für die Änderungen wirksam werden neu starten.

Im folgenden applicationHost.xdt Beispiel veranschaulicht, wie eine neue benutzerdefinierte Umgebungsvariable eine Web app hinzufügen, 5.4 von PHP verwendet.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Eine Protokolldatei mit Transformationsstatus und Details ist der Stammwebsitesammlung FTP-unter LogFiles\Transform verfügbar.

Weitere Beispiele finden Sie unter [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Notiz**<br />
Elemente in der Liste der Module unter `system.webServer` nicht entfernt oder neu angeordnet, aber Ergänzungen zur Liste sind möglich.


##<a name="a-idextenda-extend-your-web-app"></a><a id="extend"></a>Erweitern Sie die Web app

###<a name="a-idoverviewa-overview-of-private-web-app-extensions"></a><a id="overview"></a>Übersicht über private Web app-Erweiterungen

App-Dienst unterstützt Web app-Erweiterungen als ein Erweiterungspunkt für administrative Vorgänge an. Tatsächlich werden einige Features der App-Service-Plattform als vorinstalliert Erweiterungen implementiert. Während die integrierten Plattform Erweiterungen nicht geändert werden können, können Sie erstellen und Konfigurieren von privaten Erweiterungen für Ihre eigenen Web app. Diese Funktionalität beruht auch auf XDT Deklarationen. Die wichtigsten Schritte zum Erstellen einer privaten Web app-Erweiterung gehören die folgenden:

1. Web app-Erweiterung **Inhalt**: Erstellen von App-Dienst unterstützt jede Web-Anwendung
2. Web app Erweiterung **Deklaration**: Erstellen einer Datendatei ApplicationHost.xdt
3. Web app Erweiterung **Bereitstellung**: Platzieren von Inhalt in den Ordner SiteExtensions unter`root`

Interne Hyperlinks für das Web app sollte auf einen Pfad relativ zum Pfad Anwendung, die in der Datei ApplicationHost.xdt angegebenen verweisen. Änderungen an der Datei ApplicationHost.xdt erfordert eine Web app-Papierkorb.

**Hinweis**: Weitere Informationen: diese Schlüsselelemente steht unter [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Ein detailliertes Beispiel ist enthalten, um die Schritte zum Erstellen und Aktivieren der privaten Web app-Erweiterung zu veranschaulichen. Das folgende Beispiel Manager von PHP-Quellcode kann von [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager)heruntergeladen werden.

###<a name="a-idsitesamplea-web-app-extension-example-php-manager"></a><a id="SiteSample"></a>Beispiel für Web app-Erweiterung: Manager von PHP

Manager von PHP ist Web app-Erweiterung, die Web app-Administratoren auf einfache Weise anzeigen und Konfigurieren ihrer PHP Einstellungen für eine Web-Schnittstelle anstatt direkt ändern von PHP INI-Dateien ermöglicht. Allgemeine Konfigurationsdateien für PHP enthalten die php.ini-Datei finden Sie unter Programme und der. user.ini Datei im Stammordner Ihrer Web App. Da die php.ini-Datei nicht direkt bearbeitet werden kann, auf der App-Service-Plattform ist, die Erweiterung Manager von PHP verwendet die. user.ini Datei ändert sich die Einstellung zu übernehmen.

####<a name="a-idphpwebappa-the-php-manager-web-application"></a><a id="PHPwebapp"></a>Die Manager von PHP-Webanwendung

Im folgenden finden die Homepage der Bereitstellung von PHP-Manager:

![TransformSitePHPUI][TransformSitePHPUI]

Wie Sie sehen können, ist die Web app-Erweiterung eine normale Webanwendung, vergleichbar mit einer zusätzlichen ApplicationHost.xdt-Datei im Stammordner des Web app (Weitere Details zur Datei ApplicationHost.xdt stehen im nächsten Abschnitt dieses Artikels) platziert.

Die Erweiterung von PHP Manager erstellt wurde, verwenden die Vorlage für Visual Studio 4 ASP.NET-MVC-Webanwendung. Die folgende Ansicht von Lösung Explorer zeigt die Struktur der Manager von PHP Erweiterung.

![TransformSiteSolEx][TransformSiteSolEx]

Die einzige besondere Logik für Datei e/a besteht darin, anzugeben, wo sich der Web-app Verzeichnis Wwwroot befindet. Wie im folgenden Code wird gezeigt, die Umgebung Variable "HOME" zeigt an, der Web-app Stammpfad und der Pfad Wwwroot durch Anfügen von "Site\wwwroot" erstellt werden kann:

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Nachdem Sie den Pfad zum Verzeichnis haben, können Sie reguläre Datei e/a-Vorgängen zum Lesen und Schreiben von Dateien.

Vorsicht mit Web app-Erweiterungen einen Punkt in Bezug auf die Behandlung von internen Links.  Wenn Sie alle Links, die in HTML-Dateien, die absolute Pfaden auf interne Hyperlinks auf Web app gewähren haben, müssen Sie sicherstellen, dass diese Links mit Ihrem Erweiterungsnamen als Ihre Stamm vorangestellt werden. Dies ist erforderlich, da der Stamm für die Erweiterung jetzt ist "/`[your-extension-name]`/" und nicht nur "/", daher müssen alle internen Links müssen entsprechend aktualisiert werden. Nehmen Sie beispielsweise an, dass Ihr Code einen Link zu den folgenden enthält:

`"<a href="/Home/Settings">PHP Settings</a>"`

Wenn der Link Teil einer Web app-Erweiterung ist, muss der Link in folgender Form aus:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Können Sie diese Anforderung umgehen, indem Sie entweder nur relative Pfade innerhalb der Webanwendung oder im Falle von ASP.NET Applications, mithilfe der `@Html.ActionLink` Methode, die den geeigneten Link für Sie erstellt.

####<a name="a-idxdta-the-applicationhostxdt-file"></a><a id="XDT"></a>Die applicationHost.xdt-Datei

Der Code für die Erweiterung Ihrer Web app wechselt unter %HOME%\SiteExtensions\[Ihr Erweiterung Name]. Wir nennen dies die Erweiterung aus.  

Um die Datei applicationHost.config die Web app-Erweiterung registrieren, müssen Sie eine Datei namens ApplicationHost.xdt im Stammverzeichnis Erweiterung zu platzieren. Der Inhalt der Datei ApplicationHost.xdt sollte wie folgt aussehen:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Der Name, den Sie als Ihren Erweiterungsnamen auswählen sollten denselben Namen als Stammordner Erweiterung haben.

Dies wirkt sich zum Hinzufügen eines neuen Anwendung Pfads für die `system.applicationHost` Websiteliste unter der SCM-Website. Die SCM-Website ist eine Website Administration-Endpunkt mit bestimmten Access-Anmeldeinformationen. Es hat die URL `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a name="a-iddeploya-web-app-extension-deployment"></a><a id="deploy"></a>Erweitern der Bereitstellung Web app

Um die Erweiterung Ihrer Web-app installiert haben, können FTP So kopieren Sie alle Dateien der Webanwendung, um die `\SiteExtensions\[your-extension-name]` Ordner des Web app auf dem Sie die Erweiterung installieren möchten.  Achten Sie darauf, um die Datei ApplicationHost.xdt an dieser Stelle auch zu kopieren. Starten der Web-app, um die Erweiterung zu aktivieren.

Sie sollten die Erweiterung Ihrer Web app bei verdeckt werden sollen:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Beachten Sie, dass die URL einfach die URL für Ihre Web-app aussieht, außer dass er HTTPS verwendet und ".scm" enthält.

Es ist möglich, deaktivieren alle als "Privat" (nicht vorinstallierte) Erweiterungen für Ihre Web app bei der Entwicklung und Untersuchungen durch Hinzufügen einer app-Einstellungen mit dem Schlüssel `WEBSITE_PRIVATE_EXTENSIONS` und ein Wert von `0`.

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
