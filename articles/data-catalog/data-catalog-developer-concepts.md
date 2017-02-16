<properties
    pageTitle="Datenkatalog Entwicklertools Konzepte | Microsoft Azure"
    description="Einführung in die wichtigsten Punkte in Azure Datenkatalog konzeptionelle Modell als zugänglicher durch den Katalog REST-API."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="10/11/2016"
    ms.author="spelluru"/>  

# <a name="azure-data-catalog-developer-concepts"></a>Azure Datenkatalog Entwicklertools Konzepte

Microsoft **Azure Datenkatalog** ist eine vollständig verwaltete Cloud-Dienst, der Funktionen für die Quelle Datenermittlung und Metadaten zu Datenquellen Crowdsourcing bereitstellt. Entwickler können den Dienst über seine REST-APIs verwenden. Verständnis der Konzepte des Diensts implementiert ist wichtig für Entwickler mit **Azure Datenkatalog**erfolgreich integriert werden soll.

## <a name="key-concepts"></a>Grundlegende Konzepte

Konzeptionelle Modell **Datenkatalog Azure** basiert auf vier grundlegende Konzepte: der **Katalog**, **Benutzer**, **Ressourcen**und **Anmerkungen**.

![Konzept][1]

*Abbildung 1: Azure Datenkatalog vereinfachte konzeptionelle Modell*

### <a name="catalog"></a>Katalog

**Katalog** ist der Container der obersten Ebene für die Metadaten, die eine Organisation speichert. Es gibt eine **Katalog** pro Azure-Konto ein. Kataloge zu einem Azure-Abonnement verknüpft sind, aber nur einen **Katalog** , obwohl mehrere Abonnements ein Konto besitzen, kann für alle angegebenen Azure-Konto erstellt werden.

Katalog enthält **Benutzer** und **Anlagen**.

### <a name="users"></a>Benutzer

Benutzer sind Sicherheitsprincipals, die Berechtigung zum Ausführen von Aktionen (den Katalog durchsuchen, hinzufügen, bearbeiten oder Entfernen von Elementen, usw.) im Katalog.

Es gibt verschiedene Rollen, die ein Benutzer verfügen kann. Informationen zu Rollen finden Sie unter dem Abschnitt Rollen und Autorisierung.

Einzelne Benutzer und Sicherheitsgruppen können hinzugefügt werden.

Azure Datenkatalog verwendet Azure Active Directory für die Verwaltung von Identität und Access. Jeder Katalog Benutzer muss ein Mitglied von Active Directory für das Konto.

### <a name="assets"></a>Posten

**Katalog** enthält Datenbestände. **Vermögenswerte** sind der Maßeinheit von Genauigkeit von dem Katalog verwaltet werden.

Die Genauigkeit der eines Wirtschaftsguts variieren je nach Datenquelle. Für SQL Server oder Oracle-Datenbank kann eine Anlage einer Tabelle oder einer Ansicht sein. Für SQL Server Analysis Services kann eine Anlage ein Measure, eine Dimension oder Key Performance Indicator (KPI) sein. Für SQL Server Reporting Services ist eine Anlage eines Berichts.

Eine **Anlage** wird das, was, das Sie hinzufügen oder Entfernen aus einem Katalog. Es ist der Maßeinheit von Ergebnis, das Sie aus **Suchvorgängen**zurückzukehren.

Ein **Objekt** besteht aus den Namen, Speicherort und Typ, und Anmerkungen, die werden weitere beschreiben Sie es.

### <a name="annotations"></a>Anmerkungen

Anmerkungen sind Elemente, die Metadaten zu Posten darstellen.

Beispiele für Anmerkungen sind Beschreibung, Tags, Schema, Dokumentation usw. an. Eine vollständige Liste der Anlagentypen und Anmerkungstypen werden im Abschnitt Modell Anlage Objekt.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Crowdsourcing Anmerkungen und Benutzerperspektive (Multiplizität von marketingorientierten)

Ein wichtiger Aspekt des Azure Datenkatalog ist wie der Crowdsourcing Metadaten im System unterstützt. Im Gegensatz zu einem Wiki-Ansatz –, wo es ist nur eine marketingorientierten und der letzten jüngste Änderung gewinnt – das Modell Datenkatalog Azure ermöglicht mehreren Meinung zu nebeneinander im System live.

Dieser Ansatz widerspiegeln der Praxis von Unternehmensdaten, wo andere Benutzer verschiedene Perspektiven auf einer bestimmten Anlage haben:

-   Ein Datenbankadministrator vorsehen Informationen zum Servicelevel Dienst oder das Verarbeitungsfenster verfügbar für Massen ETL-Vorgänge
-   Eine Data Steward vorsehen Informationen gilt die Anlage von Geschäftsprozessen oder die Einstufung, die Unternehmen darauf angewendet wurde
-   Eine Finanzen Analysten vorsehen Informationen darüber, wie die Daten während Ende des Zeitraums reporting Aufgaben verwendet wird

