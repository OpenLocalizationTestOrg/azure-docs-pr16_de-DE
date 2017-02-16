<properties
    pageTitle="Ändern der Dienstalter Ebene und Leistung einer SQL Azure-Datenbank | Microsoft Azure"
    description="Ändern der Dienstebene und zeigt, wie die SQL-Datenbank nach oben oder unten skalieren Leistungsstufe einer SQL Azure-Datenbank. Ändern der Preisgestaltung Ebene einer SQL Azure-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Ändern der Ebene und Leistung Dienstalter (Preise Ebene) einer SQL-Datenbank mithilfe des Azure-Portals


> [AZURE.SELECTOR]
- [**Azure-portal**](sql-database-scale-up.md)
- [PowerShell](sql-database-scale-up-powershell.md)


Dienst Ebenen und Leistung Ebenen beschreiben der Features und Ressourcen für die SQL-Datenbank zur Verfügung und können als den Anforderungen Ihrer Anwendung Änderung aktualisiert werden. Weitere Informationen finden Sie unter [Service Ebenen](sql-database-service-tiers.md).

Beachten Sie, dass die Dienstebene ändern und/oder Leistungsstufe einer Datenbank erstellt eine Kopie der ursprünglichen Datenbank in die neue Leistungsstufe und wechselt dann Verbindungen über an die. Dabei ist keine Daten verloren während der kurzen Moment, wenn wir an die schalten, Verbindungen mit der Datenbank sind jedoch deaktiviert, damit einige Transaktionen in Flight rückgängig gemacht werden können. In diesem Fenster wird aktualisiert, aber ist im Durchschnitt weniger als 4 Sekunden und in mehr als 99 % der Fälle ist weniger als 30 Sekunden. Sehr selten, insbesondere dann, wenn es gibt großen Anzahl von Transaktionen in Flight bei den Pearsonschen Verbindungen deaktiviert sind, in diesem Fenster mehr.  

Die Dauer des gesamten Skalierung Prozesses hängt von der Umfang und Service Ebene der Datenbank vor und nach der Änderung. Beispielsweise sollte eine 250 GB-Datenbank, die an, von oder innerhalb einer Dienstebene Standard-geändert hat innerhalb von 6 Stunden abgeschlossen werden. Für eine Datenbank die gleiche Größe, die Performance innerhalb der Premium Service Ebene geändert hat, sollten sie innerhalb von 3 Stunden durchführen.


Verwenden Sie die Informationen in [Azure SQL-Datenbank Dienst Ebenen und Leistung Ebenen](sql-database-service-tiers.md) anhand der entsprechenden Ebene und Leistung Dienstalter für die Azure SQL-Datenbank.

- Um eine Datenbank downgrade, sollten kleiner als die zulässige maximale Größe der Ziel-Dienst Stufe die Datenbank. 
- Beim Upgrade einer Datenbank mit [Geo-Replikation](sql-database-geo-replication-overview.md) aktiviert, müssen Sie zunächst die sekundären Datenbanken an die gewünschte Leistung Schicht aktualisieren, vor dem Upgrade der primären Datenbank.
- Wenn eine Stufe Dienst das Herabstufen, müssen Sie zuerst alle Geo-Replikation Beziehungen beenden. 
- Wiederherstellen-Service-Angebote unterscheiden sich für die verschiedenen Ebenen der Dienst. Wenn Sie das Herabstufen sind möglicherweise nicht mehr zu einem Zeitpunkt wiederherstellen oder einen unteren Sicherung Aufbewahrungszeitraum haben. Weitere Informationen finden Sie unter [Azure SQL-Datenbank sichern und Wiederherstellen](sql-database-business-continuity.md).
- Ändern die Datenbank, die Preise Ebene ändert sich nicht auf die Größe der Datenbank max. Wenn Sie die Datenbank ändern [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)maximale Größe verwenden.
- Die neuen Eigenschaften für die Datenbank werden nicht angewendet, bis die Änderungen abgeschlossen sind.



**Zum Durchführen dieses Artikels benötigen Sie Folgendes:**

- Einer SQL Azure-Datenbank. Wenn Sie nicht mit eine SQL-Datenbank verfügen, erstellen Sie eine folgen die Schritten in diesem Artikel: [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Ändern der Dienstalter Ebene und Leistung der Datenbank


Öffnen Sie das Blade SQL-Datenbank für die Datenbank, die, der Sie nach oben oder unten skalieren möchten:

1.  Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **Weitere Dienste** > **SQL-Datenbanken**.
2.  Klicken Sie auf die Datenbank, die Sie ändern möchten.
3.  Klicken Sie auf das Blade **SQL-Datenbank** auf **Preise Ebene (Maßstab DTUs)**:

    ![Preise Ebene][1]

1.  Wählen Sie eine neue Kategorie aus, und klicken Sie auf **auswählen**:

    **Wählen Sie** auf fordert die Skalierung die Preisgestaltung Ebene ändern. Abhängig von der Größe der Datenbank kann die Skalierung eine Weile zu abgeschlossen (siehe Informationen oben in diesem Artikel) dauern.

    > [AZURE.NOTE] Ändern die Datenbank, die Preise Ebene ändert sich nicht auf die Größe der Datenbank max. Wenn Sie die Datenbank ändern [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)maximale Größe verwenden.

    ![Wählen Sie Preisgestaltung Ebene aus.][2]

3.  Klicken Sie auf das Benachrichtigungssymbol (Glocke), in der oberen rechten Ecke:

    ![Benachrichtigungen][3]

    Klicken Sie auf die Benachrichtigungstext um den Detailbereich zu öffnen, in dem Sie den Status der Anfrage sehen können.




## <a name="next-steps"></a>Nächste Schritte

- Ändern Sie die max Datenbankgröße mit [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- [Aus- und skalieren](sql-database-elastic-scale-get-started.md)
- [Verbinden und Abfragen mit SSMS eine SQL-Datenbank](sql-database-connect-query-ssms.md)
- [Exportieren einer SQL Azure-Datenbank](sql-database-export.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Business Continuity (Übersicht)](sql-database-business-continuity.md)
- [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
