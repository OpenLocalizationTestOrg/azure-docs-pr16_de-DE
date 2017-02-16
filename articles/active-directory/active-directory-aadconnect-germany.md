<properties
    pageTitle="Azure AD in Deutschland Microsoft Cloud verbinden"
    description="Azure AD verbinden wird die lokalen Verzeichnissen mit Azure Active Directory integrieren. Dies können Sie eine allgemeine Identität für Office 365, Azure und SaaS Applikationen mit Azure AD integriert bereitstellen."
    keywords="Einführung in Azure AD-verbinden, Installieren von Azure AD verbinden Übersicht, was Azure AD-verbinden, ist active Directory, Deutschland, Schwarz Gesamtstruktur"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Verbinden von Azure AD in Deutschland für Microsoft Cloud - Public Preview-Version

## <a name="introduction"></a>Einführung
Verbinden von Azure AD bietet Synchronisierung zwischen der lokalen Active Directory und Azure Active Directory.
Derzeit müssen viele der Szenarios in [Microsoft Cloud-Deutschland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) durch den Operator ausgeführt wird. Wenn Microsoft Cloud-Deutschland verwenden zu können, müssen Sie beachten Sie Folgendes sein:


- Die folgenden URLs müssen auf einen Proxyserver für die Synchronisierung erfolgreich durchgeführt geöffnet werden:
    - *. microsoftonline.de
    - *. Windows
    - + Zertifikatsperrlisten

- Wenn Sie in Ihrem Verzeichnis Azure AD-anmelden, müssen Sie ein Konto in der Domäne onmicrosoft.de verwenden.
- Die folgenden Features sind nicht verfügbar:
    - Azure AD verbinden Dienststatus
    - Automatische updates
    - Kennwort abgeschlossenen writebackvorgängen

## <a name="download"></a>Herunterladen
Sie können aus dem Azure AD verbinden Blade innerhalb des Portals Azure AD verbinden herunterladen.  Suchen Sie das Blade Azure AD-verbinden Sie mit aufgeführten Schritte ausführen.

### <a name="the-azure-ad-connect-blade"></a>Verbinden von Azure AD Blade

Nachdem Sie Azure-Portal angemeldet haben, führen Sie folgende Schritte aus:

1. Wechseln Sie zu durchsuchen
2.  Wählen Sie aus Azure-Active Directory
3.  Wählen Sie dann auf Azure AD-verbinden

Sie sollten Folgendes angezeigt:

![Azure AD verbinden Blade](media\active-directory-aadconnect-germany\germany1.png)

 
Die folgende Tabelle beschreibt die Features in das Blade angezeigt.


Titel|Beschreibung|
----- | ----- |
SYNCHRONISIERUNGSSTATUS|Lassen Sie uns wissen Sie, ob die Synchronisierung aktiviert oder deaktiviert ist.|
LETZTE SYNCHRONISIEREN|Der letzten Berechnung Synchronisierung erfolgreiche abgeschlossen.|
PARTNERVERBUNDKONTAKTE DOMÄNEN|Zeigt die Anzahl der verbundenen Domänen, die aktuell konfiguriert.|


## <a name="installation"></a>Installation
Um Azure AD verbinden installiert haben, können Sie die Dokumentation [hier](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Erweiterte Funktionen und Weitere Informationen
Beginnen Sie für Weitere Informationen und Hilfe benutzerdefinierte Einstellungen oder erweiterten Konfigurationen mit [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md).  Diese Seite enthält Informationen und Links zu zusätzliche Anleitungen.
