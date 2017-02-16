<properties 
    pageTitle="Ändern der Dienstalter Ebene und Leistung einer SQL Azure-Datenbank mithilfe der PowerShell | Microsoft Azure" 
    description="Ändern der Dienstebene und Leistungsstufe einer SQL Azure-Datenbank gezeigt, wie die SQL-Datenbank mit PowerShell nach oben oder unten skalieren. Ändern der Preisgestaltung Ebene einer SQL Azure-Datenbank mit PowerShell an." 
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


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Ändern der Ebene und Leistung Dienstalter (Preisgestaltung Ebene) einer SQL-Datenbank mit PowerShell


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-scale-up.md)
- [**PowerShell**](sql-database-scale-up-powershell.md)


Dienst Ebenen und Leistung Ebenen beschreiben der Features und Ressourcen für die SQL-Datenbank zur Verfügung und können als den Anforderungen Ihrer Anwendung Änderung aktualisiert werden. Weitere Informationen finden Sie unter [Service Ebenen](sql-database-service-tiers.md).

Beachten Sie, dass die Dienstebene ändern und/oder Leistungsstufe einer Datenbank erstellt eine Kopie der ursprünglichen Datenbank in die neue Leistungsstufe und wechselt dann Verbindungen über an die. Keine Daten verloren sind, dabei aber während der kurzen Moment, wenn wir an die schalten, Verbindungen, um die Datenbank sind deaktiviert, damit einige Transaktionen in Flight rückgängig gemacht werden können. In diesem Fenster wird aktualisiert, aber ist im Durchschnitt weniger als 4 Sekunden und in mehr als 99 % der Fälle ist weniger als 30 Sekunden. Sehr selten, insbesondere dann, wenn es gibt großen Anzahl von Transaktionen in Flight bei den Pearsonschen Verbindungen deaktiviert sind, in diesem Fenster mehr.  

Die Dauer des gesamten Skalierung Prozesses hängt von der Umfang und Service Ebene der Datenbank vor und nach der Änderung. Beispielsweise sollte eine 250 GB-Datenbank, die an, von oder innerhalb einer Dienstebene Standard-geändert hat innerhalb von 6 Stunden abgeschlossen werden. Für eine Datenbank die gleiche Größe, die Performance innerhalb der Premium Service Ebene geändert hat, sollten sie innerhalb von 3 Stunden durchführen.


- Um eine Datenbank downgrade, sollten kleiner als die zulässige maximale Größe der Ziel-Dienst Stufe die Datenbank. 
- Beim Upgrade einer Datenbank mit [Geo-Replikation](sql-database-geo-replication-portal.md) aktiviert, müssen Sie zunächst die sekundären Datenbanken an die gewünschte Leistung Schicht aktualisieren, vor dem Upgrade der primären Datenbank.
- Wenn aus einer Premium Service Ebene herunterstufen, müssen Sie zuerst alle Geo-Replikation Beziehungen beenden. Führen Sie die Schritte im Thema [Wiederherstellen aus einem Ausfall](sql-database-disaster-recovery.md) So beenden Sie den Replikationsprozess zwischen der primären und der aktiven sekundären Datenbanken beschrieben.
- Wiederherstellen-Service-Angebote unterscheiden sich für die verschiedenen Ebenen der Dienst. Wenn Sie das Herabstufen sind möglicherweise nicht mehr zu einem Zeitpunkt wiederherstellen oder einen unteren Sicherung Aufbewahrungszeitraum haben. Weitere Informationen finden Sie unter [Azure SQL-Datenbank sichern und Wiederherstellen](sql-database-business-continuity.md).
- Die neuen Eigenschaften für die Datenbank werden nicht angewendet, bis die Änderungen abgeschlossen sind.



**Zum Durchführen dieses Artikels benötigen Sie Folgendes:**

- Ein Azure-Abonnement. Wenn Sie ein Abonnement Azure lediglich auf **Kostenloses Konto** oben auf dieser Seite und dann wieder zum Ende dieses Artikels.
- Einer SQL Azure-Datenbank. Wenn Sie nicht mit eine SQL-Datenbank verfügen, erstellen eine folgen die Schritten in diesem Artikel: [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Ändern der Dienstalter Ebene und Leistung der SQL-Datenbank

Ausführen der [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) Cmdlet und Festlegen der **-RequestedServiceObjectiveName** , um die Leistungsstufe der gewünschten Stufe Preisgestaltung; z. B. *S0* *S1*, *S2*, *S3*, *P1*, *P2*...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>PowerShell-Skript zum Ändern der Dienstalter Ebene und Leistung der SQL-Datenbank (Beispiel)

Ersetzen Sie ```{variables}``` mit Ihren Werten (enthalten nicht die geschweiften Klammern).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Nächste Schritte

- [Aus- und skalieren](sql-database-elastic-scale-get-started.md)
- [Verbinden und Abfragen mit SSMS eine SQL-Datenbank](sql-database-connect-query-ssms.md)
- [Exportieren einer SQL Azure-Datenbank](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Business Continuity (Übersicht)](sql-database-business-continuity.md)
- [SQL-Datenbank-Dokumentation](http://azure.microsoft.com/documentation/services/sql-database/)
- [SQL Azure-Datenbank-Cmdlets] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/Bibliothek/Azure/mt619433(v=azure.300\).aspx.aspx)