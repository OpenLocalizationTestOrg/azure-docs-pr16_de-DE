<properties
    pageTitle="Schreiben von Ausdrücken für Zuordnungen Attribut in Azure-Active Directory | Microsoft Azure"
    description="Erfahren Sie, wie mit Ausdruck Zuordnungen Attributwerte in einer zulässigen Format umwandeln, während der automatisierte Bereitstellung von SaaS app-Objekten in Azure Active Directory."
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
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Schreiben von Ausdrücken für Zuordnungen Attribut in Azure-Active Directory

Beim Konfigurieren einer SaaS-Anwendung bereitgestellt ist eine Arten von Attribut Zuordnungen, die Sie angeben können, eine Zuordnung Ausdruck aus. Bei diesen müssen Sie einen Skript ähnelt Ausdruck schreiben, der es ermöglicht zu Ihrer Benutzer für den Daten in Formate transformieren, die für die Anwendung SaaS mehr zulässig sind.





## <a name="syntax-overview"></a>Syntax (Übersicht)

Die Syntax für Ausdrücke für Zuordnungen Attribut ist ähnlicher Visual Basic für Applikationen (VBA)-Funktionen.

- Der gesamte Ausdruck muss in Bezug auf die Funktionen definiert werden, die durch die Argumente in Klammern hinter dem Namen einer enthalten: <br>
*Funktionsname (<< Argument 1 >> <<argument N>>)*


- Sie können Funktionen ineinander schachteln. Beispiel: <br> *FunctionOne (FunctionTwo (<<argument1>>))*


- Sie können drei verschiedene Typen von Argumenten in Funktionen übergeben:

   1. Attributen, die in Quadratisches eckigen Klammern eingeschlossen werden müssen. Beispiel: [AttributeName]

   2. Zeichenfolgenkonstanten, die in doppelte Anführungszeichen eingeschlossen sein müssen. Beispiel: "USA"

   3. Andere Funktionen. Beispiel: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))


