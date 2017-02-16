<properties
    pageTitle="Einrichten von automatischen Registrierung von Windows Domänenverbund Geräte mit Azure Active Directory | Microsoft Azure"
    description="Richten Sie Ihre Domäne Windows-Geräte automatisch und im Hintergrund mit Azure Active Directory registrieren aus."
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
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Einrichten der automatischen Registrierung von Windows Domänenverbund Geräte mit Azure Active Directory

Um [Azure Active Directory-basierten Gerät bedingten Zugriff](active-directory-conditional-access.md)zu verwenden, müssen Ihre Domäne Windows-Computer mit Azure Active Directory (Azure AD) registriert sein. In diesem Artikel erfahren Sie, was Sie tun müssen, um die Registrierung von Windows Domänenverbund Geräte mit Azure AD-in Ihrer Organisation einrichten.

Verwenden von bedingten Access in Azure AD bietet Ihnen folgende Vorteile:

-   Erweiterte einmaliges Anmelden (SSO) Erfahrung in Azure AD-apps durch geschäftlichen oder schulnotizbücher Konten
-   Enterprise-roaming von Einstellungen für Geräte
-   Zugriff auf Windows Store für Unternehmen
-   Sicheres Authentifizierung und bequeme-Anmeldung mit Windows Hallo

> [AZURE.NOTE] Die Windows-10-November Aktualisierung bietet einige der erweiterten Benutzerfunktionalität in Azure AD-, aber das Windows 10 Jahrestag aktualisieren vollständig unterstützt bedingte Access-basierten Gerät. Weitere Informationen zu bedingter Access finden Sie unter [Azure Active Directory-basierten Gerät bedingten Zugriff](active-directory-conditional-access.md). Weitere Informationen zu Windows-10-Geräten am Arbeitsplatz und wie ein Benutzer ein Windows-10-Gerät mit Azure AD registriert, finden Sie unter [Windows 10 für die Enterprise: Verwenden von Geräten für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md).

Sie können einige früheren Versionen von Windows, einschließlich der folgenden Versionen registrieren:

-   Windows 8.1
-   Windows 7

Wenn Sie einen Computer mit Windows Server als Desktop verwenden, können Sie diese Plattformen registrieren:

- WindowsServer 2016
- Windows Server 2012 R2
- WindowsServer 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Erforderliche Komponenten

Das Hauptfenster Vorbedingung für die automatische Registrierung Domänenverbund Geräte mithilfe von Azure AD ist eine aktuelle Version von Azure Active Directory verbinden (Azure AD verbinden) haben.

Je nach der Bereitstellung Azure AD verbinden und, ob Sie eine ausdrückliche oder benutzerdefinierte Installation oder ein Upgrade von in-situ verwendet die folgenden Vorkenntnisse möglicherweise automatisch konfiguriert wurden:

-   **Dienstverbindungspunkt in lokalen Active Directory**. Für die Ermittlung der Azure AD-Mandanten Informationen von Computern registrieren, die für Azure AD.

-   **Active Directory Federation Services (AD FS) Emission transformieren Regeln**. Für die Computerauthentifizierung für die Registrierung (gilt für partnerverbundkontakte Konfigurationen).

Wenn einige Geräte in Ihrer Organisation nicht Windows 10 Domänenverbund Geräte sind, stellen Sie sicher, dass Sie die folgenden Aufgaben ausführen:

- Festlegen von einer Richtlinie in Azure AD, damit Benutzer Geräte registrieren können
- Festlegen Sie integrierte Windows-Authentifizierung (IWA) als eine gültige Alternative zum kombinierte Authentifizierung in AD FS



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Festlegen der einen Verbindungspunkt für die Ermittlung der Azure AD-Mandanten

Ein Service-Objekt muss vorhanden sein, in die Konfigurationsnamenpartition Kontext der die Struktur der Domäne, in dem Computern hinzugefügt werden. Der Dienstverbindungspunkt enthält Discovery-Informationen zu den Azure AD-Mandanten, auf dem Computer registrieren. In einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen muss der Dienstverbindungspunkt in allen Gesamtstrukturen vorhanden sind, Domänenverbund Computer verwenden.

Legen Sie den Dienstverbindungspunkt an folgenden Speicherorten in Active Directory fest:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Für wird von eine Struktur, in dem Active Directory Domänennamen *Beispiel.com*, die Konfiguration Kontext benennen CN = Konfiguration, DC = Beispiel, DC = com.

