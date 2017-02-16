<properties
   pageTitle="Externe Benutzer Objekt Attribut Änderungen für die Zusammenarbeit Preview Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B unterstützt Ihrer Beziehungen unternehmensweit Business Partner an Ihre corporate Applikationen Selektives Zugriff auf"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure AD B2B Zusammenarbeit Vorschau: externe Benutzer Objekt Attribut Änderungen

Jeder Benutzer in einem Azure AD-Verzeichnis wird von einem Objekt dargestellt. Das Benutzerobjekt in Azure AD-durchläuft Attribut Änderungen in verschiedenen Phasen der Zusammenarbeit an Dokumenten B2B einladen-einlösen Fluss. Die Benutzer Objekt darstellen der Partner Benutzer im Verzeichnis Attribute verfügt, die geändert am einlösen Zeit, wenn der Partner-Benutzer auf den Link in der e-Mail einladen klickt. Insbesondere:

- Die Attribute **SignInName** und **AltSecId** werden aufgefüllt.
- Das **DisplayName** -Attribut von Änderungen aus Benutzerprinzipalnamen (user_fabrikam.com#EXT#@contoso.com) auf den Namen Anmeldung(user@fabrikam.com)

Überwachen diese Attribute in Azure AD bei der Problembehandlung hilfreich, und zwar unabhängig davon, ob ein Partner-Benutzer ihre Einladung zur Zusammenarbeit von B2B eingelöst hat.

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Weitere Artikel auf Azure AD B2B für die Zusammenarbeit an:

- [Was ist für die Zusammenarbeit Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [So funktioniert es](active-directory-b2b-how-it-works.md)
- [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Token Format für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
- [Aktuelle Vorschau Einschränkungen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
