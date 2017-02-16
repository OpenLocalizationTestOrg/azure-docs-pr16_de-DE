<properties
    pageTitle="Active Directory Federation Services-Verwaltung und Anpassung mit Azure AD verbinden | Microsoft Azure"
    description="AD FS-Verwaltung mit Azure AD verbinden und Anpassung der Benutzer AD FS-in Erfahrung mit Azure AD verbinden und PowerShell."
    keywords="AD FS, ADFS, AD FS Verwaltung AAD verbinden, verbinden, Anmeldung AD FS Anpassung vertrauen, Office 365, Föderation, Identitätsempfänger reparieren"
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
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory Federation Services-Verwaltung und Anpassung mit Azure AD-verbinden

Dieser Artikel Details Aufgaben im Zusammenhang mit Active Directory Federation Services (AD FS), die mit Microsoft Azure Active Directory verbinden ausgeführt werden können, sowie andere allgemeinen AD FS-Aufgaben, die für eine vollständige Konfiguration von einem AD FS-Farm erforderlich sein können.

| Thema | Was es behandelt. |
|:------|:-------------|
|**AD FS-Verwaltung**|
|[Reparieren Sie die Vertrauensstellung](#repairthetrust)| Reparieren die Föderation Vertrauensstellung mit Office 365 |
|[Hinzufügen eines AD FS-server](#addadfsserver) | Erweitern der AD FS-Farm mit einer zusätzlichen AD FS-server|
|[Hinzufügen einer AD FS Web-Proxy-server](#addwapserver) | Erweitern der AD FS-Farm mit einer zusätzlichen WAP-server|
|[Hinzufügen einer verbunddomäne](#addfeddomain)| Hinzufügen einer verbunddomäne|
| **AD FS-Anpassung**|
|[Hinzufügen einer benutzerdefinierten Firmenlogo oder (Abbildung)](#customlogo)| Anpassen einer AD FS-Anmeldeseite mit einem Firmenlogo und (Abbildung) |
|[Hinzufügen einer Beschreibung Anmeldung](#addsignindescription) | Hinzufügen einer Beschreibung der Anmeldeseite |
|[Ändern von AD FS anfordern Regeln](#modclaims) | Ändern von AD FS-Ansprüchen für verschiedene Föderation Szenarien |

## <a name="ad-fs-management"></a>AD FS-Verwaltung

Azure AD verbinden bietet verschiedene Aufgaben im Zusammenhang mit dem AD FS, der mithilfe des Assistenten Azure AD-Verbinden mit minimalen Eingriff ausgeführt werden können. Nachdem Sie Azure AD verbinden mithilfe des Assistenten zum Installieren abgeschlossen haben, können Sie den Assistenten erneut aus, um zusätzliche Aufgaben ausführen.

### <a name="repair-the-trust-a-namerepairthetrusta"></a>Reparieren Sie die Vertrauensstellung<a name=repairthetrust></a>

Azure AD verbinden kann für den aktuellen Zustand der Vertrauensstellung AD FS und Azure Active Directory überprüfen und reparieren Sie die Vertrauensstellung geeignete Maßnahmen ergreifen. Wie folgt vor, um Ihre Azure AD reparieren und AD FS vertrauen.

1. Wählen Sie **Reparieren AAD und ADFS vertrauen** , aus der Liste der zusätzliche Aufgaben.
![Reparieren AAD und ADFS vertrauen](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Klicken Sie auf der Seite **mit Azure AD** Erläutern Sie Ihre Anmeldeinformationen ein globaler Administrator für Azure AD, und klicken Sie auf **Weiter**.
![Herstellen einer Verbindung Azure AD mit](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Geben Sie auf der Seite **RAS Anmeldeinformationen** die Anmeldeinformationen für den Domänenadministrator ein.
![Anmeldeinformationen für den Remote-Zugriff](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Nachdem Sie auf **Weiter**geklickt haben, Azure AD verbinden überprüft Zertifikat Gesundheit und Probleme anzuzeigen.

    ![Bundesstaat von Zertifikaten](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **Bereit zum Konfigurieren der** Seite wird die Liste der Aktionen angezeigt, die ausgeführt wird, um die Vertrauensstellung zu reparieren.

    ![So konfigurieren Sie bereit.](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Klicken Sie auf **Installieren** , um die Vertrauensstellung zu reparieren.

>[AZURE.NOTE] Azure AD verbinden können nur reparieren oder Act auf Zertifikate, die selbst signiertes sind. Azure AD-Verbinden von Drittanbietern Zertifikate nicht repariert werden.

### <a name="add-an-ad-fs-server-a-nameaddadfsservera"></a>Hinzufügen eines AD FS-server<a name=addadfsserver></a>

> [AZURE.NOTE] Verbinden von Azure AD erfordert die PFX-Zertifikatsdatei in einem AD FS-Server hinzufügen. Daher können Sie diesen Vorgang nur, wenn Sie die AD FS-Farm mit Azure AD Verbinden konfiguriert.

1. **Bereitstellen eines weiteren Föderation Servers** wählen Sie aus, und klicken Sie auf **Weiter**.
![Zusätzliche Föderation server](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Klicken Sie auf der Seite **mit Azure AD** Geben Sie Ihre Anmeldeinformationen ein globaler Administrator für Azure Anzeige, und klicken Sie auf **Weiter**.
![Herstellen einer Verbindung Azure AD mit](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Geben Sie die Domäne Administratoranmeldeinformationen.
![Administratorberechtigungen für die Domäne](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Verbinden von Azure AD fragt für das Kennwort des PFX-Datei, die Sie beim Konfigurieren der neuen AD FS-Farm mit Azure AD verbinden bereitgestellt. Klicken Sie auf **Kennwort eingeben** , um das Kennwort für die PFX-Datei anzugeben.
![Zertifikatkennwort](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Geben Sie ein Zertifikat Zertifizierungsstelle](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Geben Sie auf der Seite **AD FS-Servern** den Servernamen oder die IP-Adresse der AD FS-Farm hinzugefügt werden.
![AD FS-Server](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Navigieren in der letzten Seite des **Konfigurieren** , und klicken Sie auf **Weiter** . Nachdem Azure AD Verbinden der Server für die AD FS-Farm hinzugefügt wurde, erhalten Sie beim die Option aus, um die Verbindung zu überprüfen.
![So konfigurieren Sie bereit.](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Installation abgeschlossen](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### <a name="add-an-ad-fs-web-application-proxy-server-a-nameaddwapservera"></a>Hinzufügen einer AD FS Web-Proxy-server<a name=addwapserver></a>

> [AZURE.NOTE] Verbinden von Azure AD erfordert die PFX-Zertifikatsdatei in einem Proxyserver Anwendung hinzufügen. Daher werden Sie möglicherweise für diesen Vorgang nur, wenn Sie die AD FS-Farm mit Azure AD Verbinden konfiguriert.

1. Wählen Sie in der Liste der verfügbaren Aufgaben **Bereitstellen Web Anwendungsproxy** aus.
![Bereitstellen von Web-Anwendungsproxy](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Geben Sie die Azure globaler Administrator-Anmeldeinformationen an.
![Herstellen einer Verbindung Azure AD mit](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Geben Sie das Kennwort für die PFX-Datei, die Sie beim Konfigurieren der AD FS-Farm mit Azure AD verbinden bereitgestellt, auf der Seite **Zertifikat Zertifizierungsstelle angeben** .
![Zertifikatkennwort](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Geben Sie ein Zertifikat Zertifizierungsstelle](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Fügen Sie den Server als Webproxy-Anwendung hinzugefügt werden. Da Proxyserver der Web-Anwendung nicht der Domäne hinzugefügt werden kann, fordert Sie der Assistent zur administrativen Anmeldeinformationen auf dem Server hinzugefügt werden.
![Administrator-Serveranmeldeinformationen](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Geben Sie auf der Seite **Proxy Trust Anmeldeinformationen** administrative Anmeldeinformationen zum Konfigurieren der Proxy-Trust und zum Zugreifen auf des primären Servers in der Farm AD FS aus.
![Proxy-Trust-Anmeldeinformationen](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Klicken Sie auf der Seite **bereit zum Konfigurieren** zeigt der Assistent die Liste der Aktionen, die ausgeführt werden sollen.
![So konfigurieren Sie bereit.](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Klicken Sie auf **Installieren** , um die Konfiguration abzuschließen. Nach Abschluss die Konfiguration bietet der Assistent Ihnen die Möglichkeit, die Verbindung zu den Servern überprüfen. Klicken Sie auf **Überprüfen** , um die Konnektivität zu prüfen.
![Installation abgeschlossen](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### <a name="add-a-federated-domain-a-nameaddfeddomaina"></a>Hinzufügen einer verbunddomäne<a name=addfeddomain></a>

Es ist einfach zum Hinzufügen einer Domäne mithilfe von Azure AD-Verbinden mit Azure AD verbunden sein. Verbinden von Azure AD addiert die Domäne für die Föderation und ändert die Regeln anfordern, um den Herausgeber richtig wiederzugeben, wenn Sie mehrere Domänen Verbund mit Azure AD haben.

1. Wählen Sie zum Hinzufügen einer verbunddomäne die Aufgabe **Hinzufügen einer weiteren Azure AD-Domäne**.
![Zusätzliche Azure AD-Domäne](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Geben Sie auf der nächsten Seite des Assistenten die globaler Administrator-Anmeldeinformationen für Azure AD.
![Herstellen einer Verbindung Azure AD mit](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Bereitstellen Sie die Domäne wird auf der Seite **RAS Anmeldeinformationen** Anmeldeinformationen mit Administratorrechten.
![Anmeldeinformationen für den Remote-Zugriff](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Klicken Sie auf der nächsten Seite wird der Assistent eine Liste von Azure AD-Domänen bereitstellen, mit dem Ihrer lokalen Verzeichnis Föderation. Wählen Sie die Domäne aus der Liste aus.
![Azure AD-Domäne](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Nachdem Sie die Domäne auswählen, wird der Assistent bieten Ihnen entsprechende Informationen zu weiteren Aktionen, die der Assistent ausführt und den Einfluss der Konfiguration. In einigen Fällen Wenn Sie eine Domäne auswählen, die noch nicht in Azure AD überprüft wird wird der Assistent Informationen, die Ihnen die Domäne überprüfen beschleunigen. Weitere Informationen hierzu finden Sie unter [Hinzufügen Ihrer benutzerdefinierten Domänennamen zu Azure Active Directory](active-directory-add-domain.md) .

5. Klicken Sie auf **Weiter**, und die Seite **bereit zum Konfigurieren** wird die Liste der Aktionen, die Azure AD verbinden ausgeführt wird angezeigt. Klicken Sie auf **Installieren** , um die Konfiguration abzuschließen.
![So konfigurieren Sie bereit.](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS-Anpassung

Die folgenden Abschnitte enthalten Informationen zu einigen der allgemeinen Aufgaben, die möglicherweise ausführen, wenn Ihre AD FS-Anmeldeseite anpassen müssen.

### <a name="add-a-custom-company-logo-or-illustration-a-namecustomlogoa"></a>Hinzufügen einer benutzerdefinierten Firmenlogo oder (Abbildung)<a name=customlogo></a>

Um das Logo des Unternehmens zu ändern, klicken Sie auf **der Anmeldeseite** angezeigt wird, verwenden Sie die folgenden Windows PowerShell-Cmdlets und Syntax ein.

> [AZURE.NOTE] Die empfohlenen Maße für das Logo sind 260 x 35 @ 96 dpi mit einer Dateigröße nicht größer als 10 KB sein.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] Der Parameter *TargetName* ist erforderlich. Das Standarddesign, das mit AD FS freigegeben ist heißt Standard.


### <a name="add-a-sign-in-description-a-nameaddsignindescriptiona"></a>Hinzufügen einer Beschreibung Anmeldung<a name=addsignindescription></a>

Um eine Beschreibung der Anmeldeseite auf der **Anmeldeseite**hinzufügen möchten, verwenden Sie die folgenden Windows PowerShell-Cmdlets und die Syntax aus.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### <a name="modify-ad-fs-claim-rules-a-namemodclaimsa"></a>Ändern von AD FS anfordern Regeln<a name=modclaims></a>

AD FS unterstützt eine leistungsfähigen anfordern Sprache, die Sie zum Erstellen von benutzerdefinierten Anspruch Regeln verwenden können. Weitere Informationen finden Sie unter [Der Rolle der Sprache Regel anfordern](https://technet.microsoft.com/library/dd807118.aspx).

In den folgenden Abschnitten beschreiben, wie Sie benutzerdefinierte Regeln für einige Szenarien, die Azure AD betreffen schreiben können und AD FS-Verbund.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Unveränderlich ID bedingte auf einen Wert, die in das Attribut vorhanden

Azure AD verbinden können Sie angeben ein Attribut als Verankerung auf einer Quelle verwendet werden, wenn Sie Objekte auf Azure Active Directory synchronisiert werden. Wenn der Wert im benutzerdefinierten Attribut nicht leer ist, sollten Sie eine unveränderliche ID anfordern ' Problemdetails '. Sie möglicherweise beispielsweise **ms-ds-Consistencyguid** als das Attribut für den Anker Quelle auswählen und **ImmutableID** als **ms-ds-Consistencyguid** ausgeben, für den Fall, dass das Attribut einen Wert dafür hat möchten. Wenn kein Wert für das Attribut vorhanden ist, stellen Sie die **Objekt-GUID** als unveränderlich-ID  Sie können den Satz von benutzerdefinierten Anspruch Regeln wie im folgenden Abschnitt beschrieben erstellen.

**Regel 1: Abfragen von Attributen**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Sie sind in dieser Regel die Werte **ms-ds-Consistencyguid** und **Objekt-GUID** für den Benutzer aus Active Directory Abfragen. Ändern Sie den Namen Store, um einen geeigneten Store Namen in der AD FS-Bereitstellung. Ändern Sie Ansprüche Typ wird auch in ein anderes gemischte Ansprüche für Ihre Föderation definierten für **Objekt-GUID** und **ms-ds-Consistencyguid**.

Auch mithilfe von **Hinzufügen** und kein **Problem**, Sie vermeiden Sie das Hinzufügen einer ausgehenden Problem für die Entität, und verwenden Sie die Werte als temporäre Werte können. Sie werden den Anspruch in einer Regel für spätere ausgeben, nachdem Sie einführen, welcher Wert als unveränderlich-ID verwenden

**Regel 2: Überprüfen Sie, wenn ms-ds-Consistencyguid für den Benutzer vorhanden ist.**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Mit dieser Regel wird eine temporäre Kennzeichnung **Idflag** , die auf **Useguid** festgelegt ist, es ist keine **ms-ds-Concistencyguid** aufgefüllt für den Benutzer aufgerufen definiert. Die Logik besteht darin, dass ADFS keine leeren Ansprüche zulässt. Also wenn Sie in der Regel 1 Ansprüche http://contoso.com/ws/2016/02/identity/claims/objectguid und http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid hinzufügen, haben am Ende einer **Msdsconsistencyguid** beanspruchen nur, wenn der Wert für den Benutzer ausgefüllt wird. Wenn es nicht ausgefüllt wird, sieht AD FS an, dass es einen leeren Wert und legt es unmittelbar ab. Alle Objekte werden **Objekt-GUID**, weshalb Anspruch es immer genommen wird, nachdem die Regel 1 ausgeführt wird.

**Regel 3: Ms-ds-Consistencyguid als unveränderlich ID Emission, wenn er vorhanden ist**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dies ist eine Prüfung auf implizit **vorhanden** . Wenn der Wert für den Anspruch vorhanden ist, dann geben Sie aus, die als unveränderlich-ID Im vorhergehenden Beispiel wird die **Nameidentifier** anfordern. Sie müssen diese auf den entsprechenden anfordern unveränderlich ID in Ihrer Umgebung zu ändern.

**Regel 4: Erteilen Sie Objekt-GUID als unveränderlich ID ein, wenn ms-ds-ConsistencyGuid nicht vorhanden ist.**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

In dieser Regel, die Sie einfach die temporäre Kennzeichnung **Idflag**überprüfen. Sie entscheiden, ob den Anspruch basierend auf deren Wert ausgeben.

> [AZURE.NOTE] Die Reihenfolge der folgenden Regeln ist wichtig.

#### <a name="sso-with-a-subdomain-upn"></a>Mit einer Unterdomäne Benutzerprinzipalnamen SSO

Sie können mehrere Domänen verbunden sein, mithilfe von Azure AD-verbinden, wie unter [Hinzufügen eine neue verbunddomäne](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain)hinzufügen. Sie müssen UPN-Anspruch ändern, sodass die Domäne aus und nicht die Unterdomäne, die Herausgeber-ID entspricht, da die partnerverbundkontakte Stammdomäne auch die untergeordneten behandelt.

Standardmäßig ist die Regel anfordern Herausgeber-ID als festlegen:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standardmäßige Herausgeber-ID anfordern](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Die Standard-Regel einfach verwendet das UPN-Suffix und ihn in die Herausgeber-ID anfordern. Beispielsweise Johann ist ein Benutzer in sub.contoso.com, "contoso.com" befindet und Verbund mit Azure AD. Johann eingibt john@sub.contoso.com als bei der Anmeldung bei Azure Active Directory, den Benutzernamen und die Herausgeber-ID beanspruchen Regel in AD FS wird auf folgende Weise behandelt.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Wert beanspruchen:** http://sub.contoso.com/adfs/services/trust/

Damit nur die Root-Domäne in den Wert des Herausgebers anfordern, Ändern der anfordern Regel die folgenden Kriterien entsprechen.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Benutzeroptionen Anmeldung](active-directory-aadconnect-user-signin.md).
