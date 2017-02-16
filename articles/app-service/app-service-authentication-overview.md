<properties
    pageTitle="Authentifizierung und Autorisierung in Azure-App-Verwaltungsdienst | Microsoft Azure"
    description="Konzeptionelle Bezug und Übersicht über die Authentifizierung / Autorisierung bereitstellen für Azure-App-Verwaltungsdienst"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Authentifizierung und Autorisierung in Azure-App-Verwaltungsdienst

## <a name="what-is-app-service-authentication--authorization"></a>Was ist von App-Authentifizierung / Autorisierung?

App-Authentifizierung / Autorisierung ist ein Feature, das bietet eine Möglichkeit für eine Anwendung Benutzer anmelden müssen, damit Sie nicht auf die app Back-End-Code zu ändern. Es stellt eine einfache Möglichkeit zum Schützen Ihrer Anwendungs und Arbeiten mit Daten pro Benutzer.

App-Dienst verwendet verbundenen Identitäten, in dem eine Drittanbieter-Identitätsanbieter speichert Konten und Benutzer authentifiziert. Die Anwendung benötigt des Anbieters Identitätsinformationen, dass die app aufweist, diese Informationen selbst gespeichert werden. App-Dienst unterstützt fünf Identitätsanbieter im Paket: Azure Active Directory, Facebook, Google, Microsoft Account und Twitter. Ihre app können beliebig viele dieser Identitätsanbieter den Benutzern mit Optionen für wie diese anmelden. Sie können zum Erweitern des integrierten Supports anderen Identitätsanbieter oder [eigene benutzerdefinierte Identität Lösung]integrieren[custom-auth].

Wenn Sie sofort beginnen möchten, finden Sie die folgenden Lernprogramme:

