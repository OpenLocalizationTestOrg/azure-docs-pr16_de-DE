<properties 
   pageTitle="Häufig gestellte Fragen zur Azure SQL-Datenbank" 
   description="Antworten auf häufige Fragen Kunden Fragen nach dem Clouddatenbanken und Azure SQL-Datenbank, Microsoft relationalen Datenbank Management-System (RDBMS) und Datenbank als in der Cloud-Dienst." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>Häufig gestellte Fragen zu der SQL-Datenbank

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Wie angezeigt die Verwendung der SQL-Datenbank auf meiner Zahlung? 
SQL-Datenbank Rechnungen auf einen vorhersehbar Stundensatz basierend auf beide Dienstebene + Leistungsstufe für einzelne Datenbanken oder eDTUs pro Pool flexible Datenbank. Ist-Verwendung wird berechnet und habe stündlich, damit Ihre Rechnung möglicherweise Brüche einer Stunde anzeigen. Wenn Sie eine Datenbank für 12 Stunden in einem Monat vorhanden ist, zeigt beispielsweise Ihrer Rechnung Verwendung von 0,5 Tage. Darüber hinaus werden Dienst Ebenen + Leistungsstufe und eDTUs pro Pool aufgeteilt in der Rechnung, damit es einfacher ist, um die Anzahl der Tage der Datenbank anzuzeigen, die Sie für jede in einem einzelnen Monat verwendet.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Was passiert, wenn eine einzelne Datenbank weniger als einer Stunde ist aktiv oder verwendet eine höhere Service Stufe für weniger als einer Stunde?
Sie sind in Rechnung gestellt für jede Stunde, die eine Datenbank vorhanden ist, mit der höchsten Dienstebene + Leistungsstufe, die in dieser Stunde, unabhängig davon: Einsatz "oder" gibt an, ob die Datenbank für weniger als einer Stunde aktiv war angewendet. Wenn Sie eine einzelne Datenbank erstellen und löschen Sie ihn später fünf Minuten widerspiegeln Ihrer Rechnung beispielsweise eine Gebühr für eine Datenbank Stunde an. 

Beispiele
    
- Wenn Sie eine einfache Datenbank erstellen, und klicken Sie dann auf Standard S1 sofort aktualisieren, unterliegen Sie in der Standardansicht S1 Geschwindigkeit für die erste Stunde.

- Wenn Sie eine Datenbank von Basic auf Premium 10:00 Uhr aktualisieren und die Aktualisierung abgeschlossen ist, 1:35 Uhr Klicken Sie auf den folgenden Tag Sie mit einer 1:00 Uhr ab Premium Geschwindigkeit unterliegen 

