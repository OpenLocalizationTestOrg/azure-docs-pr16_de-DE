
<properties
    pageTitle="Verwenden von Attributen zum Erstellen erweiterter Regeln | Microsoft Azure"
    description="Wie-, der zum Erstellen erweiterter Regeln für eine Gruppe einschließlich Regeloperatoren für Ausdrücke und Parameter unterstützt."
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
    ms.date="08/15/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules"></a>Verwenden von Attributen zum Erstellen erweiterter Regeln

Im klassische Azure-Portal bietet Ihnen die Möglichkeit zum Erstellen erweiterter Regeln um komplexere Attribut-basierte dynamische Mitgliedschaften für die Gruppen Azure Active Directory (Azure AD) aktivieren.  

Wenn alle Attribute eines Benutzers ändern, das System auswertet werden alle dynamischen Gruppe Regeln im Verzeichnis um festzustellen, ob das Attribut ändern des Benutzers eine Gruppe auslösen würde hinzugefügt oder entfernt. Wenn ein Benutzer eine Regel einer Gruppe entspricht, in dieser Gruppe als Mitglied hinzugefügt. Wenn sie die Regel einer Gruppe, die, denen Sie Mitglied sind nicht mehr erfüllen, aus dieser Gruppe als eine Mitglieder entfernt.

## <a name="to-create-the-advanced-rule"></a>Zum Erstellen der erweiterten Regel

