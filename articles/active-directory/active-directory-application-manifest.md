<properties
   pageTitle="Grundlegendes zu den Azure-Active Directory-Anwendungsmanifest | Microsoft Azure"
   description="Detaillierte des Anwendungsmanifests Azure Active Directory, die einer Identität Anwendungskonfiguration in einem Azure AD-Mandanten darstellt, und wird verwendet, um OAuth Autorisierung, Zustimmung Erfahrung und mehr zu erleichtern."
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
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Grundlegendes zu Anwendungsmanifest Azure Active Directory

Programme, die mit Azure Active Directory (AD) integrieren müssen mit einem Azure AD-Mandanten, somit eine beständigen Identität Konfiguration für die Anwendung registriert sein. Diese Konfiguration wird zur Laufzeit, aktivieren Szenarien, mit denen eine Anwendung Auslagern und broker-Authentifizierung/Autorisierung über Azure AD gehört. Weitere Informationen über das Modell der Azure AD-Anwendung finden Sie unter [Hinzufügen, aktualisieren, und Entfernen einer Anwendung] [ ADD-UPD-RMV-APP] Artikel.

## <a name="updating-an-applications-identity-configuration"></a>Aktualisieren einer Anwendungskonfiguration Identität

Tatsächlich mehrere Optionen stehen zur Verfügung, für die Aktualisierung der Eigenschaften auf einer Anwendungskonfiguration Identität die Funktionen und Grad der Probleme, einschließlich der folgenden Unterschiede aufweisen:

- Die ** [Azure klassischen Portal] [ AZURE-CLASSIC-PORTAL] Web-Benutzeroberfläche** ermöglicht es Ihnen, die am häufigsten verwendeten Eigenschaften der Anwendung zu aktualisieren. Dies ist die schnellste und mindestens Fehler häufig Methode Ihrer Anwendung Eigenschaften aktualisieren, aber es Ihnen nicht Vollzugriff auf alle Eigenschaften, wie die folgenden beiden Methoden.
- Für erweiterte Szenarien, in dem Sie Eigenschaften aktualisieren, die nicht in der klassischen Azure-Portal verfügbar gemacht werden müssen, können Sie das **Anwendungsmanifest**ändern. Dies liegt der Schwerpunkt dieses Artikels und wird im nächsten Abschnitt beginnend ausführlicher erläutert.
- Es ist es möglich, **Schreiben Sie eine Anwendung, die die [Graph-API] verwendet[ GRAPH-API] ** Ihrer Anwendung, aktualisieren, die am häufigsten leistungsgesteuert einzugeben. Eine attraktive Option möglicherweise zwar, wenn Sie Managementsoftware schreiben oder Anwendungseigenschaften in regelmäßigen Abständen automatisch und aktualisieren müssen.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Verwenden das Anwendungsmanifest einer Anwendungskonfiguration Identität aktualisieren
Über das [Azure klassischen Portal][AZURE-CLASSIC-PORTAL], Sie können Ihre Anwendungskonfiguration Identität verwalten, indem Sie herunterladen und Hochladen einer JSON-Datei Darstellung, die ein Anwendungsmanifest bezeichnet wird. Keine tatsächliche Datei wird im Verzeichnis gespeichert. Das Anwendungsmanifest ist lediglich einen HTTP GET-Vorgang für die Entität Azure AD Graph-API-Anwendung und der Upload eines Vorgangs HTTP-PATCH für die Anwendung Entität.

Daher müssen Sie akzeptieren, um das Format und die Eigenschaften des Anwendungsmanifests zu verstehen, die Graph-API [Anwendung Entität] Bezug[ APPLICATION-ENTITY] Dokumentation. Beispiele für Updates, die ausgeführt werden können, obwohl Anwendungsmanifest Upload sind:

