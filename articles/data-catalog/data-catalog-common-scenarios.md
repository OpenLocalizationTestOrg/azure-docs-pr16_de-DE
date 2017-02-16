<properties
   pageTitle="Häufige Szenarien mit Azure Datenkatalog | Microsoft Azure"
   description="Eine Übersicht über häufige Szenarien Azure Datenkatalog, einschließlich der Registrierung und Erkennung von hohem Wert Datenquellen Aktivieren der Self-service Business Intelligence, und erfassen vorhandene tribal wissen zu Datenquellen und Prozessen."
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
   ms.date="10/03/2016"
   ms.author="maroche"/>


# <a name="azure-data-catalog-common-scenarios"></a>Häufige Szenarien mit Azure Datenkatalog

In diesem Artikel präsentiert allgemeine Szenarien, die Stelle, an der Azure Datenkatalog größeren Nutzen aus ihrer vorhandenen Datenquellen abrufen Organisationen helfen können.

## <a name="scenario-1---registration-of-central-data-sources"></a>Szenario #1: Registrierung von zentralen Datenquellen

Organisationen haben oft eine Anzahl von Datenquellen mit hohem Wert. Diese Datenquellen sind unter anderem Textzeile Business OLTP-Systeme, Daten Lager und Business Intelligence / Analytics-Datenbanken. Die Anzahl der Systeme und die Überlappung zwischen Systemen, häufig wächst, über einen Zeitraum als den Anforderungen der Business Evolve, und als Unternehmen selbst durch Fusionen und Akquisitionen weiterentwickelt.

Häufig ist es schwierig, damit Benutzer wissen, wo Sie die Daten in diesen Datenquellen gesucht werden soll. Solche Fragen sind viel zu häufig:

- Der drei HR Systeme innerhalb des Unternehmens verwendet, die verwende ich diese Art von Bericht zu erstellen?
- Wo sollte ich mich, um die zertifizierten Verkaufszahlen für das Geschäftsjahr abzurufen, die gerade beendet?
- Wer soll ich Fragen oder Erstellungsprozess, die ich erhalten Zugriff auf das Datawarehouse verwenden soll?
- Ich weiß nicht, wenn diese Zahlen rechts – sind, die ich Fragen können für einen Einblick auf wie diese Daten verwendet werden, bevor ich mein Team diese Dashboard freigeben überfüllt ist?

In diesem Szenario kann Azure Datenkatalog helfen. Die zentralen, wertvolle, IT-Abteilung verwalteten Datenquellen, die in der gesamten Organisation verwendet werden, ist häufig logische Ausgangspunkt für den Katalog auffüllen. Obwohl jeder Benutzer eine Datenquelle registrieren kann, wird den Katalog mit den Datenquellen, die am ehesten Wert die größte Zahl von Benutzern zur Verfügung stellen, kick-started Laufwerk Annahme und Verwenden des Systems unterstützen. Für Kunden, die erste Schritte mit Azure Datenkatalog können identifizieren und registriert die wichtigsten viele verschiedene Teams von Datennutzer verwendeten Datenquellen im ersten Schritt zum Erfolg sein.

Dieses Szenario bietet sich auch die Möglichkeit zur Kommentierung von hohem Wert Datenquellen um leichter zu verstehen und den Zugriff zu machen. Ein Key Aspekt dieser Aufwand ist einschließen Informationen wie Benutzern Zugriff auf die Datenquelle anfordern können. Datenkatalog Azure ermöglicht Benutzern die e-Mail-Adresse des Benutzers oder zum Steuern des Zugriffs für Datenquelle, Links zu vorhandenen Tools oder Dokumentation oder kostenlosen Text, der den Access Anforderungsprozess beschreibt, verantwortlichen Team bereitstellen. Mit diesen Informationen im Katalog enthalten können Benutzer, die registrierten Datenquellen ermitteln, aber wer verfügen noch nicht über Berechtigungen zum Zugriff auf die Daten, auf einfache Weise Access mithilfe der Prozesse definiert und gesteuert, indem Sie die Datenquelle Besitzer anfordern.

## <a name="scenario-2---self-service-business-intelligence"></a>Szenario #2: Self-service Business intelligence

Obwohl traditionelle corporate Business Intelligence-Lösungen ein besonderem Bestandteil viele Organisationen Daten Landschaften sein weiterhin, hat die Änderung Tempo des Business Self-service-BI wichtiger. Self-service-BI ermöglicht Information Worker und Analysten, ihre eigenen Berichte, Arbeitsmappen und Dashboards zu erstellen, ohne sich auf einem zentralen IT-Team – oder durch die IT-Team Zeitplan und Verfügbarkeit eingeschränkt zu verlassen.