1. Im [Azure klassischen Portal](https://manage.windowsazure.com)wählen Sie **Active Directory aus**, und öffnen Sie dann auf das Firmenverzeichnis.

2. Wählen Sie die Registerkarte **Gruppen** aus, und klicken Sie dann öffnen Sie die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren** , wählen Sie die Option **erweiterte Regel** , und geben Sie dann die erweiterte Regel in das Textfeld ein.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Erstellen einer erweiterten Regel Textkörper

Die erweiterte Regel, die Sie für die dynamischen Mitgliedschaften für Gruppen erstellen können ist im Wesentlichen ein binäre Ausdruck, der besteht aus drei Teilen und in ein Ergebnis true oder false ergibt. Die drei Bereiche sind:

- Linke parameter
- Binäre operator
- Rechte Konstante

Eine vollständige erweiterte Regel sieht ungefähr wie folgt aus: (LeftParameter BinaryOperator "RightConstant"), wobei die öffnende und schließende Klammer für den gesamten binäre Ausdruck erforderlich sind, doppelte Anführungszeichen sind erforderlich, damit die richtigen Konstante und die Syntax für den linken Parameter ist user.property. Eine erweiterte Regel kann mehrere binäre Ausdrücke durch getrennt bestehen die - und, - oder, und - nicht logische Operatoren.
Es folgen Beispiele für eine ordnungsgemäß erstellter erweiterte Regel:

- (user.department - Eq "Umsatz")- oder (user.department - Eq "Marketing")
- (user.department - Eq "Umsatz")- und - nicht (user.jobTitle-enthält "SDE")

Die vollständige Liste der unterstützten Parameter und Operatoren für Ausdrücke Regel finden Sie in den folgenden Abschnitten.

Die Gesamtlänge der Hauptteil der erweiterten Regel darf 2048 Zeichen nicht überschreiten.

> [AZURE.NOTE]
>Zeichenfolge und Regex Vorgänge Groß-und Kleinschreibung. Sie können auch ausführen Null Prüfungen mit $null als Konstante, beispielsweise, user.department - Eq $null.
Zeichenfolgen mit Angebote "sollte mit Escapezeichen ' Zeichen, beispielsweise user.department - Eq \`"Umsatz".

## <a name="supported-expression-rule-operators"></a>Unterstützte Ausdruck Regeloperatoren
Die folgende Tabelle enthält alle unterstützten Ausdruck Regeloperatoren und deren Syntax im Textkörper der erweiterten Regel verwendet werden:

| Operator        | Syntax         |
|-----------------|----------------|
| Ist nicht gleich      | Neuer-            |
| Gleich          | -eq            |
| Nicht beginnt mit: | -notStartsWith |
| Beginnt mit:     | StartsWith-    |
| Nicht enthält.    | -notContains   |
| Enthält        | -enthält      |
| Keine Übereinstimmung       | -notMatch      |
| Vergleich           | -entsprechen         |


## <a name="query-error-remediation"></a>Behebung von Abfrage zurück
Die folgende Tabelle listet mögliche Fehler und zur Behebung, wenn sie auftreten

| Fehler beim Analysieren der Abfrage     | Verwendung der Fehler       | Korrigierte Verwendung             |
|-----------------------|-------------------|-----------------------------|
| Fehler: Attribut nicht unterstützt.                                      | (user.invalidProperty - Eq "Wert")       | (user.department - Eq "Wert")<br/>Eigenschaft sollte eine aus der [Eigenschaftenliste unterstützt](#supported-properties)entsprechen.                          |
| Fehler: Operator wird auf Attribut nicht unterstützt.                       | (user.accountEnabled-WAHR enthält)                                                                               | (user.accountEnabled - Eq WAHR)<br/>Eigenschaft ist des Typs Boolean. Verwenden Sie die unterstützten Operatoren (-Eq oder - neuer) für booleschen Typ in der vorstehenden Liste aus.                                                                                                                                   |
| Fehler: Fehler beim Kompilieren der Abfrage.                                      | (user.department - Eq "Umsatz")- und (user.department - Eq "Marketing")(user.userPrincipalName-match"*@domain.ext") | (user.department - Eq "Umsatz")- und (user.department - Eq "Marketing")<br/>Logische Operatoren sollte eine aus der Liste der unterstützten Eigenschaften für den oben angegebenen übereinstimmen. (user.userPrincipalName-entsprechen ".*@domain.ext")or(user.userPrincipalName -entsprechen "@domain.ext$")Error in reguläre Ausdrücke. |
| Fehler: Binäre Ausdruck ist nicht im richtigen Format.                     | (user.department – Eq "Umsatz") (user.department - Eq "Umsatz") (user.department-Eq "Umsatz")                             | (user.accountEnabled - Eq WAHR)- und (user.userPrincipalName-enthält"alias@domain")<br/>Abfrage enthält mehrere Fehler vorliegen. Klammer nicht in den richtigen Ort.                                                                                                                            |
| Fehler: Unbekannte Fehler bei dynamische Mitgliedschaften einrichten. | (user.accountEnabled - Eq "True" und user.userPrincipalName-enthält"alias@domain")                               | (user.accountEnabled - Eq WAHR)- und (user.userPrincipalName-enthält"alias@domain")<br/>Abfrage enthält mehrere Fehler vorliegen. Klammer nicht in den richtigen Ort.                                                                                                                            |

## <a name="supported-properties"></a>Unterstützte Eigenschaften
Im folgenden sind alle Benutzereigenschaften, die Sie in die erweiterte Regel verwenden können:

### <a name="properties-of-type-boolean"></a>Eigenschaften des Typs boolean

Zulässige Operatoren

* -eq


* Neuer-


| Eigenschaften     | Zulässigen Werte  | Verwendung                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | wahr falsch      | user.accountEnabled - Eq WAHR)  |
| dirSyncEnabled | wahr falsch null | (user.dirSyncEnabled - Eq WAHR) |

### <a name="properties-of-type-string"></a>Eigenschaften des Typs string

Zulässige Operatoren

* -eq


* Neuer-


* -notStartsWith


* StartsWith-


* -enthält


* -notContains


* -entsprechen


* -notMatch

| Eigenschaften                 | Zulässigen Werte                                                                                        | Verwendung                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Ort                       | Alle Zeichenfolgenwert oder $null                                                                           | (user.city - Eq "Wert")                                   |
| Land                    | Alle Zeichenfolgenwert oder $null                                                                            | (user.country - Eq "Wert")                                |
| Abteilung                 | Alle Zeichenfolgenwert oder $null                                                                          | (user.department - Eq "Wert")                             |
| displayName                | Jeder Zeichenfolgenwert, der                                                                                 | (user.displayName - Eq "Wert")                            |
| facsimileTelephoneNumber   | Alle Zeichenfolgenwert oder $null                                                                           | (user.facsimileTelephoneNumber - Eq "Wert")               |
| Vorname                  | Alle Zeichenfolgenwert oder $null                                                                           | (user.givenName - Eq "Wert")                              |
| Position                   | Alle Zeichenfolgenwert oder $null                                                                           | (user.jobTitle - Eq "Wert")                               |
| e-Mail-Nachrichten                       | Alle Zeichenfolgenwert oder $null (SMTP-Adresse des Benutzers)                                                  | (user.mail - Eq "Wert")                                   |
| mailNickName               | Alle Zeichenfolgenwert (e-Mail-Alias des Benutzers)                                                            | (user.mailNickName - Eq "Wert")                           |
| Mobile                     | Alle Zeichenfolgenwert oder $null                                                                           | (user.mobile - Eq "Wert")                                 |
| objectId                   | GUID des User-Objekts                                                                            | (user.objectId - Eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Keine DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - Eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Alle Zeichenfolgenwert oder $null                                                                            | (user.physicalDeliveryOfficeName - Eq "Wert")             |
| Postleitzahl                 | Alle Zeichenfolgenwert oder $null                                                                            | (user.postalCode - Eq "Wert")                             |
| preferredLanguage          | ISO 639-1-code                                                                                        | (user.preferredLanguage - Eq "En-US")                      |
| sipProxyAddress            | Alle Zeichenfolgenwert oder $null                                                                            | (user.sipProxyAddress - Eq "Wert")                        |
| Bundesstaat                      | Alle Zeichenfolgenwert oder $null                                                                            | (user.state - Eq "Wert")                                  |
| streetAddress              | Alle Zeichenfolgenwert oder $null                                                                            | (user.streetAddress - Eq "Wert")                          |
| Nachname                    | Alle Zeichenfolgenwert oder $null                                                                            | (user.surname - Eq "Wert")                                |
| telephoneNumber            | Alle Zeichenfolgenwert oder $null                                                                            | (user.telephoneNumber - Eq "Wert")                        |
| usageLocation              | Zwei nummerierte Ländercode                                                                           | (user.usageLocation - Eq "USA")                             |
| userPrincipalName          | Jeder Zeichenfolgenwert, der                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| Benutzertyp                   | Mitglied Gast $null                                                                                    | (user.userType - Eq "Element")                              |

### <a name="properties-of-type-string-collection"></a>Eigenschaften des Typs String Websitesammlung

Zulässige Operatoren

* -enthält


* -notContains

| Poperties      | Zulässigen Werte                        | Verwendung                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Jeder Zeichenfolgenwert, der                      | (user.otherMails-enthält"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-enthält "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Erweiterung und benutzerdefinierten Attributen
Erweiterung und benutzerdefinierten Attributen werden in dynamischen Mitgliedschaftsregeln unterstützt.

Erweiterungsattribute werden von lokal Fenster Server Active Directory synchronisiert, und führen das Format der "ExtensionAttributeX", wobei X 1-15 entspricht.
Ein Beispiel für eine Regel, die eine Erweiterungsattribut verwendet werden sollte

(user.extensionAttribute15 - Eq "Marketing")

Benutzerdefinierte Attribute werden lokal Windows Server Active Directory oder aus einer verbundenen SaaS-Anwendung von synchronisiert und die des Formats "user.extension_[GUID]\__ [Attribut]", wobei [GUID] den eindeutigen Bezeichner in AAD für die Anwendung, die das Attribut in AAD erstellt und [Attribut] ist der Name des das Attribut aus, wie sie erstellt wurde.
Ein Beispiel für eine Regel, die ein benutzerdefiniertes Attribut verwendet wird.

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Das Attributname des benutzerdefinierten finden Sie im Verzeichnis durch Abfragen eines Benutzers des Attribut Graph Explorer, und suchen Sie nach dem Attributnamen.

## <a name="direct-reports-rule"></a>Regel direkten Vorgesetzten
Sie können nun die Mitglieder in einer Gruppe basierend auf dem Attribut Manager eines Benutzers auffüllen.

**So konfigurieren Sie eine Gruppe als Gruppe "Manager"**

1. Im Portal Azure klassischen klicken Sie auf **Active Directory**, und klicken Sie dann auf den Namen des Verzeichnisses Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen** aus, und klicken Sie dann öffnen Sie die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren** , und wählen Sie dann auf **Erweiterte Regel**.

4. Geben Sie die Regel mit der folgenden Syntax ein:

    Direkte Berichte für *Vorgesetzten für {ObectID_of_manager}*. Ist ein Beispiel für eine gültige Regel für direkten Vorgesetzten

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    Dabei ist "62e19b97-8b3d-4d4a-a106-4ce66896a863" ObjectID des Managers. Die Objekt-ID kann in Azure AD auf die **Registerkarte Profil** auf der Benutzerseite für den Benutzer gefunden werden, wer der Manager ist.

3. Wenn Sie diese Regel zu speichern, werden alle Benutzer, die die Regel erfüllen als Mitglieder der Gruppe angehören. Es kann einige Minuten für die Gruppe Anfangs gefüllt dauern.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Erstellen von Regeln für Geräteobjekte unter Verwendung von Attributen

Sie können auch eine Regel erstellen, die Geräteobjekte zur Mitgliedschaft in einer Gruppe markiert. Die folgenden Gerätattribute können verwendet werden:

| Eigenschaften              | Zulässigen Werte                  | Verwendung                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| displayName             | jeder Zeichenfolgenwert, der                | (device.displayName - Eq "Robert Iphone")                       |
| deviceOSType            | jeder Zeichenfolgenwert, der                | (device.deviceOSType - Eq "IOS")                             |
| deviceOSVersion         | Alle Zeichenfolgenwert                | (Gerät. OSVersion - Eq "9.1")                                |
| isDirSynced             | wahr falsch null                 | (device.isDirSynced - Eq "True")                             |
| isManaged               | wahr falsch null                 | (device.isManaged - Eq "False")                              |
| isCompliant             | wahr falsch null                 | (device.isCompliant - Eq "True")                             |
| deviceCategory          | jeder Zeichenfolgenwert, der                | (device.deviceCategory - Eq "")                              |
| Einträge deviceManufacturer      | jeder Zeichenfolgenwert, der                | (device.deviceManufacturer - Eq "Microsoft")                 |
| deviceModel             | jeder Zeichenfolgenwert, der                | (device.deviceModel - Eq "IPhone 7 +")                        |
| deviceOwnership         | jeder Zeichenfolgenwert, der                | (device.deviceOwnership - Eq "")                             |
| Domänenname              | jeder Zeichenfolgenwert, der                | (device.domainName - Eq "contoso.com")                       |
| enrollmentProfileName   | jeder Zeichenfolgenwert, der                | (device.enrollmentProfileName - Eq "")                       |
| isRooted                | wahr falsch null                 | (device.deviceOSType - Eq "True")                            |
| managementType          | Alle Zeichenfolgenwert                | (device.managementType - Eq "")                              |
| organizationalUnit      | jeder Zeichenfolgenwert, der                | (device.organizationalUnit - Eq "")                          |
| Geräte-ID                | eine gültige Geräte-ID                | (device.deviceId - Eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [AZURE.NOTE]
> Diese Geräteregeln werden nicht mithilfe der Dropdownliste "einfache Regel" in der klassischen Azure-Portal erstellt.


## <a name="additional-information"></a>Weitere Informationen
Die folgenden Artikel enthalten weitere Informationen zum Azure Active Directory.

* [Problembehandlung bei dynamischen Mitgliedschaften für die Gruppen](active-directory-accessmanagement-troubleshooting.md)

* [Verwalten des Zugriffs auf Ressourcen mit Azure-Active Directory-Gruppen](active-directory-manage-groups.md)

* [Azure Active Directory-Cmdlets für die Konfiguration von gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)

* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
