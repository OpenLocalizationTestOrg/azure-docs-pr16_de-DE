<properties
   pageTitle="Zum Registrieren von Datenquellen | Microsoft Azure"
   description="Gewusst wie-Artikel zum Registrieren von Datenquellen mit Azure Datenkatalog, einschließlich der bei der Registrierung extrahierten Metadatenfeldern hervorheben."
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


# <a name="how-to-register-data-sources"></a>Zum Registrieren von Datenquellen

## <a name="introduction"></a>Einführung
**Microsoft Azure Datenkatalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und der Suche für Enterprise-Datenquellen System dient. Kurzum, geht **Azure Datenkatalog** beiträgt ermitteln, verstehen und Verwenden von Datenquellen und Hilfe Organisationen, um größeren Nutzen aus ihrer vorhandenen Daten zu ziehen. Und im erste Schritt eine Datenquelle über **Azure Datenkatalog** auffindbar zu machen ist, um die Datenquelle zu registrieren.
## <a name="registering-data-sources"></a>Registrieren von Datenquellen
Die Registrierung ist die Vorgehensweise zum Extrahieren von Metadaten aus der Datenquelle, und kopieren die Daten in den **Datenkatalog Azure** -Dienst an. Die Daten bleiben, wo sie aktuell befindet, und unter dem Steuerelement der Administratoren und Richtlinien des aktuellen Systems bleibt.

Um eine Datenquelle zu registrieren, starten Sie einfach **Azure Datenkatalog** Datenquelle Tools für die Registrierung vom **Datenkatalog Azure** -Portal. Melden Sie sich mit Ihrer Arbeit oder Schule Konto (die gleichen Azure Active Directory-Anmeldeinformationen, die Sie verwenden, um mit dem Portal anmelden wird) und wählen Sie dann die Datenquelle, die Sie erfassen möchten.
Finden Sie das [Erste Schritte mit Azure Datenkatalog](data-catalog-get-started.md) Lernprogramm Weitere schrittweise Details.

Nachdem die Datenquelle registriert wurde, wird der Katalog verfolgt die Position und seine Metadaten, indiziert, damit Benutzer suchen, durchsuchen, und die Datenquelle ermitteln und verwenden Sie dann die Position, um mithilfe der Anwendung oder einem Tool ihrer Wahl eine Verbindung herstellen können.

## <a name="sources-supported"></a>Unterstützte Datenquellen
Lizenzinformationen finden Sie [Datenkatalog DSR](data-catalog-dsr.md) , für die Liste der derzeit unterstützten Datenquellen.
<br/>


## <a name="structural-metadata"></a>Strukturelle Metadaten
Wenn Sie eine Datenquelle registriert sind, Tools für die Registrierung extrahiert Informationen über die Struktur der Objekte, die Sie auswählen – dies gemäß als strukturelle Metadaten ist.

Für alle Objekte werden diese strukturelle Metadaten Position des Objekts, einbeziehen, sodass die Benutzer, die die Daten zu entdecken, die Informationen für die Verbindung auf das Objekt in den Clienttools ihrer Wahl verwenden können. Andere strukturelle Metadaten enthält Objektname und Typ aus, und geben Sie Attribut/Spaltenname und Daten.

## <a name="descriptive-metadata"></a>Beschreibende Metadaten
Zusätzlich zu den Core strukturelle Metadaten aus der Datenquelle extrahiert werden Daten-Tools für die Registrierung der Quelle auch sowie beschreibende Metadaten extrahieren. Für SQL Server Analysis Services und SQL Server Reporting Services wird dies aus der Beschreibung Eigenschaften verfügbar gemacht werden, indem Sie diese Dienste übernommen. Für SQL Server werden Werte liegen, verwenden die erweiterte Eigenschaft Ms_description extrahiert. Für Oracle-Datenbank werden Daten-Tools für die Registrierung der Quelle die Spalte Kommentare in der Ansicht ALL_TAB_COMMENTS extrahieren.

Zusätzlich zu den beschreibenden Metadaten extrahiert aus einer Datenquelle können Benutzer auch beschreibende Metadaten mithilfe der Tools für die Datenquelle Registrierung eingeben. Benutzer können Hinzufügen von Kategorien und identifizieren Experten für die Objekte, die gerade registriert wird. Alle diese beschreibenden Metadaten wird in der **Datenkatalog Azure** Service sowie die strukturelle Metadaten kopiert.

