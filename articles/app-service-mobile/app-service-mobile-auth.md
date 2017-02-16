<properties
    pageTitle="Authentifizierung und Autorisierung in Azure mobilen Apps | Microsoft Azure"
    description="Konzeptionelle Bezug und Übersicht über die Authentifizierung / Autorisierung für Azure Mobile-Apps bereitstellen"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Authentifizierung und Autorisierung in Azure Mobile-Apps

## <a name="what-is-app-service-authentication--authorization"></a>Was ist von App-Authentifizierung / Autorisierung?

> [AZURE.NOTE] In diesem Thema wird migriert zu einem konsolidierten [App-Authentifizierung / Autorisierung](../app-service/app-service-authentication-overview.md) Thema, das Web und Mobile Apps-API behandelt.

App-Authentifizierung / Autorisierung ist ein Feature, das mit ohne Code Änderungen erforderlich, klicken Sie auf die app Back-End-Benutzer angemeldet werden können. Es stellt eine einfache Möglichkeit zum Schützen Ihrer Anwendungs und Arbeiten mit Daten pro Benutzer.

App-Dienst verwendet verbundenen Identitäten, in dem ein 3rd Drittanbietern **Identitätsanbieter** ("IDP") speichert Konten und Benutzer authentifiziert, und die Anwendung verwendet diese Identität anstelle ihrer eigenen. App-Dienst unterstützt fünf Identitätsanbieter im Paket: _Azure Active Directory_, _Facebook_, _Google_, _Microsoft-Konto_und _Twitter_. Sie können auch diese Unterstützung für Ihre apps erweitern, durch die Integration von einem anderen Identitätsanbieter oder eigene benutzerdefinierte Identität-Lösung.

Ihre app kann eine beliebige Anzahl von diese Identitätsanbieter genutzt werden, damit Sie Ihre Endbenutzer mit Optionen für wie bei der Anmeldung bereitstellen können.

Wenn Sie sofort beginnen möchten, finden Sie eine der folgenden Lernprogramme:

- [Authentifizierung für Ihre app für iOS hinzufügen]
- [Authentifizierung für Ihre app Xamarin.iOS hinzufügen]
- [Authentifizierung für Ihre app Xamarin.Android hinzufügen]
- [Hinzufügen von Authentifizierung zu Ihrem Windows-app]

## <a name="how-authentication-works"></a>Funktionsweise der Authentifizierung

Für die Authentifizierung mit einem der Identität müssen Sie zuerst der Identitätsanbieter kennen an Ihrer Anwendung zu konfigurieren. Der Identitätsanbieter wird dann bieten Ihnen-IDs und Kennwörter, die Sie wieder zur Anwendung bereitstellen. Dies schließt die Vertrauensstellung und App-Verwaltungsdienst auf Identitäten bereitgestellt, um es zu überprüfen.

Diese Schritte sind in den folgenden Themen aufgeführt:

- [So konfigurieren Sie Ihre app zum Azure-Active Directory-Konto verwenden]
- [So konfigurieren Sie Ihre app, um die Facebook-Login verwenden]
- [So konfigurieren Sie Ihre app zum Verwenden von Google-Anmeldung]
- [So konfigurieren Sie Ihre app, um Microsoft Account Login verwenden]
- [So konfigurieren Sie Ihre app, um Twitter Login verwenden]

Nachdem Sie alle Elemente im Back-End konfiguriert ist, können Sie Ihren Kunden bei der Anmeldung beim ändern. Es gibt zwei Vorgehensweisen hier ein:

- Verwenden eine einzelne Zeile mit Code, lassen Sie den Client SDK melden Sie sich Benutzer Mobile-Apps.
- Nutzen Sie ein SDK veröffentlicht von einer angegebenen Identitätsanbieter Identität, und klicken Sie dann auf App-Dienst zugreifen.

>[AZURE.TIP] Die meisten Applikationen sollte einen Anbieter SDK verwenden, können Sie eine weitere einheitlichen Gefühl Login Erfahrung zu gelangen und aktualisieren Support und andere anbieterspezifische Vorteile nutzen.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Funktionsweise der Authentifizierung ohne einen Anbieter SDK

Wenn Sie keinen Anbieter SDK einrichten möchten, können Sie Mobile-Apps zum Ausführen der Benutzername für Sie zulassen. Das Mobile-Apps-Client SDK wird an den Anbieter Ihrer Wahl eine Webansicht zu öffnen und Abschließen der Anmeldung. Gelegentlich auf Blogs und Foren, die angezeigt werden, dass dies bezeichnet als "Fluss Server" oder "Server gerichtete Bewegung" seit dem Server, ist der Benutzername verwalten und der Client SDK empfängt nie das Token Anbieter.

Der Code für die diese Datenfluss starten wird in der Authentifizierung Lernprogramm für jede Plattform behandelt. Am Ende des Ablaufs das Client SDK weist eine App-Dienst Token, und das Token automatisch alle Anfragen an die Back-End-zugeordnet ist.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Funktionsweise der Authentifizierung bei einem SDK-Anbieter

