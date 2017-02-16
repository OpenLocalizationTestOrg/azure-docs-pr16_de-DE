<properties
   pageTitle="Fehler bei der Modus Analyse | Microsoft Azure"
   description="Richtlinien zur Durchführung Fehler Modus Analyse für basierend auf Azure Cloudlösungen."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="mwasson"/>

# <a name="azure-resiliency-guidance-failure-mode-analysis"></a>Azure Stabilität Anleitungen: Fehler Modus Analyse

Fehler bei der Modus Analyse (FMA) umfasst zum Erstellen der Stabilität in einem System kenntlich möglicherweise Fehlerpunkte im System. FMA, sollten Teil der Architektur und den Entwurf Phasen, damit Sie Wiederherstellung nach einem Fehler in das System ab dem Beginn erstellen können.

Hier ist die allgemeine Vorgehensweise zum Durchführen einer FMA ein: 

1. Identifizieren Sie alle Komponenten in das System ein. Externe Abhängigkeiten, beispielsweise als Identitätsanbieter, Drittanbieter-Services usw. einbeziehen   

2. Identifizieren Sie für jede Komponente potenzielle Fehler, die auftreten können. Möglicherweise müssen Sie eine einzelne Komponente mehr als einen Fehlermodus. Beispielsweise sollten Sie erwägen gelesen Fehlern und Fehlern separat geschrieben werden, da die Auswirkung und möglich Problembehebungen unterscheiden.

3. Bewerten Sie jeweiligen Fehlermodus entsprechend den allgemeinen Risikos. Berücksichtigen Sie die folgenden Faktoren:  

    - Was ist die Wahrscheinlichkeit des Fehlers. Ist es relativ häufig? Extrememly seltene? Sie benötigen keine genaue Zahlen; der Zweck ist die Priorität Rangfolge helfen.

    - Was ist die Auswirkung auf die Anwendung, die in Bezug auf die Verfügbarkeit, Datenverlust, monetäre Kosten und Business Unterbrechung? 

4. Ermitteln Sie für jede fehlerhaften wie reagieren und Wiederherstellen die Anwendung aus. Erwägen Sie Kompromisse in Kosten und Anwendungsdatenbank Komplexität aus.   

Dieser Artikel enthält als Ausgangspunkt für den Prozess FMA einen Katalog von mögliche Fehlermodi und deren Problembehebungen an. Der Katalog ist nach Technologie oder Azure Service sowie eine allgemeine Kategorie für das Design der Anwendung Ebene angeordnet. Der Katalog ist nicht vollständig, aber viele der wichtigen Azure behandelt Services. 


## <a name="app-service"></a>App-Dienst

### <a name="app-service-app-shuts-down"></a>App-Dienst app wird beendet.

**Erkennung**. Mögliche Gründe:

- Erwartetes war(en)
    
    - Die Anwendung wird ein Operator beendet; verwenden beispielsweise im Azure-Portal.

    - Die app ist nicht mehr geladen, da es im Leerlauf war. (Nur, wenn die `Always On` Einstellung deaktiviert ist.)

- Unerwartete Beenden

    - Die app stürzt ab.

    - Eine Instanz der App-Dienst virtueller Computer ist nicht mehr verfügbar.


Protokollierung Application_End abgefangen das app-Domäne beenden (weiche Prozess stürzt ab) und ist die einzige Möglichkeit, um die Anwendung Domäne Abschaltvorgänge abzufangen.

**Wiederherstellung**

- Wenn das Beenden erwartet wurde, mithilfe der Anwendung war(en) Ereignisses ordnungsgemäße Schließen. Beispielsweise in ASP.NET, verwenden Sie die `Application_End` Methode.

- Wenn die Anwendung entfernt im Leerlauf war, wird er bei der nächsten Anforderung automatisch neu gestartet. Sie werden jedoch "kalt starten" Kosten entstehen. 

- Um zu verhindern, dass die Anwendung von entladenen im Leerlauf, aktivieren Sie die `Always On` in der Web-app festlegen. [Konfigurieren von Web apps in Azure-App-Dienst]finden Sie unter[app-service-configure].

- Um zu verhindern, dass einen Operator die app beendet, legen Sie die Sperre für eine Ressource mit `ReadOnly` Ebene. [Sperrenressourcen Azure Ressourcenmanager]finden Sie unter[rm-locks].

- Wenn die app stürzt ab oder eine App Dienst virtueller Computer zur Verfügung steht, App-Dienst wird automatisch die app neu gestartet. 


**Diagnose**. Anwendung Protokolle und Protokolle für Web-Server. [Aktivieren Sie das Diagnoseprotokoll für Web apps in Azure-App-Dienst]finden Sie unter[app-service-logging].


### <a name="a-particular-user-repeatedly-makes-bad-requests-or-overloads-the-system"></a>Ein bestimmter Benutzer wiederholt schlechten fordert oder das System überladen. 

**Erkennung**. Benutzerauthentifizierung und Benutzer-ID in der von Anwendungsprotokollen einbeziehen.

**Wiederherstellung**

- Verwenden von [Azure-API Management] [ api-management] auf Steuerung Anfragen vom Benutzer. [Erweiterte Anforderung begrenzungsebene mit Azure-API Management] finden Sie unter[api-management-throttling]

- Blockieren des Benutzers an.

**Diagnose**. Melden Sie sich alle Authentifizierungsanfragen.


### <a name="a-bad-update-was-deployed"></a>Eine fehlerhafte Aktualisierung bereitgestellt wurde.

