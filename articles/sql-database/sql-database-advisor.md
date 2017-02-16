<properties 
   pageTitle="Advisor für SQL Azure-Datenbank" 
   description="Der Azure SQL-Datenbank Advisor Leitfaden für Ihre vorhandene SQL-Datenbanken, die aktuelle abfrageleistung verbessert werden kann." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor"></a>SQL-Datenbank Advisor

> [AZURE.SELECTOR]
- [SQL-Datenbank Advisor (Übersicht)](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Azure SQL-Datenbank lernfähig und passt sich mit Ihrer Anwendung und Leitfaden angepasste es Ihnen ermöglicht, die Leistung von Ihrer SQL-Datenbanken maximieren. Der SQL-Datenbank Advisor Leitfaden für erstellen und Indizes ablegen, parametrisieren Abfragen und Behebung von Problemen Schema an. Der Advisor bewertet Leistung durch die Verwendung Verlauf Ihrer SQL-Datenbank zu analysieren. Die Empfehlungen, die zum Ausführen Ihrer Datenbank normalen Arbeiten am besten geeignet sind, werden empfohlen. 

Folgende Aspekte stehen für V12 Servers (Empfehlungen sind nicht verfügbar für V11 Server). Zurzeit können Sie festlegen, automatisch angewendet werden, finden Sie unter [Automatische Index Management](sql-database-advisor-portal.md#enable-automatic-index-management) Details Ratschläge Index erstellen und ablegen.

## <a name="create-index-recommendations"></a>Erstellen von Index Empfehlungen 

**Create Index** Empfehlungen angezeigt werden, wenn der SQL-Datenbank-Dienst von fehlenden Indizes erkennt, die Ihrer Datenbanken Arbeitsbelastung (nur nicht gruppierte Indizes) von Wenn erstellt haben, nutzen können.

## <a name="drop-index-recommendations"></a>Legen Sie Index Empfehlungen

**Drop Index** Empfehlungen angezeigt werden, wenn der SQL-Datenbank-Dienst doppelte Indizes erkennt (in der Vorschau und gilt nur für doppelte Indizes).

## <a name="parameterize-queries-recommendations"></a>Parametrisieren Abfragen Empfehlungen

**Parameterize Abfragen** Empfehlungen angezeigt werden, wenn Sie eine oder mehrere Abfragen verfügen, die ständig aber Ende von mit der gleichen Abfrageausführungsplan erneut kompiliert werden. Diese Bedingung öffnet eine Verkaufschance anzuwendende erzwungenen Parametrisierung, wodurch können Abfrage-Pläne zwischengespeichert und zukünftigen Verbessern der Leistung und Ressource: Einsatz verringern wiederverwendet werden soll. 

Jeder Abfrage, die ursprünglich für SQL Server ausgestellt muss um einen Plan für die Ausführung generieren kompiliert werden. Jeden generierten Plan hinzugefügt, die den Plan zwischengespeichert und nachfolgende Ausführungen derselben Abfrage diesem Plan aus dem Cache, zusätzliche Kompilierung überflüssig wiederverwenden können. 

Anwendungen, die Senden von Abfragen, die nicht parametrisierte Werte enthalten, können zu einem Verwaltungsaufwand Leistung führen, wird für jede solche Abfrage mit verschiedenen Parameterwerte Plan für die Ausführung erneut kompiliert. In vielen Fällen dieselben Abfragen mit anderen Parameter Werte generieren, die die gleichen Ausführungspläne, aber diese Pläne weiterhin separat dem Plan Cache hinzugefügt werden. Kompilieren Ausführungspläne Datenbankressourcen verwenden, vergrößern die Abfrage unter Dauer angegebene Zeit und Überlauf Plancache bewirken, dass Pläne, die aus dem Cache entfernt werden. Dieses Verhalten von SQL Server kann geändert werden, indem Sie die Option erzwungenen Parametrisierung in der Datenbank festlegen. 

Damit Sie den Einfluss der diese Empfehlungen schätzen können, sind Sie mit einen Vergleich zwischen den tatsächlichen CPU-Auslastung und der geplanten CPU-Auslastung bereitgestellt, (als wäre empfohlen angewendet wurde). Zusätzlich CPU Spareinlagen verkleinert Ihrer Abfragedauer für den Zeitaufwand in eine Kompilierung aus. Außerdem werden viel weniger Aufwand Plan Cache, gleicht Mehrzahl der Pläne im Cache bleiben und wiederverwendet werden. Sie können diese Empfehlungen schnell und einfach, indem Sie auf den Befehl **Übernehmen** anwenden. 

Nachdem Sie diese Empfehlungen anwenden, wird es erzwungenen Parametrisierung in Minuten auf Ihre Datenbank aktivieren, und den überwachen Prozess ungefähr 24 Stunden dauert beginnt. Nach diesem Zeitraum werden Sie möglicherweise den Überprüfung-Bericht anzeigen, der CPU-Auslastung des Ihrer Datenbank zeigt 24 Stunden vor und nach der Anwendung empfohlen. SQL-Datenbank Advisor verfügt über ein Sicherheit Verfahren, die automatisch angewendete empfohlen wird zurückgesetzt, falls eine Regression Leistung erkannt wurde.

## <a name="fix-schema-issues-recommendations"></a>Empfehlungen im Zusammenhang mit Schema Probleme beheben

**Beheben von Problemen mit Schema** Empfehlungen angezeigt werden, wenn der SQL-Datenbank-Dienst eine Abweichung im die Anzahl der Fehler im Zusammenhang mit dem Schema SQL weiterhin auf Ihre SQL Azure-Datenbank bemerkt. Diese Empfehlungen wird in der Regel angezeigt, wenn Ihre Datenbank innerhalb einer Stunde nach mehreren Schema-bezogene Fehler (Ungültiger Spaltenname, Ungültiger Objektname usw.) findet.

"Schema Probleme" sind eine Klasse Syntaxfehler in SQL Server, die ausgeführt werden, wenn die Definition der SQL-Abfrage und der Definition das Datenbankschema nicht ausgerichtet werden. Beispielsweise ist eine der Spalten, die von der Abfrage erwartet möglicherweise fehlt in der Zieltabelle (oder umgekehrt). 

Empfehlungen "Schema Problem beheben" angezeigt wird, wenn Azure SQL-Datenbank-Dienst eine Abweichung im die Anzahl der Fehler im Zusammenhang mit dem Schema SQL weiterhin auf Ihre SQL Azure-Datenbank bemerkt. Die folgende Tabelle zeigt die Fehlern, die auf Schema Probleme verwandt sind:

|SQL-Fehlercode|Nachricht|
|--------------|-------|
|201|Prozedur oder Funktion '*'erwartet Parameter'*', der nicht angegeben wurde.|
|207|Ungültiger Spaltenname ' *'.|
|208|Ungültiger Objektname ' *'. |
|213|Spaltenname oder Anzahl der bereitgestellten Werte entspricht nicht der Definition der Tabelle. |
|2812|Gespeicherte Prozedur nicht gefunden "*". |
|8144|Prozedur oder Funktion * zu viele Argumente angegeben wurde. |

## <a name="next-steps"></a>Nächste Schritte

Überwachen Sie Ihre Empfehlungen und weiterhin anwenden, um die Leistung zu optimieren. Datenbanken sind dynamisch und kontinuierlich ändern. SQL-Datenbank Advisor weiterhin überwachen und Vorschläge, wie, die potenziell Ihrer Datenbank Leistung verbessern können. 

 - Schritte zum Verwenden von SQL-Datenbank Advisor Azure-Portal finden Sie unter [SQL-Datenbank Advisor Azure-Portal](sql-database-advisor-portal.md) .
 - Finden Sie unter [Abfrage Leistung Einsichten](sql-database-query-performance.md) zu lernen und der Leistung durch Ihre erste Abfragen anzeigen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Abfrage-Store](https://msdn.microsoft.com/library/dn817826.aspx)
- [INDEX ERSTELLEN](https://msdn.microsoft.com/library/ms188783.aspx)
- [Rollenbasierte Access-Steuerelement](../active-directory/role-based-access-control-configure.md)



