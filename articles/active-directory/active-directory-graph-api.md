<properties
   pageTitle="Azure-Active Directory-Diagramm API | Microsoft Azure"
   description="Eine Übersicht und Schnellstart Leitfaden für die Graph-API die programmgesteuerten Zugriff auf Azure AD über REST-API Endpunkte ermöglicht."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure-Active Directory-Diagramm-API

> [AZURE.IMPORTANT] Azure AD-Graph-API Funktionalität steht auch über [Microsoft Graph](https://graph.microsoft.io/), eine einheitliche API, die APIs von anderen Microsoft-Diensten, wie etwa OneDrive, Outlook, OneNote, Planer und Office Graph an, der Zugriff über einen einzelnen Endpunkt und mit einem einzelnen Token enthält.

Die Azure Active Directory Graph-API bietet programmgesteuerten Zugriff auf Azure AD über REST-API Endpunkte. Applikationen können mithilfe der Graph-API ausführen erstellen, lesen, aktualisieren und löschen Sie Vorgänge wie Directory-Daten und Objekte. Die Graph-API unterstützt beispielsweise die folgenden allgemeinen Vorgänge für ein Benutzerobjekt:

- Erstellen Sie einen neuen Benutzer im Verzeichnis

- Abrufen von detaillierten Eigenschaften eines Benutzers, beispielsweise ihre Gruppen

- Aktualisieren der Eigenschaften eines Benutzers, z. B. deren Position und Rufnummer, oder Ändern ihres Kennworts

- Überprüfen Sie die Gruppenmitgliedschaft eines Benutzers, für den Zugriff rollenbasierte

- Deaktivieren Sie Konto eines Benutzers oder löschen Sie ihn vollständig

Zusätzlich zu Benutzerobjekten können Sie ähnlich wie Vorgänge an anderen Objekten wie Gruppen und Anwendungen durchführen. Um die Graph-API auf ein Verzeichnis aufzurufen, die Anwendung muss bei Azure AD registriert sein und konfiguriert werden, um den Zugriff auf das Verzeichnis zulassen. Dies wird normalerweise durch einen Benutzer oder Administrator Zustimmung Fluss erreicht.

Beginnen mit der Azure Active Directory Graph-API, finden im [Diagramm-API Schnellstarthandbuch](active-directory-graph-api-quickstart.md)oder Anzeigen der [Dokumentation zur interaktiven Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Features

Die Graph-API bietet die folgenden Features:

- **REST-API Endpunkte**: das Graph-API ist ein Rest-Diensts besteht aus Endpunkten, die mit der standardmäßigen HTTP-Anfragen zugegriffen werden. Die Graph-API unterstützt XML- oder Javascript Object Notation (JSON) Inhaltstypen für Besprechungsanfragen und Antworten an. Weitere Informationen finden Sie unter [Azure AD Graph REST-API-Referenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Authentifizierung mit Azure AD**: jeder Anforderung an das Graph-API durch Anfügen einer JSON Web Token (JWT) in der Kopfzeile Autorisierung der Anfrage authentifiziert werden muss. Dieses Token wird erworben, indem er eine Anforderung an token Azure AD-Endpunkt und Bereitstellen gültige Anmeldeinformationen. Können OAuth 2.0 Client Anmeldeinformationen illustrieren oder Autorisierung erteilt Fluss, ein Token zum Aufrufen des Diagramms zu erhalten. Weitere Informationen, [OAuth 2.0 in Azure Active Directory](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Rollenbasierte Autorisierung (RBAC)**: Sicherheitsgruppen zum Ausführen von RBAC in der Graph-API verwendet werden. Angenommen, wenn Sie feststellen, ob ein Benutzer auf eine bestimmte Ressource zugreifen möchten, kann die Anwendung den Vorgang [Gruppenmitgliedschaft aktivieren (transitiven)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) aufrufen, der WAHR oder falsch zurückgibt.

- **Differenz Abfrage**: Wenn Sie prüfen, ob Änderungen im Verzeichnis zwischen zwei Perioden ohne häufig verwendete Abfragen an die Graph-API vornehmen möchten, können Sie die Anforderung einer Abfrage Differenz vornehmen. Diese Art der Anforderung zurückgegeben werden nur die zwischen der vorherigen Differenz Abfrage Anforderung und die aktuelle Anforderung vorgenommenen Änderungen kann. Weitere Informationen finden Sie unter [Azure AD Graph-API Differenz Abfrage](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **Directory-Erweiterungen**: Wenn Sie eine Anwendung, die Lese-oder Schreibzugriff eindeutige Eigenschaften für Directory-Objekte muss entwickeln, können Sie registriert und Erweiterung Werte mithilfe der Graph-API verwenden. Beispielsweise wenn die Anwendung eine Skype-ID-Eigenschaft für jeden Benutzer erfordert, können Sie die neue Eigenschaft im Verzeichnis registrieren, und es werden auf jedes Benutzerobjekt zur Verfügung. Weitere Informationen finden Sie unter [Azure AD Graph-API Directory Schema Erweiterungen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Sicheres durch Berechtigung Bereiche**: AAD Graph-API macht Berechtigung Bereiche, mit denen Zugriff auf AAD Daten secure/zugestimmt und unterstützen eine Vielzahl von Clients app Dateitypen, einschließlich:
 - die mit dem gewährt werden-Benutzeroberfläche delegiert Access mit Daten über die Autorisierung von der angemeldeten Benutzer (delegierte)
  - Verwenden Sie, die definieren Anwendung-rollenbasierte Access-Steuerelement, z. B. Dienst/Dämon Clients (app Rollen)

    Sowohl delegiert und app Rolle über die Berechtigung Bereiche darstellen einer Rechte von der Graph-API verfügbar gemacht und von Clientanwendungen durch Anwendung Registrierung Berechtigungen [Features in der klassischen Azure-Portal](https://manage.windowsazure.com)angefordert werden können. Clients können überprüfen die Bereiche über die Berechtigung erteilt können durch den Bereich "" (scp) Anspruch im Access Token für delegierte Berechtigungen erhalten prüfen und die Rollen ("Rollen") beanspruchen app Rolle Berechtigungen. Weitere Informationen zu [Azure AD Graph-API Berechtigung Bereiche](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Szenarien

Die Graph-API ermöglicht viele Anwendungsszenarien. Die folgenden Szenarien werden die am häufigsten verwendeten:

- **Line of Business (einzelne Mandant) Anwendung**: In diesem Szenario funktioniert ein Enterprise-Entwickler für eine Organisation, die ein Office 365-Abonnement enthält. Der Entwickler ist einer Anwendung, die mit Azure AD für Aufgaben interagiert erstellen und solche Zuordnen einer Lizenz für einen Benutzer. Diese Aufgabe erfordert Zugriff auf das Graph-API, damit die Entwicklertools Register Anwendung in Azure AD-Mandanten und konfiguriert das einzelne Lese- und für die Graph-API Schreibberechtigungen. Und dann die Anwendung konfiguriert ist, um eine eigene Anmeldeinformationen oder des Benutzers anmelden aktuell verwenden, um ein Token, um die Graph-API aufzurufen zu erwerben.

- **Software als Service-Anwendung (mit mehreren Mandanten)**: In diesem Szenario ist eine unabhängige Software Softwareanbietern () gehosteten mit mehreren Mandanten Webanwendung, die Benutzer Verwaltungsfunktionen für andere Organisationen bereitstellt, mit denen Azure AD entwickeln. Diese Features erfordern den Zugriff auf Directory-Objekte, und die Anwendung muss also die Graph-API aufzurufen. Der Entwickler registriert die Anwendung in Azure AD, zur erfordern gelesen und Schreibberechtigungen für die Graph-API konfiguriert und dann ermöglicht externen Zugriff, damit Ihr Einverständnis anderen Organisationen in ihrem Verzeichnis die Anwendung verwenden können. Wenn ein Benutzer in anderen Organisationen mit der Anwendung zum ersten Mal authentifiziert, werden ein Zustimmungsdialogfeld mit den Berechtigungen für angezeigt, die Anwendung erfordert ist.  Gewährt wird, erteilen Erlaubnis klicken Sie dann die Anwendung angefordert die Berechtigungen zur Verwaltung der Graph-API im Verzeichnis des Benutzers. Weitere Informationen über das Framework Zustimmung finden Sie unter [Übersicht über Framework Zustimmung](active-directory-integrating-applications.md).

## <a name="see-also"></a>Siehe auch

[Schnellstarthandbuch Azure AD-Graph-API](active-directory-graph-api-quickstart.md)

[Anzeige Graph REST Dokumentation](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory developer's guide](active-directory-developers-guide.md)