**Erkennung**. Überwachen Sie die Anwendung Integrität über das Azure-Portal (finden Sie unter [Monitor Azure Web app Leistung][app-insights-web-apps]) oder die [Gesundheit Endpunkt Überwachung Muster]implementieren[health-endpoint-monitoring-pattern].

**Wiederherstellung**. Verwenden von mehreren [Bereitstellung Steckplätze] [ app-service-slots] und mit der letzten funktionierenden Bereitstellung zurücksetzen. Weitere Informationen finden Sie unter [grundlegende Web Anwendung Bezug Architektur][ra-web-apps-basic].


## <a name="azure-active-directory"></a>Azure-Active Directory

### <a name="openid-connect-oidc-authentication-fails"></a>OpenID verbinden (OIDC) Authentifizierung schlägt fehl.

**Erkennung**. Möglicher Fehlermodi umfassen:

1. Azure AD steht nicht zur Verfügung und kann nicht erreicht werden, da ein Netzwerkproblem. Umleitung an den Endpunkt Authentifizierung schlägt fehl, und die OIDC Middleware eine Ausnahme auslösen.
2. Azure AD-Mandanten ist nicht vorhanden. Umleitung an den Endpunkt Authentifizierung gibt eines HTTP-Fehlercodes und die Middleware OIDC eine Ausnahme auslösen.
3. Benutzer kann keine Authentifizierung. Keine Erkennung Strategie ist erforderlich. Azure AD übernimmt Anmeldefehler.

**Wiederherstellung**

1. Fangen Sie Ausnahmefehler aus der Middleware.
2. Verarbeitet `AuthenticationFailed` Ereignisse.
3. Leiten Sie den Benutzer zu einer Seite zurück.
4. Benutzer Wiederholungsversuche.


## <a name="azure-search"></a>Azure suchen

### <a name="writing-data-to-azure-search-fails"></a>Schreiben von Daten in Azure Suche schlägt fehl.

**Erkennung**. Fangen `Microsoft.Rest.Azure.CloudException` Fehler.

**Wiederherstellung**

Die [Suche .NET SDK] [ search-sdk] automatisch nach vorübergehende Fehler wiederholt. Alle Ausnahmen vom Client SDK sollten als fehlerhaft dauerhaftes behandelt werden.

Die Standardrichtlinie "Wiederholen" verwendet exponentiellen Back deaktivieren. Zum Verwenden einer anderen "Wiederholen" Richtlinie rufen `SetRetryPolicy` auf die `SearchIndexClient` oder `SearchServiceClient` Class. Weitere Informationen finden Sie unter [Automatische Wiederholungsversuche][auto-rest-client-retry].

**Diagnose**. Verwenden der [Suche Datenverkehr Analytics][search-analytics].


### <a name="reading-data-from-azure-search-fails"></a>Lesen von Daten aus Azure Suche schlägt fehl.

**Erkennung**. Fangen `Microsoft.Rest.Azure.CloudException` Fehler.

**Wiederherstellung**

Die [Suche .NET SDK] [ search-sdk] automatisch nach vorübergehende Fehler wiederholt. Alle Ausnahmen vom Client SDK sollten als fehlerhaft dauerhaftes behandelt werden.

Die Standardrichtlinie "Wiederholen" verwendet exponentiellen Back deaktivieren. Zum Verwenden einer anderen "Wiederholen" Richtlinie rufen `SetRetryPolicy` auf die `SearchIndexClient` oder `SearchServiceClient` Class. Weitere Informationen finden Sie unter [Automatische Wiederholungsversuche][auto-rest-client-retry].

**Diagnose**. Verwenden der [Suche Datenverkehr Analytics][search-analytics].



## <a name="cassandra"></a>Cassandra


### <a name="reading-or-writing-to-a-node-fails"></a>Lesen und Schreiben in einem Knoten schlägt fehl.

