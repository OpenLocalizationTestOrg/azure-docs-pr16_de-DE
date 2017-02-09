Sie können mehrere Dienste innerhalb eines Abonnements, jeweils nach der Bereitstellung auf einer bestimmten Stufe, nur durch die Anzahl der Dienste, die in jeder Ebene zugelassen begrenzt erstellen. Beispielsweise konnten Sie bis zu 12 Services auf der grundlegende Stufe und eine andere 12 Services auf der Stufe S1 innerhalb des gleichen Abonnements erstellen. Weitere Informationen zu Ebenen finden Sie unter [Auswählen eines SKU oder Ebene für die Suche Azure](../articles/search/search-sku-tier.md).

Maximale Service Grenzwerte können auf Anforderung ausgelöst werden. Wenn Sie weitere Dienste innerhalb des gleichen Abonnements benötigen, wenden Sie sich an Azure-Support.

Ressource|Kostenlose|Grundlegende|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Maximale services |1 |12 |12  |6 |6 |6 
Maximale Skalierung in SU <sup>2</sup>|N/v <sup>3</sup>|3 SU <sup>4</sup> |36 SU|36 SU|36 SU|12 SU, 3 SU <sup>5</sup>

<sup>1</sup> S3 HD unterstützt keine [Indexer](../articles/search/search-indexer-overview.md) zu diesem Zeitpunkt. 

<sup>2</sup> suchen Einheiten (SU) sind berechenbaren Einheiten pro Dienst, als ein *Replikat* oder der *Partition*zugeordnet. Sie benötigen beide Ressourcen für Speicher, Indizierung und Abfragevorgänge. Weitere Informationen zum zulässigen Kombinationen, die unter den Höchstgrenzen bei bleiben, finden Sie unter [Skalieren Ressourcenebenen für die Abfrage und Index Auslastung](../articles/search/search-capacity-planning.md). 

<sup>3</sup> Free basiert auf freigegebenen Ressourcen, die von mehreren Abonnenten verwendet. Auf diese Stufe gibt es keine Ressourcen für eine einzelne Abonnenten. Aus diesem Grund ist die maximalen Dezimalstellen als nicht verfügbar markiert.

<sup>4</sup> Basic verfügt über eine feste Partition. Auf diese Stufe werden zusätzliche SUs zum Zuweisen von weiteren Kopien für höhere Abfrage Auslastung.

<sup>5</sup> S3 HD verfügt über eine andere Zuteilung Struktur im Hinblick auf zulässige Kombinationen. Für Replikations können maximal 12 sein. Bei Partitionen beträgt die maximale 3.




