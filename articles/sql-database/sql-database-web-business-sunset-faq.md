<properties
   pageTitle="Azure SQL-Datenbank Web und Business Edition Sonnenuntergangs häufig gestellte Fragen zu | Microsoft Azure"
   description="Erfahren Sie, wenn die Datenbanken Azure SQL-Web und Business gelöscht werden werden, und erfahren Sie über die Features und Funktionen der neuen Service leisten mehr."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Web und Business Edition Sonnenuntergangs häufig gestellte Fragen

Azure SQL-Web und Business Datenbanken sind jetzt Rente. Die Ebenen Basic, Standard, Premium und elastisch ersetzen die Abschaltung Web und Business-Datenbanken.

Um Unterstützung bei der Aktualisierung Web und Business-Datenbanken, empfiehlt die SQL-Datenbank eine geeignete Ebene und Leistung Dienstalter (Preisgestaltung Ebene) für jede Datenbank basierend auf deren zurückliegenden Arbeitsbelastung.

**Die Ebene Empfehlungen Gebühren erhalten:**

- [Upgrade auf SQL-Datenbank V12 über das Azure-Portal](sql-database-upgrade-server-portal.md)
- [Upgrade auf SQL-Datenbank V12 mithilfe der PowerShell](sql-database-upgrade-server-powershell.md)
- [Ändern der Preisgestaltung Ebene einer Web oder Business-Datenbank](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Warum wird das Azure-Portal meine Web und Business Edition-Datenbanken wie Rente dargestellt?

Da Web- und Business Edition-Datenbanken nicht verfügbar nach September 2015 werden, Etiketten im Portal Web und Business-Datenbanken wie Rente aus. Die deaktivierte Bezeichnung enthält auch eine Erinnerung, dass alle Web- und Business-Datenbanken auf Standard, Basic oder Premium aktualisiert werden sollen. Ausführliche Informationen zum Aktualisieren von vorhandener Web oder Business-Datenbanken auf den neuen Dienst Ebenen finden Sie unter [Upgrade auf Azure SQL-Datenbank V12](sql-database-upgrade-server-portal.md).

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Welche neuen Dienstebene ist die beste Option dar, meine vorhandene Web oder Business-Datenbank zu aktualisieren?

Auswählen einer geeigneten neuen Ebene und Leistung Dienstalter für Ihre vorhandene Datenbank für Web oder Business, hängt von den bestimmten Features und Leistung Anforderungen für eine Anwendung.

Verwenden Sie die Preisgestaltung Ebene Empfehlungen beschriebenen nach oben, oder detaillierte Informationen, um Sie bei der Auswahl eines entsprechenden neuen Dienst Ebenen unterstützen, finden Sie unter [Upgrade auf Azure SQL-Datenbank V12](sql-database-upgrade-server-portal.md).

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Warum ist Microsoft neue Dienst Ebenen Vorstellung?

Basierend auf Feedback von Kunden, ist Azure SQL-Datenbank neue Dienst Ebenen, damit Kunden Weitere relationale Datenbanken problemlos unterstützen können einführen. Die neuen Ebenen dienen für vorhersehbar Performance über ein ganzes Spektrum sieben Ebenen für leicht zu schwer Transaktionen Anwendung erfordert. Die neuen Ebenen bieten darüber hinaus einen Zellbereich Business Continuity-Funktionen, ein sicheres Verfügbarkeit-Vereinbarung zum SERVICELEVEL, größere der Datenbank für weniger Geld und eine höhere Abrechnung Qualität.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Wo kann weitere Informationen zu den neuen Dienst Ebenen mich?

Ausführliche Informationen zu den neuen Dienst Ebenen und Leistungsmodell finden Sie unter [Service Ebenen](sql-database-service-tiers.md). Detaillierte Informationen für den neuen Dienst Ebenen Preise, finden Sie unter [Preise SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Welche Features und Funktionen sind in Basic, Standard und Premium nicht verfügbar?

Das Feature Verbünden wird mit Web- und Business-Editionen deaktiviert werden. Kunden, die auf ihre Datenbanken Skalierung werden empfohlen, stattdessen verwenden oder Migrieren auf [flexible Datenbanktools](sql-database-elastic-scale-get-started.md) für [Azure SQL-Datenbank](sql-database-elastic-scale-get-started.md), die vereinfacht das Erstellen und Verwalten von einer Anwendung, die Sharding verwendet. Eine .NET Client-Bibliothek kann Anwendung definieren, wie Daten auf mehrere Shards hinweg und leitet OLTP Anfragen zu entsprechenden Datenbanken zugeordnet werden. Management-Vorgänge unterstützt, die neu konfigurieren, wie die Daten auf mehrere Shards hinweg verteilt ist, ist die Vorlage ein Azure-Cloud-Dienst enthalten, in Ihrem eigenen Azure-Abonnement gehostet werden kann. Zusätzlich zu den [flexible Datenbanktools](sql-database-elastic-scale-get-started.md)weiterhin Microsoft erstellen und Veröffentlichen von [benutzerdefinierten Sharding Muster und Methoden Anleitungen](https://msdn.microsoft.com/library/azure/dn764977.aspx) basierend auf Befunde von Aufträgen Tiefe Kunden. Neue Kunden, die Skalierung Funktionalität benötigen soll [flexible Datenbanktools](sql-database-elastic-scale-get-started.md) Auschecken und/oder wenden Sie sich an Microsoft Support, um ihre Optionen ausgewertet werden.

Microsoft wird auch die Datenbank kopieren Erfahrung mit Premium Datenbanken ändern. Zuvor als Premium Datenbank Kontingent beschränkt ist, wurde die erstellen Sie Datenbank... ALS eine Kopie in erstellt T-SQL eine unterbrochen Premium-Datenbank ohne reservierte Ressourcen, die mit derselben Geschwindigkeit als Business Datenbank in Rechnung gestellt wurde. Als Premium Kontingent jetzt stärkerem verfügbar ist, wird unterbrochen Premium nicht mehr unterstützt. Datenbankkopien werden nun mit dem gleichen Edition und Leistungsstufe als Quelle erstellt werden und werden in Rechnung gestellt werden entsprechend. Downgrade zu einer anderen Dienst Stufe oder Leistung Ebene, deren Kosten zu verringern, falls gewünscht kopierte Datenbanken können Kunden wählen. Vorhandene unterbrochen Premium Datenbanken werden in Business Edition als Teil dieser Version konvertiert. Es kann davon ausgehen, dass die Einführung von [Point-In-Time wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore) müssen Sicherungskopien der Datenbanken verringern wird.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Wie verbessert Basic, Standard und Premium meiner Abrechnung Erfahrung?

Grundlegende, Standard, Premium Azure SQL-Datenbanken pro Stunde in Rechnung gestellt werden, und Sie haben die Möglichkeit, jede Datenbank nach oben oder unten skalieren. Sie sind Abrechnung, um eine feste Rate basierend auf der höchsten Ebene und Leistung Dienstalter für jede Stunde ausgewählt haben. Darüber hinaus Leistungsmerkmale (Beispiel: Basic, S1 und P2) in der Rechnung, damit es einfacher ist, um die Anzahl der Datenbank Tage/Stunden sehen Sie in einem einzelnen Monat für jede Ebene Leistung tatsächlich Spalten aufgeführt. Web und Business Datenbanken weiterhin in Rechnung gestellt werden mithilfe der Datenbank Einheiten basierend auf die Größe der Datenbank. Besuchen Sie die [SQL-Datenbank Preise Seite](https://azure.microsoft.com/pricing/details/sql-database/) erfahren Sie mehr über die Preise und die Unterschiede zwischen den neuen Dienst Ebenen.


## <a name="see-also"></a>Siehe auch

[SQL Azure-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)

[Web und Preise Business](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Dienst Ebenen](sql-database-service-tiers.md)

[Upgrade auf SQL Azure-Datenbank V12](sql-database-upgrade-server-portal.md)
