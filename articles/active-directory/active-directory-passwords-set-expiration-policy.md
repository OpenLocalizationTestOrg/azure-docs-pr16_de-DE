<properties
    pageTitle="Festlegen von Richtlinien zum Kennwortablauf in Azure Active Directory | Microsoft Azure"
    description="Erfahren Sie, wie Richtlinien zum Kennwortablauf überprüfen und ändern Kennwortablauf Benutzer aus, entweder einzeln oder in Massen für Azure-Active Directory-Kennwörter"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Festlegen Sie, dass Kennwort Ablaufrichtlinien in Azure Active Directory

> [AZURE.IMPORTANT] **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).

Als globaler Administrator für ein Microsoft-Cloud-Dienst können Sie die Microsoft Azure Active Directory-Modul für Windows PowerShell Benutzerkennwörter einrichten, nicht, dass es abläuft. Können Sie auch Windows PowerShell-Cmdlets zum Entfernen der nie-abläuft Konfiguration oder welche Benutzer finden Sie unter Kennwörter sind einrichten nicht zur läuft ab. Dieser Artikel bietet Hilfe für Clouddienste, wie etwa Microsoft Intune und Office 365, die Identität und Directory-Dienste von Microsoft Azure Active Directory abhängig.

  > [AZURE.NOTE] Nur Kennwörter für Benutzerkonten, die nicht über Verzeichnissynchronisation synchronisiert werden können nicht ablaufen konfiguriert werden. Weitere Informationen zu Verzeichnissynchronisation finden Sie unter Liste der Hilfethemen in [verzeichnissynchronisierung: Wegweiser](https://msdn.microsoft.com/library/azure/hh967642.aspx).

Wenn Sie Windows PowerShell-Cmdlets verwenden zu können, müssen Sie zuerst installieren.

## <a name="what-do-you-want-to-do"></a>Was möchten Sie tun?

- [So aktivieren Sie Ablaufrichtlinie für ein Kennwort](#how-to-check-expiration-policy-for-a-password)

- [Legen Sie ein Kennwort abläuft](#set-a-password-to-expire)

- [Festlegen eines Kennworts aus, damit es nicht abläuft](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>So aktivieren Sie Ablaufrichtlinie für ein Kennwort

1.  Verbinden Sie mit Windows PowerShell mit Administrator-Anmeldeberechtigungen Unternehmen.

2.  Führen Sie eine der folgenden Aktionen aus:

    - Erkennen, ob das Kennwort eines einzelnen Benutzers festgelegt ist, dass es nie abläuft, führen Sie das folgende Cmdlet aus mithilfe der Benutzer Benutzerprinzipalnamen (UPN) (z. B. aprilr@contoso.onmicrosoft.com) oder die Benutzer-ID des Benutzers, die Sie überprüfen möchten:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Wenn Sie die Einstellung "Kennwort läuft nie ab" für alle Benutzer anzeigen möchten, führen Sie das folgende Cmdlet aus:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Legen Sie ein Kennwort abläuft

1.  Verbinden Sie mit Windows PowerShell mit Administrator-Anmeldeberechtigungen Unternehmen.

2.  Führen Sie eine der folgenden Aktionen aus:

    - Wenn das Kennwort eines einzelnen Benutzers festlegen möchten, dass das Kennwort abläuft, führen Sie das folgende Cmdlet mit den wichtigsten Benutzernamen (Benutzerprinzipalnamen) oder die Benutzer-ID des Benutzers:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Wenn die Kennwörter aller Benutzer in der Organisation festlegen möchten, dass sie ablaufen werden, verwenden Sie das folgende Cmdlet aus:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Legen Sie ein Kennwort nie abläuft

1. Verbinden Sie mit Windows PowerShell mit Administrator-Anmeldeberechtigungen Unternehmen.

2.  Führen Sie eine der folgenden Aktionen aus:

    - Zum Festlegen des Kennworts eines einzelnen Benutzers, dass es nie abläuft, führen Sie das folgende Cmdlet aus mithilfe der Benutzer Benutzerprinzipalnamen (UPN) oder die Benutzer-ID des Benutzers:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Wenn die Kennwörter aller Benutzer in einer Organisation, dass es nie abläuft festlegen möchten, führen Sie das folgende Cmdlet aus:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Nächste Schritte

* **Sie hier sind, weil Sie Probleme beim Anmelden haben?** Wenn dies der Fall ist, [So wie Sie ändern können, und Ihr eigenes Kennwort zurücksetzen](active-directory-passwords-update-your-own-password.md).