Sie können mithilfe des folgenden Windows PowerShell-Skripts für die Azure AD-Mandanten Objekt und Discovery-Werte überprüfen. (Ersetzen Sie Konfigurationsnamenskontext im Beispiel mit dem Konfigurationsnamenskontext).

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

Die `$scp.Keywords` Ausgabe zeigt die Informationen der Azure AD-Mandanten:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Wenn Sie der Dienstverbindungspunkt nicht vorhanden ist, erstellen Sie ihn durch Ausführen des folgenden PowerShell-Skripts auf dem Server Azure AD verbinden:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Beim Ausführen von `$aadAdminCred = Get-Credential`, verwenden Sie das Format *user@example.com* für den Benutzernamen im Dialogfeld **Get-Credential** .  
> Wenn Sie das Cmdlet Initialisierung-ADSyncDomainJoinedComputerSync ausführen, ersetzen Sie [*Verbinder Kontonamen*] durch das Domänenkonto, das in der Active Directory-Connector-Konto verwendet wird.  
> Das Cmdlet verwendet das Active Directory-PowerShell-Modul, die in einer Domäne Active Directory-Webdiensten benötigt. Active Directory-Webdiensten wird in Domänencontroller in Windows Server 2008 R2 oder höher unterstützt. Verwenden Sie für die Domänencontroller in Windows Server 2008 oder einer früheren Version die System.DirectoryServices-API über PowerShell, um den Dienstverbindungspunkt zu erstellen und weisen Sie die Stichwörter Werte zu.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Erstellen von AD FS-Regeln für die Sofortsuche Gerät Registrierung in verbundenen Unternehmen

In einer verbundenen Azure AD-Konfiguration Geräte aufsetzen AD FS (oder auf dem lokalen Föderation Server) zur Azure AD Authentifizierung. Registrieren sie Sie dann gegen Azure Active Directory-Gerät Registrierungsdienst (Azure AD-Gerät Registrierung).

> [AZURE.NOTE] In einer Konfiguration nicht Partnersuche Benutzerkennworthashes auf Azure Active Directory synchronisiert werden, und zur Windows 10 und Windows Server 2016 Domänenverbund Computern Authentifizierung Azure AD-Gerät Registration Service. Ein Benutzer authentifiziert sich unter Verwendung von Anmeldeinformationen, die der Benutzer in ihrer lokalen Computerkonten verfasst und welche wird an den Azure AD über Azure AD verbinden weitergeleitet. Für Windows - nicht 10 und Windows Server 2016 Computern in einer nicht Partnerbenutzern-Konfiguration müssen Sie die Optionen für ein Gerät-basierten Zertifizierungsstelle in Ihrer Organisation einrichten. Weitere Informationen finden Sie im Abschnitt Herunterladen von Windows Installer-Paketen für Windows - nicht 10 Computern in diesem Artikel.

Für Windows 10 und Windows Server 2016 Computern ordnet Azure AD verbinden in Azure Active Directory das Objekt mit dem lokalen Computer Kontoobjekt an. Die folgenden Ansprüche müssen vorhanden sein, während der Authentifizierung für Azure AD-Gerät Registration Service zum Abschließen der Registrierung und erstellen Sie das Geräteobjekt:

- http://Schemas.Microsoft.com/WS/2012/01/AccountType enthält den DJ-Wert, der die wichtigsten Authentifizierung als einem Computer Domänenverbund identifiziert.
- http://Schemas.Microsoft.com/Identity/Claims/onpremobjectguid enthält den Wert für das Attribut **Objekt-GUID** des Kontos lokalen Computer an.
- http://Schemas.Microsoft.com/WS/2008/06/Identity/Claims/primarysid enthält des Computers der primären Sicherheits-ID (SID), der die **ObjectSid** Attributwert des Kontos lokalen Computer entspricht.
- http://Schemas.Microsoft.com/WS/2008/06/Identity/Claims/issuerid enthält den Wert, der Azure AD wird verwendet, um das Token ausgestellt von AD FS oder aus der lokalen Security Token Service (STS) vertrauen. Dies ist wichtig, in einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen. In dieser Konfiguration möglicherweise Computer mit einer Gesamtstruktur als den verbunden werden, die eine Verbindung mit Azure AD bei der AD FS oder lokalen STS. Verwenden Sie den Wert für die AD FS http://<*Domänenname*>/Adfs/services/Trust /, wobei <*Domänenname*> der überprüften Domänenname in Azure AD ist.

Erstellen Sie diese Regeln manuell in AD FS verwenden Sie das folgende PowerShell-Skript in einer Sitzung, die mit dem Server verbunden ist. Ersetzen Sie die erste Zeile mit Ihrem Domänennamen überprüften Organisation in Azure Active Directory.

