<properties
    pageTitle="Aktualisierung von Mobile Dienste auf Azure App-Verwaltungsdienst"
    description="Erfahren Sie, wie Ihre Mobile Services-Anwendung zu einer App-Dienst Mobile-App Upgrades"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Aktualisieren Sie Ihre vorhandene .NET Azure Mobile Service auf App-Dienst

Mobile-App-Dienst ist eine neue Möglichkeit, die mit Microsoft Azure Windows-Dienste zu erstellen. Weitere Informationen finden Sie unter [Was Mobile-Apps werden?].

In diesem Thema beschrieben, wie eine vorhandene .NET Back-End-Anwendung von Azure Mobile-Dienste zu einer neuen App Dienst Mobile-Apps zu aktualisieren. Während Sie diese Aktualisierung durchführen, kann Ihre vorhandene Mobile Services-Anwendung weiterhin ausgeführt werden.   Wenn Sie eine Node.js Back-End-Anwendung aktualisieren müssen, lesen Sie [Upgrade Ihrer Node.js Mobile Dienste](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Wenn eine mobile Back-End-auf App-Verwaltungsdienst Azure aktualisiert wird, hat Zugriff auf alle App-Funktionen und entsprechend der [App-Dienst Preise]nicht Mobile Dienste Preise in Rechnung gestellt werden.

##<a name="migrate-vs-upgrade"></a>Migrieren von im Vergleich zu upgrade

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Es wird empfohlen, die Sie [Ausführen eine Migration](app-service-mobile-migrating-from-mobile-services.md) vor dem Wechsel durch eine Aktualisierung. Auf diese Weise können Sie setzen beide Versionen der Anwendung auf der gleichen App Dienst planen und ohne zusätzliche Kosten entstehen.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Verbesserte Mobile Apps .NET Server SDK

Upgrade auf das neue [Mobile Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) bietet folgende Vorteile:

- Größere Flexibilität auf NuGet Abhängigkeiten. Die Hostinganbieter Umgebung bietet nicht mehr eigenen Versionen von NuGet-Paketen, sodass Sie alternative kompatible Versionen verwenden können. Wenn Sie neue kritische Bugfixes oder Sicherheitsupdates zum Mobile Server SDK oder Abhängigkeiten vorhanden sind, müssen Sie Ihrem Dienst jedoch manuell aktualisieren.

- Größere Flexibilität im mobilen SDK. Sie können explizit steuern, welche Features und weitergeleitet werden eingerichtet, wie z. B. Authentifizierung, Tabelle APIs und den Endpunkt des Pushbenachrichtigungen Registrierung. Weitere Informationen finden Sie unter [dem Server .NET SDK für Mobile-Apps Azure verwenden](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Unterstützung für andere Projekttypen ASP.NET und leitet. Sie können jetzt MVC und Web-API Controller im selben Projekt wie das mobile Back-End-Projekt hosten.

- Unterstützung für neue App Service Authentifizierung-Features, die Ihnen ermöglichen, eine allgemeine Authentifizierungskonfiguration über Ihre Web und mobile-apps zu verwenden.

##<a name="a-nameoverviewabasic-upgrade-overview"></a><a name="overview"></a>Grundlegende Update (Übersicht)

In vielen Fällen werden Upgrade so einfach wie das Wechseln in den neuen Mobile-Apps Server SDK und erneut veröffentlichen von Code auf eine neue Instanz von Mobile-App. Es gibt jedoch einige Szenarien, die benötigen einige zusätzliche Konfiguration, wie z. B. erweiterte Authentifizierungsszenarien und Arbeiten mit Einzelvorgänge geplant. Jede dieser wird in den späteren Abschnitten behandelt.

>[AZURE.TIP] Es wird empfohlen, dass Sie gelesen und im weiteren Verlauf dieses Themas vollständig, verstehen bevor Sie ein Upgrade zu beginnen. Notieren Sie sich alle Features verwenden Sie die unter Hervorhebung.

Die Mobile Services Client SDKs sind **nicht** kompatibel mit dem neuen Mobile-Apps Server SDK. Um kontinuierlichen Service für Ihre app bereitstellen zu können, sollten die Änderungen zu einer Website stellt aktuell veröffentlichte Clients nicht veröffentlicht werden. In diesem Fall sollten Sie eine neue mobile-app erstellen, die als ein Duplikat dient. Dieser Anwendung setzen Sie auf der gleichen App Serviceplan zu vermeiden Sie zusätzliche Kosten anfallen.

Sie müssen zwei Versionen der Anwendung: eine, die die Ressourceneinheiten und fungiert apps in der Natur, und die Sie dann aktualisieren können andere und Zielliste mit einer neuen Client-Version veröffentlicht. Sie können verschieben und Testen Sie den Code in Ihrem Tempo, aber stellen Sie sicher, dass alle Updates, die Sie vornehmen auf beide angewendet. Nachdem Sie den Eindruck, dass die gewünschte Anzahl von Clients, die in der Natur apps auf die neueste Version aktualisiert haben, Sie die ursprüngliche migrierte app löschen können, wenn Sie wünschen.

Die vollständige Gliederung für den Upgradevorgang sieht wie folgt aus:

1. Erstellen Sie eine neue Mobile-App
2. Aktualisieren Sie das Projekt, um den neuen Server SDKs verwenden
3. Lassen Sie eine neue Version von der Clientanwendung
4. (Optional) Löschen der ursprünglichen migrierten Instanz

##<a name="a-namemobile-app-versionacreating-a-second-application-instance"></a><a name="mobile-app-version"></a>Erstellen eine zweite Anwendungsinstanz
Der erste Schritt beim Upgrade ist die Ressource Mobile-App erstellen, die die neue Version Ihrer Anwendung gehostet wird. Wenn Sie bereits einen vorhandenen mobilen Service migriert haben, werden Sie diese Version auf den gleichen Hostinganbieter Plan erstellen möchten. Öffnen Sie das [Azure-Portal] an, und navigieren Sie zu Ihrer migrierte Anwendung. Notieren Sie die App planen Service ausgeführt wird.

Erstellen Sie dann die zweite Anwendungsinstanz anhand der [Anweisungen für .NET Back-End-Erstellung](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app)ein. Wenn Sie dazu aufgefordert werden, wählen Sie wählen Sie App-Serviceplan oder "Plan Hostinganbieter" den Plan, der migrierte Anwendung.

Sie möchten wahrscheinlich dieselbe Datenbank und Benachrichtigung Hub verwenden, wie in Mobile-Dienste. Sie können diese Werte durch [Azure-Portal] zu öffnen, und Navigieren in der ursprünglichen Anwendung kopieren und dann auf **Einstellungen** > **ApplicationSettings**. Kopieren Sie im Bereich **Verbindungszeichenfolgen**, `MS_NotificationHubConnectionString` und `MS_TableConnectionString`. Navigieren Sie zu Ihrer neuen Upgrade-Website, und fügen Sie diese in alle vorhandenen Werte überschreiben. Wiederholen Sie diesen Vorgang für alle anderen Einstellungen der Anwendung Ihren Anforderungen app. Wenn einen migrierten Dienst nicht verwenden, können Sie die Verbindungszeichenfolgen und Einstellungen für die app auf der Registerkarte **Konfigurieren** des Abschnitts des [Azure klassischen Portal]Mobile Dienste lesen.

Erstellen Sie eine Kopie des Projekts ASP.NET für eine Anwendung und veröffentlichen Sie dann auf die neue Website. Verwenden eine Kopie Ihrer Clientanwendung, durch die neue URL aktualisiert, überprüfen Sie, ob alles wie erwartet funktioniert.

## <a name="updating-the-server-project"></a>Aktualisieren der Project server

Mobile-Apps bietet eine neue [Mobile App Server SDK] viele die gleiche Funktionalität wie die Laufzeit Mobile-Dienste bietet. Entfernen Sie zunächst alle Verweise auf die Mobile-Dienste Pakete. Suchen Sie in der NuGet-Paket-Manager, `WindowsAzure.MobileServices.Backend`. Die meisten apps werden finden Sie unter mehrere Pakete, einschließlich `WindowsAzure.MobileServices.Backend.Tables` und `WindowsAzure.MobileServices.Backend.Entity`. In diesem Fall, beginnen Sie mit den niedrigsten Paket in der Strukturansicht Abhängigkeit wie `Entity`, und entfernen Sie diese. Wenn Sie dazu aufgefordert werden, entfernen Sie alle abhängigen Pakete nicht. Wiederholen Sie diesen Vorgang, bis Sie entfernt haben `WindowsAzure.MobileServices.Backend` selbst.

An diesem Punkt müssen Sie ein Projekt, die nicht mehr Mobile Services SDKs verweist.

Fügen Sie als Nächstes Verweise die Mobile Apps SDK. Für dieses Upgrade, werden die meisten Entwickler herunterladen und installieren sollen die `Microsoft.Azure.Mobile.Server.Quickstart` Paket erstellen möchten, wie dies in der gesamten erforderlichen Menge extrahieren wird.

Ein paar Unterschiede zwischen der SDKs infolge Compiler-Fehler werden, aber diese leicht zu Adresse und werden in den Rest der in diesem Abschnitt behandelt.

### <a name="base-configuration"></a>Basis-Konfiguration

Klicken Sie dann können in WebApiConfig.cs ersetzen Sie:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

mit

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Wenn Sie weitere Informationen zu den neuen .NET Server SDK sowie zum Hinzufügen/Features aus der app entfernen möchten, finden Sie unter dem Thema [How to die Server SDK verwenden] .

Wenn Ihre app ist der Authentifizierungsfeatures verwenden, müssen auch eine OWIN Middleware registrieren. In diesem Fall sollten Sie den oben angegebenen Konfigurationscode in eine neue OWIN Start-Klasse verschieben.

1. Hinzufügen von NuGet-Paket `Microsoft.Owin.Host.SystemWeb` , wenn es nicht bereits in Ihrem Projekt enthalten ist.
2. Klicken Sie in Visual Studio, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** -> **Neues Element**. Wählen Sie **Web** -> **Allgemeine** -> **OWIN Start Class**.
3. Verschieben Sie den oben angegebenen Code für MobileAppConfiguration aus `WebApiConfig.Register()` zu den `Configuration()` Methode Ihrer neuen Start-Klasse.

Stellen Sie sicher, dass die `Configuration()` Methode endet mit:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Es gibt weitere Änderungen im Zusammenhang mit der Authentifizierung die im Abschnitt vollständige Authentifizierung fallen.

### <a name="working-with-data"></a>Arbeiten mit Daten

Mobile-Dienste served den Namen der mobilen Anwendung als Standardname Schema in der Einrichtung Entität Framework.

Um sicherzustellen, dass Sie dasselbe Schema wie zuvor verwenden Sie das Schema in der DbContext für eine Anwendung festlegen die folgenden verwiesen wird:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Stellen Sie sicher, dass Sie MS_MobileServiceName festlegen, wenn Sie die oben genannten tun haben. Sie können auch einen anderen Schemanamen bereitstellen, wenn eine Anwendung zuvor angepasst.

### <a name="system-properties"></a>Systemeigenschaften

#### <a name="naming"></a>Benennen

Auf dem Server Azure Mobile Dienste SDK Systemeigenschaften immer einen doppelten Unterstrich enthalten (`__`) Präfix für die Eigenschaften:

- __createdAt
- __updatedAt
- __deleted
- __version

Der Mobile Services Client SDKs haben spezielle Logik zum Analysieren von Systemeigenschaften in diesem Format an.

Systemeigenschaften nicht mehr haben ein spezielles Format Azure Mobile-Apps und die folgenden Namen haben:

- createdAt
- updatedAt
- Gelöschte
- Version

Die Mobile-Apps SDKs Kunde den Namen der neuen Eigenschaften System, sodass keine Änderungen an Clientcode erforderlich sind. Jedoch, wenn Sie direkt REST des Diensts aufrufen sollten Sie Ihre Abfragen entsprechend ändern.

#### <a name="local-store"></a>Lokalen Speicher

Die Änderungen vor, um die Namen der Systemeigenschaften, bedeutet, dass die lokale Datenbank ein offlinesynchronisierung für Mobile-Dienste nicht mit Mobile-Apps kompatibel ist. Falls möglich, sollten Sie vermeiden, Aktualisieren von Clients, die apps von Mobile Dienste für Mobile-Apps bis nach ausstehender Änderungen an den Server gesendet wurden. Klicken Sie dann sollten die neueste Version aktualisierte app einen neuen Datenbankdateinamen verwenden.

Wenn eine app Client von Mobile-Dienste zu Mobile-Apps durchgeführt wurde während offline Änderungen in der Vorgangswarteschlange ausstehen, muss die Datenbank aktualisiert werden, um den neuen Spaltennamen verwenden. Unter iOS kann dies erreicht werden mit einfachen Migration die Spaltennamen ändern. Schreiben Sie Android und der .NET verwaltete Client benutzerdefinierte SQL, um die Spalten für Ihre Daten Objekt Tabellen umbenennen.

Klicken Sie auf iOS sollten Sie Ihre Daten Core Schema für Ihre Daten-Einheiten folgt aussieht ändern. Beachten Sie, dass die Eigenschaften `createdAt`, `updatedAt` und `version` nicht mehr haben ein `ms_` Präfix:

| Attribut |  Typ   | Notiz                                                 |
|---------- |  ------ | -----------------------------------------------------|
| ID        | Zeichenfolge, die erforderlichen markiert  | Primärschlüssel remote Store         |
| createdAt | Datum    | (optional) Maps CreatedAt System-Eigenschaft         |
| updatedAt | Datum    | (optional) Maps UpdatedAt System-Eigenschaft         |
| Version   | Zeichenfolge  | (optional) verwendet, um Konflikte, Karten, Version zu erkennen |

#### <a name="querying-system-properties"></a>Abfragen von Systemeigenschaften

In Azure Mobile Dienste Systemeigenschaften werden nicht gesendet, standardmäßig, aber nur, wenn sie mit der Abfragezeichenfolge angefordert werden `__systemProperties`. Im Gegensatz dazu sind in Azure Mobile-Apps System Eigenschaften **immer aktiviert** , da sie Teil der Server SDK Objektmodell sind.

Diese Änderung wirkt sich hauptsächlich auf benutzerdefinierte Implementierung von Domain-Manager, wie z. B. Erweiterungen des `MappedEntityDomainManager`. In Mobile-Dienste, wenn ein Client nie alle Systemeigenschaften anfordert ist es möglich mit einem `MappedEntityDomainManager` , die alle Eigenschaften nicht tatsächlich zugeordnet. In Azure Mobile-Apps, werden diese nicht zugeordneten Eigenschaften jedoch einen Fehler in GET-Abfragen verursachen.

Die einfachste Möglichkeit zur Lösung des Problems ist Ihre DTOs ändern, sodass sie von erben `ITableData` anstelle von `EntityData`. Fügen Sie dann die `[NotMapped]` Attribut in die Felder, die ausgelassen werden soll.

Im folgenden definiert z. B. `TodoItem` mit keine Systemeigenschaften:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Hinweis: Wenn Sie Fehlermeldungen erhalten, klicken Sie auf `NotMapped`, fügen Sie einen Verweis auf die Assembly `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobile-Dienste enthalten einige Unterstützung für CORS, indem Sie die Lösung ASP.NET CORS umbrechen. Diese Funktion zum Umbrechen Ebene wurde entfernt dem Entwickler besser steuern möchten, zu verleihen, sodass Sie [ASP.NET CORS Unterstützung](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)direkt nutzen können.

Die wichtigsten Bereiche bei Verwendung von CORS sind, die die `eTag` und `Location` Kopfzeilen dürfen in Reihenfolge für den Client SDKs ordnungsgemäß funktioniert.

### <a name="push-notifications"></a>Pushbenachrichtigungen
Für Pushbenachrichtigungen ist das Hauptfenster Element, das Sie aus dem Server SDK herausfinden möglicherweise fehlt die PushRegistrationHandler-Klasse an. Registrierungen in Mobile-Apps etwas anders gehandhabt werden, und Mischung aus Registrierungen sind standardmäßig aktiviert. Verwalten von Kategorien kann mithilfe von benutzerdefinierten APIs erreicht werden. Wenden Sie die Anweisungen [zum Registrieren für Kategorien](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) für Weitere Informationen an.

### <a name="scheduled-jobs"></a>Geplante Aufträge
Geplante Aufträge werden nicht in Mobile-Apps integriert, sodass alle bestehenden Projekte, die Sie in Ihrem .NET Back-End-aufweisen werden einzeln aktualisiert werden müssen. Eine Möglichkeit besteht im Erstellen einer geplanten [Webauftrag] auf der Website der Mobile-App-Code. Sie könnten auch einen Controller, der Ihre Position Code einrichten und Konfigurieren der [Azure Scheduler] , um diesen Endpunkt der erwarteten Terminplan zu treffen.

### <a name="miscellaneous-changes"></a>Sonstige Änderungen
Alle ApiControllers, die von einem mobilen Client genutzt werden wird jetzt müssen die `[MobileAppController]` Attribut. Dies ist nicht mehr standardmäßig enthalten, damit Sie von anderen ApiControllers zum Wechseln von der mobilen Formatierungsprogramme nicht betroffen.

Die `ApiServices` Objekt ist nicht mehr Teil des SDK. Um Mobile-App Einstellungen zuzugreifen, können Sie Folgendes verwenden:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Protokollierung wird auf ähnliche Weise jetzt über das standard ASP.NET Spur schreiben erreicht:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="a-nameauthenticationaauthentication-considerations"></a><a name="authentication"></a>Authentifizierung Aspekte

Die Komponenten Authentifizierung Mobile Services wurden jetzt in der App-Dienst Authentifizierung/Autorisierung Feature verschoben. Sie können dies für Ihre Website aktivieren, indem Sie im Thema [Hinzufügen Authentifizierung mit Ihrem mobilen](app-service-mobile-ios-get-started-users.md) erfahren.

Für einige Anbieter, wie z. B. AAD, Facebook und Google sollten Sie die vorhandene Registrierung aus Ihrer Kopie Anwendung nutzen. Sie müssen einfach navigieren Sie zu der Identitätsanbieter Portal und Hinzufügen einer neuen Redirect-URL in der Registrierung. Klicken Sie dann konfigurieren Sie App Dienst Authentifizierung/Autorisierung mit den Client-ID und den Schlüssel ein.

### <a name="controller-action-authorization"></a>Controller Aktion Autorisierung
Alle Instanzen von der `[AuthorizeLevel(AuthorizationLevel.User)]` Attribut muss jetzt geändert werden, um die standardmäßige ASP.NET verwenden `[Authorize]` Attribut. Darüber hinaus sind Controller jetzt anonym standardmäßig, wie in anderen ASP.NET Applications.
Wenn Sie eine der anderen AuthorizeLevel Optionen, wie z. B. Administrator oder einer Anwendung, verwendet wurden Bitte beachten Sie, dass dies nicht mehr vorhanden sind. Stattdessen können Sie AuthorizationFilters für gemeinsame Kennwörter einrichten oder Konfigurieren einer AAD Dienst Tilgungsanteile, um Anrufe Dienst sicher zu aktivieren.

### <a name="getting-additional-user-information"></a>Abrufen von weiteren Benutzerinformationen

Gelangen Sie zusätzliche Benutzerinformationen, einschließlich Access Token durch die `GetAppServiceIdentityAsync()` Methode:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Wenn eine Anwendung Abhängigkeiten auf Benutzer-IDs, akzeptiert wie in einer Datenbank gespeichert werden, ist es darüber hinaus zu beachten, dass die Benutzer-IDs zwischen Mobile Dienste und der App-Dienst Mobile-Apps unterscheiden. Sie können die Mobile-Dienste-Benutzer-ID weiterhin durch abrufen. Alle untergeordneten Klassen ProviderCredentials verfügen über eine Benutzer-ID-Eigenschaft. Damit Sie den Vorgang fortsetzen aus dem Beispiel vor:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Wenn Ihre app Abhängigkeiten auf Benutzer-IDs verwendet werden, ist es wichtig, dass Sie dieselbe Form der Registrierung falls möglich mit einem Identitätsanbieter nutzen. Benutzer-IDs sind in der Regel in der Registrierung der Anwendung, die verwendet wurde, beschränkt, sodass die Einführung in einer neuen Registrierung Probleme mit übereinstimmenden Benutzer auf ihre Daten erstellt werden konnte.

### <a name="custom-authentication"></a>Benutzerdefinierte Authentifizierung

Wenn Ihre app eine Lösung für die benutzerdefinierte Authentifizierung verwendet wird, werden Sie möchten sicherstellen, dass die aktualisierte Website auf das System zugreifen kann. Folgen Sie den neuen Anweisungen für benutzerdefinierte Authentifizierung im [.NET Server SDK Übersicht] Ihre Lösung integriert werden soll. Bitte beachten Sie, dass die benutzerdefinierte Authentifizierungskomponenten immer noch in der Seitenansicht sind.

##<a name="a-nameupdating-clientsaupdating-clients"></a><a name="updating-clients"></a>Aktualisieren von clients
Nachdem Sie eine Betrieb Mobile-App Back-End-haben, können Sie auf eine neue Version von der Clientanwendung arbeiten, die es verbraucht. Mobile-Apps enthält auch eine neue Version des SDKs-Clients und ähnelt dem Serverupgrade über und Sie müssen alle Verweise auf die Mobile Services SDKs vor der Neuinstallation der Mobile-Apps Versionen entfernen.

Eine über die wichtigsten Änderungen zwischen den Versionen ist, dass die Konstruktoren eine Anwendungstaste nicht mehr erforderlich ist. Sie übergeben jetzt einfach in der URL für Ihre Mobile-App. Auf den .NET-Clients, beispielsweise die `MobileServiceClient` Methode ist jetzt:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Sie können weitere Informationen zum Installieren der neuen SDKs und verwenden die neue Struktur über die folgenden Links:

- [iOS-Version 3.0.0 oder höher](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) Version 2.0.0 oder höher](app-service-mobile-dotnet-how-to-use-client-library.md)

Wenn eine Anwendung ist von Pushbenachrichtigungen verwenden, notieren Sie den Anweisungen spezielle Erfassung für jede Plattform, wie es bei einigen Änderungen es auch wurden.

Wenn Sie die neue Clientversion bereit haben, probieren Sie es aus anhand Ihres Projekts aktualisierten Server. Nach dem Überprüfen, dass er immer funktioniert, können Sie eine neue Version von Ihrer Anwendung an Benutzer freigeben. Nachdem Sie Ihre Kunden die Möglichkeit, diese Updates zu erhalten hatten, können Sie schließlich die Mobile Services-Version der app löschen. An diesem Punkt haben Sie zu einer App-Dienst Mobile-App verwenden den neuesten Mobile-Apps Server SDK vollständig aktualisiert.

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure klassischen-portal]: https://manage.windowsazure.com/
[Was sind Mobile-Apps aus?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile-App-Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web-Position]: ../app-service-web/websites-webjobs-resources.md
[So verwenden Sie die .NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App-Verwaltungsdienst Preise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET Server SDK (Übersicht)]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
