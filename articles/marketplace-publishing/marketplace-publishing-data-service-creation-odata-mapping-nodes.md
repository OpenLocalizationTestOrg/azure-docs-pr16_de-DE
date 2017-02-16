<properties
   pageTitle="Leitfaden zum Erstellen einer Data Service von Marketplace | Microsoft Azure"
   description="Wenn Sie ausführliche Anweisungen zum Erstellen, zertifizieren und Bereitstellen einer Data Service für kaufen auf Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Grundlegendes zum Schema für die Zuordnung eines vorhandenen Webdiensts mit OData bis CSDL Knoten

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Data Service Herausgeber. Neue Dataservices wird nicht für Auflistung genehmigt abrufen.** Wenn Sie eine SaaS Business-Anwendung haben Sie auf Elemente verwenden veröffentlichen möchten Sie weitere Informationen finden Sie [hier](https://appsource.microsoft.com/partners). Oder wenn Sie eine IaaS Applikationen Entwicklertools Dienst auf Azure Marketplace, die Sie veröffentlichen möchten weitere Informationen finden Sie [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Dieses Dokument wird die Knotenstruktur für die Zuordnung ein OData-Protokoll zu CSDL verdeutlichen. Es ist wichtig, beachten Sie, dass die Knotenstruktur gut ist formatierter XML-Code. Somit ist Root, über- und untergeordneten Schema anwendbare beim Entwerfen der OData-Zuordnung.

## <a name="ignored-elements"></a>Ignorierte Elemente
Im folgenden sind die auf hoher Ebene CSDL Elemente (XML-Knoten), die nicht von der Azure Marketplace Back-End-während des Importvorgangs den Webdienst-Metadaten verwendet werden. Sie können vorhanden sein, jedoch werden ignoriert.

| Element | Bereich |
|----|----|
| Mithilfe des Elements | Der Knoten, Sub-Knoten und alle Attribute |
| Dokumentationselement | Der Knoten, Sub-Knoten und alle Attribute |
| ComplexType | Der Knoten, Sub-Knoten und alle Attribute |
| Association Element | Der Knoten, Sub-Knoten und alle Attribute |
| Erweiterte Eigenschaft | Der Knoten, Sub-Knoten und alle Attribute |
| EntityContainer | Nur die folgenden Attribute werden ignoriert: *Erweitert* und *AssociationSet* |
| Schema | Nur die folgenden Attribute werden ignoriert: *Namespace* |
| FunctionImport | Nur die folgenden Attribute werden ignoriert: *Modus* (Standardwert ln ist vorausgesetzt) |
| EntityType | Nur die folgenden Sub-Knoten werden ignoriert: *Schlüssel* und *PropertyRef* |

Im folgenden werden die Änderungen (hinzugefügt und Elemente ignoriert) zu den verschiedenen CSDL XML-Knoten im Detail beschrieben.

## <a name="functionimport-node"></a>FunctionImport Knoten
Ein FunctionImport-Knoten stellt eine URL (Einstiegspunkt), die einen Dienst für den Endbenutzer verfügbar macht. Der Knoten ermöglicht, beschreibt, wie die URL adressiert ist, werden die Parameter für den Endbenutzer verfügbar sind und wie diese Parameter bereitgestellt.

Details zu diesem Knoten befinden sich am [Hier] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Im folgenden sind die zusätzlichen Attribute (oder Ergänzungen Attributen), die von den Knoten FunctionImport verfügbar gemacht werden:

**D:baseUri** -
der URI-Vorlage für die restlichen Ressource, die für Marketplace verfügbar gemacht wird. Marketplace verwendet die Vorlage zum Erstellen von Abfragen für die REST-Webdiensts ein. Die URI-Vorlage enthält Platzhalter für die Parameter in Form von {ParameterName}, wobei ParameterName der Name des Parameters ist. Kurs ApiVersion = {ApiVersion}.
Parameter können als URI-Parameter oder als Teil des URL-Pfads angezeigt werden. Wenn die Darstellung in den Pfad sind sie immer obligatorisch (kann nicht markiert werden als NULL-Werte zulässt). *Beispiel:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Name** - der Name der importierten Funktion.  Identisch mit anderen definierten Namen in der CSDL nicht möglich.  Kurs Name = "GetModelUsageFile"

**EntitySet** *(optional)* – Wenn die Funktion eine Auflistung von Entitätstypen, gibt der Wert von **EntitySet** muss mit dem die Auflistung gehört. Andernfalls muss das **EntitySet** -Attribut nicht verwendet werden. *Beispiel:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(Optional)* – gibt den Typ der Elemente, die vom URI zurückgegeben.  Verwenden Sie dieses Attribut nicht, wenn die Funktion keinen Wert zurückgibt. Im folgenden sind die unterstützten Typen:

 - **Websitesammlung (<Entity type name>)**: Gibt eine Auflistung von definierten Entitätstypen. Der Name ist im Name-Attribut des Knotens EntityType vorhanden. Ein Beispiel ist die Websitesammlung (WXC. HourlyResult).
 - **Rohstoffe (<mime type>)**: Gibt einen unformatierten Dokument/Blob, die an den Benutzer zurückgegeben wird. Ein Beispiel ist Raw(image/jpeg) Weitere Beispiele aufgeführt:

  - ReturnType="Raw(text/plain)"
  - ReturnType = "Websitesammlung (Sage. DeleteAllUsageFilesEntity) "*

**D:Paging** - gibt an, wie die Seitennavigation durch die restlichen Ressource verarbeitet wird. Die Parameterwerte verwendeten innerhalb der geschweiften Bereiche eintragen lassen, z. B. Seite = {$page} & Itemsperpage = {$size} die verfügbaren Optionen sind:

- **Keine:** keine Seitennavigation steht.
- **Überspringen:** Seitennavigation wird durch einen logischen "Überspringen" und "Übernehmen" (oben) ausgedrückt. Überspringen springt über M Elemente aus und gibt übernehmen der nächsten N Elemente. Übergebener Wert: $skip
- **Optimieren:** Nehmen Sie gibt den nächsten N Elemente in Anspruch. Übergebener Wert: $take
- **PageSize:** Seitennavigation wird durch einen logischen Seite und den Schriftgrad (Elemente pro Seite) ausgedrückt. Seite darstellt, die aktuelle Seite, die zurückgegeben wird. Übergebener Wert: $page
- **Größe:** Größe darstellt, die Anzahl der Elemente, die Sie für jede Seite zurückgegeben. Übergebener Wert: $size

**d:AllowedHttpMethods** *(Optional)* – gibt an, welche Verb durch die restlichen Ressource behandelt wird. Darüber hinaus schränkt zulässigen Verb auf den angegebenen Wert ein.  Standard = Beitrag.  *Beispiel:* `d:AllowedHttpMethods="GET"` Die verfügbaren Optionen sind:

- **Abrufen:** zur Rückgabe von Daten in der Regel verwendet.
- **Beitrag:** normalerweise verwendet, um neue Daten einfügen
- **Setzen:** zum Aktualisieren von Daten in der Regel verwendet.
- **Löschen:** verwendet, um Daten löschen

Weitere untergeordnete Knoten (nicht der Dokumentation CSDL fallen) innerhalb der FunctionImport Knoten sind:

**d:RequestBody** *(Optional)* – der Anforderungstexts wird verwendet, um anzugeben, dass die Anforderung eine Stelle gesendet werden erwartet. Parameter können innerhalb der Anforderungstexts angegeben sein. Sie sind ausgedrückt innerhalb der geschweiften Klammern ein, z. B. {ParameterName}. Diese Parameter sind aus der Eingabe des Benutzers in den Textkörper zugeordnet, die an den Inhalt Anbieter-Dienst übertragen werden. Das Element RequestBody enthält ein Attribut mit Namen HttpMethod. Das Attribut ermöglicht zwei Werte an:

- **Beitrag:** Ist die Anforderung einer HTTP POST verwendet
- **Abrufen:** Verwendet, wenn die Anforderung HTTP GET ist.

    Beispiel:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**D:Namespaces** und **D:Namespace** - werden diese Knoten die Namespaces, die in die XML-Datei definiert sind, die von der Funktion importieren (URI-Endpunkt) zurückgegeben wird. Die XML-Daten, die vom Back-End-Dienst zurückgegeben wird möglicherweise eine beliebige Anzahl von Namespaces, um den Inhalt zu unterscheiden, der zurückgegeben wird, enthalten. **Alle diese Namespaces, wenn in D:Map oder D:Match XPath-Abfragen verwendet müssen aufgeführt sein.** Der D:Namespaces-Knoten enthält eine Menge/Liste D:Namespace Knoten. Jede von ihnen Listen einen Namespace in die Back-End-Dienst Antwort verwendet werden. Im folgenden finden das Attribut des Knotens D:Namespace:

-   **D:Prefix:** Das Präfix für den Namespace, wie in der XML-Ergebnisse zurückgegeben, die vom Dienst, z. B. F:FirstName, F:LastName, wobei f das Präfix ist zu sehen.
- **D:Uri:** Die vollständige URI des Namespace im Ergebnis Dokument verwendet. Es stellt den Wert, den das Präfix zur Laufzeit in aufgelöst wird.

**d:ErrorHandling** *(Optional)* – diese Knoten führt Umstände auf, für die Fehlerbehandlung. Jeder Bedingung wird das Ergebnis überprüft, die von den Inhalt Anbieter Dienst zurückgegeben wird. Wenn eine Bedingung den vorgeschlagenen HTTP-Fehlercode entspricht wird eine Fehlermeldung für den Endbenutzer zurückgegeben.

**d:ErrorHandling** *(Optional)* und **D:Condition** enthält *(Optional)* – ein Bedingungsknoten eine Bedingung, die im Ergebnis zurückgegebene Content-Anbieter-Dienst aktiviert ist. Im folgenden sind die **erforderlichen** Attribute:

- **D:Match:** Ein XPath-Ausdruck, der überprüft, ob ein angegebener Knoten/Wert in den Inhalt Anbieter Ausgabe XML-vorhanden ist. Der XPath-Ausdruck für die Ausgabe ausgeführt und sollte zurückgeben true, wenn die Bedingung als Übereinstimmung oder falsch ist.
- **D:HttpStatusCode:** Dieses Zeichen entspricht der HTTP-Statuscode, der durch Marketplace in der Groß-/Kleinschreibung die Bedingung zurückgegeben werden soll. Marketplace signalizes Fehler dem Benutzer über HTTP Statuscodes. Eine Liste der HTTP-Statuscodes finden Sie unter http://en.wikipedia.org/wiki/HTTP_status_code
- **D:ErrorMessage:** Die Fehlermeldung, die – mit dem HTTP-Statuscode – an den Endbenutzer zurückgegeben wird. Dies sollte eine kurze Fehlermeldung sein, die keine vertrauliche Daten enthält.

**d:Title** *(Optional)* – ermöglicht, die den Titel der Funktion beschreibt. Der Wert für den Titel bald aus

- Das optionale Karte Attribut (Xpath) die angibt, wo Sie den Titel in der Antwort zurückgegeben, aus der Serviceanfrage zu finden.
- – Oder – den Titel, die als Wert des Knotens angegeben.

**d:Rights** *(Optional)* – der Verwaltung von Informationsrechten (z. B. das Urheberrecht) die Funktion zugeordnet. Der Wert für die Verwaltung von Informationsrechten bald aus:

- Das optionale Karte Attribut (Xpath) die angibt, wo die Verwaltung von Informationsrechten in der Antwort zurückgegeben, aus der Serviceanfrage zu finden.
-   – Oder – die Rechte, die als Wert des Knotens angegeben.

**d:Description** *(Optional)* – eine kurze Beschreibung für die Funktion. Der Wert für die Beschreibung bald aus

- Das optionale Karte Attribut (Xpath) die angibt, wo die Beschreibung finden Sie in der Antwort aus der Serviceanfrage.
- – Oder – die Beschreibung, die als Wert des Knotens angegeben.

**D:EmitSelfLink** - *finden Sie unter über Beispiel "FunctionImport für das 'Auslagern' von zurückgegebenen Daten"*

**D:EncodeParameterValue** - optionale Erweiterung mit OData

**D:QueryResourceCost** - optionale Erweiterung mit OData

**D:Map** - optionale Erweiterung mit OData

**D:Headers** - optionale Erweiterung mit OData

**D:Headers** - optionale Erweiterung mit OData

**D:Value** - optionale Erweiterung mit OData

**D:HttpStatusCode** - optionale Erweiterung mit OData

**D:ErrorMessage** - optionale Erweiterung mit OData

## <a name="parameter-node"></a>Parameterknoten

Diese Knoten darstellt, einen Parameter, die als Teil der Vorlage URI verfügbar ist / Anfordern von Text, der im Knoten FunctionImport angegeben wurde.

Eine Dokumentseite sehr hilfreich, Details über den Knoten "Parameter Element" befindet sich am [hier](http://msdn.microsoft.com/library/ee473431.aspx) (verwenden Sie die **Andere Version** Dropdownmenü zu eine andere Version wählen Sie bei Bedarf zum Anzeigen von Dokumenten). *Beispiel:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parameterattribut | Ist erforderlich | Wert |
|----|----|----|
| Namen | Ja | Der Name des Parameters. Groß-/Kleinschreibung beachtet!  Die Groß-/BaseUri. **Beispiel:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ | Ja | Der Parametertyp. Der Wert muss eine **EDMSimpleType** oder einem komplexen Typ, der innerhalb des Gültigkeitsbereichs des Modells liegt. Weitere Informationen finden Sie unter "6 unterstützte Parameter/Eigenschaft Typen".  (Groß-/Kleinschreibung beachtet! Erste Zeichen Großbuchstaben, Rest sind Kleinbuchstaben.)  Siehe auch [konzeptionelle Modell Typen CSDL-] [MSDNParameterLink]. **Beispiel:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Modus | Nein | **In**, Out oder InOut je nachdem, ob der Parameter eine Eingabe, Ausgabe oder Eingabe/Ausgabe-Parameter. (Nur "IN" Azure Marketplace zur Verfügung.) **Beispiel:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Nein | Die maximal zulässige Länge des Parameters an. **Beispiel:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Genauigkeit | Nein | Die Genauigkeit des Parameters an. **Beispiel:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Skalieren | Nein | Die Skalierung des Parameters an. **Beispiel:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Im folgenden finden den Attributen, die die CSDL Spezifikation hinzugefügt wurden:

| Parameterattribut | Beschreibung |
|----|----|
| **d:Regex** *(Optional)* | Eine Regex-Anweisung verwendet, um den Eingabewert für den Parameter zu überprüfen. Wenn der Eingabewert nicht dem entsprechen der-Anweisung wird der Wert zurückgewiesen. Auf diese Weise können auch eine Reihe von möglichen Werten, z. B. angeben ^ [0-9] +? $ mit nur Zahlen zu ermöglichen. **Beispiel:** ' < Parametername = Modus "name" = "In" Typ "Zeichenfolge" d =: NULL-Werte zulässt = "false" D:Regex = "^ [a-zA-Z] * $" D:Description = "einen Namen, die keine Leerzeichen oder nicht alphanumerischen ASCII-Zeichen enthalten kann nicht" D:SampleValues = "Georg|Johann|Thomas|James "/ >' |
| **d:Enum** *(Optional)* | Eine Pipe getrennte Liste mit Werten für den Parameter gültig. Der Typ der Werte muss den definierten Typ des Parameters entsprechen. Beispiel: ' Englisch|Metrisch|unformatierten`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< Parametername = "Dauer" Typ "" Zeichenfolgenmodus = = "In" NULL-Werte zulässt = "true" D:Enum = "1 Jahr|5years|10years "/ >' |
| **d: NULL-Werte zulässt** *(Optional)* | Ermöglicht das definieren, ob der Parameter null sein kann. Die Standardeinstellung ist: true. Parameter, die als Teil der Pfad in der Vorlage URI verfügbar gemacht werden darf jedoch nicht null sein. Wenn das Attribut auf falsch für diesen Parameter – festgelegt ist, wird die Eingabe des Benutzers überschrieben. **Beispiel:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Optional)* | Ein Beispiel für Wert als eine Notiz an den Kunden in der Benutzeroberfläche angezeigt werden.  Es ist möglich, mehrere Werte d.h. mithilfe einer Pipe getrennte Liste hinzufügen ' ein|b|c` **Example:** `< Parametername = "BikeOwner" Typ "" Zeichenfolgenmodus = = "In" D:SampleValues = "Georg|Johann|Thomas|James "/ >' |

## <a name="entitytype-node"></a>EntityType Knoten

Dieser Knoten stellt einen der Typen, die von Marketplace an den Endbenutzer zurückgegeben werden. Darüber hinaus enthält die Zuordnung aus der Ausgabe, die von den Inhalt Anbieter Dienst auf die Werte zurückgegeben wird, die für den Endbenutzer zurückgegeben werden.

Details zu diesem Knoten befinden sich am [hier](http://msdn.microsoft.com/library/bb399206.aspx) (Verwenden der Dropdownliste **Andere Version** zu eine andere Version wählen Sie bei Bedarf zum Anzeigen von Dokumenten).

| Attributname | Ist erforderlich | Wert |
|----|----|----|
| Namen | Ja | Der Name des Entitätstyps. **Beispiel:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | Nein | D. h. Geben Sie der Namen einer anderen Person den Basistyp des Entitätstyps, der definiert wird. **Beispiel:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Im folgenden finden den Attributen, die die CSDL Spezifikation hinzugefügt wurden:

**D:Map** - ein XPath-Ausdruck für die Ausgabe der Dienst ausgeführt. Die Hierbei wird davon ausgegangen, dass die Ausgabe der Dienst einen Satz von Elementen, die sich, enthält wiederholen, wie ein ATOM Where-feed vorhanden ist eine Gruppe von Eintragsknoten, die sich wiederholen. Jede dieser wiederholte Knoten enthält einen Datensatz aus. Der XPath-Ausdruck angegeben klicken Sie dann auf einzelne wiederholten Knoten in der Inhalt Anbieter Dienst Ergebnis verweisen, die die Werte für einen einzelnen Datensatz enthält. Beispiel vom Dienst ausgeben:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

Der XPath-Ausdruck würde werden /foo/Balken-, da jeder des Balkens-Knoten ist der wiederholten Knoten in der Ausgabe und enthält den eigentlichen Inhalt, der für den Endbenutzer zurückgegeben wird.

**Schlüssel** – dieses Attribut wird vom Marketplace ignoriert. REST-Webdiensten basiert, im Allgemeinen nicht verfügbar machen einen Primärschlüssel.


## <a name="property-node"></a>Eigenschaftenknoten

Dieser Knoten enthält eine Eigenschaft des Datensatzes.

Details zu diesem Knoten befinden sich am [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (Verwenden der Dropdownliste **Andere Version** zu eine andere Version wählen Sie bei Bedarf zum Anzeigen von Dokumenten). *Beispiel:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Erforderlich | Wert |
|----|----|----|
| Namen | Ja | Der Name der Eigenschaft. |
| Typ | Ja | Die Art der Wert der Eigenschaft. Der Eigenschaft Value-Typ muss eine **EDMSimpleType** oder einem komplexen Typ (dargestellt durch einen vollqualifizierten Namen), der im Bereich des Modells liegt. Weitere Informationen finden Sie unter konzeptionelle Modell Typen (CSDL). |
| NULL-Werte zulassen | Nein | **Wahr** (der Standardwert) oder **falsch,** je nachdem, ob die Eigenschaft einen Nullwert haben kann. Hinweis: In der Version des Namespace [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) erkennbar CSDL Eigenschaft eines komplexen Typs müssen NULL-Werte zulässt = "False". |
| Standardwert | Nein | Der Standardwert der Eigenschaft. |
|MaxLength | Nein | Die maximale Länge der Wert der Eigenschaft. |
| FixedLength | Nein | **Wahr** oder **falsch,** je nachdem, ob der Wert der Eigenschaft als Zeichenfolge der Länge Fiexed gespeichert werden. |
| Genauigkeit | Nein | Bezieht sich auf die maximale Anzahl von Ziffern in den numerischen Wert beibehalten. |
| Skalieren | Nein | Maximale Anzahl von Dezimalstellen in den numerischen Wert beibehalten. |
| Unicode | Nein | **Wahr** oder **falsch,** je nachdem, ob der Wert der Eigenschaft werden als eine Unicode-Zeichenfolge gespeichert. |
| Sortierung | Nein | Eine Zeichenfolge, die angibt, die Sortierreihenfolge, in der Datenquelle verwendet werden soll. |
| ConcurrencyMode | Nein | **Keine** (der Standardwert) oder **fest**. Wenn der Wert **mit fester**festgelegt ist, wird der Wert der Eigenschaft in vollständige Parallelität Prüfungen verwendet werden. |

Im folgenden sind die zusätzlichen Attribute, die die CSDL Spezifikation hinzugefügt wurden:

**D:Map** - XPath-Ausdruck für den Dienst aufgerufen ausgeben und eine Eigenschaft der Ausgabe extrahiert. Der angegebene XPath bezieht sich auf den sich wiederholenden Knoten, der in den EntityType-Knoten XPath ausgewählt wurde. Es ist auch möglich, geben Sie einen absoluten XPath zulässig ausgeben einschließlich eine statische Ressource in jeder der Ausgabeknoten, wie beispielsweise eine copyright-Anweisung, die in den ursprünglichen Dienst nur einmal gefunden wird, aber in jede Zeile in der Ausgabe OData vorhanden sein soll. Beispiel vom Dienst:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Der XPath-Ausdruck wäre ./bar/baz0 den Knoten baz0 aus den Inhalten Anbieter Dienst abrufen.

**D:CharMaxLength** – für Zeichenfolgentyp, geben Sie die maximale Länge. Finden Sie unter DataService CSDL-Beispiel

**D:IsPrimaryKey** - zeigt an, ob die Spalte der Primärschlüssel in der Tabellenansicht ist. Siehe Beispiel DataService CSDL.

**D:isExposed** – bestimmt, wenn das Tabellenschema verfügbar gemacht wird (in der Regel WAHR). Finden Sie unter DataService CSDL-Beispiel

**d:IsView** *(Optional)* – true zurück, wenn dies auf eine Sicht statt einer Tabelle basiert.  Finden Sie unter DataService CSDL-Beispiel

**D:Tableschema** - finden Sie unter DataService CSDL Beispiel

**D:ColumnName** - ist der Name der Spalte in der Tabellenansicht.  Finden Sie unter DataService CSDL-Beispiel

**D:IsReturned** - ist der boolesche Wert, der bestimmt, ob der Dienst macht diesen Wert an den Kunden verfügbar.  Finden Sie unter DataService CSDL-Beispiel

**D:IsQueryable** - ist der boolesche Wert, der bestimmt, ob die Spalte in einer Datenbankabfrage verwendet werden kann.   Finden Sie unter DataService CSDL-Beispiel

**D:OrdinalPosition** - ist der Spalte numerischen Position Darstellung, x, in der Tabelle oder die Ansicht, wobei x zwischen 1 auf die Anzahl der Spalten in der Tabelle ist.  Finden Sie unter DataService CSDL-Beispiel

**D:DatabaseDataType** - ist der Datentyp der Spalte in der Datenbank, d. h. SQL-Datentyp. Finden Sie unter DataService CSDL-Beispiel

## <a name="supported-parametersproperty-types"></a>Typen von unterstützten Parameters-Eigenschaft
Im folgenden sind die unterstützten Datentypen für Parameter und Eigenschaften. (Groß-/Kleinschreibung wird beachtet)

| Grundtypen | Beschreibung |
|----|----|
| NULL-Werten | Das Fehlen eines Werts angibt |
| Boolesch | Stellt das mathematische Konzept von Binary-Werte-Logik|
| Byte-Zeichen | Wert 8-Bit-Ganzzahl ohne Vorzeichen|
|"DateTime"| Datum und Uhrzeit darstellt, mit Werten zwischen 12:00:00 Mitternacht, Januar 1, 1753 z. zurück. bis 11:59:59 Uhr, Dezember 9999 z. zurück.|
|Dezimalzahl | Stellt numerische Werte mit fester Genauigkeit und Dezimalstellen an. Dieses Typs beschreiben können Sie einen numerischen Wert im Bereich von-10 ^ 255 + 1 auf positive 10 ^ 255-1 ab.|
| Double | Stellt eine Gleitkommazahl mit einer Genauigkeit von 15 Stellen, die mit ungefähren Bereich von ± 2,23E-308 bis ± 1,79E Werte dargestellt werden kann +308. **Verwenden Sie Decimal aufgrund Exel exportieren Problem**|
| Einzelne | Stellt eine Gleitkommazahl mit einer Genauigkeit von 7 Ziffern, die Werte mit ungefähren Bereich von ± 1,18E-38 bis ± 3.40e dargestellt werden kann +38|
|GUID |Stellt einen 16-Byte-Zeichen (128-Bit) eindeutige ID-Wert |
|Int16|Stellt einen Wert von 16-Bit-Ganzzahl mit Vorzeichen |
|Int32|Stellt einen 32-Bit-Ganzzahl-Wert |
|Int64|Stellt einen 64-Bit-Ganzzahl-Wert |
|Zeichenfolge | Darstellt oder Variable-Zeichendaten fester Länge |


## <a name="see-also"></a>Siehe auch
- Wenn Sie die allgemeine OData-Zuordnungsprozess und Zweck Grundlegendes interessiert sind, lesen Sie diesen Artikel [Data Service OData-Zuordnung](marketplace-publishing-data-service-creation-odata-mapping.md) zu Definitionen, Strukturen und Anweisungen zu überprüfen.
- Wenn Sie Beispiele überprüfen möchten, lesen Sie diesen Artikel [Data Service OData Zuordnen von Beispielen](marketplace-publishing-data-service-creation-odata-mapping-examples.md) finden Sie unter Beispielcode und Syntax von Feldfunktionen und den Kontext verstehen.
- Lesen Sie diesen Artikel [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md), um den vorgegebenen Pfad für die Veröffentlichung von einem Datendienst zu Azure Marketplace zurückzukehren.
