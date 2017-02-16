<properties
    pageTitle="Aktualisierung von Mobile Dienste auf Azure App-Verwaltungsdienst - Node.js"
    description="Erfahren Sie, wie Ihre Mobile Services-Anwendung zu einer App-Dienst Mobile-App Upgrades"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Aktualisieren Sie Ihre vorhandene Node.js Azure Mobile Service auf App-Dienst

Mobile-App-Dienst ist eine neue Möglichkeit, die mit Microsoft Azure Windows-Dienste zu erstellen. Weitere Informationen finden Sie unter [Was Mobile-Apps werden?].

In diesem Thema beschrieben, wie eine vorhandene Node.js Back-End-Anwendung von Azure Mobile-Dienste zu einer neuen App Dienst Mobile-Apps zu aktualisieren. Während Sie diese Aktualisierung durchführen, kann Ihre vorhandene Mobile Services-Anwendung weiterhin ausgeführt werden.  Wenn Sie eine Node.js Back-End-Anwendung aktualisieren müssen, lesen Sie [Upgrade Ihrer .NET Mobile Dienste](./app-service-mobile-net-upgrading-from-mobile-services.md).

Wenn eine mobile Back-End-auf App-Verwaltungsdienst Azure aktualisiert wird, hat Zugriff auf alle App-Funktionen und entsprechend der [App-Dienst Preise]nicht Mobile Dienste Preise in Rechnung gestellt werden.

## <a name="migrate-vs-upgrade"></a>Migrieren von im Vergleich zu upgrade

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Es wird empfohlen, die Sie [Ausführen eine Migration](app-service-mobile-migrating-from-mobile-services.md) vor dem Wechsel durch eine Aktualisierung. Auf diese Weise können Sie setzen beide Versionen der Anwendung auf der gleichen App Dienst planen und ohne zusätzliche Kosten entstehen.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Verbesserte Mobile Apps Node.js Server SDK

Upgrade auf das neue [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) bietet viele Verbesserungen, einschließlich:

- Ausgehend von der [Express Framework](http://expressjs.com/en/index.html), das neue Knoten SDK ist leicht und mit neuen Knoten Versionen beibehalten, wie sie stammen erleichtern. Sie können das Verhalten der Anwendung mit Express Middleware anpassen.

- Signifikante leistungsverbesserungen im Vergleich zu den Mobile Dienste SDK.

- Sie können nun eine Website zusammen mit Ihrem mobilen Back-End-hosten; auf ähnliche Weise ganz einfach die Azure Mobile SDK alle vorhandenen express.v4 Anwendung hinzu.

- Für die Entwicklung von Plattformen und lokale erstellt, kann lokal auf Windows, Linux und OSX Plattformen das Mobile-Apps SDK entwickelt und ausgeführt werden. Es ist jetzt benutzerfreundliche allgemeine Knoten Entwicklungstechniken wie Ausführen von [Mocha](https://mochajs.org/) Tests vor der Bereitstellung.

## <a name="a-nameoverviewabasic-upgrade-overview"></a><a name="overview"></a>Grundlegende Update (Übersicht)

Zur Unterstützung bei der Aktualisierung einer Node.js Back-End-weist Azure-App-Verwaltungsdienst ein Paket Kompatibilität bereitgestellt.  Nach Aktualisierung haben Sie eine Niew-Website, die auf einer neuen Website der App-Dienst bereitgestellt werden können.

Die Mobile Services Client SDKs sind **nicht** kompatibel mit dem neuen Mobile-Apps Server SDK. Um kontinuierlichen Service für Ihre app bereitstellen zu können, sollten die Änderungen zu einer Website stellt aktuell veröffentlichte Clients nicht veröffentlicht werden. In diesem Fall sollten Sie eine neue mobile-app erstellen, die als ein Duplikat dient. Dieser Anwendung setzen Sie auf der gleichen App Serviceplan zu vermeiden Sie zusätzliche Kosten anfallen.

Sie müssen zwei Versionen der Anwendung: eine, die die Ressourceneinheiten und fungiert apps in der Natur, und die Sie dann aktualisieren können andere und Zielliste mit einer neuen Client-Version veröffentlicht. Sie können verschieben und Testen Sie den Code in Ihrem Tempo, aber stellen Sie sicher, dass alle Updates, die Sie vornehmen auf beide angewendet. Nachdem Sie den Eindruck, dass die gewünschte Anzahl von Clients, die in der Natur apps auf die neueste Version aktualisiert haben, Sie die ursprüngliche migrierte app löschen können, wenn Sie wünschen. Es wird keine weiteren monetären Kosten entstehen, wenn in der gleichen App Serviceplan als Ihre Mobile-App gehostet.

Die vollständige Gliederung für den Upgradevorgang sieht wie folgt aus:

1. Laden Sie Ihre vorhandene (migrierte) Azure Mobile Service ein.
2. Konvertieren Sie das Projekt in das Paket Kompatibilität mit einer Azure Mobile-App.
3. Korrigieren Sie alle Unterschiede (z. B. Authentifizierungseinstellungen).
4. Bereitstellen des konvertierten Azure Mobile-App-Projekts einen neuen App-Dienst.
4. Lassen Sie eine neue Version von der Clientanwendung, die die neue Mobile-App zu verwenden.
5. (Optional) Löschen der ursprünglichen migrierte mobile Service-app.

Löschvorgang kann auftreten, wenn die Datenverkehr Ihrer ursprünglichen migrierte mobilen Dienst angezeigt wird.

## <a name="a-nameinstall-npm-packagea-install-the-pre-requisites"></a><a name="install-npm-package"></a>Installieren Sie die erforderlichen Komponenten

Sie sollten [Knoten] auf Ihrem lokalen Computer installieren.  Sie sollten auch das Compatibility-Paket installieren.  Nach der Installation von Knoten können Sie eine neue Cmd oder PowerShell-Eingabeaufforderung den folgenden Befehl ausführen:

```npm i -g azure-mobile-apps-compatibility```

## <a name="a-nameobtain-ams-scriptsa-obtain-your-azure-mobile-services-scripts"></a><a name="obtain-ams-scripts"></a>Beziehen Sie Ihre Azure Mobile Dienste Skripts

- Melden Sie sich bei der [Azure-Portal].
- Verwenden **Alle Ressourcen** oder **App Services**, finden Sie Ihrer Website für Mobile-Dienste.
- Klicken Sie auf der Website, klicken Sie auf **Extras** -> **Kudu** -> **wechseln** , um die Kudu-Website zu öffnen.
- Klicken Sie auf **Debuggen Console** -> **PowerShell** auf die Verwaltungskonsole Debuggen zu öffnen.
- Navigieren Sie zu `site/wwwroot/App_Data/config` , indem Sie auf jedes Verzeichnis wiederum
- Klicken Sie auf das Symbol "herunterladen" neben der `scripts` Directory.

Dadurch wird die Skripts in ZIP-Format herunterladen.  Erstellen Sie ein neues Verzeichnis auf dem lokalen Computer und Entpacken die `scripts.ZIP` Datei im Datenverzeichnis.  Dies erstellt einen `scripts` Directory.

## <a name="a-namescaffold-appa-scaffold-the-new-azure-mobile-apps-backend"></a><a name="scaffold-app"></a>Scaffold Back-End-neue Azure Mobile-Apps

Führen Sie den folgenden Befehl aus dem Verzeichnis mit dem Skriptverzeichnis aus:

```scaffold-mobile-app scripts out```

Dadurch wird eine scaffolded Mobile-Apps Azure Back-End-in erstellt die `out` Directory.  Zwar nicht erforderlich, es ist eine gute Idee, den `out` Verzeichnis in einer Quelle Code Repository Ihrer Wahl.

## <a name="a-namedeploy-ama-appa-deploy-your-azure-mobile-apps-backend"></a><a name="deploy-ama-app"></a>Ihre Mobile-Apps Azure Back-End-Bereitstellung

Während der Bereitstellung müssen Sie eine der folgenden Aktionen ausführen:

1. Erstellen Sie eine neue Mobile-App im [Azure-Portal]an.
2. Führen Sie die `createViews.sql` Skript in der verbundenen Datenbank.
3. Verknüpfen Sie die Datenbank, die mit Ihrem Mobile-Dienst an den neuen App-Dienst verknüpft ist.
4. Verknüpfen einer beliebigen anderen Ressourcen (z. B. Benachrichtigung Hubs) auf den neuen App-Dienst an.
5. Bringen Sie den generierten Code auf Ihrer neuen Website ein.

### <a name="create-a-new-mobile-app"></a>Erstellen Sie eine neue Mobile-App

1. Melden Sie sich bei der [Azure-Portal].

2. Klicken Sie auf **+ neue** > **Web + Mobile** > **Mobile-App**, geben Sie dann einen Namen für Ihre Mobile-App Back-End.

3. Wählen Sie für die **Ressourcengruppe**eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Zuordnung (mit demselben Namen wie Ihre app). 
 
    Sie können einer anderen App Serviceplan auswählen oder Erstellen eines neuen Kontos. Weitere Informationen zu App Services Pläne und zum Erstellen eines neuen Plans in einer anderen Preise gestuft und an der gewünschten Stelle angezeigt, finden Sie unter [Azure-App-Verwaltungsdienst Pläne detaillierter Überblick](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Für die **App-Serviceplan**ist der Plan Standard (in der [Standardansicht Ebene](https://azure.microsoft.com/pricing/details/app-service/)) aktiviert. Sie können auch einen anderen Plan oder [einen neuen erstellen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)auswählen. Der App-Serviceplan Einstellungen bestimmen [Speicherort, Features, Kosten und Berechnen von Ressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihre app zugeordnet. 

    Nachdem Sie auf den Plan entschieden, haben klicken Sie auf **Erstellen**. Dadurch wird die Mobile-App Back-End-erstellt. 


### <a name="run-createviewssql"></a>CreateViews.SQL ausführen

Die scaffolded app enthält eine Datei namens `createViews.sql`.  Dieses Skript muss der Zieldatenbank ausgeführt werden.  Die Verbindungszeichenfolge für die Zieldatenbank kann aus dem migrierte mobilen Dienst aus dem Blade **Einstellungen** unter **Verbindungszeichenfolgen**abgerufen werden.  Es ist mit dem Namen `MS_TableConnectionString`.

Sie können dieses Skript aus in SQL Server Management Studio oder Visual Studio ausführen.

### <a name="link-the-database-to-your-app-service"></a>Verknüpfen Sie die Datenbank an den App-Dienst

Verknüpfen Sie die vorhandene Datenbank zu Ihrem App-Dienst:

- Öffnen Sie Ihre App-Verwaltungsdienst im [Portal Azure].
- Wählen Sie **Alle Einstellungen** -> **Datenverbindungen**.
- Klicken Sie auf **+ Add**.
- Wählen Sie in der Dropdownliste **SQL-Datenbank**
- Klicken Sie unter **SQL-Datenbank**wählen Sie Ihre vorhandene Datenbank, und dann klicken Sie auf **auswählen**.
- Klicken Sie unter **Verbindungszeichenfolge**Geben Sie den Benutzernamen und das Kennwort für die Datenbank, und klicken Sie dann klicken Sie auf **OK**.
- Klicken Sie in das **Hinzufügen von datenverbindungen** Blade auf **OK**.

Benutzername und Kennwort können gefunden werden, indem Sie die Verbindungszeichenfolge für die Zieldatenbank in Ihrem migrierte Mobile Service anzeigen.


### <a name="set-up-authentication"></a>Einrichten von Authentifizierung

Azure Mobile-Apps können Sie Azure Active Directory, Facebook, Google, Microsoft und Twitter-Authentifizierung innerhalb des Diensts konfigurieren.  Benutzerdefinierter Authentifizierung separat entwickelt werden müssen.  Finden Sie unter [Authentifizierungskonzepte] Dokumentation und [Authentifizierung Schnellstart] Dokumentation weitere Informationen.  

## <a name="a-nameupdating-clientsaupdate-mobile-clients"></a><a name="updating-clients"></a>Aktualisieren von mobilen clients

Nachdem Sie eine Betrieb Mobile-App Back-End-haben, können Sie auf eine neue Version von der Clientanwendung arbeiten, die es verbraucht. Mobile-Apps enthält auch eine neue Version des SDKs-Clients und ähnelt dem Serverupgrade über und Sie müssen alle Verweise auf die Mobile Services SDKs vor der Neuinstallation der Mobile-Apps Versionen entfernen.

Eine über die wichtigsten Änderungen zwischen den Versionen ist, dass die Konstruktoren eine Anwendungstaste nicht mehr erforderlich ist. Sie übergeben jetzt einfach in der URL für Ihre Mobile-App. Auf den .NET-Clients, beispielsweise die `MobileServiceClient` Methode ist jetzt:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Sie können weitere Informationen zum Installieren der neuen SDKs und verwenden die neue Struktur über die folgenden Links:

- [Android Version 2.2 oder höher](app-service-mobile-android-how-to-use-client-library.md)
- [iOS-Version 3.0.0 oder höher](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) Version 2.0.0 oder höher](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova Version 2.0 oder höher](app-service-mobile-cordova-how-to-use-client-library.md)

Wenn eine Anwendung ist von Pushbenachrichtigungen verwenden, notieren Sie den Anweisungen spezielle Erfassung für jede Plattform, wie es bei einigen Änderungen es auch wurden.

Wenn Sie die neue Clientversion bereit haben, probieren Sie es aus anhand Ihres Projekts aktualisierten Server. Nach dem Überprüfen, dass er immer funktioniert, können Sie eine neue Version von Ihrer Anwendung an Benutzer freigeben. Nachdem Sie Ihre Kunden die Möglichkeit, diese Updates zu erhalten hatten, können Sie schließlich die Mobile Services-Version der app löschen. An diesem Punkt haben Sie zu einer App-Dienst Mobile-App verwenden den neuesten Mobile-Apps Server SDK vollständig aktualisiert.

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Was sind Mobile-Apps aus?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App-Verwaltungsdienst Preise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Authentifizierungskonzepte]: ../app-service/app-service-authentication-overview.md
[Authentifizierung Schnellstart]: app-service-mobile-auth.md

[Azure-Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