- Für Zeichenfolgenkonstanten Wenn Sie ein umgekehrter Schrägstrich (\) oder Anführungszeichen (") in der Zeichenfolge, benötigen muss es mit dem Symbol umgekehrter Schrägstrich (\) Escapezeichen umgewandelt werden. Beispiel: "Firmenname: \"" Contoso "\""



## <a name="list-of-functions"></a>Liste der Funktionen

[Anfügen](#append) &nbsp;&nbsp;&nbsp;&nbsp; [FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp;&nbsp;&nbsp; [Not](#not) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)





----------
### <a name="append"></a>Anfügen

**Funktion:**<br> Append(Source, Suffix)

**Beschreibung:**<br> Verwendet einen Zeichenfolgenwert Quelle und fügt das Suffix, bis zum Ende des es.
 
**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Datenquelle** | Erforderlich | Zeichenfolge | In der Regel einen Namen für das Attribut aus dem Quellobjekt |
| **Suffix** | Erforderlich | Zeichenfolge | Die Zeichenfolge, die Sie an das Ende des Werts Quelle anfügen möchten. |


----------
### <a name="formatdatetime"></a>FormatDateTime

**Funktion:**<br> FormatDateTime (Quelle, InputFormat, Ausgabeformat)

**Beschreibung:**<br> Akzeptiert eine Datumszeichenfolge von einem Format, und es in ein anderes Format konvertiert.
 
**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Datenquelle** | Erforderlich | Zeichenfolge | In der Regel Name für das Attribut aus dem Quellobjekt. |
| **inputFormat** | Erforderlich | Zeichenfolge | Erwartetes Format des Werts Quelle. Unterstützte Formate finden Sie unter [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **Ausgabeformat** | Erforderlich | Zeichenfolge | Datumsformat Ausgabe an. |



----------
### <a name="join"></a>Teilnehmen an

**Funktion:**<br> Teilnehmen an (Trennzeichen, source1 source2,...)

**Beschreibung:**<br> Join() ähnelt Append(), außer dass es mehrere **Datenquellen** Zeichenfolgenwerten zu einer einzigen Zeichenfolge kombinieren kann und jeder Wert, durch **ein Trennzeichen** getrennt werden wird.

Ist eines der Quellwerte von einem Attribut mit mehreren Werten, und klicken Sie dann auf jeder Wert im wird dieses Attribut miteinander verknüpft werden sollen, den Wert Trennzeichen getrennt.

 
**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Trennzeichen** | Erforderlich | Zeichenfolge | Die Zeichenfolge, mit der Quellwerte zu trennen, wenn sie in einer Zeichenfolge verkettet werden. Werden können "" ist keine Trennzeichen erforderlich. |
| **... Source1 SourceN** | Erforderlich, Variable-Anzahl, wie oft | Zeichenfolge | Zeichenfolge: Werte miteinander verknüpft werden. |



----------
### <a name="mid"></a>Teil

**Funktion:**<br> Mid (Quelle, Start, Länge)

**Beschreibung:**<br> Gibt eine Teilzeichenfolge des Werts Quelle. Eine Teilzeichenfolge ist eine Zeichenfolge, die nur einige Zeichen aus der Quellzeichenfolge enthält.


**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Datenquelle** | Erforderlich | Zeichenfolge | Normalerweise Name für das Attribut. |
| **Starten** | Erforderlich | ganze Zahl | Indizieren der **Quelle** Zeichenfolge Stelle, an der Teilzeichenfolge beginnen soll. Erstes Zeichen der Zeichenfolge haben, wird Index 1, Zeichen der zweiten Index 2 haben, und vieles mehr. |
| **Länge** | Erforderlich | ganze Zahl | Länge der Teilzeichenfolge. Wenn Länge außerhalb der **Quelle** Zeichenfolge endet, gibt die Funktion Teilzeichenfolge aus **Starten** Index bis zum Ende der **Quelle** Zeichenfolge zurück. |




----------
### <a name="not"></a>Nicht

**Funktion:**<br> NOT(Source)

**Beschreibung:**<br> Wird den booleschen Wert der **Quelle**gespiegelt. Wenn **"*True*" ist,** gibt "*False*". Andernfalls gibt "*True*" zurück.


**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Datenquelle** | Erforderlich | Boolesche Zeichenfolge | Erwartet **Quellwerte** sind "Wahr" oder "Falsch"... |



----------
### <a name="replace"></a>Ersetzen

**Funktion:**<br> ObsoleteReplace (Quelle, OldValue, RegexPattern, RegexGroupName, Ersatzwert, ReplacementAttributeName, Vorlage)

**Beschreibung:**<br>
Innerhalb einer Zeichenfolge Werte ersetzt. Es funktioniert anders je nach den Parametern bereitgestellt:

- Wenn **OldValue** und **Ersatzwert** bereitgestellt werden:

   - Ersetzt alle Vorkommen des in der Quelle OldValue durch Ersatzwert

- Wenn **OldValue** und **Vorlage** bereitgestellt werden:

   - Ersetzt alle Vorkommen der **OldValue** in der **Vorlage** mit dem Wert **Quelle**

- Wenn **OldValueRegexPattern**, **OldValueRegexGroupName**, **Ersatzwert** stehen zur Verfügung:

   - Alle Werte in der Quellzeichenfolge mit Ersatzwert OldValueRegexPattern passenden ersetzt

- Wenn **OldValueRegexPattern**, **OldValueRegexGroupName**, **ReplacementPropertyName** stehen zur Verfügung:

   - Wenn **Quelle** Wert vorhanden ist, wird die **Quelle** zurückgegeben.

   - Wenn die **Datenquelle** keinen Wert aufweist, verwendet **OldValueRegexPattern** und **OldValueRegexGroupName** neuer Wert aus der Eigenschaft mit **ReplacementPropertyName**extrahieren. Neuer Wert wird als Ergebnis zurückgegeben.


**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Datenquelle** | Erforderlich | Zeichenfolge | In der Regel Name für das Attribut aus dem Quellobjekt. |
| **oldValue** | Optional | Zeichenfolge | Der Wert in der **Quell-** oder **Vorlage**ersetzt werden soll. |
| **regexPattern** | Optional | Zeichenfolge | Regex Muster für den Wert in der **Datenquelle**ersetzt werden soll. Oder, wenn ReplacementPropertyName verwendet wird, Muster, Wert von Ersatz Eigenschaft zu extrahieren. |
| **regexGroupName** | Optional | Zeichenfolge | Der Name der Gruppe innerhalb **RegexPattern**. Nur, wenn ReplacementPropertyName verwendet wird, werden wir Wert dieser Gruppe als Ersatzwert aus Ersatz Eigenschaft extrahieren. |
| **Ersatzwert** | Optional | Zeichenfolge | Neuer Wert, mit alten zu ersetzen. |
| **replacementAttributeName** | Optional | Zeichenfolge | Name des das Attribut Ersatz Wert verwendet werden, wenn die Datenquelle keinen Wert aufweist. |
| **Vorlage** | Optional | Zeichenfolge | Wenn die **Vorlage** Wert angegeben wird, wir **OldValue** innerhalb der Vorlage suchen und Ersetzen Sie ihn durch den Wert für die Quelle. |



----------
### <a name="stripspaces"></a>StripSpaces

**Funktion:**<br> StripSpaces(source)

**Beschreibung:**<br> Entfernt alle Leerzeichen ("") Zeichen aus der Quellzeichenfolge.

**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Datenquelle** | Erforderlich | Zeichenfolge | Wert der **Quelle** zu aktualisieren. |



----------
### <a name="switch"></a>Wechseln

**Funktion:**<br> Wechseln (Quelle, Standardwert, Schlüssel1, Wert1, Schlüssel2, Wert2;...)

**Beschreibung:**<br> Wenn Wert für die **Quelle** ein **Schlüssel**übereinstimmt, gibt **Wert** für diesen **Schlüssel**. Wenn Wert für die **Quelle** keine Tasten übereinstimmt, gibt **Standardwert**.  **Schlüssel** und **Wert** Parameter müssen immer paarweise zugeordnet werden. Die Funktion erwartet immer eine gerade Zahl von Parametern an.

**Parameter:**<br> 

|Namen| Erforderliche / wiederholte | Typ | Notizen |
|--- | ---                 | ---  | ---   |
| **Datenquelle** | Erforderlich | Zeichenfolge | Wert der **Quelle** zu aktualisieren. |
| **Standardwert** | Optional | Zeichenfolge | Der Standardwert verwendet werden, wenn Quelle keine Tasten übereinstimmt. Dies kann leere Zeichenfolge (""). |
| **Schlüssel** | Erforderlich | Zeichenfolge | Zu **Quelle** Wert mit vergleichenden **Schlüssel** . |
| **Wert** | Erforderlich | Zeichenfolge | Neuer Wert für die **Datenquelle** mit dem Key übereinstimmen. |



## <a name="examples"></a>Beispiele

### <a name="strip-known-domain-name"></a>Bekannte Domänennamen Streifen

Sie müssen einen bekannten Domänennamen aus eines Benutzers einer e-Mail an einen Benutzer erhalten entfernen. <br>
Angenommen, wenn die Domäne "contoso.com" ist, können klicken Sie dann den folgenden Ausdruck Sie:


**Ausdruck:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Eingabe / Ausgabe (Beispiel):** <br>

- **Eingabe** (e-Mail):"john.doe@contoso.com"

- **Ergebnis**: "john.doe"


### <a name="append-constant-suffix-to-user-name"></a>Anfügen von Konstanten Suffix Benutzernamen

Bei Verwendung eines Sandkastens Vertrieb, müssen Sie eine zusätzliche Suffix an alle Benutzernamen anfügen, bevor diese synchronisiert.




**Ausdruck:** <br>
`Append([userPrincipalName], ".test"))`

**Ein-/Ausgabe-Beispiel:** <br>

- **Eingabe**: (UserPrincipalName):"John.Doe@contoso.com"


- **Ergebnis**:"John.Doe@contoso.com.test"





### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Generieren Sie Benutzeralias nach verketten Teile des vor-und Nachnamen

Sie müssen einen Benutzer alias generieren, indem Sie zuerst 3 Buchstaben des Vornamens des Benutzers und ersten 5 Buchstaben des Nachnamens des Benutzers.


**Ausdruck:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Ein-/Ausgabe-Beispiel:** <br>

- **Eingabe** (Vorname): "Johann"

- **Eingabe** (Nachname): "Doe"

- **Ergebnis**: "JohDoe"




### <a name="output-date-as-a-string-in-a-certain-format"></a>Ausgabedatum als Zeichenfolge in einem bestimmten format

Möchten Sie Datumsangaben in einer Anwendung SaaS in einem bestimmten Format zu senden. <br>
Sie möchten beispielsweise zum Formatieren von Datumswerten für ServiceNow.



**Ausdruck:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Ein-/Ausgabe-Beispiel:**

- **Eingabe** (extensionAttribute1): "20150123105347.1Z"

- **Ergebnis**: "2015 01 23"





### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Ersetzen eines Werts basierend auf vordefinierten Satz von Optionen

Sie müssen die Zeitzone des Benutzers basierend auf den Ländercode Azure AD gehörende Kehrmatrix definieren. <br>
Wenn das Bundesland eine der vordefinierten Optionen nicht dem entsprechen, verwenden Sie "Australien/Sydney" Standardwert.


**Ausdruck:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Ein-/Ausgabe-Beispiel:**

- **Eingabe** (Status): "QLD"

- **Ergebnis**: "Australien/Brisbane"


##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Automatisieren von Benutzer Bereitstellung/aufheben zu SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Anpassen von Attribut Zuordnungen für Benutzer bereitgestellt](active-directory-saas-customizing-attribute-mappings.md)
- [Bereichsdefinition Filter für Benutzer bereitgestellt](active-directory-saas-scoping-filters.md)
- [Aktivieren die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory Applications mithilfe von SCIM](active-directory-scim-provisioning.md)
- [Bereitstellung von Benachrichtigungen zu berücksichtigen](active-directory-saas-account-provisioning-notifications.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)
