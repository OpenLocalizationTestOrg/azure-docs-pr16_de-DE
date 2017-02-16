<properties
    pageTitle="Automatische Gerät Registrierung mit Azure Active Directory for Windows Domain-Joined Geräten | Microsoft Azure"
    description="IT-Administratoren können festlegen, dass ihre Domäne Windows-Geräte automatisch und im Hintergrund mit Azure Active Directory (Azure AD) zu registrieren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="markvi"/>

# <a name="automatic-device-registration-with-azure-active-directory-for-windows-domain-joined-devices"></a>Automatische Gerät Registrierung mit Domänenverbund Active Directory für Windows Azure-Geräten

Als IT-Administrator können Sie auswählen, um automatisch und im Hintergrund Ihrer Domäne Windows-Geräten mit Azure Active Directory (Azure AD) registrieren. Dies ist nützlich, wenn Sie konfiguriert haben bedingten Zugriff Gerät basierend auf Office 365-Richtlinien Applikationen oder Applikationen verwalteten lokalen per AD FS. Weitere Informationen zu den Gerät Registrierung Szenarien Weitere Lesen der [Azure Active Directory Gerät Registrierung Overview](active-directory-conditional-access-device-registration-overview.md).

>AZURE. Hinweis neueste Anweisungen zum Einrichten von automatischen Gerät Registrierung finden Sie unter [so eingerichtet automatische Registrierung des Windows-Domäne beigetreten Geräte mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

Automatische Gerät Registrierung mit Azure Active Directory steht für Windows 7 und Windows 8.1-Computer, auf denen Active Directory-Domäne hinzugefügt haben. Dies sind in der Regel corporate im Besitz Maschinen, die für Information Worker erteilt wurde.

Wenn Sie Ihre Domäne registriert beigetreten Windows-Geräten mit Azure AD-, führen Sie die folgenden Komponenten. Nachdem Sie die erforderlichen Komponenten abgeschlossen haben, konfigurieren Sie automatische Gerät Registrierung für Ihre Domäne auf Windows-Geräten.

## <a name="prerequisites-for-automatic-device-registration-of-domain-joined-windows-devices-with-azure-active-directory"></a>Erforderliche Komponenten für die automatische Gerät Registrierung von Domänennamen beigetreten Windows-Geräten mit Azure Active Directory

<a name="deploy-ad-fs-and-connect-to-azure-active-directory-using-azure-active-directory-connect"></a>Bereitstellen von AD FS und Verbinden mit Azure Active Directory mithilfe von Azure Active Directory verbinden
----------------------------------------------------------------------------------------------
1. Verwenden Sie zum Bereitstellen von Active Directory Federation Services (AD FS) mit Windows Server 2012 R2 und Einrichten einer Beziehung Föderation mit Azure Active Directory Azure Active Directory verbinden.
2. Konfigurieren einer zusätzlichen Azure Active Directory sich verlassen Partei Trust anfordern Regel.
3. Öffnen Sie die AD FS-Verwaltungskonsole, und navigieren Sie zu **AD FS**>**vertrauen Beziehungen > verlassen vertrauen**. Mit der rechten Maustaste auf das Microsoft Office 365-Identitätsplattform sich verlassen Partei Trust Objekt, und wählen Sie **Bearbeiten der Regeln anfordern...**
4. Wählen Sie auf der Registerkarte **Emission transformieren Regeln** **Regel hinzufügen**aus.
5. Wählen Sie **Senden mithilfe einer benutzerdefinierten Regel Ansprüche** aus der **Regel beanspruchen** Vorlage Dropdown-Feld ein. Wählen Sie **Weiter**aus.
6. Geben Sie *Autorisierende Methode anfordern Regel* in der **Regelname anfordern:** Textfeld.
7. Geben Sie die folgenden anfordern Regel in der **anfordern Regel:** Textfeld:

        c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
        => issue(claim = c);

8. Klicken Sie auf **OK** , zweimal, um das Dialogfeld zu abzuschließen.

<a name="configure-an-additional-azure-active-directory-relying-party-trust-authentication-class-reference"></a>Konfigurieren Sie eine zusätzliche Azure Active Directory verlassen Partei Trust Referenz für die Authentifizierung
-----------------------------------------------------------------------------------------------------
Öffnen Sie auf dem Server Föderation eine Windows PowerShell-Befehlsfenster und Typ aus:


  `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

Wo <RPObjectName> ist der sich verlassen Partei Objektname für Ihre Azure Active Directory-Objekt Trust sich verlassen Party. Dieses Objekt ist in der Regel mit der Bezeichnung Microsoft Office 365-Identitätsplattform.

<a name="ad-fs-global-authentication-policy"></a>AD FS-Authentifizierung globale Richtlinie
-----------------------------------------------------------------------------
Konfigurieren der AD FS globale primären Authentifizierungsrichtlinie für das Intranet integrierte Windows-Authentifizierung zulässig (Dies ist die Standardeinstellung).


<a name="internet-explorer-configuration"></a>Internet Explorer-Konfiguration
------------------------------------------------------------------------------
Konfigurieren Sie die folgenden Einstellungen auf Ihrem Windows-Geräten für die lokale Intranetzone in Internet Explorer:

- Keine Aufforderung zur Clientzertifikatsauswahl, wenn nur ein Zertifikat vorhanden ist: **Aktivieren**
- Skripting zulassen: **Aktivieren**
- Automatisches Anmelden nur in der Intranetzone: **Checked**

Dies sind die Standardeinstellungen für die Internet Explorer lokale Intranetzone. Anzeigen oder verwalten Sie diese Einstellungen in Internet Explorer, indem Sie navigieren zu **Internetoptionen** > **Sicherheit** > Lokales Intranet > Stufe. Sie können auch diese Einstellungen mit Active Directory-Gruppenrichtlinien konfigurieren.

<a name="network-connectivity"></a>Netzwerkkonnektivität
-------------------------------------------------------------
Domäne beigetreten Windows Geräte benötigen, eine Verbindung zu AD FS und einer Active Directory-Domänencontroller automatisch mit Azure AD registrieren. Dies bedeutet normalerweise, dass der Computer mit dem Firmennetzwerk verbunden werden muss. Dies kann eine kabelgebundene Verbindung, eine WLAN-Verbindung, DirectAccess oder VPN enthalten.

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Konfigurieren der Suche Azure Active Directory-Gerät Registrierung
Windows 7 und Windows 8.1 Geräte werden das Gerät Registrierungsserver durch Kombinieren der aktuelle Kontoname mit bekannten Gerät Registrierung Servernamen ermitteln. Sie müssen einen DNS-CNAME-Eintrag erstellen, der auf Ihre Azure Active Directory-Gerät Registrierung-Dienst zugeordnet A-Eintrag verweist. Der CNAME-Eintrag muss die bekannten Präfix **Enterpriseregistration** gefolgt von Benutzerkonten in Ihrer Organisation verwendeten Benutzerprinzipalnamen-Suffix verwenden. Wenn Ihre Organisation mehrere UPN-Suffixe verwendet, müssen mehrere CNAME-Einträge in DNS erstellt werden.

Wenn Sie zwei Benutzerprinzipalnamen-Suffixe in Ihrer Organisation mit dem Namen verwenden, z. B. @contoso.com und @region.contoso.com, erstellen Sie die folgenden DNS-Einträge.

| Eintrag                                     | Typ  | Adresse                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.Windows.NET |
| enterpriseregistration.Region.contoso.com | CNAME | enterpriseregistration.Windows.NET |

##<a name="configure-automatic-device-registration-for-windows-7-and-windows-81-domain-joined-devices"></a>Konfigurieren Sie automatische Gerät Registrierung für Windows 7 und Windows 8.1-Domäne beitreten-Geräte

Konfigurieren von automatischen Gerät Registrierung für Ihren Windows 7 und Windows 8.1-Domäne beitreten-Geräten mit den nachstehenden Links. Achten Sie darauf, dass Sie die oben aufgeführten erforderlichen Komponenten abgeschlossen haben, bevor Sie fortfahren.

* [Konfigurieren von automatischen Gerät Registrierung für Windows 8.1-Domäne beitreten Geräten](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)

* [Konfigurieren von automatischen Gerät Registrierung für Windows 7-Domäne beitreten Geräten](active-directory-conditional-access-automatic-device-registration-windows7.md)

* [Automatische Device Registration wird mit Azure Active Directory for Windows 10 Domain-Joined Geräten](active-directory-azureadjoin-devices-group-policy.md)

<a name="additional-notes"></a>Zusätzliche Hinweise
--------------------------------------------------------------------

Gerät Registrierung mit Azure AD-bietet die größte Menge an Gerätefunktionen. Mit der Registrierung Azure AD-Gerät können Sie persönliche (BYOD) und mobile Geräte und Unternehmen, die im Besitz registrieren, Domäne beigetreten Geräte. Die Geräte können mit beide gehosteten Dienste wie Office 365 und-Diensten verwalteten lokalen mit AD FS verwendet werden.

Unternehmen, die sowohl Geräte und traditionelle Geräten oder, verwenden Office 365, Azure AD oder anderen Microsoft-Diensten sollten Geräte in Azure Active Directory mithilfe des Diensts für Azure AD-Gerät Registrierung registrieren verwenden. Wenn Ihr Unternehmen nicht mobile Geräte verwendet und keine Microsoft-Diensten wie Office 365, Azure AD oder Microsoft Intune und stattdessen Hosts nur mit lokal gehosteten Applications, können Sie entscheiden Geräte in Active Directory registrieren AD FS verwenden.

Sie können weitere Informationen zur Bereitstellung von Gerät Registrierung mit AD FS [hier](https://technet.microsoft.com/library/dn486831.aspx).

## <a name="additional-topics"></a>Weitere Themen

- [Azure-Active Directory-Gerät Registrierung (Übersicht)](active-directory-conditional-access-device-registration-overview.md)
- [Konfigurieren der automatischen Gerät Registrierung für Windows 7-Domäne beitreten-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Konfigurieren der automatischen Gerät Registrierung für Windows 8.1-Domäne beitreten Geräte](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische Gerät Registrierung mit Domänenverbund Azure Active Directory für Windows 10-Geräte](active-directory-azureadjoin-devices-group-policy.md)
