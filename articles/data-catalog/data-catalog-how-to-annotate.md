<properties
   pageTitle="So kommentieren von Datenquellen | Microsoft Azure"
   description="Gewusst wie-Artikel so Datenbestände in Azure Datenkatalog, einschließlich Anzeigenamen, Kategorien, Beschreibungen und Experten kommentieren hervorheben."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>So kommentieren von Datenquellen

## <a name="introduction"></a>Einführung
**Microsoft Azure Datenkatalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und der Suche für Enterprise-Datenquellen System dient. Kurzum, geht Datenkatalog beiträgt ermitteln, verstehen und Verwenden von Datenquellen und Hilfe Organisationen, um größeren Nutzen aus ihrer vorhandenen Daten zu ziehen. Wenn Sie eine Datenquelle mit Datenkatalog registriert ist, seine Metadaten kopiert und vom Dienst indiziert ist, aber die Geschichte wird nicht es beenden. Datenkatalog kann Benutzer eigene beschreibende Metadaten – wie etwa Beschreibungen und Tags – ergänzen die Metadaten aus der Datenquelle extrahiert und nehmen Sie die Datenquelle an mehrere Personen verständlicher bereitstellen.

## <a name="annotation-and-crowdsourcing"></a>Anmerkung und crowdsourcing
Jeder kann Stellung. Und dies ist eine gute Sache.
Datenkatalog erkennt, dass andere Benutzer verschiedene Perspektiven für Enterprise-Datenquellen verfügen und jeder der folgenden Perspektiven für nützlich sein kann. Erwägen Sie das folgende Szenario:

* Der Systemadministrator weiß Servicelevel für den Server oder Dienste, die als Host der Datenquelle.
* Der Datenbankadministrator kennt den Sicherung Zeitplan für jede Datenbank und der zulässige Datenlebenszyklus Windows.
* Der Besitzer des Systems weiß Aktualisierungsprozess für Benutzer zum Anfordern des Zugriffs auf die Datenquelle an.
* Der Data Steward weiß das Enterprise-Datenmodell wie den Bestand und die Attribute in der Datenquelle zugeordnet.
* Die Analysten weiß, wie die Daten im Kontext von Geschäftsprozessen verwendet werden, die er unterstützt.

Jeder der folgenden Perspektiven besitzt, und Datenkatalog verwendet einen Ansatz Crowdsourcing-Metadaten, die jeweils erfasst und zum Angeben der eines vollständigen Bilds der registrierten Datenquellen verwendet werden können. Über das Datenkatalog-Portal an, kann jeder Benutzer hinzufügen und Bearbeiten von eigenem Anmerkungen, beim Anzeigen von Anmerkungen, die durch andere Benutzer bereitgestellt wird.

## <a name="different-types-of-annotations"></a>Verschiedene Typen von Anmerkungen
Datenkatalog unterstützt die folgenden Arten von Anmerkungen an:

| Anmerkung     | Notizen                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Anzeigename  | Anzeigenamen können zur Datenebene auf Anlage, die einfacher verstanden Datenbestände vornehmen bereitgestellt werden. Anzeigenamen sind besonders hilfreich, wenn Sie den Objektnamen des zugrunde liegenden nicht unmittelbar verständlicher, abgekürzten oder andernfalls nicht für Benutzer von Bedeutung ist.                                                                                                                            |
| Beschreibung    | Beschreibungen bereitgestellt werden können, die Daten Objekt und Attribut / Spalte Ebenen. Beschreibungen sind Freiform-kurzer Text Anmerkungen, die des Benutzers beschreiben Perspektive auf die Anlage Daten oder deren Nutzung.                                                                                                                                                              |
| Kategorien (Benutzer Tags)          | Kategorien bereitgestellt werden können, die Daten Objekt und Attribut / Spalte Ebenen. Benutzer-Tags sind benutzerdefinierte Etiketten, die zum Kategorisieren Datenbestände oder Attributen verwendet werden können.                                                                                                                                                                                                    |
| Kategorien (Glossar Tags)          | Kategorien bereitgestellt werden können, die Daten Objekt und Attribut / Spalte Ebenen. Glossar Kategorien sind zentral definiert Glossar Ausdrücke, die zum Kategorisieren Datenbestände oder eine allgemeine Business Taxonomie mit Attributen verwendet werden können. Weitere Informationen finden Sie unter [Informationen zum Einrichten der Business-Glossar für Tagging geregelt](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Experten        | Experten können auf die Anlage Datenebene bereitgestellt werden. Experten können Identifizieren von Benutzern oder Gruppen mit Experten Perspektiven auf der Registerkarte Daten und dienen als Ansprechpartner für Benutzer, die die registrierten Datenquellen ermitteln und haben Sie Fragen, die nicht von der vorhandenen Anmerkungen beantwortet werden.  |
| Anfordern des Zugriffs | Anfordern von Access Informationen kann auf die Anlage Datenebene bereitgestellt werden. Diese Informationen sind für Benutzer, die eine Datenquelle ermitteln, die sie noch nicht über die Berechtigungen für den Zugriff auf verfügen. Benutzer können die e-Mail-Adresse des Benutzers oder der Gruppe, die die URL des Prozesses oder Tool an die Benutzer Zugang benötigen, Zugriff gewährt oder den Prozess selbst als Text eingeben. |
| Dokumentation | Dokumentation kann auf die Anlage Datenebene bereitgestellt werden. Anlage Dokumentation ist Rich-Text-Informationen, die Links und Bilder enthalten können und die können alle Informationen, die nicht über eine Beschreibung und Tags vermittelt bereitstellen. |


## <a name="annotating-multiple-assets"></a>Mehrere Anlagen Stapels
Wenn Sie mehrere Datenbestände im Portal Datenkatalog auswählen, können Benutzer alle ausgewählte Anlagen in einem einzigen Vorgang kommentieren. Anmerkungen gilt für alle ausgewählten Anlagen, sodass sie mühelos zu markieren, und stellen Sie eine konsistente Beschreibung und Sätze von Tags und Experten für verwandte Datenbestände.

> [AZURE.NOTE] Kategorien und Experten können auch beim Registrieren Datenbestände Datenkatalog Daten mithilfe des Tools für Registrierung Datenquelle bereitgestellt werden.

Wann wird Auswahl mehrerer Tabellen und Sichten, nur Spalten, dass alle Daten ausgewählt, die Ressourcen gemeinsam haben im Portal Datenkatalog angezeigt. Dies ermöglicht Benutzern, Tags und Beschreibungen für alle Spalten mit demselben Namen für alle ausgewählten Anlagen bereitzustellen.

## <a name="annotations-and-discovery"></a>Anmerkungen und Suche
Genauso wie die Metadaten aus der Datenquelle extrahiert werden, während der Registrierung Datenkatalog Suchindex hinzugefügt wird, wird auch benutzerdefinierte Metadaten indiziert. Dies bedeutet, dass nicht nur Anmerkungen erleichtern Benutzern, die Daten zu verstehen, die sie ermitteln, Anmerkungen auch erleichtern für Benutzer zum Ermitteln der kommentierten Datenbestände durch Suchen mit den Konditionen, die sinnvoll zu.

## <a name="summary"></a>Zusammenfassung
Registrieren einer Datenquelle mit Datenkatalog macht die Daten durch Kopieren strukturelle und beschreibenden Metadaten aus der Datenquelle in den Katalog-Dienst sichtbar. Nachdem eine Datenquelle registriert wurde, können Benutzer bereitstellen, Anmerkungen, ermitteln und aus innerhalb des Portals Datenkatalog Grundlegendes zu vereinfachen.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Azure Datenkatalog](data-catalog-get-started.md) Lernprogramm schrittweise ausführliche Informationen zum Kommentieren von Datenquellen.
