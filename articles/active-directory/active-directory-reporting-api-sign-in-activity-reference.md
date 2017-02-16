<properties
    pageTitle="Azure Active Directory Anmeldung Aktivitätsbericht-API-Referenz | Microsoft Azure"
    description="Referenz für die Azure Active Directory Anmeldung Aktivität Bericht-API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory Anmeldung Aktivitätsbericht-API-Referenz


Dieses Thema ist Teil einer Sammlung von Themen zu den reporting API Azure-Active Directory.  
Azure AD-Berichte bietet eine API, die Sie auf Anmeldung Aktivität Berichtsdaten mithilfe von Code oder verwandte Tools zugreifen kann.
Der Umfang dieses Themas ist zu beschleunigen Referenzinformationen zu des **Berichts-API Aktivität Anmeldung**.

Siehe:

- [Anmeldung Aktivitäten](active-directory-reporting-azure-portal.md#sign-in-activities) Weitere grundlegende Informationen
- [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) für Weitere Informationen über die reporting API.

Wenden Sie für Fragen, Probleme oder Feedback sich an [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Wer kann die API Daten zugreifen?

- Benutzer in der Rolle Sicherheitsadministrator oder Sicherheit Reader

- Globale Administratoren

- Eine beliebige app, die Autorisierung zum Zugreifen auf der API enthält (app Autorisierung kann eingerichtet werden nur basierend auf globaler Administrator Berechtigung)



## <a name="prerequisites"></a>Erforderliche Komponenten

Um diesen Bericht über die reporting-API zugreifen zu können, benötigen Sie Folgendes:

- Ein [Azure Active Directory Premium P1 oder P2 edition](active-directory-editions.md)

- Abgeschlossen die [Voraussetzungen für die Berichterstattung API Azure AD-Zugriff auf](active-directory-reporting-api-prerequisites.md). 


##<a name="accessing-the-api"></a>Zugreifen auf die-API

Sie können entweder diese API über das [Graph-Explorer](https://graphexplorer2.cloudapp.net) oder programmgesteuert Zugriff mit PowerShell. Damit PowerShell in AAD Graph REST Anrufe verwendete OData Filtersyntax korrekt interpretieren, müssen Sie die umgekehrten Apostroph verwenden (QuickInfos: Graviszeichen) Zeichen das Zeichen $ "Escapezeichen". Das Zeichen einfaches schließendes Anführungszeichen dient als [PowerShells-Escapezeichen](https://technet.microsoft.com/library/hh847755.aspx), gleicht PowerShell führen Sie eine literale Interpretation des Zeichens $, und vermeiden Sie es als Variablenname PowerShell verwirrenden (ie: $filter).

Der Schwerpunkt dieses Themas liegt auf der Graph-Explorer. Ein Beispiel PowerShell finden Sie unter dieses [PowerShell-Skript](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).


## <a name="api-endpoint"></a>Endpunkt-API

Sie können diese mit den folgenden Basis-URI API zugreifen:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Aufgrund der Datenmenge sind diese API maximal eine Millionen Datensätze zurückgegeben. 

Daten werden von dieser Anruf stapelweise zurückgegeben. Jeder Stapel weist maximal 1000 Einträge.  
Verwenden Sie den nächsten Stapel von Datensätzen, um den nächsten Link. Abrufen von [Skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) Informationen aus den ersten Satz von zurückgegebenen Datensätze. Das Überspringen Token wird am Ende des Ergebnisses festgelegt werden.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Unterstützte Filter

Sie können die Anzahl der Datensätze, die durch eine API zurückgegeben werden eingrenzen in Form eines Filters anrufen.  
Für die Anmeldung API verknüpfte Daten, die folgenden Filter werden unterstützt:

- **$top =\<Anzahl der zurückzugebenden Datensätze\> ** - ein, um die Anzahl der zurückgegebenen Datensätze zu beschränken. Dies ist ein teurer Vorgang. Verwenden Sie diesen Filter nicht, wenn Sie Tausende von Objekten zurückgeben möchten.  
- **$filter =\<der Filters-Anweisung\> ** – an, auf der Grundlage der unterstützten Filterfelder, Datensätze geben Sie interessiert



## <a name="supported-filter-fields-and-operators"></a>Unterstützte Filterfelder und Operatoren

Um den Typ von Datensätzen angeben, die Sie interessieren, können Sie eine Filter-Anweisung erstellen, die entweder eine oder eine Kombination der folgenden Filterfelder enthalten kann:

- [SigninDateTime](#signindatetime) - definiert werden, ein Datum oder Datumsbereich

- [Benutzer-ID](#userid) - definiert einen bestimmten Benutzer Grundlage des Benutzers-ID an.

- [UserPrincipalName](#userprincipalname) - definiert einen bestimmten Benutzer Grundlage der Benutzername des Benutzers Hauptbenutzer (Benutzerprinzipalnamen)

- [AppId](#appid) - definiert eine bestimmten app Grundlage des app ID

- [AppDisplayName](#appdisplayname) - definiert eine bestimmten app basiert Anzeigename des app

- [LoginStatus](#loginStatus) - definiert den Status der Benutzernamen (Erfolg / Fehler)


> [AZURE.NOTE] Graph-Explorer zu verwenden, wird Sie verwenden die Groß-/Kleinschreibung für jeden Buchstaben müssen Ihre Filterfelder.


Um den Bereich der zurückgegebenen Daten zu beschränken, können Sie Kombinationen der unterstützten Filter und Filterfelder erstellen. Beispielsweise gibt die folgende Anweisung die obersten 10 Datensätze zwischen Juli 1st 2016 und Juli 6. 2016:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Unterstützte Operatoren**: Eq, ge, le, Gt, Lt

**Beispiel**:

Verwenden ein bestimmtes Datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Verwenden einen Datumsbereich    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Notizen**:

Der Datetime-Parameter muss im UTC-Format sein. 


----------

### <a name="userid"></a>Benutzer-ID

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Notizen**:

Der Wert von Benutzer-ID wird ein Zeichenfolgenwert



----------

### <a name="userprincipalname"></a>userPrincipalName

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Notizen**:

Der Wert von UserPrincipalName wird ein Zeichenfolgenwert

----------

### <a name="appid"></a>appId

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Notizen**:

Der Wert von AppId ist ein Zeichenfolgenwert

----------


### <a name="appdisplayname"></a>appDisplayName

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Notizen**:

Der Wert von AppDisplayName ist ein Zeichenfolgenwert

----------

### <a name="loginstatus"></a>loginStatus

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=loginStatus+eq+'1'  


**Notizen**:

Es gibt zwei Optionen für die LoginStatus: 0 - Erfolg, 1 - Fehler

----------



## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie die Beispiele für gefilterten Aktivitäten anzeigen? Schauen Sie sich die [Azure Active Directory Anmeldung Aktivität Bericht API Beispiele](active-directory-reporting-api-sign-in-activity-samples.md).

- Möchten Sie mehr über die Azure AD-API reporting wissen? Finden Sie unter [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md).