In diesem Beispiel wird unterstützt, kann jeder Benutzer – der Datenbankadministrator, die Daten Steward und die Analysten – eine Beschreibung einer Tabelle hinzufügen, die im Katalog registriert wurde. Alle Beschreibungen werden im System beibehalten und im Portal Azure Datenkatalog alle Beschreibungen angezeigt werden.

Dieses Muster wird für die meisten Elemente in das Objektmodell angewendet, damit Objekttypen in den JSON-Nutzdaten häufig Matrizen für Eigenschaften werden, in denen Sie möglicherweise einem einzelnen erwarten.

Klicken Sie unter der Anlage beträgt beispielsweise Root ein Array von Objekten Beschreibung an. Die Matrix-Eigenschaft ist mit der Bezeichnung "Beschreibung". Ein Beschreibungsobjekt enthält eine Eigenschaft - Beschreibung an. Das Muster ist, dass jeder Benutzer, die Typen Beschreibung eine Beschreibungsobjekt wird für den Wert durch eine benutzerdefinierte erstellt.

Bedienen kann dann auswählen, wie die Kombination angezeigt. Es gibt drei verschiedene Muster für die Anzeige aus.

-   Das einfachste Muster ist "Alle anzeigen". In diesem Muster werden alle Objekte in einer Listenansicht angezeigt. Im Portal Azure Datenkatalog UX verwendet dieses Muster Beschreibung ein.
-   Ein weiteres Muster ist "Zusammenführen". Bei diesem Muster werden alle Werte aus der anderen Benutzern zusammen, mit doppelten entfernt zusammengeführt. Beispiele für dieses Muster im Datenkatalog Azure-Portal UX sind die Kategorien und Experten Eigenschaften an.
-   Ein dritte Muster ist "letzten Autor Vorrang". Bei diesem Muster wird nur der letzte Wert in eingegeben wird angezeigt. FriendlyName ist ein Beispiel für dieses Muster.

## <a name="asset-object-model"></a>Objektmodell Anlage

Wie im Abschnitt Schlüssel Konzepte eingeführt, enthält das Objektmodell **Azure Datenkatalog** Elemente, die Posten oder Anmerkungen dienen. Elemente besitzen Eigenschaften, die optional oder erforderlich sein können. Einige Eigenschaften gelten für alle Elemente. Einige Eigenschaften gelten für alle Anlagen. Einige Eigenschaften gelten nur für bestimmte Anlagentypen.

### <a name="system-properties"></a>Systemeigenschaften

<table><tr><td><b>Eigenschaftsname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr><tr><td>Zeitstempel</td><td>"DateTime"</td><td>Zeitpunkt der letzten des Elements Änderung. Dieses Feld wird vom Server generiert, wenn ein Element eingefügt wird, und jedes Mal, wenn ein Element aktualisiert wird. Der Wert dieser Eigenschaft wird bei der Eingabe der ignoriert Vorgänge veröffentlichen.</td></tr><tr><td>ID</td><td>URI</td><td>Absolute Url des Elements (schreibgeschützt). Es ist der eindeutige adressiert URI für das Element aus.  Der Wert dieser Eigenschaft wird bei der Eingabe der ignoriert Vorgänge veröffentlichen.</td></tr><tr><td>Typ</td><td>Zeichenfolge</td><td>Die Art der Anlage (schreibgeschützt).</td></tr><tr><td>ETag</td><td>Zeichenfolge</td><td>Eine Zeichenfolge, die die Version des Elements, das zur Steuerung der vollständigen Parallelität verwendet werden kann, beim Ausführen von Vorgängen, die Elemente im Katalog aktualisieren entspricht. "*" kann verwendet werden, um einen beliebigen Wert entsprechen.</td></tr></table>

### <a name="common-properties"></a>Allgemeine Eigenschaften

Diese Eigenschaften gelten für alle Stamm Anlagentypen und alle Anmerkungstypen.

<table>
<tr><td><b>Eigenschaftsname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>
<tr><td>fromSourceSystem</td><td>Boolesch</td><td>Gibt an, ob Elementdaten aus einer Quellsystem (wie etwa Sql Server-Datenbank, Oracle-Datenbank) abgeleitet oder die von einem Benutzer verfasst ist.</td></tr>
</table>

### <a name="common-root-properties"></a>Allgemeine Stammeigenschaften
<p>
Diese Eigenschaften gelten für alle Stamm Anlagentypen.
<table><tr><td><b>Eigenschaftsname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr><tr><td>Namen</td><td>Zeichenfolge</td><td>Einen Namen aus der Speicherort Datenquelleninformationen abgeleitet</td></tr><tr><td>DSL</td><td>DataSourceLocation</td><td>Eindeutig beschrieben, die Datenquelle und den Bezeichner für die Anlage ist. (Siehe Identitätsabschnitt Zeitzonen).  Die Struktur der Dsl variieren je nach Typ das Protokoll und die Quelle.</td></tr><tr><td>Datenquelle</td><td>DataSourceInfo</td><td>Weitere Informationen zu den Typ der Anlage.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Beschreibt den Benutzer, der diese Anlage zuletzt registriert hat.  Enthält die eindeutige Id für den Benutzer (Benutzerprinzipalnamen) und einen Anzeigenamen (Nachname und Vorname).</td></tr><tr><td>containerId</td><td>Zeichenfolge</td><td>ID eines Containers Wirtschaftsgutes für die Datenquelle. Diese Eigenschaft wird für den Containertyp nicht unterstützt.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Allgemeine nicht einzelne Anmerkungseigenschaften

