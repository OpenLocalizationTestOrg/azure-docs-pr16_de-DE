<properties
    pageTitle="Enterprise Zustand Roaming Übersicht | Microsoft Azure"
    description="Enthält Informationen zur Enterprise Zustand Roaming Einstellungen in Windows-Geräten. Enterprise Zustand Roaming ermöglicht eine einheitliche Verwendung über ihren Windows-Geräten und Zeit zum Konfigurieren eines neuen Geräts verkürzt."
    services="active-directory"
    keywords="Was ist Enterprise Zustand bei Roaming, Enterprise, cloud windows"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="enterprise-state-roaming-overview"></a>Übersicht über die Enterprise Zustand Roaming

Mit Windows-10 erhalten [Azure Active Directory (Azure AD)](active-directory-whatis.md) Benutzer die Möglichkeit, ihre Benutzer und Anwendung Einstellungen in der Cloud sicher zu synchronisieren. Enterprise Zustand Roaming ermöglicht eine einheitliche Verwendung über ihren Windows-Geräten und Zeit zum Konfigurieren eines neuen Geräts verkürzt. Enterprise Zustand Roaming arbeitet ähnlich wie in Windows 8 bot standard [Consumer Einstellungen synchronisieren](http://windows.microsoft.com/en-US/windows-8/sync-settings-pcs) . Darüber hinaus bietet Enterprise Zustand Roaming:

- **Trennung der corporate und Consumer Daten** – Organisationen stehen Kontrolle über ihre Daten, und es wird keine Mischen von Unternehmensdaten in einem Consumer Cloud-Konto oder Consumer, der Daten in einem Enterprise-Cloud-Konto.
- **Verbesserte Sicherheit** – Daten werden vor Verlassen des Benutzers Windows 10 Gerät mithilfe der Verwaltung von Informationsrechten Azure (Azure RMS) automatisch verschlüsselt und Daten bleibt verschlüsselte statisch sind in der Cloud. Alle Inhalte bleibt verschlüsselte statisch sind in der Cloud, eine Ausnahme bilden jedoch die Namespaces, wie Einstellungen und Windows-app Names.  
- **Bessere Verwaltung und Überwachung** – stellt Steuerelement und Sichtbarkeit über die Einstellungen in Ihrer Organisation und für die jeweiligen Geräte über die Portalseite Azure AD-Integration synchronisiert. 

Enterprise Zustand Roaming steht in mehreren Azure Regionen zur Verfügung. Sie können die aktualisierte Liste der verfügbaren Bereiche auf der Seite [Azure-Dienste von Regionen](https://azure.microsoft.com/regions/#services) unter Azure Active Directory suchen.



| Artikel                                         | Beschreibung                                                                                                                                                                                             |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Enterprise-Aktivierungsstatus Roaming in Azure-Active Directory](active-directory-windows-enterprise-state-roaming-enable.md) | Enterprise Zustand Roaming steht für eine Organisation ein Abonnement Premium Azure-Active Directory (Azure AD). Weitere Details zum Abrufen eines Azure AD-Abonnements finden Sie auf der Seite [Azure AD-Produkt](https://azure.microsoft.com/services/active-directory) . |
| [Einstellungen und roaming häufig gestellte Fragen zu Daten](active-directory-windows-enterprise-state-roaming-faqs.md)                    | In diesem Thema werden einige Fragen, die IT-Administratoren zu Einstellungen, und Synchronisieren der app-Daten möglicherweise beantwortet.                                                                                                        |
| [Für Gruppenrichtlinien und MDM Einstellungen für Einstellungen synchronisieren](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)  | Windows-10 bietet Gruppenrichtlinien und Verwaltung mobiler Geräte Richtlinieneinstellungen (MDM), um Einstellungen synchronisieren zu beschränken.                                                                                             |
| [Windows-10 roaming Einstellungen Bezug](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)            | Im folgenden finden eine vollständige Liste der die Einstellungen, die und/oder werden ist ein Roaming gesicherte in Windows 10.                                                                                                |