> [AZURE.NOTE] Wenn Sie für Ihren lokalen Föderation Server nicht AD FS arbeiten, führen Sie die Ihres Herstellers Anweisungen zum Erstellen von Regeln, die diese Ansprüche ausgeben.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Verwenden Sie nur die ersten drei Regeln an, wenn Ihre Umgebung lokal ist eine einzige Gesamtstruktur. Wenn Ihre Computer in einer anderen Struktur als der Synchronisierung mit Azure AD befinden oder die Sie alternative Namen für den Preisen in der Konfiguration Synchronisation verwenden, müssen Sie auch die verbleibenden Regeln einschließen.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows-10 und Windows Server 2016 Domänenverbund Computern Authentifizierung über IWA zu einem aktiven WS-Trust-Endpunkt, der von AD FS gehostet wird. Stellen Sie sicher, dass der Endpunkt aktiv ist. Wenn Sie die Anwendung Web Proxy verwenden, achten Sie darauf, dass diese Endpunkt über den Proxy veröffentlicht wird. Der Endpunkt ist Adfs/Services/Trust/13/Windowstransport. Prüfen, ob es in aktiv ist die AD FS-Verwaltungskonsole, wechseln Sie zu dem **Dienst** > **Endpunkte**. Wenn Sie für Ihren lokalen Föderation Server nicht AD FS arbeiten, führen Sie die Anweisungen des Herstellers, um sicherzustellen, dass der entsprechende Endpunkt aktiv ist.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Einrichten von AD FS für die Authentifizierung der Registrierung Gerät

Stellen Sie sicher, dass eine gültige Alternative zum kombinierte Authentifizierung für Gerät Registrierung in AD FS IWA ausgewählt ist. Dazu müssen Sie eine Regel, die durch die Authentifizierungsmethode verläuft, Transformieren Emission haben.

1. Wechseln Sie in der AD FS-Verwaltungskonsole zu **AD FS** > **Vertrauen Beziehungen** > **Verlassen vertrauen**.

2. Mit der rechten Maustaste in des Microsoft Office 365-Identitätsplattform sich verlassen Partei Trust Objekts, und wählen Sie dann **Bearbeiten anfordern Regeln**.

3.  Wählen Sie auf der Registerkarte **Emission transformieren Regeln** **Regel hinzufügen**aus.

4.  Wählen Sie in der Liste **anfordern Regel** Vorlage **Senden Ansprüche mithilfe einer benutzerdefinierten Regel**ein.

5.  Wählen Sie **Weiter**aus.

6.  Geben Sie im Feld **Regelname anfordern** **Autorisierende Methode anfordern Regel**ein.

7.  Geben Sie im Feld **anfordern Regel** diesen Befehl aus:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Geben Sie auf dem Server Föderation diesen PowerShell-Befehl aus:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> ist der sich verlassen Partei Objektname für Ihre Azure AD-Objekt Trust sich verlassen Party. Dieses Objekt ist normalerweise mit der Bezeichnung *Microsoft Office 365-Identitätsplattform*.



## <a name="deployment-and-rollout"></a>Bereitstellung und Einführung

Wenn Domänenverbund Computern die erforderlichen Komponenten entsprechen, sind sie bereit sind, mit Azure AD registrieren.

Die Windows 10 Jahrestag Update und Windows Server 2016 Domänenverbund Computer registrieren Azure AD das nächste Mal das Gerät neu gestartet wird oder wenn sich der Benutzer anmeldet Windows automatisch. Neue Computern, die Mitglied der Domäne sind registrieren Azure AD, wenn das Gerät nach der Domäne Join-Vorgang neu gestartet wird.

> [AZURE.NOTE] Windows-10 Domänenverbund Computern registrieren automatisch Azure AD nur, wenn das Gruppenrichtlinienobjekt Einführung festgelegt ist.

Ein Gruppenrichtlinienobjekt können die Einführung einer automatischen Registrierung von Windows 10 und Windows Server 2016 Domänenverbund Computern steuern. Wenn Sie die automatische Registrierung von nicht - Windows 10 Domänenverbund Computern bereit, können Sie ein Windows Installer-Paket auf Computern bereitstellen, die Sie auswählen.

