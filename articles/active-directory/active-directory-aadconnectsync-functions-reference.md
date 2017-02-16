<properties
    pageTitle="Synchronisieren von Azure AD verbinden: Funktionen Verweis | Microsoft Azure"
    description="Bezug der deklarative provisioning Ausdrücke in Azure AD verbinden synchronisieren."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Synchronisieren von Azure AD verbinden: über Funktionen

In Azure AD-Verbindung herstellen sind Funktionen zum Bearbeiten von Attributwert einer während der Synchronisierung verwendet.  
Die Syntax der Funktionen wird ausgedrückt in folgendem Format ein:  
`<output type> FunctionName(<input type> <position name>, ..)`

Wenn eine Funktion akzeptiert mehrere Syntax überlastet ist, werden alle gültigen Syntax aufgeführt.  
Die Funktionen sind stark eingegeben haben, und sie stellen Sie sicher, dass der Typ in entspricht den dokumentierten Typ übergeben.  
Wenn der Typ nicht übereinstimmt, wird ein Fehler ausgelöst.

Die Typen werden mit der folgenden Syntax ausgedrückt werden:

- **Papierkorb** – Binärzahl
- **Bool** – Boolesch
- **dt** – UTC-Datum/Uhrzeit
- **Enumeration** – Enumeration bekannte Konstanten
- **exp** -Ausdruck, der einen booleschen Wert ausgewertet werden soll
- **Mvbin** – mehrwertigen binäre
- **Mvstr** – mehrwertigen Zeichenfolge
- **Mvref** – mehrwertigen Bezug
- **Num** – numerischen
- **Ref** – Bezug
- **str** – Zeichenfolge
- **Varianz** – eine Variante (fast) einen anderen Typ
- **void** – zurückgeben kein Werts

Mehrwertige Attribute können nur die Funktionen mit den Typen **Mvbin**, **Mvstr**und **Mvref** arbeiten. Funktionen, mit dem **Papierkorb**, **str**und **Ref** eindeutiges und mehrwertigen Attributen arbeiten.

## <a name="functions-reference"></a>Funktionen Bezug

