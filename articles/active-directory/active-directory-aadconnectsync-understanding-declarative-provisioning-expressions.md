<properties
    pageTitle="Synchronisieren von Azure AD verbinden: Grundlegendes zu deklarative Provisioning Ausdrücke | Microsoft Azure"
    description="Erläutert die deklarative provisioning Ausdrücke."
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
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Synchronisieren von Azure AD verbinden: Grundlegendes zu deklarative Provisioning Ausdrücke
Azure AD verbinden synchronisieren basiert auf deklarative provisioning zuerst in Forefront Identität Manager 2010 eingeführt werden. Sie können zum Implementieren der vollständige Identität Integration von Geschäftslogik ohne kompilierten Code schreiben zu müssen.

Ein wesentlicher Teil des deklarativen bereitgestellt wird der Ausdruckssprache in Attribut Zahlungen verwendet werden. Die verwendete Sprache ist eine Teilmenge der Microsoft Visual Basic® für Applikationen (VBA). Diese Sprache in Microsoft Office verwendet wird, und Benutzer mit der Benutzeroberfläche von VBScript auch erkennt. Der deklarativen Ausdruckssprache bereitgestellt wird nur mithilfe von Funktionen und ist keine strukturierte Sprache. Es gibt keine Methoden oder Anweisungen aus. Funktionen sind stattdessen express Programmfluss verschachtelt.

Weitere Informationen hierzu finden Sie unter [Willkommen im Visual Basic für Applikationen Sprache Bezug für Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

Die Attribute sind stark eingegeben haben. Eine Funktion akzeptiert nur Attribute die richtige Art. Es ist auch die Groß-/Kleinschreibung beachtet. Sowohl Funktionsnamen und die Namen der Attribute müssen gemischte Groß-/Kleinschreibung oder ein Fehler ausgegeben.

## <a name="language-definitions-and-identifiers"></a>Definitionen der Programmiersprache und Bezeichner

- Funktionen weisen einen anderen Namen gefolgt von Argumenten in Klammern: Funktionsname (Argument 1, Argument N).
- Attribute rechteckige Klammern identifiziert werden: [AttributeName]
- Parameter werden durch Prozentzeichen identifiziert: ParameterName %
- Zeichenfolgenkonstanten Anführungszeichen gesetzt werden: beispielsweise "Contoso" (Notiz: Verwenden Sie gerade Anführungszeichen muss "" und nicht typografische Anführungszeichen "")
- Numerische Werte werden ohne Anführungszeichen ausgedrückt und Dezimaltrennzeichen werden soll. Hexadezimale Werte werden mit dem Präfix & H. Beispielsweise 98052 & HFF
- Boolesche Werte mit Konstanten ausgedrückt werden: WAHR, falsch.
- Integrierte Konstanten und literalen mit nur seinen Namen ausgedrückt werden: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Funktionen
Deklarative provisioning verwendet viele Funktionen so aktivieren Sie die Möglichkeit zum Attributwerte umwandeln. Diese Funktionen können geschachtelt werden, damit das Ergebnis aus eine Funktion in einer anderen Funktion weitergegeben wird.

`Function1(Function2(Function3()))`

Die vollständige Liste der Funktionen finden Sie in der [Funktionsverweis](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parameter
Ein Parameters wird durch einen Verbinder oder von einem Administrator mithilfe der PowerShell definiert. Parameter enthalten normalerweise Werte, die zwischen den Systemen unterscheiden, beispielsweise der Namen der Domäne des Benutzers befindet. Diese Parameter können in Attribut Zahlungen verwendet werden.

Active Directory-Connector vorgesehen die folgenden Parameter für eingehende Synchronisierungsregeln an:

| Parameternamen | Kommentar |
| --- | --- |
| Domain.Netbios | NetBIOS-Format der Domäne aktuell importiert wird, beispielsweise FABRIKAMSALES |
| Domain.FQDN | Formatieren von FQDN der Domäne aktuell importiert wird, beispielsweise sales.fabrikam.com |
| Domain.LDAP | LDAP-Format der Domäne aktuell importiert wird, beispielsweise DC = Sales, DC = Fabrikam, DC = com |
| Forest.Netbios | Der Gesamtstrukturname aktuell importiert wird, beispielsweise FABRIKAMCORP NetBIOS-Formats |
| Forest.FQDN | Der Gesamtstrukturname aktuell importiert wird, beispielsweise fabrikam.com FQDN Formats |
| Forest.LDAP | LDAP-Formats der Gesamtstrukturname aktuell importiert wird, beispielsweise DC = Fabrikam, DC = com |

Das System bietet den folgenden Parameter, die zum Abrufen des Bezeichner des Verbinders gerade ausgeführt verwendet wird:  
`Connector.ID`

Hier ein Beispiel, das die Metaverse Attributdomäne mit dem NetBIOS-Namen der Domäne auffüllt, wo sich der Benutzer befindet:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operatoren
Die folgenden Operatoren können verwendet werden:

- **Vergleich**: <, < = <>, =, >, > =
- **Mathematik**: +, -, \*, -
- **Zeichenfolge**: & (Verkettung)
- **Logisch**: & & (und), || (oder)
- **Auswertungsreihenfolge**:)

Operatoren links nach rechts ausgewertet und dieselbe Auswertung Priorität aufweisen. D. h., die \* (Multiplikator) wird nicht ausgewertet, bevor Sie - (Subtraktion). 2\*(5 + 3) ist nicht identisch mit 2\*5 + 3. Der Klammern () werden verwendet, um die Auswertungsreihenfolge zu ändern, wenn von links nach rechts Auswertungsreihenfolge nicht geeignet ist.

## <a name="multi-valued-attributes"></a>Mehrwertige Attribute
Die Funktionen können auf eindeutiges und mehrwertigen Attributen ausgeführt werden. Für mehrwertig Attributen die Funktion arbeitet über jeder Wert und jeder Wert gilt die gleiche Funktion.

Beispiel:  
`Trim([proxyAddresses])`Führen Sie eine Kürzen von jeder Wert im Attribut ProxyAddress.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Für jeden Wert mit einem @-sign, Domäne ersetzen durch @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Suchen Sie nach der SIP-Adresse ein, und trennen sie die Werte.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Konfigurationsmodell in [Grundlegendes zu deklarative bereitgestellt](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Finden Sie unter wie deklarative verwendeten Out-of-Box Verständnis [der Standard-Konfigurations](active-directory-aadconnectsync-understanding-default-configuration.md)bereitgestellt wird.
- Informationen Sie zum Praxis mit deklarativen provisioning in [So stellen Sie eine Änderung an der Standardkonfiguration](active-directory-aadconnectsync-change-the-configuration.md)geändert haben.

**Themen (Übersicht)**

- [Synchronisieren von Azure AD verbinden: verstehen und Anpassen der Synchronisierung](active-directory-aadconnectsync-whatis.md)
- [Integrieren von Ihrem lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

**Verweis Themen**

- [Synchronisieren von Azure AD verbinden: über Funktionen](active-directory-aadconnectsync-functions-reference.md)
