<properties
    pageTitle="SQL-Datenbank die Optimierung Tipps | Microsoft Azure"
    description="Tipps zum Optimieren in Azure SQL-Datenbank mithilfe der Auswertung und Verbesserung der Leistung."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="SQL-Leistung optimieren, Datenbank-Performance optimieren, Tipps, die Sql Optimierung Sql Datenbank Leistung optimieren"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>Hinweise zur SQL-Datenbank Leistung Feinabstimmung
Ändern der [Dienstebene](sql-database-service-tiers.md) einer einzelnen Datenbank oder der eDTUs einer Datenbank flexible Ressourcenpool zu einem beliebigen Zeitpunkt für optimale Leistung erhöhen können, aber Sie Verkaufschancen zu verbessern und optimieren die Leistung von Abfragen zuerst identifizieren möchten. Fehlende Indizes und schlecht optimierte Abfragen werden häufige Gründe für die Leistung der Datenbank beeinträchtigt. Dieser Artikel enthält Hinweise zur Optimierung in SQL-Datenbank.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Schritte zum Auswerten und Abstimmung-Leistung
1.  Im [Portal Azure](https://portal.azure.com)-klicken Sie auf **SQL-Datenbanken**, wählen Sie die Datenbank, und klicken Sie dann anhand des Diagramms Überwachung um Annäherung an ihre Maximum Ressourcen zu suchen. DTU Verbrauch wird standardmäßig angezeigt. Klicken Sie auf **Bearbeiten** , um die Zeitraums und die angezeigten Werte ändern.
2.  Verwenden Sie [Die Leistung Einblick Abfrage](sql-database-query-performance.md) für die Abfragen mithilfe von DTUs ausgewertet werden soll, und dann [SQL-Datenbank Advisor](sql-database-advisor.md) Empfehlungen für das Erstellen und Indizes ablegen, parametrisieren Abfragen und Behebung von Problemen Schema anzeigen.
3.  Dynamische Management Ansichten (DMVs), Extended Events (Xevents) und den Abfrage-Speicher in SSMS können, um Leistungsparameter in Echtzeit zu gelangen. Finden Sie unter der [Leistung Anleitungen Thema](sql-database-performance-guidance.md) detaillierte Überwachung und Optimieren von Tipps.


    > [AZURE.IMPORTANT] Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Schritte zum Verbessern der Leistung der Datenbank mit Weitere Ressourcen
1.  Für die einzelnen Datenbanken können Sie [Service Ebenen ändern](sql-database-scale-up.md) bei Bedarf für optimale Leistung der Datenbank.
2.  Erwägen Sie für mehrere Datenbanken [flexible Datenbank Pools](sql-database-elastic-pool-guidance.md) verwenden, um Ressourcen automatisch zu skalieren.
