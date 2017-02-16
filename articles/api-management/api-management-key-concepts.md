<properties 
    pageTitle="Grundlegende Konzepte Management-API" 
    description="Informationen Sie zu APIs, Produkten, Rollen, Gruppen und andere Konzepte API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Was ist API Management?

API Management hilft Organisationen APIs für externe, Partner und interne Entwickler zum Entsperren des Potenzials ihre Daten und Dienste zu veröffentlichen. Unternehmen sind überall suchen, um ihre Vorgänge als digitale Plattform zu erweitern, erstellen neue Kanäle, neue Kunden suchen und steuernde tieferen Engagement mit bereits vorhandenen Dateien. Verwaltung von API bietet die Core Kompetenzen um erfolgreich API Programm durch Entwicklertools Engagement, Business Einblicken, Analytics, Sicherheit und Schutz sicherzustellen.

Die folgenden Video gibt Ihnen einen Überblick Azure-API Management und erfahren, wie Sie Management-API verwenden, um Ihre API, einschließlich der Access-Steuerelements, Rate einschränken, Überwachung, Protokollierung und Antwort Zwischenspeichern mit minimalen Aufwand ihrerseits viele Funktionen hinzuzufügen.

> [AZURE.VIDEO azure-api-management-overview]

Administratoren erstellen APIs, um Management-API verwenden zu können. Jede API besteht einen oder mehrere Vorgänge aus, und jeder API ein oder mehrere Produkte hinzugefügt werden kann. Um eine API zu verwenden, können Entwickler abonnieren ein Produkt, das die API enthält, und diese Sie die-API-Operation unterliegen Verwendungsrichtlinien, die möglicherweise in Kraft.

Dieses Thema bietet einen Überblick über grundlegende Konzepte API Management.

>[AZURE.NOTE] Weitere Informationen finden Sie unter der [cloudbasierten API Management: nutzen den Power-APIs](http://j.mp/ms-apim-whitepaper) PDF-Whitepaper. Diese Whitepaper zur Einführung auf API Verwaltung von CITO Research behandelt: 
>
> - Allgemeine API Anforderungen und Probleme
>     - Entkopplung APIs und Präsentieren von Fassaden
>     - Entwickler Einstieg schnell einsatzbereit
>     - Sichern des Zugriffs
>     - Analytics und Kriterien
> - Kontrolle und Einblicke mit einer API Management-Plattform
> - Verwenden der Cloud im Vergleich mit lokalen Lösungen
> - Azure-API Management

## <a name="apis"> </a>APIs und Vorgänge

APIs sind die Grundlage für eine Instanz der API Management-Dienst. Jede API stellt eine Reihe von Vorgängen Entwickler zur Verfügung. Jede API enthält einen Verweis auf die Back-End-Dienst, der die API implementiert und seiner Zuordnung Vorgänge für Vorgänge von Back-End-Dienst implementiert. Vorgänge in API Management sind hochgradig konfigurierbare, mit Kontrolle über die URL-Zuordnung, Abfrage und Pfad Parameter, Anforderung und Antwortinhalt und Vorgang Antwort zwischenspeichern. Zins Grenzwert, Kontingente und IP-Einschränkung Richtlinien können auch am API oder einzelne Vorgang Ebene implementiert werden.

Weitere Informationen finden Sie unter [So erstellen Sie APIs][] und [zum Hinzufügen von Vorgängen zu einer API][].


## <a name="products"></a> Produkte

Produkte sind wie APIs für Entwickler dargestellt werden. Produkte-API Management enthalten einen oder mehrere APIs und mit einem Titel, Beschreibung und Nutzungsbedingungen konfiguriert sind. Produkte können **Öffnen** oder **geschützten**werden. Geschützte Produkte müssen abonniert werden, bevor sie, während Sie geöffnet verwendet werden können Produkte ohne ein Abonnement verwendet werden können. Wenn ein Produkt für die Verwendung durch Entwickler bereit ist, kann er veröffentlicht werden. Nachdem er nicht veröffentlicht wird, kann werden angezeigt (und im Falle von geschützten Produkte abonniert) von Entwicklern. Abonnement Genehmigung kann Ebene Produkt konfiguriert ist und entweder Administrator Genehmigung erforderlich, oder automatisch genehmigt werden.

Gruppen dienen zum Verwalten der Sichtbarkeit von Produkten für Entwickler. Produkte erteilen Sichtbarkeit von Gruppen und Entwickler anzeigen und abonnieren Sie den Produkten, die sichtbar sind die Gruppen, in denen sie angehören, können. 

Weitere Informationen finden Sie unter [Erstellen und Veröffentlichen eines Produkts][] und das folgende Video.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Gruppen

Gruppen dienen zum Verwalten der Sichtbarkeit von Produkten für Entwickler. Projektmanagement-API weist die folgenden unveränderlichen Systemgruppen.

-   **Administratoren** - Abonnement Azure-Administratoren sind Mitglieder dieser Gruppe. Administratoren verwalten-API Management Service Instanzen, erstellen APIs, Vorgänge und Produkte, die von Entwicklern verwendet werden.
-   **Entwickler** - authentifizierten Entwicklerportal, die Benutzer in dieser Gruppe liegen. Entwickler sind die Kunden, die mit Ihrem APIs Applications erstellen. Entwickler werden erteilt Zugriff auf das Portal Entwicklertools und Erstellen von Applications, die die Vorgänge einer API aufrufen.
-   **Gäste** - Portal Benutzer nicht authentifizierte Entwicklertools, wie z. B. Interessenten des Besuchs der Entwicklerportal einer Instanz-API Management zu dieser Gruppe gehören. Sie können erteilt werden bestimmte schreibgeschützten Zugriff, wie etwa die Möglichkeit, APIs anzeigen, aber nicht aufrufen können.

Zusätzlich zu diesen Systemgruppen können Administratoren benutzerdefinierte Gruppen oder [nutzen Sie externe Gruppen in zugeordneten Azure Active Directory-Mandanten](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group)erstellen. Benutzerdefinierte und externen Gruppen können entlang Systemgruppen in Entwickler Sichtbarkeit und Zugriff zu API Products zugewiesen verwendet werden. Beispielsweise könnten Sie erstellen eine benutzerdefinierte Gruppe für Entwickler mit einer bestimmten Partnerorganisation verbunden ist, und den sie Zugriff auf die APIs aus einem Produkt, das nur die relevanten APIs enthält. Ein Benutzer kann mehr als einer Gruppe angehören.

Weitere Informationen finden Sie unter [Erstellen und verwenden Gruppen][].

## <a name="developers"></a> Entwickler

Entwickler stellen die Benutzerkonten in einer API Management Service-Instanz dar. Entwickler erstellt oder die von Administratoren Teilnahme eingeladen werden können, oder vom [Entwicklerportal][]können anmelden. Jeder Entwickler ist ein Mitglied einer oder mehreren Gruppen, und kann sein, Produkte abonnieren, die Sichtbarkeit zu diesen Gruppen erteilen.

Wenn Entwickler ein Produkt abonnieren, werden diese den primären und sekundären Schlüssel für das Produkt erteilt. Dieser Schlüssel wird verwendet, wenn in der Produkt APIs aufrufen.

Weitere Informationen finden Sie unter [Erstellen oder Entwickler einladen][] und [So Gruppen mit Entwickler zuzuordnen][].

## <a name="policies"></a> Richtlinien

Richtlinien sind eine leistungsfähige Funktion Management-API, mit denen Herausgeber das Verhalten der API durch Konfiguration ändern können. Richtlinien sind eine Zusammenstellung von Anweisungen, die sequenziell auf die Anfrage ausgeführt werden oder einer API Antwort an. Beliebte Anweisungen einschließen Format Konvertierung von XML-JSON und Anruf Zins einschränken, um das eingehende Anrufe von einem Entwickler zu beschränken und viele andere Richtlinien zur Verfügung stehen.

Richtlinienausdrücke können als Attributwerte oder Textwerte in keines der API Informationsverwaltungsrichtlinien verwendet werden, die Richtlinie nicht anders angegeben. Einige Richtlinien, wie etwa die Richtlinien [Fluss Steuerelements](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) , und [Legen Sie die Variable](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) basieren auf Richtlinienausdrücke. Weitere Informationen finden Sie unter [Erweiterte Richtlinien](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx), und schauen Sie sich das folgende Video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Eine vollständige Liste der API Informationsverwaltungsrichtlinien finden Sie unter [Richtlinie Bezug][]. Weitere Informationen zum verwenden und Konfigurieren von Richtlinien finden Sie unter [API Informationsverwaltungsrichtlinien][]. Ein Lernprogramm zum Erstellen von ein Produkt mit Zins Grenzwert und Kontingent Richtlinien finden Sie unter [zum Erstellen und Konfigurieren von Produkt Erweiterte Einstellungen][]. Eine Demo finden Sie im folgende Video.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> -Entwicklerportal

Entwicklerportal handelt, wo Entwickler erfahren Sie mehr über Ihre Vorgänge APIs, anzeigen und Anruf können, und abonnieren Sie Produkte. Interessenten können die Entwicklerportal besuchen APIs und Vorgänge anzeigen und registrieren. Die URL für Ihre Entwicklerportal befindet sich auf dem Dashboard im klassischen Azure-Portal für Ihre API Management Service-Instanz.

Sie können das Aussehen und Verhalten Ihrer Entwicklerportal durch Hinzufügen benutzerdefinierten Inhalt, Formatvorlagen anpassen und Ihr branding anpassen.

## <a name="api-management-and-the-api-economy"></a>Projektmanagement-API und Wirtschaft API

Sehen Sie weitere Informationen zur Verwaltung von API finden Sie die folgende Präsentation aus der Microsoft Ignite 2015 Konferenz ein.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Entwicklerportal]: #developer-portal

[So erstellen Sie APIs]: api-management-howto-create-apis.md
[Zum Hinzufügen von Vorgängen zu einer API]: api-management-howto-add-operations.md
[So erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[So erstellen und Verwenden von Gruppen]: api-management-howto-create-groups.md
[So Entwickler Gruppen zuordnen]: api-management-howto-create-groups.md#associate-group-developer
[Zum Erstellen und Konfigurieren von Produkt Erweiterte Einstellungen]: api-management-howto-product-with-rules.md
[So erstellen oder Entwickler einladen]: api-management-howto-create-or-invite-developers.md
[Richtlinie Bezug]: api-management-policy-reference.md
[Informationsverwaltungsrichtlinien-API]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
