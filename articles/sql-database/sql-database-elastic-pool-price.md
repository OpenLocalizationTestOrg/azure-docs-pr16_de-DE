<properties
    pageTitle="SQL-Datenbank flexible Ressourcenpool Preis und Leistung"
    description="Speziell für flexible Datenbank Pools Preisinformationen."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Flexible Datenbank Ressourcenpool Abrechnung und Preisinformationen

Flexible Datenbank Pools pro folgende Merkmale in Rechnung gestellt werden:

- Eine flexible Ressourcenpool wird bei seiner Erstellung, Abrechnung, selbst wenn keine Datenbanken vorhanden, in dem Pool sind.
- Eine flexible Ressourcenpool ist stündlich in Rechnung gestellt. Hierbei handelt es sich um die gleichen Messfunktionen Häufigkeit wie für Leistungsstufe des einzelnen Datenbanken.
- Wenn eine flexible Ressourcenpool zu einer neuen Umfang der eDTUs Größe geändert wird, ist dann Pool nicht in Rechnung gestellt entsprechend den neuen Umfang der eDTUS bis zum Abschluss der Größenänderung Vorgangs. Entspricht dem gleiche Muster als die Leistungsstufe der eigenständigen Datenbanken ändern.


- Der Preis von einem flexible Pool basiert auf die Anzahl der eDTUs im Ressourcenpool. Der Preis einer flexible Ressourcenpool ist unabhängig von der Anzahl und Nutzung der flexible Datenbanken darin.
- Kurs wird berechnet, indem (Anzahl der Ressourcenpool eDTUs) x (Einzelpreises pro eDTU).

EDTU Einzelpreises für eine flexible Ressourcenpool ist größer als der Einzelpreis DTU für eine Datenbank eigenständigen in der gleichen Dienstebene. Weitere Informationen finden Sie unter [Preise SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/). 


Die Ebenen eDTUs und -Dienst finden Sie unter [Optionen für SQL-Datenbank und Leistung](sql-database-service-tiers.md).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Datenbank flexible Ressourcenpool](sql-database-elastic-pool-create-portal.md)
- [Überwachen, verwalten und Größe einer Datenbank flexible Ressourcenpool](sql-database-elastic-pool-manage-portal.md)
- [SQL-Datenbank-Optionen und Leistung: verstehen, was in jeder Kategorie Dienst verfügbar ist.](sql-database-service-tiers.md)
- [PowerShell-Skript zum Identifizieren von Datenbanken für eine Datenbank flexible Ressourcenpool geeignet](sql-database-elastic-pool-database-assessment-powershell.md)
