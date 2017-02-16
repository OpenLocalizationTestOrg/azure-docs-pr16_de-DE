<properties
    pageTitle="Zum Arbeiten mit dem Node.js Back-End-Server SDK für Mobile-Apps | Azure App-Verwaltungsdienst"
    description="Informationen Sie zum Arbeiten mit dem Node.js Back-End-Server SDK für Azure App Dienst Mobile-Apps."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>So verwenden Sie die Azure Mobile Apps Node.js SDK

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dieser Artikel enthält ausführliche Informationen und Beispiele zum Arbeiten mit einer Node.js Back-End-in Azure App Dienst Mobile-Apps mit.

## <a name="a-nameintroductionaintroduction"></a><a name="Introduction"></a>Einführung

Azure App Dienst Mobile-Apps ermöglicht das Hinzufügen einer Mobile optimiert Datenzugriffsseite Web-API zu einer Webanwendung.  Die Azure App Service Mobile Apps SDK wird für ASP.NET und Node.js Webanwendungen bereitgestellt.  Das SDK enthält die folgenden Vorgänge:

- Tabellenvorgänge (Lesen, einfügen, aktualisieren, löschen) für den Zugriff auf Daten
- Benutzerdefinierte API Vorgänge

Beide Vorgänge bereitstellen für die Authentifizierung über alle Identitätsanbieter zulässig von Azure-App-Verwaltungsdienst, einschließlich der sozialen Identitätsanbieter wie etwa Facebook, Twitter, Google und Microsoft sowie Azure Active Directory für Enterprise-Identität.

Sie können Beispiele für jede Anwendungsfall-im [Beispielverzeichnis auf GitHub]suchen.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Die Azure Mobile Apps Knoten SDK unterstützt die aktuelle LTS Version des Knotens und höher.  Zum Zeitpunkt der Erstellung ist die neueste Version von LTS Knoten v4.5.0.  Andere Versionen von Knoten funktionieren, aber Sie werden nicht unterstützt.

Die Azure Mobile Apps Knoten SDK unterstützt zwei Datenbanktreiber – der Knoten Mssql Treiber unterstützt SQL Azure und lokalen SQL Server-Instanzen.  Der sqlite3-Treiber unterstützt SQLite Datenbanken auf nur einer einzelnen Instanz.

### <a name="a-namehowto-cmdline-basicappahow-to-create-a-basic-nodejs-backend-using-the-command-line"></a><a name="howto-cmdline-basicapp"></a>So: Erstellen einer einfachen Node.js Back-End-, über die Befehlszeile

Jede Azure App Dienst Mobile-App Node.js Back-End-wird als ExpressJS Anwendung gestartet.  ExpressJS ist der am häufigsten verwendeten Webdienst-Framework für Node.js verfügbar.  Sie können eine grundlegende erstellen [Express] -Anwendung wie folgt:

1. Erstellen Sie in einem Befehl oder PowerShell-Fenster ein Verzeichnis für ein Projekt aus.

        mkdir basicapp

2. Führen Sie die Initialisierung Npm, um die Paketstruktur Initialisierung.

        cd basicapp
        npm init

    Der Befehl Npm Initialisierung gefragt werden, eine Reihe von Fragen, die das Projekt Initialisierung.  Finden Sie in der Ausgabe des:

    ![Die Ausgabe der Initialisierung der npm][0]

3. Installieren Sie die Bibliotheken express und Azure-Mobile-apps aus dem Npm Repository aus.

        npm install --save express azure-mobile-apps

4. Erstellen einer app.js-Datei, um die grundlegende mobile Server implementieren.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Diese Anwendung erstellt eine WebAPI Mobile optimiert mit einer einzelnen Endpunkt (`/tables/TodoItem`), der nicht authentifizierten Zugriff auf einen zugrunde liegenden SQL-Datenspeicher mithilfe der dynamischen Schemas bereitstellt.  Eignet sich für die schnelle Starten des Client-Bibliothek folgen:

- [Android-Client-Schnellstart]
- [Apache Cordova Client Schnellstart]
- [iOS-Client-Schnellstart]
- [Schnellstart für Windows Store-Client]
- [Xamarin.iOS Client Schnellstart]
- [Xamarin.Android Client Schnellstart]
- [Xamarin.Forms Client Schnellstart]

Finden Sie den Code für diese einfache Anwendung in der [Stichprobe von Basicapp auf GitHub].

### <a name="a-namehowto-vs2015-basicappahow-to-create-a-node-backend-with-visual-studio-2015"></a><a name="howto-vs2015-basicapp"></a>So: Erstellen einer Back-End-Knoten-mit Visual Studio 2015

Visual Studio 2015 erfordert eine Erweiterung Node.js Applikationen innerhalb der IDE entwickeln möchte.  Installieren Sie zum Starten des [Node.js Tools 1.1 für Visual Studio]aus.  Nachdem die Node.js-Tools für Visual Studio installiert sind, erstellen Sie Anwendung 4.x Express aus:

1. Öffnen Sie das Dialogfeld **Neues Projekt** (aus **Datei** > **neu** > **Projekt...**).

2. Erweitern Sie **Vorlagen** > **JavaScript** > **Node.js**.

3. Wählen Sie die **grundlegenden Azure Node.js Express 4 Anwendung**.

4. Füllen Sie den Projektnamen ein.  Klicken Sie auf *OK*.

    ![Neues Projekt Visual Studio 2015][1]

5. Mit der rechten Maustaste in des Knotens **Npm** , und wählen Sie **neue installieren... Npm Pakete**.

6. Möglicherweise müssen Sie den Katalog Npm über das Erstellen Ihrer ersten Node.js-Anwendungs zu aktualisieren.  Falls erforderlich, klicken Sie auf **Aktualisieren** .

7. Geben Sie in das Suchfeld _Azure-Mobile-apps_ .  Klicken Sie auf das Paket **Azure-Mobile-apps 2.0.0** , und klicken Sie auf **Paket installieren**.

    ![Installieren Sie die neue Npm Pakete][2]

8. Klicken Sie auf **Schließen**.

9. Öffnen Sie die Datei _app.js_ , um Unterstützung für die Azure Mobile Apps SDK hinzuzufügen.  In Zeile 6 at Ende der Bibliothek Anweisungen erforderlich, fügen Sie den folgenden Code hinzu:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Fügen Sie bei ungefähr Zeile 27 nach der anderen app.use Anweisungen den folgenden Code ein:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Speichern Sie die Datei ein.