- Wenn Sie eine Datenbank von Premium zum einfachen 11:00 Uhr downgrade 2:15 Uhr abgeschlossen ist, und dann die Datenbank wird in der Geschwindigkeit Premium bis 3:00 Uhr, nach dem ist es zu den grundlegenden Sätzen belastet belastet.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Wie werden flexible Datenbank Ressourcenpool Verwendung auf meiner Zahlung und was passiert bei einer Änderung eDTUs pro Pool?
Flexible Datenbank Ressourcenpool Gebühren werden auf Ihrer Rechnung als flexible DTUs (eDTUs) in den Schritten unter eDTUs pro Pool auf [der Seite Preisgestaltung](https://azure.microsoft.com/pricing/details/sql-database/)angezeigt. Es ist kostenlos pro Datenbank Pools flexible Datenbank ein. Sie sind für jede Stunde Abrechnung, die einem Ressourcenpool vorhanden ist, bei der höchsten eDTU, unabhängig davon: Einsatz oder ob der Pool für weniger als einer Stunde aktiv war. 

Beispiele

- Wenn Sie einem Standard flexible Datenbank Ressourcenpool mit 200 eDTUs 18 Uhr erstellen, Hinzufügen von fünf Datenbanken zum Pool Sie für 200 eDTUs für die gesamte Stunde, unterliegen beginnend in 11 Uhr durch den Rest des Tages.
- Tag 2, um 5:05 Uhr Datenbank 1 50 eDTUs Verarbeitung beginnt und bis zum Tag konstanter enthält. Datenbanken 2 bis 5 schwanken zwischen 0 und 80 eDTUs. Während des Tages fügen Sie fünf Datenbanken, die sich während des Tages mit wechselnder eDTUs nutzen. Tag 2 handelt es sich um eine vollständige Tag bei 200 eDTU in Rechnung gestellt. 
- Klicken Sie auf Tag 3, um 5 Uhr Sie fügen Sie ein anderes 15 Datenbanken hinzu. Datenbankverwendung erhöht sich während des Tages an die Stelle, wo Sie eDTUs für den Pool von 200 bis 400 8:05 Uhr vergrößern Gebühren Ebene der 200 eDTU wurden facto bis 20 Uhr und erhöht sich auf 400 eDTUs für die verbleibenden vier Stunden. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Wie angezeigt die Verwendung der aktiven Geo-Replikation in einer Datenbank flexible Ressourcenpool auf meiner Zahlung?
Im Gegensatz zu den einzelnen Datenbanken auswirken nicht [Aktiv Geo-Replikation](sql-database-geo-replication-overview.md) mit flexible Datenbanken mit einer direkten Abrechnung.  Sie unterliegen nur für die eDTUs nach der Bereitstellung für jede der Pools (Ressourcenpool primären und sekundären Ressourcenpool)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Wie wirkt sich die Verwendung von das Überwachungsfeature auf meiner Zahlung aus? 
Überwachung ist in der SQL-Datenbank-Dienst, ohne zusätzliche integriert Kosten und Basic, Standard und Premium Datenbanken verfügbar ist. Zum Speichern der Überwachungsprotokolle, basiert die ü Feature verwendet, die ein Konto Azure-Speicher und Sätzen für Tabellen und Warteschlangen in Azure-Speicher anwenden auf die Größe Ihrer Überwachungsprotokolls.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Wie finde ich die richtige Ebene und Leistung Dienstalter für einzelne Datenbanken und flexible Datenbank Pools? 
Es gibt ein paar Tools zur Verfügung. 

- Für lokale Datenbanken verwenden die [DTU Ziehpunkts Advisor](http://dtucalculator.azurewebsites.net/) , um die Datenbanken und DTUs erforderlich wird empfohlen und mehrere Datenbanken für flexible Datenbank Pools ausgewertet werden.
- Wenn Sie eine einzelne Datenbank, nicht in einem Ressourcenpool profitieren würden, empfiehlt intelligente des Azure-Engine ein Ressourcenpool flexible Datenbank, wenn sie ein Verwendungsmuster zurückliegenden sieht, die es rechtfertigt. Finden Sie unter [Überwachen und Verwalten eines Ressourcenpool flexible Datenbank mit dem Portal Azure](sql-database-elastic-pool-manage-portal.md). Weitere Informationen dazu, wie Sie die mathematische selbst durchführen, finden Sie unter [Preis und Leistung Aspekte für eine Datenbank flexible Ressourcenpool](sql-database-elastic-pool-guidance.md)
- Erkennen, ob Sie eine einzelne Datenbank nach oben oder unten wählen müssen, finden Sie unter [Leistung-Leitfaden für einzelne Datenbanken](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Wie oft kann ich die Dienstalter Stufe oder Leistung einer einzelnen Datenbank ändern? 
Mit V12 Datenbanken können Sie die Ebene Service (zwischen Basic, Standard und Premium) oder die Leistungsstufe innerhalb einer Ebene Service (z. B. S1 auf S2) ändern beliebig oft. Für Datenbanken aus früheren Versionen können Sie das Dienstalter Stufe oder Leistung insgesamt viermal in einem Zeitraum von 24 Stunden ändern.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Wie oft kann ich die eDTUs pro Pool anpassen? 
So oft wie gewünscht.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Wie lange dauert es so ändern das Dienstalter Stufe oder Leistung einer einzelnen Datenbank oder Verschieben einer Datenbank in und aus einer Datenbank flexible Ressourcenpool? 
Ändern der Dienstebene einer Datenbank und Verschieben von ein-und einem Ressourcenpool erfordert die Datenbank auf die Plattform als Hintergrundvorgang kopiert werden. Ändern der Dienstebene kann mehrere Stunden abhängig von der Größe der Datenbanken in wenigen Minuten dauern. In beiden Fällen bleiben die Datenbanken online und verfügbar während der verschieben. Weitere Informationen zum Ändern der einzelner Datenbanken finden Sie unter [Ändern der Dienstebene einer Datenbank](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Wann sollte ich eine Datenbank im Vergleich zu flexible Datenbanken werden verwendet? 
Flexible Datenbank Pools dienen im Allgemeinen für eine typische [Software als Service (SaaS) Anwendung Muster](sql-database-design-patterns-multi-tenancy-saas-applications.md), wobei es eine Datenbank pro Kunden oder den Mandanten gibt. Erwerben einzelne Datenbanken und Bereitstellung überproportional vieler, um die Variable entsprechen und bei Bedarf für jede Datenbank maximale ist häufig nicht effizient Kosten. Sie Verwalten der kollektiven Leistung von dem Pool Pools und die Datenbanken skalieren nach oben oder unten automatisch. 

Intelligente des Azure-Engine empfiehlt einem Ressourcenpool für Datenbanken, wenn ein Verwendungsmuster es rechtfertigt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise Ebene Empfehlungen](sql-database-service-tier-advisor.md). Ausführliche Anleitung auswählen zwischen den einzelnen und flexible Datenbanken finden Sie unter [Preis und Leistung Aspekte für flexible Datenbank Pools](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Was bedeutet es, bis zu 200 % der maximale bereitgestellte Datenbank-Speicher für zusätzliche Speicher haben? 
Zusätzliche Speicher ist die Speicherung zugeordnet Datenbanksicherungskopien automatisierte, die für [Point-In-Time--Wiederherstellung verwendet werden](sql-database-recovery-using-backups.md#-point-in-time-restore) und [Geo-wiederherstellen](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL-Datenbank bietet bis zu 200 % der maximalen bereitgestellte Datenbank Speicher Sicherung Speicher ohne zusätzliche Kosten. Wenn Sie eine Instanz Standard DB mit einer bereitgestellte DB Größe von 250 GB haben, werden Sie beispielsweise mit 500 GB zusätzliche Speicherkapazität kostenlos bereitgestellt. Wenn die Datenbank den bereitgestellten Sicherung Speicher überschreitet, können Sie zum Verringern des Aufbewahrungszeitraum durch Kontaktieren des Supports Azure oder für die zusätzliche Sicherung Speicherung Standardsatz Lesezugriff geografischen redundante Speicherung (RAS-GRS) in Rechnung gestellt bezahlen auswählen. Weitere Informationen zu RAS-GRS Abrechnung finden Sie unter Speicher Preise Details.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Muss ich bin Online-Business zu den neuen Dienst Ebenen Verschieben von was ich noch wissen?
Azure SQL-Web und Business Datenbanken sind jetzt Rente. Die Ebenen Basic, Standard, Premium und elastisch ersetzen die Abschaltung Web und Business-Datenbanken. Wir haben zusätzliche häufig gestellte Fragen, die Ihnen in diesem Übergangszeitraum helfen sollen. [Web und Business Edition Sonnenuntergangs häufig gestellte Fragen](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Was einer positiven erwarteten Replikation bei der Geo Replikation ist eine Datenbank zwischen zwei Bereiche innerhalb der gleichen Azure Geography?  
Wir sind derzeit unterstützt, einen RPO von fünf Sekunden und die Replikation positiven wurde kleiner ist als der bei der Geo-sekundären in der Azure gehostet wird für Region empfohlen und auf der gleichen Dienstebene.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Was ist eine erwarteten Replikation positiven Wenn Geo-sekundären in der gleichen Region wie die primäre Datenbank erstellt wird?  
Ausgehend von Summen-Daten, ist es nicht zu viel Unterschied zwischen innerhalb-Region und zwischen Region Replikation positiven Wenn die Azure empfohlen, dass für Region verwendet wird. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Wenn ein Fehler bei der zwischen zwei Bereiche, wie funktioniert die Logik "Wiederholen", wenn Geo-Replikation eingerichtet ist?  
Ist eine Trennung der Verbindung, wiederholen wir alle 10 Sekunden, um wieder herstellen von Verbindungen.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Was kann ich tun, damit sichergestellt ist, dass eine kritische Änderung in der primären Datenbank repliziert wird?
Die Geo-sekundäre ist einer asynchronen Replikation, und wir versuchen Sie nicht in der vollständigen Synchronisierung mit der primären behalten. Aber wir bieten eine Methode zum Erzwingen der Synchronisierung, um sicherzustellen, dass die Replikation von kritischen Änderungen (z. B. Kennwort Updates). Erzwungene Synchronisierung beeinträchtigt die Leistung, da den einen Thread blockiert, bis alle Transaktionen repliziert werden. Weitere Informationen finden Sie unter [Sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Welche Tools zum Überwachen der Replikation Verzögerung zwischen der primären Datenbank und Geo-sekundären erhältlich sind?
Wir verfügbar machen die Replikation in Echtzeit Verzögerung zwischen der primären Datenbank und Geo-sekundären durch eine DMV. Weitere Informationen finden Sie unter [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).
