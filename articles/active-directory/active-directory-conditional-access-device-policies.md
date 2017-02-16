<properties
    pageTitle="Bedingte Gerät-Richtlinien für Office 365-Diensten | Microsoft Azure"
    description="Details zur Vorschriften wie Gerät-basierten Steuern des Zugriffs auf Office 365-Dienste. Während der Office 365-Dienste wie Exchange und SharePoint Online in Arbeit oder Schule von ihren persönlichen Geräten Informationsarbeitern () möchten, möchte ihre IT-Administrator den Zugriff auf sein, dass secure.IT Administratoren bedingte Gerät-Richtlinien zum Sichern Ihres Unternehmens Ressourcen, während gleichzeitig gleicht IAS auf kompatiblen Geräten Zugriff auf die Dienste bereitstellen können."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>
# <a name="conditional-access-device-policies-for-office-365-services"></a>Bedingte Gerät-Richtlinien für Office 365-Diensten

Der Begriff "bedingte zugreifen" weist viele Bedingungen wie mehrstufige authentifizierten Benutzer authentifiziert zugeordnet, kompatible Gerät usw.. In diesem Thema focusses hauptsächlich auf Gerät-basierte Bedingungen zum Steuern des Zugriffs auf Office 365-Dienste. Während der Office 365-Dienste wie Exchange und SharePoint Online in Arbeit oder Schule von ihren persönlichen Geräten Informationsarbeitern () möchten, möchte ihre IT-Administrator den Zugriff auf secure werden. IT-Administratoren können bedingte Gerät-Richtlinien zum Sichern Ihres Unternehmens Ressourcen, während gleichzeitig gleicht IAS auf kompatiblen Geräten Zugriff auf die Dienste bereitstellen. Bedingte Richtlinien zu Office 365 können vom Microsoft Intune bedingte Access-Portal konfiguriert werden.

Azure-Active Directory erzwingt bedingte Richtlinien zum Sichern des Zugriffs auf Office 365-Dienste. Ein mandantenadministrator kann eine bedingte Zugriffsrichtlinie erstellen, die einen Benutzer auf einem nicht kompatiblen Gerät aus den Zugriff auf ein Office 365-Dienst blockiert. Der Benutzer muss Gerät-Richtlinien des Unternehmens entsprechen vor dem Zugriff auf den Dienst gewährt werden kann. Der Administrator kann alternativ auch eine Richtlinie erstellen, die Benutzern die aktivierten Geräte Zugriff auf ein Office 365-Dienst nur Registrierung erforderlich sind. Richtlinien möglicherweise angewendet, die für alle Benutzer einer Organisation, oder ein paar Zielgruppen eingeschränkt und erweiterte über einen Zeitraum weitere Zielgruppen aufnehmen möchten.

Eine Voraussetzung zum Erzwingen von Gerät ist für Benutzer aktivierten Geräte mit Azure Active Directory-Gerät Registrierung Service registrieren. Sie können optional kombinierte Authentifizierung (MFA) für die Registrierung von Geräten mit Azure Active Directory-Gerät Registrierung Service aktivieren. MFA empfiehlt sich für den Dienst Azure Active Directory-Gerät Registrierung. Wenn MFA aktiviert ist, werden Benutzer aktivierten Geräte mit Azure Active Directory-Gerät Registrierung Service registrieren für die zweite Faktor Authentifizierung zur Eingabe aufgefordert.

##<a name="how-does-conditional-access-policy-work"></a>Wie funktioniert die bedingte Zugriffsrichtlinie?

