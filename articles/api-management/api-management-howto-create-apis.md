<properties 
    pageTitle="So erstellen Sie APIs in Azure-API Management" 
    description="Informationen Sie zum Erstellen und Konfigurieren von APIs in Azure-API Management." 
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
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-apis-in-azure-api-management"></a>So erstellen Sie APIs in Azure-API Management

Eine API in API Management stellt eine Reihe von Vorgängen, die von Clientanwendungen aufgerufen werden können. Neue APIs im Portal Publisher erstellt werden, und klicken Sie dann die gewünschten Vorgänge hinzugefügt werden. Nachdem die Vorgänge hinzugefügt haben, wird die API ist ein Produkt hinzugefügt und kann veröffentlicht werden. Sobald eine API veröffentlicht wurde, kann es abonniert und von Entwicklern verwendet wird.

Dieses Handbuch zeigt im ersten Schritt im Prozess: So erstellen und konfigurieren eine neue API in API Management. Weitere Informationen zum Hinzufügen von Vorgängen und Veröffentlichen eines Produkts finden Sie unter [Hinzufügen von Vorgängen an eine API][] und [So erstellen und Veröffentlichen eines Produkts][].

## <a name="create-new-api"> </a>Erstellen Sie eine neue API

APIs erstellt und im Publisher-Portal konfiguriert. Wenn Sie das Publisher-Portal zugreifen zu können, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **APIs** , und klicken Sie dann auf **API hinzufügen**.

![Erstellen von API][api-management-create-api]

Verwenden Sie im Fenster **neue API hinzufügen** , um die neue API konfigurieren.

![Hinzufügen von neuen API][api-management-add-new-api]

Die folgenden Felder werden zum Konfigurieren der neuen-API verwendet.

-   **Name der Web-API** bietet einen eindeutigen und aussagekräftigen Namen für die-API. Es wird in der Entwickler und Herausgeber Portals angezeigt.
-   **Webdienst-URL** verweist auf die API implementieren HTTP-Dienst. API Management leitet Anfragen an diese Adresse.
-   **Suffix API-Web-URL** ist die Basis-URL für die API-Verwaltungsdienst angefügt. Die Basis URL häufig alle APIs durch eine Instanz der API Management-Dienst gehostet werden. API Management unterscheidet APIs, indem Sie ihre Suffix und daher das Suffix muss für jede API für einen bestimmten Herausgeber eindeutig sein.
-   **Web API URL-Schema** bestimmt, welche Protokolle verwendet werden können, auf die API zugreifen. HTTPs wird standardmäßig angegeben.
-   Um diese neuen API optional ein Produkt hinzuzufügen, klicken Sie auf den Dropdown- **Produkte (optional)** , und wählen Sie ein Produkt aus. Dieser Schritt kann mehrmals mehrere Produkte die API hinzuzufügende wiederholt werden.

Nachdem Sie die gewünschten Werte konfiguriert haben, klicken Sie auf **Speichern**. Nachdem die neue API erstellt wurde, wird die Seite "Zusammenfassung" für die-API im Portal Publisher angezeigt.

![Zusammenfassung der API][api-management-api-summary]

## <a name="configure-api-settings"> </a>API Konfigurieren von Einstellungen

Sie können die Registerkarte **Einstellungen** zu überprüfen und bearbeiten Sie die Konfiguration für eine API verwenden. **Name der Web-API**, **Webdienst-URL**und **Web API URL-Suffix** Anfangs Wenn die-API erstellt und geändert werden kann hier festgelegt. **Die Beschreibung** bietet eine optionale Beschreibung und **Web API URL-Schema** bestimmt, welche Protokolle verwendet werden können, auf die API zugreifen.

![API-Einstellungen][api-management-api-settings]

Wählen Sie die Registerkarte **Sicherheit** , um die Gateway-Authentifizierung für die Back-End-Dienst implementieren der-API konfigurieren. Der **mit Anmeldeinformationen** Dropdown-Liste kann zum Konfigurieren der **grundlegenden HTTP** oder **Client-Zertifikate** Authentifizierung verwendet werden. Wenn HTTP-Standardauthentifizierung verwenden möchten, geben Sie einfach die gewünschten Anmeldeinformationen. Finden Sie unter Informationen zur Verwendung von Client Zertifikatauthentifizierung [wie Sie mithilfe der Client Zertifikatauthentifizierung in Azure-API Management Back-End-Dienste zu schützen][].

Die Registerkarte **Sicherheit** kann auch verwendet werden, die **Autorisierung des Benutzers** mit OAuth 2.0 konfiguriert. Weitere Informationen finden Sie unter [So autorisieren Entwicklertools Konten mit OAuth 2.0 in Azure-API Management][].

![Standardauthentifizierung-Einstellungen][api-management-api-settings-credentials]

Klicken Sie auf **Speichern** , um die Änderungen zu speichern, die an den Einstellungen API vorgenommen werden.

## <a name="next-steps"> </a>Nächste Schritte

Nachdem eine API erstellt wird, und die Einstellungen, die so konfiguriert ist, die nächsten Schritten werden die Vorgänge an die API hinzufügen, ein Produkt der API hinzu, und veröffentlichen Sie, sodass es für Entwickler verfügbar ist. Weitere Informationen finden Sie unter den folgenden Artikeln.

-   [Zum Hinzufügen von Vorgängen an eine API][]
-   [So erstellen und Veröffentlichen eines Produkts][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Zum Hinzufügen von Vorgängen zu einer API]: api-management-howto-add-operations.md
[So erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md

[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Wie Back-End-Services-Clients Zertifikatauthentifizierung in Azure-API Management gesichert]: api-management-howto-mutual-certificates.md
[So autorisieren Entwicklertools Konten OAuth 2.0 in Azure-API Management verwenden]: api-management-howto-oauth2.md