Diese Eigenschaften gelten für alle Arten von nicht einzelne Anmerkungen (Anmerkungen, die mehrere werden darf pro Anlage).

<table>
<tr><td><b>Eigenschaftsname</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>
<tr><td>Schlüssel</td><td>Zeichenfolge</td><td>Ein benutzerdefinierter Schlüssel aus, der die Anmerkung in der aktuellen Auflistung identifiziert. Länge des darf 256 Zeichen nicht überschreiten.</td></tr>
</table>

### <a name="root-asset-types"></a>Quadratwurzel Anlagentypen

Quadratwurzel Anlagentypen werden diese Typen, die die verschiedenen Typen von Datenbestände darstellen, die im Katalog registriert werden kann. Für jeden Stamm ist es eine Ansicht, in der Anlage und Anmerkungen in die Ansicht aufgenommen werden. Name der Ansicht sollte in die entsprechende Url {Ansichtsname} Segment verwendet werden beim Veröffentlichen einer Anlage über die REST-API.

<table><tr><td><b>Anlagentyp (View Name)</b></td><td><b>Zusätzliche Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Zulässige Anmerkungen</b></td><td><b>Kommentare</b></td></tr><tr><td>Tabelle ("Tabellen")</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Kategorie<p>Schema<p>ColumnDescription<p>ColumnTag<p> Experten<p>Vorschau<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Dokumentation<p></td><td>Eine Tabelle stellt eine Tabellendaten.  Beispiel: SQL-Tabelle, SQL-Ansicht, tabellarischen Analysis Services-Tabelle, mehrdimensionalen Analysis Services-dimension, usw. Oracle-Tabelle.   </td></tr><tr><td>Measure ("Measures")</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Kategorie<p>Experten<p>AccessInstruction<p>Dokumentation<p></td><td>Dieser Typ stellt eine Analysis Services-Measure.</td></tr><tr><td></td><td>Measure</td><td>Spalte</td><td></td><td>Metadaten, die das Measure beschreiben</td></tr><tr><td></td><td>isCalculated </td><td>Boolesch</td><td></td><td>Gibt an, ob das Measure oder nicht berechnet wird.</td></tr><tr><td></td><td>Vorgang zum</td><td>Zeichenfolge</td><td></td><td>Physische Container für measure</td></tr><td>KPI ("Kpis")</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Kategorie<p>Experten<p>AccessInstruction<p>Dokumentation</td><td></td></tr><tr><td></td><td>Vorgang zum</td><td>Zeichenfolge</td><td></td><td>Physische Container für measure</td></tr><tr><td></td><td>goalExpression</td><td>Zeichenfolge</td><td></td><td>Einen numerischen MDX-Ausdruck oder eine Berechnung, die den KPI-Zielwert zurückgibt.</td></tr><tr><td></td><td>valueExpression</td><td>Zeichenfolge</td><td></td><td>Ein numerische MDX-Ausdruck, der den tatsächlichen Wert des KPI zurückgibt.</td></tr><tr><td></td><td>Ausdruck.Status Ausdruck ist</td><td>Zeichenfolge</td><td></td><td>Ein MDX-Ausdruck, der den Zustand des KPI zu einem bestimmten Zeitpunkt Zeitpunkt darstellt.</td></tr><tr><td></td><td>trendExpression</td><td>Zeichenfolge</td><td></td><td>Ein MDX-Ausdruck, der den Wert des KPI über einen Zeitraum ergibt. Der Trend kann ein Kriterium zeitbasierte sein, die in einem bestimmten Business Kontext eignet.</td>
<tr><td>Bericht ("Berichte")</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Kategorie<p>Experten<p>AccessInstruction<p>Dokumentation<p></td><td>Dieses Typs stellt einen SQL Server Reporting Services-Bericht </td></tr><tr><td></td><td>assetCreatedDate</td><td>Zeichenfolge</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Zeichenfolge</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Zeichenfolge</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Zeichenfolge</td><td></td><td></td></tr><tr><td>Container ("Container")</td><td></td><td></td><td>Beschreibung<p>FriendlyName<p>Kategorie<p>Experten<p>AccessInstruction<p>Dokumentation<p></td><td>Dieses Typs stellt einen Container anderer Vermögenswerte wie einer SQL-Datenbank, eine Azure Blobs Container oder ein Analysis Services-Modell dar.</td></tr></table>

### <a name="annotation-types"></a>Anmerkungstypen

Anmerkungstypen darstellen Arten von Metadaten, die für andere Typen innerhalb des Katalogs zugeordnet werden kann.

