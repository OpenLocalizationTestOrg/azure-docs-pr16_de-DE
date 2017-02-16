<properties
   pageTitle="Aktuelle Vorschau Einschränkungen für die Zusammenarbeit Azure Active Directory B2B | Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure AD B2B Zusammenarbeit Vorschau: aktuelle Vorschau Einschränkungen

- Mehrstufige Authentifizierung (MFA) für externe Benutzer nicht unterstützt. Beispielsweise können nicht Wenn Contoso verfügt über MFA, Partner Organigramm jedoch nicht, dann Partner Organisations-Benutzer MFA durch B2B Zusammenarbeit gewährt werden.
- Einladungen sind nur über die CSV möglich. einzelne Einladungen und API-Zugriff wird nicht unterstützt.
- Azure AD globale Administratoren können nur CSV-Dateien hochladen.
- Von Einladungen an Consumer e-Mail-Adressen (z. B. hotmail.com, Gmail.com oder für comcast.net) werden derzeit nicht unterstützt.
- Externe Benutzerzugriff auf lokale Applikationen nicht getestet.
- Externe Benutzer sind nicht automatisch bereinigt, wenn der jeweilige Benutzer aus ihrem Verzeichnis gelöscht wird.
- Einladungen an Verteilerlisten werden nicht unterstützt.
- Bis zu 2.000 Datensätze kann über CSV hochgeladen werden.

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Weitere Artikel auf Azure AD B2B für die Zusammenarbeit an:

- [Was ist für die Zusammenarbeit Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [So funktioniert es](active-directory-b2b-how-it-works.md)
- [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Token Format für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
- [Externe Benutzer Objekt Attribut Änderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