Arbeiten mit einem Anbieter ermöglicht SDK die Log in Erfahrung mehr eng mit der OS-Plattform interagieren, auf die app ausgeführt wird. Dies ermöglicht Ihnen außerdem, ein Anbieter Token und einige Benutzerinformationen auf den Client, der vereinfacht viel Graph-APIs nutzen und die benutzererfahrung. Gelegentlich auf Blogs und Foren sehen Sie dies als "Client illustrieren" bezeichnet oder "Client geleitet Fluss" seit Code auf dem Client wird das Login Behandlung und Client-Code hat Zugriff auf ein Token Anbieter.

Nachdem Sie ein Token Anbieter erhalten haben, muss es für die Überprüfung App-Dienst gesendet werden. Am Ende des Ablaufs das Client SDK weist eine App-Dienst Token, und das Token automatisch alle Anfragen an die Back-End-zugeordnet ist. Der Entwickler kann auch einen Verweis auf das Token Anbieter beibehalten, daher bedenkenlos.

## <a name="how-authorization-works"></a>Funktionsweise der Autorisierung

App-Authentifizierung / Autorisierung macht eine Reihe von Optionen für die **Aktion ausgeführt werden soll, wenn die Anforderung nicht authentifiziert wird**. Bevor Sie eine angegebenen Code eingeht, stehen Ihnen können App Dienst überprüfen, ob die Anforderung authentifiziert wird und If nicht, ablehnen, und versuchen Sie, dass den Benutzer anmelden, bevor Sie erneut versuchen.

Eine Möglichkeit besteht darin Anfragen umleiten zu einem der Identität nicht authentifizierte haben. Dies würde den Benutzer zu einer neuen Seite tatsächlich dauern, in einem Webbrowser. Jedoch Ihren mobile Client kann nicht auf diese Weise umgeleitet werden, und nicht authentifizierte Antworten erhalten eine Antwort HTTP _401 nicht autorisiert_ . Ausgehend hiervon wird die erste Anforderung, die Ihren Kunden macht sollten immer an den Endpunkt Login, und nehmen können Sie Anrufe an eine beliebige andere APIs. Wenn Sie versuchen, eine andere API rufen Sie vor dem anmelden, erhalten Ihren Kunden einen Fehler auf.

Wenn Sie eine genauere haben möchten, steuern, über welche Endpunkte Authentifizierung erforderlich ist, können Sie auch auswählen "keine Aktion (Anforderung zulassen)" für nicht authentifizierte Anfragen. In diesem Fall werden alle Authentifizierung Entscheidungen an Ihrer Anwendungscode zurückgestellt. Dies ermöglicht außerdem den Zugriff auf bestimmte Benutzer basierend auf benutzerdefinierte Autorisierungsregeln.

## <a name="documentation"></a>Dokumentation

Die folgenden Lernprogramme anzeigen, wie Ihre mobile Clients, die mithilfe von App-Dienst Authentifizierung hinzugefügt:

- [Authentifizierung für Ihre app für iOS hinzufügen]
- [Authentifizierung für Ihre app Xamarin.iOS hinzufügen]
- [Authentifizierung für Ihre app Xamarin.Android hinzufügen]
- [Hinzufügen von Authentifizierung zu Ihrem Windows-app]

Die folgenden Lernprogramme anzeigen zum Konfigurieren der App-Verwaltungsdienst, um andere Authentifizierungsanbieter nutzen zu können:

- [So konfigurieren Sie Ihre app zum Azure-Active Directory-Konto verwenden]
- [So konfigurieren Sie Ihre app, um die Facebook-Login verwenden]
- [So konfigurieren Sie Ihre app zum Verwenden von Google-Anmeldung]
- [So konfigurieren Sie Ihre app, um Microsoft Account Login verwenden]
- [So konfigurieren Sie Ihre app, um Twitter Login verwenden]

Wenn Sie hier eine Identität System anderen als den bereitgestellten verwenden möchten, können Sie auch die [Vorschau von benutzerdefinierten Authentifizierung Unterstützung in der Server SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth)nutzen.

[Authentifizierung für Ihre app für iOS hinzufügen]: app-service-mobile-ios-get-started-users.md
[Authentifizierung für Ihre app Xamarin.iOS hinzufügen]: app-service-mobile-xamarin-ios-get-started-users.md
[Authentifizierung für Ihre app Xamarin.Android hinzufügen]: app-service-mobile-xamarin-android-get-started-users.md
[Hinzufügen von Authentifizierung zu Ihrem Windows-app]: app-service-mobile-windows-store-dotnet-get-started-users.md

[So konfigurieren Sie Ihre app zum Azure-Active Directory-Konto verwenden]: app-service-mobile-how-to-configure-active-directory-authentication.md
[So konfigurieren Sie Ihre app, um die Facebook-Login verwenden]: app-service-mobile-how-to-configure-facebook-authentication.md
[So konfigurieren Sie Ihre app zum Verwenden von Google-Anmeldung]: app-service-mobile-how-to-configure-google-authentication.md
[So konfigurieren Sie Ihre app, um Microsoft Account Login verwenden]: app-service-mobile-how-to-configure-microsoft-authentication.md
[So konfigurieren Sie Ihre app, um Twitter Login verwenden]: app-service-mobile-how-to-configure-twitter-authentication.md