## <a name="including-previews"></a>Einschließlich Vorschauen

Standardmäßig nur Metadaten aus Datenquellen extrahiert und mit dem **Datenkatalog Azure** -Dienst kopiert haben, aber das Verständnis eine Datenquelle häufig vereinfacht wird durch sehen ein Beispiel für die Daten enthält.

Tools für die **Azure Datenkatalog** Datenquelle Registrierung ermöglicht Benutzern beziehen Sie eine Momentaufnahme Vorschau der Daten in jeder Tabelle und Ansicht, die registriert ist. Wenn der Benutzer-Optionen-in zum Einschließen von Vorschauen während der Registrierung werden Tools für die Registrierung bis zu 20 Datensätze aus jeder Tabelle und Ansicht einschließen. Diese Momentaufnahme wird dann in den Katalog zusammen mit den strukturelle und beschreibenden Metadaten kopiert.


> [AZURE.NOTE]  Möglicherweise müssen Sie Breite Tabellen mit einer großen Anzahl von Spalten in deren Vorschau weniger als 20 Datensätze.


## <a name="including-data-profiles"></a>Einschließlich Daten Profile

Wie einschließlich Vorschauen wertvolle Kontext für Benutzer von Datenquellen in **Azure Datenkatalog**nach bereitgestellt werden, kann einschließlich eines Profils Daten auch ermittelten Datenquellen Grundlegendes zu erleichtern.

Tools für die **Azure Datenkatalog** Datenquelle Registrierung ermöglicht Benutzern einschließen ein Profils Daten für jede Tabelle und Ansicht, die registriert ist. Wenn der Benutzer während der Registrierung ein Profils Daten aufnehmen möchten, enthalten Tools für die Registrierung Statistiken über die Daten in jeder Tabelle und Ansicht, einschließlich:

* Die Anzahl der Zeilen und die Größe der Daten in das Objekt
* Das Datum der letzten Aktualisierung der Daten und das Objektschema
* Die Anzahl der Datensätze mit Nullwerten und verschiedene Werte für Spalten
* Die Werte für Minimum, Maximum, Mittelwert und Standardabweichung für Spalten

Diese Statistiken werden in den Katalog zusammen mit den strukturelle und beschreibenden Metadaten kopiert.

> [AZURE.NOTE]  Text und Datum Spalten werden Mittelwert oder die Standardabweichung Statistiken nicht im Datenprofil der jeweiligen enthalten.

## <a name="updating-registrations"></a>Aktualisieren von Registrierungen

Registrieren einer Datenquelle wird im **Datenkatalog Azure** mithilfe der Metadaten und optional Vorschau während der Registrierung extrahiert auffindbar machen. Wenn die Datenquelle im Katalog aktualisiert werden (z. B., wenn das Schema eines Objekts geändert hat, Tabellen, die ursprünglich ausgeschlossen einbezogen werden soll, oder ein Benutzer möchte Aktualisieren der Daten in der Vorschau enthalten muss) kann Daten-Tools für die Registrierung der Quelle erneut ausführen.

Erneutes Registrieren einer bereits registriert Datenquelle führt einen Vorgang zusammenführen "Upsert": vorhandene Objekte aktualisiert, während die neuen Objekte erstellt werden sollen. Die Spalten Metadaten, die von den Benutzern über das Portal **Datenkatalog Azure** bereitgestellt werden.

## <a name="summary"></a>Zusammenfassung
Eine Datenquelle mit **Azure Datenkatalog** registrieren, sind die Datenquelle leichter zu ermitteln und zu verstehen, indem Sie strukturelle und beschreibenden Metadaten aus der Datenquelle in der Katalog-Dienst kopieren. Nachdem eine Datenquelle registriert wurde, können sie dann, verwalteten und mit dem **Datenkatalog Azure** -Portal ermittelten kommentierenden.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Azure Datenkatalog](data-catalog-get-started.md) Lernprogramm schrittweise Details zum Registrieren von Datenquellen.
