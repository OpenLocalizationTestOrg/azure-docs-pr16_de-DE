<properties
    pageTitle="Verbinden von Geräten Domänenverbund mit Azure AD, für Windows 10 auftritt | Microsoft Azure"
    description="Erläutert, wie Administratoren Gruppenrichtlinien zum Aktivieren von Geräten werden mit dem Unternehmensnetzwerk Domänenverbund konfigurieren können."
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

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Herstellen einer Verbindung Azure AD für Windows 10 Erfahrung mit Domänenverbund Geräte

Beitreten zu einer Domäne ist, dass die herkömmlichen Art Organisationen Geräte für die Arbeit für den letzten 15 Jahren und vieles mehr verbunden haben. Es wurden Benutzer melden Sie sich bei ihren Geräten, mit ihrer Windows Server Active Directory (Active Directory) Arbeit oder Schule Konten aktiviert und zugelassen IT vollständig diese Geräte verwalten. Organisationen verlassen sich normalerweise auf imaging Methoden zum Bereitstellen von Geräten für Benutzer und im allgemeinen System Center Konfigurations-Manager (SCCM) oder Gruppenrichtlinien verwenden, um sie zu verwalten.

Beitreten zu einer Domäne in Windows-10 bietet die folgenden Vorteile aus, nachdem Sie die Geräte mit Azure Active Directory (Azure AD) verbunden:

- Einmaliges Anmelden (SSO) auf Azure AD-Ressourcen überall
- Zugriff auf die Enterprise-Windows Store mithilfe von geschäftlichen oder schulnotizbücher Konten (kein Microsoft-Konto erforderlich)
- Enterprise-kompatible roaming der benutzereinstellungen zwischen den verschiedenen Geräten mithilfe von geschäftlichen oder schulnotizbücher Konten (kein Microsoft-Konto erforderlich)
- Strenge Authentifizierung und bequeme-Anmeldung für geschäftlichen oder schulnotizbücher Konten mit Microsoft Passport und Windows Hallo
- Möglichkeit zum Einschränken des Zugriffs nur für Geräte, das einhalten organisationsinterne Gruppenrichtlinien des Audiogeräts

## <a name="prerequisites"></a>Erforderliche Komponenten

Beitreten zu einer Domäne weiterhin hilfreich sein. Jedoch zu profitieren Sie von SSO Azure AD-, roaming der Einstellungen für die Arbeit oder Schule Konten, und greifen Windows Store mit geschäftlichen oder schulnotizbücher Konten, benötigen Sie Folgendes:

- Azure AD-Abonnements
- Azure AD Herstellen einer Verbindung mit das lokalen Verzeichnis zu Azure AD erweitern
- Richtlinie, die Verbindung Domänenverbund Geräte zu Azure AD festgelegt wurde
- Windows-10-Generator (Build 10551 oder höher) für Geräte

Wenn Microsoft Passport für Arbeit und Windows Hallo aktivieren möchten, benötigen Sie auch Folgendes:

- Öffentlicher Schlüssel Infrastruktur für Benutzer Zertifikate Emission.
- System Center-Konfigurations-Manager Version 1509 für Technical Preview. Weitere Informationen finden Sie unter [Microsoft System Center Konfiguration Manager Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) und [System Center-Konfigurations-Manager-Teamblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). Dies ist erforderlich, basierend auf Microsoft Passport Tasten Zertifikate für Benutzer bereitgestellt werden.

Als Alternative zur Anforderung PKI-Bereitstellung können Sie die folgenden Aktionen ausführen:

- Haben Sie ein paar Domänencontroller mit Windows Server 2016 Active Directory-Domänendiensten.

Zum Aktivieren des Zugriffs bedingte können Sie Gruppenrichtlinien erstellen, die auf Geräten mit keine zusätzlichen Bereitstellungen Domänenverbund zugreifen dürfen. Zum Verwalten von Access Steuerung Kompatibilität des Geräts benötigen Sie Folgendes:

- System Center-Konfigurations-Manager Version 1509 für Technical Preview für Passport-Szenarien

## <a name="deployment-instructions"></a>Anweisungen für Bereitstellung



### <a name="step-1-deploy-azure-active-directory-connect"></a>Schritt 1: Bereitstellen von Azure Active Directory verbinden

