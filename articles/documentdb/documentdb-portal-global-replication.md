<properties
    pageTitle="Globale Datenbankreplikation DocumentDB | Microsoft Azure"
    description="Erfahren Sie, wie die globale Replikation Ihres Kontos DocumentDB über das Azure-Portal zu verwalten."
    services="documentdb"
    keywords="globale Datenbank, Replikation"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Globale Datenbankreplikation DocumentDB über das Azure-Portal durchführen

Informationen Sie zum Azure-Portal zu verwenden, um Daten in mehreren Bereichen für globale Verfügbarkeit von Daten in Azure DocumentDB repliziert.

Weitere Informationen zur Datenbank wie globale Replikation in DocumentDB, finden Sie unter [Verteilen von Daten mit DocumentDB Global](documentdb-distribute-data-globally.md). Informationen zum globalen Datenbankreplikation programmgesteuert ausführen finden Sie unter [Entwickeln mit mehreren Region DocumentDB Konten](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Globale Verteilung der DocumentDB Datenbanken ist in der Regel verfügbar und für alle neu erstellten DocumentDB Konten automatisch aktiviert. Wir arbeiten an globale Verteilung auf alle vorhandenen Konten aktivieren, aber in der Zwischenzeit Bedarf globale Verteilerlisten auf Ihr Konto aktiviert wenden Sie sich bitte [an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , und wir werden Aktivieren der Arbeitsmappe für Sie jetzt.

## <a name="a-idaddregionaadd-global-database-regions"></a><a id="addregion"></a>Globale Datenbank Regionen hinzufügen

DocumentDB steht in den meisten [Azure Regionen] [azureregions]. Nachdem Sie die Konsistenz Standardstufe für Ihr Datenbankkonto ausgewählt haben, können Sie einen oder mehrere Bereiche (je nach verwendeter Standard Konsistenz Ebene und globale Verteilung Anforderungen) zuordnen.

1. Klicken Sie im [Portal Azure](https://portal.azure.com/)in der Jumpbar **DocumentDB-Konten**aus.
2. Wählen Sie das Datenbankkonto zu ändern, in das Blade **DocumentDB Konto** .
3. Klicken Sie in das Konto Blade aus dem Menü auf **Global repliziert Daten** .
4. Klicken Sie in das Blade **Global repliziert Daten** wählen Sie die Regionen hinzufügen oder entfernen, und klicken Sie dann auf **Speichern**. Zum Hinzufügen von Regionen Kosten vorhanden ist, finden Sie unter die [Preise Seite](https://azure.microsoft.com/pricing/details/documentdb/) oder [Verteilen von Daten mit DocumentDB Global](documentdb-distribute-data-globally.md) Artikel Weitere Informationen.

    ![Klicken Sie auf die Bereiche in der Karte, um Sie hinzuzufügen oder zu entfernen][1]

### <a name="selecting-global-database-regions"></a>Auswählen eines globalen Datenbank Regionen

Wenn Sie zwei oder mehr Regionen konfigurieren, wir empfehlen Regionen ausgewählt sind basierend auf der Region Paare beschrieben die [Business Continuity- und Disaster Wiederherstellung (BCDR): Azure gepaart Regionen]  [ bcdr] Artikel.

Insbesondere wenn Sie auf mehrere Bereiche zu konfigurieren, stellen Sie sicher, wählen Sie die gleiche Anzahl von Regionen (+/-1 für ungerade/gerade) in jede Spalte für Region. Beispielsweise, wenn Sie vier US-Bereiche bereitstellen möchten, wählen Sie aus zwei US-Regionen aus der linken Spalte und zwei von rechts. Ja, werden die folgenden würde einen entsprechenden Satz: Westen US ostasiatischen US, US North Central und Süd zentralen USA.

Dieser Leitfaden ist wichtig, denen Sie folgen, wenn nur zwei Bereiche für Wiederherstellungssituationen konfiguriert sind. Für mehr als zwei Bereiche nach dieser Anleitung empfiehlt, aber nicht unbedingt als Teil der ausgewählten Regionen an diese Verbindung halten.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a name="a-idnextanext-steps"></a><a id="next"></a>Nächste Schritte

Erfahren Sie, wie die Konsistenz Ihres Kontos global repliziert zu verwalten, indem Sie [Konsistenz Ebenen in DocumentDB](documentdb-consistency-levels.md)lesen.

Weitere Informationen zur Datenbank wie globale Replikation in DocumentDB, finden Sie unter [Verteilen von Daten mit DocumentDB Global](documentdb-distribute-data-globally.md). Informationen zur programmgesteuert Replikation von Daten in mehreren Bereichen finden Sie unter [Entwickeln mit mehreren Region DocumentDB Konten](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
