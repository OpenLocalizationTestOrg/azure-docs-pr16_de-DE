<properties
   pageTitle="Azure AD B2B Zusammenarbeit Vorschau: Funktionsweise | Microsoft Azure"
   description="Erläutert, wie Azure Active Directory B2B Zusammenarbeit Business Partner an Ihre corporate Applikationen Selektives Zugriff auf Ihre unternehmensweit Beziehungen unterstützt"
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

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure AD B2B Zusammenarbeit Vorschau: Funktionsweise
Azure AD B2B Zusammenarbeit basiert auf einer einladen und Modell einlösen. Sie bieten die e-Mail-Adressen der Parteien gewünschten mit entwickelt, zusammen mit der Anwendung Sie verwenden sollen. Azure AD sendet ihnen eine e-Mail-Einladung mit einem Link. Der Benutzer Partner der Hyperlink und wird aufgefordert, melden Sie sich mit ihrer Azure AD-Konto oder melden Sie sich können eine neue Azure Anzeige zu berücksichtigen.

1. Ihr Administrator lädt Partnerbenutzer durch Hochladen [einer strukturierten CSV-Datei](active-directory-b2b-references-csv-file-format.md) mit der Azure-Portal an.
2. Sendet der Portalseite Laden Sie diesen Benutzern Partner-e-Mails.
3. Partnerbenutzer klicken Sie auf den Link in der e-Mail, und Sie werden aufgefordert, melden Sie sich mit ihrer Arbeit Anmeldeinformationen (Wenn sie bereits in Azure AD sind) oder als Benutzer für die Zusammenarbeit Azure AD B2B registrieren.
4. Partnerbenutzer werden mit der Anwendung umgeleitet, die, denen diese, eingeladen wurden, in dem sie jetzt zugreifen können.

## <a name="directory-operations"></a>Directory-Vorgänge
Partnerbenutzer, die in Ihrer Azure AD als externe Benutzer vorhanden sind. Diese bedeutet, dass Ihr Administrator Bereitstellung von Lizenzen, Gruppenmitgliedschaft zuweisen und weiteren gewähren des Zugriffs auf corporate apps durch Azure-Portal oder Azure PowerShell einfach wie für Benutzer in Ihrem Unternehmen verwenden kann.

Während ein kostenpflichtiges Azure AD Abonnement (Basic oder Premium) ist nicht erforderlich, Azure AD B2B, Mandanten verwenden, die ein kostenpflichtiges verfügen Azure AD-Abonnements (Basic oder Premium) erhalten Sie die folgenden zusätzlichen Vorteile:

 - Administratoren können apps, Gruppen zuweisen bereitstellen für einfacher Verwaltung des Benutzerzugriffs eingeladen.
 - Branding Admin-Mandanten wird verwendet, um die Einladung e-Mails Branding und Rückzahlung zur Verfügung steht, um mehr Kontext bereitstellen eingeladen Partnerbenutzer.

## <a name="related-articles"></a>Verwandte Artikel
 Durchsuchen von den anderen Artikeln zu Azure AD B2B für die Zusammenarbeit

 - [Was ist für die Zusammenarbeit Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
 - [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
 - [Token Format für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
 - [Externe Benutzer Objekt Attribut Änderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Aktuelle Vorschau Einschränkungen](active-directory-b2b-current-preview-limitations.md)
 - [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