Wenn ein Benutzer Anfragen Zugriff auf Office 365-Dienst aus einem unterstützten Geräteplattform, authentifiziert Azure Active Directory Benutzer und Gerät, aus denen der Benutzer die Anfrage startet; und gewährt Zugriff auf den Dienst nur, wenn der Benutzer die Richtlinie für den Dienst entspricht. Benutzern, die keine Geräts registriert haben gewährt schwächeren Anweisungen zum Registrieren und erst kompatiblen auf corporate O365-Dienste zuzugreifen. Benutzer auf iOS und Android müssen aktivierten Geräte mit Unternehmen Portal Anwendung zu registrieren. Wenn ein Benutzer seine Gerät registriert wird, ist das Gerät mit Azure Active Directory registriert und für die Verwaltung Geräte und Konformität registriert. Kunden müssen den Dienst Azure Active Directory-Gerät Registrierung in Verbindung mit Microsoft Intune verwenden, um mobilen Gerät-Verwaltung für Office 365-Dienst zu aktivieren. Gerät Registrierung ist eine Vorbedingung für Benutzer zu Office 365-Dienste an, wenn Geräterichtlinien umgesetzt werden.

Wenn ein Benutzer seine Gerät erfolgreich registriert wird, wird das Gerät als vertrauenswürdig eingestuft. Stellt Azure-Active Directory-einmaliges Anmelden auf Access Unternehmen Anwendungen und erzwingt bedingte Zugriffsrichtlinie um Zugriff auf einer nicht nur die ersten zum Gewähren des Benutzers, Access, aber jedes Mal des Benutzers anfordert anfordert, Access zu erneuern. Der Benutzer wird verweigert Zugriff auf Dienste beim Anmeldeinformationen werden geändert, Gerät ist verloren gegangen oder gestohlen oder die Richtlinie nicht erfüllt, zum Zeitpunkt der Anforderung zur Erneuerung zur Verfügung ist.

## <a name="deployment-considerations"></a>Aspekte beim Bereitstellen:
Müssen Azure Active Directory-Gerät Registrierung Dienst für die Registrierung Gerät verwenden.

Wenn Benutzer sind lokal authentifiziert werden, ist Active Directory Federation Services (AD FS) (1.0 und höher) erforderlich. Mehrstufige Authentifizierung für Arbeitsplatz teilnehmen schlägt fehl, wenn der Identitätsanbieter nicht kombinierte Authentifizierung kann. Beispielsweise AD FS 2.0 ist nicht mehrstufige Authentifizierung-fähigen. Mandantenadministrator muss sicherstellen, dass die lokalen AD FS MFA Lage und eine gültige MFA-Methode aktiviert ist, bevor UFI-MFA Azure Active Directory-Gerät Registrierung-Dienst. Angenommen, hat AD FS unter Windows Server 2012 R2 MFA-Funktionen. Sie müssen auch eine zusätzliche gültige (MFA) Authentifizierungsmethode auf dem ADFS-Server aktivieren, bevor UFI-MFA Azure Active Directory-Gerät Registrierung-Dienst. Weitere Informationen zu unterstützten MFA Methoden in AD FS finden Sie unter AD FS zusätzliche Authentifizierungsmethoden konfigurieren.

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

F: Was ist die Standardrichtlinie Ausschluss für nicht unterstützte Plattformen?

A: zur Zeit werden bedingte Richtlinien auf Benutzer iOS und Android Selektives erzwungen. Applikationen auf anderen Geräteplattformen sind standardmäßig, durch die bedingte Zugriffsrichtlinie für iOS und Android nicht betroffen. Mandantenadministrator möglicherweise, jedoch die Möglichkeit, die globale Richtlinie zum Zugriff auf die Benutzer nicht unterstützte Plattformen verbieten außer Kraft setzen.
Diese Einstellung ist der Wegweiser für bedingte Zugriffsrichtlinie für Benutzer auf anderen Plattformen, einschließlich Windows zu erweitern.

F: Wann wird bedingte Zugriffsrichtlinie auf Office 365-Diensten auf browserbasierte apps (beispielsweise OWA, SharePoint browserbasierte) erweitert werden.

A: zur Zeit ist bedingte Zugriff auf Office 365-Dienste auf Rich-Anwendungen auf dem Gerät beschränkt. Diese Einstellung ist der Wegweiser für bedingte Zugriffsrichtlinie Benutzern den Zugriff auf die Dienste von Browsern zu erweitern.
