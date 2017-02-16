<properties
    pageTitle="Verbinden von Azure AD- und Föderation | Microsoft Azure"
    description="Diese Seite ist der zentrale Speicherort alle Dokumentation zur Azure AD-Verbinden mit AD FS-Vorgänge"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD verbinden und Föderation

Azure AD verbinden können Sie Föderation zu konfigurieren, mit der lokalen AD FS und Azure AD-. Mit Föderation melden Sie sich auf können Sie Benutzer Azure AD-basierte Dienste mit ihrer lokalen Kennwörter und, klicken Sie auf das Unternehmensnetzwerk bei auf ohne ihre Kennwörter erneut eingeben zu müssen aktivieren. Die Option Föderation mit AD FS können Sie ein neues bereitstellen oder einer vorhandenen AD FS in Windows Server 2012 R2 Farm angeben.

In diesem Thema ist die Homepage für Föderation Informationen zu Funktionen für Azure AD-verbinden und Listen Links im Zusammenhang mit allen anderen Themen im Zusammenhang mit es. Links zu Azure AD verbinden finden Sie unter Integration von Ihrem lokalen Identitäten mit Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Verbinden mit Azure AD - Partnerverbund Themen

| Thema | Was es behandelt und wann lesen |
|:------|:-----------|
| **Azure AD verbinden Anmeldung Benutzeroptionen** ||
| [Grundlegendes zu Anmeldung Benutzeroptionen](active-directory-aadconnect-user-signin.md) | Beschreibung der verschiedenen Anmeldung Benutzeroptionen und Einfluss Azure-Benutzeroberfläche |
| **AD FS-Installation mit Azure AD-verbinden**||
| [Erforderliche Komponenten](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Erforderlichen Komponenten für eine erfolgreiche AD FS-Installation über Azure AD-verbinden|
| [Konfigurieren von AD FS-farm](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | So installieren Sie eine neue AD FS-Farm mit Azure AD-verbinden |
| **Ändern der AD FS-Konfiguration** | |
| [Reparieren die Vertrauensstellung](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Reparieren der aktuellen Vertrauensstellung zwischen lokalen AD FS und Office 365 / Azure |
| [Hinzufügen eines neuen AD FS-Servers](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Erweitern AD FS-Farm mit zusätzlichen AD FS-Erstinstallation Server Beitrag |
| [Hinzufügen eines neuen AD FS WAP-Servers](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Erweitern AD FS-Farm mit zusätzlichen WAP-Erstinstallation Server Beitrag |
| [Hinzufügen einer neuen verbunddomäne](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Hinzufügen einer weiteren Domäne mit Azure AD verbunden sein |
|**Aufgaben nach der installation**||
| [Hinzufügen von benutzerdefinierten Firmenlogo / (Abbildung)](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Ändern der Anmeldeverhalten durch Angabe der benutzerdefinierten Logos, die auf dem AD FS-Anmeldeseite angezeigt werden |
| [Hinzufügen einer Anmeldung Beschreibung](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Ändern der Beschreibung Anmeldung auf dem AD FS-Anmeldeseite | 
| [Ändern von AD FS beanspruchen Regeln](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Ändern Sie / fügen anfordern Regeln in AD FS entspricht Azure AD verbinden synchronisieren Konfiguration hinzu |


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
* [AD FS-Bereitstellung in Azure](active-directory-aadconnect-azure-adfs.md)
* [Hohe Verfügbarkeit Cross geografischen AD FS-Bereitstellung in Azure mit Azure Datenverkehr Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