> [AZURE.NOTE] Die Gruppenrichtlinien für Einführung Steuerelement wird auch die Registrierung von Windows 8.1 Domänenverbund Computern ausgelöst. Sie können die Richtlinie für die Registrierung von Windows 8.1 Domänenverbund Computer verwenden. Registrieren, oder, wenn Sie eine Kombination aus Windows-Versionen, Windows 7 oder Windows Server-Versionen, einschließlich enthalten Sie Ihre Windows - nicht 10 und Windows Server 2016 Computern mithilfe von Windows Installer-Paket.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Erstellen Sie ein Gruppenrichtlinienobjekt, um zu steuern, die UC-Bereitstellung der automatischen Registrierung

Um die Einführung einer automatischen Registrierung von Computern mit Azure AD Domänenverbund zu steuern, können Sie die Gruppenrichtlinien **Registrieren Domänenverbund Computer als Geräte** auf den Computern bereitstellen, die Sie erfassen möchten. Beispielsweise können Sie die Richtlinie zu einer Organisationseinheit oder eine Sicherheitsgruppe bereitstellen.

Wenn die Richtlinie festlegen möchten, führen Sie folgende Schritte aus:

1. Öffnen Sie Server-Manager, und wechseln Sie dann zu **Tools** > **Gruppenrichtlinien-Verwaltungskonsole**.

2. Wechseln Sie zu den Domänenknoten, der die die Domäne entspricht, für die automatische Registrierung von Windows 10 oder Windows Server 2016 Computern aktivieren möchten.

3. Mit der rechten Maustaste in **Die Richtlinienobjekte gruppieren**, und wählen Sie dann auf **neu**.

4. Geben Sie einen Namen für Ihre Gruppenrichtlinienobjekt aus. Angenommen, *Die automatische Registrierung zu Azure AD*. Wählen Sie **OK**aus.

4. Mit der rechten Maustaste in des neuen Gruppenrichtlinienobjekts, und wählen Sie dann auf **Bearbeiten**.

5. Wechseln Sie zu dem **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Gerät Registrierung**. Mit der rechten Maustaste **Register Domäne beigetreten Computern als Geräte**, und wählen Sie dann auf **Bearbeiten**.

    > [AZURE.NOTE] Diese Vorlage wurde aus früheren Versionen der Gruppenrichtlinien-Verwaltungskonsole umbenannt. Wenn Sie eine frühere Version der Verwaltungskonsole verwenden, wechseln Sie zur **Konfiguration des Computers** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Arbeitsplatz Verknüpfung** > **automatisch Arbeitsplatz Verknüpfung Clientcomputer**.

6. Wählen Sie **aktiviert**aus, und wählen Sie dann auf **Übernehmen**.

7. Wählen Sie **OK**aus.

8. Verknüpfen Sie das Gruppenrichtlinienobjekt an einem Speicherort Ihrer Wahl. Beispielsweise können Sie es mit einer bestimmten Organisationseinheit verknüpfen. Auch konnte mit einer bestimmten Sicherheitsgruppe Computer verknüpft werden, die automatisch mit Azure AD zu registrieren. Wenn diese Richtlinie für alle Domänenverbund Windows 10 und Windows Server 2016 Computer in Ihrer Organisation festlegen möchten, verknüpfen Sie das Gruppenrichtlinienobjekt mit der Domäne aus.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Herunterladen von Windows Installer-Paketen für nicht von Windows - 10-Computern  

Um Domänenverbund Computern unter Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 zu registrieren, können Sie herunterladen und installieren diese Windows Installer MSI-Dateien:

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Bereitstellen des Pakets mithilfe eines Systems für Software-Verteilung wie System Center-Konfigurations-Manager. Das Paket unterstützt die Optionen standard Hintergrundinstallation mit dem *stillen* Parameter. System Center-Konfigurations-Manager 2016 bietet weitere Vorteile aus früheren Versionen, wie die Möglichkeit zum Nachverfolgen der fertigen Registrierungen. Weitere Informationen finden Sie unter [System Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Das Installationsprogramm erstellt einen geplanten Vorgang auf dem System, das in dem Kontext des Benutzers ausgeführt wird. Die Aufgabe wird ausgelöst, wenn der Benutzer Windows anmeldet. Der Vorgang registriert das Gerät im Hintergrund mit Azure AD mit Anmeldeinformationen des Benutzers nach der Authentifizierung über IWA. Um den geplanten Vorgang anzuzeigen, wechseln Sie zu **Microsoft** > **Arbeitsplatz teilnehmen**, und navigieren Sie dann zu der Bibliothek Taskplaner.



## <a name="next-steps"></a>Nächste Schritte

- [Bedingte Azure Active Directory-Zugriff](active-directory-conditional-access.md)
