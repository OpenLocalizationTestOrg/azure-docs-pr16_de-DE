<properties
   pageTitle="Azure-Active Directory-Entwickler-Glossar | Microsoft Azure"
   description="Eine Liste der Begriffe für häufig verwendete Azure Active Directory Developer Konzepte und Funktionen."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory Developer-Glossar
Dieser Artikel enthält Definitionen für einige der grundlegenden Azure Active Directory (AD) Entwicklertools Konzepte, die bei der Entwicklung von Azure AD-lernen hilfreich sind.

## <a name="access-token"></a>Access-token
Ein Typ von [Sicherheitstoken](#security-token) ausgestellt von einem [Server Autorisierung](#authorization-server)und von einer [Clientanwendung](#client-application) verwendet wird, um eine [geschützte Ressourcenserver](#resource-server)zugreifen zu können. In der Regel in Form von einer [JSON Web Token (JWT)][JWT], das Token enthält die Genehmigung an den Client, indem Sie der [Ressourcenbesitzer](#resource-owner)für eine angeforderte Zugriffsebene. Das Token enthält alle ergänzenden [Ansprüche](#claim) über den Betreff, aktivieren die Clientanwendung, es als eine Form von Anmeldeinformationen verwenden, wenn Sie eine bestimmte Ressource zugreifen. Dadurch auch müssen für die Ressourcenbesitzer um Anmeldeinformationen an den Kunden verfügbar zu machen.

Access Token werden manchmal als "Benutzer + App" oder "App-Only", je nach den Anmeldeinformationen, die angezeigt wird bezeichnet. Wenn beispielsweise eine Clientanwendung verwendet die:

- ["Autorisierungscode" Autorisierung erteilen](#authorization-grant), authentifiziert den Endbenutzer zuerst als Ressourcenbesitzer Delegieren der Autorisierung an den Kunden auf die Ressource zuzugreifen. Der Client authentifiziert danach beim Abrufen des Access-Tokens. Das Token kann manchmal zu genauer als "Benutzer + App" Token, verwiesen werden, wie es sowohl mit den Benutzer darstellt, der der Clientanwendung und die Anwendung berechtigt.
- ["Client-Anmeldeinformationen" Autorisierung erteilen](#authorization-grant), stellt dem Client die einzige Authentifizierung funktionsfähiges ohne Ressource-Besitzer Authentifizierung/Autorisierung, damit das Token manchmal auf als ein "App-Only" Token verwiesen werden kann.

Finden Sie unter [Azure AD Token Referenz] [ AAD-Tokens-Claims] Weitere Details.

## <a name="application-manifest"></a>Anwendungsmanifest  
Ein Feature vom [Azure klassischen Portal]bereitgestellt[AZURE-classic-portal], der die Identität Anwendungskonfiguration, dafür, aktualisieren die zugehörige [Anwendung] verwendet eine JSON Darstellung erzeugt[ AAD-Graph-App-Entity] und [ServicePrincipal] [ AAD-Graph-Sp-Entity] Einheiten. [Grundlegendes zu Azure Active Directory Anwendungsmanifest] finden Sie unter[ AAD-App-Manifest] Weitere Details.

## <a name="application-object"></a>Application-Objekt  
Wenn Sie registrieren/aktualisieren eine Anwendung in [Azure klassischen Portal][AZURE-classic-portal], im Portal erstellt/Updates sowohl ein Objekt Application und ein entsprechendes [Hauptbenutzer Dienstobjekt](#service-principal-object) für diesen Mandanten. Die Anwendung Objekt *definiert* die Anwendung der Identität Konfiguration global (über alle Mandanten, bei denen es Access ist), Bereitstellen einer Vorlage, die aus dem zugehörigen entsprechenden Dienst wichtigsten Objekte sind zur Verwendung lokal zur Laufzeit (in einem bestimmten Mandanten) *abgeleitet* .

Finden Sie unter [Anwendung und Dienst Tilgungsanteile Objekte] [ AAD-App-SP-Objects] Weitere Informationen.

## <a name="application-registration"></a>Registrierung Anwendung  
Um eine Anwendung zu integrieren und Delegieren Azure AD-Funktionen gezeigt Identität und Access-Verwaltung zu ermöglichen, muss es mit einem Azure AD- [Mandanten](#tenant)registriert sein. Wenn Sie eine Anwendung mit Azure AD registrieren, sind Sie somit eine Identität Konfiguration für eine Anwendung, womit sie Azure AD integrieren und Verwenden von Features wie:

- Robuste Management des einmaligen Anmeldens Azure AD Identitätsmanagement und [OpenID verbinden] mit[ OpenIDConnect] Protokoll Implementierung
- Vermittelten Zugriff auf [geschützte Ressourcen](#resource-server) von [Clientanwendungen](#client-application)über Azure AD-OAuth 2.0 [Autorisierung Server](#authorization-server) Implementierung
- [Zustimmung Framework](#consent) , für die Verwaltung von Clientzugriff auf geschützte Ressourcen, basierend auf Ressource Besitzer Autorisierung.

Finden Sie unter [Integration von Applications mit Azure Active Directory] [ AAD-Integrating-Apps] Weitere Details.

## <a name="authentication"></a>Authentifizierung
Der Vorgang eine Herausforderung einer Party seriösen Anmeldeinformationen für die Bereitstellung der Basis für die Erstellung eines Sicherheitsprinzipals für Identität und Access-Steuerelement verwendet werden soll. Während einer [OAuth2 Autorisierung erteilen](#authorization-grant) ist die Partei authentifizieren beispielsweise die Rolle des [Ressourcenbesitzer](#resource-owner) oder die [Clientanwendung](#client-application), je nach der verwendeten erteilen ausfüllen.

## <a name="authorization"></a>Autorisierung
Der Vorgang der Berechtigung einer authentifizierten Sicherheit Hauptbenutzer, etwas zu tun. Es gibt zwei primäre verwenden Fällen im Modell Azure AD-Programmierung:

- Während einer Fluss [OAuth2 Autorisierung zu gewähren](#authorization-grant) : Wenn Sie der [Ressourcenbesitzer](#resource-owner) mit der [Clientanwendung](#client-application)Autorisierung erteilt, gleicht des Clients Zugriff auf die Ressource Besitzers Ressourcen.
- Während der Zugriff auf Ressourcen vom Client: entsprechend der Implementierung durch die [Ressourcenserver](#resource-server)mithilfe der [beanspruchen](#claim) Werte präsentieren im [Zugriffstoken](#access-token) auf diese auf Grundlage von Access Steuerelement Entscheidungen treffen.

## <a name="authorization-code"></a>Autorisierungscode
Ein kurzen halten "token" enthalten, das eine [Clientanwendung](#client-application) von den [Endpunkt Autorisierung](#authorization-endpoint), als Teil des Ablaufs "Autorisierungscode" eine der vier OAuth2 [Autorisierung gewährt](#authorization-grant). Der Code wird zurückgegeben, mit der Clientanwendung Reaktion auf Authentifizierung für eine [Ressourcenbesitzer](#resource-owner), die angibt, dass der Ressourcenbesitzer Autorisierung Zugriff auf die angeforderten Ressourcen delegiert hat. Als Teil des Ablaufs ist der Code für eine [Access-Token](#access-token)später eingelöst.

## <a name="authorization-endpoint"></a>Autorisierung Endpunkt
Einen der Endpunkte vom [Server Autorisierung](#authorization-server), um eine [Autorisierung erteilen](#authorization-grant) während einer OAuth2 Autorisierung bereitstellen Interaktion mit den [Ressourcenbesitzer](#resource-owner) verwendet erteilen Fluss. Je nach den Autorisierung erteilen Fluss verwendet kann die tatsächliche erteilen bereitgestellten einschließlich einer [Autorisierungscode](#authorization-code) oder [Sicherheitstoken](#security-token)variieren.

Finden Sie unter der OAuth2-Spezifikation [Autorisierung Typen erteilen] [ OAuth2-AuthZ-Grant-Types] und [Autorisierung Endpunkt] [ OAuth2-AuthZ-Endpoint] Abschnitte und der [Spezifikation OpenIDConnect] [ OpenIDConnect-AuthZ-Endpoint] für weitere Details.

## <a name="authorization-grant"></a>Autorisierung erteilen
Anmeldeinformationen, die der [Ressource Besitzers](#resource-owner) [Autorisierung](#authorization) Zugriff auf deren geschützte Ressourcen, die an eine [Clientanwendung](#client-application)erteilt darstellt. Eine Clientanwendung können einen der [vier erteilen Typen von OAuth2 Autorisierung Framework definiert] [ OAuth2-AuthZ-Grant-Types] Grant, je nach Client Typ/Anforderungen zu erhalten: "Autorisierung Code erteilen", "Clientanmeldeinformationen erteilen", "implizit erteilen" und "Ressourcen Besitzer Kennwort Anmeldeinformationen erteilen". Die Anmeldeinformationen, die an den Client zurückgegeben wird entweder ein [Access Token](#access-token)oder eine [Autorisierungscode](#authorization-code) (später austauschen, für eine Access-Token), je nach Typ der Autorisierung erteilen verwendet.

## <a name="authorization-server"></a>Autorisierung server
Durch das [OAuth2 Autorisierung Framework]definierten[OAuth2-Role-Def], der Server für die Access verantwortlich Token an den [Kunden](#client-application) nach erfolgreich authentifiziert der [Ressourcenbesitzer](#resource-owner) und dem ihre Genehmigung abrufen. Eine [Clientanwendung](#client-application) interagiert mit dem Server Autorisierung zur Laufzeit über deren [Autorisierung](#authorization-endpoint) und [token](#token-endpoint) Endpunkte, gemäß der OAuth2 definiert [Autorisierung gewährt](#authorization-grant).

Im Falle von Azure AD-Anwendungsintegration Azure AD die Autorisierung Serverrolle für Applikationen Azure AD- und Microsoft-Dienst APIs, beispielsweise [Microsoft Graph-APIs]implementiert[Microsoft-Graph].

## <a name="claim"></a>anfordern
Ein [Sicherheitstoken](#security-token) enthält Ansprüche, die Assertionen zu einer Entität (z. B. einer [Clientanwendung](#client-application) oder [Ressourcenbesitzer](#resource-owner)) bieten an eine andere Einheit (beispielsweise die [Ressourcenserver](#resource-server)) an. Ansprüche sind Name/Wert-Paare, die Informationen zu den token Betreff (beispielsweise die Sicherheit der Tilgungsanteile, die vom [Server Autorisierung](#authorization-server)authentifiziert wurde) weitergeben. Die Ansprüche in einem angegebenen Token vorhanden sind mehrere Variablen, einschließlich den Typ der Token, den Typ der Anmeldeinformationen verwendet, um den Betreff, die Anwendungskonfiguration usw. authentifizieren abhängig.

Finden Sie unter [token Azure AD-Referenz] [ AAD-Tokens-Claims] Weitere Details.

## <a name="client-application"></a>Clientanwendung  
Durch das [OAuth2 Autorisierung Framework]definierten[OAuth2-Role-Def], eine Anwendung, geschützte Ressourcen Anfragen für den [Ressourcenbesitzer](#resource-owner). Der Begriff "Client" bedeutet keine bestimmte Hardware Implementierung Merkmale (z. B., ob die Anwendung auf einem Server, einen Desktop oder andere Geräte ausgeführt wird).  

Eine Clientanwendung fordert [Autorisierung](#authorization) vom Ressourcenbesitzer einer zur Teilnahme an einer Fluss [erteilen OAuth2 Autorisierung](#authorization-grant) und möglicherweise APIs/Daten im Auftrag des Besitzers der Ressource zugreifen. [Zwei Arten von Clients definiert]OAuth2 Autorisierung Framework[OAuth2-Client-Types], "Vertraulich" und "Öffentliche", basierend auf den Kundennamen Möglichkeit zum Verwalten von der Vertraulichkeit von deren Anmeldeinformationen. Applikationen können einen [WebClient (vertraulich)](#web-client) Implementieren der ausgeführt, auf einem Webserver, einem [native Client (öffentlich wird)](#native-client) installiert haben, klicken Sie auf einem Gerät oder ein [Benutzer-Agents-basierten Client (öffentlich)](#user-agent-based-client) der Browser des Geräts ausgeführt wird.

## <a name="consent"></a>Zustimmung
Der Vorgang eine [Ressourcenbesitzer](#resource-owner) Autorisierung mit einer [Clientanwendung](#client-application), die bestimmte [Berechtigungen](#permissions) Zugriff auf geschützte Ressourcen, für die Ressourcenbesitzer erteilen. Je nach den Berechtigungen, die vom Client angeforderten wird ein Administrator oder Benutzer aufgefordert, den Zugriff auf ihre Organisation/Einzelperson Daten Hilfethemas Zustimmung. Beachten Sie in einem Szenario [mit mehreren Mandanten](#multi-tenant-application) der Anwendung [Dienst Hauptbenutzer](#service-principal-object) auch in den Mandanten des Benutzers consenting angegeben wurde.

## <a name="id-token"></a>Token-ID
Eine [OpenID verbinden] [ OpenIDConnect-ID-Token] [Sicherheitstoken](#security-token) bereitgestellt durch einen [Autorisierung des Servers](#authorization-server) [Autorisierung Endpunkt](#authorization-endpoint), die [Ansprüche](#claim) in Verbindung mit der Authentifizierung des Endbenutzers [Ressourcenbesitzer](#resource-owner)Gruppenrichtlinien enthält. Wie ein Access-Token ID Token auch als ein digital signiertes [JSON Web Token (JWT)]dargestellt werden[JWT]. Im Gegensatz zu einer Access-Token ein ID-Token Ansprüche nicht für Zwecke im Zusammenhang mit dem Zugriff auf Ressourcen verwendet werden und der Zugriff auf speziell Steuerelement.

Finden Sie unter [token Azure AD-Referenz] [ AAD-Tokens-Claims] Weitere Details.

## <a name="multi-tenant-application"></a>mehrere Mandanten Anwendung
Melden Sie sich eine Klasse der [Clientanwendung](#client-application) , die ermöglicht und nach der Bereitstellung von Benutzern [Zustimmung](#consent) in einem beliebigen Azure AD- [Mandanten](#tenant), einschließlich Mandanten als die Stelle, an der der Client registriert ist. Hingegen würde Anwendung registriert als einzelnes-Mandanten, nur ermöglichen Sign-ins von Benutzerkonten in der gleichen Mandanten die bereitgestellt, in dem die Anwendung registriert ist. [Systemeigene](#native-client) Clientanwendungen sind standardmäßig mit mehreren Mandanten, während Sie [Web-](#web-client) Clientanwendungen haben die Möglichkeit, zwischen einzelner und mehrerer Mandanten auszuwählen.

Erfahren Sie, [wie sich in jeder Azure AD-Benutzer, die unter Verwendung des Anwendungsmusters mit mehreren Mandanten] [ AAD-Multi-Tenant-Overview] Weitere Details.

## <a name="native-client"></a>Native client
Eine Art von [Clientanwendung](#client-application) , systembedingt auf einem Gerät installiert wird. Da der gesamte Code auf einem Gerät ausgeführt wird, gilt einen "öffentlichen" Client aufgrund der Unfähigkeit zum Speichern von Anmeldeinformationen privat/vertraulich. Finden Sie unter [OAuth2 Clienttypen und Profile] [ OAuth2-Client-Types] Weitere Details.

## <a name="permissions"></a>Berechtigungen
Eine [Clientanwendung](#client-application) erhält Zugriff auf einen [Ressourcenserver](#resource-server) durch berechtigungsanforderungen deklarieren. Zwei Arten zur Verfügung:

- "Delegierte" Berechtigungen, die [Bereichsbasierte](#scopes) Zugriff unter anfordern delegiert Autorisierung aus der [Ressourcenbesitzer](#resource-owner)angemeldet, werden als ["scp" Ansprüche](#claim) in den Kundennamen [Access Token](#access-token), die der Ressource zur Laufzeit dargestellt.
- "Anwendung" Berechtigungen, die [Rollen-basierte](#roles) Zugang unter der Clientanwendung Anmeldeinformationen/Identität anfordern, werden für die Ressource zur Laufzeit als in den Kundennamen Access Token [Ansprüche "Rollen"](#claim) angezeigt.

Diese auch während des Prozesses [Zustimmung](#consent) , mit dem der Administrator oder Ressourcenbesitzer die Möglichkeit zum Erteilen/verweigern Clientzugriff auf Ressourcen in ihren Mandanten einbinden.

Berechtigungsanforderungen konfiguriert sind, klicken Sie auf "Applications" / "Konfigurieren" Registerkarte im [Azure klassischen Portal][AZURE-classic-portal], klicken Sie unter "Berechtigungen in anderen Programmen" nach Auswahl der gewünschten "delegierte Berechtigungen" und "Berechtigungen" (erfordert die Mitgliedschaft in der Rolle des globalen Administrators letztere). Da Anmeldeinformationen einen [öffentlichen Client](#client-application) beibehalten werden kann, kann er nur zugewiesenen Berechtigungen anfordern, während ein [vertrauliche Client](#client-application) die Möglichkeit, die Anfrage beide delegiert und Anwendungsberechtigungen verfügt. Im Client [Anwendungsobjekt](#application-object) speichert die deklarierten Berechtigungen in seiner [Eigenschaft RequiredResourceAccess][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>Ressourcenbesitzer
Durch das [OAuth2 Autorisierung Framework]definierten[OAuth2-Role-Def], eine Entität gewähren des Zugriffs auf eine geschützte Ressource werden kann. Wenn Sie der Ressourcenbesitzer um eine Person handelt, wird es als ein Endbenutzer bezeichnet. Beispielsweise, wann eine [Clientanwendung](#client-application) eines Benutzerpostfachs über die [Microsoft Graph-API]zugreifen möchte[Microsoft-Graph], es ist die Berechtigung aus der Ressourcenbesitzer des Postfachs erforderlich.

## <a name="resource-server"></a>Ressourcenserver
Durch das [OAuth2 Autorisierung Framework]definierten[OAuth2-Role-Def], ein Server, dass Hosts Ressourcen, annehmen und beantworten kann geschützt geschützte Ressource Anfragen von [Clientanwendungen](#client-application) , die eine [Zugriff auf Token](#access-token)präsentieren. Auch bekannt als ein geschützte Ressourcenserver oder Ressource Anwendung.

Ein Ressourcenserver APIs stellt zahlreiche und Zugriff auf die geschützten Ressourcen durch [Bereiche](#scopes) und [Rollen](#roles), mit dem OAuth 2.0 Autorisierung Framework erzwingt. Beispiele für die Azure AD Graph-API ermöglicht den Zugriff auf Azure AD-Mandanten Daten, und die Office 365-APIs, die Zugriff auf Daten wie e-Mail und Ihren Kalender zu ermöglichen. Diese beiden auch über die [Microsoft Graph-API]zugegriffen werden[Microsoft-Graph].  

Wie eine Clientanwendung wird Ressource Anwendungskonfiguration Identität über [Registration wird](#application-registration) in einem Azure AD-Mandanten, der Anwendung und die wichtigsten Dienstobjekt bereitstellt eingerichtet. Einige Microsoft bereitgestellten APIs, wie etwa die Azure AD Graph-API haben Dienst Hauptbenutzer in alle Mandanten zur Verfügung gestellt, während der Bereitstellung bereits registriert.

## <a name="roles"></a>Rollen
Wie [Bereiche](#scopes)Möglichkeit Rollen für eine [Ressourcenserver](#resource-server) mit Zugriff auf die geschützten Ressourcen gesteuert. Es gibt zwei Arten: eine Rolle "Benutzer" implementiert rollenbasierte Access-Steuerelement für Benutzer/Gruppen, die erfordern den Zugriff auf die Ressource, während eine Rolle "Anwendung" implementiert für [Clientanwendungen](#client-application) , die Zugriff benötigen.

Rollen Zeichenfolgen Ressource definiert sind (beispielsweise "Fußball Genehmiger", "Schreibgeschützt", "Directory.ReadWrite.All"), die [Azure klassischen Portal] verwalteten[ AZURE-classic-portal] über die Ressource [Anwendungsmanifest](#application-manifest), und in der Ressource [AppRoles Eigenschaft]gespeichert[AAD-Graph-Sp-Entity]. Das Azure klassische Portal dient auch zum Zuweisen von Benutzern, die "Benutzer" Rollen und Konfigurieren von Client- [Anwendungsberechtigungen](#permissions) zum Zugreifen auf einer Rolle "Anwendung".

Ausführliche Informationen zu der Anwendungsrollen von Azure AD-Graph-API verfügbar gemacht werden, finden Sie unter [Graph API Berechtigung Bereichen][AAD-Graph-Perm-Scopes]. Ein Implementierungsbeispiel schrittweise finden Sie unter [rollenbasierte Zugriffssteuerung in Cloud-Clientanwendungen Azure AD-][Duyshant-Role-Blog].

## <a name="scopes"></a>Bereiche
Wie [Rollen](#roles)bieten Bereiche eine Möglichkeit für eine [Ressourcenserver](#resource-server) mit Zugriff auf die geschützten Ressourcen gesteuert. Bereiche werden verwendet, um die [Bereichsbasierte] implementieren[ OAuth2-Access-Token-Scopes] zugreifen-Steuerelement für eine [Clientanwendung](#client-application) , die vom Besitzer delegierten Zugriff auf die Ressource gewährt wurde.

Bereiche sind Ressource definierten Zeichenfolgen (beispielsweise "Mail.Read", "Directory.ReadWrite.All") im [Azure klassischen Portal] verwaltete[ AZURE-classic-portal] über die Ressource [Anwendungsmanifest](#application-manifest), in der Ressource [oauth2Permissions Eigenschaft][AAD-Graph-Sp-Entity]. Im klassische Azure-Portal dient auch so konfigurieren Sie Client Anwendung [delegierte Berechtigungen](#permissions) , um einen Bereich zuzugreifen.

Eine bewährte Methode Benennungskonvention, besteht darin, ein Format "resource.operation.constraint" verwenden. Ausführliche Informationen zu den Bereichen von Azure AD-Graph-API verfügbar gemacht werden, finden Sie unter [Graph API Berechtigung Bereichen][AAD-Graph-Perm-Scopes]. Bereiche von Office 365-Diensten verfügbar gemachten finden Sie unter [Office 365-API Berechtigungen Bezug][O365-Perm-Ref].

## <a name="security-token"></a>Sicherheitstoken
Ein signiertes Dokument, Ansprüche, wie etwa ein OAuth2 Token oder SAML 2.0-Bewertung enthält. Für eine OAuth2 [Autorisierung erteilen](#authorization-grant), eine [Access-Token](#access-token) (OAuth2) und ein [Token-ID](OpenID Connect) -Typen von Sicherheitstokens sind, werden beide als eine [JSON Web Token (JWT)]implementiert[JWT].

## <a name="service-principal-object"></a>Hauptbenutzer Service-Objekt
Wenn Sie registrieren/aktualisieren eine Anwendung in [Azure klassischen Portal][AZURE-classic-portal], im Portal erstellt/Updates [Application-Objekt](#application-object) und ein entsprechendes Hauptbenutzer Service-Objekt für die Mandanten. Die Anwendung Global (über alle Mandanten, in dem die zugeordnete Anwendung wurde Zugriff gewährt), die Identität Anwendungskonfiguration Objekt *definiert* und die Vorlage aus dem zugehörigen entsprechenden Dienst wichtigsten Objekte sind zur Verwendung lokal zur Laufzeit (in einem bestimmten Mandanten) *abgeleitet* wird.

Finden Sie unter [Anwendung und Dienst Tilgungsanteile Objekte] [ AAD-App-SP-Objects] Weitere Informationen.

## <a name="sign-in"></a>Anmeldung
Der Vorgang des [Clientanwendung](#client-application) Endbenutzer-Authentifizierung initiieren und zugehörigen Zustand, zum Zweck ein [Sicherheitstoken](#security-token) erwerben und die Anwendung Sitzung in diesen Status Bereichsdefinition erfassen. State können Elemente wie Benutzerprofildaten einbeziehen und Informationen von token Ansprüche abgeleitet.

Die Funktion der Anwendung wird in der Regel-einmaliges Anmelden (SSO) implementiert wird verwendet. Es kann auch eine Funktion "Anmeldung" als Einstiegspunkt für den Zugriff auf eine Anwendung (bei der erstmaligen Anmeldung) Endbenutzer vorangestellt werden. Die Anmelde-Funktion wird verwendet, um sammeln und behalten zusätzliche Zustand speziell für den Benutzer, und [Benutzer Zustimmung](#consent)erfordern möglicherweise.

## <a name="sign-out"></a>Abmelden
Die Vorgehensweise zum dauerhaften Authentifizierung zugeordnet Endbenutzer, trennen den Status des Benutzers mit der [Clientanwendung](#client-application) Sitzung bei der [Anmeldung](#sign-in)

## <a name="tenant"></a>Mandanten
Eine Instanz von einem Azure AD-Verzeichnis wird als eine Azure AD-Mandanten bezeichnet. Es bietet eine Vielzahl von Funktionen, einschließlich:

- ein Registrierungsdienst für integrierte Applikationen
- Authentifizierung von Benutzerkonten und registrierten Applikationen
- REST-Endpunkte verschiedene Protokolle, einschließlich OAuth2 und SAML, einschließlich der [Autorisierung Endpunkt](#authorization-endpoint), [token Endpunkt](#token-endpoint) und der "Allgemeine" Endpunkt von [Applications mit mehreren Mandanten](#multi-tenant-application)verwendet Unterstützung erforderlich sind.

Ein Mandanten ist ebenfalls ein Azure AD zugeordnet oder Office 365-Abonnement während der Bereitstellung des Abonnements, Bereitstellen von Identität und Access Management-Funktionen für das Abonnement. Informationen [zum Abrufen von einem Mandanten Azure Active Directory] [ AAD-How-To-Tenant] ausführliche Informationen über die verschiedenen Optionen erhalten Sie Zugriff auf einen Mandanten. Finden Sie unter [wie Azure-Abonnements stehen im Zusammenhang mit Azure Active Directory] [ AAD-How-Subscriptions-Assoc] ausführliche Informationen über die Beziehung zwischen Abonnements und einem Azure AD-Mandanten.

## <a name="token-endpoint"></a>Token Endpunkt
Einer der Endpunkte vom [Server Autorisierung](#authorization-server) zur Unterstützung von OAuth2 [Autorisierung gewährt](#authorization-grant). Je nach dem erteilen Hiermit können Sie werden in einer [Access-Token](#access-token) (und verwandte "Aktualisieren" Token) erwerben einem [Kunden](#client-application)oder [Token-ID](#ID-token) bei Verwendung mit der [Herstellen OpenID] [ OpenIDConnect] Protokoll.

## <a name="user-agent-based-client"></a>Benutzer-Agents-basierten client
Eine Art von [Clientanwendung](#client-application) , Code aus einem Webserver downloads innerhalb eines Benutzer-Agent (z. B. einem Webbrowser), wie etwa ein SP-Anwendung (einzelne Seite a) ausgeführt wird. Da der gesamte Code auf einem Gerät ausgeführt wird, gilt einen "öffentlichen" Client aufgrund der Unfähigkeit zum Speichern von Anmeldeinformationen privat/vertraulich. Finden Sie unter [OAuth2 Clienttypen und Profile] [ OAuth2-Client-Types] Weitere Details.

## <a name="user-principal"></a>Benutzerprinzipalnamen
Ähnlich wie ein Dienst Hauptbenutzer Objekt zur Darstellung einer Anwendungsinstanz verwendet wird, ist ein Benutzerprinzipalobjekt eine andere Art von Sicherheit Hauptbenutzer, die einen Benutzer darstellt. Die Azure AD Graph- [Benutzer Entität] [ AAD-Graph-User-Entity] definiert das Schema für einen Benutzerobjekt, einschließlich der Benutzer-bezogene Eigenschaften wie z. B. vor-und Nachnamen, Benutzerprinzipalnamen, Directory Rollenmitgliedschaft. Dadurch wird die Konfiguration der Identität für Azure AD-eines Benutzerprinzipalnamen zur Laufzeit herstellen. Die Benutzer der Tilgungsanteile wird verwendet, um die Darstellung eines authentifizierten Benutzers für einmaliges Anmelden, Aufzeichnung Delegation [Zustimmung](#consent) , Access Steuerelement Entscheidungen usw..

## <a name="web-client"></a>WebClient
Eine Art von [Client-Anwendung](#client-application) , die alle Code auf einem Webserver und in der Lage ausführt als "Vertraulich" Client funktioniert nach sicheres seine Anmeldeinformationen auf dem Server gespeichert. Finden Sie unter [OAuth2 Clienttypen und Profile] [ OAuth2-Client-Types] Weitere Details.

## <a name="next-steps"></a>Nächste Schritte
Der [Azure AD Developer Guide] [ AAD-Dev-Guide] ist das Portal für alle Azure AD-Entwicklung verwendet Verwandte Themen, einschließlich einen Überblick über die [Integration der Anwendung] [ AAD-How-To-Integrate] und die Grundlagen von [Azure AD-Authentifizierung und unterstützte Authentifizierungsszenarien][AAD-Auth-Scenarios].

Verwenden Sie die folgenden Disqus Kommentarbereich um Feedback geben und helfen Sie uns verfeinern und unsere Inhalte modellieren.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