<table>
<tr><td><b>Anmerkungstyp (Ansichtsname geschachtelt)</b></td><td><b>Zusätzliche Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>

<tr><td>Beschreibung ("Beschreibungen")</td><td></td><td></td><td>Diese Eigenschaft enthält eine Beschreibung für eine Anlage. Jeder Benutzer des Systems kann eigene Beschreibung hinzufügen.  Nur für diesen Benutzer kann das Objekt Beschreibung bearbeiten.  (Administratoren und Anlage Besitzer können Löschen des Objekts Beschreibung jedoch nicht bearbeiten). Das System verwaltet Benutzer Beschreibungen getrennt.  Es gibt also ein Array von Beschreibungen zu den einzelnen Ressourcen (eine für jeden Benutzer, die ihre Kenntnisse über die Anlage, sowie demjenigen stammen, die Informationen aus der Datenquelle abgeleitet enthält).</td></tr>
<tr><td></td><td>Beschreibung</td><td>Zeichenfolge</td><td>Eine kurze Beschreibung (Zeilen 2 und 3) für die Anlage</td></tr>

<tr><td>Tag ("Kategorien")</td><td></td><td></td><td>Diese Eigenschaft definiert eine Kategorie für eine Anlage. Jeder Benutzer des Systems kann mehrere Tags für eine Anlage hinzufügen.  Nur der Benutzer, der Kategorie Objekte erstellt hat, können sie bearbeiten.  (Administratoren und Anlage Besitzer können gelöscht das Tag-Objekt jedoch nicht bearbeiten). Das System verwaltet Benutzer Tags getrennt.  Es gibt also ein Array von Tag-Objekten auf jedes Objekt.</td></tr>
<tr><td></td><td>Kategorie</td><td>Zeichenfolge</td><td>Eine Kategorie, die das Objekt beschreibt.</td></tr>

<tr><td>FriendlyName ("FriendlyName")</td><td></td><td></td><td>Diese Eigenschaft enthält einen Anzeigenamen für eine Anlage. FriendlyName ist eine einzelne Anmerkung: nur eine FriendlyName eine Anlage hinzugefügt werden kann.  Nur der Benutzer, der FriendlyName Objekt erstellt hat, können sie bearbeiten. (Administratoren und Anlage Besitzer können Löschen des Objekts FriendlyName jedoch nicht bearbeiten). Das System verwaltet Anzeigenamen Benutzer getrennt.</td></tr>
<tr><td></td><td>friendlyName</td><td>Zeichenfolge</td><td>Einen Anzeigenamen für die Anlage.</td></tr>

<tr><td>Schema ("Schema")</td><td></td><td></td><td>Das Schema beschreibt die Struktur der Daten.  Es listet die Attributnamen (Spalte, Attribut, Feld usw.), sowie andere Metadaten Dateitypen.  Diese Informationen werden von der Datenquelle abgeleitet.  Schema wird eine einzelne Anmerkung: nur ein Schema für eine Anlage hinzugefügt werden kann.</td></tr>
<tr><td></td><td>Spalten</td><td>Spalte]</td><td>Ein Array von Column-Objekten. Beschreiben sie die Spalte mit den Informationen aus der Datenquelle abgeleitet.</td></tr>

<tr><td>ColumnDescription ("ColumnDescriptions")</td><td></td><td></td><td>Diese Eigenschaft enthält eine Beschreibung für eine Spalte.  Jeder Benutzer des Systems kann eigene Beschreibungen für mehrere Spalten (höchstens eine pro Spalte) hinzufügen. Nur der Benutzer, der ColumnDescription Objekte erstellt haben, können sie bearbeiten.  (Administratoren und Anlage Besitzer können Löschen des Objekts ColumnDescription jedoch nicht bearbeiten). Das System behält diese des Benutzers spaltenbeschreibungen separat aus.  Es gibt also ein Array von ColumnDescription Objekte auf jede Anlage (eine pro Spalte für jeden Benutzer, die ihre Kenntnisse über der Spalte neben demjenigen stammen, die Informationen aus der Datenquelle abgeleitet enthält).  Die ColumnDescription gebunden ist grob auf das Schema, damit es nicht synchron abrufen kann. Die ColumnDescription kann eine Spalte beschreiben, die im Schema nicht mehr vorhanden ist.  Es ist auf der Entwickler zur Beschreibung und Schemadateien synchron bleiben.  Die Datenquelle hat möglicherweise auch Spalten Beschreibungsinformationen und sind für zusätzliche ColumnDescription-Objekte, die beim Ausführen des Tools erstellt werden.</td></tr>
<tr><td></td><td>columnName</td><td>Zeichenfolge</td><td>Der Name der Spalte, der auf diese Beschreibung verweist.</td></tr>
<tr><td></td><td>Beschreibung</td><td>Zeichenfolge</td><td>eine kurze Beschreibung (Zeilen 2 und 3) für die Spalte.</td></tr>

