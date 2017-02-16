<properties
    pageTitle="SQL-Datenbank Leistung und Optionen: Service Ebenen | Microsoft Azure"
    description="Vergleichen Sie die SQL-Datenbank Leistung und Business Continuity-Funktionen der Dienst leisten, Kosten und Videofunktionen abzuwägen beim Skalieren."
    keywords="Datenbankoptionen, die Leistung der Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-Datenbank-Optionen und Leistung: verstehen, was in jeder Kategorie Dienst verfügbar ist.

[Azure SQL-Datenbank](sql-database-technical-overview.md) bietet drei Service leisten mit mehreren Ebenen von Leistung, anderen Auslastung zu behandeln. Jede Leistungsstufe bietet eine wachsende Sammlung von Ressourcen bieten zunehmend höheren Durchsatz. Sie können jede Datenbank in einem eigenen [Dienstebene](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) mit einem eigenen Leistungsstufe verwalten. Sie können auch mehrere Datenbanken in einem [flexible Ressourcenpool](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) mit einer freigegebenen Gruppe von Ressourcen verwalten. Die verfügbaren Ressourcen für eigenständigen Datenbanken werden in Bezug auf die Datenbank Transaktion Einheiten (DTUs) und für flexible Pools im Hinblick auf flexible DTUs oder eDTUs ausgedrückt. Weitere Informationen zum DTUs und eDTUs finden Sie unter [Was ist ein DTU](sql-database-what-is-a-dtu.md). 

Beziehen Sie in beiden Fällen die Ebenen Dienst **grundlegende**, **Standard**und **Premium**. Datenbankoptionen in diesen Stufen sind für eigenständigen Datenbanken und flexible Pools ähnlich, es gibt jedoch einige zusätzliche Aspekte für flexible Pools. Dieser Artikel enthält die Details der Dienst leisten für eigenständigen Datenbanken und flexible Pools.

## <a name="service-tiers-and-database-options"></a>Dienst Ebenen und Datenbankoptionen
Grundlegende, Standard, und Premium Service Ebenen alle haben eine Verfügbarkeit Vereinbarung zum SERVICELEVEL von 99,99 % und bieten vorhersehbar Leistung, flexible Business Continuity-Optionen, Sicherheitsfeatures und stündlich Abrechnung. Die folgende Tabelle enthält Beispiele die beste Ebenen für verschiedene Auslastung geeignet ist.

| Dienstebene | Ziel Auslastung |
|---|---|
| **Grundlegende** | Am besten geeignet für eine kleine Datenbank, in der Regel einer einzelnen aktiven Operation zu einem bestimmten Zeitpunkt unterstützen. Beispiele für die Entwicklung oder die Prüfung verwendete Datenbanken oder kleine Applikationen selten verwendet. |
| **Standard** | Die Option Gehe zu an die meisten Cloudanwendungen unterstützt mehrere gleichzeitige Abfragen. Beispiele für Arbeitsgruppe oder Web Applications. |
| **Premium** | Speziell für hohe Transaktionen Volumen, viele gleichzeitige Benutzer unterstützt wird und der höchsten Ebene Business Continuity-Funktionen. Beispiele für sind Datenbanken, die für umwandeln. |

