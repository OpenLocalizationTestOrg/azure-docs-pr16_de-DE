<properties
    pageTitle="Azure Benachrichtigung Hubs"
    description="Informationen Sie zum Verwenden von Pushbenachrichtigungen in Azure. In c# mithilfe der .NET API geschriebenen Codebeispielen."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure Benachrichtigung Hubs

##<a name="overview"></a>(Übersicht)

Azure Benachrichtigung Hubs bieten eine einfache verwenden, für mehrere Plattformen, skalierten Pushbenachrichtigungen Infrastruktur, die Sie zum Senden von mobile-Pushbenachrichtigungen von einem beliebigen Back-End-(in der Cloud oder lokal) zu einer beliebigen mobilen Plattform ermöglicht.

Mit Benachrichtigung Hubs können Sie problemlos zwischen Plattformen, individuellen Pushbenachrichtigungen senden abstrahiert die Details der anderen Plattform Benachrichtigungssysteme (PNS). Mit einer einzelnen API-Anruf können Sie einzelne Benutzer oder der gesamten Zielgruppe Segmente mit Millionen von Benutzern, über alle aktivierten Geräte anpassen.

Sie können die Benachrichtigung Hubs für den geschäftlichen und Szenarien verwenden. Beispiel:

- Senden Sie abgeschnitten werden News Benachrichtigungen Millionen mit niedriger Wartezeit (Benachrichtigung Hubs wird Bing Applikationen vorinstalliert auf allen Windows und Windows Phone-Geräten).
- Senden Sie Gutscheine standortbasierte an Benutzersegmente.
- Senden Sie Benutzern oder Gruppen für Applikationen Sports/Finanzen/Spiele Ereignis Benachrichtigungen.
- Benachrichtigen Sie Benutzer des Enterprise Ereignisse wie neue Nachrichten/e-Mails und sales Leads.
- Senden Sie one-einmalig-Kennwörter für kombinierte Authentifizierung erforderlich ist.



##<a name="what-are-push-notifications"></a>Was sind Pushbenachrichtigungen?

Smartphones und Tablets können "Benutzer benachrichtigen" Wenn ein Ereignis aufgetreten ist. Diese Benachrichtigungen können viele Formen annehmen.

