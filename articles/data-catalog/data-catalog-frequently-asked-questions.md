<properties
   pageTitle="Azure Datenkatalog häufig gestellte Fragen | Microsoft Azure"
   description="Häufig gestellte Fragen zur Azure-Datenkatalog, einschließlich der Funktionen für die Quelle Datenermittlung, Anmerkungen und Management."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure Datenkatalog

Dieser Artikel bietet Antworten für häufig gestellte Fragen im Zusammenhang mit Microsoft **Azure Datenkatalog** Dienst an.

## <a name="q-what-is-azure-data-catalog"></a>F: Was ist Datenkatalog Azure?

A: Microsoft Azure Datenkatalog ist eine vollständig verwaltete Dienst in der Cloud von Microsoft Azure, die als ein System der Registrierung und der Suche für Enterprise-Datenquellen System dient gehostet wird. Azure Datenkatalog bietet Funktionen, mit denen jeder Benutzer – aus Daten Wissenschaftlern für Entwickler – registrieren, Analysten ermitteln, verstehen und Datenquellen nutzen.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>F: Welche Kunden fordert Azure Datenkatalog bedeutet lösen?

Azure Datenkatalog Adressen die Herausforderung Quelle Datenermittlung und "dunkel-Daten" durch Benutzer ermitteln und Enterprise-Datenquellen zu verstehen.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>F: Wer sind die Zielgruppe für Datenkatalog Azure?

Azure Datenkatalog bietet Funktionen für Benutzer von technischen und nicht technischen, einschließlich:

- Daten Entwickler, BI und Analytics-Experten: Wer sind verantwortlich für die Erstellung von Daten und Analytics Inhalte für andere Personen Sie nutzen
-   Data Stewards: Diejenigen, die die Kenntnisse über die Daten, was dies bedeutet und wie es vorgesehen ist, werden und für welche Zwecke haben
- Datennutzer: Körperlich kann leicht ermitteln, verstehen und eine Verbindung mit Daten müssen, ihre Aufgaben mit dem Tool ihrer Wahl auszuführen erforderlich
- Zentrale IT: nur die müssen, die hundert Datenquellen für die Benutzer Business auffindbar zu machen, und wer müssen Aufsicht über wie die Daten verwendet wird und von wem

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>F: Was ist die Verfügbarkeit von Azure Datenkatalog Region?

Azure Datenkatalog Services sind in den folgenden Daten Centers derzeit verfügbar:

- Westen US
- Ostasiatische US
- Westen Europa
- North Europa
- Australien OST
- Oder Asien

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>F: Was sind die Grenzwerte für die Anzahl der Datenbestände in Azure Datenkatalog?

Die kostenlose Edition von Azure Datenkatalog beträgt 5.000 eingetragene Datenbestände.

Der Standard Edition von Azure Datenkatalog unterstützt bis zu 100.000 eingetragene Datenbestände.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>F: Was sind die unterstützten Datentypen Quell- und Anlage?

Lizenzinformationen finden Sie [Datenkatalog DSR](data-catalog-dsr.md) , für die Liste der derzeit unterstützten Datenquellen.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>F: wie anfordern ich Unterstützung für eine andere Datenquelle?

Feature Besprechungsanfragen und andere Feedback können im [Datenkatalog Azure-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)gesendet werden.

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>F: wie Schritte kann ich mit Azure Datenkatalog?

Am besten Schritte wird anhand der Anweisungen in [Erste Schritte mit dem Datenkatalog](data-catalog-get-started.md). In diesem Artikel wird eine End-to-End-Übersicht über die Funktionen im Dienst.

## <a name="q-how-do-i-register-my-data"></a>F: Wie registriere ich meine Daten?

Um die Daten in Azure Datenkatalog registrieren, starten Sie Tools für die Registrierung Azure Datenkatalog aus dem Bereich "Veröffentlichen" des Portals Azure Datenkatalog. Klicken Sie in der Veröffentlichung Datenkatalog Azure-Anwendung melden Sie sich mit den gleichen Anmeldeinformationen, die Sie verwenden, um das Datenkatalog Azure-Portal zugreifen, und wählen Sie dann aus der Datenquelle und den bestimmten Anlagen, die Sie erfassen möchten.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>F: welche Eigenschaften für Datenbestände extrahiert werden, die registriert werden?