- **Declare Berechtigung Bereiche (oauth2Permissions)** von Ihrem Web-API verfügbar gemacht werden. Finden Sie im Thema "Verfügbarmachen von Web-APIs zu anderen Applications" in [Clientanwendungen mit Azure Active Directory Integration] [ INTEGRATING-APPLICATIONS-AAD] Informationen zum Implementieren der Benutzeridentitätswechsel mithilfe der oauth2Permissions Delegierte Berechtigung Bereich. Wie zuvor schon erwähnt, sind Anwendung Elementeigenschaften in das Graph-API [Entität und komplexe Typ] dokumentierten[ APPLICATION-ENTITY] Bezug Artikel, einschließlich der oauth2Permissions-Eigenschaft also eine Auflistung vom Typ [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Declare Anwendungsrollen (AppRoles), Ihre app bereitgestellt werden**. Die Anwendung-Entität AppRoles Eigenschaft folgt eine Zusammenstellung von Typ [AppRole][APPLICATION-ENTITY-APP-ROLE]. Die [rollenbasierte Zugriffssteuerung in Azure AD-Cloud-Clientanwendungen] finden Sie unter[ RBAC-CLOUD-APPS-AZUREAD] Artikel für ein Beispiel für die Implementierung.
- **Declare bekannte Client Applications (KnownClientApplications)**, die Ihnen ermöglichen, die Zustimmung der angegebene Client Anwendung(en) Ressource/Web API logisch einbinden.
- **Anfordern von Azure AD Gruppe ausgeben, die Mitgliedschaften beanspruchen** , für die Anmeldung bei Benutzer (GroupMembershipClaims).  Dies kann auch zum Ausführen Ansprüche über die Mitgliedschaften des Benutzers Directory Rollen konfiguriert werden. Finden Sie unter die [Autorisierung in Clientanwendungen Cloud Entwurfsphase AD] [ AAD-GROUPS-FOR-AUTHORIZATION] Artikel für ein Beispiel für die Implementierung.
- **Zulassen der Anwendung zur Unterstützung von OAuth 2.0 implizit erteilen** Zahlungen (oauth2AllowImplicitFlow). Diese Art von erteilen Fluss mit eingebetteten JavaScript-Webseiten oder (gesicherte KENNWORTAUTHENTIFIZIERUNG einzelne Seite Applications) verwendet. Weitere Informationen über die implizite Autorisierung erteilen, finden Sie unter [Grundlegendes zu den implizit OAuth2 Fluss in Azure Active Directory erteilen][IMPLICIT-GRANT].
- **Aktivieren der Verwendung von X509 als geheimen Schlüssel Zertifikate** (KeyCredentials). Das [Erstellen von Dienst und Daemon apps in Office 365] finden Sie unter[ O365-SERVICE-DAEMON-APPS] und [Entwicklers Leitfaden für autorisierende API Ressourcenmanager Azure] [ DEV-GUIDE-TO-AUTH-WITH-ARM] Artikel Beispiele für die Implementierung.
- **Hinzufügen einer neuen App-ID-URI** für eine Anwendung (identifierURIs[]). App-ID-URIs werden verwendet, um die Anwendung innerhalb der Azure AD-Mandanten (oder über mehrere Azure AD-Mandanten, für Szenarien mit mehreren Mandanten, wenn über überprüft benutzerdefinierte Domäne qualifiziert) eindeutig identifizieren. Sie werden beim Anfordern von Berechtigungen für eine Ressource Anwendung oder beim Abrufen eines Access-Token für eine Ressource Anwendung verwendet. Wenn Sie dieses Element aktualisieren, besteht das Update ServicePrincipalNames []-Auflistung die entsprechenden Dienst Tilgungsanteile, die in der Anwendung Start Mandanten verfügbar ist.

Das Anwendungsmanifest enthält auch eine gute Möglichkeit, um den Status Ihrer Anwendung Registrierung nachzuverfolgen. Da es im JSON-Format verfügbar ist, kann die Darstellung der Datei in Ihrer Datenquellen-Steuerelements, zusammen mit Ihrer Anwendung Quellcode eingecheckt werden.

## <a name="step-by-step-example"></a>Schritt anhand eines Beispiels Schritt
Jetzt können die Schritte erforderlich, um Ihre Identität Anwendungskonfiguration durch das Anwendungsmanifest aktualisieren durchzuführen. Wir werden von den vorherigen Beispielen, eine hervorheben, so deklarieren Sie einen neuen Berechtigungsstufe Bereich auf eine Ressource Anwendung mit:

1. Navigieren Sie zu der [Azure klassischen Portal] [ AZURE-CLASSIC-PORTAL] , und melden Sie sich mit einem Konto, Dienst oder Co-Administratoren Berechtigungen verfügt.

2. Nachdem Sie authentifiziert haben, führen Sie einen nach unten Bildlauf und wählen im linken Navigationsbereich (1) die Erweiterung Azure "Active Directory" an, und klicken Sie auf registered der Azure AD-Mandanten, wo finde ich eine Anwendung (2).

    ![Wählen Sie aus den Azure AD-Mandanten][SELECT-AZURE-AD-TENANT]

3. Wenn die Seite Verzeichnis auftauchen, klicken Sie am oberen Rand der Seite, um eine Liste der Anwendungen in den Mandanten registriert finden Sie unter "Applications" (1) auf. Suchen Sie dann die Anwendung, die Sie verwenden möchten, in der Liste aktualisieren aus, und klicken darauf (2).

    ![Wählen Sie aus den Azure AD-Mandanten][SELECT-AZURE-AD-APP]

4. Jetzt, da Sie Hauptseite der Anwendung ausgewählt haben, beachten Sie das Feature "Manifest verwalten" an den unteren Rand der Seite (1). Wenn Sie auf diesen Link klicken, werden Sie aufgefordert herunterzuladen oder zu die JSON-Manifestdatei hochladen. Klicken Sie auf "Download Manifest" (2). Daraufhin wird sofort mit der Download Bestätigungsdialogfeld Aufforderung, bestätigen, indem Sie auf "Herunterladen Manifest" (3), und klicken Sie dann entweder öffnen oder Speichern der Datei lokal (4) folgen.

    ![Verwalten des Manifests, Option herunterladen][MANAGE-MANIFEST-DOWNLOAD]

    ![Laden Sie das manifest][DOWNLOAD-MANIFEST]

5. In diesem Beispiel gespeichert wir die Datei lokal, da wir in einem Editor öffnen, nehmen Sie Änderungen an den JSON und speichern Sie erneut. So sieht wie die JSON-Struktur in den Visual Studio JSON-Editor aussieht. Beachten Sie, dass sie das Schema für die [Anwendung Entität] folgt[ APPLICATION-ENTITY] wie bereits zuvor erwähnt:

    ![Aktualisieren der Manifesten JSON][UPDATE-MANIFEST]

    Beispielsweise würde unter der Voraussetzung, dass wir Implementieren/einer neuen Berechtigungsstufe namens "Employees.Read.All" auf unsere Anwendung Ressource (API) verfügbar machen möchten, Sie einfach eine neue/zweite Element in der Websitesammlung oauth2Permissions ie hinzufügen:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Der Eintrag muss eindeutig sein, und Sie müssen daher Generieren einer neuen ID (GLOBALLY Unique) für die `"id"` Eigenschaft. In diesem Fall, da wir angegeben `"type": "User"`, diese Berechtigung zugestimmt werden kann, um von einem beliebigen Konto authentifiziert, indem Sie die Azure AD-Mandanten, in denen die Ressource/API-Anwendung registriert ist. Damit wird den Client erteilt Anwendungsberechtigung für den Zugriff auf Auftrag des Kontos. Die Beschreibung und Anzeige Namenszeichenfolgen werden während der Zustimmung und für die Anzeige in der klassischen Azure-Portal verwendet.

6. Wenn Sie das Manifest aktualisieren abgeschlossen haben, kehren Sie zur Seite Azure AD-Anwendung im klassischen Azure-Portal, und klicken Sie auf "der Verwalten Manifest"-Feature erneut (1). Aber dieses Mal, wählen Sie die Option "Hochladen Manifest" (2). Ähnlich wie das Herunterladen, werden Sie erneut mit einem zweiten Dialogfeld, angezeigt und Sie werden aufgefordert, den Speicherort der JSON-Datei. Klicken Sie auf "Datei... Suchen" (3), dann verwenden Sie im Dialogfeld "Wählen Sie Datei zum Hochladen", um die JSON-Datei (4) auszuwählen, und klicken Sie auf "Öffnen". Nachdem das Dialogfeld verschwindet, wählen Sie das Häkchen "OK" (5) und ein-Manifests hochgeladen werden soll.  

    ![Verwalten des Manifests, Option hochladen][MANAGE-MANIFEST-UPLOAD]

    ![Hochladen der Manifesten JSON][UPLOAD-MANIFEST]

    ![Hochladen der Manifesten JSON - Bestätigung][UPLOAD-MANIFEST-CONFIRM]

Nachdem Sie nun das Manifest gespeichert wurde, können Sie eine eingetragene geben Client Anwendungszugriff auf die neue Berechtigungsstufe, die wir über hinzugefügt. Diesmal Azure klassischen des Portals Web-Benutzeroberfläche können Sie statt der Clientanwendung Manifest zu bearbeiten:  

1. Zuerst navigieren Sie zu der Seite "Konfigurieren" der Clientanwendung mit der Sie den Zugriff auf die neue API hinzufügen möchten, und klicken Sie auf die Schaltfläche "Anwendung hinzufügen".
2. Dann wird die Liste der registrierten Ressource Applikationen (APIs) in den Mandanten angezeigt werden. Klicken Sie auf das Pluszeichen / +-Symbol neben dem Namen der Ressource-Anwendung, um ihn auszuwählen.  
3. Klicken Sie dann auf das Häkchen in der unteren rechten Ecke.
4. Wenn Sie im Abschnitt "Anwendung hinzufügen" auf der Seite des Kunden diagnostizieren zurückkehren, sehen Sie die neue Ressource Anwendung in der Liste. Wenn Sie auf rechts neben die Zeile im Abschnitt "Delegierte Berechtigungen" zeigen, sehen Sie eine Dropdownliste angezeigt. Klicken Sie auf die Liste, und wählen Sie dann die neue Berechtigungsstufe, damit er den Kunden angeforderten Liste der Berechtigungen hinzugefügt. Hinweis: Diese neue Berechtigung wird in der Clientanwendung Identität Konfiguration, in der Eigenschaft "RequiredResourceAccess" Websitesammlung gespeichert werden.

![Berechtigungen für andere Programme][PERMS-TO-OTHER-APPS]

![Berechtigungen für andere Programme][PERMS-SELECT-APP]

![Berechtigungen für andere Programme][PERMS-SELECT-PERMS]

Das war's auch. Jetzt werden der Anwendung mit ihrer neuen Identität Konfiguration ausgeführt.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen hierzu auf die Beziehung zwischen einer Anwendung Anwendung und Dienst Tilgungsanteile Objekte finden Sie unter [Anwendung und Dienst wichtigsten Objekte in Azure AD-][AAD-APP-OBJECTS].
- Sehen Sie sich die [Azure AD-Entwicklertools Glossar] [ AAD-DEVELOPER-GLOSSARY] für einige der grundlegenden Azure Active Directory (AD) Developer Konzepte Definitionen.

Verwenden Sie im Kommentarbereich DISQUS unten um Feedback geben und helfen Sie uns verfeinern und unsere Inhalte modellieren.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