In Windows Store und Windows Phone-Clientanwendungen, kann die Benachrichtigung in Form einer _Spruch_sein: nicht modales Fenster angezeigt wird, mit einem akustischen Signal, um eine neue Benachrichtigung anzuzeigen. Andere Arten von Benachrichtigung, die unterstützt werden gehören die _Kachel_, _unformatierten_und _Badge_ Benachrichtigungen. Weitere Informationen zu den Typen von Benachrichtigungen auf Windows-Geräten unterstützt finden Sie unter [Kacheln, Badges, und Benachrichtigungen](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Auf einem Apple iOS-Geräte benachrichtigt der Pushbenachrichtigungen entsprechend den Benutzer mit einem Dialogfeld anfordern des Benutzers anzeigen oder schließen Sie die Benachrichtigung. Klicken Sie auf **Ansicht** , öffnet die Anwendung, die die Nachricht empfängt. Weitere Informationen unter iOS Benachrichtigungen finden Sie unter [iOS Benachrichtigungen](http://go.microsoft.com/fwlink/?LinkId=615245).

Pushbenachrichtigungen Hilfe mobile Geräte frisch Informationen während verbleibende Aufwand effizient anzuzeigen. Benachrichtigungen können von Back-End-Systemen auf mobilen Geräten gesendet werden, auch wenn der entsprechende apps auf einem Gerät nicht aktiv sind. Pushbenachrichtigungen sind eine wichtige Komponente für Consumer-apps, in dem verwendeten diese app Engagement und die Verwendung zu vergrößern. Benachrichtigungen eignen sich auch zu Unternehmen, wenn die Informationen auf dem neuesten Stand Mitarbeiter Reaktionszeiten auf geschäftliche Ereignisse erhöht.

Beispiele für bestimmte mobile Engagement Szenarien sind:

1.  Aktualisieren einer Kachel auf Windows 8 oder Windows Phone mit aktuellen Finanzdaten.
2.  Warnen eines Benutzers mit einem Spruch, die die Benutzer in einer app für die Enterprise-Workflow-basierten einige Arbeitselement zugewiesen wurde.
3.  Anzeigen eines Badges mit der Anzahl der aktuellen Verkäufe von leads in einer app CRM (wie etwa Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Wie Pushbenachrichtigungen Benachrichtigungen Arbeit

Pushbenachrichtigungen geleitet bis Plattform-spezifische Infrastruktur _Plattform Benachrichtigungssysteme_ (PNS) bezeichnet. Eine PNS bietet Barebone-Funktionen (d. h., keine Unterstützung für übertragen, Personalisierung) und keine gemeinsame Schnittstelle verfügen. Um eine Benachrichtigung zu einer Windows Store-app zu senden, muss ein Entwickler beispielsweise die WNS (Windows-Benachrichtigungsdienst) wenden. Um eine Benachrichtigung zu einem iOS-Gerät zu senden, muss der gleiche Entwickler Kontakt APNS (Apple Pushbenachrichtigungen Benachrichtigungsdienst) aus, und senden Sie die Nachricht ein zweites Mal. Azure Benachrichtigung Hubs helfen können, indem Sie eine allgemeine Schnittstelle, zusammen mit anderen Features zur Unterstützung von Pushbenachrichtigungen für jede Plattform.

Auf hoher Ebene aber folgen alle Plattform Benachrichtigungssysteme demselben Muster:

1.  Die app Client Kontakte die PNS zum Abrufen von deren _behandeln_. Ziehpunkt richtet sich nach dem System. Für WNS, ist es ein URI oder "Benachrichtigung bereitgestellt." Bei APNS handelt es sich um ein Token.
2.  Die app Client speichert dieses Handle im app- _Back-End-_ zur späteren Verwendung. Für WNS ist die Back-End in der Regel einen Clouddienst an. Apple heißt das System einen _Anbieter_.
3.  Zum Senden einer Pushbenachrichtigung Kontakte die Back-End-app die PNS verwenden den Ziehpunkt eine bestimmten Client-app-Instanz werden sollen.
4.  Der PNS leitet die Benachrichtigung, um das Gerät angegeben werden, indem Sie den Ziehpunkt an.

![][0]

##<a name="the-challenges-of-push-notifications"></a>Die Herausforderung Pushbenachrichtigungen

Während diese Systeme sehr leistungsfähige sind, lassen sie weiterhin viel Arbeit in dem app-Entwickler damit auch bekannte Pushbenachrichtigungen Benachrichtigungsszenarien, beispielsweise zum übertragen oder Senden von Pushbenachrichtigungen für segmentierter Benutzer implementieren.

Pushbenachrichtigungen sind eines der am häufigsten angeforderten Features in der Cloud Services für mobile-apps. Der Grund dafür ist, dass die Infrastruktur arbeiten müssen relativ komplexe und hauptsächlich nicht im Zusammenhang mit dem Hauptfenster Geschäftslogik der app. Einige der Herausforderung beim Erstellen einer bei Bedarf Pushbenachrichtigungen Infrastruktur sind:

- **Plattform Abhängigkeit.** Um Benachrichtigungen zu Geräten auf unterschiedlichen Plattformen zu senden, müssen mehrere Schnittstellen in die Back-End codiert werden. Nicht nur die Low-Level-Details unterscheiden, sondern die Präsentation der Benachrichtigung (Kachel, Spruch oder Badge) ist ebenfalls Plattform verschieden. Diese Unterschiede können zu komplex und schwer zu verwalten Back-End-Code führen.

- **Anzahl der Dezimalstellen.** Diese Infrastruktur Skalierung hat zwei Aspekte:
    + Pro PNS Richtlinien müssen Gerät Token aktualisiert werden jedes Mal, wenn die app gestartet wird. Dies führt zu einem großen Zeitraum Datenverkehr (und Zugriff auf die Datenbank infolge), um das Gerät Token auf dem neuesten Stand zu halten. Wenn die Anzahl der Geräte (möglicherweise bis Millionen) wächst, sind die Kosten erstellen und verwalten diese Infrastruktur nonnegligible.

    + Die meisten PNSs unterstützt übertragen auf mehreren Geräten nicht. Es folgt, dass ein übertragen an Millionen von Geräten Millionen von Anrufe an die PNSs ergibt. Möglichkeit, diese Anfragen zu skalieren ist nicht trivialen, da normalerweise app-Entwickler die Gesamtwartezeit zu halten möchten. Beispielsweise das letzte Gerät erhalten Sie die Nachricht sollte nicht die Benachrichtigung erhalten 30 Minuten, nachdem die Benachrichtigungen gesendet wurde, wie bei vielen Fällen widerspricht Zweck Pushbenachrichtigungen haben.
- **Routing.** PNSs bieten eine Möglichkeit zum Senden einer Nachricht an ein Gerät. Allerdings werden in den meisten apps Benachrichtigungen Benutzer und/oder Zinsen Gruppen (beispielsweise alle Mitarbeiter eines bestimmten Kundenkontos) ausgelegt. Daher muss die Back-End-app akzeptieren, um die Benachrichtigungen an die richtige Geräte weiterleiten möchten, eine Registrierung verwalten, die Zinsen Gruppen Gerät Token zuordnet. Dieser Aufwand fügt die Gesamtzeit für Markt und Wartung Kosten der app.

##<a name="why-use-notification-hubs"></a>Gründe für die Verwendung der Benachrichtigung Hubs

Benachrichtigung Hubs Komplexität: Sie müssen nicht die sichere Pushbenachrichtigungen verwalten. Stattdessen können Sie eine Benachrichtigung Hub verwenden. Benachrichtigung Hubs verwenden Sie eine vollständige für mehrere Plattformen, skalierten Pushbenachrichtigungen Benachrichtigungsinfrastruktur und erheblich reduzieren den Pushbenachrichtigungen-spezifischen Code, der in der app Back-End-ausgeführt wird. Benachrichtigung Hubs implementieren alle die Funktionalität der Infrastruktur für Pushbenachrichtigungen. Geräte sind nur für die Registrierung von PNS Ziehpunkte verantwortlich und die Back-End-Plattform-unabhängige Nachrichten an Benutzer oder Gruppen Zinsen gesendet verantwortlich ist, wie in der folgenden Abbildung dargestellt:

![][1]


Benachrichtigung Hubs bieten eine sofort einsatzbereite Pushbenachrichtigungen Benachrichtigungsinfrastruktur folgende Vorteile:

- **Mehrere Plattformen.**
    +  Unterstützung für alle wichtigen mobilen Plattformen. Benachrichtigung Hubs können Pushbenachrichtigungen für Windows Store, iOS, Android und Windows Phone apps senden.

    +  Benachrichtigung Hubs bieten eine allgemeine Schnittstelle zum Senden von Benachrichtigungen für alle unterstützten Plattformen. Plattform-spezifische Protokolle sind nicht erforderlich. Die Back-End-app, kann in den Formaten Plattform-spezifische oder Plattform unabhängig Benachrichtigungen senden. Die Anwendung kommuniziert nur mit Benachrichtigung Hubs.

    +  Verwaltung von Gerät Ziehpunkt. Benachrichtigung Hubs unterhält Ziehpunkt Registrierung und Feedback von PNSs.

- **Funktioniert mit einem beliebigen Back-End**: Cloud oder lokal .NET, PHP, Java, Knoten, usw..

- **Anzahl der Dezimalstellen.** Benachrichtigung Hubs Skalieren auf Millionen von Geräten, ohne die müssen neu strukturieren oder Shard.


- **Umfassender Satz an Übermittlung Muster**:

    - *Übertragen*: in der Nähe Gleichzeitiges übertragen an Millionen von Geräten mit einer einzelnen API Anruf ermöglicht.

    - *Bestimmte/Multicast*: drücken, um die Kategorien, die einzelne Benutzer, einschließlich aller aktivierten Geräte; darstellt. oder breiter Gruppe; Trennen Sie beispielsweise Formularfaktoren (Tablet im Vergleich zu Smartphone).

    - *Segmentierung*: Drücken Sie, um komplexe Segment nach Kategorie Ausdrücke (z. B. Geräte in New York Sommerabenden folgen) definiert.

    Jedem Gerät, das beim Senden von deren Ziehpunkt an einen Verteiler Benachrichtigung kann angeben, dass ein oder mehrere _Kategorien_. Weitere Informationen zu [Kategorien]. Tags haben keine vorab bereitgestellt oder freigegeben werden. Kategorien bieten eine einfache Möglichkeit zum Senden von Benachrichtigungen für Benutzer oder Gruppen Zinsen. Da Kategorien einen beliebigen app-spezifische Bezeichner (z. B. Benutzer oder eine Gruppe IDs) enthalten können, wird ihre Verwendung die app-Back-End aus die Einhaltung der zum Speichern und Verwalten von Gerät Ziehpunkte Probleme freigegeben.

- **Personalisierung**: jedem Gerät kann eine oder mehrere Vorlagen pro Gerät Lokalisierung und Anpassung erzielen, ohne die Back-End-Code haben.

- **Sicherheit**: Access geheim (SAS) oder partnerverbundkontakte Authentifizierung freigegeben.

- **Rich-telemetrieprotokoll**: verfügbar im Portal und programmgesteuert.


##<a name="integration-with-app-service-mobile-apps"></a>Integration in mobilen Apps App-Dienst

Um eine nahtlose und einheitliche Darstellung für Azure-Dienste zu erleichtern, hat [App Dienst Mobile-Apps] integrierten Unterstützung für Pushbenachrichtigungen Benachrichtigung Hubs verwenden. [App-Dienst Mobile-Apps] bietet eine global verfügbar ist, hochgradig skalierbare mobilen Anwendung Entwicklungsplattform für Enterprise-Entwickler und Systemintegratoren, die eine umfangreiche Menge von Funktionen für mobile Entwickler bereitstellt.

Mobile-Apps Entwickler können die Benachrichtigung Hubs mit den folgenden Workflow nutzen:

1. Abrufen von Gerät PNS Ziehpunkt
2. Registrieren Sie Gerät und [Vorlagen] mit Benachrichtigung Hubs über geeignete Mobile Apps Client SDK Register-API
    + Beachten Sie, dass Mobile-Apps abwesend alle Kategorien auf Registrierungen aus Sicherheitsgründen entfernt. Arbeiten Sie mit Benachrichtigung Hubs aus der Back-End-direkt an Geräte Kategorien zuordnen.
3. Senden von Benachrichtigungen aus Ihrer app Back-End-mit Benachrichtigung Hubs

Hier sind einige Vorteile für Entwickler mit dieser Integration eingebracht hat:

- **Apps-Mobilclient SDKs.** Diese Multi-Plattform-SDKs bieten einfache APIs für Registrierung und sprechen Sie mit der Benachrichtigung Hub von mit der mobilen app verknüpft werden, automatisch. Entwickler müssen nicht über Benachrichtigung Hubs Anmeldeinformationen einsteigen und mit einem weiteren Dienst zusammenwirken.
    + Die SDKs kategorisieren automatisch das angegebene Gerät mit Mobile-Apps authentifizierten Benutzer-ID für Benutzerszenario Pushbenachrichtigungen aktivieren.
    + Die SDKs verwenden die Mobile Apps Installation ID automatisch als GUID zum Registrieren bei Benachrichtigung Hubs, speichern Entwickler die Probleme beim Verwalten mehrerer Service GUIDs.
    
- **Installation Modell.** Mobile-Apps funktioniert mit Benachrichtigung-Hubs neuesten Pushbenachrichtigungen Modell zur Darstellung aller Pushbenachrichtigungen Eigenschaften zugeordnet ist, mit einem Gerät in eine JSON-Installation, die mit den Diensten von Pushbenachrichtigungen Benachrichtigung richtet und ist einfach zu verwenden.

- **Flexibilität.** Entwickler können auch mit der Integration direkte Benachrichtigung Hubs direkt konzipiert immer auswählen.

- **Integrierte Lösung [Azure]-Portal.** Drücken Sie, wie eine Funktion in Mobile-Apps visuell dargestellt wird, und Entwickler leicht mit dem Hub zugeordneten Benachrichtigung über Mobile-Apps arbeiten können.



##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Benachrichtigung Hubs können finden Sie unter folgenden Themen:

+ **[Wie Kunden Benachrichtigung Hubs verwenden]**

+ **[Benachrichtigung Hubs Lernprogramme und Führungslinien]**

+ **Erste Schritte mit Benachrichtigung Hubs Lernprogramme** ([iOS], [Android], [Windows-Dienst], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Die relevanten .NET managed API Verweise für Pushbenachrichtigungen hier befinden:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Wie Kunden Benachrichtigung Hubs verwenden]: http://azure.microsoft.com/services/notification-hubs
  [Benachrichtigung Hubs Lernprogramme und Führungslinien]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Windows-Dienst]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [App-Dienst Mobile-Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [Vorlagen]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure-portal]: https://portal.azure.com
  [Kategorien]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