Welche Eigenschaften unterscheidet sich von der Datenquelle Datenquelle, aber im Allgemeinen wird der Veröffentlichung Datenkatalog Azure-Dienst extrahiert die folgende Informationen:

- Name der Anlage
- Objekt-Datentyp
- Beschreibung der Anlage
- Attribut/Spaltennamen
- Attribut/Spalte Datentypen
- Beschreibung der Spalte/Attribut

> [AZURE.IMPORTANT] Registrieren Datenbestände mit Azure Datenkatalog nicht verschieben oder Kopieren von Daten in der Cloud. Anlagen aus einer Datenquelle registrieren, wird die Anlagen Metadaten in Azure kopieren, aber die Daten bleiben in den vorhandenen Speicherort der Datenquelle. Die einzige Ausnahme zu dieser Regel ist, wenn ein Benutzer Vorschau Datensätze oder ein Datenprofil hochladen möchte, bei der Registrierung Posten. Wenn einschließlich einer Vorschau der bis zu 20 Datensätze von jeder Anlage kopiert werden, und werden als Momentaufnahme in Datenkatalog Azure gespeichert. Wenn Sie ein Datenprofil einschließen, aggregieren Informationen (wie etwa die Größe von Tabellen, die Prozentsatz null-Werte pro Spalte und die Werte für minimum, maximum und Mittelwert für Spalten) berechnet und enthalten, in den Metadaten im Katalog gespeichert.

<br/>

