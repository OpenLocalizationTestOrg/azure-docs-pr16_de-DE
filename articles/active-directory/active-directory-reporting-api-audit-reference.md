<properties
    pageTitle="Azure Active Directory Audit-API-Referenz | Microsoft Azure"
    description="Gewusst wie: Erste Schritte mit der Azure-Active Directory-Audit-API"
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
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory Audit-API-Referenz

Dieses Thema ist Teil einer Sammlung von Themen zu den reporting API Azure-Active Directory.  
Azure AD-Berichte bietet eine API, die Sie auf Daten im Überwachungsbericht mithilfe von Code oder verwandte Tools zugreifen kann.
Der Bereich dieses Themas ist zu Referenzinformationen zu den **überwachenden API**beschleunigen.

Siehe:

- [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#audit-logs) Weitere grundlegende Informationen
- [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md) für Weitere Informationen über die reporting API.

Wenden Sie für Fragen, Probleme oder Feedback sich an [AAD Reporting helfen](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Wer kann die Daten zugreifen?

- Benutzer in der Rolle Sicherheitsadministrator oder Sicherheit Reader

- Globale Administratoren

- Eine beliebige app, die Autorisierung zum Zugreifen auf der API enthält (app Autorisierung kann eingerichtet werden nur basierend auf globaler Administrator Berechtigung)



## <a name="prerequisites"></a>Erforderliche Komponenten

Um diesen Bericht über die Reporting-API zugreifen zu können, benötigen Sie Folgendes:

- Ein [Azure Active Directory kostenlosen oder eine bessere edition](active-directory-editions.md)

- Abgeschlossen die [Voraussetzungen für die Berichterstattung API Azure AD-Zugriff auf](active-directory-reporting-api-prerequisites.md). 
 

##<a name="accessing-the-api"></a>Zugreifen auf die-API

Sie können entweder diese API über das [Graph-Explorer](https://graphexplorer2.cloudapp.net) oder programmgesteuert Zugriff mit PowerShell. Damit PowerShell in AAD Graph REST Anrufe verwendete OData Filtersyntax korrekt interpretieren, müssen Sie die umgekehrten Apostroph verwenden (QuickInfos: Graviszeichen) Zeichen das Zeichen $ "Escapezeichen". Das Zeichen einfaches schließendes Anführungszeichen dient als [PowerShells-Escapezeichen](https://technet.microsoft.com/library/hh847755.aspx), gleicht PowerShell führen Sie eine literale Interpretation des Zeichens $, und vermeiden Sie es als Variablenname PowerShell verwirrenden (ie: $filter).

Der Schwerpunkt dieses Themas liegt auf der Graph-Explorer. Ein Beispiel PowerShell finden Sie unter dieses [PowerShell-Skript](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## <a name="api-endpoint"></a>Endpunkt-API


Sie können diese mit dem folgenden URI API zugreifen:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Es gibt keine Beschränkung für die Anzahl der Datensätze, die von der Azure AD-Audit-API (mithilfe von OData Seitenumbruch) zurückgegeben.
Lesen Sie für Aufbewahrung Grenzwerte auf Berichtsdaten [Reporting Aufbewahrungsrichtlinien](active-directory-reporting-retention.md)aus.

Daten werden von dieser Anruf stapelweise zurückgegeben. Jeder Stapel weist maximal 1000 Einträge.  
Verwenden Sie den nächsten Stapel von Datensätzen, um den nächsten Link. Abrufen von Skiptoken Informationen aus den ersten Satz von zurückgegebenen Datensätze. Das Überspringen Token wird am Ende des Ergebnisses festgelegt werden.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Unterstützte Filter

Sie können die Anzahl der Datensätze, die durch eine API zurückgegeben werden eingrenzen in Form eines Filters anrufen.  
Für die Anmeldung API verknüpfte Daten, die folgenden Filter werden unterstützt:

- **$top =\<Anzahl der zurückzugebenden Datensätze\> ** - ein, um die Anzahl der zurückgegebenen Datensätze zu beschränken. Dies ist ein teurer Vorgang. Verwenden Sie diesen Filter nicht, wenn Sie Tausende von Objekten zurückgeben möchten.     
- **$filter =\<der Filters-Anweisung\> ** – an, auf der Grundlage der unterstützten Filterfelder, Datensätze geben Sie interessiert



## <a name="supported-filter-fields-and-operators"></a>Unterstützte Filterfelder und Operatoren

Um den Typ von Datensätzen angeben, die Sie interessieren, können Sie eine Filter-Anweisung erstellen, die entweder eine oder eine Kombination der folgenden Filterfelder enthalten kann:

- [ActivityDate](#activitydate) - definiert werden, ein Datum oder Datumsbereich
- [ActivityType](#activitytype) - definiert den Typ einer Aktivität
- [Aktivität](#activity) - definiert die Aktivität als Zeichenfolge  
- [Akteur/Name](#actorname) - definiert den Akteur in Form eines Namens
- [Akteur/Objectid](#actorobjectid) - definiert den Akteur in Form von der Akteur ID   
- [Akteur/Upn](#actorupn) - definiert den Akteur in Form eines Benutzers Prinzip Namens (Benutzerprinzipalnamen) 
- [Ziel-Name](#targetname) - definiert das Ziel in Form eines Namens
- [Ziel/Objectid](#targetobjectid) - definiert das Ziel in Form von ID des Ziels  
- [Ziel/Upn](#targetupn) - definiert den Akteur in Form eines Benutzers Prinzip Namens (Benutzerprinzipalnamen)   




----------

### <a name="activitydate"></a>' ActivityDate '

**Unterstützte Operatoren**: Eq, ge, le, Gt, Lt

**Beispiel**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Notizen**:

DateTime sollten im UTC-format

----------

### <a name="activitytype"></a>activityType

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=activityType eq 'User'  

**Notizen**:

Groß-/Kleinschreibung

----------

### <a name="activity"></a>Aktivität

**Unterstützte Operatoren**: Eq, enthält, StartsWith

**Beispiel**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Notizen**:

Groß-/Kleinschreibung

----------

### <a name="actorname"></a>Akteur/name

**Unterstützte Operatoren**: Eq, enthält, StartsWith

**Beispiel**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Notizen**:

Groß-und Kleinschreibung

    

----------
### <a name="actorobjectid"></a>Akteur/objectId

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>Ziel-name

**Unterstützte Operatoren**: Eq, enthält, StartsWith

**Beispiel**:

    $filter=targets/any(t: t/name eq 'some name')   

**Notizen**:

Groß-und Kleinschreibung

----------

### <a name="targetupn"></a>Ziel/upn

**Unterstützte Operatoren**: Eq, StartsWith

**Beispiel**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Notizen**:

- Groß-und Kleinschreibung
- Sie müssen den vollständigen Namespace hinzufügen, wenn Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity Abfragen

----------

### <a name="targetobjectid"></a>Ziel/objectId

**Unterstützte Operatoren**: Eq

**Beispiel**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>Akteur/upn

**Unterstützte Operatoren**: Eq, StartsWith

**Beispiel**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Notizen**:

- Groß-und Kleinschreibung 
- Sie müssen den vollständigen Namespace hinzufügen, wenn Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity Abfragen

----------




## <a name="next-steps"></a>Nächste Schritte

- Möchten Sie die Beispiele für Systemaktivitäten gefilterten anzeigen? Schauen Sie sich die [Azure Active Directory überwachenden API Beispiele](active-directory-reporting-api-audit-samples.md).

- Möchten Sie mehr über die Azure AD-API reporting wissen? Finden Sie unter [Erste Schritte mit der Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md).