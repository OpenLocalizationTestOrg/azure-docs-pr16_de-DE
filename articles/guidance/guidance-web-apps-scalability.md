<properties
   pageTitle="Skalierbare Webanwendung | Azure Bezug Architektur | Microsoft Azure"
   description="Verbessern der Skalierbarkeit in einer Webanwendung, die in Microsoft Azure ausgeführt."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Verbessern der Skalierbarkeit in einer Webanwendung 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel zeigt eine empfohlene Architektur zum Verbessern der Skalierbarkeit und Leistung in einer Webanwendung, die auf Microsoft Azure ausgeführt wird. Die Architektur basiert auf [Azure Bezug Architektur: grundlegende Webanwendung][basic-web-app]. Die Empfehlungen und Aspekte in diesem Artikel gelten für diese Architektur ebenfalls.

>[AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: Ressourcenmanager und Classic. In diesem Artikel wird Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt verwendet.

## <a name="architecture-diagram"></a>Architekturdiagramm

![[0]][0]

Die Architektur weist die folgenden Komponenten:

- **Ressourcengruppe**. Eine [Ressourcengruppe] [ resource-group] ist ein logischer Container für Azure Ressourcen. 

- ** [Web app] [ app-service-web-app] ** und ** [API app][app-service-api-app]**. Eine typische moderne Anwendung möglicherweise sowohl eine Website und einen oder mehrere Rest Web-APIs sind. Eine Web-API möglicherweise durch AJAX-Browserclients, systemeigenen Clientanwendungen oder serverseitigen Applikationen genutzt werden. Aspekte Entwerfen von Web APIs, finden Sie unter [API Entwurf Anleitungen][api-guidance].    

- **WebJob**. Verwenden von [Azure WebJobs] [ webjobs] langer Vorgänge im Hintergrund ausführen. WebJobs können auf einen Zeitplan, dort ständig, oder als Reaktion auf einen Trigger, z. B. mit einer Nachricht in einer Warteschlange ausgeführt werden. Eine WebJob wird als Hintergrundprozess im Kontext einer app-App-Dienst ausgeführt. 

- **Warteschlange**. In der hier gezeigten Architektur die Anwendungswarteschlangen Hintergrundaufgaben, indem Sie mit einer Nachricht auf eine [Azure Warteschlangenspeicher] [ queue-storage] Warteschlange. Die Nachricht auslöst eine Funktion in der WebJob. 

    Alternativ können Sie Servicebuswarteschlangen verwenden. Einen Vergleich finden Sie unter [Azure Warteschlangen Servicebuswarteschlangen - verglichen und bereitstehen][queues-compared].

- **Cache**. Halb statische Daten in [Azure Redis Cache]gespeichert[azure-redis].  

- **CDN**. Verwenden von [Azure Content Delivery Network] [ azure-cdn] (CDN) öffentlich zugängliche Zwischenspeichern von Inhalten, für den unteren Wartezeit und schnellere Zustellung von Inhalten.

- **Speichern von Daten.** Verwenden von [Azure SQL-Datenbank] [ sql-db] für relationale Daten. Erwägen Sie zur nicht-relationalen Daten NoSQL-Speichers, z. B. Azure Table Storage oder [DocumentDB][documentdb].

- **Azure zu suchen**. Verwenden Sie die [Suche Azure] [ azure-search] Suchfunktionen, einschließlich Suchvorschläge, fuzzy-Suche und sprachspezifische Suche hinzufügen. Azure Suche wird in der Regel in Verbindung mit einem anderen Datenspeicher, verwendet, insbesondere dann, wenn der primäre Datenspeicher strict Konsistenz erfordert. Bei dieser Vorgehensweise würde die autorisierenden Daten in der anderen Datenquelle, und setzen den Suchindex in Azure suchen. Azure suchen kann auch zum Konsolidieren von eines einzelnen Suchindexes aus mehreren Datenspeichern verwendet werden.  

- **E-Mail/SMS**. Wenn die Anwendung benötigt, e-Mail oder SMS-Nachrichten zu senden, verwenden Sie einer Drittanbieter-Service wie SendGrid oder Twilio, statt zu dieses Feature direkt in die Anwendung zu erstellen.

## <a name="recommendations"></a>Empfehlungen

### <a name="app-service-apps"></a>App-Service-apps 

Es empfiehlt sich, erstellen die Webanwendung und die Web-API als separate App-Service-apps. Dieser Entwurf können Sie die führen sie in separaten App-Service-Pläne, der wiederum Sie diese unabhängig voneinander zu skalieren können. Wenn Sie die Ebene der Skalierbarkeit am benötigen zuerst, können Sie der apps in demselben Plan bereitstellen, und verschieben sie in separaten Plänen später bei Bedarf. (Für die Basic Standard und Premium-Pläne, sind Sie für die Instanzen virtueller Computer in den Plan, nicht für einzelne app Abrechnung. [Preise App-Dienst]finden Sie unter[app-service-pricing])

Wenn Sie beabsichtigen, die *Einfach Tabellen* oder *Einfach APIs* Features von Mobile-Apps für App-Dienst verwenden, erstellen Sie eine separate App-Service-app für diesen Zweck ein.  Diese Features basieren auf eine bestimmte Anwendungsframework zu aktivieren.

### <a name="webjobs"></a>WebJobs

Ist der WebJob viele Ressourcen beanspruchen, darüber nachdenken Sie, es zu einer leeren App-Service-app in einem separaten App Serviceplan dedizierte Instanzen der WebJob hinzufügen. [Hintergrund Aufträge Anleitungen]finden Sie unter[webjobs-guidance].  

### <a name="cache"></a>Cache

Sie können die Leistung und Skalierbarkeit verbessern, indem Sie [Azure Redis Cache] [ azure-redis] einiger Daten zwischenspeichern. Cache für Redis in Betracht:

- Halb statischen Transaktionsdaten.

- Sitzungszustand.

- HTML-Ausgabe. Dies kann in Clientanwendungen hilfreich sein, die komplexe HTML-Ausgabe gerendert werden. 

Ausführlichere Anleitung zum Entwerfen einer Zwischenspeichern Strategie finden Sie unter [Zwischenspeichern Anleitungen][caching-guidance].

### <a name="cdn"></a>CDN 

Verwenden von [Azure CDN] [ azure-cdn] in statischen Cacheinhalt. Der wichtigste Vorteil der einem CDN ist Wartezeit für Benutzer zu verringern, da bei einer *Kante Server* Zwischenspeichern von Inhalten, die in der Nähe der Benutzer geografischen ist. CDN kann auch auf der Anwendung, laden reduzieren, da dieser Datenverkehr nicht von der Anwendung behandelt wird.

- Wenn Ihre app hauptsächlich statische Seiten umfasst, erwägen Sie, die CDN die gesamte app Zwischenspeichern. [Azure CDN in Azure App-Dienst verwenden,]finden Sie unter[cdn-app-service].

- Andernfalls Dateien setzen statische Inhalte wie Bilder, CSS und HTML-, in Azure-Speicher und mithilfe von CDN diese Dateien im cache. [Integrieren von einem Speicherkonto mit CDN]finden Sie unter[cdn-storage-account].

> [AZURE.NOTE] Azure CDN kann keine Inhalte bereitstellen, die Authentifizierung erforderlich ist.

Ausführlichere Anleitungen finden Sie unter [Anleitungen Content Delivery Network (CDN)][cdn-guidance]. 

### <a name="storage"></a>Speicher

Moderne Applikationen verarbeiten häufig große Datenmengen aus. Damit der Cloud zu skalieren, ist es wichtig, den richtigen Speicher auswählen. Hier sind einige Vorschläge Basisplan aus.  Ausführlichere Anleitungen finden Sie unter [Bewertung von Daten Store-Funktionen für polyglotte Beibehaltung Lösungen][polyglot-storage].

Was Sie speichern möchten. | Beispiel | Empfohlene Speicherung
--- | --- | ---
Dateien | Bilder, Dokumente, PDF-Dateien | Azure Blob-Speicher
Schlüssel/Wert-Paare | Benutzerprofildaten nachgeschlagen durch Benutzer-ID | Azure Table Storage
Kurze Nachrichten soll weitere Verarbeitung auslösen | Reihenfolge Besprechungsanfragen | Azure Warteschlangenspeicher, Service Bus Warteschlange oder Service Bus Thema
Nicht relationaler Daten mit einem flexible Schema, dass grundlegende Abfragen | Produktkatalog | Dokument-Datenbank, wie z. B. Azure DocumentDB, MongoDB oder Apache CouchDB
Relationale Daten, dass umfangreichere Unterstützung von Abfragen, die nur Schema und die signifikante Konsistenz | Produktbestands | SQL Azure-Datenbank

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

### <a name="app-service-app"></a>App-Service-app

Wenn Ihre Lösung mehrere App Dienst apps enthält, darüber nachdenken Sie, um Dienstpläne App zu trennen. Dieser Ansatz können Sie diese unabhängig voneinander, zu skalieren, da diese auf separaten Instanzen ausgeführt werden. Weitere Informationen zu Skalierung, finden Sie unter der [Skalierbarkeit Aspekte] [ basic-web-app-scalability] Abschnitt in der [grundlegenden Web Anwendungsarchitektur][basic-web-app].

Auf ähnliche Weise Erwägen einer WebJob in einem eigenen Plan, sodass Hintergrundaufgaben für dieselben Instanzen ausgeführt werden, die HTTP-Anfragen behandelt.  

### <a name="sql-database"></a>SQL-Datenbank

Skalierbarkeit einer SQL-Datenbank erhöhen, indem Sie die Datenbank- *Sharding* &mdash; d. h., horizontal partitionieren der Datenbank. Sharding können Sie die Datenbank horizontal skalieren mit [flexible Datenbanktools][sql-elastic]. Sharding mögliche folgende Vorteile:

- Bessere Transaktionsdurchsatz.

- Abfragen können über eine Teilmenge der Daten schneller ausgeführt werden. 

### <a name="azure-search"></a>Azure suchen

Azure Search entfernt den Aufwand für die Suche komplexer Daten aus dem primären Datenspeicher, und es so Last bewältigt skaliert werden kann. Finden Sie unter [Skalieren Ressourcenebenen für die Abfrage und Indizierung Auslastung in Azure-Suche][azure-search-scaling].

## <a name="security-considerations"></a>Zur Sicherheit

### <a name="cross-origin-resource-sharing-cors"></a>Cross-Origin Ressource freigeben (CORS)

Wenn Sie eine Website und Web-API als separate apps erstellen, kann keine die Website clientseitige AJAX-Aufrufe der-API vornehmen, es sei denn, Sie CORS aktivieren. 

> [AZURE.NOTE] Browser-Sicherheit verhindert, dass eine Webseite AJAX Besprechungsanfragen in eine andere Domäne. Diese Einschränkung heißt die Richtlinie same Origin und verhindert, dass eine bösartige Website lesen Sentitive Daten aus einer anderen Website. CORS ist ein W3C-Standard, der einen Server die Richtlinie same Origin entspannen und einige Cross-Origin-Anfragen zulassen ermöglicht, während andere ablehnen.

App-Services bietet eine integrierte Unterstützung für CORS, ohne Anwendungscode schreiben. [Verbrauchen JavaScript CORS mithilfe einer app API]finden Sie unter[cors]. Hinzufügen der Website zur Liste der zulässigen Ursprung für die-API. 

### <a name="sql-database-encryption"></a>SQL-Datenbank-Verschlüsselung

[Transparente Verschlüsselung] verwenden[ sql-encryption] Wenn Sie Daten in der Datenbank verschlüsseln müssen. Dieses Feature führt in Echtzeit und Verschlüsselung von eine gesamte Datenbank (einschließlich Sicherungskopien und Transaktionsprotokolldateien), ohne dass Änderungen an der Anwendung. Verschlüsselung werden in einiger Wartezeit, das hinzugefügt, sodass es sich empfiehlt, die Daten, die in einem eigenen Datenbank sicher sein müssen, und aktivieren Sie die Verschlüsselung nur für diese Datenbank zu trennen.  

## <a name="next-steps"></a>Nächste Schritte

- Höhere Verfügbarkeit die Anwendung in mehr als eine Region bereitstellen und Verwenden von [Azure Datenverkehr Manager] [ tm] Failoververarbeitung. Weitere Informationen finden Sie unter [Azure Bezug Architektur: Web-Anwendung mit hoher Verfügbarkeit][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Web-Anwendung in Azure mit verbesserter Skalierbarkeit"