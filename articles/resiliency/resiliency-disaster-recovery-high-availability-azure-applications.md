<properties
   pageTitle="Wiederherstellung und hohe Verfügbarkeit für Applikationen Azure | Microsoft Azure"
   description="Technische Übersichten und ausführliche Informationen zu Entwerfen von Applications für hohen Verfügbarkeit und Disaster Wiederherstellung von Applications auf Microsoft Azure erstellt."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Wiederherstellung und hohe Verfügbarkeit bei Microsoft Azure

##<a name="introduction"></a>Einführung

In diesem Artikel liegt der Schwerpunkt auf hohen Verfügbarkeit für Applikationen in Azure ausgeführt. Eine allgemeine Strategie für hohe Verfügbarkeit enthält auch den Aspekt der Wiederherstellung. Planen von Fehlern und Schäden in der Cloud, müssen Sie den Fehler schnell zu erkennen. Sie können dann eine Strategie, die Ihre Fehlertoleranz für der Ausfallzeit entspricht implementieren. Darüber hinaus müssen Sie das Ausmaß von Datenverlusten zu berücksichtigen, die die Anwendung tolerieren kann, ohne die unerwünschten Business folgen verursacht, während sie wiederhergestellt wird.

Die meisten Unternehmen angenommen, dass er für temporäre und umfangreichen Fehlern vorbereitet sind. Bevor Sie diese Frage für sich selbst testen Ihr Unternehmen diese Fehler? Testen Sie die Wiederherstellung von Datenbanken, um sicherzustellen, dass Sie die richtige Prozesse angeordnet haben? Wahrscheinlich nicht. Dies liegt daran erfolgreiche Wiederherstellung mit vielen Planung und Architektur zum Implementieren der folgende Prozesse wird gestartet. Wie viele andere funktioniert nicht Anforderungen, wie z. B. Sicherheit ruft Wiederherstellung selten der betreiben Analyse und die Uhrzeit Zuteilung, die erforderlich ist. Darüber hinaus müssen die meisten Unternehmen nicht die Ausgaben für örtlich verteilte Regionen mit redundanten Kapazität. Daher sind gerade umwandeln häufig gemischte Notfallplan Wiederherstellung ausgeschlossen.

Cloud-Plattformen, z. B. Azure bieten weit auseinander liegenden Regionen auf der ganzen Welt. Diese Plattformen bieten auch Funktionen, die Verfügbarkeit und einer Vielzahl von Wiederherstellungssituationen unterstützen. Nun kann jede Aufgabe kritische Cloud-Anwendung angegeben werden fällig Aspekte Disaster Prüfung des Systems durchführen. Azure hat Stabilität und Wiederherstellung in zahlreiche seiner Dienste integriert. Untersuchen Sie diese Plattformfeatures sorgfältig, und klicken Sie mit der Anwendung Strategien ergänzen.