<tr><td>ColumnTag ("ColumnTags")</td><td></td><td></td><td>Diese Eigenschaft enthält eine Kategorie für eine Spalte. Jeder Benutzer des Systems kann mehrere Tags für eine bestimmte Spalte hinzufügen und Hinzufügen von Kategorien für mehrere Spalten kann. Nur der Benutzer, der ColumnTag Objekte erstellt haben, können sie bearbeiten. (Administratoren und Anlage Besitzer können Löschen des Objekts ColumnTag jedoch nicht bearbeiten). Das System behält diese Benutzer Spalte Kategorien getrennt.  Es gibt also ein Array von ColumnTag Objekte auf jedes Objekt.  Die ColumnTag gebunden ist grob auf das Schema, damit es nicht synchron abrufen kann. Die ColumnTag kann eine Spalte beschreiben, die im Schema nicht mehr vorhanden ist.  Es ist auf der Autor Spalte Kategorie und Schemadateien synchron bleiben.</td></tr>
<tr><td></td><td>columnName</td><td>Zeichenfolge</td><td>Der Name der Spalte, der auf dieses Tag verweist.</td></tr>
<tr><td></td><td>Kategorie</td><td>Zeichenfolge</td><td>Eine Kategorie, die die Spalte beschreibt.</td></tr>

<tr><td>Experten ("Experten")</td><td></td><td></td><td>Diese Eigenschaft enthält einen Benutzer, der mit einem Experten aus der Datenmenge betrachtet wird. Die Experten opinions(descriptions) Blase an den Anfang der die Benutzerfunktionalität Wenn Beschreibungen aufgelistet. Jeder Benutzer kann eigene Experten angeben. Nur für diesen Benutzer kann das Objekt Experten bearbeiten. (Administratoren und Anlage Besitzer können Experten Objekte löschen jedoch nicht bearbeiten).</td></tr>
<tr><td></td><td>Experten</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Vorschau ("Vorschau")</td><td></td><td></td><td>Die Vorschau enthält eine Momentaufnahme der obersten 20 Zeilen mit Daten für die Anlage. Vorschau nur sinnvoll für bestimmte Typen von Anlagen (es ist sinnvoll, für die Tabelle jedoch nicht für Measure).</td></tr>
<tr><td></td><td>Vorschau</td><td>Objekt]</td><td>Ein Array von Objekten, die eine Spalte darstellen.  Jedes Objekt verfügt über eine Eigenschaft Zuordnung auf eine Spalte mit einem Wert für die Spalte für die Zeile.</td></tr>

