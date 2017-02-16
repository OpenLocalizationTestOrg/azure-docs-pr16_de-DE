<properties
   pageTitle="Verwalten von berechnen Power in Azure SQL-Data Warehouse (Übersicht) | Microsoft Azure"
   description="Leistung Skalierung-Funktionen in Azure SQL-Data Warehouse. Skalieren Sie indem Sie DWUs anpassen oder Anhalten Sie und fortsetzen Sie berechnen Ressourcen zum Speichern von Kosten."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Verwalten von berechnen Power in Azure SQL-Data Warehouse (Übersicht)

> [AZURE.SELECTOR]
- [(Übersicht)](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

Die Architektur von SQL Data Warehouse trennt die Speicherung und berechnen, sodass jeder dieser unabhängig voneinander zu skalieren. Daher können Sie die Leistung beim Speichern der Kosten nur bei Bedarf bezahlen Leistung skalieren. 

In dieser Übersicht werden die folgenden Funktionen der Leistung Skalierung des SQL-Data Warehouse und bietet Empfehlungen erfahren Sie, wie und wann sie verwendet. 

- Maßstab Power berechnen, indem Sie anpassen [Data warehouse-Einheiten (DWUs)][]
- Anhalten Sie oder fortsetzen Sie berechnen Ressourcen

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Maßstab Leistung

In SQL Data Warehouse können Sie schnell die Leistung, oder indem Sie wieder steigenden oder abnehmenden berechnen Ressourcen CPU, Arbeitsspeicher und e/a-Bandbreite skalieren. Um die Leistung zu skalieren, Sie müssen, lediglich passen Sie die Anzahl der [Data warehouse-Einheiten (DWUs)][] , die die Datenbank SQL Data Warehouse zuweist. SQL Data Warehouse schnell nimmt die Änderung und die zugrunde liegenden Änderungen an der Hardware oder Software behandelt.

Nicht mehr angezeigt werden die Tage an, in welcher der Prozessoren Recherchieren werden müssen, wie viel Speicher oder welche Art von Speicher, muss hohe Leistung in Ihr Datawarehouse. Ihr Data Warehouse in der Cloud, einfügen, müssen Sie nicht mehr Low Hardwareprobleme behandelt. In diesem Fall SQL Data Warehouse fragt Sie diese Frage: wie schnell möchten Sie Ihre Daten analysieren? 

### <a name="how-do-i-scale-performance"></a>Wie skalieren ich Leistung?

Elastisch vergrößern oder Verkleinern der Power berechnen, ändern Sie einfach die Einstellung [Data warehouse-Einheiten (DWUs)][] für die Datenbank ein. Während Sie weitere DWU hinzufügen, wird die Leistung linear erhöhen.  Höherer DWU müssen Sie mehr als 100 DWUs um eine signifikante Verbesserung der Leistung zu beachten hinzufügen. Damit Sie aussagekräftige Sprünge DWUs wählen können, bieten wir DWU Ebenen, die die besten Ergebnisse erzielen.
 
Um DWUs anzupassen, können Sie eine der folgenden Methoden einzelnen verwenden.

- [Maßstab berechnen Power mit Azure-portal][]
- [Maßstab berechnen Power mit PowerShell][]
- [Maßstab berechnen Power mit REST-APIs][]
- [Maßstab berechnen Power mit TSQL][]

### <a name="how-many-dwus-should-i-use"></a>Wie viele DWUs sollte ich verwenden?
 
Leistung in SQL Data Warehouse linear skaliert und Ändern von einer berechnen Maßstab in ein anderes (sagen von 100 DWUs zu DWUs 2000) geschieht in Sekunden. Dadurch werden die Flexibilität so experimentieren Sie mit verschiedenen Einstellungen der DWU, bis Sie Ihrem Szenario die optimale Breite bestimmen.

Zu verstehen, was Ihre ideale DWU Wert ist, versuchen Sie nach oben oder unten skalieren und einige Abfragen nach dem Laden der Daten ausgeführt. Da Skalierung Symbolleiste ist, können Sie versuchen, eine Anzahl von verschiedenen Ebenen der Leistung in eine Stunde oder weniger. Gehen Sie wie folgt Bedenken Sie, die SQL Data Warehouse große Datenmengen verarbeiten und deren true-Funktionen für die Skalierung finden Sie unter ausgelegt ist, besonders bei der größere skaliert, die wir anbieten, wird eine große Datenmenge verwenden möchten erreicht oder überschreitet 1 TB.

Empfehlungen für die beste DWU für Ihre Arbeitsbelastung suchen:

1. Beginnen Sie für ein Datawarehouse in Entwicklung, indem Sie eine kleine Anzahl von DWUs auswählen.  Ein guter Ausgangspunkt ist DW400 oder DW200.
2. Überwachen Sie die Leistung Ihrer Anwendung, die Anzahl der ausgewählten DWUs im Vergleich zu die Leistung, die Sie beobachten beobachten.
3. Bestimmen Sie, wie viel Leistung beschleunigen oder verlangsamen möchten Sie die optimale Leistung Ebene für Ihren Anforderungen Voraussetzung linearen Maßstab zu erreichen sein soll.
4. Vergrößern Sie oder verkleinern Sie die Anzahl der DWUs im Verhältnis zur wie viel schneller oder langsamer Ihrer Arbeitsbelastung ausführen möchten. Der Dienst wird schnell Antworten und Anpassen der Ressourcen berechnen, um die neue DWU erfüllen.
5. Fahren Sie Anpassungen vornehmen, bis Sie eine optimale Leistungsstufe für Ihre Bedürfnisse erreicht haben.

### <a name="when-should-i-scale-dwus"></a>Wann sollte ich DWUs skalieren?

Wenn Sie schneller Ergebnisse erforderlich ist, erhöhen Sie Ihre DWUs und größere Leistung bezahlen.  Wenn Sie müssen weniger Power zu berechnen, Ihrer DWUs verkleinern und bezahlen nur für die gewünschten Informationen. 

Empfehlungen für den Einsatz von DWUs skalieren:

1. Anzupassen Sie Spitzen und niedrigen Punkte, wenn Ihrer Anwendung enthält eine veränderlichen Arbeitsbelastung, skalieren DWU nach oben Ebenen oder unten. Beispielsweise, wenn Ihre Arbeitsbelastung in der Regel am Ende des Monats größten ist, Planen Sie weitere DWUs hinzufügen, während diese Höchstwert Tage, und dann abwärts skalieren, nachdem über der Zeitraum Höchstwert befindet.
2. Bevor Sie einen großer Datenmengen laden oder eine Transformation Vorgang durchführen, skalieren Sie DWUs, damit die Daten schneller verfügbar ist.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Anhalten berechnen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Um eine Datenbank anzuhalten, verwenden Sie eine der folgenden Methoden einzelnen.

- [Anhalten berechnen mit Azure-portal][]
- [Anhalten berechnen mit PowerShell][]
- [Anhalten berechnen mit REST-APIs][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Berechnen des Lebenslaufs

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Um eine Datenbank fortzusetzen, verwenden Sie eine der folgenden Methoden einzelnen.

- [Berechnen der Lebenslauf mit Azure-portal][]
- [Berechnen der Lebenslauf mit PowerShell][]
- [Berechnen der Lebenslauf mit REST-APIs][]

## <a name="permissions"></a>Berechtigungen

Skalierung der Datenbank wird in [ALTER DATABASE][]beschriebenen Berechtigungen erforderlich.  Anhalten und fortsetzen benötigen die Berechtigung [SQL DB Mitwirkender][] , insbesondere Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Finden Sie in den folgenden Artikeln, die Ihnen einige zusätzliche Key Performance Konzepte verstehen erleichtern:

- [Arbeitsbelastung und Parallelität management][]
- [Tabelle Entwurf (Übersicht)][]
- [Tabelle Verteilung][]
- [Tabelle Indizierung][]
- [Tabellenpartitionierung][]
- [Tabellenstatistiken][]
- [Bewährte Methoden][]

<!--Image reference-->

<!--Article references-->
[Data warehouse-Einheiten (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Maßstab berechnen Power mit Azure-portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Maßstab berechnen Power mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Maßstab berechnen Power mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Maßstab berechnen Power mit TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Anhalten berechnen mit Azure-portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Anhalten berechnen mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Anhalten berechnen mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Berechnen der Lebenslauf mit Azure-portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Berechnen der Lebenslauf mit PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Berechnen der Lebenslauf mit REST-APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Arbeitsbelastung und Parallelität management]: ./sql-data-warehouse-develop-concurrency.md
[Tabelle Entwurf (Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Tabelle Verteilung]: ./sql-data-warehouse-tables-distribute.md
[Tabelle Indizierung]: ./sql-data-warehouse-tables-index.md
[Tabellenpartitionierung]: ./sql-data-warehouse-tables-partition.md
[Tabellenstatistiken]: ./sql-data-warehouse-tables-statistics.md
[Bewährte Methoden]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL-DB Mitwirkender]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ÄNDERN DER DATENBANK]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