>[AZURE.NOTE] Web und Business-Editionen Rente sind. Lesen Sie den [Sonnenuntergang häufig gestellte Fragen](https://azure.microsoft.com/pricing/details/sql-database/web-business/) , wenn Sie weiterhin Web und Business-Editionen verwenden möchten.

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Eigenständige Datenbank Dienst Ebenen und Leistung Ebenen
Für eigenständigen Datenbanken gibt es mehrere Performance innerhalb jeder Kategorie Dienst. Sie haben die Flexibilität, wählen Sie die Ebene, die Ihre Arbeitsbelastung des erfordert am besten entspricht. Wenn Sie nach oben oder unten skalieren müssen, können Sie einfach die Ebenen der Datenbank ändern. Details finden Sie unter [Ändern der Datenbank Service-Ebenen und Leistung Ebenen](sql-database-scale-up.md) .

Hier aufgeführten Leistungsmerkmale gelten für Datenbanken mit [SQL-Datenbank V12](sql-database-v12-whats-new.md)erstellt. Unabhängig von der Anzahl der Datenbanken gehostet die Datenbank erhält eine gesicherte Reihe von Ressourcen und die erwarteten Leistungsmerkmale der Datenbank sind hiervon nicht betroffen.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Eine ausführliche Erläuterung von: alle anderen Zeilen in dieser Tabelle der Dienst Ebenen finden Sie unter [Service-Funktionen, die Ebene und Grenzwerte](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Flexible Ressourcenpool Dienst Ebenen und Leistung in eDTUs
Zusätzlich zu erstellen und dieselbe Skalierung einer eigenständigen Datenbank, müssen Sie auch die Möglichkeit, mehrere Datenbanken innerhalb eines [flexible Ressourcenpool](sql-database-elastic-pool.md)verwalten. Alle Datenbanken in einer flexible Ressourcenpool verfügen über gemeinsame Ressourcen. Die Leistungsmerkmale werden durch *Flexible Datenbank Transaktion Einheiten* (eDTUs) gemessen. Während der eigenständigen Datenbanken Pools in drei Service leisten im Lieferumfang: **grundlegende**, **Standard-**und **Premium**. Für Pools definieren diese drei Dienst Ebenen weiterhin die Gesamtsystemleistung Grenzwerte und verschiedene Funktionen.

Pools zulassen Datenbanken zum Freigeben und Nutzen von Ressourcen DTU ohne jede Datenbank im Pool eines bestimmten Leistungsstufe zuweisen. Beispielsweise kann eine eigenständigen Datenbank in einem Standard-Pool mit 0 eDTUs, um die maximale Datenbank eDTU, die eingerichtet werden, wenn Sie den Pool konfigurieren wechseln. Pools können mehrere Datenbanken mit unterschiedlichen Auslastung eDTU verfügbaren Ressourcen effizient an den gesamten Pool verwenden. Details finden Sie unter [Preis und Leistung Aspekte für eine flexible Ressourcenpool](sql-database-elastic-pool-guidance.md) .

Die folgende Tabelle beschreibt die Merkmale der Ressourcenpool Service leisten.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Jede Datenbank in einem Ressourcenpool hält auch die eigenständigen Datenbank Merkmale für die Ebene. Angenommen, die grundlegende Ressourcenpool sind maximal für max Sitzungen pro Pool von 4800-28800, jedoch eine einzelne Datenbank innerhalb einer einfachen Ressourcenpool sind Datenbank maximal 300 Sitzungen.

## <a name="choosing-a-service-tier"></a>Eine Stufe Dienst auswählen

Um auf einer Ebene Service entscheiden, zunächst bestimmen, ob die Datenbank werden von einem eigenständigen Datenbank oder eine flexible Ressourcenpool gehört, werden soll. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Auswählen einer Dienstebene für eine Datenbank eigenständigen

Um eine Dienstebene für eine Datenbank eigenständigen festgelegt haben, starten Sie bestimmen den Features, die Sie benötigen, Ihre SQL-Datenbank-Edition auswählen:

- Datenbankgröße (für für Standard grundlegende, bis zu 250 GB maximal 2 GB und 500 GB zu für Premium – je nach der Leistungsstufe maximal 1 TB)
- Datenbank Sicherungskopie Aufbewahrungszeitraum (7 Tage für Basic 35 Tagen für Standard und 35 Tagen für Premium)

Wenn Sie die SQL-Datenbank-Edition festgestellt haben, sind Sie bereit, um die Leistungsstufe für die Datenbank (die Anzahl der DTUs) zu bestimmen. Sie können abzuschätzen und [Skalieren nach oben oder nach unten dynamisch](sql-database-scale-up.md) basierend auf den tatsächlichen Erfahrung. Die [DTU Rechner](http://dtucalculator.azurewebsites.net/) können Sie auch die Anzahl der DTUs erforderlich ungefähre. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Auswählen einer Dienstebene für eine Datenbank flexible Ressourcenpool an.

Um auf der Dienstebene für eine Datenbank flexible Ressourcenpool entscheiden, Sie zunächst bestimmen den Features, die Sie benötigen die Dienstebene für Ihre Pool auswählen.

- Datenbankgröße (2 GB für Basic, 250 GB für Standard und 500 GB für Premium)
- Datenbank Sicherungskopie Aufbewahrungszeitraum (7 Tage für Basic 35 Tagen für Standard und 35 Tagen für Premium)
- Anzahl der Datenbanken pro Pool (400 für Basic, 400 für Standard und 50 für Premium)
- Maximale Speicherplatz pro Pool (117 GB für Basic, 1200 für Standard, und 750 für Premium)

Wenn Sie die Dienstebene für Ihre Ressourcenpool festgestellt haben, sind Sie bereit, um die Leistungsstufe für den Ressourcenpool (eDTUs) zu bestimmen. Sie können abzuschätzen und dann [Skalieren nach oben oder unten dynamisch skalieren](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) ausgehend von ist-Oberfläche. Die [DTU Rechner](http://dtucalculator.azurewebsites.net/) können Sie die Anzahl der DTUs für jede Datenbank in einem Ressourcenpool erforderlich, die Obergrenze für den Ressourcenpool ermitteln ungefähre.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die Preise für diese Ebenen auf [Die Preise der SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/).
- Erfahren Sie die Details der [flexible Pools](sql-database-elastic-pool-guidance.md) und [Preis und Leistung Aspekte für flexible Pools](sql-database-elastic-pool-guidance.md)an.
- Erfahren Sie, wie [Monitor, verwalten und Ändern der Größe flexible Pools](sql-database-elastic-pool-manage-portal.md) und [Überwachen der Leistung von eigenständigen Datenbanken](sql-database-single-database-monitor.md).
- Jetzt, da Sie die Ebenen der SQL-Datenbank kennen, mit einem [kostenlosen Konto](https://azure.microsoft.com/pricing/free-trial/) testen Sie und erfahren Sie, [wie Sie Ihre erste SQL-Datenbank erstellen](sql-database-get-started.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Entwurfmustern für mehrere Mandanten SaaS Applikationen Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy videokurses auf flexible Datenbankfunktionen in Azure SQL-Datenbank](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