<tr><td>AccessInstruction ("AccessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>MIME-Typ</td><td>Zeichenfolge</td><td>Der MIME-Typ des Inhalts.</td></tr>
<tr><td></td><td>Inhalt</td><td>Zeichenfolge</td><td>Informationen zur Verwendung der Zugriff auf diese Daten Anlage zu erhalten. Der Inhalt kann eine URL, eine e-Mail-Adresse oder eine Reihe von Anweisungen sein.</td></tr>

<tr><td>TableDataProfile ("TableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>Ganzzahl</td><td>Die Anzahl der Zeilen in der Datengruppe zurück</td></tr>
<tr><td></td><td>Größe</td><td>lange</td><td>Die Größe der Datenmenge in Byte.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>Zeichenfolge</td><td>Zeitpunkt der letzten wurde das Schema geändert.</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>Zeichenfolge</td><td>Der letzten Datenmenge geändert wurde (Daten hinzugefügt wurde, wird geändert, oder löschen)</td></tr>

<tr><td>ColumnsDataProfile ("ColumnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Spalten</td></td><td>ColumnDataProfile]</td><td>Ein Array von Spalte Daten Profile.</td></tr>

<tr><td>ColumnDataClassification ("ColumnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName</td><td>Zeichenfolge</td><td>Der Name der Spalte, der auf dieser Klassifizierung verweist.</td></tr>
<tr><td></td><td>Klassifizierung</td><td>Zeichenfolge</td><td>Die Klassifizierung der Daten in dieser Spalte.</td></tr>

<tr><td>Dokumentation ("Dokumentation")</td><td></td><td></td><td>Eine angegebene Anlage kann nur eine Dokumentation zugeordnet verfügen.</td></tr>
<tr><td></td><td>MIME-Typ</td><td>Zeichenfolge</td><td>Der MIME-Typ des Inhalts.</td></tr>
<tr><td></td><td>Inhalt</td><td>Zeichenfolge</td><td>Inhalt der Dokumentation.</td></tr>

</table>

### <a name="common-types"></a>Gängige Typen

Gängige Typen als Typen für Eigenschaften verwendet werden können, aber nicht Elemente sind.
<table>
<tr><td><b>Allgemeine Typ</b></td><td><b>Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sourceType</td><td>Zeichenfolge</td><td>Beschreibt die Art der Datenquelle.  Beispiel: SQLServer, Oracle-Datenbank usw..  </td></tr>
<tr><td></td><td>Objekttyp</td><td>Zeichenfolge</td><td>Beschreibt die Art des Objekts in der Datenquelle. Beispiel: Tabelle, zeigen Sie für SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Protokoll</td><td>Zeichenfolge</td><td>Erforderlich. Beschreibt, ein Protokoll zur Kommunikation mit der Datenquelle verwendet wird. Beispiel: "Tds" für SQl Server, "Oracle" für Oracle usw.. Die Liste der derzeit unterstützten Protokolle finden Sie unter [Datenquelle Spezifikation - DSL Struktur verweisen](data-catalog-dsr.md) .</td></tr>
<tr><td></td><td>Adresse</td><td>Wörterbuch<string, object></td><td>Erforderlich. Adresse ist eine Gruppe von Daten, die speziell für das Protokoll, das zum Identifizieren der Datenquelle, auf die verwiesen wird. Die Adressdaten auf ein bestimmtes Protokoll ausgelegte ist d. h., es sinnlos ohne das Protokoll.</td></tr>
<tr><td></td><td>Authentifizierung</td><td>Zeichenfolge</td><td>Optional. Die Authentifizierung des Farbschemas verwendet zur Kommunikation mit der Datenquelle. Beispiel: Windows, Oauth usw..</td></tr>
<tr><td></td><td>connectionProperties</td><td>Wörterbuch<string, object></td><td>Optional. Weitere Informationen zum Herstellen einer Verbindung mit einer Datenquelle.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>Die Back-End-führt keine Validierung von bereitgestellten Eigenschaften gegen AAD während der Veröffentlichung.</td></tr>
<tr><td></td><td>Benutzerprinzipalnamen</td><td>Zeichenfolge</td><td>Eindeutige e-Mail-Adresse des Benutzers. Muss angegeben werden, wenn ObjectId nicht angegeben ist oder im Kontext der "LastRegisteredBy"-Eigenschaft, andernfalls optional.</td></tr>
<tr><td></td><td>objectId</td><td>GUID</td><td>Benutzer oder eine Sicherheitsgruppe Gruppe AAD Identität. Optional. Muss angegeben werden, wenn Benutzerprinzipalnamen nicht, andernfalls optional bereitgestellt wird.</td></tr>
<tr><td></td><td>Vorname</td><td>Zeichenfolge</td><td>Der Vorname des Benutzers (zu Anzeigezwecken). Optional. Gültig nur im Kontext der Eigenschaft "LastRegisteredBy". Kann nicht angegeben werden, wenn Sicherheit Tilgungsanteile "Rollen", "Berechtigungen" und "Experten" zur Verfügung.</td></tr>
<tr><td></td><td>Nachname</td><td>Zeichenfolge</td><td>Nachname des Benutzers (zu Anzeigezwecken). Optional. Gültig nur im Kontext der Eigenschaft "LastRegisteredBy". Kann nicht angegeben werden, wenn Sicherheit Tilgungsanteile "Rollen", "Berechtigungen" und "Experten" zur Verfügung.</td></tr>

<tr><td>Spalte</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Namen</td><td>Zeichenfolge</td><td>Name der Spalte / Attribut.</td></tr>
<tr><td></td><td>Typ</td><td>Zeichenfolge</td><td>der Datentyp der Spalte / Attribut. Die zulässigen Typen abhängig von Daten SourceType der Anlage.  Es wird nur eine Teilmenge der Typen unterstützt.</td></tr>
<tr><td></td><td>maxLength</td><td>Ganzzahl</td><td>Die maximale Länge für die Spalte oder Attribut zulässig sind. Abgeleitet von Datenquelle. Gilt nur für einige Arten von Datenquellen.</td></tr>
<tr><td></td><td>Genauigkeit</td><td>Byte-Zeichen</td><td>Die Genauigkeit für die Spalte oder das Attribut. Abgeleitet von Datenquelle. Gilt nur für einige Arten von Datenquellen.</td></tr>
<tr><td></td><td>isNullable</td><td>Boolesch</td><td>Gibt an, ob die Spalte zulässig ist einen Nullwert oder nicht haben. Abgeleitet von Datenquelle. Gilt nur für einige Arten von Datenquellen.</td></tr>
<tr><td></td><td>Ausdruck</td><td>Zeichenfolge</td><td>Wenn der Wert eine berechnete Spalte ist, enthält das Feld den Ausdruck, der den Wert angibt. Abgeleitet von Datenquelle. Gilt nur für einige Arten von Datenquellen.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>Zeichenfolge</td><td>Der Name der Spalte</td></tr>
<tr><td></td><td>Typ </td><td>Zeichenfolge</td><td>Den Typ der Spalte</td></tr>
<tr><td></td><td>Min </td><td>Zeichenfolge</td><td>Den kleinsten Wert in der Datengruppe zurück</td></tr>
<tr><td></td><td>Max </td><td>Zeichenfolge</td><td>Den größten Wert in der Datengruppe zurück</td></tr>
<tr><td></td><td>Mittelwert </td><td>Doppelte</td><td>Mittelwert der mittlere Wert in der Datengruppe zurück</td></tr>
<tr><td></td><td>STABW </td><td>Doppelte</td><td>Die Standardabweichung für den Datensatz ein</td></tr>
<tr><td></td><td>nullCount </td><td>Ganzzahl</td><td>Die Anzahl der null-Werte in der Datengruppe zurück</td></tr>
<tr><td></td><td>distinctCount  </td><td>Ganzzahl</td><td>Die Anzahl von unterschiedlichen Werten in der Datengruppe zurück</td></tr>


</table>

## <a name="asset-identity"></a>Anlage Identität
Azure Datenkatalog verwendet "Protokoll" und Identität Eigenschaften aus der "Adresse" Eigenschaftensammlung der Eigenschaft "Dsl" DataSourceLocation, um verwendet wird, um die Anlage innerhalb des Katalogs Adresse Identität der Ressource, zu generieren.
Angenommen, hat das Protokoll "Tds" Identität Eigenschaften "Server", "Datenbank", "Schema" und "Objekt". Die Kombinationen aus das Protokoll und die Identitätseigenschaften werden verwendet, um die Identität des die SQL Server-Tabelle Anlage generieren.
Azure Datenkatalog bietet mehrere integrierten Datenquelle Protokolle, bei [Datenquellen-Referenz Spezifikation - DSL Struktur](data-catalog-dsr.md)aufgelistet werden.
Festlegen der unterstützten Protokolle kann programmgesteuert erweitert werden (Siehe Datenkatalog REST-API-Referenz). Administratoren des Katalogs können benutzerdefinierte Datenquelle Protokolle registrieren. Die folgende Tabelle beschreibt die Eigenschaften zum Registrieren eines benutzerdefinierten Protokolls erforderlich sind.

### <a name="custom-data-source-protocol-specification"></a>Benutzerdefinierte Protokoll Datenquellenangabe
<table>
<tr><td><b>Typ</b></td><td><b>Eigenschaften</b></td><td><b>Datentyp</b></td><td><b>Kommentare</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Namespace</td><td>Zeichenfolge</td><td>Der Namespace des das Protokoll. Namespace muss von 1 bis 255 Zeichen umfassen, eine oder mehrere nicht leeres Parts getrennt durch Punkt (.) enthalten. Umschalttaste muss von 1 bis 255 Zeichen lang sein, mit einem Buchstaben beginnen und nur Buchstaben und Zahlen enthalten.</td></tr>
<tr><td></td><td>Namen</td><td>Zeichenfolge</td><td>Der Name der das Protokoll. Name muss von 1 bis 255 Zeichen lang sein, mit einem Buchstaben beginnen und nur Buchstaben, Zahlen und der Bindestrich (-) enthalten.</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty]</td><td>Liste der Identitätseigenschaften, muss mindestens eine, aber nicht mehr als 20 Eigenschaften enthalten. Beispiel: "Server", "Datenbank", "Schema", "Objekt" sind Identitätseigenschaften für das Protokoll "Tds".</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet]</td><td>Liste der Identität festgelegt. Definiert Sätze Identitätseigenschaften, die gültige Anlage Identität darstellen. Muss mindestens eine, aber nicht mehr als 20 Datensätze enthalten. Beispiel: {"Server", "Datenbank", "Schema" und "Objekt"} ist eine Identität festlegen für Protokoll "Tds" die Identität des Sql Server-Tabelle Objekt definiert.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Namen</td><td>Zeichenfolge</td><td>Der Name der Eigenschaft. Name liegt zwischen 1 und 100 Zeichen lang sein, die mit einem Buchstaben beginnen und kann nur Buchstaben und Zahlen enthalten.</td></tr>
<tr><td></td><td>Typ</td><td>Zeichenfolge</td><td>Die Art der Eigenschaft. Werte unterstützt: "boolesche" Boolesch ","Byte","Guid","Int","Integer","long","string","Url"</td></tr>
<tr><td></td><td>ignoreCase</td><td>bool</td><td>Gibt an, ob der Fall ignoriert werden soll, wenn der Wert Eigenschaft verwenden. Können nur für Eigenschaften vom Typ "String" angegeben werden muss. Standardwert ist "false".</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>Bool]</td><td>Gibt an, ob die Anfrage für jedes Segment die Url der Pfad ignoriert werden soll. Können nur für Eigenschaften vom Typ "Url" angegeben werden muss. Standardwert ist [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Namen</td><td>Zeichenfolge</td><td>Der Name der Identität festgelegt.</td></tr>
<tr><td></td><td>Eigenschaften</td><td>String]</td><td>Die Liste von Identitätseigenschaften in diese Identität festgelegt. Es kann keine Duplikate enthalten. Jede Eigenschaft festgelegte Identität optimiert muss in der Liste der "IdentityProperties", der das Protokoll definiert werden.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Rollen und Autorisierung

Microsoft Azure Datenkatalog bietet Autorisierungsfunktionen für Vorgänge auf Bestand und Anmerkungen.

## <a name="key-concepts"></a>Grundlegende Konzepte

Die Datenkatalog Azure verwendet zwei Autorisierungsmechanismen:

- Rollenbasierte Autorisierung
- Berechtigung-basierte Autorisierung

### <a name="roles"></a>Rollen

Es gibt drei Rollen: **Administrator**, **Besitzer**und **Mitwirkender**.  Jede Rolle verfügt über Umfang und die Verwaltung von Informationsrechten, die in der folgenden Tabelle zusammengefasst werden.

<table><tr><td><b>Rolle</b></td><td><b>Bereich</b></td><td><b>Verwaltung von Informationsrechten</b></td></tr><tr><td>Administrator</td><td>Katalog (alle Objekte/Anmerkungen im Katalog)</td><td>Lesen löschen ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Besitzer</td><td>Jede Anlage (Root Element)</td><td>Lesen löschen ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Mitwirkenden</td><td>Jede einzelne Anlage und Anmerkungen</td><td>Lesen Update löschen ViewRoles Hinweis: alle Rechte sind widerrufen, wenn lesen direkt auf das Element aus der Mitwirkende gesperrt ist</td></tr></table>

> [AZURE.NOTE] **Lesen**, **Aktualisieren**, **Löschen**, **ViewRoles** Rechte gelten für alle Elemente (Posten oder Anmerkungen) während **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** gelten nur für die Anlage aus.
>
>**Löschen** rechts gilt für ein Element und alle Unterelemente oder darunter einzelnes Element. Löschen einer Anlageguts löscht beispielsweise auch alle Anmerkungen für die Anlage.

### <a name="permissions"></a>Berechtigungen

Berechtigung ist von Einträgen in einer Liste. Jeder Eintrag weist Satz von Rechten für Hauptbenutzer eines Wertpapiers zurück. Berechtigungen können nur für eine Anlage (d. h. Root Element) angegeben werden, und beziehen sich auf die Anlage und Unterelemente.

Nur **gelesen** wird bei der Vorschau **Azure-Datenkatalog** in der Berechtigungsliste zum Aktivieren eines Wirtschaftsguts Sichtbarkeit einschränken Szenarios unterstützt.

Standardmäßig weist jeder authentifizierter Benutzer **gelesen** rechts für alle Elemente im Katalog, es sei denn, Sichtbarkeit auf den Satz von Hauptbenutzer im Feld Berechtigungen beschränkt ist.

## <a name="rest-api"></a>REST-API

**Setzen** und **Beitrag** Ansicht Element Anfragen zum Steuern von Rollen und Berechtigungen verwendet werden können: Zusätzlich Element gefährliche Fracht, zwei Systemeigenschaften angegebenen **Rollen** und **Berechtigungen**werden können.

> [AZURE.NOTE]
>
> die **Berechtigungen** gilt nur für ein Element aus.
>
> **Besitzer** Rolle gilt nur für ein Element aus.
>
> Wenn ein Element im Katalog erstellt wird, wird deren **Mitwirkender** standardmäßig an den gerade authentifizierten Benutzer festgelegt. Wenn Artikel nach jeder aktualisierbar sein soll, sollte **Mitwirkender** festgelegt werden, um &lt;jeder&gt; spezielle Sicherheit Hauptbenutzer in die Eigenschaft **Rollen** , wenn das Element zum ersten Mal veröffentlicht (finden Sie im folgenden Beispiel). **Mitwirkender** nicht geändert werden können und die Ressourceneinheiten beim Lebensdauer eines Elements (auch **Administrator** oder **Besitzer** verfügt nicht über das Recht, die **Mitwirkender**ändern). Unterstützt für die explizite Einstellung des **Mitwirkender** ist nur ein Wert &lt;jeder&gt;: **Mitwirkender** kann nur ein Benutzer, der ein Element erstellt oder &lt;jeder&gt;.

###<a name="examples"></a>Beispiele
**Legen Sie Mitwirkender auf &lt;jeder&gt; beim Veröffentlichen eines Elements.**
Spezielle Sicherheit Tilgungsanteile &lt;jeder&gt; ObjectId "00000000-0000-0000-0000-000000000201" enthält.
  **Beitrag** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

  > [AZURE.NOTE] Einige HTTP-Client-Implementierungen möglicherweise automatisch Besprechungsanfragen in der Antwort auf eine 302 vom Server wiederholen, aber Autorisierung Überschriften aus der Anforderung in der Regel zu entfernen. Da die Autorisierung Kopfzeile Azure Datenkatalog anzufordern erforderlich ist, müssen Sie sicherstellen, dass die Autorisierung Kopfzeile weiterhin bereitgestellt wird, wenn eine Anforderung an eine Umleitung Position von Azure Datenkatalog angegebenen erneut ausgestellt. Der folgende Code veranschaulicht diese mithilfe des Objekts .NET HttpWebRequest.

**Textkörper**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Besitzer zuweisen und Einschränken der Sichtbarkeit für einen vorhandenen Stamm-Artikel**: https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30 **setzen**

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [AZURE.NOTE] Abgelegt es wurde nicht erforderlich, geben Sie ein Element Aufkommen im Textkörper: sich kann verwendet werden, um nur die Rollen und/oder Berechtigungen zu aktualisieren.

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