Liste der Funktionen | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Konvertierung** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Datum / Uhrzeit** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Jetzt](#now)
[NumFromDate](#numfromdate) |  
**Verzeichnis** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Auswertung** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Mathematik** |  
[Bitund](#bitand) | [Bitoder](#bitor) | [RandomNum](#randomnum)
**Mit mehreren Werten** |  
[Enthält](#contains) | [Zählen](#count) | [Element](#item) | [ItemOrNull](#itemornull)
[Teilnehmen an](#join) | [RemoveDuplicates](#removeduplicates) | [Teilen](#split) |
**Programmfluss** |  
[Fehler](#error) | [IIF](#iif)  | [Wechseln](#switch)
**Text** |  
[GUID](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [Kleinbst](#lcase)
[Nach links](#left) | [Länge](#len) | [LTrim](#ltrim) | [Teil](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Ersetzen](#replace)
[ReplaceChars](#replacechars) | [Rechts](#right) | [RTrim](#rtrim) | [Kürzen](#trim)
[Großbst](#ucase) | [Word](#word)

----------
### <a name="bitand"></a>Bitund

**Beschreibung:**  
Die Funktion Bitund legt angegebenen Bits nach einem Wert.

**Syntax:**  
`num BitAnd(num value1, num value2)`

- Wert1; Wert2: numerische Werte, die Funktionsschlüssel zusammen werden sollen

**Hinweise:**  
Diese Funktion wandelt beide Parameter in die binäre Darstellung und legt eine kurze Anmerkung auf:

- 0 -, wenn eine oder beide der entsprechenden Bits in *Eingabeformat* und *Kennzeichnung* 0 sind
- 1: Wenn beide der entsprechenden Bits 1 sind.

Kurzum, wird 0 zurückgegeben in allen Fällen, außer wenn die entsprechenden Bits der beiden Parameter 1 sind.

**Beispiel:**  
`BitAnd(&HF, &HF7)`  
Gibt 7 zurück, da hexadezimale "F" und "F7" auf diesen Wert ausgewertet.

----------
### <a name="bitor"></a>Bitoder

**Beschreibung:**  
Die Funktion Bitoder legt angegebenen Bits nach einem Wert.

**Syntax:**  
`num BitOr(num value1, num value2)`

- Wert1; Wert2: numerische Werte, die Operator OR zusammen werden sollen

**Hinweise:**  
Diese Funktion wandelt beide Parameter in die binäre Darstellung und legt eine kurze Anmerkung 0 und 1, wenn eine oder beide der entsprechenden Bits in Eingabeformat und Kennzeichnung 1, wenn beide der entsprechenden Bits 0 sind. Kurzum, liefert 1 in allen Fällen, es sei denn, in dem die entsprechenden Bits der beiden Parameter 0 sind.

----------
### <a name="cbool"></a>CBool

**Beschreibung:**  
Die Funktion CBool gibt einen Boolean zurück, der ausgehend von des ausgewerteten Ausdrucks

**Syntax:**  
`bool CBool(exp Expression)`

**Hinweise:**  
Wenn der Ausdruck einen Wert ungleich null ergibt, CBool Gibt WAHR zurück, liefert else False.

**Beispiel:**  
`CBool([attrib1] = [attrib2])`  

Gibt True, wenn beide Attribute aufweisen den gleichen Wert.

----------
### <a name="cdate"></a>CDate

**Beschreibung:**  
CDate-Funktion gibt einen UTC-DateTime-Wert aus einer Zeichenfolge zurück. DateTime ist kein synchron systemeigenen Attributtyp, aber wird von einigen Funktionen verwendet.

**Syntax:**  
`dt CDate(str value)`

- Value: Eine Zeichenfolge mit einem Datum, Uhrzeit, und optional (Bildschirmdruck)

**Hinweise:**  
Die zurückgegebene Zeichenfolge ist immer in UTC.

**Beispiel:**  
`CDate([employeeStartTime])`  
Gibt ein DateTime-Wert des Mitarbeiters abhängig Startzeit

`CDate("2013-01-10 4:00 PM -8")`  
Gibt einen "DateTime" zurück "2013-01-11 12:00 Uhr"

----------
### <a name="cguid"></a>CGuid

**Beschreibung:**  
Die CGuid-Funktion wandelt die Zeichenfolge Darstellung einer GUID in die entsprechende binäre Darstellung.

**Syntax:**  
`bin CGuid(str GUID)`

- Eine Zeichenfolge in diesem Muster formatiert: Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx oder {Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}

----------
### <a name="contains"></a>Enthält

**Beschreibung:**  
Contains-Funktion sucht nach einer Zeichenfolge in einem mehrwertigen Attribut

**Syntax:**  
`num Contains (mvstring attribute, str search)`-Groß-/Kleinschreibung  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-Groß-/Kleinschreibung

- Attribut: das mehrwertige Attribut zu suchen.
- Suche: Zeichenfolge in das Attribut suchen.
- Casetype: CaseInsensitive oder CaseSensitive.

Gibt den Index in das mehrwertige Attribut, in dem die Zeichenfolge gefunden wurde. Wenn die Zeichenfolge nicht gefunden wird, wird 0 zurückgegeben.

**Hinweise:**  
Für mehrwertig Zeichenfolgenattributen findet die Suche untergeordneten Zeichenfolgen in die Werte aus.  
Für den Verweis Attributen muss die zu durchsuchenden Zeichenfolge den Wert, um eine Übereinstimmung betrachtet genau übereinstimmen.

**Beispiel:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Wenn das Attribut ProxyAddresses eine primäre e-Mail-Adresse hat (erkennbar Großbuchstaben "SMTP:"), klicken Sie dann das Attribut ProxyAddress zurückzukehren, andere einen Fehler zurück.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Beschreibung:**  
Die ConvertFromBase64-Funktion wandelt den Wert der angegebenen base64-codierte in eine normale Zeichenfolge an.

**Syntax:**  
`str ConvertFromBase64(str source)`– setzt voraus Unicode für Codierung  
`str ConvertFromBase64(str source, enum Encoding)`

- Datenquelle: Base64-codierte Zeichenfolge  
- -Codierung: Unicode, ASCII, UTF8

**Beispiel**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Beide Beispiele zurückgeben "*Hallo Welt!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Beschreibung:**  
Die Funktion ConvertFromUTF8Hex konvertiert den angegebenen UTF8 Hex codierte Wert in einer Zeichenfolge an.

**Syntax:**  
`str ConvertFromUTF8Hex(str source)`

- Datenquelle: UTF8 2 Bytes codierten Sting

**Hinweise:**  
Der Unterschied zwischen dieser Funktion und in ConvertFromBase64([],UTF8), dass das Ergebnis für das Attribut DN geeignet ist.  
Dieses Format wird als DN von Azure Active Directory verwendet.

**Beispiel:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Gibt "*Hallo Welt!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Beschreibung:**  
Die Funktion ConvertToBase64 konvertiert eine Zeichenfolge in Unicode base64.  
Wandelt den Wert eines Arrays von ganzen Zahlen in die entsprechende Zeichenfolge Darstellung, die mit Base-64-Ziffern codiert ist.

**Syntax:**  
`str ConvertToBase64(str source)`

**Beispiel:**  
`ConvertToBase64("Hello world!")`  
Gibt "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Beschreibung:**  
Die Funktion ConvertToUTF8Hex konvertiert eine Zeichenfolge in einen UTF8 Hex codierte Wert an.

**Syntax:**  
`str ConvertToUTF8Hex(str source)`

**Hinweise:**  
Das Ausgabeformat dieser Funktion wird als DN Attributformat von Azure Active Directory verwendet.

**Beispiel:**  
`ConvertToUTF8Hex("Hello world!")`  
Gibt 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Zählen

**Beschreibung:**  
Die Count-Funktion gibt die Anzahl der Elemente in einem mehrwertigen Attribut

**Syntax:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Beschreibung:**  
Die CNum-Funktion akzeptiert eine Zeichenfolge und gibt einen numerischen Datentyp.

**Syntax:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Beschreibung:**  
Konvertiert eine Zeichenfolge in einem Bezug Attribut

**Syntax:**  
`ref CRef(str value)`

**Beispiel:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Beschreibung:**  
Die CStr-Funktion wandelt einen String-Datentyp.

**Syntax:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- Wert: einen numerischen Wert, Verweisattribut oder Boolesch werden können.

**Beispiel:**  
`CStr([dn])`  
Könnte zurückgeben "Cn = Helmut, dc = Contoso, dc = com"

----------
### <a name="dateadd"></a>DateAdd

**Beschreibung:**  
Gibt ein Datum mit einem Datum, zu dem ein angegebenes Zeitintervall hinzugefügt wurde.

**Syntax:**  
`dt DateAdd(str interval, num value, dt date)`

- Intervall: Zeichenfolgenausdruck, der das zurückzugebende Zeitintervall hinzugefügt werden sollen. Die Zeichenfolge muss einen der folgenden Werte enthalten:
 - JJJJ Jahr
 - f Quartal
 - m Monat
 - y-Tag des Jahres
 - d Tag
 - Weekday-w
 - WW Woche
 - h Stunde
 - n Minute
 - s zweiten
- Wert: die Anzahl der Einheiten, die Sie hinzufügen möchten. Es kann positiv (für Datumsangaben in der Zukunft) oder negativ (zum Abrufen von Datumsangaben in der Vergangenheit) sein.
- Datum: DateTime, Datum, zu dem das Intervall hinzugefügt wird.

**Beispiel:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Fügt 3 Monate hinzu, und gibt einen datetime-Wert "2001-04-01" darstellt.

----------
### <a name="datefromnum"></a>DateFromNum

**Beschreibung:**  
Formatieren ein Wert in der Werbung Datum in einen DateTime konvertiert die DateFromNum-Funktion.

**Syntax:**  
`dt DateFromNum(num value)`

**Beispiel:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Gibt einen datetime-Wert darstellt 2012-01-01 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Beschreibung:**  
Die DNComponent-Funktion gibt den Wert einer angegebenen DN-Komponente für den Wechsel von links.

**Syntax:**  
`str DNComponent(ref dn, num ComponentNumber)`

- DN: die Verweisattribut für die Interpretation von
- ComponentNumber: In den DN zurückzugebenden Komponente

**Beispiel:**  
`DNComponent([dn],1)`  
Ist der dn "Cn = Helmut, Organisationseinheit..., =" Es gibt Helmut

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Beschreibung:**  
Die DNComponentRev-Funktion gibt den Wert einer angegebenen DN-Komponente für den Wechsel von rechts (Ende).

**Syntax:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- DN: die Verweisattribut für die Interpretation von
- ComponentNumber - Komponente in den DN zurück
- Optionen: DC – alle Komponenten mit ignorieren "dc ="

**Beispiel:**  
Ist dn "Cn = Helmut, Organisationseinheit = Aachen, Organisationseinheit = GA, Organisationseinheit = US, dc = Contoso, dc = com" klicken  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Beide uns.

----------
### <a name="error"></a>Fehler

**Beschreibung:**  
Die Fehlerfunktion wird verwendet, um einen benutzerdefinierten Fehler zurückzukehren.

**Syntax:**  
`void Error(str ErrorMessage)`

**Beispiel:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Wenn das Attribut Kontoname nicht vorhanden ist, Auslösen eines Fehlers auf das Objekt.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Beschreibung:**  
Die EscapeDNComponent-Funktion akzeptiert eine Komponente eines DN und versieht es, sodass es in LDAP dargestellt werden kann.

**Syntax:**  
`str EscapeDNComponent(str value)`

**Beispiel:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Sichergestellt, dass das Objekt in einem LDAP-Verzeichnis erstellt werden kann, selbst wenn das DisplayName-Attribut Zeichen enthält, die im LDAP-Escapezeichen umgewandelt werden müssen.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Beschreibung:**  
FormatDateTime-Funktion wird verwendet, um einen datetime-Wert in eine Zeichenfolge mit einem angegebenen Format zu formatieren.

**Syntax:**  
`str FormatDateTime(dt value, str format)`

- Wert: ein Wert im DateTime-Format
- Format: eine Zeichenfolge mit das Format zu konvertieren.

**Hinweise:**  
Die möglichen Werte für das Format finden Sie hier: [Benutzerdefinierte Datums-/Uhrzeitformate (Format-Funktion)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Beispiel:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
"2007-12-25" ergibt.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Kann "20140905081453.0Z" führen

----------
### <a name="guid"></a>GUID

**Beschreibung:**  
Die Funktion GUID generiert eine neue zufällige GUID

**Syntax:**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**Beschreibung:**  
Die Wenn-Funktion gibt einen von eine Reihe von möglichen Werten basierend auf eine angegebene Bedingung.

**Syntax:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- Bedingung: ein beliebiger Wert oder Ausdruck, der zu true oder false ausgewertet werden kann.
- WertWennWahr: Wenn die Bedingung als true, den zurückgegebenen Wert ausgewertet wird.
- Funktion Valueiffalse zurück: Wenn die Bedingung falsch, der zurückgegebene Wert ergibt.

**Beispiel:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Wenn der Benutzer eine Intern ist, gibt, dass der Alias eines Benutzers mit "t-" an den Anfang der es sonst hinzugefügt des Benutzers Alias ungeändert zurückgegeben.

----------
### <a name="instr"></a>InStr

**Beschreibung:**  
InStr-Funktion ermittelt die ersten Auftretens einer Teilzeichenfolge in einer Zeichenfolge

**Syntax:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- StringCheck: zu durchsuchenden Zeichenfolge
- StringMatch: Zeichenfolge gefunden werden
- Starten: Startposition die Teilzeichenfolge genügt
- vergleichen: VbTextCompare oder VbBinaryCompare

**Hinweise:**  
Gibt die Position, wo die Teilzeichenfolge gefunden wurde, oder 0, wenn nicht gefunden.

**Beispiel:**  
`InStr("The quick brown fox","quick")`  
Evalues auf 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Ausgewertet zu 7

----------
### <a name="instrrev"></a>InStrRev

**Beschreibung:**  
InStrRev-Funktion sucht das letzte Vorkommen einer Teilzeichenfolge in einer Zeichenfolge

**Syntax:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- StringCheck: zu durchsuchenden Zeichenfolge
- StringMatch: Zeichenfolge gefunden werden
- Starten: Startposition die Teilzeichenfolge genügt
- vergleichen: VbTextCompare oder VbBinaryCompare

**Hinweise:**  
Gibt die Position, wo die Teilzeichenfolge gefunden wurde, oder 0, wenn nicht gefunden.

**Beispiel:**  
`InStrRev("abbcdbbbef","bb")`  
Gibt 7

----------
### <a name="isbitset"></a>IsBitSet

**Beschreibung:**  
Die Funktion IsBitSet überprüft, wenn ein wenig wird oder nicht festgelegt.

**Syntax:**  
`bool IsBitSet(num value, num flag)`

- Wert: einen numerischen Wert, der evaluated.flag: einen numerischen Wert, der das Bit ausgewertet werden soll

**Beispiel:**  
`IsBitSet(&HF,4)`  
Gibt WAHR zurück, weil Bit "4" in den hexadezimalen Wert "F" festgelegt wird

----------
### <a name="isdate"></a>IsDate

**Beschreibung:**  
Wenn der Ausdruck sein kann als eines DateTime-Datentyps ausgewertet und dann die IsDate-Funktion als WAHR ausgewertet wird.

**Syntax:**  
`bool IsDate(var Expression)`

**Hinweise:**  
Verwendet, um festzustellen, ob CDate() erfolgreich sein können.

----------
### <a name="isempty"></a>IsEmpty

**Beschreibung:**  
Wenn das Attribut in der CS oder MV vorhanden ist jedoch auf eine leere Zeichenfolge ausgewertet wird, ist die Funktion IsEmpty wahr.

**Syntax:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Beschreibung:**  
Wenn die Zeichenfolge in einen GUID konvertiert werden kann, klicken Sie dann die Funktion IsGuid true ausgewertet.

**Syntax:**  
`bool IsGuid(str GUID)`

**Hinweise:**  
Eine GUID wird als Zeichenfolge folgen einem dieser Muster definiert: Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx oder {Xxxxxxxx-Xxxx-Xxxx-Xxxx-Xxxxxxxxxxxx}

Verwendet, um festzustellen, ob CGuid() erfolgreich sein können.

**Beispiel:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Wenn die StrAttribute ein GUID-Format hat, eine binäre Darstellung zurückzukehren, andernfalls einen NULL-Wert zurück.

----------
### <a name="isnull"></a>IsNull

**Beschreibung:**  
Wenn der Ausdruck Null ergibt, gibt die Funktion IsNull true zurück.

**Syntax:**  
`bool IsNull(var Expression)`

**Hinweise:**  
Für ein Attribut wird ein NULL-Wert durch das Fehlen das Attribut ausgedrückt.

**Beispiel:**  
`IsNull([displayName])`  
Gibt WAHR zurück, wenn das Attribut nicht in der CS oder MV vorhanden ist.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Beschreibung:**  
Wenn der Ausdruck Null oder eine leere Zeichenfolge ist, gibt die Funktion IsNullOrEmpty true zurück.

**Syntax:**  
`bool IsNullOrEmpty(var Expression)`

**Hinweise:**  
Für ein Attribut würde dies den Wert True ergeben, wenn das Attribut nicht vorhanden ist oder vorhanden ist, jedoch wird eine leere Zeichenfolge zurück.  
Die Umkehrung dieser Funktion heißt IsPresent.

**Beispiel:**  
`IsNullOrEmpty([displayName])`  
Gibt WAHR zurück, wenn das Attribut nicht vorhanden ist oder eine leere Zeichenfolge in der CS oder MV ist.

----------
### <a name="isnumeric"></a>IsNumeric

**Beschreibung:**  
Die IsNumeric-Funktion gibt der boolesche Wert, der angibt, ob ein Ausdruck als Zahl Typ ausgewertet werden kann.

**Syntax:**  
`bool IsNumeric(var Expression)`

**Hinweise:**  
Verwendet, um festzustellen, ob CNum() erfolgreich zu analysieren des Ausdrucks werden können.

----------
### <a name="isstring"></a>IsString

**Beschreibung:**  
Wenn der Ausdruck in einen Zeichenfolgentyp ausgewertet werden kann, ist die Funktion IsString wahr.

**Syntax:**  
`bool IsString(var expression)`

**Hinweise:**  
Verwendet, um festzustellen, ob CStr() erfolgreich zu analysieren des Ausdrucks werden können.

----------
### <a name="ispresent"></a>IsPresent

**Beschreibung:**  
Wenn der Ausdruck zu einer Zeichenfolge ausgewertet, die nicht Null und nicht leer ist wird, gibt die Funktion IsPresent true zurück.

**Syntax:**  
`bool IsPresent(var expression)`

**Hinweise:**  
Die Umkehrung dieser Funktion heißt IsNullOrEmpty.

**Beispiel:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Element

**Beschreibung:**  
Die Element-Funktion gibt ein Element aus einer Zeichenfolge/Attribut mit mehreren Werten.

**Syntax:**  
`var Item(mvstr attribute, num index)`

- Attribut: Attribut mit mehreren Werten
- Index: Index zu einem Element in der Zeichenfolge mit mehreren Werten.

**Hinweise:**  
Die Funktion Element eignet sich zusammen mit der Funktion enthält, da die Funktion letztere den Index zu einem Element in der mehrwertigen Attribut zurückgibt.

Löst einen Fehler an, wenn Index außerhalb des zulässigen Bereichs liegt.

**Beispiel:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Gibt die primäre e-Mail-Adresse.

----------
### <a name="itemornull"></a>ItemOrNull

**Beschreibung:**  
Die ItemOrNull-Funktion gibt ein Element aus einer Zeichenfolge/Attribut mit mehreren Werten.

**Syntax:**  
`var ItemOrNull(mvstr attribute, num index)`

- Attribut: Attribut mit mehreren Werten
- Index: Index zu einem Element in der Zeichenfolge mit mehreren Werten.

**Hinweise:**  
Die Funktion ItemOrNull eignet sich in Verbindung mit der Funktion enthält, da die Funktion letztere den Index zu einem Element in der mehrwertigen Attribut zurückgibt.

Wenn Index außerhalb des zulässigen Bereichs ist, gibt einen Null-Wert zurück.

----------
### <a name="join"></a>Teilnehmen an

**Beschreibung:**  
Die Verknüpfung-Funktion akzeptiert eine Zeichenfolge mit mehreren Werte und gibt eine Zeichenfolge eindeutiges mit angegebene Trennzeichen zwischen den Elementen eingefügt.

**Syntax:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- Attribut: Attribut mit mehreren Werten, die Zeichenfolgen verknüpft werden.
- Trennzeichen: eine beliebige Zeichenfolge, verwendet, um die untergeordneten Zeichenfolgen in die zurückgegebene Zeichenfolge getrennt. Wenn nicht angegeben, wird das Leerzeichen ("") verwendet wird. Trennzeichen ist eine leere Zeichenfolge ("") oder nichts, werden alle Elemente in der Liste ohne Trennzeichen verkettet.

**Hinweise**  
Es gibt Unstimmigkeit zwischen den Funktionen teilnehmen an und teilen. Die Verknüpfung-Funktion akzeptiert ein Array von Zeichenfolgen und verknüpft mit Zeichenfolge für ein Trennzeichen, und gibt eine einzige Zeichenfolge zurück. Die geteilten-Funktion akzeptiert eine Zeichenfolge und trennt diese am Trennzeichen und gibt ein Array von Zeichenfolgen zurückzugeben. Ein wichtiger Unterschied ist jedoch, dass Zeichenfolgen mit beliebigen Trennzeichen können verketten, teilen kann nur Zeichenfolgen mit einem einzelnen Trennzeichen trennen.

**Beispiel:**  
`Join([proxyAddresses],",")`  
Konnte zurück:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>Kleinbst

**Beschreibung:**  
Kleinbst-Funktion konvertiert alle Zeichen in einer Zeichenfolge in Kleinbuchstaben.

**Syntax:**  
`str LCase(str value)`

**Beispiel:**  
`LCase("TeSt")`  
Gibt "Test".

----------
### <a name="left"></a>Nach links

**Beschreibung:**  
Left-Funktion gibt eine bestimmte Anzahl von Zeichen von der linken Seite einer Zeichenfolge zurück.

**Syntax:**  
`str Left(str string, num NumChars)`

- Zeichenfolge: die Zeichenfolge zurückzugebenden Zeichen
- NumChars: eine Nummer, kennzeichnet die Anzahl von Zeichen vom Anfang der Zeichenfolge (links) zurück

**Hinweise:**  
Eine Zeichenfolge, die die ersten NumChars-Zeichen in der Zeichenfolge enthält:

- Wenn NumChars = 0, leere Zeichenfolge zurück.
- Wenn NumChars < 0 zurück eingegebene Zeichenfolge.
- Wenn die Zeichenfolge null ist, leere Zeichenfolge zurück.

Wenn Zeichenfolge weniger Zeichen als die Nummer im angegebenen NumChars enthält, wird eine Zeichenfolge identisch mit Zeichenfolge (die alle Zeichen in Parameter 1 mit ist,) zurückgegeben.

**Beispiel:**  
`Left("John Doe", 3)`  
Gibt "Joh".

----------
### <a name="len"></a>Länge

**Beschreibung:**  
Len-Funktion gibt die Anzahl von Zeichen in einer Zeichenfolge zurück.

**Syntax:**  
`num Len(str value)`

**Beispiel:**  
`Len("John Doe")`  
Gibt 8

----------
### <a name="ltrim"></a>LTrim

**Beschreibung:**  
Die Funktion LTrim-entfernt führende Leerzeichen aus einer Zeichenfolge enthält.

**Syntax:**  
`str LTrim(str value)`

**Beispiel:**  
`LTrim(" Test ")`  
Gibt "Test"

----------
### <a name="mid"></a>Teil

**Beschreibung:**  
Die Funktion Teil gibt eine bestimmte Anzahl von Zeichen einer bestimmten Position in einer Zeichenfolge zurück.

**Syntax:**  
`str Mid(str string, num start, num NumChars)`

- Zeichenfolge: die Zeichenfolge zurückzugebenden Zeichen
- Starten: eine Nummer, kennzeichnet das Start-positionieren innerhalb der Zeichenfolge in Zeichen zurück.
- NumChars: eine Zahl, die die Anzahl der Zeichen identifiziert Position in einer Zeichenfolge zurück

**Hinweise:**  
Zurückgeben von NumChars-Zeichen vom Anfang der Position in der Zeichenfolge ab.  
Eine Zeichenfolge, die mit NumChars-Zeichen vom Anfang der Position in der Zeichenfolge:

- Wenn NumChars = 0, leere Zeichenfolge zurück.
- Wenn NumChars < 0 zurück eingegebene Zeichenfolge.
- Wenn start > die Länge der Zeichenfolge, einer Zeichenfolge zurück.
- Wenn starten < = 0, liefert eingegebene Zeichenfolge zurück.
- Wenn die Zeichenfolge null ist, leere Zeichenfolge zurück.

Wenn es keine NumChar Zeichen, die verbleibende Zeichenfolge ab der Position Start, sind wie viele Zeichen wie möglich zurückgegeben werden.

**Beispiel:**  
`Mid("John Doe", 3, 5)`  
Gibt "hn führen".

`Mid("John Doe", 6, 999)`  
Gibt "Doe"

----------
### <a name="now"></a>Jetzt

**Beschreibung:**  
Die Now-Funktion gibt einen datetime-Wert zurück, der das aktuelle Datum und die Uhrzeit, gemäß Systemdatum und die Uhrzeit des Computers angibt.

**Syntax:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Beschreibung:**  
Die NumFromDate-Funktion gibt ein Datum in der Werbung Datumsformat.

**Syntax:**  
`num NumFromDate(dt value)`

**Beispiel:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Gibt 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Beschreibung:**  
Den PadLeft Funktion links-Blöcken eine Zeichenfolge, um einen bestimmten Zeitraum unter Verwendung eines Zeichens bereitgestellten Abstand.

**Syntax:**  
`str PadLeft(str string, num length, str padCharacter)`

- Zeichenfolge: die Zeichenfolge, die Wähltastatur.
- Länge: eine ganze Zahl, die die gewünschte Länge der Zeichenfolge darstellt.
- PadCharacter: eine Zeichenfolge, die aus einem einzelnen Zeichen, das als Füllzeichen verwendet

**Hinweise:**

- Wenn die Länge der Zeichenfolge kleiner als die Länge ist, wird PadCharacter wiederholt angefügt, an den Anfang (links) der Zeichenfolge, bis er eine Länge aufweist Länge gleich.
- PadCharacter ein Leerzeichen werden kann, jedoch keinen null-Wert.
- Wenn die Länge der Zeichenfolge gleich oder größer als die Länge ist, wird die Zeichenfolge unverändert zurückgegeben.
- Wenn Zeichenfolge eine Länge, die größer oder gleich Länge aufweist, wird eine Zeichenfolge, die identisch mit Zeichenfolge zurückgegeben.
- Wenn die Länge der Zeichenfolge kleiner als die Länge ist, wird eine neue Zeichenfolge, der die gewünschte Länge enthaltenden Zeichenfolge mit einer PadCharacter aufgefüllt zurückgegeben.
- Wenn die Zeichenfolge null ist, gibt die Funktion eine leere Zeichenfolge zurück.

**Beispiel:**  
`PadLeft("User", 10, "0")`  
Gibt "000000User".

----------
### <a name="padright"></a>PadRight

**Beschreibung:**  
Den PadRight Funktion rechts-Blöcken eine Zeichenfolge, um einen bestimmten Zeitraum unter Verwendung eines Zeichens bereitgestellten Abstand.

**Syntax:**  
`str PadRight(str string, num length, str padCharacter)`

- Zeichenfolge: die Zeichenfolge, die Wähltastatur.
- Länge: eine ganze Zahl, die die gewünschte Länge der Zeichenfolge darstellt.
- PadCharacter: eine Zeichenfolge, die aus einem einzelnen Zeichen, das als Füllzeichen verwendet

**Hinweise:**

- Wenn die Länge der Zeichenfolge kleiner als die Länge ist, wird PadCharacter wiederholt angefügt bis zum Ende der Zeichenfolge (rechts), bis es sich um eine Länge gleich Länge aufweist.
- PadCharacter ein Leerzeichen werden kann, jedoch keinen null-Wert.
- Wenn die Länge der Zeichenfolge gleich oder größer als die Länge ist, wird die Zeichenfolge unverändert zurückgegeben.
- Wenn Zeichenfolge eine Länge, die größer oder gleich Länge aufweist, wird eine Zeichenfolge, die identisch mit Zeichenfolge zurückgegeben.
- Wenn die Länge der Zeichenfolge kleiner als die Länge ist, wird eine neue Zeichenfolge, der die gewünschte Länge enthaltenden Zeichenfolge mit einer PadCharacter aufgefüllt zurückgegeben.
- Wenn die Zeichenfolge null ist, gibt die Funktion eine leere Zeichenfolge zurück.

**Beispiel:**  
`PadRight("User", 10, "0")`  
Gibt "User000000".

----------
### <a name="pcase"></a>PCase

**Beschreibung:**  
Die PCase-Funktion wandelt das erste Zeichen aller Leerzeichen getrennt Wörter in einer Zeichenfolge in Großbuchstaben und alle anderen Zeichen in Kleinbuchstaben konvertiert werden.

**Syntax:**  
`String PCase(string)`

**Hinweise:**

- Diese Funktion bietet derzeit keine gemischte Groß-und Kleinschreibung, um ein Wort konvertiert, die vollständig in Großbuchstaben, z. B. ein Akronym ist.

**Beispiel:**  
`PCase("TEsT")`  
Gibt "Test".

`PCase(LCase("TEST"))`  
Gibt "Test"

----------
### <a name="randomnum"></a>RandomNum

**Beschreibung:**  
Die RandomNum-Funktion gibt eine Zufallszahl zwischen einem angegebenen Intervall.

**Syntax:**  
`num RandomNum(num start, num end)`

- Starten: eine Zahl identifizieren die Untergrenze des Werts Zufallszahl generieren
- Ende: eine Zahl, identifizieren die Obergrenze des Werts Zufallszahl generieren

**Beispiel:**  
`Random(100,999)`  
Kann 734 zurückgeben.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Beschreibung:**  
Die RemoveDuplicates-Funktion akzeptiert eine Zeichenfolge mit mehreren Werte, und stellen Sie sicher, dass jeder Wert eindeutig ist.

**Syntax:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Beispiel:**  
`RemoveDuplicates([proxyAddresses])`  
Gibt ein sicherer ProxyAddress Attribut, in dem alle doppelten Werte wurden aktualisiert.

----------
### <a name="replace"></a>Ersetzen

**Beschreibung:**  
Die Replace-Funktion ersetzt alle Vorkommen einer Zeichenfolge in einer anderen Zeichenfolge.

**Syntax:**  
`str Replace(str string, str OldValue, str NewValue)`

- Zeichenfolge: Ersetzen von Werten in einer Zeichenfolge.
- OldValue: Die Zeichenfolge zum für Suchen und ersetzen.
- Neuerwert: Die Zeichenfolge zu ersetzen.

**Hinweise:**  
Die Funktion erkennt die folgenden Inhalte Moniker:

- \n – neue Zeile
- \r – Wagenrücklauf
- \t – Registerkarte

**Beispiel:**  
`Replace([address],"\r\n",", ")`  
CRLF durch ein Komma und ein Leerzeichen ersetzt, und kann dazu führen, dass "1-Microsoft Möglichkeit, Redmond, WA, USA"

----------
### <a name="replacechars"></a>ReplaceChars

**Beschreibung:**  
Die Funktion ReplaceChars ersetzt alle Vorkommen der Zeichen in der Zeichenfolge ReplacePattern gefunden.

**Syntax:**  
`str ReplaceChars(str string, str ReplacePattern)`

- Zeichenfolge: einer Zeichenfolge in Zeichen zu ersetzen.
- ReplacePattern: eine Zeichenfolge, die ein Wörterbuch mit den zu ersetzenden Zeichen enthält.

Das Format ist {source1}: {target1}, {source2}: {target2}, {SourceN}, {TargetN} Quelle ist, in dem das Zeichen suchen und Ersetzen durch die Zeichenfolge adressieren.

**Hinweise:**

- Die Funktion jedes Vorkommen des definierten Quellen akzeptiert und mit den Zielen ersetzt.
- Die Datenquelle muss genau Zeichens (Unicode).
- Die Quelle kann nicht leer oder mehr als ein Zeichen (Parser-Fehler) sein.
- Das Ziel kann mehrere Zeichen, beispielsweise Ö:oe, β:ss verfügbar.
- Das Ziel kann leer sein, gibt an, dass das Zeichen entfernt werden soll.
- Die Quelle wird die Groß-/Kleinschreibung beachtet und muss eine genaue Übereinstimmung.
- (Komma) und: (Doppelpunkt) sind reservierte Zeichen und kann nicht mithilfe dieser Funktion ersetzt werden.
- Leerzeichen und anderen Zeichen, die in der Zeichenfolge ReplacePattern weißen werden ignoriert.

**Beispiel:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Gibt Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Gibt "ONeil", die einzelnen Teilstriche ist definiert werden entfernt.

----------
### <a name="right"></a>Rechts

**Beschreibung:**  
Right-Funktion gibt eine bestimmte Anzahl von Zeichen von rechts (Ende) einer Zeichenfolge zurück.

**Syntax:**  
`str Right(str string, num NumChars)`

- Zeichenfolge: die Zeichenfolge zurückzugebenden Zeichen
- NumChars: eine Zahl, die die Anzahl der Zeichen identifiziert (rechts) Ende einer Zeichenfolge zurück

**Hinweise:**  
NumChars-Zeichen werden von der letzten Position der Zeichenfolge zurückgegeben.

Eine Zeichenfolge, die die letzten NumChars-Zeichen in einer Zeichenfolge enthält:

- Wenn NumChars = 0, leere Zeichenfolge zurück.
- Wenn NumChars < 0 zurück eingegebene Zeichenfolge.
- Wenn die Zeichenfolge null ist, leere Zeichenfolge zurück.

Wenn Zeichenfolge weniger Zeichen als die Nummer im angegebenen NumChars enthält, wird eine Zeichenfolge identisch mit Zeichenfolge zurückgegeben.

**Beispiel:**  
`Right("John Doe", 3)`  
Gibt "Doe".

----------
### <a name="rtrim"></a>RTrim

**Beschreibung:**  
Die Funktion RTrim-entfernt nachgestellte Leerzeichen aus einer Zeichenfolge enthält.

**Syntax:**  
`str RTrim(str value)`

**Beispiel:**  
`RTrim(" Test ")`  
Gibt "Test".

----------
### <a name="split"></a>Teilen

**Beschreibung:**  
Die geteilten-Funktion akzeptiert eine Zeichenfolge, die durch Trennzeichen getrennt und eine mehrwertige Zeichenfolge erleichtert.

**Syntax:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- Wert: die Zeichenfolge mit ein Trennzeichen voneinander trennen.
- Trennzeichen: einzelnes Zeichen als Trennzeichen verwendet werden soll.
- Beschränkung: maximale Anzahl von Werten, die zurückgegeben werden kann.

**Beispiel:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Gibt eine mehrwertige Zeichenfolge mit 2 Elementen für das Attribut ProxyAddress hilfreich.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Beschreibung:**  
Die StringFromGuid-Funktion akzeptiert eine binäre GUID und wandelt sie in einer Zeichenfolge

**Syntax:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Beschreibung:**  
Die StringFromSid-Funktion wandelt ein Byte-Array, die eine Sicherheits-ID in eine Zeichenfolge enthält.

**Syntax:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Wechseln

**Beschreibung:**  
Switch-Funktion wird verwendet, um einen einzelnen Wert, basierend auf Bedingungen ausgewerteten zurückzukehren.

**Syntax:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- Ausdruck: Variant-Ausdruck ausgewertet werden soll.
- Wert: Wert zurückgegeben werden soll, wenn der entsprechende Ausdruck True ist.

**Hinweise:**  
Die Switch-Funktion Argumentliste besteht aus Paare von Ausdrücken und Werten. Die Ausdrücke werden von links nach rechts ausgewertet, und der Wert zugeordnet ist, mit dem ersten Ausdruck als WAHR ausgewertet wird zurückgegeben. Wenn die Teile nicht richtig paarweise angegeben werden, tritt ein Laufzeitfehler auf.

Wenn Ausdruck1 wahr ist, gibt wechseln beispielsweise Wert1 zurück. Wenn Ausdruck-1 falsch ist, aber Ausdruck-2 gleich WAHR ist, gibt wechseln Wert-2 usw. an.

Switch gibt eine nichts ist:

- Keiner der Ausdrücke sind wahr.
- Der erste WAHR Ausdruck weist einen entsprechenden Wert, der NULL ist.

Wechseln wertet alle Ausdrücke, auch wenn nur eine von ihnen zurückgegeben. Daher sollten Sie für unerwünschten achten. Angenommen, wenn die Auswertung eines Ausdrucks einer Division durch 0 (null) zu einem Fehler führt, tritt ein Fehler auf.

Wert kann auch die Fehlerfunktion sein, die eine benutzerdefinierte Zeichenfolge zurückgegeben würde.

**Beispiel:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Gibt die Sprache, die in einige wichtigsten Orte, andernfalls wird einen Fehler zurückgegeben.

----------
### <a name="trim"></a>Kürzen

**Beschreibung:**  
Die Funktion GLÄTTEN entfernt führende und nachfolgende Leerzeichen aus einer Zeichenfolge enthält.

**Syntax:**  
`str Trim(str value)`  

**Beispiel:**  
`Trim(" Test ")`  
Gibt "Test".

`Trim([proxyAddresses])`  
Entfernt führende und nachfolgende Leerzeichen für jeden Wert in das Attribut ProxyAddress.

----------
### <a name="ucase"></a>Großbst

**Beschreibung:**  
Großbst-Funktion wandelt alle Zeichen in einer Zeichenfolge in Großbuchstaben.

**Syntax:**  
`str UCase(str string)`

**Beispiel:**  
`UCase("TeSt")`  
Gibt "TEST".

----------
### <a name="word"></a>Word

**Beschreibung:**  
Die Word-Funktion gibt ein Wort innerhalb einer Zeichenfolge, die auf Grundlage von Parametern, beschreibt die Trennzeichen verwenden und wie Word zurückzugebenden enthalten.

**Syntax:**  
`str Word(str string, num WordNumber, str delimiters)`

- Zeichenfolge: die Zeichenfolge, um ein Wort aus zurückzukehren.
- WordNumber: eine Nummer, welche Word-Nummer kennzeichnet sollte zurückgeben.
- Trennzeichen: eine Zeichenfolge, die die Delimiter(s), die verwendet werden soll, um Wörter zu identifizieren darstellt.

**Hinweise:**  
Jede Zeichenfolge in Zeichenfolge, die von einem der Zeichen in Trennzeichen getrennt als Wörter angegeben sind:

- Wenn Zahl < 1, gibt eine leere Zeichenfolge.
- Wenn die Zeichenfolge null ist, zurückgegeben leere Zeichenfolge.

Wenn Zeichenfolge weniger als die Anzahl der Wörter enthält oder Zeichenfolge enthält keine Wörter, die durch Trennzeichen identifiziert, wird eine leere Zeichenfolge zurückgegeben.

**Beispiel:**  
`Word("The quick brown fox",3," ")`  
Gibt "Braun"

`Word("This,string!has&many separators",3,",!&#")`  
"Hat" zurück

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Grundlegendes zu deklarative Provisioning Ausdrücke](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD verbinden synchronisieren: Anpassen von Optionen für die Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
