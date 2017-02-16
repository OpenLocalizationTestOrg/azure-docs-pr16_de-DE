<properties
   pageTitle="So erstellen Sie eine Anwendung, die in jeder Azure Active Directory-Benutzer anmelden können | Microsoft Azure"
   description="Schritt für Schritt können Anweisungen zum Erstellen einer Anwendung, die in einem Benutzer von einem beliebigen Azure Active Directory Mandanten, auch bekannt als eine mit mehreren Mandanten Anwendung anmelden."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Wie sich in jeder Azure Active Directory (AD) Benutzer unter Verwendung des Anwendungsmusters mit mehreren Mandanten
Wenn Sie eine Software als Service-Anwendung zu viele Organisationen anbieten, können Sie Ihrer Anwendung anmelden-ins von einem beliebigen Azure AD-Mandanten konfigurieren.  In Azure AD heißt dies Ihrer Anwendung mit mehreren Mandanten vornehmen.  Benutzer in einer beliebigen Azure AD-Mandanten werden an Ihrer Anwendung nach damit einverstanden, verwenden Sie ihr Konto mit Ihrer Anwendung anmelden können.  

Wenn Sie eine vorhandene Anwendung, die ein eigenes Konto System verfügen, oder andere Arten von Anmelden aus anderen Cloud-Anbieter unterstützt, wird ein Azure AD-, melden Sie sich von einem beliebigen Mandanten hinzufügen so einfach wie das Registrieren der app, melden Sie sich über OAuth2, OpenID verbinden oder SAML Code hinzufügen und eine Anmelden mit Microsoft-Schaltfläche auf Ihrer Anwendung einfügen. Klicken Sie auf die Schaltfläche unten, um weitere Informationen zur branding Ihrer Anwendungs.

[! [Melden Sie auf Schaltfläche an] [AAD-Anmeldung]][AAD-App-Branding]


In diesem Artikel wird vorausgesetzt, dass Sie bereits mit der Erstellung einer einzelnen Mandanten Anwendungs für Azure AD-vertraut sind.  Wenn Sie nicht, Kopf nach [Entwicklertools Leitfaden-Homepage] sind[ AAD-Dev-Guide] , und führen Sie eine der unsere schnellen Starts!

Es gibt vier einfache Schritte für die Anwendung in eine Azure AD-Mandanten mit mehreren app zu konvertieren:

1.  Aktualisieren Sie Ihrer Anwendung Registrierung benutzerspezifisch mit mehreren Mandanten
2.  Aktualisieren Sie den Code zum Senden von Anfragen an die/Common Endpunkt 
3.  Aktualisieren Sie den Code, um mehrere Herausgeber Werte zu behandeln.
4.  Grundlegendes zu Benutzer- und Administrator Zustimmung und nehmen entsprechende Code vor

Betrachten Sie jeden Schritt im Detail. Sie können auch direkt in [dieser Liste mit mehreren Mandanten Beispiele]springen[AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Aktualisieren der Registrierung werden mehrere Mandanten
Standardmäßig sind Web app-API Registrierungen in Azure AD-einzelnen Mandanten.  Sie können Ihre Anmeldung mit mehreren Mandanten machen, indem Sie den Schalter "Anwendung wird mit mehreren Mandanten" auf der Konfigurationsseite Ihrer Anwendung Registrierung im [Azure klassischen Portal] suchen[ AZURE-classic-portal] und das Festlegen auf "Ja".

Hinweis: Bevor eine Anwendung mit mehreren Mandanten gemacht werden kann, muss Azure AD der App-ID-URI der Anwendung global eindeutig sein. Der App-ID-URI ist eine Möglichkeit, die Anwendung in Protokollnachrichten identifiziert wird.  Für eine einzelne Mandanten-app ist es ausreichend für die App-ID-URI innerhalb dieser Mandanten eindeutig sein.  Für eine Anwendung mit mehreren Mandanten muss es global eindeutig sein, damit Azure AD die Anwendung über alle Mandanten zu finden.  Globale Eindeutigkeit wird erzwungen durch Anfordern der App-ID-URI ein Hostname haben, die eine Domäne überprüfte den Azure AD-Mandanten entspricht.  Angenommen, wenn der Name des Ihrem Mandanten contoso.onmicrosoft.com dann einen gültigen wurde App-ID-URI wäre `https://contoso.onmicrosoft.com/myapp`.  Wenn Sie Ihrem Mandanten eine Domäne überprüfte haben `contoso.com`, und klicken Sie dann einen gültigen App-ID-URI auch wäre `https://contoso.com/myapp`.  Festlegen der Anwendung als mit mehreren Mandanten schlägt fehl, wenn die App-ID-URI diesem Muster folgen nicht.

Native Clientregistrierungen sind standardmäßig mit mehreren Mandanten.  Sie müssen keine bezüglich einer einheitlichen vornehmen Client Anwendung Registration wird mit mehreren Mandanten.

## <a name="update-your-code-to-send-requests-to-common"></a>Aktualisieren Sie den Code zum Senden von Anfragen an/Common
In einer einzelnen Mandanten-Anwendung anmelden Anfragen an des Mandanten anmelden Endpunkt gesendet.   Für contoso.onmicrosoft.com würde beispielsweise der Endpunkt angeben:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

An einen Mandanten Endpunkt gesendete Anfragen können in Benutzer (oder Gäste) in diesem Mandanten Applications in diesem Mandanten anmelden.  Mit einer Anwendung mit mehreren Mandanten nicht die Anwendung kennen von Anfang welche Mandanten aus, der Benutzer ist, sodass Sie keine Besprechungsanfragen an einen Mandanten Endpunkt senden können.  In diesem Fall werden Anfragen an den Endpunkt gesendet, die über alle Azure AD-Mandanten multiplexes:

    https://login.microsoftonline.com/common

Wenn Azure AD erhält eine Besprechungsanfrage auf der/Common Endpunkt, den Benutzer anmeldet und daher erkennt welche Mandanten aus der Benutzer ist.  Die/gemeinsamen Endpunkt funktioniert mit allen der Authentifizierungsprotokolle von Azure AD unterstützt: OpenID verbinden, OAuth 2.0, SAML 2.0 und WS-Federation.

Die Vorzeichen als Antwort auf die Anwendung dann enthält ein Token, das den Benutzer darstellt.  Der Herausgeber der Wert im Token weist einer Anwendung welche Mandanten aus der Benutzer ist.  Wenn eine Antwort zurückgibt, aus der/Common Endpunkt, der Herausgeber Wert im Token wird Mandanten des Benutzers entsprechen.  Es ist wichtig, beachten Sie die/Common Endpunkt ist keinen Mandanten, und ist kein Herausgeber, es ist eine multiplexer.  Wenn/Common verwenden zu können, muss die Logik in Ihrer Anwendung Token überprüft aktualisiert werden, um dies zu berücksichtigen. 

Wie zuvor schon erwähnt, müssen mehrere Mandanten Applikationen auch eine konsistente Anmeldeverhalten für Benutzer, die Richtlinien branding Azure AD-Anwendung folgen bereitstellen. Klicken Sie auf die Schaltfläche unten, um weitere Informationen zur branding Ihrer Anwendungs.

[! [Melden Sie auf Schaltfläche an] [AAD-Anmeldung]][AAD-App-Branding]

Werfen Sie einen Blick auf die Verwendung von der/Common Endpunkt und der Code Implementierung ausführlicher.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Aktualisieren Sie den Code, um mehrere Herausgeber Werte zu behandeln.
Webanwendungen Web APIs erhalten und Token aus Azure Active Directory überprüfen.  

> [AZURE.NOTE] Während einer systemeigenen Clientanwendungen anfordern und Token von Azure AD erhalten, verwenden sie dazu zum wahlweise auf APIs, wo sie überprüft werden.  Systemeigene Applikationen Token nicht überprüfen und müssen als undurchsichtig behandeln.

Schauen Sie sich an, wie eine Anwendung Token überprüft vom Azure AD empfängt.  Eine einzelne Mandanten Anwendung dauert einen Endpunktwert wie gewohnt:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

und ihre Verwendung zum Erstellen einer Metadaten-URL (in diesem Fall OpenID verbinden) wie:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

kritische zweierlei Informationen herunterladen, mit denen Token überprüfen: Tasten und Herausgeber Wert, der der Mandanten signieren.  Jede Azure AD-Mandanten weist einen eindeutigen Herausgeber Wert des Formulars:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

GUID-Wert ist, in dem die umbenennen geeigneten Version der Mandanten-ID für den Mandanten.  Wenn Sie auf den Link Metadaten über für klicken Sie auf `contoso.onmicrosoft.com`, Sie können diesen Wert Herausgeber im Dokument anzeigen.

Eine einzelne Mandanten Anwendung ein Token überprüft wurde, überprüft die Signatur das Token anhand der signierenden Schlüssel aus dem Metadatendokument und sichergestellt, dass der Wert Herausgeber im Token übereinstimmt, die im Metadatendokument gefunden wurde.

Seit dem/Common Endpunkt nicht auf einen Mandanten entsprechen und nicht ist ein Herausgeber, wenn untersuchen Sie den Wert Herausgeber in den Metadaten für/allgemeine weist eine mit Vorlagen-URL anstelle der tatsächlichen Wert:

    https://sts.windows.net/{tenantid}/

Daher kann keine Anwendung mit mehreren Mandanten Token überprüft werden, indem Sie einfach in den Metadaten mit den Herausgeber-Wert entspricht der `issuer` Wert im Token.  Eine Anwendung mit mehreren Mandanten Logik entscheiden, welche Herausgeber Werte gültig sind und welche werden nicht benötigt, basierend auf den Mandanten-ID-Teil des Werts Herausgeber.  

Beispielsweise, wenn eine Anwendung mit mehreren Mandanten nur anmelden aus bestimmten Mandanten, die für den Dienst angemeldet haben zulässt, es muss aktivieren Sie dann entweder den Wert des Herausgebers oder die `tid` Wert in das Token, um sicherzustellen, dass die Mandanten wird in ihrer Liste der Abonnenten beanspruchen.  Wenn eine Anwendung mit mehreren Mandanten nur befasst sich mit Einzelpersonen und keine Access entscheiden basierend auf den Mandanten, können sie den Wert des Herausgebers ganz ignorieren.

In den Beispielen für mehrere Mandanten, die Sie im Abschnitt am Ende dieses Artikels [Verknüpften Inhalt finden,](#related-content) ist Herausgeber Überprüfung deaktiviert, um alle Azure AD-Mandanten anmelden aktivieren.

Jetzt sehen wir uns die Benutzerfunktionalität für Benutzer, die mit mehreren Mandanten Applications anmelden.

## <a name="understanding-user-and-admin-consent"></a>Grundlegendes zu Benutzer- und Administrator Zustimmung
Für einen Benutzer bei einer Anwendung in Azure AD anmelden muss die Anwendung in des Benutzers Mandanten dargestellt werden.  Dadurch wird die Organisation Schritte ausführen, wie eindeutige Richtlinien angewendet werden, wenn Benutzer von ihren Mandanten bei der Anwendung anmelden.  Für eine einzelne Mandanten Anwendung ist diese Registrierung einfach. Es ist das Objekt, das passiert, wenn Sie die Anwendung im [Azure klassischen Portal]registrieren[AZURE-classic-portal].

Für eine Anwendung mit mehreren Mandanten befindet sich die ursprüngliche Registrierung für die Anwendung in den Azure AD-Mandanten vom Entwickler verwendet.  Wenn ein Benutzer von einem anderen Mandanten mit der Anwendung zum ersten Mal anmeldet, fragt Azure AD, damit die Berechtigungen von der Anwendung angefordert Zustimmung.  Wenn sie zustimmen, woraufhin eine Darstellung der *wichtigsten Service* -Anwendung des Benutzers Mandanten erstellt wird, und Anmelden fortsetzen können. Eine Delegierung wird auch im Verzeichnis erstellt, das Einträge des Benutzers Zustimmung zur Anwendung. Finden Sie unter [Anwendung und Dienst Tilgungsanteile Objekte] [ AAD-App-SP-Objects] Weitere Informationen über die Anwendung der Anwendung und ServicePrincipal Objekte, und wie diese miteinander verknüpfen.

![Zustimmung zur app einzelne Ebene][Consent-Single-Tier] 

Diese Zustimmung Erfahrung ist durch die Berechtigungen von der Anwendung angefordert betroffen.  Azure AD unterstützt zwei Arten von app nur und delegierte Berechtigungen:

- Eine delegierte Berechtigung erteilt eine Anwendung, die die Möglichkeit, die als ein Anmeldung bei Benutzer für eine Teilmenge der Aktionen des Benutzers dienen ausführen kann.  Beispielsweise können Sie einer Anwendung delegierten berechtigt sein, lesen die Anmeldung bei Benutzer Kalender erteilen.
- Eine app nur direkt an die Identität der Anwendung Berechtigung ist.  Angenommen, Sie können einer Anwendung app nur berechtigt sein, die Liste der Benutzer in einen Mandanten Lesen erteilen, und er wird möglicherweise Aktion unabhängig davon, wer die Anwendung angemeldet ist.

Einige Berechtigungen können von einem Benutzer reguläre zugestimmt werden während andere wiederum einer mandantenadministrator Zustimmung erfordern. 

### <a name="admin-consent"></a>Zustimmung Administrator
Nur App-Berechtigungen erfordern immer eine mandantenadministrator Zustimmung.  Wenn die Anwendung, eine app nur Berechtigung anfordert und normaler Benutzer versucht, sich bei der Anwendung anmelden, erhalten eine Anwendung eine Fehlermeldung angezeigt, die besagt, dass der Benutzer nicht mehr Zustimmung ist.

Bestimmte zugewiesenen Berechtigungen erfordern auch eine mandantenadministrator Zustimmung.  Angenommen, die Möglichkeit zum Schreiben wieder Azure AD wie für der Anmeldung bei Benutzer eine mandantenadministrator Zustimmung erforderlich.  Wie nur app-Berechtigungen Wenn ein normaler Benutzer versucht, die zur Anwendung anmelden, die eine delegierte Berechtigung anfordert, die Administrator Zustimmung, erfordert wird Ihrer Anwendung eine Fehlermeldung.  Unabhängig davon, ob eine Berechtigung erforderlich ist Administrator Zustimmung vom Entwickler, der die Ressource veröffentlicht bestimmt wird, und kann in der Dokumentation für die Ressource gefunden werden kann.  Links zu Themen, in denen die verfügbaren Berechtigungen für den Azure AD Graph-API und Microsoft Graph-API sind im Abschnitt [Verknüpften Inhalt](#related-content) dieses Artikels.

Wenn die Anwendung Berechtigungen, die Administrator Zustimmung erfordern verwendet, müssen Sie haben eine Bewegung in der Anwendung beispielsweise eine Schaltfläche oder einen Link, in dem der Administrator die Aktion initiieren kann.  Die Anforderung der Anwendung gesendete für diese Aktion ist eine üblichen OAuth2/OpenID verbinden Autorisierung Anforderung, aber enthält, die auch die `prompt=admin_consent` Zeichenfolgenparameter Abfragen.  Sobald der Administrator zugestimmt hat und die Tilgungsanteile Service wird in der vom Kunden Mandanten erstellt, nachfolgende anmelden Anfragen muss nicht die `prompt=admin_consent` Parameter.   Da der Administrator, dass die angeforderten Berechtigungen zulässig sind entschieden hat, werden keine anderen Benutzern in den Mandanten für Zustimmung zu diesem Zeitpunkt aufgefordert.

Die `prompt=admin_consent` Parameter kann auch von Applications, die Berechtigungen, die keine Administrator Zustimmung erforderlich anfordern, aber möchten, geben Sie eine Stelle, an der der mandantenadministrator "anmeldet" für die Anwendung einmal verwendet und keine anderen Benutzern Aufforderung zur Zustimmung zu diesem Zeitpunkt auf.

Wenn eine Anwendung Administrator Zustimmung erfordert, und der Administrator zur Anwendung anmeldet jedoch die `prompt=admin_consent` Parameter nicht gesendet, der Administrator kann erfolgreich Zustimmung zur Anwendung, aber sie werden nur Zustimmung für ihr Konto.  Normale Benutzer werden möglicherweise trotzdem nicht anmelden und Zustimmung zur Anwendung.  Dies ist sinnvoll, wenn Sie der mandantenadministrator die Möglichkeit zum Erforschen Ihrer Anwendungs, bevor Sie anderen Benutzern Zugriff gewähren möchten.

Mandantenadministrator kann die Möglichkeit für reguläre Benutzer Applications Zustimmung deaktivieren.  Wenn diese Funktion deaktiviert ist, ist Admin Zustimmung immer erforderlich, damit die Anwendung in den Mandanten eingerichtet werden.  Wenn Testen Ihrer Anwendung mit regulärer Benutzer Zustimmung deaktiviert werden soll, finden Sie die Konfiguration wechseln in den Azure AD-Mandanten Konfigurationsabschnitt des [Azure klassischen Portal][AZURE-classic-portal].

> [AZURE.NOTE] Einige Applikationen soll eine Erfahrung, wo reguläre Benutzer können Anfangs Zustimmung und höher kann die Anwendung die Unternehmensadministrator "und" Anforderung Berechtigungen, die erfordern Administrator Zustimmung umfassen.  Es gibt keine Möglichkeit, dazu Azure AD heute mit einer einzelnen Anwendung Registrierung erforderlich.  Die bevorstehende Azure AD-Version 2-Endpunkt, anstelle von Applications Anforderung Berechtigungen zur Laufzeit zulassen, zum Zeitpunkt der Registrierung die dieses Szenario zu aktivieren.  Weitere Informationen finden Sie unter der [Azure AD-App-Modells Version 2 Developer Guide][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Zustimmung und mit mehreren Ebenen Applikationen
Die Anwendung möglicherweise mehrere Ebenen verfügen, dargestellt durch einen eigenen Registrierung in Azure AD sind.  Beispielsweise ruft eine systemeigene Anwendung, die eine Web-API ruft oder eine Webanwendung, die eine Web-API.  In beiden Fällen fordert der Client (native app oder Web app) Berechtigungen zum Aufrufen der Ressource (Web-API).  Damit der Client erfolgreich in einem Kunden Mandanten zugestimmt werden müssen alle Ressourcen, die sie Berechtigungen anfordert, bereits in der vom Kunden Mandanten vorhanden sein.  Wenn diese Bedingung nicht erfüllt, gibt Azure AD einen Fehler zurück, dass die Ressource zuerst hinzugefügt werden muss.

Dies kann ein Problem sein, wenn die logische Anwendung aus zwei oder mehr Anwendung Registrierungen, beispielsweise einen separaten Client und Ressourcen besteht.  Wie erhalte Sie die Ressource in den Kunden Mandanten ersten?  Azure AD behandelt diesem Fall durch Aktivieren von Client und Ressourcen auf die Stelle, an der der Benutzer die Gesamtsumme über die Berechtigungen von Client und Ressourcen auf der Seite Zustimmung angefordert sieht in einem Schritt zugestimmt werden.  Um dieses Verhalten zu aktivieren, muss der Ressource Anwendung Registrierung des Clients des App-ID als enthalten einen `knownClientApplications` in seinem Anwendungsmanifest.  Beispiel:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Diese Eigenschaft kann über die Ressource [Anwendungsmanifest]aktualisiert werden[AAD-App-Manifest], und wird in einem systemeigenen mit mehreren Ebenen-Client aufrufen Web API Stichprobe im Abschnitt am Ende dieses Artikels [Verknüpften Inhalt](#related-content) veranschaulicht. Im nachstehenden Diagramm bietet einen Überblick über die Zustimmung für eine app mit mehreren Ebenen:

![Zustimmung zu bekannten mit mehreren Ebenen-Client-app][Consent-Multi-Tier-Known-Client] 

Eine ähnliche Anfrage passiert, wenn die verschiedenen Ebenen der Anwendung in anderen Mandanten registriert sind.  Betrachten Sie beispielsweise die Groß-/Kleinschreibung von einer systemeigenen Clientanwendung, die die Office 365 Exchange Online-API ruft erstellen aus.  Bei der Entwicklung die systemeigenen Anwendung und höher für die ursprüngliche Anwendung auf einem Kunden Mandanten ausgeführt werden muss die Exchange Online-Dienst Tilgungsanteile vorhanden sein.  Der Kunde muss in diesem Fall für den Dienst in ihren Mandanten erstellt werden Hauptbenutzer Exchange Online erwerben.  Im Falle einer API erstellt von einer Organisation als Microsoft muss der Entwickler der API bieten eine Möglichkeit für ihre Kunden zum ihrer Anwendung in einem Kunden Mandanten, zum Beispiel eine Webseite Zustimmung, die steuert Zustimmung, die in diesem Artikel beschriebenen Verfahren verwenden.  Nachdem die Dienst Tilgungsanteile in den Mandanten erstellt wurde, kann die ursprüngliche Anwendung Token für die-API erhalten.

Im nachstehenden Diagramm bietet einen Überblick über die Zustimmung zu einer mit mehreren Ebenen app registriert in anderen Mandanten:

![Zustimmung mit mehreren Ebenen mit mehreren Teilnehmern][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Zustimmung widerrufen
Benutzer und Administratoren können Zustimmung an Ihrer Anwendung zu einem beliebigen Zeitpunkt widerrufen:

- Benutzer widerrufen des Zugriffs auf einzelne Anwendungen durch Entfernen sie ihre [Access-Systemsteuerung Applikationen] [ AAD-Access-Panel] Liste.
- Administratoren widerrufen des Zugriffs auf Anwendungen aus Azure Active Directory mithilfe des [Azure klassischen Portal]Abschnitt Management Azure AD-löschen[AZURE-classic-portal].

Wenn ein Administrator zur Anwendung für alle Benutzer in einen Mandanten einwilligt, können nicht Benutzer Access einzeln widerrufen.  Nur der Administrator Zugriff zu widerrufen, und nur für die gesamte Anwendung.

### <a name="consent-and-protocol-support"></a>Zustimmung und Protokoll-Support
Zustimmung wird in Azure AD über die OAuth, OpenID verbinden, unterstützt WS-Verbund und SAML-Protokolle.  Die Protokolle SAML und WS-Verbund bieten keine Unterstützung für die `prompt=admin_consent` Parameter, sodass Administrator Zustimmung nur über OAuth, und verbinden Sie OpenID möglich ist.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Mehrere Mandanten Applikationen und Access Token Zwischenspeichern
Mehrere Mandanten Applications erhalten auch Access Token APIs aufrufen, die durch Azure AD geschützt werden.  Ein häufiger Fehler, wenn ein Token für einen Benutzer mit/Common, Anfangs anfordern ist mit dem Active Directory Authentifizierung Bibliothek (ADAL) mit einer Anwendung mit mehreren Mandanten empfangen eine Antwort, und fordern Sie dann ein nachfolgende Token für/Common ebenfalls mit demselben Benutzer.  Da die Antwort aus Azure AD kommt nicht von einem Mandanten, / allgemeine, ADAL speichert das Token als von den Mandanten. Der nachfolgende Anruf an/Common einer Access-Token für den Benutzer abrufen Zugriffe des Eintrags im Cache, und der Benutzer wird aufgefordert, sich erneut anzumelden.  Um zu vermeiden, den Cache fehlen, sicherzustellen Sie, dass für einen Benutzer bereits Anmeldung bei nachfolgende Anrufen an den Endpunkt des Mandanten vorgenommen werden.

## <a name="related-content"></a>Siehe auch

- [Anwendungsbeispiele mit mehreren Mandanten][AAD-Samples-MT]
- [Richtlinien für Applikationen Branding][AAD-App-Branding]
- [Azure AD Developer Guide][AAD-Dev-Guide]
- [Anwendung und Dienst Tilgungsanteile Objekte][AAD-App-SP-Objects]
- [Integrieren von Applications in Azure-Active Directory][AAD-Integrating-Apps]
- [Übersicht über die Zustimmung Framework][AAD-Consent-Overview]
- [Microsoft Graph-API Berechtigung Bereiche][MSFT-Graph-AAD]
- [Azure AD-Graph-API Berechtigung Bereiche][AAD-Graph-Perm-Scopes]

Verwenden Sie im Kommentarbereich Disqus unten um Feedback geben und helfen Sie uns verfeinern und unsere Inhalte modellieren.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














