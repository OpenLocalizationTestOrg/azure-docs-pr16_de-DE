<properties
   pageTitle="Vergleich der Funktionen für die Verwaltung von externen Identitäten Azure Active Directory mithilfe von | Microsoft Azure"
   description="Vergleicht Azure Active Directory B2B Zusammenarbeit, B2C und mehrere Mandanten App für die Unterstützung der Authentifizierung und Autorisierung für externe Identitäten"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Vergleich der Funktionen für die Verwaltung von externen Identitäten mit Azure Active Directory

Azure Active Directory (Azure AD) können zusätzlich zum Verwalten von mobilen Mitarbeiter Zugriff auf SaaS apps, Ihrer Organisation Freigeben von Ressourcen für Business-Partner zur Verfügung und bieten eine Anwendung Unternehmen und Nutzer helfen.

## <a name="developing-applications-for-businesses"></a>Zur Entwicklung von Applications für Unternehmen

Bieten Sie einem Dienst oder einer Anwendung, wie z. B. eines Diensts Payroll zu Unternehmen? Azure AD stellt die Identitätsplattform bereit, mit dem Sie Applikationen zu erstellen, die nahtlos mit Millionen von Organisationen zu integrieren, die bereits Azure AD als Teil der Bereitstellung von Office 365 oder einem anderen Enterprise-Dienst konfiguriert haben.

**Beispiel:** Eine pharmazeutische Distributor bietet medizinische benötigtes Material und IT-Abteilung zu Gesundheitswesen. Diese Anwendung Analytics medizinische Methoden und gewünschte Kunden zum Verwalten ihrer eigenen Identitäten Feld erforderlich sind. Diese Firma dorthin Azure AD als Identitätsplattform für ihre Methode Management-Anwendung, Azure AD-Identitäten bei Bedarf an ihre Kunden at-Zeichen von bereitstellen. Weitere Informationen finden Sie unter [Azure Active Directory developer's Guide](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Aktivieren Business Partnerzugriff auf Ihre Ihres Unternehmens Ressourcen

Haben Sie Business Partner oder andere Personen außerhalb des Unternehmens, die auf Ihre Enterprise-Ressourcen, wie etwa einer SharePoint-Website oder Ihrem ERP-System zugreifen? Azure AD ermöglicht Administratoren, dass externe Benutzer (die sich möglicherweise oder möglicherweise nicht in Azure AD vorhanden), melden Sie sich beim Zugriff auf corporate Applikationen einzelner gewähren. Dadurch wird die Sicherheit verbessert, wie Benutzer Access gesperrt, wenn sie die Partnerorganisation verlassen, während Sie die Access-Richtlinien innerhalb Ihrer Organisation steuern. Dies vereinfacht auch die Verwaltung, wie Sie benötigen, um eine externe Partner Verzeichnis oder pro Partner Föderation Beziehungen verwalten.

**Beispiel:** Ein imaging Unternehmen Einzelhändler Foto imaging Services bietet und das größte Retail Netzwerk von drucken Kioske in Betrieb sind. Sie entschieden sich Azure AD aktivieren Tausende von Benutzern in deren Retail Business Partner eigene Anmeldeinformationen zum Herunterladen der neuesten Partner marketing-Materials und Neuanordnen von Fotos benötigtes Material aus des Unternehmens Lieferanten extranet Verarbeitung verwendet. Weitere Informationen finden Sie unter [Zusammenarbeit Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Zur Entwicklung von Applications für Verbraucher

Benötigen Sie sicher und kostengünstiger online Anwendungen, wie eine Retail Store Vorderseite, mehrere Millionen Nutzer veröffentlichen? Azure AD bietet eine Plattform für soziale Netzwerke sowie Benutzername und Kennwort anmelden aktivieren, firmenspezifischen Self-Service anmelden und Self-service-Kennwort zurücksetzen für Nutzer von Ihrer Anwendung. Dies erhöht Komfort für Ihre Consumer beim Laden der Entwickler zu verringern.

**Beispiel:** Die \#1 Sport Franchise in der Welt zum direkt mit Ventilatoren 450 Millionen populärer erforderlich sind. Hierzu erstellt diese eine mobile app Azure AD-für Benutzer Anmelde- und Profil-Speicher. Lüftung vereinfachte Registrierung abrufen und durch die Verwendung von Konten für soziale wie Facebook anmelden, oder verwenden traditionelle Benutzernamen/Kennwörter für eine nahtlose Verwendung über iOS, Android und Windows Telefone. Klicken Sie auf das gängige erstellen Azure AD-Plattform benutzerdefinierten Code erheblich reduziert während zugewiesen Franchise branding und wenn man Bedenken hinsichtlich der Sicherheit, und aus Skalierbarkeit angepasst. Weitere Informationen finden Sie unter [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Vergleich von Azure AD-Funktionen

Jeder der bereits in diesem Artikel beschriebenen Szenarien ist von Funktionen innerhalb von Azure AD berücksichtigt. In dieser Tabelle hilfreiche Informationen verdeutlichen möchten, welche Funktionen, die für Sie relevant sind:

| **Erwägen Sie dieses Produkt...**       | [Azure AD-Mandanten mit mehreren SaaS app](active-directory-developers-guide.md)    | [Azure AD B2B für die Zusammenarbeit](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Wenn ich möchte angeben...** | einen Dienst für Unternehmen | Partnerzugriff auf Meine apps  | einer Dienstleistung für Verbraucher |
| **Ich bin ähnliche und...**  | Pharma distributor      | Imaging Unternehmen            | Sport franchise       |
| **Bereitstellen einer app für...**  | Verwaltung     | Lieferanten extranet          | Fußballfans            |
| **Verwendet...**        | Arztpraxis Büros        | Genehmigten Business Partner | Jede Person mit e-Mail      |
| **Zugegriffen werden, wenn...**      | Kunden-Admin einwilligt | Meine Administrator Einladungen           | Der Consumer anmeldet      |
