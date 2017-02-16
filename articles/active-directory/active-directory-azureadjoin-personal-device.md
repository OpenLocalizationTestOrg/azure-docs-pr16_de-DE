

<properties
    pageTitle="Teilnehmen an einer persönlichen Gerät Ihrer Organisation | Microsoft Azure"
    description="Wird erläutert, wie Benutzer ihren persönlichen Windows-10-Geräten in das Unternehmensnetzwerk registrieren können, sowie die Bereitstellungsschritte für ein BYOD Szenario."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="join-a-personal-device-to-your-organization"></a>Teilnehmen an einer persönlichen Gerät Ihrer Organisation

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Ein Windows-10-Gerät mit Ihrer Organisation zu verknüpfen.

1.  Wählen Sie im Menü **Start** **Einstellungen**aus.
2.  Wählen Sie **Konten**aus, und klicken Sie dann auf **Ihr Konto**.
3.  Klicken Sie auf **Hinzufügen, Arbeit oder Schule Konto**, und geben Sie in dem organisationskonto an.
4.  Klicken Sie auf der Anmeldeseite für Ihre Organisation geben Sie Ihren Benutzernamen und Ihr Kennwort ein, und klicken Sie dann auf **OK**.
5.  Sie werden aufgefordert, eine kombinierte Authentifizierung Herausforderung. (Dieser Herausforderung vom IT-Administrator konfiguriert ist.)
6.  Azure Active Directory (Azure AD) überprüft, ob das Gerät Mobilgerät Management Registrierung erforderlich ist.
7.  Windows das Gerät im Verzeichnis der Organisation in Azure AD registriert und registriert in mobilen Gerät Verwaltung wird, sofern zutreffend.
8.  Wenn Sie eine verwaltete Benutzer sind, öffnet Windows den Desktop über die automatische Anmeldung.
9.  Wenn Sie ein Partnerbenutzer sind, werden Sie zu einem Windows-Anmeldebildschirm weitergeleitet, geben Sie Ihre Anmeldeinformationen ein.

## <a name="additional-information"></a>Weitere Informationen
* [Windows-10 für das Unternehmen: Methoden für die Arbeit mit Geräten](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern Sie die Cloud-Funktionen, die auf Windows-10-Geräte über Azure Active Directory teilnehmen](active-directory-azureadjoin-user-upgrade.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Informationen Sie zu Szenarios für die Verwendung für Azure AD teilnehmen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Herstellen einer Verbindung Azure AD für Windows 10 Erfahrung mit Domänenverbund Geräte](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD teilnehmen](active-directory-azureadjoin-setup.md)