Verbinden von Azure AD ermöglicht Ihnen Computern lokal als Geräteobjekte in der Cloud bereitstellen. Um Azure AD verbinden bereitstellen zu können, finden Sie in "Installieren Azure AD verbinden" im Artikel [Integration von Ihrem lokalen Identitäten mit Azure Active Directory](active-directory-aadconnect.md#install-azure-ad-connect).

 - Wenn Sie eine [benutzerdefinierte Installation für Azure AD-verbinden](./connect/active-directory-aadconnect-get-started-custom.md) (nicht die Express-Installation) befolgt haben, folgen Sie dann Verfahrens **erstellen eine Verbindung Dienst zeigen Sie in der lokalen Active Directory**, später in diesem Schritt.
 - Wenn Sie haben eine partnerverbundkontakte Konfiguration mit Azure AD vor der Neuinstallation Azure AD-verbinden (zum Beispiel, wenn Sie vor dem Active Directory Federation Services (AD FS) bereitgestellt haben), dann gehen Sie wie **Konfigurieren AD FS anfordern Regeln** , später in diesem Schritt.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Erstellen Sie einen Verbindungspunkt in lokalen Active Directory

Domänenverbund Geräte werden den Dienstverbindungspunkt zum Ermitteln von Azure AD-Mandanten Informationen zum Zeitpunkt der automatischen Registrierung mit dem Gerät Azure Registrierungsdienst verwendet.

Führen Sie auf dem Server Azure AD verbinden folgende PowerShell Befehle aus:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Wenn das Cmdlet $aadAdminCred ausgeführt = Get-Credential, verwenden Sie das Format *user@example.com* für den Benutzernamen der Anmeldeinformationen, die eingegeben werden, wenn das Popup Get-Credential angezeigt wird.

Bei der Ausführung des Cmdlets Initialisierung-ADSyncDomainJoinedComputerSync... Ersetzen Sie [*Verbinder Kontonamen*] mit der Domänenkonto, das als Active Directory-Connector-Konto verwendet wird.

#### <a name="configure-ad-fs-claim-rules"></a>Konfigurieren von AD FS anfordern Regeln
Konfigurieren der AD FS-Anfordern von Regeln ermöglicht erfolgt Registrierung von einem Computer mit Azure Gerät Registrierung Service, da die Computer mithilfe von Kerberos/NTLM über AD FS authentifizieren. Ohne diesen Schritt werden Computern Azure AD in eine verzögerte gegenseitig (unterliegen Azure AD verbinden synchronisieren Zeiten) Antworten.

>[AZURE.NOTE]
Wenn Sie keine AD FS als die Föderation Server lokal haben, folgen Sie den Anweisungen des Herstellers der zum Erstellen von Regeln anfordern.

Auf dem ADFS-Server (oder klicken Sie auf eine Sitzung mit dem AD FS-Server verbunden) führen Sie die folgenden PowerShell-Befehle aus:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows-10-Computern authentifiziert mithilfe der integrierten Windows-Authentifizierung zu einem aktiven WS-Trust Endpunkt von AD FS gehostet wird. Stellen Sie sicher, dass diese Endpunkt aktiviert ist. Wenn Sie die Web-Proxy-Authentifizierung verwenden, auch Stellen Sie sicher, dass diese Endpunkt über den Proxy veröffentlicht wird. Dies ist möglich, indem Sie die Adfs/Services/Trust/13/Windowstransport aktivieren. Es sollte angezeigt, wie in der AD FS-Verwaltungskonsole unter **Dienst**aktiviert > **Endpunkte**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Schritt 2: Konfigurieren der automatischen Gerät Registrierung über Gruppenrichtlinien in Active Directory.

Gruppenrichtlinien können in Active Directory so konfigurieren Sie Ihre Windows 10 Domänenverbund Geräte mit Azure AD automatisch registrieren.

> [AZURE.NOTE]
> Neueste Anweisungen zum Einrichten von automatischen Gerät Registrierung finden Sie unter, [zum Einrichten der automatischen Registrierung von Windows-Domäne beigetreten Geräte mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Diese Vorlage wurde umbenannt in Windows 10. Wenn Sie das Tool Gruppenrichtlinien aus einem Windows-10-Computer ausgeführt werden, wird die Richtlinie als angezeigt: <br>
> **Registrieren der Domäne verbundene Computer als Geräte**<br>
> Die Richtlinie befindet sich in folgendem Speicherort:<br>
> ***Computer-Konfiguration/Richtlinien/Administrative Vorlagen/Windows-Komponenten/Gerät Registrierung***


## <a name="additional-information"></a>Weitere Informationen
* [Windows-10 für das Unternehmen: Methoden für die Arbeit mit Geräten](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern Sie die Cloud-Funktionen, die auf Windows-10-Geräte über Azure Active Directory teilnehmen](active-directory-azureadjoin-user-upgrade.md)
* [Informationen Sie zu Szenarios für die Verwendung für Azure AD teilnehmen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Herstellen einer Verbindung Azure AD für Windows 10 Erfahrung mit Domänenverbund Geräte](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD teilnehmen](active-directory-azureadjoin-setup.md)
