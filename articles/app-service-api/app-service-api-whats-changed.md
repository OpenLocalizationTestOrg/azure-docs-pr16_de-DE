<properties
    pageTitle="Was sich geändert wird der App-Verwaltungsdienst API Apps - | Microsoft Azure"
    description="Erfahren Sie, API Apps in Azure-App-Verwaltungsdienst Neuigkeiten."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>App-Verwaltungsdienst API Apps – was geändert wird

Bei der Veranstaltung Connect() im November 2015 wurden eine Zahl von Verbesserungen Azure-App-Verwaltungsdienst [angekündigt](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Diese Verbesserungen gehören zugrunde liegenden Änderungen API Apps besser ausrichten mit Mobile und Web Apps, verringern des Konzepts zählen und Verbessern der Bereitstellung und der Leistung zur Laufzeit. Neue API-apps 30 November 2015, beginnend mit der Azure-Verwaltungsportal erstellten oder der neuesten Werkzeuge wider diese Änderungen. In diesem Artikel werden die betreffenden Änderungen sowie wie vorhandene apps, um die Funktionen nutzen erneut bereitgestellt.

## <a name="feature-changes"></a>Feature Änderungen
Die wichtigsten Features von API Apps – Authentifizierung, CORS und API Metadaten – wurden direkt in der App-Dienst verschoben. Mit dieser Änderung gespeichert haben sind die Features über das Web, Mobile und API Apps verfügbar. Alle drei freigeben tatsächlich den gleichen **Microsoft.Web/sites** Ressourcentyp in Ressourcenmanager. Das Gateway-API Apps ist nicht mehr erforderlich oder mit API Apps angeboten. Dies vereinfacht auch Azure-API Management verwendet werden, da es nur das einzelne API Management Gateway werden.

![Übersicht über die API-Apps](./media/app-service-api-whats-changed/api-apps-overview.png)

Ein Entwurfsprinzip Key in der Apps-API Aktualisierung besteht darin, Sie Ihre API ungeändert in einer Sprache Ihrer Wahl wieder abrufen können.  Wenn Ihre API bereits als Web App oder Mobile-App bereitgestellt wird, müssen Sie keinen der app, um die neuen Features nutzen erneut bereitstellen. Wenn Sie derzeit in der Preview-API Apps sind, ist die Migration – Anleitung nachfolgend detailliert beschrieben.

### <a name="authentication"></a>Authentifizierung
Die vorhandenen sofort einsatzbereite API Apps, Mobile Services/Apps und Web Apps-Authentifizierungsfeatures haben unified wurde und stehen in einer einzelnen Azure App Service-Authentifizierung-Karte vorausgesetzt im Verwaltungsportal. Eine Einführung in Authentifizierung Services im App-Dienst, finden Sie unter [Erweitern App-Verwaltungsdienst-Authentifizierung / Autorisierung](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Für API Szenarien gibt es eine Reihe von relevanten neue Funktionen:

- **Unterstützung für die Verwendung von direkt Azure Active Directory**, ohne das AAD Token für eine Sitzungstoken exchange müssen Client-Code: Ihren Kunden kann nur die AAD Token in der Kopfzeile Autorisierung einbeziehen, entsprechend der Person token Spezifikation. Dies bedeutet auch, dass auf der Seite Client- oder keine App Service-spezifische SDK erforderlich ist. 
- **Dienst oder "Internal" Access**: Wenn Sie Dämonprozess oder einem anderen Client, benötigen Zugriff auf APIs ohne Benutzeroberfläche verfügen, Sie können ein Token mithilfe einer AAD Dienst Tilgungsanteile anfordern und App-Dienst für die Authentifizierung mit Ihrer Anwendung übergeben.
- **Verzögerte Autorisierung**: viele Clientanwendungen haben mit wechselnder zugriffseinschränkungen für verschiedene Teile der Anwendung. Möglicherweise möchten Sie einige APIs öffentlich verfügbar sein, während andere Anmeldung erforderlich machen. Das ursprüngliche Authentifizierung/Autorisierung Feature wurde nichts, mit der gesamte Website mit Anforderung der Anmeldung. Diese Option noch vorhanden ist, aber Sie können auch Ihrer Anwendungscode Access Entscheidungen gerendert nach der Benutzerauthentifizierung App-Dienst.
 
Weitere Informationen zu den neuen Authentifizierungsfeatures finden Sie unter [Authentifizierung und Autorisierung für API Apps in Azure-App-Dienst](app-service-api-authentication.md). Informationen zum Migrieren von vorhandenen API apps aus dem vorherigen API apps Modell in das neue, finden Sie unter [Migrieren von vorhandenen API apps](#migrating-existing-api-apps) weiter unten in diesem Artikel.
 
### <a name="cors"></a>CORS
Anstelle eines Kommas als Trennzeichen **MS_CrossDomainOrigins** app Einstellung gibt es nun eine Blade im Azure-Verwaltungsportal für das Konfigurieren von CORS. Alternativ können sie konfiguriert werden mithilfe von Tools wie Azure PowerShell, CLI oder [Ressource Explorer](https://resources.azure.com/)Ressourcenmanager. Legen Sie die Eigenschaft **Cors** auf den **Microsoft.Web/sites/config** Ressourcentyp für Ihre ** &lt;Websitenamen&gt;/web** Ressource. Beispiel:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-Metadaten
Das API Definition Blade ist jetzt über das Web, Mobile und API Apps zur Verfügung. Im Verwaltungsportal können Sie entweder eine relative Url oder eine absolute Url auf diese Hosts eines Swagger 2.0 Darstellung Ihrer API-einen Endpunkt angeben. Alternativ können sie konfiguriert sein Ressourcenmanager Tools verwenden. Legen Sie die Eigenschaft **ApiDefinition** auf den **Microsoft.Web/sites/config** Ressourcentyp für Ihre ** &lt;Websitenamen&gt;/web** Ressource. Beispiel:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Zu diesem Zeitpunkt muss der Metadaten-Endpunkt ohne Authentifizierung für viele untergeordneten Clients (z. B. Visual Studio REST-API Client Generation und PowerApps "hinzufügen API" Fluss) es nutzen öffentlich zugänglich sein. Dies bedeutet Wenn Sie die App-Authentifizierung verwenden und die Definition API aus innerhalb der app ähneln verfügbar machen möchten, müssen Sie die Option zurückgestellt-Authentifizierung, damit der Routing-Metadaten entspricht der Swagger öffentlich ist zuvor beschriebenen verwenden.

## <a name="management-portal"></a>Verwaltungsportal
Auswählen von **Neu > Web + Mobile > API App** im Portal API-apps, die die neuen Funktionen, die in diesem Artikel beschriebenen widerspiegeln erstellen. **Durchsuchen > API Apps** zeigt nur die neuen API apps. Nachdem Sie in einer app API durchsuchen, teilt das Blade das gleiche Layout und Funktionen, mit denen der Web- und Mobile-Apps. Die einzige Unterschiede sind Schnellstart Inhalt und Sortierung der Einstellungen.

Vorhandene API-apps (oder Marketplace-API apps Logik Apps Dokumentvorlagen) mit der vorherigen Vorschau Funktionen werden weiterhin sichtbar im Designer Logik Apps und beim Durchsuchen aller Ressourcen in einer Ressourcengruppe.

## <a name="visual-studio"></a>Visual Studio

Die meisten Web Apps Tools funktionieren mit neuen API apps, da sie den gleichen zugrunde liegenden **Microsoft.Web/sites** Ressourcentyp freigeben. Azure Visual Studio Werkzeuge, sollten aktualisierte Version 2.8.1 oder höher, da sie eine Reihe von Funktionen, die speziell für die APIs macht jedoch zu. Das SDK von der [Azure-Downloadseite](https://azure.microsoft.com/downloads/)herunterladen.

Veröffentlichen Sie mit der Rationalisierung der App Typen, ist auch unter unified **Veröffentlichen > Microsoft Azure-App-Verwaltungsdienst**:

![Veröffentlichen von API-Apps](./media/app-service-api-whats-changed/api-apps-publish.png)

Lesen Sie weitere Informationen zum SDK 2.8.1 [Blogbeitrag](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)Ankündigung ein.

Alternativ können Sie das Veröffentlichungsprofil aus dem Verwaltungsportal veröffentlichen aktivieren manuell importieren. Jedoch Cloud-Explorer, Code Generation und API app Auswahl/Erstellung erfordert SDK 2.8.1 oder höher.

## <a name="migrating-existing-api-apps"></a>Migrieren von vorhandenen API-apps
Wenn Ihre benutzerdefinierten API auf der vorherigen Vorschauversion von Apps-API bereitgestellt wird, bitten Sie in das neue Modell für API-Apps mit 31 Dezember 2015 migrieren. Da sowohl die alten und neuen Modell auf Web-APIs in der App-Dienst gehostet basieren, kann die meisten vorhandenen Code wiederverwendet werden.

### <a name="hosting-and-redeployment"></a>Gehostet und erneute Bereitstellung
Die Schritte zum erneuten Bereitstellen sind identisch mit jeder vorhandenen Web-API App-Dienst bereitstellen. Schritte aus:

1. Erstellen einer leeren API-app an. Dies kann im Portal mit neu > App-API in Visual Studio aus veröffentlichen oder aus Ressourcenmanager Tools. Wenn Ressourcenmanager Werkzeuge oder Vorlagen verwenden zu können, legen Sie den Wert **Art** **-API** , auf die Ressourcenart **Microsoft.Web/sites** das Schnellstart und die Einstellungen im Verwaltungsportal Orientierung in Richtung API Szenarien aufweisen.
2. Verbinden Sie und Bereitstellen Sie des Projekts mit der leeren API verhindert Deployment-Verfahren, die vom Dienst App unterstützt werden. Lesen Sie [Azure-App-Verwaltungsdienst Bereitstellungsdokumentation](../app-service-web/web-sites-deploy.md) Weitere Informationen. 
  
### <a name="authentication"></a>Authentifizierung
Die App-Dienst Authentifizierungsdienste unterstützen die gleichen Funktionen, die mit dem vorherigen API Apps Modell verfügbar waren. Wenn Sie die Sitzungstoken verwenden und SDKs erforderlich, verwenden Sie die folgenden Client- und Server-SDKs:

- Client: [Azure Mobilclient SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Server: [Microsoft Azure Mobile-App .NET Authentifizierung Erweiterung](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Wenn Sie stattdessen die App-Dienst Alpha SDKs verwendet haben, sind diese jetzt veraltet:

- Client: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Insbesondere ist mit Azure Active Directory, jedoch keine App-spezifischen erforderlich, wenn Sie das Token AAD direkt verwenden.

### <a name="internal-access"></a>Internen Zugriff
Vorherige API Apps Modell enthalten eine integrierte interne Zugriffsebene festlegen. Dies erforderlich Verwenden des SDK zum Signieren Serviceanfragen. Wie zuvor beschrieben mit dem neuen API Apps Modell können AAD Dienst Hauptbenutzer eine Alternative für Dienst-Authentifizierung verwendet werden ohne eine App-Service-spezifische SDK. Erfahren Sie mehr im [Hauptbenutzer Authentifizierung für API Apps im App-Verwaltungsdienst Azure Service](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Suche
Das vorherige API Apps Modell hatten APIs für andere API apps zur Laufzeit in derselben Ressourcengruppe hinter demselben Gateway entdecken. Dies ist besonders hilfreich in Architekturen, die Microservice Muster implementieren. Während Sie dies nicht direkt unterstützt wird, gibt eine Reihe von Optionen zur Verfügung:

1. Verwenden Sie für die Ermittlung der Azure Ressourcenmanager-API.
2. Setzen Sie Azure-API Management vor der App-Dienst gehostete APIs. Azure-API Management dient als Fassade und können eine unveränderliche externen zugänglichen Url bereitstellen, auch wenn internen Suchtopologie ändert.
3. Erstellen Sie Ihre eigenen Discovery-API-Anwendung, und haben Sie andere API-apps mit der Discovery-app auf Start registriert.
4. Füllen Sie zum Zeitpunkt der Bereitstellung der app-Einstellungen für den gesamten API apps (und Clients) mit den Endpunkten des anderen API apps. Dies ist in Vorlage Bereitstellungen und da API Apps Sie jetzt erteilen Kontrolle über die Url.

## <a name="using-api-apps-with-logic-apps"></a>Verwenden von API-Apps mit Logik Apps

Das neue API apps Modell funktioniert auch mit [Logik Apps Schemaversion 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie weitere Artikel im [Abschnitt API Apps Dokumentation](https://azure.microsoft.com/documentation/services/app-service/api/)ein. Diese entsprechend das neue Modell für API Apps aktualisiert. Darüber hinaus erreichen Sie in den Foren Weitere Details oder Anleitungen Migration:

- [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Stapelüberlauf](http://stackoverflow.com/questions/tagged/azure-api-apps)