10. Führen Sie die Anwendung lokal (Http://localhost:3000 ist die API zugestellt) oder in Azure veröffentlichen.

### <a name="a-namecreate-node-backend-portalahow-to-create-a-nodejs-backend-using-the-azure-portal"></a><a name="create-node-backend-portal"></a>So: Erstellen einer Node.js Back-End-über das Azure-Portal

Sie können eine Mobile-App Back-End-rechts im [Azure-Portal]erstellen. Sie können führen Sie die folgenden Schritte oder einem Client und Server zusammen, indem Sie das Lernprogramm [Erstellen Sie eine mobile-app](app-service-mobile-ios-get-started.md) erstellen. Das Lernprogramm enthält eine vereinfachte Version der folgenden Schritte und für Nachweis des Konzepts Projekte am besten geeignet ist.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Wählen Sie wieder in das _Erste Schritte_ -Blade, klicken Sie unter **Erstellen einer Tabelle API** **Node.js** als **Back-End-Sprache**aus. Aktivieren Sie das Kontrollkästchen für "**ich bestätigen, dass dies alle Website Inhalt. überschrieben werden**", und klicken Sie auf **Tabelle TodoItem erstellen**.

### <a name="a-namedownload-quickstartahow-to-download-the-nodejs-backend-quickstart-code-project-using-git"></a><a name="download-quickstart"></a>So: herunterladen das Node.js Back-End-Schnellstart-Codeprojekt Git verwenden

Beim Erstellen einer Node.js Mobile-App Back-End-mithilfe des Portals **Schnellstart** Blade ein Projekts Node.js für Sie erstellt und auf Ihrer Website bereitgestellt. Sie können Hinzufügen von Tabellen und APIs und Codedateien für die Node.js Back-End-im Portal bearbeiten. Sie können auch verschiedene Bereitstellungstools verwenden, das Back-End-Projekt herunterladen, damit Sie können hinzufügen oder Ändern von Tabellen und APIs und dann erneut des Projekts veröffentlichen. Weitere Informationen finden Sie im [Bereitstellungshandbuch für Azure App-Dienst]. Das folgende Verfahren wird ein Repository Git zum Herunterladen des Schnellstart Projekt Codes verwendet.

1. Installieren Sie Git, wenn Sie dies nicht bereits getan haben. Die erforderlichen Schritte zum Installieren der Git unterschiedlich Betriebssysteme aus. Betriebssystem-spezifische Verteilung und Installation Anleitungen finden Sie unter [Installieren von Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) .

2. Führen Sie die Schritte in der Git Repository für Ihre Back-End-Website, die eine Notiz der Bereitstellung Benutzername und Kennwort aktivieren [das App-Dienst app Repository aktivieren](../app-service-web/app-service-deploy-local-git.md#Step3) aus.

3. Notieren Sie in das Blade für Ihre Mobile-App Back-End-die Einstellung **Git Klonen URL** .

4. Führen Sie die `git clone` Befehl mithilfe der Git Klonen URL, Ihr Kennwort ein, wenn erforderlich, wie im folgenden Beispiel eingeben:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Navigieren Sie zu dem lokalen Verzeichnis, das also im vorherigen Beispiel /todolist, und beachten Sie, dass Project-Dateien heruntergeladen wurden. Suchen Sie nach der `todoitem.json` Datei wird die `/tables` Directory.  Diese Datei definiert Berechtigungen für die Tabelle.  Suchen Sie auch die `todoitem.js` Datei im selben Verzeichnis, die die CRUD-Vorgang definiert Skripts für die Tabelle.

6. Nachdem Sie Project-Dateien geändert haben, führen Sie die folgenden Befehle hinzufügen, übernehmen und dann die Änderungen zu der Website hochladen:

        $ git commit -m "updated the table script"
        $ git push origin master

    Wenn Sie neue Dateien zum Projekt hinzufügen, müssen Sie zunächst Ausführen der `git add .` Befehl.

Jedes Mal, wenn ein neuer Satz von Commit zu der Website abgelegt ist, ist die Website veröffentlicht.

### <a name="a-namehowto-publish-to-azureahow-to-publish-your-nodejs-backend-to-azure"></a><a name="howto-publish-to-azure"></a>So: Veröffentlichen Sie Ihre Node.js Back-End-Azure

Microsoft Azure bietet viele Verfahren für die Veröffentlichung der Azure-App-Dienst Mobile Apps Node.js Back-End-zum Azure Dienst an.  Hierzu gehören mit Bereitstellungstools in Visual Studio integriert, Befehlszeile Tools und fortlaufender Bereitstellungsoptionen basierend auf Datenquellen-Steuerelement.  Weitere Informationen zu diesem Thema finden Sie unter der [Azure App Dienst Bereitstellungshandbuch].

Azure App-Verwaltungsdienst hat bestimmten Ratschläge für Node.js-Anwendung, die Sie vor der Bereitstellung überprüfen sollten:

- So [Geben Sie die Version von Knoten]
- So [Verwenden Sie Knoten Module]

### <a name="a-namehowto-enable-homepageahow-to-enable-a-home-page-for-your-application"></a><a name="howto-enable-homepage"></a>So: Aktivieren einer Homepage für eine Anwendung

Viele Clientanwendungen sind eine Kombination aus Web- und mobile-apps und das ExpressJS Framework ermöglicht es Ihnen, die zwei Aspekte zu kombinieren.  Manchmal ist es möglicherweise, möchten Sie jedoch nur eine mobile Schnittstelle implementieren.  Es ist sinnvoll, bereitzustellen, dass eine Seite aus, um die app-Verwaltungsdienst sicherstellen ausgeführt wird.  Sie können Ihre eigene Homepage bereitstellen oder eine temporäre Homepage aktivieren.  Klicken Sie zum Aktivieren einer temporären Homepage anhand der folgenden instanziieren Azure Mobile-Apps:

    var mobile = azureMobileApps({ homePage: true });

Wenn Sie diese Option steht zur Verfügung nur bei der Entwicklung lokal möchten, können Sie diese Einstellung, damit Hinzufügen Ihrer `azureMobile.js` Datei.

## <a name="a-nametableoperationsatable-operations"></a><a name="TableOperations"></a>Tabellenvorgänge 

Das Azure-Mobile-apps Node.js Server SDK enthält Verfahren zum Bereitstellen von Datentabellen in SQL Azure-Datenbank als eine WebAPI gespeichert.  Fünf Vorgänge stehen zur Verfügung.

| Vorgang | Beschreibung |
| --------- | ----------- |
| Abrufen von /tables/_Tabellenname_ | Alle Datensätze in der Tabelle erhalten |
| Abrufen von /tables/_Tabellenname_/:id | Abrufen von einem bestimmten Datensatz in der Tabelle |
| Beitrag /tables/_Tabellenname_ | Erstellen Sie einen Eintrag in der Tabelle |
| PATCH /tables/_Tabellenname_/:id | Aktualisieren Sie einen Eintrag in der Tabelle |
| Löschen von /tables/_Tabellenname_/:id | Löschen Sie einen Datensatz in der Tabelle |

Diese WebAPI unterstützt [OData] und erweitert das Tabellenschema zur Unterstützung von [offline-Daten synchronisieren].

### <a name="a-namehowto-dynamicschemaahow-to-define-tables-using-a-dynamic-schema"></a><a name="howto-dynamicschema"></a>So: Definieren von Tabellen mithilfe der dynamischen Schemas

Bevor Sie eine Tabelle verwendet werden kann, muss er definiert werden.  Tabellen können definiert werden, mit einem statischen Schema (Stelle, an der der Entwickler die Spalten innerhalb des Schemas definiert) oder dynamisch (Stelle, an der das SDK gesteuert wird des Schemas basierend auf eingehenden Anfragen). Darüber hinaus kann der Entwickler bestimmte Aspekte der WebAPI steuern, indem Sie Javascript-Code der Definition hinzufügen.

Als bewährte Methode sollten Sie jede Tabelle in einer Javascript-Datei im Verzeichnis Tabellen definieren und dann mit der tables.import()-Methode können Sie die Tabellen importieren.  Erweitern die Basic-app, würde die Datei app.js angepasst werden:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definieren die Tabelle. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabellen verwenden dynamische Schema standardmäßig an.  Legen Sie zum Deaktivieren des dynamischen Schema Global der App-Einstellung **MS_DynamicSchema** auf falsch innerhalb des Portals Azure ein.

Sie können ein vollständiges Beispiel in der [Stichprobe erledigen, klicken Sie auf GitHub]suchen.

### <a name="a-namehowto-staticschemaahow-to-define-tables-using-a-static-schema"></a><a name="howto-staticschema"></a>So: Definieren von Tabellen mithilfe einer statischen Schema

Sie können die Spalten, die über die WebAPI verfügbar machen explizit definieren.  Der Azure-Mobile-apps Node.js SDK fügt automatisch alle zusätzlichen Spalten erforderlich für offline-Daten synchronisieren der Liste, die Sie bereitstellen.  Die Schnellstart-Clientanwendungen erfordern beispielsweise eine Tabelle mit zwei Spalten: Text (Zeichenfolge) und abschließen (ein boolescher).  
Die Tabelle kann in der Tabelle Definition JavaScript-Datei (befindet sich im Verzeichnis Tabellen) wie folgt definiert werden:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Wenn Sie Tabellen statisch definieren, müssen Sie auch die tables.initialize()-Methode, um das Schema der Datenbank beim Start erstellen aufrufen.  Die Methode tables.initialize() gibt eine [Versprechen] , damit der Webdienst dienen keinen Anfragen vor der Initialisierung Datenbank.

### <a name="a-namehowto-sqlexpress-setupahow-to-use-sql-express-as-a-development-data-store-on-your-local-machine"></a><a name="howto-sqlexpress-setup"></a>So: SQL Express als Datenspeicher Entwicklung auf Ihrem lokalen Computer verwenden

Das Azure Mobile-Apps im AzureMobile Apps Knoten SDK bietet drei Optionen für die Bereitstellung von Daten im Paket: SDK bietet drei Optionen für die Bereitstellung von Daten im Paket:

- Verwenden Sie den **Arbeitsspeicher** Treiber eine nicht beständige Beispiel Store bereitstellen
- Verwenden Sie den **Mssql** -Treiber zu einen Datenspeicher SQL Express zur Entwicklung bereitstellen
- Verwenden Sie den **Mssql** -Treiber Herstellung einen Azure SQL-Datenbank Datenspeicher hinzufügen

Die Azure Mobile Apps Node.js SDK verwendet die [Mssql Node.js Paket] zu erstellen und eine Verbindung mit SQL Express und SQL-Datenbank zu verwenden.  Dieses Paket benötigt, auf der SQL Express-Instanz TCP-Verbindungen zu aktivieren.

> [AZURE.TIP]Memory-Treiber bietet keinen vollständigen Satz von Funktionen zum Testen.  Wenn Sie Ihre Back-End-lokal testen möchten, empfehlen wir die Verwendung von einem Datenspeicher SQL Express und der Mssql-Treiber.

1. Herunterladen Sie und installieren Sie [Microsoft SQL Server 2014 Express].  Stellen Sie sicher, dass Sie die SQL Server 2014 Express mit Tools Edition installieren.  Es sei denn, Sie 64-Bit-Unterstützung explizit erforderlich ist, verbraucht die 32-Bit-Version weniger Speicher beim Ausführen.

2. Führen Sie den SQL Server 2014 Konfigurations-Manager.

  1. Erweitern Sie in der linken Struktur Menü **SQL Server-Netzwerkkonfiguration** .
  2. Klicken Sie auf **Protokolle für SQL Express**.
  3. Mit der rechten Maustaste **TCP/IP** , und wählen Sie **Aktivieren**aus.  Klicken Sie im Popupfenster auf **OK** .
  4. Mit der rechten Maustaste **TCP/IP** , und wählen Sie **Eigenschaften**aus.
  5. Klicken Sie auf der Registerkarte **IP-Adressen** .
  6. Suchen Sie den Knoten **IPAll** .  Geben Sie im Feld **TCP-Anschluss** **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Klicken Sie auf **OK**.  Klicken Sie im Popupfenster auf **OK** .
  8. Klicken Sie auf das Menü linken Struktur **SQL Server-Dienste** .
  9. Mit der rechten Maustaste in **SQL Server (SQLEXPRESS)** , und wählen Sie **neu starten**
  10. Schließen Sie den SQL Server 2014 Konfigurations-Manager.

3. Führen Sie die SQL Server 2014 Management Studio und Verbinden mit Ihrem lokalen SQL Express-Instanz

  1. Mit der rechten Maustaste in der Instanz im Objekt-Explorer, und wählen Sie **Eigenschaften** aus.
  2. Wählen Sie die Seite **Sicherheit** .
  3. Stellen Sie sicher, dass die **SQL Server und Windows-Authentifizierungsmodus** ausgewählt ist
  4. Klicken Sie auf **OK**

        ![Konfigurieren der SQL Express-Authentifizierung][4]

  5. Erweitern Sie **Sicherheit** > **Benutzernamen** im Objekt-Explorer
  6. Klicken Sie auf **Anmeldung** , und wählen Sie **Neuer Benutzername...**
  7. Geben Sie einen Benutzernamen ein.  Wählen Sie **SQL Server-Authentifizierung**aus.  Geben Sie ein Kennwort ein, und klicken Sie dann im Feld **Kennwort bestätigen**Geben Sie dasselbe Kennwort ein.  Das Kennwort muss Windows Komplexität Anforderungen entsprechen.
  8. Klicken Sie auf **OK**

        ![Hinzufügen eines neuen Benutzers mit SQL Express][5]

  9. Mit der rechten Maustaste in Ihren neuen Benutzernamen und wählen Sie **Eigenschaften** aus.
  10. Wählen Sie die Seite **Serverrollen**
  11. Aktivieren Sie das Kontrollkästchen neben der Rolle **Dbcreator** server
  12. Klicken Sie auf **OK**
  13. Schließen Sie das SQL Server 2015 Management Studio

Stellen Sie sicher, dass Sie aufzeichnen, den Benutzernamen und das Kennwort ein, das Sie ausgewählt haben.  Sie müssen möglicherweise zusätzliche Servertypen Rollen oder je nach Ihren Anforderungen Datenbank bestimmte Berechtigungen zuweisen.

Die Anwendung Node.js liest die **SQLCONNSTR_MS_TableConnectionString** Umgebungsvariable für die Verbindungszeichenfolge für diese Datenbank.  Sie können diese Variable in Ihrer Umgebung festlegen.  PowerShell können Sie diese Umgebungsvariable festzulegen:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Zugriff auf die Datenbank über eine Verbindung TCP/IP, und geben Sie einen Benutzernamen und ein Kennwort für die Verbindung.

### <a name="a-namehowto-config-localdevahow-to-configure-your-project-for-local-development"></a><a name="howto-config-localdev"></a>So: Konfigurieren des Projekts für lokale Entwicklung

Azure Mobile-Apps liest eine JavaScript-Datei im lokalen Dateisystem _azureMobile.js_ aufgerufen.  Verwenden Sie diese Datei nicht zum Konfigurieren der Herstellung der Azure Mobile Apps SDK: Verwenden Sie stattdessen die Einstellungen für die App innerhalb des [Azure-Portal] aus.  Die Datei _azureMobile.js_ sollte ein Konfigurationsobjekt exportieren.  Die am häufigsten verwendeten Einstellungen sind:

- Datenbank-Einstellungen
- Diagnoseprotokolle Einstellungen
- Alternative CORS-Einstellungen

Eine _azureMobile.js_ -Beispieldatei Implementieren der vorherigen Datenbank Einstellungen umfasst:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Es empfiehlt sich, dass Sie die Datei _.gitignore_ _azureMobile.js_ hinzufügen (oder andere quellcodeverwaltung ignorieren-Datei) zu verhindern, dass Kennwörter, die in der Cloud gespeichert werden.  Konfigurieren von Einstellungen für die Herstellung immer in Einstellungen für die App innerhalb des [Azure-Portal].

### <a name="a-namehowto-appsettingsahow-configure-app-settings-for-your-mobile-app"></a><a name="howto-appsettings"></a>So wird's: Konfigurieren von Einstellungen für die App für Mobile-App

Die meisten Einstellungen in der Datei _azureMobile.js_ haben eine entsprechende App-Einstellung der [Azure-Portal]an.  Verwenden Sie die folgende Liste so konfigurieren Sie die app in den Einstellungen für die App aus:

| App-Einstellung                 | _azureMobile.js_ -Einstellung  | Beschreibung                               | Gültige Werte                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | Namen                      | Die Namen der app                       | Zeichenfolge                                      |
| **MS_MobileLoggingLevel**   | Logging.Level             | Protokolldatei minimal Ebene Nachrichten protokollieren      | Fehler, Warnung, Informationen, ausführliche, Debuggen alberne |
| **MS_DebugMode**            | Debuggen                     | Aktivieren oder Deaktivieren von Debuggen-Modus              | WAHR, falsch                                 |
| **MS_TableSchema**          | Data.Schema               | Schema Standardnamen für SQL-Tabellen        | Zeichenfolge (Standard: Dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Aktivieren oder Deaktivieren von Debuggen-Modus              | WAHR, falsch                                 |
| **MS_DisableVersionHeader** | Version (festgelegt zu nicht definierten)| Deaktiviert die Kopfzeile X-ZUMO-Server-Version | WAHR, falsch                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Wird die-Client-API Version Prüfung deaktiviert     | WAHR, falsch                                 |

So richten Sie eine App-Einstellung

1. Melden Sie sich mit dem [Azure-Portal]an.
2. Wählen Sie **alle Ressourcen** oder **App-Dienste** , und klicken Sie auf den Namen der App Mobile.
3. Standardmäßig wird das Blade Einstellungen geöffnet. Wenn dies nicht der Fall, klicken Sie auf **Einstellungen**.
4. Klicken Sie im Menü "Allgemein" auf **Einstellungen für die Anwendung** .
5. Führen Sie einen Bildlauf zum Abschnitt Einstellungen für die App.
6. Wenn Ihre app festlegen bereits vorhanden ist, klicken Sie auf den Wert für die app-Einstellung, um den Wert zu bearbeiten.
7. Wenn Ihre app-Einstellung nicht vorhanden ist, geben Sie die App-Einstellung im Feld Schlüssel und der Wert im Feld Wert ein.
8. Sobald Sie abgeschlossen sind, klicken Sie auf **Speichern**.

Ändern die meisten Einstellungen für die app erfordert einen Neustart der Dienste.

### <a name="a-namehowto-use-sqlazureahow-to-use-sql-database-as-your-production-data-store"></a><a name="howto-use-sqlazure"></a>So: SQL-Datenbank als der Herstellung Datenspeicher verwenden

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Verwendung von Azure SQL-Datenbank als Datenspeicher ist für alle Arten von App-Verwaltungsdienst Azure-Anwendung identisch. Gehen folgendermaßen Sie vor, zum Erstellen einer Mobile-App Back-End-, wenn Sie dies noch nicht getan haben.

1. Melden Sie sich mit dem [Azure-Portal]an.

2. Oben links im Fenster, klicken Sie auf die Schaltfläche **+ neu** > **Web + Mobile** > **Mobile-App**, geben Sie dann einen Namen für Ihre Mobile-App Back-End.

3. Geben Sie im Feld **Ressourcengruppe** denselben Namen wie Ihre app ein.

4. Der Standard-App-Serviceplan ausgewählt ist.  Wenn Sie Ihre App-Serviceplan ändern möchten, können Sie dies ausführen, durch Klicken auf die App-Serviceplan > **+ neu erstellen**.  Geben Sie einen Namen für die neue App-Serviceplan, und wählen Sie eine geeignete Stelle.  Klicken Sie auf die Preise Leiste, und wählen Sie einen entsprechenden Preisgestaltung Ebenen für den Dienst. Wählen Sie **Ansicht alle** Weitere Optionen Preisgestaltung, wie z. B. **Free** und **freigegebene**anzeigen.  Nachdem Sie die Preise Ebene ausgewählt haben, klicken Sie auf die Schaltfläche **auswählen** .  Wieder in der **App-Serviceplan** Blade klicken Sie auf **OK**.

5. Klicken Sie auf **Erstellen**. Eine Mobile-App Back-End-Bereitstellung kann einige Minuten dauern.  Nachdem Sie die Mobile-App Back-End-bereitgestellt wird, wird im Portal das Blade **Einstellungen** für die Mobile-App Back-End-geöffnet.

Nachdem die Mobile-App Back-End-erstellt wurde, können Sie auswählen, um Ihre Mobile-App Back-End-eine vorhandene SQL­Datenbank herzustellen oder eine neue SQL-Datenbank erstellen.  In diesem Abschnitt erstellen wir eine SQL-Datenbank.

> [AZURE.NOTE]Wenn Sie bereits eine Datenbank im selben Speicherort wie das mobile-app Back-End-verfügen, können stattdessen **Verwenden einer vorhandenen Datenbank** auszuwählen und wählen Sie dann auf diese Datenbank. Die Verwendung einer Datenbank an einem anderen Speicherort ist aufgrund der höheren Wartezeiten nicht empfohlen.

6. Klicken Sie in der neuen Mobile-App Back-End- **Einstellungen**auf > **Mobile-App** > **Daten** > **+ Hinzufügen**.

7. Klicken Sie in das Blade **Verbindung zum Hinzufügen von Daten** auf **SQL-Datenbank - erforderliche Einstellungen konfigurieren** > **Erstellen einer neuen Datenbank**.  Geben Sie den Namen der neuen Datenbank in das Feld **Name** ein.

8. Klicken Sie auf **Server**.  Geben Sie in das **neue Server** -Blade einen eindeutigen Servernamen in das Feld " **Servername** ", und geben Sie einen geeigneten **Server Administrator-Benutzernamen** und **ein Kennwort**.  Stellen Sie sicher, dass der **Server Zugriff zulassen Azure-Dienste** aktiviert ist.  Klicken Sie auf **OK**.

    ![Erstellen einer SQL Azure-Datenbank][6]

9. Klicken Sie auf das **neue Datenbank** Blade klicken Sie auf **OK**.

10. Klicken Sie auf das Blade **Verbindung zum Hinzufügen von Daten** wählen Sie **Verbindungszeichenfolge**aus, geben Sie den Benutzernamen und das Kennwort, die Ihnen beim Erstellen der Datenbank erteilt.  Wenn Sie eine vorhandene Datenbank verwenden, geben Sie die Anmeldeinformationen für die Datenbank.  Wenn Sie eingegeben haben, klicken Sie auf **OK**.

11. Klicken Sie auf die **Verbindung zum Hinzufügen von Daten** Blade wieder zurück, klicken Sie auf **OK** , um die Datenbank zu erstellen.

<!--- END OF ALTERNATE INCLUDE -->

Erstellung der Datenbank kann einige Minuten dauern.  Verwenden Sie **im Benachrichtigungsbereich** , um den Fortschritt der Bereitstellung überwachen.  Führen Sie die keine ausgeführt, bis die Datenbank erfolgreich bereitgestellt wurde.  Sobald Sie erfolgreich bereitgestellt haben, wird eine Verbindungszeichenfolge für die Instanz SQL-Datenbank in der mobilen Back-End-Einstellungen für die App erstellt.  Sie können diese Einstellung app in den **Einstellungen**finden Sie unter > **Anwendungseinstellungen** > **Verbindungszeichenfolgen**.

### <a name="a-namehowto-tables-authahow-to-require-authentication-for-access-to-tables"></a><a name="howto-tables-auth"></a>So: Authentifizierung für den Zugriff auf Tabellen

Wenn Sie die App-Authentifizierung mit dem Tabellen-Endpunkt verwenden möchten, müssen Sie zuerst App-Authentifizierung im [Portal Azure] konfigurieren.  Weitere Informationen zum Konfigurieren der Authentifizierung in einer App-Verwaltungsdienst Azure finden Sie im Konfigurations-Leitfaden für die Identitätsanbieter, die Sie verwenden möchten:

- [So konfigurieren Sie Azure-Active Directory-Authentifizierung]
- [So konfigurieren Sie die Facebook-Authentifizierung]
- [So konfigurieren Sie die Google-Authentifizierung]
- [So konfigurieren Sie Microsoft Authentication]
- [So konfigurieren Sie die Twitter-Authentifizierung]

Jede Tabelle weist eine Access-Eigenschaft, die zum Steuern des Zugriffs auf die Tabelle verwendet werden kann.  Im folgende Beispiel wird eine statisch definierte Tabelle mit Authentifizierung erforderlich.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Die Access-Eigenschaft kann einen der drei Werte annehmen.

  - *anonyme* gibt an, dass die Clientanwendung zum Lesen von Daten ohne Authentifizierung zulässig ist
  - *authentifiziert* zeigt an, dass die Clientanwendung eine gültige Authentifizierungstoken mit der Anforderung gesendet werden müssen
  - *deaktiviert* zeigt an, dass diese Tabelle aktuell deaktiviert ist

Wenn Sie die Access-Eigenschaft definiert ist, ist nicht authentifizierter Zugriff gewährt.

### <a name="a-namehowto-tables-getidentityahow-to-use-authentication-claims-with-your-tables"></a><a name="howto-tables-getidentity"></a>So: Authentifizierung Ansprüche mit Tabellen verwenden

Sie können verschiedene Ansprüche einrichten, die angefordert werden, wenn Authentifizierung eingerichtet ist.  Diese Ansprüche sind nicht normal verfügbar über die `context.user` Objekt.  Jedoch, sie können mit abgerufen werden die `context.user.getIdentity()` Methode.  Die `getIdentity()` Methode gibt ein Versprechen, die in ein Objekt aufgelöst wird.  Das Objekt ist nach der Authentifizierungsmethode (Facebook, Google, Twitter, Microsoftaccount oder Aad) zu sortieren.

Wenn Sie Microsoft Account Authentifizierung und der Anforderung, die die e-Mail-Adressen beanspruchen einrichten, können Sie beispielsweise die e-Mail-Adresse auf den Datensatz mit der folgenden Tabelle Controller hinzufügen:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Um anzuzeigen, welche Ansprüche verfügbar sind, verwenden Sie einen Webbrowser zum Anzeigen der `/.auth/me` Endpunkt Ihrer Website.

### <a name="a-namehowto-tables-disabledahow-to-disable-access-to-specific-table-operations"></a><a name="howto-tables-disabled"></a>So: Deaktivieren des Zugriffs auf bestimmte Tabellenvorgänge

Zusätzlich in der Tabelle angezeigt werden, kann die Access-Eigenschaft verwendet werden, einzelne Vorgänge zu steuern.  Es gibt vier Vorgänge aus:

  - *Lesen* ist der Vorgang Rest erhalten, klicken Sie auf die Tabelle
  - *Einfügen* ist der Rest Beitrag Vorgang in der Tabelle
  - *Aktualisieren* ist der Rest PATCH Vorgang in der Tabelle
  - *Löschen* ist der Vorgang Rest löschen, klicken Sie auf die Tabelle

Möglicherweise möchten Sie eine schreibgeschützte nicht authentifizierte Tabelle zur Verfügung:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="a-namehowto-tables-queryahow-to-adjust-the-query-that-is-used-with-table-operations"></a><a name="howto-tables-query"></a>So: Passen Sie die Abfrage, die mit Vorgängen Tabelle verwendet wird

Eine allgemeine Anforderung für Tabellenvorgänge stellen eine eingeschränkte Ansicht der Daten bereit.  Beispielsweise können Sie eine Tabelle angeben, die mit authentifizierten Benutzer-ID markiert ist, so, dass nur gelesen oder Ihre eigenen Einträge aktualisieren können.  Die folgende Tabellendefinition stellt diese Funktionalität bereit:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Vorgänge, die normalerweise einer Abfrage ausführen weisen eine Abfrageeigenschaft, die Sie, mit einer Where anpassen können-Klausel. Die Abfrageeigenschaft ist ein [QueryJS] -Objekt, das mit dem OData-Abfrage auf bestimmte konvertiert, die die Daten Back-End-verarbeitet werden kann.  Einfache Übereinstimmungsvergleiche Fällen (wie das vorherige) kann eine Karte verwendet werden. Sie können auch bestimmte SQL-Klauseln hinzufügen:

    context.query.where('myfield eq ?', 'value');

### <a name="a-namehowto-tables-softdeleteahow-to-configure-soft-delete-on-a-table"></a><a name="howto-tables-softdelete"></a>So: Konfigurieren der weiche löschen auf einer Tabelle

Weiche löschen Löscht tatsächlich keine Datensätze.  Stattdessen kennzeichnet sie als in der Datenbank gelöscht, indem Sie die gelöschten Spalte auf True.  Azure Mobile-Apps-SDK werden automatisch weiche gelöschte Datensätze aus Ergebnisse entfernt, es sei denn, das Mobile-Client-SDK IncludeDeleted() verwendet.  Zum Konfigurieren einer Tabelle für weiche löschen legen die `softDelete` Eigenschaft in der Tabelle Formulardefinitionsdatei:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Sie sollten ein Verfahren für das Löschen von Datensätzen – entweder von einer Clientanwendung über eine WebJob, Azure-Funktion oder über eine benutzerdefinierte API einführen.

### <a name="a-namehowto-tables-seedingahow-to-seed-your-database-with-data"></a><a name="howto-tables-seeding"></a>So: Startwert für Ihre Datenbank mit Daten

Beim Erstellen einer neuen Anwendungs möchten Sie möglicherweise eine Tabelle mit Daten zu starten.  Dies kann in der Tabelle Definition JavaScript-Datei wie folgt erfolgen:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Sendet Daten erfolgt nur, wenn die Tabelle vom Azure Mobile Apps SDK erstellt wird.  Wenn die Tabelle in der Datenbank bereits vorhanden ist, werden keine Daten in die Tabelle eingefügt.  Wenn dynamische Schema aktiviert ist, wird das Schema aus den Daten vergebene abgeleitet.

Es empfiehlt sich, dass Sie explizit aufrufen der `tables.initialize()` Methode zum Erstellen einer Tabelle beim Starten des Diensts ausgeführt.

### <a name="a-nameswaggerahow-to-enable-swagger-support"></a><a name="Swagger"></a>So: Aktivieren der Unterstützung von Swagger

Azure App Dienst Mobile-Apps im Lieferumfang von integrierten [Swagger] Support.  Installieren Sie zuerst die Swagger-Benutzeroberfläche als Abhängigkeit, um Swagger Support zu aktivieren:

    npm install --save swagger-ui

Nach der Installation können Sie Swagger Support im Konstruktor Azure Mobile-Apps aktivieren:

    var mobile = azureMobileApps({ swagger: true });

Sie wahrscheinlich nur Swagger Unterstützung in Entwicklung Editionen aktivieren möchten.  Hierzu können Sie mit der `NODE_ENV` app-Einstellung:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Der Endpunkt Swagger befindet sich unter http://_Standortname_.azurewebsites.net/swagger.  Sie können über die Benutzeroberfläche Swagger zugreifen, die `/swagger/ui` Endpunkt.  Wenn Sie der gesamten Anwendung Authentifizierung erforderlich ist auswählen, erzeugt Swagger einen Fehler auf.  Wählen Sie die besten Ergebnisse erzielen Sie, um nicht authentifizierte Anfragen bis in der Azure-App-Authentifizierung zulässig sind / Autorisierung Einstellungen, klicken Sie dann steuern Authentifizierung mithilfe der `table.access` Eigenschaft.

Sie können auch die Option Swagger zum Hinzufügen Ihrer `azureMobile.js` ablegen, wenn Sie nur Swagger Unterstützung bei der Entwicklung lokal.

## <a name="a-namepushpush-notifications"></a><a name="push">Pushbenachrichtigungen

Mobile-Apps arbeitet mit Azure Benachrichtigung Hubs, sodass Sie gezielte Pushbenachrichtigungen an Millionen von Geräten über alle wichtigen Plattformen senden können. Mithilfe von Hubs Benachrichtigung können Sie Pushbenachrichtigungen senden, um iOS, Android und Windows-Geräten. Weitere Informationen zu allen, Sie können mit Benachrichtigung Hubs führen, finden Sie unter [Übersicht über die Benachrichtigung Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

### <a name="aa-namesend-pushahow-to-send-push-notifications"></a></a><a name="send-push"></a>So: Senden von Pushbenachrichtigungen

Im folgende Code veranschaulicht, wie das Objekt Pushbenachrichtigungen mithilfe einer übertragenen Pushbenachrichtigung registrierten iOS-Geräte senden:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Durch das Erstellen einer Vorlage Pushbenachrichtigungen Registrierung vom Client, können Sie stattdessen eine Vorlage Pushbenachrichtigungen Nachricht auf Geräte auf allen unterstützten Plattformen senden. Der folgende Code wird gezeigt, wie eine Vorlage Benachrichtigung gesendet werden:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="a-namepush-userahow-to-send-push-notifications-to-an-authenticated-user-using-tags"></a><a name="push-user"></a>So: Senden von Pushbenachrichtigungen für einen authentifizierten Benutzer mithilfe von Kategorien

Wenn ein authentifizierter Benutzer für Pushbenachrichtigungen registriert hat, wird ein Benutzer-ID-Tag automatisch auf die Erfassung hinzugefügt. Mithilfe dieses Tag, können Sie auf alle Geräte, die von einem bestimmten Benutzer registriert Pushbenachrichtigungen senden. Im folgenden Code wird die SID des Benutzers, die die Anforderung ausführenden und sendet der Pushbenachrichtigung eine Vorlage auf jedem Gerät Registrierung für diesen Benutzer:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Beim Registrieren für Pushbenachrichtigungen von einem authentifizierten Client, stellen Sie sicher, dass die Authentifizierung abgeschlossen ist, bevor Sie versuchen, die Registrierung.

## <a name="a-namecustomapia-custom-apis"></a><a name="CustomAPI"></a>Benutzerdefinierte APIs

###  <a name="a-namehowto-customapi-basicahow-to-define-a-custom-api"></a><a name="howto-customapi-basic"></a>So: definieren eine benutzerdefinierte-API


Die Daten-Access API über den Endpunkt /tables können Azure Mobile-Apps benutzerdefinierte API Schutz informieren.  Benutzerdefinierte APIs werden in ähnlicher Weise auf die Tabellendefinitionen definiert und können die gleichen Funktionen, einschließlich Authentifizierung zugreifen.

Wenn Sie die App-Authentifizierung API eine benutzerdefinierte verwenden möchten, müssen Sie zuerst App-Authentifizierung im [Portal Azure] konfigurieren.  Weitere Informationen zum Konfigurieren der Authentifizierung in einer App-Verwaltungsdienst Azure finden Sie im Konfigurations-Leitfaden für die Identitätsanbieter, die Sie verwenden möchten:

- [So konfigurieren Sie Azure-Active Directory-Authentifizierung]
- [So konfigurieren Sie die Facebook-Authentifizierung]
- [So konfigurieren Sie die Google-Authentifizierung]
- [So konfigurieren Sie Microsoft Authentication]
- [So konfigurieren Sie die Twitter-Authentifizierung]

Benutzerdefinierte APIs werden ähnlich wie die Tabellen-API definiert.

1. Erstellen einer **API-** Verzeichnis
2. Erstellen einer API Definition JavaScript-Datei im Verzeichnis **api** .
3. Verwenden Sie die Methode importieren, um **api** Verzeichnis zu importieren.

Hier sind die Prototypen api Definition basierend auf den Basic-app-Beispieldaten, den wir zuvor verwendet.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Sehen wir uns ein Beispiel-API, die das Datum des Servers mithilfe der Methode _Date.now()_ zurückgibt.  Hier wird die api/date.js-Datei ein:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Jeder Parameter ist eines der standard Rest Verben - abrufen, einen Beitrag, PATCH oder löschen.  Die Methode ist eine Standardansicht [ExpressJS Middleware] -Funktion, die die gewünschten Ausgabe sendet.

### <a name="a-namehowto-customapi-authahow-to-require-authentication-for-access-to-a-custom-api"></a><a name="howto-customapi-auth"></a>So: Authentifizierung für den Zugriff auf eine benutzerdefinierte API erforderlich

Azure Mobile Apps SDK implementiert Authentifizierung auf die gleiche Weise für den Endpunkt Tabellen und benutzerdefinierte APIs.  Fügen Sie einer **Access** -Eigenschaft hinzu, um die im vorherigen Abschnitt entwickelt API Authentifizierung hinzuzufügen:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Sie können auch die Authentifizierung auf bestimmte Vorgänge angeben:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Das gleiche Token, das für den Endpunkt Tabellen verwendet wird, muss für benutzerdefinierte APIs Anfordern einer Authentifizierung verwendet werden.

### <a name="a-namehowto-customapi-authahow-to-handle-large-file-uploads"></a><a name="howto-customapi-auth"></a>So: Behandeln von großen Dateiuploads

Azure Mobile Apps SDK verwendet die [Textkörper-Parser Middleware](https://github.com/expressjs/body-parser) akzeptieren und Entschlüsseln Textkörpers in Ihren Beitrag ein.  Sie können vordefinierte Textkörper-Parser zum annehmen, dass größere Dateiuploads konfigurieren:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Die Datei ist Base-64-codierte vor der Übertragung an.  Dadurch wird die Größe des tatsächlichen Uploads erhöht (und daher die Größe Sie müssen Konto).

### <a name="a-namehowto-customapi-sqlahow-to-execute-custom-sql-statements"></a><a name="howto-customapi-sql"></a>So: benutzerdefinierte SQL-Anweisung ausführen

Die Mobile Apps SDK Azure ermöglicht den Zugriff auf den gesamten Kontext über das Anforderungsobjekt ermöglicht es Ihnen leicht auszuführende parametrisierte SQL-Anweisungen für den definierten-Datenanbieter:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="a-namedebuggingadebugging-easy-tables-and-easy-apis"></a><a name="Debugging"></a>Für das Debuggen, einfache Tabellen und einfach APIs

### <a name="a-namehowto-diagnostic-logsahow-to-debug-diagnose-and-troubleshoot-azure-mobile-apps"></a><a name="howto-diagnostic-logs"></a>So: Debuggen, diagnose und Problembehandlung bei Azure Mobile-apps

Der Azure-App-Dienst bietet verschiedene für das Debuggen und Techniken für Applikationen Node.js zur Problembehandlung.
Finden Sie in den folgenden Artikeln bei der Problembehandlung Ihrer Node.js Mobile Back-End-Schritte:

- [Für die Überwachung eines Azure App-Diensts]
- [Diagnoseprotokolle in Azure App-Verwaltungsdienst aktivieren]
- [Behandeln von Problemen mit einer Azure App-Dienst in Visual Studio]

Node.js Applikationen Zugang zu einer Vielzahl von Tools Diagnoseprotokoll.  Intern verwendet die Azure Mobile Apps Node.js SDK [Winston] für diagnoseprotokollierung.  Protokollierung wird automatisch durch das Debuggen-Modus aktivieren oder indem Sie die app-Einstellung **MS_DebugMode** auf WAHR im [Portal Azure]aktiviert. Generierte Protokolle werden in den Diagnoseprotokollen in [Azure-Portal].

### <a name="a-namein-portal-editingaa-namework-easy-tablesahow-to-work-with-easy-tables-in-the-azure-portal"></a><a name="in-portal-editing"></a><a name="work-easy-tables"></a>So: Arbeiten mit Tabellen einfach im Azure-Portal

Einfache Tabellen im Portal lassen Sie das Erstellen von und Arbeiten mit Tabellen rechts im Portal. Tabellenvorgänge mit dem App-Service-Editor können Sie auch bearbeiten.

Wenn Sie **einfache Tabellen** in der Einstellungen für die Back-End-Website klicken, können Sie hinzufügen, ändern oder Löschen einer Tabelle. Sie können auch die Daten in der Tabelle anzeigen.

![Arbeiten mit Tabellen einfach](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Die folgenden Befehle zur Verfügung, auf der Befehlsleiste für eine Tabelle:

+ **Berechtigungen ändern** – ändern die Berechtigung zum Lesen, einfügen, aktualisieren und Vorgänge in der Tabelle löschen. 
  Optionen sind und anonyme Authentifizierung erforderlich ist, oder deaktivieren alle Zugriff auf den Vorgang Zugriff zulassen. 
+ **Bearbeiten von Skript** - Skriptdatei für die Tabelle wird in der App-Service-Editor geöffnet.
+ **Verwalten Schema** - hinzufügen oder Löschen von Spalten oder ändern Sie den Tabellenindex.
+ **Tabelle löschen** - kürzt eine vorhandene Tabelle werden alle Datenzeilen löschen, aber das Schema verlassen unverändert.
+ **Löschen von Zeilen** : einzelne Zeilen mit Daten löschen.
+ **Anzeigen von Protokollen streaming** -, die Sie mit der streaming Log-Dienst für Ihre Website verbunden.

###<a name="a-namework-easy-apisahow-to-work-with-easy-apis-in-the-azure-portal"></a><a name="work-easy-apis"></a>So: Arbeiten mit einfach APIs der Azure-Portal

Einfache APIs im Portal können Sie erstellen und Arbeiten mit benutzerdefinierten APIs rechts im Portal. Sie können die API Skripts mit dem App-Service-Editor bearbeiten.

Wenn Sie **Einfach APIs** in der Einstellungen für die Back-End-Website klicken, können Sie hinzufügen, ändern oder Löschen einen benutzerdefinierten API Endpunkt.

![Arbeiten mit einfach APIs](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Im Portal können Sie ändern die Zugriffsberechtigungen für einen angegebenen HTTP-Aktion, die API-Skriptdatei im App-Service-Editor bearbeiten oder das streaming Protokolle anzeigen.

###<a name="a-nameonline-editorahow-to-edit-code-in-the-app-service-editor"></a><a name="online-editor"></a>So: Code in den App-Service-Editor bearbeiten

Azure-Portal können Sie Ihre Node.js Back-End-Skript-Dateien in der App-Editor-Dienst zu bearbeiten, ohne dass das Projekt auf Ihrem lokalen Computer herunterzuladen. Skriptdateien in den online-Editor bearbeiten zu können:

1. In Ihre Mobile-App Back-End-Blade, klicken Sie auf **Alle Einstellungen** > entweder **einfache Tabellen** oder **Einfach APIs**, einer Tabelle oder API klicken Sie auf **Skript bearbeiten**. Die Skriptdatei wird in der App-Service-Editor geöffnet.

    ![App-Editor-Dienst](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Stellen Sie Ihre Änderungen zu der Codedatei im online-Editor ein. Änderungen werden automatisch gespeichert, während der Eingabe.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android-Client-Schnellstart]: app-service-mobile-android-get-started.md
[Apache Cordova Client Schnellstart]: app-service-mobile-cordova-get-started.md
[iOS-Client-Schnellstart]: app-service-mobile-ios-get-started.md
[Xamarin.iOS Client Schnellstart]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android Client Schnellstart]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms Client Schnellstart]: app-service-mobile-xamarin-forms-get-started.md
[Schnellstart für Windows Store-Client]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[Offline-Daten synchronisieren]: app-service-mobile-offline-data-sync.md
[So konfigurieren Sie Azure-Active Directory-Authentifizierung]: app-service-mobile-how-to-configure-active-directory-authentication.md
[So konfigurieren Sie die Facebook-Authentifizierung]: app-service-mobile-how-to-configure-facebook-authentication.md
[So konfigurieren Sie die Google-Authentifizierung]: app-service-mobile-how-to-configure-google-authentication.md
[So konfigurieren Sie Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[So konfigurieren Sie die Twitter-Authentifizierung]: app-service-mobile-how-to-configure-twitter-authentication.md
[Bereitstellungshandbuch für Azure App-Verwaltungsdienst]: ../app-service-web/web-sites-deploy.md
[Für die Überwachung eines Azure App-Diensts]: ../app-service-web/web-sites-monitor.md
[Diagnoseprotokolle in Azure App-Verwaltungsdienst aktivieren]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Behandeln von Problemen mit einer Azure App-Dienst in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Geben Sie die Version von Knoten]: ../nodejs-specify-node-version-azure-apps.md
[Verwenden von Knoten Module]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Versprechen]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Basicapp Beispiel auf GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[erledigen Stichprobe auf GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[Beispielverzeichnis auf GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 für Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js-Paket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
