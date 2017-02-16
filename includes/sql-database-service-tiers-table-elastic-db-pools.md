
### <a name="basic-elastic-pool-limits"></a>Grundlegende flexible Ressourcenpool Beschränkungen

|   |  |
|---|:---:|
| Max eDTUs pro pool | &nbsp;100 &nbsp;&nbsp;&nbsp; 200 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp;&nbsp; 1200 |
| Max-Speicher pro Pool (GB) *| &nbsp;&nbsp;&nbsp;&nbsp;10 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;39 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;73 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;117 |
| Maximale Anzahl von Datenbanken pro Ressourcenpool | &nbsp;&nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 |
| Max in-Memory-OLTP-Speicher (GB) pro pool| N/V |
| Max gleichzeitige Kollegen pro pool | &nbsp;&nbsp;&nbsp;200 &nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp;&nbsp;2400 |
| Max gleichzeitige Benutzernamen pro pool | &nbsp;&nbsp;&nbsp;200 &nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp;&nbsp;2400 |
| Max gleichzeitige Sitzungen pro pool | 4800 &nbsp;9600 &nbsp; 19200 &nbsp; 28800 &nbsp; 28800 |
| Max eDTUs pro Datenbank * | 5 |
| Min eDTUs pro Datenbank * | 0,5 |
| Max-Speicher pro Datenbank (GB) ** | 2 |
| Punkt in Zeit wiederherstellen | Beliebiger Punkt, der letzten 7 Tage |
| Wiederherstellung | Aktive Geo-Replikation |
|||

* Max und Min eDTU des pro Datenbank möglicherweise eine der aufgeführten Werte festgelegt werden, solange die Ressourcenpool DTU Größe ausgewählt mindestens so groß wie die maximale eDTUs pro DB ist 

** Flexible Datenbank freigeben Ressourcenpool Speicher, Datenbankspeicher auf den kleineren beschränkt ist der verbleibenden Speicherpools oder max Speicherplatz pro Datenbank


### <a name="standard-elastic-pool-limits"></a>Standard flexible Ressourcenpool Beschränkungen

|   |  |
|---|:---:|
| Max eDTUs pro pool | &nbsp;100 &nbsp;&nbsp;&nbsp; 200 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp;&nbsp; 1200 |
| Max-Speicher pro Pool (GB) *| &nbsp;100 &nbsp;&nbsp;&nbsp; 200 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp;&nbsp; 1200 |
| Maximale Anzahl von Datenbanken pro Ressourcenpool | &nbsp;200 &nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 |
| Max in-Memory-OLTP-Speicher (GB) pro pool| N/V |
| Max gleichzeitige Worker pro pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp; 2400 |
| Max gleichzeitige Benutzernamen pro pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp; 2400 |
| Max gleichzeitige Sitzungen pro pool | 4800 &nbsp; 9600 &nbsp;19200 &nbsp;28800 &nbsp; &nbsp; 28800 |
| Max eDTUs pro Datenbank * | 10, 20, 50, 100 |
| Min eDTUs pro Datenbank * | 0, 10, 20, 50, 100 |
| Max-Speicher pro Datenbank (GB) ** | 250 |
| Punkt in Zeit wiederherstellen | Zeigen Sie eine letzten 35 Tage |
| Wiederherstellung | Aktive Geo-Replikation |
|||

* Max und Min eDTU des pro Datenbank möglicherweise eine der aufgeführten Werte festgelegt werden, solange die Ressourcenpool DTU Größe ausgewählt mindestens so groß wie die maximale eDTUs pro DB ist 

** Flexible Datenbank freigeben Ressourcenpool Speicher, Datenbankspeicher auf den kleineren beschränkt ist der verbleibenden Speicherpools oder max Speicherplatz pro Datenbank

### <a name="premium-elastic-pool-limits"></a>Premium flexible Ressourcenpool Beschränkungen

|   |  |
|---|:---:|
| Max eDTUs pro pool | 125 &nbsp;&nbsp;&nbsp; 250 &nbsp;&nbsp;&nbsp; 500 &nbsp;&nbsp;&nbsp; 1000 &nbsp;&nbsp;&nbsp; &nbsp;1500 |
| Max-Speicher pro Pool (GB) *| 250 &nbsp;&nbsp;&nbsp; 500 &nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 750 |
| Maximale Anzahl von Datenbanken pro Ressourcenpool | 50 |
| Max in-Memory-OLTP-Speicher (GB) pro pool| N/V |
| Max gleichzeitige Kollegen pro pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp; 2400 |
| Max gleichzeitige Benutzernamen pro pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp; 2400 |
| Max gleichzeitige Sitzungen pro pool | 4800 &nbsp; 9600 &nbsp;19200 &nbsp;28800 &nbsp; &nbsp; 28800 |
| Max eDTUs pro Datenbank * | 125, 250, 500 1000 |
| Min eDTUs pro Datenbank * | 0, 125, 250, 500, 1000 |
| Max-Speicher pro Datenbank (GB) ** | 500 |
| Punkt in Zeit wiederherstellen | Zeigen Sie eine letzten 35 Tage |
| Wiederherstellung | Aktive Geo-Replikation |
|||

* Max und Min eDTU des pro Datenbank möglicherweise eine der aufgeführten Werte festgelegt werden, solange die Ressourcenpool DTU Größe ausgewählt mindestens so groß wie die maximale eDTUs pro DB ist 

** Flexible Datenbank freigeben Ressourcenpool Speicher, Datenbankspeicher auf den kleineren beschränkt ist der verbleibenden Speicherpools oder max Speicherplatz pro Datenbank
