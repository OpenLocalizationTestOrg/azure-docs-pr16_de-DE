<properties
   pageTitle="Externe Benutzer token Format für die Zusammenarbeit Preview Azure Active Directory B2B | Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Azure AD B2B Zusammenarbeit Vorschau: token Format für externe Benutzer

Die Ansprüche für eine standard Azure AD Token im Artikel [Token unterstützt und Ansprüche](active-directory-token-and-claims.md) auf azure.microsoft.com beschrieben werden.

Die Ansprüche, die für einen authentifizierten B2B Zusammenarbeit mit externen Benutzer unterscheiden sind wie folgt aus:<br/>
- **OID:** die Objekt-ID aus den Mandanten Ressource<br/>
- **TID**: Mandanten ID aus den Mandanten Ressource<br/>
- **Herausgeber**: Dies ist die Ressource Mandanten<br/>
- **IDP**: Hierbei handelt es sich um den Start Mandanten des Benutzers<br/>
- **AltSecId**: Dies ist die alternative Sicherheits-ID, die Ihnen undurchsichtig ist<br/>

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Weitere Artikel auf Azure AD B2B für die Zusammenarbeit an:

- [Was ist für die Zusammenarbeit Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [So funktioniert es](active-directory-b2b-how-it-works.md)
- [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Externe Benutzer Objekt Attribut Änderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuelle Vorschau Einschränkungen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