- [Authentifizierung für Ihre app für iOS hinzufügen] [ iOS] (oder [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]oder [Cordova])
- [Benutzerauthentifizierung für API Apps in Azure-App-Verwaltungsdienst][apia-user]
- [Erste Schritte mit Azure-App-Verwaltungsdienst - Teil 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Funktionsweise der Authentifizierung in der App-Dienst

Für die Authentifizierung mithilfe der Identitätsanbieter müssen Sie zuerst der Identitätsanbieter kennen an Ihrer Anwendung zu konfigurieren. Der Identitätsanbieter liefert danach-IDs und Kennwörter, die Sie für die App-Dienst bereitstellen. Damit ist die Vertrauensstellung abgeschlossen, damit Benutzer Assertionen, wie z. B. Authentifizierungstoken, aus der Identitätsanbieter App-Dienst überprüft werden kann.

Zum Anmelden eines Benutzers mithilfe einer der folgenden Anbieter, muss der Benutzer auf einen Endpunkt umgeleitet werden, die für den ausgewählten Anbieter in Benutzer signiert. Wenn Kunden einen Webbrowser verwenden, haben Sie die App-Verwaltungsdienst automatisch alle nicht authentifizierte Benutzer an den Endpunkt weiterleiten, der in Benutzer signiert. Andernfalls müssen Sie an Ihren Kunden die direkte `{your App Service base URL}/.auth/login/<provider>`, wobei `<provider>` ist eine der folgenden Werte: Aad, Facebook, Google, Microsoft oder Twitter. Mobile und API Szenarien werden in den Abschnitten weiter unten in diesem Artikel erläutert.

Benutzer, die Interaktion mit der Anwendung über einen Webbrowser haben ein Cookie festlegen, dass sie authentifizierten verbleiben können wie Ihrer Anwendung durchsuchen. Für die anderen Typen, z. B. Mobile, ein JSON Web Token (JWT), der präsentiert werden sollte der `X-ZUMO-AUTH` Kopfzeile an den Client ausgegeben. Der Mobile-Apps Client SDKs wird dies übernehmen. Sie können auch eine Azure Active Directory Identitätstoken oder Access Token kann direkt in enthalten sein der `Authorization` Kopfzeile als eine [Person Token](https://tools.ietf.org/html/rfc6750).

App-Dienst werden überprüfen Sie alle Cookie oder Token, die Ihrer Anwendung Benutzerauthentifizierung Probleme. Zum einschränken möchten, wer die Anwendung zugreifen können, finden Sie im Abschnitt " [Autorisierung](#authorization) " weiter unten in diesem Artikel aus.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Bei einem Anbieter SDK Mobile-Authentifizierung

Nachdem alles im Back-End konfiguriert ist, können Sie sich mit App-Dienst anmelden mobilen Clients ändern. Es gibt zwei Vorgehensweisen hier ein:

- Verwenden Sie ein SDK, die einem angegebenen Identitätsanbieter veröffentlicht Identität, und klicken Sie dann auf App-Dienst zugreifen.
- Verwenden Sie eine einzelne Zeile des Codes, sodass Mobile-Apps Client SDK sich Benutzer anmelden kann.

>[AZURE.TIP] Die meisten Applikationen sollte einen Anbieter SDK für eine konsistente Umgebung zu erhalten, wenn Benutzer sich anmelden, aktualisieren Unterstützung verwenden, sowie weitere Vorteile, die angibt, der Anbieter verwenden.

Wenn Sie einen Anbieter SDK verwenden, können Benutzer eine Benutzeroberfläche anmelden, die enger in das Betriebssystem, die die app ausgeführt wird integriert, klicken Sie auf. Dies ermöglicht Ihnen außerdem, ein Anbieter Token und einige Benutzerinformationen auf den Client, der vereinfacht viel Graph-APIs nutzen und die benutzererfahrung. Gelegentlich auf Blogs und Foren sehen Sie dies genannten als fortlaufendes"Client" oder "Client geleitet Fluss" da Code auf dem Client in Benutzer und der Client-Code signiert ist Zugriff auf ein Token Anbieter.

Nachdem ein Anbieter Token abgerufen wird, muss es für die Überprüfung App-Dienst gesendet werden. Nach der App-Dienst das Token überprüft, erstellt App-Dienst ein neues App-Dienst Token, das an den Client zurückgegeben wird. Das Mobile-Apps Client SDK weist Helper Methoden zum Verwalten von dieser Exchange sowie zum Anfügen automatisch des Tokens an alle auf der Anwendung Back-End-Anfragen. Entwickler können auch einen Verweis auf das Token Anbieter beibehalten, daher bedenkenlos.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobile-Authentifizierung ohne einen Anbieter SDK

Wenn Sie keinen Anbieter SDK einrichten möchten, können Sie das Feature Mobile-Apps von Azure-App-Dienst anmelden für Sie zulassen. Das Mobile-Apps Client SDK wird an den Anbieter Ihrer Wahl eine Webansicht zu öffnen, und melden Sie sich der Benutzer. Gelegentlich auf Blogs und Foren sehen Sie dies bezeichnet als "Fluss Server" oder "Server gerichtete Bewegung", da der Server den Prozess verwaltet, der in Benutzer anmeldet und der Client SDK empfängt nie das Token Anbieter.

Code zum Starten dieser Fluss ist im Lernprogramm Authentifizierung für jede Plattform enthalten. Am Ende des Ablaufs SDK liegt und den Client ein App-Service-Token, das Token an alle auf der Anwendung Back-End-Anfragen automatisch angefügt wird.

### <a name="service-to-service-authentication"></a>Dienst-Authentifizierung

Obwohl Sie Benutzern den Zugriff auf eine Anwendung gewähren können, können Sie auch eigene API Anrufen in einer anderen Anwendung vertrauen. Beispielsweise könnten Sie eine Web app eine API in einer anderen Web app anrufen haben. In diesem Szenario verwenden Sie Anmeldeinformationen für eine Dienstkontos statt Benutzeranmeldeinformationen, ein Token abzurufen. Ein Dienstkonto ist auch bekannt als einen *Dienst Hauptbenutzer* in Azure Active Directory-Begriffen und Authentifizierung, die solche ein Konto verwendet wird auch als ein Szenario Dienst.

>[AZURE.IMPORTANT] Da mobile-apps auf Kunden-Geräten ausgeführt werden, führen Sie Windows-Dienste _nicht_ zählen als vertrauenswürdige Applikationen und sollten sich nicht auf einen Dienst Hauptbenutzer Fluss. Sie sollten stattdessen einen Fluss Benutzer verwenden, der zuvor detailliert wurde.

Für Szenarios Dienst können App-Dienst Ihrer Anwendung mithilfe von Azure Active Directory schützen. Die Anwendung einen muss nur ein Azure Active Directory-Dienst Hauptbenutzer Autorisierungstoken bereitstellen, die abgerufen werden können, indem Sie den Kunden-ID und geheimen aus Azure Active Directory-Client. Ein Beispiel für dieses Szenario, ASP.NET API apps verwendet, wird erläutert, indem Sie das Lernprogramm [Dienst Hauptbenutzer Authentifizierung für API Apps][apia-service].

Wenn Sie die App-Authentifizierung verarbeitet ein Szenario Dienst verwenden möchten, können Sie Client-Zertifikate oder Standardauthentifizierung verwenden. Informationen zu Client-Zertifikate in Azure finden Sie unter [How To konfigurieren TLS gemeinsamen Authentication für Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informationen zur Standardauthentifizierung in ASP.NET finden Sie unter [Authentifizierung Filter im ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Dienst Kontoauthentifizierung aus einer App-Dienst Logik app zu einer API-app ist einen besonderen Fall, der in [Ihrer benutzerdefinierten API auf App-Verwaltungsdienst mit apps Logik gehostet verwenden](../app-service-logic/app-service-logic-custom-hosted-api.md)detailliert beschrieben wird.

## <a name="a-nameauthorizationahow-authorization-works-in-app-service"></a><a name="authorization"></a>Funktionsweise der Autorisierung in der App-Dienst

Sie verfügen über Vollzugriff auf die Besprechungsanfragen, die die Anwendung zugreifen können. App-Authentifizierung / Autorisierung mithilfe einer der folgenden Verhaltensweisen konfiguriert werden kann:

- Lassen Sie nur authentifizierte Anfragen zu erreichen Ihrer Anwendung zu.

    Wenn ein Browser eine anonyme Anforderung eingeht, werden App-Dienst zu einer Seite für die Identitätsanbieter umgeleitet, die Sie auswählen, sodass die Benutzer sich anmelden können. Wenn die Anforderung von einem mobilen Gerät eingeht, ist die zurückgegebene Antwort eine Antwort HTTP _401 nicht autorisiert_ .

    Mit dieser Option brauchen Sie überhaupt Schreiben von Code für die Authentifizierung in Ihrer app. Wenn Sie eine genauere Autorisierung benötigen, steht der Code Informationen über den Benutzer.

- Lassen Sie alle Anfragen erreichen Ihrer Anwendung, aber Validierung von authentifizierten Anfragen und Authentifizierungsinformationen in die HTTP-Header übergeben zu.

    Diese Option wird die Autorisierung Entscheidungen an Ihrer Anwendungscode verzögert. Es bietet mehr Flexibilität bei der Behandlung anonymer Anfragen, aber Sie haben zum Schreiben von Code.

- Alle Anfragen zum Erreichen Ihrer Anwendungs zulassen, und Authentifizierungsinformationen in den Anforderungen keine Aktion ausgeführt.

    In diesem Fall, die Authentifizierung / Autorisierungsfeature ist deaktiviert. Die Vorgänge der Authentifizierung und Autorisierung sind vollständig auf Ihrer Anwendungscode.

Die vorherigen Verhalten werden durch die Option **Aktion ausgeführt werden soll, wenn die Anforderung nicht authentifiziert ist** der Azure-Portal gesteuert. Wenn Sie * *Melden Sie sich in mit *Anbieternamen* wählen **, alle Anfragen authentifiziert werden müssen.** Anforderung (keine Aktion) zulassen ** verzögert die Entscheidung über die Genehmigung zum Code, aber bietet dennoch Authentifizierungsinformationen. Wenn Sie Ihren Code alles verarbeitet haben möchten, können Sie die Authentifizierung deaktivieren / Autorisierungsfeature.

## <a name="working-with-user-identities-in-your-application"></a>Arbeiten mit Benutzeridentitäten in Ihrer Anwendung

App-Dienst übergibt einige Benutzerinformationen an Ihrer Anwendung mithilfe von speziellen Überschriften. Externe Anfragen verbieten diese Überschriften und wird nur präsentieren If festgelegt werden durch die App-Dienstauthentifizierung / Autorisierung. Einige Beispiel-Header umfassen:

* X-MS-CLIENT-TILGUNGSANTEILE-NAME
* X-MS-CLIENT-TILGUNGSANTEILE-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Code, der in einer beliebigen Sprache oder Framework geschrieben wurde, kann die erforderlichen Informationen aus diese Header abrufen. Für ASP.NET 4.6 apps wird der **ClaimsPrincipal** automatisch mit den entsprechenden Werten festgelegt.

Ihrer Anwendung kann auch zusätzliche Benutzerdetails durch ein HTTP GET abrufen, klicken Sie auf die `/.auth/me` Endpunkt Ihrer Anwendung. Ein gültiges Token, das mit der Anforderung eingeschlossen ist gibt eine JSON-Nutzlast mit Details zu den Anbieter, der verwendet wird, die zugrunde liegenden Anbieter Token und einige andere Benutzerinformationen zurück. Der Mobile-Apps Server SDKs bieten Ihnen Helper Methoden für die Arbeit mit diesen Daten. Weitere Informationen finden Sie unter [So verwenden Sie die Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)und [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentation und zusätzliche Ressourcen

### <a name="identity-providers"></a>Identitätsanbieter
Die folgenden Lernprogramme anzeigen zum Konfigurieren der App-Verwaltungsdienst, um andere Authentifizierungsanbieter verwenden:

- [So konfigurieren Sie Ihre app zum Azure-Active Directory-Konto verwenden][AAD]
- [So konfigurieren Sie Ihre app, um die Facebook-Login verwenden][Facebook]
- [So konfigurieren Sie Ihre app zum Verwenden von Google-Anmeldung][Google]
- [So konfigurieren Sie Ihre app, um Microsoft Account Login verwenden][MSA]
- [So konfigurieren Sie Ihre app, um Twitter Login verwenden][Twitter]

Wenn Sie hier eine Identität System anderen als den bereitgestellten verwenden möchten, können Sie auch die [Unterstützung von benutzerdefinierten Authentifizierung auf dem Server Mobile Apps .NET SDK Vorschau][custom-auth], die im Web apps, mobilen apps oder API apps verwendet werden kann.

### <a name="web-applications"></a>Webanwendungen
Die folgenden Lernprogramme zeigen, wie eine Webanwendung Authentifizierung hinzu:

- [Erste Schritte mit Azure-App-Verwaltungsdienst - Teil 2][web-getstarted]

### <a name="mobile-applications"></a>Windows-Dienste
Die folgenden Lernprogramme anzeigen, wie Ihre mobile Clients mithilfe des Ablaufs umgeleitet, Server Authentifizierung hinzugefügt:

- [Authentifizierung für Ihre app für iOS hinzufügen][iOS]
- [Hinzufügen von Authentifizierung bei Ihrem Android-app] [Android]
- [Fügen Sie Ihre app für Windows-Authentifizierung] [Windows]
- [Hinzufügen von Authentifizierung zu Ihrer Anwendung Xamarin.iOS] [Xamarin.iOS]
- [Hinzufügen von Authentifizierung zu Ihrer Anwendung Xamarin.Android] [Xamarin.Android]
- [Hinzufügen von Authentifizierung zu Ihrer Anwendung Xamarin.Forms] [Xamarin.Forms]
- [Authentifizierung zu Ihrer Anwendung Cordova hinzufügen] [Cordova]

Client geleitet illustrieren für Azure Active Directory verwendet werden soll, verwenden Sie die folgenden Ressourcen:

- [Verwenden der Active Directory-Authentifizierungsbibliothek für iOS][ADAL-iOS]
- [Verwenden Sie die Bibliothek Active Directory-Authentifizierung für Android][ADAL-Android]
- [Verwenden Sie die Bibliothek Active Directory-Authentifizierung für Windows und Xamarin][ADAL-dotnet]

Client geleitet illustrieren für Facebook verwendet werden soll, verwenden Sie die folgenden Ressourcen:

- [Verwenden Sie das Facebook SDK für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Client geleitet illustrieren für Twitter verwendet werden soll, verwenden Sie die folgenden Ressourcen:

- [Verwenden von Twitter-Fabric für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Client geleitet illustrieren für Google verwendet werden soll, verwenden Sie die folgenden Ressourcen:

- [Verwenden Sie die Google Anmeldung SDK für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API Applikationen
Die folgenden Lernprogramme zeigen, wie Ihre apps API schützen:

- [Benutzerauthentifizierung für API Apps in Azure-App-Verwaltungsdienst][apia-user]
- [Hauptbenutzer Service-Authentifizierung für API Apps in Azure-App-Verwaltungsdienst][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
