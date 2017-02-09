<properties
    pageTitle="Wie Sie eine Web-API Back-End-mit Azure Active Directory und -API Management schützen"
    description="Informationen Sie zum Schützen einer Web-API Back-End-mit Azure Active Directory und API-Verwaltung." 
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

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Wie Sie eine Web-API Back-End-mit Azure Active Directory und -API Management schützen

Das folgende Video wird gezeigt, wie Erstellen einer Web-API Back-End- und schützen sie OAuth 2.0-Protokoll mit Azure Active Directory und Management-API verwendet werden.  Dieser Artikel enthält eine Übersicht und Weitere Informationen zu den Schritten in das Video an. Diese 24 Minuten Video wird gezeigt, wie an:

-   Erstellen Sie einer Web-API Back-End- und Sichern Sie es mit AAD - 1:30 Uhr ab
-   Importieren Sie die API in API Management - 7:10 ab
-   Konfigurieren der Entwicklerportal zum Aufrufen der API - 9:09 ab
-   Konfigurieren einer desktop-Anwendungs aufrufen, die API - 18:08 ab
-   Konfigurieren einer Richtlinie Überprüfung JWT Anfragen - 20:47 ab vorab autorisieren

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Erstellen Sie eine Azure AD-Verzeichnis

Der Web-API gesichert gesicherten Azure Active Directory zuerst Sie müssen mithilfe einer eine AAD Mandanten. In diesem Video wird ein Mandanten, mit dem Namen **APIMDemo** verwendet. Zum Erstellen einer AAD Mandanten [Klassischen Azure-Portal](https://manage.windowsazure.com) anmelden und auf **neu**->**App Services**->**Active Directory**->**Verzeichnis**->**Benutzerdefinierte erstellen**. 

![Azure-Active Directory][api-management-create-aad-menu]

In diesem Beispiel wird ein Verzeichnis mit dem Namen **APIMDemo** mit der Standarddomäne mit dem Namen **DemoAPIM.onmicrosoft.com**erstellt. Dieses Verzeichnis wird in der gesamten das Video verwendet.

![Azure-Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Erstellen eines Web-API Diensts durch Azure Active Directory gesichert

In diesem Schritt wird eine Web-API Back-End-mit Visual Studio 2013 erstellt. In diesem Schritt des Videos beginnt um 1:30. Zum Erstellen von Web-API Back-End-Projekt in Visual Studio, klicken Sie auf **Datei**->**neu**->**Projekt**, und wählen Sie aus **der Liste der Vorlagen** **ASP.NET Web-Anwendung** aus. In diesem Video wird das Projekt **APIMAADDemo**bezeichnet. Klicken Sie auf **OK** , um das Projekt zu erstellen. 

![Visual Studio][api-management-new-web-app]

Klicken Sie auf **Web-API** aus, **Wählen Sie eine Vorlage aus** , ein Web-API Projekt erstellen. Zum Konfigurieren von Azure Directory-Authentifizierung klicken Sie auf **Authentifizierung ändern**.

![Neuen Projekts][api-management-new-project]

Klicken Sie auf **Organisationskonten**, und geben Sie die **Domäne** Ihrer AAD Mandanten. In diesem Beispiel ist die Domäne **DemoAPIM.onmicrosoft.com**. Die Domäne Ihres Verzeichnisses kann über die Registerkarte **Domains** Ihres Verzeichnisses abgerufen werden.

![Domänen][api-management-aad-domains]

Konfigurieren Sie die gewünschten Einstellungen im Dialogfeld **Authentifizierung ändern** , und klicken Sie auf **OK**.

![Ändern von Authentifizierung][api-management-change-authentication]

Wenn Sie auf **OK** geklickt Visual Studio versucht, eine Anwendung mit Ihrem Verzeichnis Azure AD-registrieren, und Sie möglicherweise aufgefordert, melden Sie sich von Visual Studio. Melden Sie sich mit einem Administratorkonto für Ihr Verzeichnis.

![Melden Sie sich bei Visual Studio][api-management-sign-in-vidual-studio]

Konfigurieren dieses Projekt als eine Azure-Web-API aktivieren Sie das Kontrollkästchen für **Host in der Cloud** , und klicken Sie dann auf **OK**.

![Neues Projekt][api-management-new-project-cloud]

Sie möglicherweise aufgefordert, die für die Anmeldung bei Azure, und klicken Sie dann können Sie die Web App konfigurieren.

![Konfigurieren][api-management-configure-web-app]

In diesem Beispiel wird mit dem Namen **APIMAADDemo** neuen **App-Serviceplan** angegeben.

Klicken Sie auf **OK** , um Konfigurieren der Web-App und das Projekt erstellen.

## <a name="add-the-code-to-the-web-api-project"></a>Fügen Sie den Code des Projekts Web-API

Im nächsten Schritt im Video hinzugefügt den Code der Web-API Projekt. Dieser Schritt beginnt am 4:35.

In diesem Beispiel die Web-API implementiert einen einfachen Taschenrechner Dienst mithilfe eines Modells und einem Controller. Wenn Sie das Modell für den Dienst hinzufügen möchten, mit der rechten Maustaste **Modelle** in **Lösung Explorer** , und wählen Sie **Hinzufügen**, **Class**. Nennen Sie die Klasse `CalcInput` , und klicken Sie auf **Hinzufügen**.

Fügen Sie den folgenden `using` an den Anfang der Anweisung der `CalcInput.cs` Datei.

    using Newtonsoft.Json;

 Ersetzen Sie die generierte Klasse mit den folgenden Code ein.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Mit der rechten Maustaste **Controller** in **Lösung Explorer** , und wählen Sie **Hinzufügen**->**Controller**. Wählen Sie **Web API 2 Controller - leeren** aus, und klicken Sie auf **Hinzufügen**. Geben Sie den Namen des Controller **CalcController** , und klicken Sie auf **Hinzufügen**.

![Controller hinzufügen][api-management-add-controller]

Fügen Sie den folgenden `using` Anweisung am oberen Rand der `CalcController.cs` Datei.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Ersetzen Sie die generierten Controller-Klasse durch den folgenden Code ein. Dieser Code implementiert die `Add`, `Subtract`, `Multiply`, und `Divide` Vorgänge der grundlegenden Rechner-API.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Drücken Sie **F6** zum Erstellen und überprüfen die Lösung.

## <a name="publish-the-project-to-azure"></a>Veröffentlichen Sie das Projekt in Azure

In diesem Schritt Visual Studio Project Azure veröffentlicht. In diesem Schritt des Videos beginnt am 5:45.

Mit der rechten Maustaste in des Projekts **APIMAADDemo** in Visual Studio, und wählen Sie **Veröffentlichen**, um das Projekt in Azure zu veröffentlichen. Beachten Sie die Standardeinstellungen im Dialogfeld im **Web veröffentlichen** und auf **Veröffentlichen**.

![Web veröffentlichen][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Erteilen von Berechtigungen zur Verwaltung der Azure AD-Back-End-Service-Anwendung

Eine neue Anwendung für die Back-End-Dienst wird in Ihrem Verzeichnis Azure AD-als Teil des Prozesses konfigurieren und Veröffentlichen des Projekts Web-API erstellt. In diesem Schritt des Videos, 6:13 beginnend, sind die Back-End-Web-API-Berechtigungen gewährt.

![Anwendung][api-management-aad-backend-app]

Klicken Sie auf den Namen der Anwendung, die erforderlichen Berechtigungen konfigurieren. Navigieren Sie zur Registerkarte **Konfigurieren** , und führen Sie einen Bildlauf nach unten bis zum Abschnitt **Berechtigungen für andere Programme** . Klicken Sie auf das Dropdownmenü neben **Windows** **Azure Active Directory** **Anwendungsberechtigungen** , aktivieren Sie das Kontrollkästchen für **die Directory Daten lesen**, und klicken Sie auf **Speichern**.

![Hinzufügen von Berechtigungen][api-management-aad-add-permissions]

>[AZURE.NOTE] Wenn **Windows** **Azure Active Directory** klicken Sie unter Berechtigungen in andere Programme nicht aufgeführt ist, klicken Sie auf **Anwendung hinzufügen** , und fügen Sie ihn aus der Liste hinzu.

Notieren Sie den **App-ID-URI** zur Verwendung in einem der folgenden Schritte bei Anwendung Azure AD-für Entwickler-API Verwaltungsportal konfiguriert ist.

![App-ID-URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Importieren Sie das Web-API in API Management

APIs werden vom API Publisher-Portal so konfiguriert ist, das über das klassische Azure-Portal zugegriffen werden kann. Klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst, um Publisher-Portal zu gelangen. Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service Instanz][] im Lernprogramm [Verwalten Ihrer ersten API][] .

![Publisher-portal][api-management-management-console]

Vorgänge können [manuell APIs hinzugefügt](api-management-howto-add-operations.md)werden, oder können importiert werden. In diesem Video werden die Vorgänge im Swagger Format ab 6:40 importiert.

Erstellen Sie eine Datei mit dem Namen `calcapi.json` folgende Themen und speichern Sie diese auf Ihren Computer. Sicherstellen, dass die `host` Punkt in der Web-API Back-End-Attribut. In diesem Beispiel `"host": "apimaaddemo.azurewebsites.net"` verwendet wird.

{"swagger": "2.0", "Info": {"Titel": "Rechner", "Beschreibung": "Arithmetics über HTTP!", "Version": "1.0"}, "Host": "apimaaddemo.azurewebsites.net", "BasePath": "/ api", "Schemas": ["http"], "Pfade": {"/ hinzufügen? eine = {a} & b = {b}": {"abrufen": {"Beschreibung": "Reagiert mit einer Summe von zwei Zahlen.", "OperationId": "Hinzufügen von zwei Zahlen", "Parameter": [{"Name": "a", "in": "Abfrage", "Beschreibung": "erste Operand. Ist der Standardwert <code>51</code>. ","erforderlich": WAHR, auf"Standard":"51","Enumeration": ["51"]}, {"Name":"b";"in":"Abfrage","Beschreibung":"zweite Operand. Ist der Standardwert <code>49</code>. ","erforderlich": WAHR, auf"Standard":"49","Enumeration": ["49"]}],"Antworten": {}}}," / Sub?a = {a} & b = {b} ": {"abrufen": {"Beschreibung":"Reagiert mit einer Differenz zwischen zwei Zahlen.","OperationId":"Subtrahieren Sie zwei Zahlen","Parameter": [{"Name":"a","in":"Abfrage","Beschreibung":"erste Operand. Ist der Standardwert <code>100</code>. ","erforderlich": WAHR, auf"Standard":"100","Enumeration": ["100"]}, {"Name":"b";"in":"Abfrage","Beschreibung":"zweite Operand. Ist der Standardwert <code>50</code>. ","erforderlich": WAHR, auf"Standard":"50","Enumeration": ["50"]}],"Antworten": {}}}," / Div?a = {a} & b = {b} ": {"abrufen": {"Beschreibung":"Reagiert mit einer Quotienten zweier Zahlen.","OperationId":"Dividieren zwei Zahlen","Parameter": [{"Name":"a","in":"Abfrage","Beschreibung":"erste Operand. Ist der Standardwert <code>100</code>. ","erforderlich": WAHR, auf"Standard":"100","Enumeration": ["100"]}, {"Name":"b";"in":"Abfrage","Beschreibung":"zweite Operand. Ist der Standardwert <code>20</code>. ","erforderlich": WAHR, auf"Standard":"20","Enumeration": ["20"]}],"Antworten": {}}}," / Mul?a = {a} & b = {b} ": {"abrufen": {"Beschreibung":"Reagiert mit Produkt aus zwei Zahlen.","OperationId":"Multiplikation zweier ganzer Zahlen","Parameter": [{"Name":"a","in":"Abfrage","Beschreibung":"erste Operand. Ist der Standardwert <code>20</code>. ","erforderlich": WAHR, auf"Standard":"20","Enumeration": ["20"]}, {"Name":"b";"in":"Abfrage","Beschreibung":"zweite Operand. Ist der Standardwert <code>5</code>. ","erforderlich": WAHR, auf"Standard":"5","Enumeration": ["5"]}],"Antworten": {}}}}}

Informationen zum Importieren den Taschenrechner API klicken Sie im Menü " **API Management** " auf der linken Seite auf **APIs** , und klicken Sie dann auf **Importieren API**.

![Schaltfläche Importieren API][api-management-import-api]

Führen Sie die folgenden Schritte aus, um den Rechner API zu konfigurieren.

1. Klicken Sie auf **aus Datei**, wechseln Sie zu der `calculator.json` Dateien gespeichert werden soll, und klicken Sie auf das Optionsfeld **Swagger** .
2. Geben Sie in das Textfeld **Web API URL-Suffix** **Calc** ein.
3. Klicken Sie im Feld **Produkte (optional)** ein, und wählen Sie **Starter**.
4. Klicken Sie auf **Speichern** , um die API importieren.

![Hinzufügen von neuen API][api-management-import-new-api]

Sobald die API importiert sind, wird die Zusammenfassungsseite für die API im Portal Publisher angezeigt.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Rufen Sie die API erfolglos aus der Entwicklerportal

Zu diesem Zeitpunkt die-API wurde in API Management importiert, jedoch kann keine noch aufgerufen werden erfolgreich aus der Entwicklerportal, da der Back-End-Dienst mit Azure AD-Authentifizierung geschützt ist. Dies wird im Video 7:40 mithilfe der folgenden Schritte ab veranschaulicht.

Klicken Sie auf **Entwicklerportal** in der oberen rechten Seite des Portals Publisher.

![-Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie auf **APIs** , und klicken Sie auf die **Rechner** API.

![-Entwicklerportal][api-management-dev-portal-apis]

Klicken Sie auf, **Probieren Sie es**.

![Probieren Sie es][api-management-dev-portal-try-it]

Klicken Sie auf **Senden** , und notieren Sie den Antwortstatus **401 nicht autorisiert**.

![Senden][api-management-dev-portal-send-401]

Die Anforderung ist nicht berechtigt, weil die Back-End-API von Azure Active Directory geschützt ist. Vor dem erfolgreiche Aufrufen der API des Entwicklers muss Portal Entwickler mit OAuth 2.0 autorisieren konfiguriert sein. Dieses Verfahren wird in den folgenden Abschnitten beschrieben.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Registrieren der Entwicklerportal als AAD Anwendung

Der erste Schritt beim Konfigurieren der Entwicklerportal um Entwickler mit OAuth 2.0 autorisieren ist Entwicklerportal als Anwendung AAD registriert. Dies wird veranschaulicht, 8:27 im Video ab.

Navigieren Sie zu den Azure AD-Mandanten, aus dem ersten Schritt dieses Videos, in diesem Beispiel **APIMDemo** , und navigieren Sie zur Registerkarte **Applications** .

![Neue Anwendung][api-management-aad-new-application-devportal]

Klicken Sie auf die Schaltfläche **Hinzufügen** , um eine neue Azure Active Directory-Anwendung zu erstellen, und wählen Sie **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**.

![Neue Anwendung][api-management-new-aad-application-menu]

Wählen Sie **Web-Anwendung und/oder Web-API**aus, geben Sie einen Namen ein, und klicken Sie auf den Pfeil nach rechts. In diesem Beispiel wird die **APIMDeveloperPortal** verwendet.

![Neue Anwendung][api-management-aad-new-application-devportal-1]

Geben Sie die URL für Ihre API Verwaltungsdienst **Anmelden URL** und Anfügen `/signin`. In diesem Beispiel wird die **https://contoso5.portal.azure-api.net/signin **verwendet.

Geben Sie die URL für Ihre API Verwaltungsdienst **App-Id-URL** und einige eindeutigen Zeichen angefügt werden soll. Dies können alle gewünschten Zeichen und in diesem Beispiel wird die **https://contoso5.portal.azure-api.net/dp** verwendet. Wenn Sie die gewünschten **Eigenschaften App** konfiguriert sind, klicken Sie auf das Häkchen, um die Anwendung zu erstellen.

![Neue Anwendung][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfigurieren einer Autorisierung API Management OAuth 2.0

Im nächsten Schritt wird zu einen OAuth 2.0 Autorisierung Server-API Management konfigurieren. Dieser Schritt wird in das Video 9:43 ab veranschaulicht.

Klicken Sie im Menü API-Management auf der linken Seite auf **Sicherheit** auf **OAuth 2.0**, und klicken Sie dann auf **Hinzufügen Autorisierung** Server.

![Autorisierung Server hinzufügen][api-management-add-authorization-server]

Geben Sie in die Felder **Name** und **Beschreibung** einen Namen und optional eine Beschreibung ein. Diese Felder werden verwendet, um den OAuth 2.0 Autorisierung Server innerhalb der API Management Service-Instanz identifizieren. In diesem Beispiel wird die **Autorisierung Server Demo** verwendet. Später bei der Angabe eines OAuth 2.0-Servers für die Authentifizierung für eine API verwendet werden soll, wählen Sie aus dieser Name.

Geben Sie für die **URL der Registrierung Client** einen Platzhalterwert ein, wie `http://localhost`.  Die **URL der Registrierung Client** verweist auf die Seite, die Benutzer verwenden können, erstellen und konfigurieren ihre eigenen Konten für OAuth 2.0-Anbieter, die Benutzer die Verwaltung von Konten zu unterstützen. In diesem Beispiel Benutzer nicht erstellen und konfigurieren ihre eigenen Konten, dass ein Platzhalter verwendet werden.

![Autorisierung Server hinzufügen][api-management-add-authorization-server-1]

Geben Sie als Nächstes **Endpunkt-URL Autorisierung** und **Token Endpunkt-URL**ein.

![Autorisierung server][api-management-add-authorization-server-1a]

Diese Werte können von der Seite **App Endpunkte** der Anwendung AAD abgerufen werden, die Sie für die Entwicklerportal erstellt haben. Für den Zugriff auf die Endpunkte des navigieren Sie zur Registerkarte für die Anwendung AAD **Konfigurieren** , und klicken Sie auf **Ansicht Endpunkte**.

![Anwendung][api-management-aad-devportal-application]

![Anzeigen von Endpunkten][api-management-aad-view-endpoints]

Kopieren Sie den **OAuth 2.0 Autorisierung Endpunkt** , und fügen Sie ihn in das Textfeld **Autorisierung Endpunkt-URL** .

![Autorisierung Server hinzufügen][api-management-add-authorization-server-2]

Kopieren Sie den **token Endpunkt OAuth 2.0** , und fügen Sie ihn in das Textfeld **Token Endpunkt-URL** .

![Autorisierung Server hinzufügen][api-management-add-authorization-server-2a]

Zusätzlich zum Einfügen in den Endpunkt token, fügen Sie ein zusätzlichen Textkörper Parameter mit dem Namen **Ressource** und Veröffentlichen des Projekts Visual Studio wurde für die Verwendung des Werts der **App-ID-URI** aus der Anwendung AAD für die Back-End-Dienst, der erstellt wurde hinzu.

![App-ID-URI][api-management-aad-sso-uri]

Geben Sie dann die Clientanmeldeinformationen an. Dies sind die Anmeldeinformationen für die Ressource, die Sie zugreifen, die in diesem Fall den Back-End-Dienst möchten.

![Clientanmeldeinformationen][api-management-client-credentials]

Rufen Sie die **Client-Id**, navigieren Sie zur Registerkarte **Konfigurieren** der Anwendung AAD für die Back-End-Dienst, und kopieren Sie die **Client-Id**.

Zum Abrufen des **Client geheim** klicken **Wählen Sie eine Dauer** Dropdown-Liste im Abschnitt **Tasten** und ein Intervall festlegen. In diesem Beispiel wird 1 Jahr verwendet.

![Client-ID][api-management-aad-client-id]

Klicken Sie auf **Speichern** , um die Konfiguration zu speichern, und zeigen Sie die Taste. 

>[AZURE.IMPORTANT] Notieren Sie sich dieses Schlüssels. Nachdem Sie das Fenster Azure Active Directory-Konfiguration schließen, kann die Taste erneut angezeigt werden.

Kopieren Sie die Taste in die Zwischenablage, wechseln Sie zurück zu Publisher-Portal, fügen Sie die Taste in das Textfeld **Geheim Client** , und klicken Sie auf **Speichern**.

![Autorisierung Server hinzufügen][api-management-add-authorization-server-3]

Unmittelbar nach der Clientanmeldeinformationen ist eine Autorisierung Code erteilen. Kopieren Sie diesen Autorisierungscode und wechseln Sie wieder zur Ihrer Azure AD-Developer Portal Anwendung konfigurieren, Seite und Einfügen die Autorisierung erteilen in das Feld **Antwort-URL** ein, und klicken Sie erneut auf **Speichern** .

![Antwort-URL][api-management-aad-reply-url]

Im nächste Schritt müssen Sie die Berechtigungen für die Anwendung AAD-Entwicklerportal konfigurieren. Klicken Sie auf **Berechtigungen** , und aktivieren Sie das Kontrollkästchen für **die Directory Daten lesen**. Klicken Sie auf **Speichern** , um diese Änderung zu speichern, und klicken Sie dann auf **Anwendung hinzufügen**.

![Hinzufügen von Berechtigungen][api-management-add-devportal-permissions]

Klicken Sie auf das Suchsymbol, geben **APIM** in die Felder starten mit Feld, wählen Sie **APIMAADDemo**aus, und klicken Sie auf das Häkchen zu speichern.

![Hinzufügen von Berechtigungen][api-management-aad-add-app-permissions]

Klicken Sie auf **Delegierte Berechtigungen** für **APIMAADDemo** und aktivieren Sie das Kontrollkästchen für **Access APIMAADDemo**, und klicken Sie auf **Speichern**. Dadurch kann der Entwickler Portal Anwendung Zugriff auf die Back-End-Dienst.

![Hinzufügen von Berechtigungen][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Aktivieren Sie OAuth 2.0 Benutzer Autorisierung für die Gehaltsrechner-API

Nachdem Sie nun der OAuth 2.0-Server konfiguriert ist, können Sie es in der Sicherheitseinstellungen für Ihre API angeben. Dieser Schritt wird in das Video ab 14:30 veranschaulicht.

Klicken Sie im linken Menü auf **APIs** , und klicken Sie auf **Rechner** anzeigen und die zugehörigen Einstellungen konfigurieren.

![Rechner-API][api-management-calc-api]

Navigieren Sie zur Registerkarte **Sicherheit** , aktivieren Sie das Kontrollkästchen **OAuth 2.0** , wählen Sie den gewünschten Autorisierung Server aus der Dropdownliste **Autorisierung Server** , und klicken Sie auf **Speichern**.

![Rechner-API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Rufen Sie die Gehaltsrechner-API erfolgreich Entwicklerportal

Jetzt, da die Autorisierung OAuth 2.0 zur-API konfiguriert ist, können die Vorgänge aus dem Developer Center erfolgreich aufgerufen werden. Dieser Schritt wird in das Video ab, um 15:00 Uhr veranschaulicht.

Navigieren Sie zurück zu den Vorgang **Hinzufügen zwei ganze Zahlen** des Diensts Rechner im Portal Entwicklertools, und klicken Sie auf, **Probieren Sie es**. Beachten Sie den neuen Eintrag im Abschnitt **Autorisierung** entspricht dem Autorisierung Server, die, den Sie soeben hinzugefügt haben.

![Rechner-API][api-management-calc-authorization-server]

Wählen Sie aus der Dropdownliste Autorisierung **Autorisierungscode** aus, und geben Sie die Anmeldeinformationen für das Konto verwenden. Wenn Sie bereits mit dem Konto angemeldet sind, die Sie möglicherweise nicht aufgefordert werden.

![Rechner-API][api-management-devportal-authorization-code]

Klicken Sie auf **Senden** , und notieren Sie den **Status Antwort** **200 OK** und die Ergebnisse des Vorgangs in der Antwortinhalt.

![Rechner-API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Konfigurieren einer desktop-Anwendungs aufrufen, die-API

Im nächste Verfahren im Video beginnt mit 16:30 Uhr und eine einfache desktop-Anwendung aufrufen, die API konfiguriert. Der erste Schritt besteht im desktop-Anwendung in Azure AD registrieren, und probieren Sie es Access, zu dem Verzeichnis und zum Back-End-Dienst. 18:25 ist eine Vorführung der desktop-Anwendung einen Vorgang auf dem Rechner API aufrufen.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Konfigurieren einer Richtlinie Überprüfung JWT Anfragen vorab autorisieren

Im letzten Schritt des Videos beginnt mit 20:48, und es wird gezeigt, wie Sie die Richtlinie [JWT überprüfen](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) , um Besprechungsanfragen vorab zu autorisieren, indem Sie die Access-Token jeder eingehenden Anforderung. Wenn die Anfrage nicht von der Richtlinie JWT überprüfen überprüft wird, wird die Anfrage wird von Management-API blockiert und nicht an die Back-End-weitergeleitet wird.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

In einer anderen Vorführung der konfigurieren und verwenden diese Richtlinie, finden Sie unter [Cloud Deckblatt Folge 177: Weitere API Verwaltungsfunktionen](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) und schnelle Vorlauf, um 13:50. Wechsel, auf 15:00, um die Richtlinien, die so konfiguriert, dass die Richtlinie-Editor finden Sie unter und dann auf 18:50 Aufrufen eines Vorgangs aus der Entwicklerportal mit und ohne das erforderliche Autorisierungstoken veranschaulicht.

## <a name="next-steps"></a>Nächste Schritte
-   Schauen Sie sich weitere [Videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) über API Management.
-   Andere Methoden zum Sichern Ihrer Back-End-Diensts finden Sie unter [Authentifizierung gemeinsamen Zertifikat](api-management-howto-mutual-certificates.md) und [Verbinden über VPN oder ExpressRoute](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Verwalten Sie Ihre erste API]: api-management-get-started.md