Dieser Artikel Gliederungen die erforderlichen Architektur Schritte, die Sie zum Disaster-Nachweis Azure-Bereitstellung ausführen müssen. Sie können den größeren Continuity Geschäftsprozess implementieren. Geschäftsplan Continuity ist als Wegweiser für Sie Vorgänge unter Umständen unerwünschten fortfahren. Dies kann ein Fehler bei der Technologie, wie etwa einer stillgelegte Dienst- oder einer Naturkatastrophe, z. B. eines Sturms oder einem Dienstausfall sein. Anwendung Stabilität für Datenverluste ist nur einen Teil der größeren Wiederherstellung, wie in diesem Dokument NIST beschrieben: [Bedingung, von der Planungshandbuch for Information Technology Systems](https://www.fismacenter.com/sp800-34.pdf).

In den folgenden Abschnitten definieren verschiedene Detailebenen Fehlern, Techniken für den Umgang mit, und Architekturen, die diese Techniken unterstützen. Diese Informationen bietet eine Eingabe Ihrer Wiederherstellungsverfahren und Verfahren, um sicherzustellen, dass Ihre Wiederherstellen nach Datenverlusten ordnungsgemäß und effizient funktioniert.

##<a name="characteristics-of-resilient-cloud-applications"></a>Eigenschaften der robuste Cloudanwendungen

Eine gut entwickelte Anwendung Videofunktionen von Fehlern bei der taktischen Ebene aufnehmen kann, und sie können auch tolerieren strategische System organisationsweite Fehlern Ebene der Region. In den folgenden Abschnitten definieren die Terminologie im gesamten Dokument zu beschreiben verschiedene Aspekte des robuste Cloud Services verwiesen wird.

###<a name="high-availability"></a>Hohe Verfügbarkeit

Eine hoch verfügbare Cloudanwendung implementiert Strategien zum Aufnehmen der Ausfall der Abhängigkeiten an, wie die verwaltete Dienste der Cloud-Plattform. Trotz möglichen Fehlern bei der Cloud-Plattform Funktionen ermöglicht dieser Ansatz die Anwendung weiterhin die erwarteten funktionsübergreifendes und nicht funktionsfähige systematische Merkmale aufweisen. Dies wird in der Reihe Channel 9 video detaillierter behandelt [Failsafe: Leitfaden für robuste Cloudarchitekturen](https://channel9.msdn.com/Series/FailSafe).

Wenn Sie die Anwendung implementieren, müssen Sie die Wahrscheinlichkeit, dass ein Ausfall Videofunktionen berücksichtigen. Beachten Sie darüber hinaus den Einfluss, die, den ein Ausfall auf die Anwendung aus der Sicht Business vor dem Einstieg tief in der Implementierungsstrategien haben wird. Ohne Fälligkeitsdatum Aspekte Auswirkung auf das Unternehmen und die Wahrscheinlichkeit, dass die Bedingung Risiko betätigen, die Implementierung kann teure und potenziell unnötige sein.

Erwägen Sie eine automotive entsprechend hohen Verfügbarkeit. Auch Teile der Qualität und übergeordnete technisch verhindert gelegentliche Fehler nicht. Wenn Ihr Auto einem flachen Reifen erhält, beispielsweise das Auto weiter ausgeführt wird, aber sie arbeitet mit beeinträchtigt Funktionalität. Wenn Sie für diese möglicher Vorfall geplant, können Sie eine der diese dünne mit Rand freien Reifen bis zu einem Kopiershop reparieren. Zwar die Ersatzreifen hohe Geschwindigkeit nicht zulässt, können Sie das Fahrzeug, bis Sie die Tire ersetzen noch ausgeführt werden. Auf ähnliche Weise kann ein Clouddienst, der für potenziellen Verlust von Funktionen Pläne relativ unbedeutendes Problem verhindern Deaktivierung der gesamten Anwendungs. Dies gilt auch, wenn der Cloud-Dienst mit beeinträchtigt Funktionalität ausgeführt werden muss.

Es gibt einige Hauptmerkmale von hoch verfügbaren Cloud Services: Verfügbarkeit, Skalierbarkeit und Fehlertoleranz. Obwohl diese Merkmale miteinander verknüpft sind, ist es wichtig zu verstehen, jeder, und wie sie auf die allgemeine Verfügbarkeit der Lösung beitragen.

###<a name="availability"></a>Verfügbarkeit

Eine verfügbare Anwendung betrachtet die Verfügbarkeit von der zugrunde liegenden Infrastruktur und abhängige Dienste. Verfügbare Anwendungen entfernen Einzelpunktversagen über Redundanz und robuste Entwurf. Wenn Sie den Gültigkeitsbereich berücksichtigen Verfügbarkeit in Azure erweitern, ist es wichtig, des Konzepts der effektiven Verfügbarkeit der Plattform zu verstehen. Verfügbarkeit von effektiven betrachtet der Dienst Ebene Vereinbarungen SERVICELEVEL einzelnen abhängige Dienste und deren kumulative Auswirkung auf die Verfügbarkeit der gesamten System.

Verfügbarkeit des Systems ist das Maß für den Prozentsatz der einen Zeitrahmen fest, die, den das System ausgeführt werden können. Die Verfügbarkeit Vereinbarung zum SERVICELEVEL von mindestens zwei Instanzen von einer Website oder Arbeitskollegen Rolle in Azure beträgt beispielsweise 99,95 % (nicht 100 %). Es misst nicht die Leistung oder die Funktionalität der Dienste für diesen Rollen ausgeführt. Die effektive Verfügbarkeit von Ihrem Cloud-Dienst ist jedoch auch durch die verschiedenen SLAs der anderen abhängige Dienste betroffen. Die weitere gleitenden Teile innerhalb des Systems, kann die weitere Pflege, die Sie ausführen müssen, um sicherzustellen, dass die Anwendung bei der Verfügbarkeit von deren Endbenutzern erfüllen.

Erwägen Sie die folgenden SLAs für einen Azure-Dienst, die Azure Dienste verwendet: Datenverarbeitung, Azure SQL-Datenbank und Azure-Speicher.

|Azure service|VEREINBARUNG ZUM SERVICELEVEL   |Mögliche Minuten Ausfallzeit pro Monat (30 Tage)|
|:------------|:-----|:----------------------------------------:|
|Berechnen      |99,95 %|21,6 Minuten                              |
|SQL-Datenbank |99,99 %|4.3 Minuten                              |
|Speicher      |99,90 %|43,2 Minuten                              |

Sie müssen alle Dienste potenziell zu unterschiedlichen Zeiten wechseln nach unten planen. In diesem Beispiel vereinfachte ist die Gesamtzahl der Minuten pro Monat, die die Anwendung nach unten werden könnten 108 Minuten. Ein Monat 30 Tage hat insgesamt 43,200 Minuten. 108 Minuten ist.25 % der Gesamtzahl der Minuten in einem Monat 30 Tage (43,200 Minuten). Dadurch erhalten Sie eine effektive Verfügbarkeit von 99.75 % für den Clouddienst.

Jedoch kann mithilfe von Verfügbarkeit Verfahren in diesem Artikel beschriebenen folgt verbessern. Wenn Sie Ihrer Anwendung entwerfen weiterhin ausgeführt, wenn der SQL-Datenbank nicht verfügbar ist, können Sie, die beispielsweise aus der Formel entfernen. Dies bedeutet möglicherweise, dass die Anwendung mit Funktionen für reduzierte ausgeführt wird, damit es auch Business Anforderungen gibt zu berücksichtigen ist. Eine vollständige Liste der Azure SLAs finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

###<a name="scalability"></a>Skalierbarkeit

Skalierbarkeit wirkt sich direkt auf die Verfügbarkeit aus. Eine Anwendung, die höhere Auslastung schlägt fehl, ist nicht mehr verfügbar. Skalierbare Applikationen sind konsistente Ergebnisse, in die zulässigen Zeitfenster höhere Nachfrage zu können. Wenn ein System skalierbar ist, wird es skaliert horizontal oder vertikal Erhöhung laden und konsistente Performance dabei verwalten. Grundlegende ausgedrückt addiert horizontale Skalierung mehr Computer die gleiche Größe (Prozessor, Arbeitsspeicher und Bandbreite), während der vertikalen Skalierung erhöht sich die Größe der vorhandenen Computer. Für Azure müssen Sie die vertikale Skalierungsoptionen zum Auswählen verschiedener Computer Größe für berechnen. Ändern der Größe der Computer erfordert jedoch eine erneute Bereitstellung. Daher sind die am häufigsten flexiblen Lösungen für die horizontale Skalierung konzipiert. Dies gilt besonders für berechnen, da Sie einfach die Anzahl der Instanzen von einer bestimmten Rolle Web oder Arbeitskollegen laufenden erhöhen können. Diese zusätzlichen Instanzen höhere Verkehr über das Web Azure-Portal, PowerShell-Skripts oder Code zu behandeln. Als Grundlage für diese Entscheidung für die Erhöhung bestimmten überwachten Kennzahlen. In diesem Szenario Benutzer Leistung oder Kennzahlen ein auffällig ablegen Auslastung nicht beeinträchtigt. In der Regel speichern im Web und Worker Rollen alle Zustand extern. Dadurch wird für flexible Lastenausgleich und sicheres Behandlung von Änderungen an Instanz zählt. Horizontale Skalierung arbeitet auch Dienste, wie die Azure-Speicher, die keine gestufte Optionen für vertikale Skalierung bieten.

Cloud-Bereitstellungen sollte als eine Zusammenstellung von Mengen-Einheiten angezeigt werden. Dadurch wird die Anwendung in den Anforderungen Durchsatz Endbenutzer Wartung flexible sein. Die Maßeinheiten sind leichter zu visualisieren auf Web- und Server-Ebene. Dies liegt daran Azure bereits statusfreie Datenverarbeitungsknoten über Web- und Worker Rollen enthält. Hinzufügen, dass weitere Mengen-Einheiten für die Bereitstellung berechnen verursacht keine Anwendung Zustand Management Seite Effekte, da berechnen Maßeinheiten statusfrei sind. Ein Lagerplatz-Skala ist verantwortlich für die Verwaltung von einem Teil der Daten (strukturierten oder unstrukturierten). Beispiele für Speicher-Maßeinheiten sind Azure Tabellenpartition, Azure Blob Container und Azure SQL-Datenbank. Auch die Verwendung von mehreren Azure-Speicher Konten wirkt sich direkt auf die Anwendungsskalierbarkeit. Entwerfen Sie einen hochgradig skalierbare Cloud-Dienst mehrere Speicher-Maßeinheiten einfügen. Wenn eine Anwendung relationale Daten verwendet, Aufteilen der Daten z. B. über mehrere SQL-Datenbanken. Auf diese Weise können den mit dem flexible berechnen Mengen-Einheit Modell aufrechterhalten-Speicher. Auf ähnliche Weise ermöglicht Azure-Speicher Phishingversuchen, die absichtlich Designs auf der Ebene berechnen Durchsatz Anforderungen erfordern Aufteilen von Daten. Eine Liste der bewährte Methoden für das Entwerfen skalierbare Cloud Services finden Sie unter [Bewährte Methoden für den Entwurf von Scale Services für Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

###<a name="fault-tolerance"></a>Fehlertoleranz

Applikationen sollten davon ausgehen, dass jeder abhängige Cloud Videofunktionen kann und werden nach unten zu einem bestimmten Zeitpunkt Zeitpunkt geleitet. Eine gegeben Fehlerstrukturanalyse-Anwendung erkennt und Schachzüge, um Fehler beim Elemente, um fortzufahren und innerhalb eines bestimmten Zeitrahmens falsche Ergebnisse zurück. Vorübergehenden Fehler Bedingungen wird ein Vergleich Fehlerstrukturanalyse-System eine Richtlinie "Wiederholen" einsetzen. Die Anwendung kann für weitere schwerwiegenden Fehlern Probleme erkennen und nicht über einen alternativen Hardware oder Notfallplan Pläne bis zur Behebung des Fehlers. Einer zuverlässige Anwendung kann ordnungsgemäß verwalten den Fehler von einem oder mehreren Teilen und weiterhin ordnungsgemäß funktioniert. Fehlerstrukturanalyse gegeben Applikationen können Entwurfsstrategien eine oder mehrere, z. B. Redundanz, Replikation oder beeinträchtigt Funktionalität.

##<a name="disaster-recovery"></a>Wiederherstellung

Eine Cloudbereitstellung von möglicherweise nicht mehr aufgrund eines systematische abhängige Dienste oder der zugrunde liegenden Infrastruktur ausgeführt werden. Unter diesen Umständen löst Continuity Geschäftsplan der Wiederherstellung. Dieser Vorgang umfasst normalerweise sowohl Mitarbeiter und automatisierte Prozeduren akzeptieren, um die Anwendung in eine verfügbare Region reaktivieren möchten. Setzt die Übertragung von Benutzern, Daten und Dienste auf der neuen Region. Darüber hinaus muss die Verwendung von Sicherung Medien oder die Replikation.

Erwägen Sie die vorherigen entsprechend, die hohen Verfügbarkeit für die Möglichkeit zum Wiederherstellen eines aus einem flachen Reifen bestimmte ein Spare verglichen. Im Gegensatz dazu umfasst Wiederherstellung die Maßnahmen, die nach einem Absturz Auto, ist das Auto nicht mehr funktionsfähig. In diesem Fall ist die bestmögliche Lösung finden Sie eine effiziente Möglichkeit zum Autos, ändern, indem Sie einen Dienst Reisekosten oder einem Freund aufrufen. In diesem Szenario ist es wahrscheinlich eine längere Verzögerung in erste wieder unterwegs sein. Es gibt auch mehr Komplexität in reparieren und in der ursprünglichen Fahrzeug zurückgeben. Auf die gleiche Weise ist die Wiederherstellung in eine andere Region eine komplexe Aufgabe, die in der Regel einige Ausfallzeit und Datenverlust umfasst. Um besser zu verstehen und Auswerten von Strategien zur Wiederherstellung, es ist wichtig, zwei Begriffe definieren: Zeigen Sie Ziel-Wiederherstellung Zeit (RTO) und Wiederherstellung Ziel (RPO).

###<a name="recovery-time-objective"></a>Ziel der Wiederherstellung-Zeit

RTO ist die maximale Zeitspanne für Wiederherstellen Anwendungsfunktionalität vorgesehen sind. Dieser Wert basiert auf geschäftliche Anforderungen, und es wird im Zusammenhang mit der Wichtigkeit der Anwendung. Kritische branchenanwendungen erfordern eine niedrige RTO.

###<a name="recovery-point-objective"></a>Wiederherstellung Punkt Ziel

RPO ist der zulässigen Zeitfensters verloren gegangener Daten aufgrund des Wiederherstellungsvorgangs. Zum Beispiel ist das RPO eine Stunde, müssen Sie vollständig sichern oder repliziert die Daten mindestens einmal pro Stunde. Nachdem Sie die Anwendung in einer alternativen Region schalten, bis zu einer Stunde nach Daten fehlen möglicherweise die gesicherten Daten. Wie RTO-adressieren kritische Applikationen eine viel kleinere RPO.

##<a name="checklist"></a>Checkliste

Wir werden die wichtigsten Punkte, die in diesem Artikel (und seine verwandten Artikel auf [hohe Verfügbarkeit](./resiliency-high-availability-azure-applications.md) und [Wiederherstellung](./resiliency-disaster-recovery-azure-applications.md) für Azure Applications) behandelt wurde. Diese Zusammenfassung fungiert als Checkliste für Elemente, die Sie für Ihre eigenen Verfügbarkeit und Wiederherstellung Notfallplan berücksichtigen sollten. Hierbei handelt es sich um bewährte Methoden, die für die Benutzer, die zum Abrufen von Fakten zur Durchführung einer erfolgreichen Lösung wurden.

1. Durchführen einer risikobewertung für jede Anwendung, da jede andere systemvoraussetzungen haben, kann. Einige Applikationen sind wichtiger als andere und würde die zusätzlichen Kosten, um sie für die Wiederherstellung Entwerfen ausrichten.
1. Verwenden Sie diese Informationen, um die RTO und RPO für jede Anwendung definieren.
1. Entwurf für Fehler, beginnend mit der Anwendungsarchitektur.
1. Bewährte Methoden für eine hohe Verfügbarkeit aufzustellen Kosten, Komplexität und Risiken zu implementieren.
1. Implementieren Sie Notfallwiederherstellungspläne und Prozessen.
  * Erwägen Sie Fehlern, die die Modulebene ganz nach einem Ausfall abgeschlossen Cloud umfassen.
  * Strategien für alle Bezug und Transaktionen Daten hergestellt werden.
  * Wählen Sie eine mehrere Standortkatastrophen Wiederherstellung Architektur aus.
1. Definieren eines bestimmten Besitzers für Wiederherstellungsverfahren, Automatisierung und testen. Der Besitzer sollte verwalten und den gesamten Prozess besitzen.
1. Dokumentieren von den Prozessen, damit diese leicht wiederholt werden. Zwar ein Besitzer vorhanden ist, sollten mehrere Personen zu verstehen, und folgen Sie die Prozessen in dringenden können.
1. Schulen der Mitarbeiter, den Prozess anzuwenden.
1. Normale Disaster Simulationen für Schulung und Überprüfung des Prozesses verwendet.

##<a name="summary"></a>Zusammenfassung

Wenn oder Hardware in Azure fehl, unterscheiden sich die Techniken und Strategien für die Verwaltung von ihnen Fehler beim lokalen Betriebssystemen auftritt. Der wichtigste Grund dafür ist, dass Cloudlösungen enthalten normalerweise weitere Abhängigkeiten auf Infrastruktur, die auf einen Bereich Azure verteilt ist, und als separate Dienste verwaltet. Sie müssen die teilweise Fehlern mit hohen Verfügbarkeit Techniken behandelt. Verwenden Sie zum Verwalten von mehr Schwerwiegender Fehlers, möglicherweise durch ein Ereignis Disaster Strategien zur Wiederherstellung aus.

Azure erkennt und viele Fehler behandelt, aber es gibt viele Arten von Fehlern, die anwendungsspezifische Strategien erfordern. Sie müssen aktiv Vorbereitung und Verwalten der Fehler Applikationen, Dienste und Daten.

Berücksichtigen Sie Verfügbarkeit und Plan zur Wiederherstellung Ihrer Anwendungs zu erstellen, die Business Folgen einer der Anwendung. Definieren von Prozessen, Richtlinien und Verfahren zu wichtigen Systeme wiederhergestellt werden, nachdem ein Ereignis schwerwiegendes dauert, Planung und Zusicherung. Ein, und nachdem Sie die Pläne eingerichtet haben, können Sie es aufheben. Sie müssen regelmäßig analysieren, testen und verbessern ständig die Pläne basierend auf Ihrer Anwendungsportfolio, geschäftliche Erfordernisse und der Technologien zur Verfügung. Azure bietet neue Funktionen und löst das robuste Anwendungen, die auftretende Fehler verarbeiten erstellen neue Herausforderung aus.

##<a name="additional-resources"></a>Zusätzliche Ressourcen

[Hohe Verfügbarkeit bei Microsoft Azure](resiliency-high-availability-azure-applications.md)

[Wiederherstellung bei Microsoft Azure](resiliency-disaster-recovery-azure-applications.md)

[Technische Anleitung Azure Stabilität](resiliency-technical-guidance.md)

[Übersicht: Cloud Business Continuity und Datenbank Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md)

[Hohe Verfügbarkeit und Disaster Wiederherstellung für SQL Server in Azure virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[Failsafe: Leitfaden für robuste Cloudarchitekturen](https://channel9.msdn.com/Series/FailSafe)

[Bewährte Methoden für den Entwurf von umfangreichen Dienste auf Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe von Artikeln dienten Wiederherstellung und hohe Verfügbarkeit für Azure Applications. Im nächsten Artikel in dieser Reihe ist eine [hohe Verfügbarkeit bei Microsoft Azure](resiliency-high-availability-azure-applications.md).