In Self-service-BI Szenarien ist es für Benutzer zum Kombinieren von Daten aus mehreren Quellen, von denen viele nicht zuvor für BI und Analyse verwendet wurden möglicherweise allgemeine. Obwohl bereits einige dieser Datenquellen bekannt sein kann, ist häufig ein Vorgang zu ermitteln, was stattfinden muss zu suchen und mögliche Datenquellen für eine bestimmte Aufgabe auswerten.

In der Vergangenheit Dieser Erkennungsvorgang ist eine manuelle: Analysten werden deren Peer-Netzwerk-Verbindungen verwenden, um andere Personen zu identifizieren, die mit den gesuchten Daten arbeiten. Nachdem Sie eine Datenquelle gefunden wird, kann verwendet werden, aber der Vorgang wiederholt sich erneut für jedes nachfolgende Self-service BI leistungsgesteuert, mit mehreren Benutzern, die gleichen redundanten manuellen Prozess der Suche ausführt.

Benutzer können mit Azure Datenkatalog diesen Zyklus überflüssiger Aufwand getrennt werden. Nachdem Sie eine Datenquelle über herkömmliche Mittel erkannt wurde, kann Analysten die Datenquelle, um es in Zukunft einfacher auffindbar zu machen registrieren. Obwohl der Benutzer Nutzwert durch die registrierten Datenbestände Stapels hinzufügen kann, muss dies nicht zur gleichen Zeit wie Registrierung stattfinden. Benutzer können im Laufe der Zeit als deren Genehmigung Terminpläne mitwirken allmählich die Datenquellen im Katalog registriert Wert hinzu.

Diese organisch Wertzuwachs Kataloginhalt ist natürlich Ergänzung zu den betreiben Registrierung von zentralen Datenquellen. Vorab Auffüllen des Katalogs mit Daten, die viele Benutzer müssen, kann ein wichtigste Faktor bei zum ersten Mal verwenden und Discovery sein. Zulassen, dass Benutzer registriert und kommentieren zusätzliche Quellen kann eine Möglichkeit, und ihre Kollegen, nicht nachlässt beibehalten werden.

Es ist auch zu beachten, dass zwar speziell auf Self-service-BI dieses Szenario Schwerpunkt, die gleichen Muster und Probleme auf großer corporate BI-Projekte anzuwenden. Alle, die eine manuelle Quelle Datenermittlung umfasst neben leistungsgesteuert ist ein Aufwand, der Wert für die Organisation durch die Verwendung von Azure Datenkatalog hinzufügen können.

## <a name="scenario-3---capturing-tribal-knowledge"></a>Szenario #3 - aufgezeichneten tribal knowledge

Woher wissen Sie, welche Daten Sie Ihre Arbeit und, wo die Daten zu finden müssen?

Wenn Sie in Ihrem Auftrag eine Weile befunden haben, wissen Sie wahrscheinlich einfach. Sie lernen Prozess schrittweise vorgenommen haben und über einen Zeitraum haben gelernt, über die Datenquellen, die-Taste, um Ihre Arbeit Tag sind.

Wenn ein neuer Mitarbeiter das Team beitritt, wie weiß er welche Daten er seine Position und wo Sie sich befinden, gehen Sie wie folgt muss?

Chancen sind, denen Sie aufgefordert werden.

Diese laufenden Übertragung tribal Kenntnisse ist Teil der Discovery-Prozess in große und kleine Unternehmen. Leitende und erfahrene Teammitglieder haben den Jahren von Knowledge aufgebaut und neuere Teammitglieder haben gelernt, wie sie Fragen, wenn sie Fragen haben. Die wichtige Informationen häufig vorhanden sind, nur in der Spitzen ein paar wichtige Personen, und wenn diese Personen über Ihre Abwesenheit sind oder das Team verlassen, bleibt die Organisation.

Manchmal werden diese Daten Experten der Leistung zu dokumentieren Sie ihr Wissen, stellen Sie per e-Mail oder in Word-Dokumenten auf einer SharePoint-Teamwebsite Freigabe. Obwohl dies hilfreich sein, es wird ein neues Discovery-Problem – eingeführt werden kann können wie Personen feststellen, welche Dokumentation vorhanden ist, und wo Sie sich befinden...

Azure Datenkatalog bietet einen Speicherort für die gemeinsame Nutzung dieses tribal wissen, und für sie leicht auffindbar zu machen. Daten-Experten Datenbestände können direkt Kommentieren und können auch Links zu vorhandenen Dokumentation einbeziehen. Nicht nur unterstützt das Wissen selbst erfassen, sondern auch zusammengeführt die Kenntnisse werden, in der gleichen Oberfläche, die für die Datenquelle Datenermittlung verwendet wird. Wenn jemand den Katalog verwendet, um zu ermitteln sich die Datenquelle nicht nur wird er die Quelle selbst suchen, er findet auch des Experten Knowledge, die zuvor nur in die Meinung von der Experte selbst.
