<properties
    pageTitle="Azure SQL-Datenbank Benchmark (Übersicht)"
    description="In diesem Thema werden der Azure SQL-Datenbank Maßstab in messen die Leistung von Azure SQL-Datenbank verwendet werden."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Azure SQL-Datenbank Benchmark (Übersicht)

## <a name="overview"></a>(Übersicht)
Microsoft Azure SQL-Datenbank bietet drei [Service Ebenen](sql-database-service-tiers.md) mit mehreren Ebenen der Leistung. Jede Leistungsstufe bietet eine wachsende Sammlung von Ressourcen oder "Power" zunehmend höheren Durchsatz bietet.

Es ist wichtig, Quantifizierung, wie die zunehmende Leistungsfähigkeit von jeder Leistungsstufe in Datenbank höhere Leistung übersetzt werden kann. Zum Ausführen dieser Microsoft hat die Azure-SQL-Datenbank-Benchmark (ASDB) entwickelt. Der Maßstab führt eine Mischung Standardvorgänge in alle OLTP Auslastung gefunden. Wir messen des Durchsatzes erzielt für Datenbanken, die in jeder Leistungsstufe ausgeführt.

In Bezug auf die [Datenbank Transaktion Einheiten (DTUs)](sql-database-technical-overview.md#understand-dtus)werden die Ressourcen und Leistungsfähigkeit von jeder Dienstalter Ebene und Leistung ausgedrückt. DTUs bieten eine Möglichkeit, die relative Kapazität einer Leistungsstufe basierend auf einer angeglichene Maß für die CPU, Speicher, beschreiben und lesen und Schreiben Sätzen von jeder Leistungsstufe angeboten. Verdoppelt die Bewertung DTU einer Datenbank entspricht der Datenbank Power verdoppelt. Der Maßstab kann wir die Auswirkung auf die Datenbank Leistung der zunehmenden Leistung durch jeden Leistungsstufe von ist-Join-Datenbankoperationen, während Sie Datenbankgröße, Anzahl von Benutzern und Transaktion Sätzen proportional die Ressourcen bereitgestellt, um die Datenbank zu skalieren nutzen angeboten beurteilen.

Durch den Durchsatz der Standard-Service-Stufe Ausdrücken mithilfe von Transaktionen pro Stunde, der Standard-Service-Ebene mit Transaktionen pro Minute und die Premium Service Ebene mit Transaktionen pro Sekunde, um schnell die Leistung verknüpfen potenzieller für jede Dienstebene den Anforderungen der Anwendung erleichtert.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Abgleichen Benchmarkergebnisse realen Datenbank Leistung
Es ist wichtig zu verstehen, dass ASDB, wie alle Benchmarks, Vertreter und einen Hinweis nur ist. Die Transaktion Sätze mit der Anwendung erreicht werden nicht die übereinstimmen, die mit einer anderen Anwendung erreicht werden kann. Der Maßstab umfasst eine Zusammenstellung von anderen Transaktion, die mit einem Schema, die eine Reihe von Tabellen und Datentypen enthält Arten ausführen. Während der Maßstab der gleichen Standardvorgänge, die alle OLTP Auslastung feldbezogene ausführt, stellt es keine bestimmte Klasse Datenbank oder einer Anwendung dar. Ziel der Maßstab ist einen angemessenen Leitfaden für die relative Leistung einer Datenbank bereitstellen, die für die Skalierung nach oben oder unten zwischen Leistungsmerkmale erwartet werden könnte. Datenbanken tatsächlich unterschiedliche Größen und Komplexität sind, andere das Verhältnis von Auslastung auftreten und werden auf verschiedene Arten reagieren. Beispielsweise Anwendung EA ankommt möglicherweise früher EA Schwellenwerte erreicht oder eine Anwendung CPU-Auslastung möglicherweise früher CPU-Grenzwerte erreicht. Es gibt keine Garantie, die eine bestimmte Datenbank auf die gleiche Weise wie der Maßstab unter zunehmender laden Skalieren wird ein.

Der Maßstab und deren Methoden werden nachfolgend ausführlicher beschrieben.

## <a name="benchmark-summary"></a>Benchmark-Zusammenfassung
ASDB misst die Leistung von gemischte grundlegende Datenbankvorgänge, die am häufigsten in online Transaktion processing (OLTP) Auslastung auftreten. Obwohl der Maßstab mit Cloud computing im Meinung, Datenbankschema, Daten Population vorgesehen ist und Transaktionen entwickelt wurden, um die grundlegenden Elemente, die am häufigsten verwendeten in OLTP Auslastung gestreut angezeigte werden.

## <a name="schema"></a>Schema
Das Schema soll ausreichend Vielzahl Komplexität zur Unterstützung von einer Vielzahl von JOIN-Operationen verfügen. Der Maßstab führt für eine Datenbank besteht aus sechs Tabellen. Tabellen können in drei Kategorien: feste Größe, skalieren und wachsende. Es gibt zwei fester Größe Tabellen aus. drei Anpassungsbereich für Tabellen; und eine wachsende Tabelle. Tabellen mit fester Größe besitzen eine bestimmten Anzahl von Zeilen. Anpassungsbereich für Tabellen weisen eine Kardinalität, die proportional zur Datenbank-Performance ist, aber nicht während der Maßstab ändern. Wie eine Skalierung Tabelle auf die ursprüngliche laden, aber die Kardinalität ändert sich im Laufe der Maßstab ausgeführt, wie Zeilen eingefügt und gelöscht werden, wird die wachsende Tabelle angepasst.

Das Schema enthält eine Mischung aus Datentypen, ganze Zahl, einschließlich numerischen Zeichen und Datum/Uhrzeit. Das Schema umfasst Primär-und sekundären, aber keine Fremdschlüssel – d. h., es gibt keine Einschränkungen referenzielle Integrität zwischen Tabellen.

Daten der zweiten Generation Programm generiert die Daten für die ursprüngliche Datenbank. Ganze Zahl und numerischen Daten werden mit verschiedenen Strategien generiert. In einigen Fällen werden die Werte in einem Datumsbereich zufällig verteilt. In anderen Fällen wird eine Wertemenge zufällig zurück permutiert wurden um sicherzustellen, dass eine bestimmte Verteilung verwaltet wird. Textfelder sind aus einer gewichteten Liste von Wörtern, die realistische aussehende Daten erzeugt.

Die Datenbank wird angepasst basierend auf ein "" Skalierungsfaktor. Der Maßstab Faktor (Abkürzung als AE) bestimmt die Kardinalität von der Skalierung und wachsende Tabellen. Wie unten im Abschnitt beschrieben Benutzer und Pacing, die Größe der Datenbank, Anzahl der Benutzer und der maximalen Leistung alle proportional zueinander skalieren.

## <a name="transactions"></a>Transaktionen.
Die Arbeitsbelastung umfasst neun Transaktion Objekttypen, wie in der folgenden Tabelle dargestellt. Jede Transaktion soll eine bestimmte Gruppe von System Merkmale in der Datenbank-Engine und System Hardware mit hohem Kontrast von den anderen Transaktionen hervorheben. Dieser Ansatz vereinfacht die Auswirkung der verschiedenen Komponenten auf allgemeine Leistung beurteilen. Die Transaktion "Gelesen beanspruchen" ergibt beispielsweise eine größeren Anzahl von vom Datenträger gelesen Vorgänge.

| Art des Geschäfts | Beschreibung |
|---|---|
| Lese | WÄHLEN SIE AUS; in-Memory; schreibgeschützt |
| Medium lesen | WÄHLEN SIE AUS; hauptsächlich in-Memory; schreibgeschützt |
| Lesen fett | WÄHLEN SIE AUS; hauptsächlich nicht im Arbeitsspeicher; schreibgeschützt |
| Lite aktualisieren | AKTUALISIEREN; in-Memory; Lese-und Schreibzugriff |
| Aktualisieren von fett | AKTUALISIEREN; hauptsächlich nicht im Arbeitsspeicher; Lese-und Schreibzugriff |
| Lite einfügen | DIE UHRZEIT EINZUFÜGEN. in-Memory; Lese-und Schreibzugriff |
| Einfügen von fett | DIE UHRZEIT EINZUFÜGEN. hauptsächlich nicht im Arbeitsspeicher; Lese-und Schreibzugriff |
| Löschen | LÖSCHEN; Mischung aus im Arbeitsspeicher und nicht im Arbeitsspeicher; Lese-und Schreibzugriff |
| CPU-fett | WÄHLEN SIE AUS; in-Memory; relativ hoher CPU-Auslastung; schreibgeschützt |

## <a name="workload-mix"></a>Arbeitsbelastung Mischung
Transaktionen werden zufällig aus einer gewichteten Zufallsvariable mit den folgenden anbieten ausgewählt. Anbieten weist schreibgeschützt Verhältnis ungefähr 2:1 an.

| Art des Geschäfts | % der Mischung |
|---|---|
| Lese | 35 |
| Medium lesen | 20 |
| Lesen fett | 5 |
| Lite aktualisieren | 20 |
| Aktualisieren von fett | 3 |
| Lite einfügen | 3 |
| Einfügen von fett | 2 |
| Löschen | 2 |
| CPU-fett | 10 |

## <a name="users-and-pacing"></a>Benutzer und Geschwindigkeit anpassen
Die Arbeitsbelastung Benchmark wird durch ein Tool gesteuert, die Transaktionen über eine Reihe von Verbindungen, um das Verhalten einer Reihe von gleichzeitigen Benutzern simulieren übermittelt. Obwohl alle Verbindungen und Transaktionen Computer generiert werden können, finden Sie zur Vereinfachung wir diese Verbindungen als "Benutzer". Zwar jeden Benutzer unabhängig von allen anderen Benutzern arbeitet, führen alle Benutzer den gleichen Zyklus der nachfolgend aufgeführten Schritte aus:

1. Stellen Sie eine datenbankverbindung.
2. Wiederholen Sie bis signalisiert zu beenden:
    - Wählen Sie eine Transaktion zufällig (aus einer gewichteten Verteilung) ein.
    - Führen Sie die ausgewählte Transaktion und messen Sie die Antwortzeit.
    - Warten Sie eine Verzögerung Geschwindigkeitsangabe aus.
3. Schließen Sie die datenbankverbindung.
4. Beenden.

Die Geschwindigkeitsangabe Verzögerungszeit (in Schritt 2c) zufällig ausgewählt ist, jedoch mit einer Verteilung zurück, die einen Mittelwert der 1,0 Sekunden hat. Daher kann, jeden Benutzer im Durchschnitt pro Sekunde maximal eine Transaktion generieren.

## <a name="scaling-rules"></a>Anpassungsbereich für Regeln
Die Anzahl der Benutzer wird durch die Größe der Datenbank (in Maßstab zweifaktorielle Varianzanalyse Einheiten) bestimmt. Ein Benutzer für jede fünf Maßstab zweifaktorielle Varianzanalyse Einheiten ist vorhanden. Aufgrund der Geschwindigkeitsangabe Verzögerung kann ein Benutzer maximal eine Transaktion pro Sekunde, Durchschnitt generieren.

Beispielsweise ein Maßstab zweifaktorielle Varianzanalyse 500 (AE = 500) Datenbank 100 Benutzer haben und können eine maximale Zinsfuß 100 TPS erzielen. Um eine höhere TPS Laufwerk erfordert Zins mehr Benutzern und einer größeren Datenbank.

In der nachfolgenden Tabelle zeigt die Anzahl der Benutzer für jede Ebene und Leistung Dienstalter tatsächlich betreffen.

| Dienstebene (Leistungsstufe) | Benutzer | Datenbankgröße |
|---|---|---|
| Grundlegende | 5 | 720 MB |
| Standard (S0) | 10 | 1 GB |
| Standard (S1) | 20 | 2.1 GB |
| Standard (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6/P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Maßeinheiten Dauer
Eine gültige Richtlinie ausführen erfordert eine Dauer Fließgleichgewicht Maße mindestens eine Stunde an.

## <a name="metrics"></a>Kennzahlen
Die wichtigsten Kriterien in der Maßstab sind Durchsatz und die Antwort an.

- Durchsatz ist das grundlegende Leistungsmeasure in der Maßstab. Durchsatz wird in Transaktionen pro Einheit-von-Zeit, gezählt alle Typen von Transaktion gemeldet.
- Antwort ist ein Maß für die Leistung Vorhersagbarkeit. Die Antwort Zeit Einschränkung variiert mit Class Servicevertrag und höheren Klassen der Dienst eine weitere strengen Zeitlimit Antwort mit, wie unten dargestellt.

| Service Klasse  | Durchsatz Measure | Antwort Zeit Anforderung |
|---|---|---|
| Premium | Transaktionen pro Sekunde | 95-Perzentil bei 0,5 Sekunden |
| Standard | Transaktionen pro minute | 90-Perzentil bei 1,0 Sekunden |
| Grundlegende | Transaktionen pro Stunde | 80. Quantil bei 2.0 Sekunden |

## <a name="conclusion"></a>Abschluss
Der Azure SQL-Datenbank Maßstab misst die relative Leistung von Azure SQL-Datenbank ausgeführt wird, über den Bereich der verfügbaren Service Ebenen und Leistung Ebenen. Der Maßstab ausführt gemischte grundlegende Datenbankvorgänge, die am häufigsten in online Transaktion processing (OLTP) Auslastung auftreten. Durch messen die tatsächliche Leistung, bietet der Maßstab eine aussagekräftigere Bewertung den Einfluss auf Durchsatz, ändern Sie die Leistungsstufe als möglich ist, indem Sie einfach die Ressourcen bereitgestellt, um jede Ebene wie CPU-Geschwindigkeit, Speichergröße und IOPS auflisten. Wir werden weiterhin in der Zukunft weiterentwickelt der Maßstab zum Erweitern des Bereichs, und erweitern die Daten zur Verfügung gestellt.

## <a name="resources"></a>Ressourcen
[Einführung in SQL-Datenbank](sql-database-technical-overview.md)

[Dienst Ebenen und Leistung Ebenen](sql-database-service-tiers.md)

[Leitfaden zur Leistung für einzelne Datenbanken](sql-database-performance-guidance.md)