> [AZURE.NOTE] Für Datenquellen wie SQL Server Analysis Services, die eine herausragende **Beschreibung** Eigenschaft verfügen, wird die Anwendung veröffentlichen Azure Datenkatalog dieser Eigenschaftswert extrahieren. Für relationale SQL Server-Datenbanken, die eine herausragende **Beschreibung** Eigenschaft verfügen, wird die veröffentlichenden Datenkatalog Azure-Anwendung den Wert aus der Ms_description erweiterte Eigenschaft für die Objekte und Spalten zu extrahieren. Weitere Informationen finden Sie unter TechNet [Verwenden von erweiterten Eigenschaften für Datenbankobjekte](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>F: wie lange sollte es für neu registrierte Anlagen Azure Datenkatalog angezeigt werden sollen?

Nachdem Sie Anlagen mit Azure Datenkatalog registriert es möglicherweise ein Zeitraum von 5 bis 10 Sekunden, bevor sie im Datenkatalog Azure-Portal angezeigt werden.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>F: wie kann ich Kommentieren und bereichern die Metadaten für meine registrierten Datenbestände?

Die einfachste Methode zum Bereitstellen von Metadaten für eingetragene Anlagen ist, wählen Sie die Anlage im Datenkatalog Azure-Portal, und geben die Metadatenwerte im Eigenschaftenbereich oder Schemabereich für das ausgewählte Objekt.

Sie können auch einige Metadaten, z. B. Experten und Tags, während der Registrierung bereitstellen. Die Werte in der Veröffentlichung Datenkatalog Azure-Dienst gelten für alle Anlagen, die zu diesem Zeitpunkt registriert wird. Zum Anzeigen der zuletzt registriert Objekte im Portal für zusätzliche Anmerkung wählen Sie **Ansicht Portal** Schaltfläche auf dem letzten Bildschirm der Anwendung veröffentlichen Azure Datenkatalog aus.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>F: wie kann ich meine registrierten Datenobjekte löschen?

Sie können ein Objekt aus Azure Datenkatalog löschen, indem Sie das Objekt im Portal markieren, und klicken Sie dann auf die Schaltfläche **Löschen** klicken. Entfernt die Metadaten für das Objekt aus Azure Datenkatalog, aber wirkt sich nicht auf die tatsächlichen zugrunde liegenden Datenquelle.

## <a name="q-what-is-an-expert"></a>F: Was ist ein Experte?

Rat ist eine Person, die eine laufenden Perspektive zu einem Datenobjekt enthält. Ein Objekt kann mehrere Experten aufweisen. Rat muss nicht "Besitzer" für ein Objekt; der Experte ist einfach eine Person, die weiß, wie die Daten können und verwendet werden soll.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>F: Wie gebe ich Informationen mit dem Datenkatalog Azure Team frei, wenn ich zu Problemen?

Verwenden Sie das Forum Azure Datenkatalog Probleme zu Berichten, gemeinsame Nutzung von Informationen und Fragen. Im Forum finden Sie unter http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>F: funktioniert Azure Datenkatalog mit dieser anderen Datenquelle, die hier interessant?
Wir arbeiten aktiv auf Weitere Datenquellen Azure Datenkatalog hinzufügen. Ist eine Datenquelle, die Sie möchten, finden Sie unter Unterstützte, wenden sie vorschlagen (oder Support für Ihren Voicemail, wenn es bereits vorgeschlagen wurden) im [Datenkatalog Azure-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>F: wie bezieht Azure Datenkatalog der Datenkatalog in Power BI für Office 365 sich auf?

Sie können als Weiterentwicklung des Katalogs Daten Datenkatalog Azure vorstellen. Azure Datenkatalog bietet ähnliche Funktionen für Datenquelle für die Veröffentlichung und Discovery, aber ist auf umfassendere Szenarien mit Fokus und nicht abhängig von Office 365. Kurz nach dem Datenkatalog Azure generell zur Verfügung steht werden in einen einzelnen Dienst die beiden Kataloge zusammenführen.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>F: benötigt welche Berechtigungen ein Benutzer Anlagen mit Azure Datenkatalog registrieren?

Der Benutzer ausführt Tools für die Registrierung Azure Datenkatalog benötigt Berechtigungen für die Datenquelle, die ihn zum Lesen der Metadaten aus der Quelle verwendet werden kann. Wenn der Benutzer auch markiert hat, eine Vorschau aufnehmen möchten, muss der Benutzer auch berechtigt, mit denen ihn in den Daten aus der Objekte, die gerade registriert lesen.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>F: werden werden Azure Datenkatalog zur lokalen Bereitstellung ebenfalls bereitgestellt?

Azure Datenkatalog ist eine Cloud-Dienst, mit der Cloud, und klicken Sie auf lokale Datenquellen zusammenarbeiten kann Vorführen einer Hybrid Datenquelle Discovery-Lösung. Es gibt derzeit keine Pläne für eine Version des Diensts Datenkatalog Azure, die lokal ausgeführt werden kann.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>F: extrahieren können wir weitere / aussagefähigere Metadaten aus den Datenquellen, die wir registrieren?

Wir arbeiten aktiv, um die Funktionen von Azure Datenkatalog zu erweitern. Ist zusätzliche Metadaten, die Sie, finden Sie unter extrahierten aus der Datenquelle während der Registrierung möchten, können Sie mir sagen sie (oder stimme zu lassen, wenn es bereits vorgeschlagen wurden) im [Datenkatalog Azure-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). In der Zukunft können wir Drittanbietern neue Arten von Datenquellen durch eine Erweiterbarkeits-API hinzufügen.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>F: wie einschränken ich die Sichtbarkeit registrierten Datenbestände, damit nur bestimmte Personen, die sie ermitteln können?

A: Wählen Sie A: der Datenbestände im Katalog Azure-Daten, und klicken Sie auf die Schaltfläche "Besitzrechte". Besitzer Datenbestände in Azure Datenkatalog können die sichtbarkeitseinstellungen für die, um alle Benutzer die Besitzer Ressourcen Ermittlung oder die Sichtbarkeit für bestimmte Benutzer einschränken Katalogs entweder zulassen ändern.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>F: wie kann ich die Registrierung für eine Anlage Daten aktualisieren, die in der Datenquelle geändert werden im Katalog angezeigt?

A: Wenn Sie um die Metadaten für Datenbestände zu aktualisieren, die bereits im Katalog registriert sind, einfach erneut registrieren Sie sich die Datenquelle, die die Objekte enthält. Alle Änderungen in der Datenquelle, wie Spalten hinzugefügt oder aus Tabellen oder Sichten entfernt, werden aktualisiert im Katalog, aber alle Anmerkungen, die vom Benutzer bereitgestellt werden beibehalten.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>F: Meine Frage ist nicht hier – Was sollte ich nicht beantwortet?

Gehen Sie auf über zu dem [Datenkatalog Azure-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Es gestellte Fragen finden Sie hier ihren Weg werden.