**Erkennung**. Fangen Sie die Ausnahme. In .NET Clients, ist das in der Regel `System.Web.HttpException`. Andere Client möglicherweise andere Ausnahme aufweisen.  Weitere Informationen finden Sie unter [Cassandra Fehlerbehandlung ist ganz einfach](http://www.datastax.com/dev/blog/cassandra-error-handling-done-right).

**Wiederherstellung**

- Jeder [Cassandra Client](https://wiki.apache.org/cassandra/ClientOptions) verfügt über eine eigene "Wiederholen" Richtlinien und Funktionen. Weitere Informationen finden Sie unter [Cassandra Fehlerbehandlung ist ganz einfach][cassandra-error-handling].
- Verwenden einer bereitstellungs auf den Shapes für Gestelle-fähige mit Datenknoten auf den Fehlerstrukturanalyse-Domänen verteilt.
- Bereitstellen Sie auf mehrere Bereiche mit lokales Quorum Konsistenz ein. Wenn ein dauerhaftes Fehler auftritt, über eine andere Region fehl.

**Diagnose**. Von Anwendungsprotokollen


## <a name="cloud-service"></a>Cloud-Dienst

### <a name="web-or-worker-roles-are-unexpectedlybeing-shut-down"></a>Web oder Arbeitskollegen Rollen werden unerwartet beendet wurde.

**Erkennung**. Die [RoleEnvironment.Stopping] [ RoleEnvironment.Stopping] wird ausgelöst.

**Wiederherstellung**. Überschreiben Sie die [RoleEntryPoint.OnStop] [ RoleEntryPoint.OnStop] Methode, um ordnungsgemäß aufräumen. Weitere Informationen finden Sie unter [Der rechts-Methode zum Behandeln von Azure OnStop Ereignisse] [ onstop-events] (Blog).


## <a name="documentdb"></a>DocumentDB

### <a name="reading-data-from-documentdb-fails"></a>Lesen von Daten aus DocumentDB schlägt fehl.

**Erkennung**. Fangen `System.Net.Http.HttpRequestException` oder `Microsoft.Azure.Documents.DocumentClientException`. 

**Wiederherstellung**

- Das SDK wiederholt automatisch fehlgeschlagene Versuche. Zum Festlegen der Anzahl der Wiederholungsversuche und die maximale Wartezeit konfigurieren `ConnectionPolicy.RetryOptions`. Ausnahmen, die der Client auslöst befinden sich über die Richtlinie "Wiederholen" oder sind nicht vorübergehende Fehler. 

- DocumentDB den Client Steuerung, liefert einen Fehler 429 HTTP. Überprüfen Sie den Statuscode die `DocumentClientException`. Wenn Fehler 429 konsistente wiedergegeben werden, sollten Sie die Durchsatzwert der Sammlung DocumentDB zunehmender.

- Repliziert die Datenbank DocumentDB über zwei oder mehr Bereiche an. Alle Replikate werden gelesen. Verwenden den Kunden SDKs, geben Sie die `PreferredLocations` Parameter. Dies ist eine sortierte Liste von Azure Regionen. Der erste verfügbaren Bereich in der Liste werden alle liest gesendet werden. Wenn die Anforderung fehlschlägt, versucht der Client die anderen Bereiche in der Liste in der Reihenfolge. Weitere Informationen finden Sie unter [Entwickeln mit mehreren Region DocumentDB Konten][docdb-multi-region].

**Diagnose**. Melden Sie sich alle Fehler auf dem Client.


### <a name="writing-data-to-documentdb-fails"></a>Schreiben von Daten in DocumentDB fehlschlägt aus.

**Erkennung**. Fangen `System.Net.Http.HttpRequestException` oder `Microsoft.Azure.Documents.DocumentClientException`. 

**Wiederherstellung**

- Das SDK wiederholt automatisch fehlgeschlagene Versuche. Zum Festlegen der Anzahl der Wiederholungsversuche und die maximale Wartezeit konfigurieren `ConnectionPolicy.RetryOptions`. Ausnahmen, die der Client auslöst befinden sich über die Richtlinie "Wiederholen" oder sind nicht vorübergehende Fehler. 

- DocumentDB den Client Steuerung, liefert einen Fehler 429 HTTP. Überprüfen Sie den Statuscode die `DocumentClientException`. Wenn Fehler 429 konsistente wiedergegeben werden, sollten Sie die Durchsatzwert der Sammlung DocumentDB zunehmender.

- Repliziert die Datenbank DocumentDB über zwei oder mehr Bereiche an. Wenn die primäre Region ein Fehler auftritt, wird eine andere Region zum Schreiben einrichten. Sie können auch einen Failover manuell auslösen. Das SDK unterstützt automatische Erkennung und routing, damit die Anwendungscode weiterhin funktionsfähig nach einem Failover. Zeitraum (normalerweise Minuten) können Vorgänge höhere Verarbeitungszeit sind, wie das SDK die neuen schreiben Region findet. Weitere Informationen finden Sie unter [Entwickeln mit mehreren Region DocumentDB Konten][docdb-multi-region].

- Als eine Alternative das Dokument an eine Sicherung Warteschlange beibehalten und die Warteschlange später zu verarbeiten.

**Diagnose**. Melden Sie sich alle Fehler auf dem Client.


## <a name="elasticsearch"></a>Elasticsearch

### <a name="reading-data-from-elasticsearch-fails"></a>Lesen von Daten aus Elasticsearch schlägt fehl.

**Erkennung**. Fangen Sie die entsprechende Ausnahme für den bestimmten [Elasticsearch Client] [ elasticsearch-client] verwendet wird. 

**Wiederherstellung**

- Verwenden Sie ein Verfahren "Wiederholen". Jeder Client verfügt über eine eigene Richtlinien "Wiederholen". 

- Bereitstellen von mehreren Elasticsearch Knoten und Replikation für hohe Verfügbarkeit verwenden.

Weitere Informationen finden Sie unter [Ausführen von Elasticsearch auf Azure][elasticsearch-azure].

**Diagnose**. Sie können Überwachungstools für Elasticsearch verwenden, oder melden Sie sich alle Fehler auf dem Client mit der Nutzlast. Finden Sie im Abschnitt 'Überwachen' in [Elasticsearch ausgeführt wird, klicken Sie auf Azure][elasticsearch-azure].


### <a name="writing-data-to-elasticsearch-fails"></a>Schreiben von Daten in Elasticsearch fehlschlägt aus.

**Erkennung**. Fangen Sie die entsprechende Ausnahme für den bestimmten [Elasticsearch Client] [ elasticsearch-client] verwendet wird.  

**Wiederherstellung**

- Verwenden Sie ein Verfahren "Wiederholen". Jeder Client verfügt über eine eigene Richtlinien "Wiederholen". 
 
- Wenn die Anwendung eine reduzierten Konsistenz Ebene tolerieren kann, sollten Sie sich mit dem Schreiben mit `write_consistency` Festlegen der `quorum`.

Weitere Informationen finden Sie unter [Ausführen von Elasticsearch auf Azure][elasticsearch-azure].

**Diagnose**. Sie können Überwachungstools für Elasticsearch verwenden, oder melden Sie sich alle Fehler auf dem Client mit der Nutzlast. Finden Sie im Abschnitt 'Überwachen' in [Elasticsearch ausgeführt wird, klicken Sie auf Azure][elasticsearch-azure].


## <a name="queue-storage"></a>Warteschlangenspeicher

### <a name="writing-a-message-to-azure-queue-storage-fails-consistently"></a>Schreiben eine Nachricht an Azure Warteschlange Speicher fehlschlägt konsistent.

**Erkennung**. Nach *N* Wiederholungsversuche, schlägt der Schreibvorgang weiterhin.

**Wiederherstellung**

- Speichern Sie die Daten in einem lokalen Cache, und Weiterleiten Sie die schreibt zu Speicher später, wenn der Dienst verfügbar ist.
- Erstellen einer sekundären Warteschlange und in die Warteschlange schreiben, wenn die primäre Warteschlange nicht verfügbar ist.

**Diagnose**. [Kennzahlen Speicher]verwenden[storage-metrics].


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Die Anwendung kann keine bestimmte Nachricht aus der Warteschlange verarbeitet werden. 

**Erkennung**. Anwendungsspezifische. Angenommen, die Nachricht enthält ungültige Daten oder die Geschäftslogik aus irgendeinem Grund fehlschlägt. 

**Wiederherstellung**

Verschieben der Nachricht in einer separaten Warteschlange. Führen Sie einen separaten Prozess zum Untersuchen der Nachrichten in der Warteschlange.

Bietet Azure Service Bus Messaging Warteschlangen die bietet eine [unzustellbare] [ sb-dead-letter-queue] Funktionalität für diesen Zweck.

Hinweis: Wenn Sie WebJobs Speicher Warteschlangen mit, das WebJobs SDK umfasst integrierte beschädigte Nachrichtenbehandlung. Finden Sie unter [Verwenden von Azure Warteschlangenspeicher mit dem SDK WebJobs][sb-poison-message].

**Diagnose**. Verwenden Sie die Anwendung Protokollierung.


## <a name="redis-cache"></a>Redis Cache

### <a name="reading-from-the-cache-fails"></a>Lesebereich aus dem Cache schlägt fehl.

**Erkennung**. Fangen `StackExchange.Redis.RedisConnectionException`.

**Wiederherstellung**

1. Wiederholen Sie die vorübergehende Fehler. Azure Redis Cache unterstützt integrierte "Wiederholen" finden Sie unter [Cache Redis "Wiederholen" Richtlinien]durchgehen[redis-retry].
2. Dauerhaftes Fehlern als ein Cache entgehen behandeln und wieder zur ursprünglichen Datenquelle liegen.

**Diagnose**. Verwenden der [Cache Redis Diagnose][redis-monitor].


### <a name="writing-to-the-cache-fails"></a>Das Schreiben in den Cache schlägt fehl.

**Erkennung**. Fangen `StackExchange.Redis.RedisConnectionException`.

**Wiederherstellung**

1. Wiederholen Sie die vorübergehende Fehler. Azure Redis Cache unterstützt integrierte "Wiederholen" finden Sie unter [Cache Redis "Wiederholen" Richtlinien]durchgehen[redis-retry].
2. Ist der Fehler dauerhaftes, ignorieren Sie es, und lassen Sie andere Transaktionen, die später in den Cache zu schreiben.

**Diagnose**. Verwenden der [Cache Redis Diagnose][redis-monitor].


## <a name="sql-database"></a>SQL-Datenbank

### <a name="cannot-connect-to-the-database-in-the-primary-region"></a>In der Datenbank in der primären Region kann keine Verbindung herstellen.

**Erkennung**. Verbindung schlägt fehl.

**Wiederherstellung**

Voraussetzung: Die Datenbank muss für aktive Geo-Replikation konfiguriert sein. [SQL-aktive Geo Datenbankreplikation]finden Sie unter[sql-db-replication].

- Lesen Sie für Abfragen von einer sekundären Replikation. 
- Einfügen und Updates manuell fehl über an einer sekundären. [Einleiten einer geplanten oder ungeplanten Failover für SQL Azure-Datenbank]finden Sie unter[sql-db-failover].

Das Replikat verwendet eine andere Verbindungszeichenfolge, damit Sie die Verbindungszeichenfolge in Ihrer Anwendung aktualisieren müssen.


### <a name="client-runs-out-of-connections-in-the-connection-pool"></a>Client aus Verbindungen im Verbindungspool ausgeführt wird.

**Erkennung**. Fangen `System.InvalidOperationException` Fehler. 

**Wiederherstellung**

- Wiederholen Sie den Vorgang.
- Als eine entschärfungsplan isolieren Sie Verbindungspools für jeden Fall verwenden, damit eine Anwendungsfall-diese Verbindungen dominieren nicht möglich.
- Erhöhen Sie die maximale Verbindungspools.

**Diagnose**. Anwendung Protokolle.


### <a name="database-connection-limit-is-reached"></a>Datenbank Verbindung Grenzwert erreicht ist. 

**Erkennung**. Azure SQL-Datenbank beschränkt die Anzahl der gleichzeitigen Kollegen, Benutzernamen und Sitzungen. Die Grenzwerte abhängig von der Dienstebene. Weitere Informationen finden Sie unter [Azure SQL-Datenbank Ressource Grenzwerte][sql-db-limits].

Zum Ermitteln von diesen Fehlern, abzufangen `System.Data.SqlClient.SqlException` und überprüfen Sie den Wert der `SqlException.Number` für den SQL-Code zurück. Eine Liste der entsprechenden Fehlercodes finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbank-Verbindungsfehler und andere Probleme][sql-db-errors].

**Wiederherstellung**. Dieser Fehler werden vorübergehend betrachtet Wiederholung dieses Problem behoben werden kann. Wenn Sie diese Fehler konsistent getroffen haben, sollten Sie die Skalierung der Datenbank.

**Diagnose**. -Der [sys.event_log] [ sys.event_log] Abfrage gibt erfolgreich Datenbankverbindungen, Verbindungsfehlern und Deadlocks.

- Erstellen Sie eine [Regel] [ azure-alerts] für Fehler bei Verbindungen.

- Aktivieren Sie die [SQL-Datenbank Überwachung] [ sql-db-audit] und fehlgeschlagener überprüfen.


## <a name="service-bus-messaging"></a>Service Bus Messaging

### <a name="reading-a-message-from-a-service-bus-queue-fails"></a>Lesen einer Nachricht aus einer Warteschlange Dienstbus schlägt fehl.

**Erkennung**. Fangen Sie Ausnahmen von der Client SDK. Die base Klasse für Dienstbus Ausnahmen ist [MessagingException][sb-messagingexception-class]. Ist der Fehler vorübergehend der `IsTransient` -Eigenschaft true ist. 

Weitere Informationen finden Sie unter [Dienstbus messaging Ausnahmen][sb-messaging-exceptions].

**Wiederherstellung**

1. Wiederholen Sie die vorübergehende Fehler. Finden Sie unter [Richtlinien wiederholen Dienstbus][sb-retry].

2. Nachrichten, die an alle Empfänger übermittelt werden können, werden in einer *unzustellbare*platziert. Verwenden Sie diese Warteschlange sehen, welche Nachrichten nicht empfangen werden konnte. Es gibt keine automatische Aufräumen der Warteschlange unzustellbare aus. Nachrichten bleiben dort auf, bis Sie explizit abrufen. Finden Sie unter [Übersicht der unzustellbare Servicebuswarteschlangen][sb-dead-letter-queue].


### <a name="writing-a-message-to-a-service-bus-queue-fails"></a>Schreiben eine Nachricht mit einem Dienst schlägt Warteschlange.

**Erkennung**. Fangen Sie Ausnahmen von der Client SDK. Die base Klasse für Dienstbus Ausnahmen ist [MessagingException][sb-messagingexception-class]. Ist der Fehler vorübergehend der `IsTransient` -Eigenschaft true ist. 

Weitere Informationen finden Sie unter [Dienstbus messaging Ausnahmen][sb-messaging-exceptions].

**Wiederherstellung**

1. Der Client Dienstbus wiederholt automatisch nach vorübergehende Fehler auf. Standardmäßig wird verwendet exponentiellen Back deaktivieren. Nachdem Sie die maximale Anzahl oder maximale Zeitlimit Ausnahme der Client eine. Weitere Informationen finden Sie unter [Richtlinien wiederholen Dienstbus][sb-retry].

2. Wenn das Warteschlangenkontingent überschritten wird, löst der Client [QuotaExceededException][QuotaExceededException]. Die Ausnahmemeldung bietet weitere Details an. Einige Nachrichten aus der Warteschlange abzuleiten, bevor Sie wiederholen, und bietet das Sicherung Muster kontinuierliche Wiederholungsversuche zu vermeiden, während das Kontingent überschritten wird. Darüber hinaus stellen Sie sicher, dass die Eigenschaft [BrokeredMessage.TimeToLive] nicht zu hoch festgelegt ist. 

3. Innerhalb eines Bereichs, Stabilität verbessert werden kann, mithilfe von [partitionierten Warteschlangen oder Themen][sb-partition]. Eine nicht aufgeteilt Warteschlange oder ein Thema wird eine messaging Store zugewiesen. Wenn diese messaging Store nicht verfügbar ist, werden alle Vorgänge in der Warteschlange oder das Thema fehl. Eine partitionierte Warteschlange oder ein Thema wird über mehrere messaging Stores aufgeteilt. 

4. Zusätzliche Stabilität erstellen Sie zwei Dienstbus Namespaces in verschiedenen Regionen, und repliziert die Nachrichten. Sie können entweder active Replikation oder passiven Replikation verwenden.

    - Aktive Replikation: der Client sendet jeder Nachricht an beide Warteschlangen. Der Empfänger hört beide Warteschlangen aus. Markieren von Nachrichten mit einem eindeutigen Bezeichner, der Client doppelte Nachrichten verwerfen kann.

    - Passive Replikation: der Client sendet die Nachricht an eine Warteschlange. Ist ein Fehler, greift der Client auf die anderen Warteschlange zurück. Der Empfänger hört beide Warteschlangen aus. Dieser Ansatz reduziert die Anzahl der doppelten Nachrichten, die gesendet werden. Allerdings muss der Empfänger noch doppelte Nachrichten behandeln.

    Weitere Informationen finden Sie unter [GeoReplication Beispiel] [ sb-georeplication-sample] und [bewährte Methoden für Hohlkörperelementen Applikationen gegen Dienstbus Ausfall und Schäden][sb-outages].


### <a name="duplicate-message"></a>Doppelte Nachricht.

**Erkennung**. Untersuchen der `MessageId` und `DeliveryCount` Eigenschaften der Nachricht.

**Wiederherstellung**

- Entwerfen Sie Aussage Verarbeitung von Vorgängen auf Idempotent sein, falls möglich. Speichern Sie andernfalls Nachrichten-IDs der Nachrichten, die bereits verarbeitet werden, und aktivieren Sie die ID, die vor der Verarbeitung einer Nachricht ein.

- Aktivieren der Erkennung von Duplikaten, durch das Erstellen der Warteschlange mit `RequiresDuplicateDetection` auf True festgelegt ist. Mit dieser Einstellung Dienstbus automatisch löscht alle Nachrichten, die mit dem gleichen gesendet wird `MessageId` als vorherigen Nachricht.  Beachten Sie Folgendes:

    -  Mit dieser Einstellung wird verhindert, dass doppelte Nachrichten werden in der Warteschlange setzen. Es verhindern nicht, dass einen Empfänger dieselbe Nachricht mehrmals verarbeiten.

    - Doppelte Erkennung hat einen Zeitrahmen fest. Wenn ein Duplikat neben diesem Fenster gesendet wird, wird nicht sie erkannt werden.

**Diagnose**. Melden Sie sich duplizierte Nachrichten.


### <a name="the-application-cannot-process-a-particular-message-from-the-queue"></a>Die Anwendung kann keine bestimmte Nachricht aus der Warteschlange verarbeitet werden. 

**Erkennung**. Anwendungsspezifische. Angenommen, die Nachricht enthält ungültige Daten oder die Geschäftslogik aus irgendeinem Grund fehlschlägt. 

**Wiederherstellung**

Es gibt zwei Fehlermodi zu berücksichtigen ist. 

- Der Empfänger erkennt den Fehler an. In diesem Fall verschieben Sie die Nachricht an die Warteschlange unzustellbare. Führen Sie einen separaten Prozess zum Untersuchen der Nachrichten in der Warteschlange unzustellbare später.

- Der Empfänger in der Mitte der Verarbeitung der Nachricht fehlschlägt &mdash; z. B. aufgrund eines Ausnahmefehler. Verwenden Sie zum Behandeln von diesem Fall `PeekLock` Modus. Wenn die Sperre abgelaufen ist, wird die Nachricht in diesem Modus für andere Empfänger verfügbar. Wenn die Nachricht die Übermittlung maximale Anzahl oder die Time-to-live überschreitet, wird die Nachricht automatisch an die unzustellbare Warteschlange verschoben.

Weitere Informationen finden Sie unter [Übersicht der unzustellbare Servicebuswarteschlangen][sb-dead-letter-queue].

**Diagnose**. Immer, wenn die Anwendung eine Nachricht in der Warteschlange unzustellbare verschiebt, Schreiben Sie ein Ereignis in die Anwendungsprotokolle.


## <a name="service-fabric"></a>Dienst Fabric

### <a name="a-request-to-a-service-fails"></a>Eine Anforderung an einen Dienst schlägt fehl.

**Erkennung**. Dienst wird einen Fehler zurückgegeben.

**Wiederherstellung**

- Suchen nach einem Proxy erneut (`ServiceProxy` oder `ActorProxy`), und rufen Sie die Methode Service/Akteur erneut. 

- **Dynamische Dienst**. Umbrechen von zuverlässigen Websitesammlungen Vorgänge in einer Transaktion. Wenn ein Fehler vorliegt, wird die Transaktion zurückgesetzt. Die Anforderung, wird, wenn eine Warteschlange entnommen erneut verarbeitet werden. 
 
- **Statusfreie Dienst**. Wenn der Dienst Daten auf einem externen Speicher weiterhin besteht, müssen alle Vorgänge Idempotent sein.


**Diagnose**. Anwendungsprotokoll

### <a name="service-fabric-node-is-shut-down"></a>Dienst Fabric Knoten abgeschaltet ist.

**Erkennung**. Ein Abbruchtoken wird an den Dienst übergeben `RunAsync` Methode. Dienst Fabric bricht den Vorgang ab, vor dem Knoten beendet.

**Wiederherstellung**. Verwenden des Kündigung Tokens war(en) erkennen. Beim Dienst Fabric Abbruch anfordert, beenden Sie etwaige Änderungen, und beenden Sie `RunAsync` so schnell wie möglich.

**Diagnose**. Von Anwendungsprotokollen


## <a name="storage"></a>Speicher

### <a name="writing-data-to-azure-storage-fails"></a>Schreiben von Daten in Azure-Speicher schlägt fehl

**Erkennung**. Der Client erhält Fehler beim Erstellen.

**Wiederherstellung**

1. Wiederholen Sie den Vorgang, um aus vorübergehende Fehler wiederherzustellen. Die [Richtlinie wiederholen] [ Storage.RetryPolicies] im Client SDK führt dies automatisch.
2. Implementieren des Schutzschalter Musters um überwältigenden Speicher zu vermeiden.
3. Wenn N Versuche fehlschlagen, führen Sie ein sicheres Ersatz aus. Beispiel:

    - Speichern Sie die Daten in einem lokalen Cache, und Weiterleiten Sie die schreibt zu Speicher später, wenn der Dienst verfügbar ist.
    - Wenn die Aktion Schreiben in einen Bereich Transaktionen wurde, kompensieren Sie die Transaktion.

**Diagnose**. [Kennzahlen Speicher]verwenden[storage-metrics].


### <a name="reading-data-from-azure-storage-fails"></a>Lesen von Daten aus Azure-Speicher schlägt fehl.

**Erkennung**. Der Client erhält Fehler beim Lesen.

**Wiederherstellung**

1. Wiederholen Sie den Vorgang, um aus vorübergehende Fehler wiederherzustellen. Die [Richtlinie wiederholen] [ Storage.RetryPolicies] im Client SDK führt dies automatisch.
2. RAS GRS Speicher Wenn der Lesebereich aus der primären Endpunkt fehlschlägt, versuchen Sie Lesebereich aus den sekundären Endpunkt. Das Client-SDK kann dies automatisch verarbeiten. Finden Sie unter [Replikation Azure-Speicher][storage-replication].
3. Ist *N* Versuche fehlschlagen, Aktion ein alternatives ordnungsgemäß eingeschränkt. Beispielsweise ein Produktbild aus dem Speicher abgerufen werden kann, zeigen Sie ein Platzhalterbild generische an.

**Diagnose**. [Kennzahlen Speicher]verwenden[storage-metrics].


## <a name="virtual-machine"></a>Virtuellen Computern

### <a name="connection-to-a-backend-vm-fails"></a>Verbindung mit einer Back-End-virtuellen Computer schlägt fehl.

**Erkennung**. Fehler bei der Netzwerk-Verbindung.

**Wiederherstellung**

- Bereitstellen von mindestens zwei Back-End-virtuellen Computern in einer Gruppe von Verfügbarkeit, hinter einem Lastenausgleich.

- Ist der Verbindungsfehler vorübergehende wiederholt manchmal TCP erfolgreich die Nachricht gesendet wird. 

- Implementieren Sie eine Richtlinie "Wiederholen" in der Anwendung. 

- Beständiger oder dauerhaftes Fehler Implementieren der [Schutzschalter] [ circuit-breaker] Muster.

- Der einen virtuellen Computer das Netzwerk Ausgang Limit überschreitet, wird die Warteschlange für ausgehende überfüllt. Wenn die Warteschlange für ausgehende konsistente voll ist, sollten Sie die Skalierung. 

**Diagnose**. Melden Sie sich am Dienst Begrenzung Ereignisse.

### <a name="vm-instance-becomes-unavailable-or-unhealthy"></a>Instanz virtueller Computer ist nicht verfügbar oder fehlerhaft.

**Erkennung**. Konfigurieren Sie einen Lastenausgleich [Gesundheit Prüfpunkt] [ lb-probe] , das zeigt, ob die Instanz virtueller Computer fehlerfrei ist. Der Prüfpunkt sollten, ob kritische Funktionen ordnungsgemäß reagiert werden. 

**Wiederherstellung**. Klicken Sie für jede Anwendungsebene setzen Sie mehrerer Instanzen von virtuellen Computer in demselben Satz Verfügbarkeit, und platzieren Sie ein Lastenausgleich vor der virtuellen Computern. Wenn die Überprüfung Gesundheit fehlschlägt, beendet Lastenausgleich das Senden von neuer Verbindungen den fehlerhaften Instanz.

**Diagnose**. -Verwenden Sie Lastenausgleich [Log Analytics][lb-monitor].
- Überwachung-System zum Überwachen der alle die Endpunkte Überwachung Integrität konfiguriert.


### <a name="operator-accidentally-shuts-down-a-vm"></a>Operator beendet versehentlich eines virtuellen Computers.

**Erkennung**. N/V

**Wiederherstellung**. Legen Sie die Sperre für eine Ressource mit `ReadOnly` Ebene. [Sperrenressourcen Azure Ressourcenmanager]finden Sie unter[rm-locks].

**Diagnose**. Verwenden von [Azure Aktivitäten protokolliert][azure-activity-logs].


## <a name="webjobs"></a>WebJobs

### <a name="continuous-job-stops-running-when-the-scm-host-is-idle"></a>Fortlaufender Auftrag angehalten wird, ausgeführt, wenn der SCM Host im Leerlauf ist.

**Erkennung**. Übergeben Sie ein Token Kündigung an die Funktion WebJob. Weitere Informationen finden Sie unter [sicheres war(en)][web-jobs-shutdown]. 

**Wiederherstellung**. Aktivieren der `Always On` in der Web-app festlegen. Weitere Informationen finden Sie unter [Ausführen von Hintergrundaufgaben mit WebJobs][web-jobs].


## <a name="application-design"></a>Entwerfen der Anwendung

### <a name="application-cant-handle-a-spike-in-incoming-requests"></a>Anwendung kann nicht auf eine Sammlung in eingehenden Anfragen behandeln.

**Erkennung**. Hängt die Anwendung. Typische Symptome:

- Die Website wird gestartet, HTTP-5xx Fehlercodes zurückgeben.
- Abhängige Dienste, wie z. B. Datenbank oder Speicher, starten Sie Besprechungsanfragen einschränken. Suchen Sie nach HTTP-Fehler wie HTTP 429 (zu viele Anfragen), je nach dem Dienst.
- HTTP-Warteschlangenlänge vergrößert wird.

**Wiederherstellung**

- Skalieren Sie erhöhte Last verarbeitet. 

- Minimieren Sie Fehler zu vermeiden, dass cascading Fehlern, die die gesamte Anwendung stören. Problembehebungsstrategien umfassen:

    - Implementieren der [Muster Begrenzungsebene] [ throttling-pattern] überwältigenden Back-End-Systeme zu vermeiden.
    - [Warteschlange-basierten laden Abgleich] verwenden[ queue-based-load-leveling] Puffer Anfragen und eine entsprechende Tempo zu verarbeiten.
    - Priorisieren von bestimmter Clients. Beispielsweise verfügt die Anwendung freie / kostenpflichtiges Ebenen, schränken Sie Kunden auf der kostenlosen Ebene, aber nicht bezahlt Kunden. [Priorität Warteschlange Muster]finden Sie unter[priority-queue-pattern].

**Diagnose**. Verwenden des [App-Verwaltungsdienst diagnostic logging][app-service-logging]. Verwenden Sie einen Dienst wie [Azure Log Analytics][azure-log-analytics], [Anwendung Einsichten][app-insights], oder [Neue Relic] [ new-relic] in den Diagnoseprotokollen zu verstehen.


### <a name="one-of-the-operations-in-a-workflow-or-distributed-transaction-fails"></a>Einer der Vorgänge in einem Workflow oder verteilte Transaktion schlägt fehl.

**Erkennung**. Nachdem *N* Wiederholungsversuche, fällt es weiter aus.

**Wiederherstellung**

- Als eine entschärfungsplan Implementieren der [Scheduler Agent Vorgesetzten] [ scheduler-agent-supervisor] Muster zum Verwalten des gesamten Workflows. 
- Wiederholen Sie nicht auf Zeitlimit. Es gibt eine niedrige Erfolg Abschlag (Disagio) dieser Fehler aus. 
- Warteschlange arbeiten müssen, um zu einem späteren Zeitpunkt wiederholen.

**Diagnose**. Melden Sie sich alle Vorgänge (erfolgreiche und fehlgeschlagene), einschließlich compensating Aktionen. Verwenden Sie Korrelations-IDs, damit Sie alle Vorgänge innerhalb derselben Transaktion verfolgen können.


### <a name="a-call-to-a-remote-service-fails"></a>Anruf an einen Remotedienst schlägt fehl.

**Erkennung**. HTTP-Fehlercode.

**Wiederherstellung**

1. Wiederholen Sie die vorübergehende Fehler. 
2. Wenn der Anruf nach fehlschlägt *N* versucht, ein alternatives agieren. (Anwendung bestimmte.)
3. Implementieren der [Schutzschalter Muster] [ circuit-breaker] cascading Fehler zu vermeiden. 

**Diagnose**. Melden Sie sich alle Fehler bei der remote-Anruf.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Prozess FMA, finden Sie unter [Stabilität standardmäßig für Cloud Services] [ resilience-by-design-pdf] (PDF-Download).

<!-- links -->

[api-management]: https://azure.microsoft.com/documentation/services/api-management/
[api-management-throttling]: ../api-management/api-management-sample-flexible-throttling.md
[app-insights]: ../application-insights/app-insights-overview.md
[app-insights-web-apps]: ../application-insights/app-insights-azure-web-apps.md
[app-service-configure]: ../app-service-web/web-sites-configure.md
[app-service-logging]: ../app-service-web/web-sites-enable-diagnostic-log.md
[app-service-slots]: ../app-service-web/web-sites-staged-publishing.md
[auto-rest-client-retry]: https://github.com/Azure/autorest/blob/master/Documentation/clients-retry.md
[azure-activity-logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[azure-alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md
[azure-log-analytics]: ../log-analytics/log-analytics-overview.md
[BrokeredMessage.TimeToLive]: https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
[cassandra-error-handling]: http://www.datastax.com/dev/blog/cassandra-error-handling-done-right
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[docdb-multi-region]: ../documentdb/documentdb-developing-with-multiple-regions.md
[elasticsearch-azure]: guidance-elasticsearch-running-on-azure.md
[elasticsearch-client]: https://www.elastic.co/guide/en/elasticsearch/client/index.html
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[onstop-events]: https://azure.microsoft.com/blog/the-right-way-to-handle-azure-onstop-events/
[lb-monitor]: ../load-balancer/load-balancer-monitor-log.md
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md#learn-about-the-types-of-probes
[new-relic]: https://newrelic.com/
[priority-queue-pattern]: https://msdn.microsoft.com/library/dn589794.aspx
[queue-based-load-leveling]: https://msdn.microsoft.com/library/dn589783.aspx
[QuotaExceededException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx
[ra-web-apps-basic]: guidance-web-apps-basic.md
[redis-monitor]: ../redis-cache/cache-how-to-monitor.md
[redis-retry]: ../best-practices-retry-service-specific.md#cache-redis-retry-guidelines
[resilience-by-design-pdf]: http://download.microsoft.com/download/D/8/C/D8C599A4-4E8A-49BF-80EE-FE35F49B914D/Resilience_by_Design_for_Cloud_Services_White_Paper.pdf
[RoleEntryPoint.OnStop]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[RoleEnvironment.Stopping]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx
[rm-locks]: ../resource-group-lock-resources.md
[sb-dead-letter-queue]: ../service-bus-messaging/service-bus-dead-letter-queues.md
[sb-georeplication-sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/GeoReplication
[sb-messagingexception-class]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
[sb-messaging-exceptions]: ../service-bus-messaging/service-bus-messaging-exceptions.md
[sb-outages]: ../service-bus/service-bus-outages-disasters.md#protecting-queues-and-topics-against-datacenter-outages-or-disasters
[sb-partition]: ../service-bus-messaging/service-bus-partitioning.md
[sb-poison-message]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#poison
[sb-retry]: ../best-practices-retry-service-specific.md#service-bus-retry-guidelines
[search-sdk]: https://msdn.microsoft.com/library/dn951165.aspx
[scheduler-agent-supervisor]: https://msdn.microsoft.com/library/dn589780.aspx
[search-analytics]: ../search/search-traffic-analytics.md
[sql-db-audit]: ../sql-database/sql-database-auditing-get-started.md
[sql-db-errors]: ../sql-database/sql-database-develop-error-messages.md#resource-governanance-errors
[sql-db-failover]: ../sql-database/sql-database-geo-replication-failover-portal.md
[sql-db-limits]: ../sql-database/sql-database-resource-limits.md
[sql-db-replication]: ../sql-database/sql-database-geo-replication-overview.md
[storage-metrics]: https://msdn.microsoft.com/library/dn782843.aspx
[storage-replication]: ../storage/storage-redundancy.md
[Storage.RetryPolicies]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.aspx
[sys.event_log]: https://msdn.microsoft.com/library/dn270018.aspx
[throttling-pattern]: https://msdn.microsoft.com/library/dn589798.aspx
[web-jobs]: ../app-service-web/web-sites-create-web-jobs.md
[web-jobs-shutdown]: ../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful

