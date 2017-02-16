<properties
   pageTitle="Grundlegendes zu den implizit OAuth2 Fluss in Azure Active Directory erteilen | Microsoft Azure"
   description="Weitere Informationen zu Azure Active Directory-Implementierung von der implizit OAuth2 erteilen Fluss, erfahren Sie, und gibt an, ob es für eine Anwendung richtig ist."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Grundlegendes zu den OAuth2 implizit erteilen Fluss in Azure Active Directory (AD)

Die OAuth2 implizit erteilen ist oftmals der erteilen, mit der längsten Liste der in der Spezifikation OAuth2 Sicherheitsgründen wird empfohlen. Und noch, dies ist der Ansatz von ADAL JS und das Element, das es empfiehlt sich beim Erstellen von Applications gesicherte KENNWORTAUTHENTIFIZIERUNG implementiert. Was bietet? Es ist, dass alle wenigen Kompromisse: und wie sich herausstellt, ist die implizit erteilen am besten, die Sie für Applikationen, die eine Web-API über JavaScript über einen Browser beanspruchen verfolgen können.

## <a name="what-is-the-oauth2-implicit-grant"></a>Was ist die OAuth2 implizit erteilen?

Das ultimative [OAuth2 Autorisierungscode erteilen](https://tools.ietf.org/html/rfc6749#section-1.3.1) , ist die Autorisierung erteilen die zwei separate Endpunkte verwendet. Der Endpunkt Autorisierung der Benutzer Interaktion Phase verwendet, für die eine Autorisierungscode ergibt. Der Endpunkt token wird dann durch den Client für den Austausch von den Code für eine Access-Token und häufig ein aktualisieren-Token verwendet. Webanwendungen sind erforderlich, um die eigene Anwendungsanmeldeinformationen mit dem token-Endpunkt präsentieren, sodass der Autorisierung Server Authentifizieren des Clients kann.

Die [OAuth2 implizit erteilen](https://tools.ietf.org/html/rfc6749#section-1.3.2) ist eine Variante Client erhalten Sie eine Access-Token (und Id_token, wenn [OpenId verbinden](http://openid.net/specs/openid-connect-core-1_0.html)) kann direkt aus den Endpunkt Autorisierung, ohne den Endpunkt token-Supportteam noch Authentifizieren der Clientanwendung. Diese Variante wurde speziell entwickelt für JavaScript-Anwendung basierte, die in einem Webbrowser ausgeführt: in der ursprünglichen OAuth2 Spezifikation Token in einem URI-Fragment zurückgegeben werden. Die bereitgestellt, die token Bits an den JavaScript-Code im Client, aber es wird sichergestellt, dass sie auf dem Server leitet eingefügt werden wird nicht. Zurückgeben von Token über den Browser leitet direkt aus dem Endpunkt Autorisierung. Sie hat auch den Vorteil beseitigen alle Anforderungen für cross Origin-Anrufe, wenn die Anwendung JavaScript erforderlich ist, wenden Sie sich an den Endpunkt token sind erforderlich.

Eine wichtige Eigenschaft von der OAuth2 implizit erteilen besteht darin, dass beispielsweise nie Absenderadresse aktualisieren Token an den Kunden fließt. Wie wir im nächsten Abschnitt angezeigt wird, die nicht wirklich erforderlich ist und wäre tatsächlich ein Sicherheitsproblem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geeignete Szenarien für das implizit OAuth2 erteilen

Wie die OAuth2 Spezifikation selbst deklariert wird, hat der implizit erteilen entwickelt wurden, zum Aktivieren von Applications Benutzer-Agents – d. h., JavaScript-Anwendungen in einem Browser ausgeführt. Die definierende Eigenschaft solche Applications ist, dass JavaScript-Code für den Zugriff auf Serverressourcen (in der Regel ein Web-API) und aktualisieren die Anwendung UX entsprechend verwendet wird. Denken Sie einfach Anwendungen wie Google Mail oder Outlook Web Access: Wenn Sie eine Nachricht in Ihrem Posteingang auswählen, ändert sich nur der Nachricht Visualisierung Bereich zum Anzeigen der neuen Auswahl, während der Rest der Seite unverändert bleibt. Dies ist im Gegensatz zu den herkömmlichen Redirect-basierten Web apps, wenn sich jeder Interaktion mit dem Benutzer in einer vollständigen Seite Postback- und einer vollständigen Seite des Renderns von der Antwort des neuen Servers ergeben.

Programme, die die JavaScript basierend auf deren extrem vorgehen werden einzelne Seite Applications, oder SPAs genannt: die Idee ist, dass diese Anwendungen dienen nur eine HTML-Startseite und die zugehörigen JavaScript, für alle nachfolgenden Interaktionen über JavaScript ausgeführt wird von Web-API-Aufrufe gesteuert. Jedoch Hybrid Vorgehensweisen, in dem die Anwendung hauptsächlich Postback leistungsgesteuert ist jedoch nur gelegentliche JS Anrufe ausführt, sind nicht selten – die Diskussion über die Verwendung von implizit Fluss ist auch für Personen relevant.

Redirect-basierten Anwendungen secure normalerweise ihre Anfragen über Cookies, jedoch die Ansatz auch nicht für Applikationen JavaScript funktioniert. Cookies funktionieren nur für die Domäne, die, der Sie für, generiert wurden während hin zu einer anderen Domänen JavaScript Anrufe weitergeleitet werden möglicherweise, auf. Tatsächlich werden, die häufig der Fall: Stellen Sie sich von Applications aufrufen Microsoft Graph-API, Office-API Azure-API – alle befinden außerhalb der Domäne aus, in dem die Anwendung bereitgestellt wird. JavaScript-Applikationen Trend geht besteht darin, überhaupt keine Back-End-haben verlassen auf 3rd Party Web APIs Implementieren ihrer Business-Funktion (100 %).

Derzeit ist die bevorzugte Methode zum Schutz der Anrufe an eine Web-API Ansatz token Person OAuth2 verwenden, in dem jeder Anruf von einem OAuth2 Access Token begleitet. Die Web-API untersucht das eingehende Access Token und findet sie darin die notwendigen Bereiche, er gewährt Zugriff auf den Vorgang. Implizite illustrieren bietet eine geeignete Methode für JavaScript Applikationen Access Token für eine Web-API abzurufenden zahlreiche Vorteile in Bezug auf Cookies:

- Token können zuverlässig müssen keine cross Origin Anrufe – obligatorisch Registrierung von die Umleitung abgerufen werden, zu dem Token zurückgegeben werden, URI ist sichergestellt, dass Token nicht ersetzt werden
- JavaScript-Applikationen können beliebig viele Access Token wie für viele Web-APIs sie anzupassen benötigten – ohne Einschränkung auf Domänen, Abrufen
- HTML5-Features wie Sitzung oder lokaler Speicher erteilen Vollzugriff auf das Zwischenspeichern von token und Verwaltung der Lebensdauer aus, während Cookies Management bei der app undurchsichtig ist
- Access Token werden nicht ausgesetzt websiteübergreifende Anforderungsfälschungsangriffe (CSRF)

Implizite erteilen illustrieren stellt keine Token aktualisieren, hauptsächlich aus Sicherheitsgründen aus. Ein aktualisieren Token ist nicht als spezifisch ausgelegte als Access Token, gewähren des viel mehr Power erster daher viel mehr Schäden aus, für den Fall, dass es sich offengelegt werden. Im implizit Fluss Token in der URL geleitet, daher ist das Risiko von abgefangen höher als in die Autorisierung Code erteilen.

Beachten Sie jedoch, eine JavaScript-Anwendung ein weiteres Verfahren an seinem Abgang für Access Token ohne wiederholt Aufforderung des Benutzers für Anmeldeinformationen erneuern aufweist. Die Anwendung kann einen ausgeblendeten Iframe verwenden, um neue token Anfragen gegen die Autorisierung Endpunkt Azure AD-ausführen: solange im Browser immer noch eine aktive Sitzung hat (lesen: weist eine Sitzungscookie) für die Azure AD-Domäne, die Authentifizierungsanfrage erfolgreich müssen keine Interaktion mit dem Benutzer auftreten kann. 

Dieses Modell gewährt der JavaScript-Anwendung die Möglichkeit, unabhängig voneinander erneuern Access Token und sogar Erfassen von neu zugewiesenen Aufgaben für eine neue API (vorausgesetzt, dass der Benutzer zuvor für diese zugestimmt. Dadurch wird die hinzugefügte Einhaltung der beim Abrufen, verwalten und Schützen von einem Element hoher Wert wie ein Token aktualisieren verhindert. Außerhalb der Anwendung wird das Element, das sodass die automatische Erneuerung möglich ist, ist, das Cookie Azure AD-Sitzung verwaltet. Ein weiterer Vorteil von diesem Ansatz ist, dass ein Benutzer aus dem Azure Active Directory verhindert, die signiert in Azure AD, in eine der Registerkarten Browser ausgeführt Applications abmelden kann. Das Ergebnis ist das Löschen des Cookies Sitzung Azure AD- und die JavaScript-Anwendung wird automatisch die Möglichkeit, für den Benutzer, signierten Token erneuern gesperrt.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Eignet sich die implizit erteilen für meine app?

Die implizit erteilen bietet weitere Risiken als andere gewährt. Die Bereiche, die Sie benötigen besonderem Interesse sind gut dokumentierten (Siehe beispielsweise [Missbrauch von Access Token Identitätswechsel Ressource Besitzer in implizit Datenfluss] [ OAuth2-Spec-Implicit-Misuse] und [OAuth 2.0 Bedrohungsmodell und zur Sicherheit][OAuth2-Threat-Model-And-Security-Implications]). Das höhere Risikoprofil ist jedoch weitgehend daran so, dass es Applikationen aktivieren, auf dem aktiven Code, von einer remote-Ressource für einen Browser verarbeiteten ausführen soll. Wenn Sie planen, eine gesicherte KENNWORTAUTHENTIFIZIERUNG Architektur haben keine Back-End-Komponenten oder eine Web-API über JavaScript aufrufen möchten, die Verwendung des Flusses implizit für token Acquisition wird empfohlen.

Ist eine Anwendung eines native Client, nicht implizit illustrieren hervorragend auf. Die Abwesenheit des Azure AD-Sitzung Cookies im Kontext einer native Client ärztlichen Ihrer Anwendung aus der bedeutet, dass eine lange kurzlebigere Sitzung zu verwalten. Was bedeutet, dass eine Anwendung fordert wiederholt den Benutzer beim Zugriffstoken für neue Ressourcen abrufen.

Wenn Sie eine Webanwendung die einer Back-End- und Verarbeitung einer API aus ihren Back-End-Code enthält entwickeln, ist der implizite Fluss auch nicht gut. Andere gewährt bieten Ihnen viel mehr Power. Beispielsweise ermöglicht es der OAuth2 Client Anmeldeinformationen erteilen Token abrufen, die die Berechtigungen für die Anwendung selbst, im Gegensatz zu Benutzerstellvertretungen zugewiesen sind. Dies bedeutet, dass der Client hat die Möglichkeit zum programmgesteuerten Zugriff auf Ressourcen gerade verwalten, wenn ein Benutzer in einer Sitzung usw. nicht aktiv aktiviert ist. Nicht nur das aber solche gewährt sehr höhere Sicherheitsgarantien. Beispielsweise Access Token nie anzuwenden, über den Browser des Benutzers, diese nicht enttäuschen in Browserverlauf gespeichert usw.. Die Clientanwendung kann auch strenge Authentifizierung ausführen, wenn Sie ein Token anfordern.

## <a name="next-steps"></a>Nächste Schritte

- Eine vollständige Liste der Ressourcen für Entwickler einschließlich Referenzinformationen für die Protokolle und OAuth2 Autorisierung erteilen Sie Zahlungen Support von Azure AD-, schlagen Sie in der [Azure AD Entwicklers Leitfaden][AAD-Developers-Guide]
- Erfahren Sie, [wie eine Anwendung mit Azure AD integrieren]  [ ACOM-How-To-Integrate] auf der Anwendung Integrationsprozess.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

