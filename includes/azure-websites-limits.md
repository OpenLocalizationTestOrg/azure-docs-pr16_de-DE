Ressource|Kostenlose|Freigegebene (Preview)|Grundlegende|Standard|Premium (Preview)</th>
---|---|---|---|---|---
[Web, Mobil oder API apps](https://azure.microsoft.com/services/app-service/) pro [-App Serviceplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Unbegrenzte<sup>2</sup>|Unbegrenzte<sup>2</sup>|Unbegrenzte<sup>2</sup>
[Logik apps](https://azure.microsoft.com/services/app-service/logic/) pro [App-Serviceplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 pro core|20 pro core
[App-Serviceplan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 pro region|10 pro Ressourcengruppe|100 pro Ressourcengruppe|100 pro Ressourcengruppe|100 pro Ressourcengruppe
Berechnen Instanztyp|Freigegeben|Freigegeben|Dedizierte<sup>3</sup>|Dedizierte<sup>3</sup>|Dedizierte<sup>3</sup></p>
[Skalierung](../articles/app-service-web/web-sites-scale.md) (max Instanzen)|1 freigegeben|1 freigegeben|3 dedizierte<sup>3</sup>|10 dedizierte<sup>3</sup>|20 dedizierter (50 in ASE)<sup>3,4</sup>
Speicher<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU-Zeit (short)<sup>6</sup>|3 Minuten|3 Minuten|Unbeschränkt, Bezahlung zu standard- [Sätzen](https://azure.microsoft.com/pricing/details/app-service/)</a>|Unbeschränkt, Bezahlung zu standard-Sätzen|Unbeschränkt, Bezahlung zu standard-Sätzen
CPU-Zeit (Tag)<sup>6</sup>|60 Minuten|240 Minuten|Unbeschränkt, Bezahlung zu standard- [Sätzen](https://azure.microsoft.com/pricing/details/app-service/)</a>|Unbeschränkt, Bezahlung zu standard-Sätzen|Unbeschränkt, Bezahlung zu standard-Sätzen
Arbeitsspeicher (1 Stunde)|1024 MB pro-App-Serviceplan|1024 MB pro app|N/V|N/V|N/V
Bandbreite|165 MB|Unbegrenzte, [Datenübertragung Sätzen](https://azure.microsoft.com/pricing/details/data-transfers/) anwenden|Unbeschränkt, Datenübertragung, die Gebühren|Unbeschränkt, Datenübertragung, die Gebühren|Unbeschränkt, Datenübertragung, die Gebühren
Architektur der Anwendung|32-bit|32-bit|32-Bit/64-bit|32-Bit/64-bit|32-Bit/64-bit
Web Sockets pro Instanz<sup>7</sup>|5|35|350|Unbegrenzte|Unbegrenzte
Aktiven [Debugger Verbindungen](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) pro Anwendung|1|1|1|5|5
[azurewebsites.NET Unterdomäne mit FTP-/ S und SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
Unterstützung für [benutzerdefinierte Domäne](../articles/app-service-web/web-sites-custom-domain-name.md)||X|X|X|X
Benutzerdefinierte Domäne, die [Unterstützung von SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Unbegrenzte|Unbeschränkt, 5 SNI SSL und 1 IP SSL-Verbindungen enthalten|Unbeschränkt, 5 SNI SSL und 1 IP SSL-Verbindungen enthalten
Integrierte Lastenausgleich||X|X|X|X
[Immer auf](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Geplante Sicherungskopien](../articles/app-service-web/web-sites-backup.md)||||Einmal pro Tag|Nachdem alle 5 Minuten<sup>8</sup>
[Automatische Skalierung](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
[Azure Scheduler](https://azure.microsoft.com/services/scheduler/) -support||X|X|X|X
[Endpunkt für die Überwachung](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Staging Steckplätze (Preview)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
App von benutzerdefinierten Domänen</a>||500|500|500|500
VEREINBARUNG ZUM SERVICELEVEL||<p>|99,9 %|99,95 %<sup>10</sup>|99,95 %<sup>10</sup>

<sup>1</sup> Apps und Speicherkontingente sind pro-App-Serviceplan, sofern nicht anders angegeben.  
<sup>2</sup> Die tatsächliche Anzahl von apps, die Sie auf diesen Computern hosten können hängt davon ab, die Aktivität der apps, die Größe der Instanzen Computer, und die entsprechenden Ressourcen Auslastung.  
<sup>3</sup> Bestimmte Instanzen können unterschiedliche Größen aufweisen. Weitere Informationen hierzu finden Sie unter [App Preisen](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) .  
<sup>4</sup> Premium Ebene kann bis zu 50 berechnet Instanzen (unterliegen Verfügbarkeit) und 500 GB Festplattenspeicher bei Verwendung von App-Service-Umgebungen und 20 berechnen andernfalls Instanzen und 250 GB-Speicher.  
<sup>5</sup> Die Speichergrenze ist die Gesamtgröße von Inhalten über alle apps in der gleichen App Serviceplan. Weitere Speicheroptionen stehen in der [App-Service-Umgebung](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Diese Ressourcen werden vom physischen Ressourcen auf die dedizierten Instanzen (die Instanzgröße und die Anzahl der Instanzen) eingeschränkt.  
<sup>7</sup> Wenn Sie eine app in zwei Instanzen der grundlegenden Ebene skalieren, müssen Sie 350 aktiven Verbindungen für jede der zwei Instanzen aus.  
<sup>8</sup> Premium Ebene kann zusätzliche Intervallen nach unten bis zu 5 Minuten bei Verwendung von App-Service-Umgebungen und 50 Mal pro Tag andernfalls.  
<sup>9</sup> Führen Sie benutzerdefinierte ausführbare Dateien und/oder Skripts bei Bedarf auf einen Zeitplan oder kontinuierlich als Hintergrundaufgabe innerhalb der App-Service-Instanz aus. Immer unter ist für kontinuierliche WebJobs Ausführung erforderlich. Azure Scheduler kostenlosen oder Standard ist für geplante WebJobs erforderlich. Es gibt keine vordefinierten Beschränkung der Anzahl der WebJobs, die in einer App-Service-Instanz ausgeführt werden kann, aber es gibt jedoch praktische Grenzwerte, die abhängen, was zu tun ist der Anwendungscode versucht.   
<sup>10</sup> Vereinbarung zum SERVICELEVEL von 99,95 % vorgesehenen Bereitstellungen, in denen mehrere Instanzen mit Azure Datenverkehr Manager verwendet, die für Failover konfiguriert werden.  
