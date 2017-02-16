<properties
    pageTitle="Programme, die in Azure Active Directory Access bedingte Regeln verwenden | Microsoft Azure"
    description="Mit bedingten Access-Steuerelement überprüft Azure Active Directory für bestimmte Bedingungen, wenn sie den Benutzer authentifiziert, und klicken Sie auf die Anwendung zugreifen dürfen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Programme, die in Azure Active Directory Access bedingte Regeln verwenden

Bedingte Access Regeln werden in Azure Active Directory (Azure AD) unterstützt-Applikationen verbunden, bereits integriert partnerverbundkontakte Software als ein Service (SaaS) dazu, mit dem Kennwort einmaliges Anmelden (SSO), Linie branchenanwendungen und Anwendungen, die Azure AD-Anwendungsproxy verwenden. Eine ausführliche Liste der Anwendungen, die für die Sie bedingte Access verwenden können, finden Sie unter [Services mit bedingten Zugriff aktiviert](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Bedingte Access funktioniert beide mit mobilen und desktop-Anwendungen, die moderne Authentifizierung verwenden. In diesem Artikel behandeln wir wie bedingte Access funktioniert in mobilen und desktop-apps.

Sie können Azure AD-Anmeldung Seiten in Clientanwendungen, die moderne Authentifizierung verwenden. Mehrstufige Authentifizierung wird ein Benutzer mit einer Anmeldeseite aufgefordert. Eine Meldung wird angezeigt, wenn Sie den Benutzerzugriff blockiert wird. Modernes Authentifizierung ist für das Gerät mit Azure AD Authentifizierung erforderlich, sodass bedingte Access Gerät basierende Richtlinien ausgewertet werden.

Es ist wichtig zu wissen, welche Applikationen bedingten Zugriffsregeln, und die Schritte, die Sie ergreifen können, um eine andere Anwendung Eintrag Punkte secure benötigen möglicherweise verwenden können.

## <a name="applications-that-use-modern-authentication"></a>Moderne Authentifizierung verwenden

> [AZURE.NOTE] Wenn Sie eine bedingte Zugriffsrichtlinie in Azure AD, die eine Entsprechung in Office 365 enthält verfügen, konfigurieren Sie beide bedingte Richtlinien. Dies würden, z. B. anwenden bedingte Richtlinien für Exchange Online oder SharePoint Online für.

Die folgenden Programme unterstützen bedingte Zugriff für Office 365 und andere Azure AD-verbunden dienstanwendungen:

| Zieldienst  | Plattform  | Anwendung                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange Online | Windows-10|E-Mail-Kalender/Personen app Outlook 2016, Outlook 2013 (mit modernen Authentifizierung), Skype for Business (mit modernen Authentifizierung)|
|Office 365 Exchange Online| Windows 8.1, Windows 7 |Outlook 2016, Outlook 2013 (mit modernen Authentifizierung), Skype for Business (mit modernen Authentifizierung)|
|Office 365 Exchange Online|iOS, Android|  Outlook mobile-app|
|Office 365 Exchange Online|Mac OS X| Outlook 2016 für kombinierte Authentifizierung und Speicherort nur; für die Zukunft, Skype für Business Support für die Zukunft geplanter geplante Gerät basierende Richtlinie-Unterstützung|
|Office 365 SharePoint Online|Windows-10| Apps für Office 2016, Universal Office-apps, Office 2013 (mit modernen Authentifizierung) OneDrive für die Zukunft, für die Zukunft, SharePoint-app-Unterstützung für die Zukunft geplanter geplante Gruppen Office-Unterstützung für Business-app (nächste Generation-Synchronisierungsclient oder NGSC) unterstützt geplanter|
|Office 365 SharePoint Online|Windows 8.1, Windows 7|Office 2016 apps, Office 2013 (mit modernen Authentifizierung), OneDrive for Business-app (Groove-Synchronisierungsclient)|
|Office 365 SharePoint Online|iOS, Android|  Office mobile-apps |
|Office 365 SharePoint Online|Mac OS X| Office 2016 apps für kombinierte Authentifizierung und Speicherort nur; für die Zukunft geplante Gerät basierende Richtlinie-Unterstützung|
|Office 365 Yammer|Windows-10, iOS und Android | Yammer-app für Office|
|Dynamics CRM|Windows-10, Windows 8.1, Windows 7, iOS und Android | Dynamics CRM-app|
|PowerBI-Dienst|Windows-10, Windows 8.1, Windows 7, iOS und Android | PowerBI-app|
|Azure Remote-App-Verwaltungsdienst|Windows-10, Windows 8.1, Windows 7, iOS, Android und Mac OS X |Azure Remote-app|
|Alle meine Apps app-Verwaltungsdienst|Android und iOS|Alle meine Apps app-Verwaltungsdienst |


## <a name="applications-that-do-not-use-modern-authentication"></a>Programme, die nicht modernes Authentifizierung verwenden

Aktuell, müssen Sie andere Methoden verwenden, den Zugriff auf apps blockieren, die nicht moderne Authentifizierung verwenden. Access Regeln für apps, die moderne Authentifizierung nicht verwenden, werden nicht von bedingten Access erzwungen. Dies ist hauptsächlich ein Aspekt für Exchange und SharePoint-Zugriff. Verwenden Sie die meisten frühere Versionen von apps älteren Access Control Protokolle aus.

### <a name="control-access-in-office-365-sharepoint-online"></a>Steuern des Zugriffs in Office 365 SharePoint Online
Sie können mithilfe des Cmdlets Set-SPOTenant älteren Protokolle für SharePoint-Zugriff deaktivieren. Verwenden Sie dieses Cmdlet, um zu verhindern, dass Office-Clients, die nicht Modern Authentifizierungsprotokolle vom Zugriff auf SharePoint Online-Ressourcen verwenden.

**Beispiel-Befehl**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Steuern des Zugriffs in Office 365 Exchange Online

Exchange bietet zwei Arten von Protokollen. Überprüfen Sie die folgenden Optionen, und wählen Sie dann auf die Richtlinie, die für Ihre Organisation geeignet ist.

-   **Exchange ActiveSync**. Standardmäßig werden bedingte Richtlinien für kombinierte Authentifizierung und einen Speicherort für die Exchange ActiveSync nicht erzwungen. Sie müssen Zugriff auf diese Dienste durch Konfigurieren von Exchange ActiveSync-Richtlinie direkt oder durch das Blockieren des Exchange ActiveSync mithilfe von Regeln Active Directory Federation Services (AD FS) schützen.
-   **Legacy Protokolle**. Sie können mit dem AD FS älteren Protokolle blockieren. Dadurch wird verhindert, den Zugriff auf älteren Office-Clients, wie Office 2013 lässt sich ohne moderne Authentifizierung aktiviert ist, und frühere Versionen von Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Verwenden Sie AD FS um zu blockierenden legacy-Protokoll

Im folgenden Beispielregeln können legacy Protokollzugriff Ebene der AD FS blockieren. Wählen Sie aus zwei allgemeine Konfigurationen.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Option 1: Ermöglichen Sie Exchange ActiveSync und zulassen legacy-apps, aber nur im intranet

Die folgenden drei Regeln auf das AD FS verlassen Partei Trust für Microsoft Office 365-Identitätsplattform, Exchange ActiveSync, und Browser und moderne Authentifizierungsdatenverkehr anwenden, können Sie zugreifen. Legacy apps werden aus dem extranet blockiert.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Option 2: Zulassen Sie Exchange ActiveSync und blockieren Sie legacy-apps

Die folgenden drei Regeln auf das AD FS verlassen Partei Trust für Microsoft Office 365-Identitätsplattform, Exchange ActiveSync, und Browser und moderne Authentifizierungsdatenverkehr anwenden, können Sie zugreifen. Legacy apps werden von beliebigen Standorten blockiert.

##### <a name="rule-1"></a>Regel 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
