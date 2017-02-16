<properties
   pageTitle="Hinzufügen von benutzerdefinierten Dienst Fabric Integritätsberichte | Microsoft Azure"
   description="Beschreibt, wie benutzerdefinierte Integritätsberichte für Azure Service Fabric Gesundheit Personen zu senden. Bietet Empfehlungen für das Entwerfen und Implementieren der Qualität Verwendungsberichte an."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="add-custom-service-fabric-health-reports"></a>Hinzufügen von benutzerdefinierten Dienst Fabric Verwendungsberichte
Azure Service-Struktur bietet ein [Modell Gesundheit](service-fabric-health-introduction.md) fehlerhafte Cluster Geschäftsbedingungen Anwendung auf bestimmte Personen zu kennzeichnen. Das Modell Gesundheit verwendet **Gesundheit Reporter** (Systemkomponenten und Watchdogs). Das Ziel ist einfach und schnell Diagnose und Reparatur. Dienst Autoren müssen im Vorfeld zu Gesundheit vorstellen. Ein beliebiger ist die Gesundheit beeinflussen kann sollten, gemeldet werden, insbesondere dann, wenn es Kennzeichnung Probleme in der Nähe der Stammwebsitesammlung helfen kann. Die Statusinformationen ein viel Zeit sparen kann und Aufwand für das Debuggen und Untersuchung einmal der Dienst ist einsatzbereit bei in der Cloud (private oder Azure).

Monitor für den Dienst Fabric-Berichterstatter gekennzeichnet relevante Bedingungen. Sie Erstellen von Berichten diese Bedingung basierend auf deren lokalen angezeigt. Der [Dienststatus speichern](service-fabric-health-introduction.md#health-store) aggregiert Gesundheitsdaten, die von allen Berichterstatter gesendet, um zu bestimmen, ob Personen Global fehlerfrei sind. Das Modell erhebt auf Rich-, flexible und einfach zu verwenden. Die Qualität der Integritätsberichte bestimmt die Genauigkeit der Ansicht Gesundheit Cluster. Falsche, die fälschlicherweise fehlerhaften Probleme anzeigen können beeinträchtigen Upgrades oder andere Dienste, die Gesundheitsdaten verwenden. Beispiele für diese Dienste sind reparieren Services und Alarmmechanismen. Daher wird Gedanken über das benötigt, zur Bereitstellung von Berichten, die Bedingungen relevante in optimal zu erfassen.

Zum Entwerfen und implementieren Berichte Gesundheit watchdogs und Systemkomponenten müssen:

- Definieren Sie die Bedingung, die, der Sie interessiert sind, die Möglichkeit, die sie überwacht wird und den Einfluss auf die Funktionalität Cluster oder einer Anwendung. Anhand dieser Informationen, legen Sie Berichtseigenschaft Gesundheit und Integritätsstatus.

- Ermitteln Sie die [Entität](service-fabric-health-introduction.md#health-entities-and-hierarchy) , die auf der Bericht angewendet wird.

- Bestimmen, wo finde ich die reporting fertig, von innerhalb des Diensts oder aus einer internen oder externen Watchdog.

- Definieren einer Quelle verwendet, um die Reporter zu identifizieren.

- Wählen Sie eine Strategie reporting regelmäßig oder klicken Sie auf Übergänge aus. Es wird empfohlen regelmäßig, da es einfacher Code erfordert und weniger häufig Fehler ist.

- Bestimmen Sie, wie lange der Bericht Fehlerhafte Bedingungen im Speicher Gesundheit bleiben soll, und wie sie gelöscht werden sollen. Entscheiden Sie anhand dieser Informationen des Berichts-Zeit, live und entfernen auf Ablauf Verhalten.

Wie erwähnt, kann die reporting von vorgenommen werden:

- Das überwachte Dienst Fabric Service Replikat.

- Interner Watchdogs als Dienst Fabric Service (beispielsweise ein Dienst Fabric statusfreie Dienst, überwacht Bedingungen und Berichte Probleme) bereitgestellt. Die Watchdogs werden können eine alle Knoten bereitgestellt oder zum überwachten Dienst affinitized werden können.

- Interne Watchdogs, die auf den Dienst Fabric Knoten ausführen, aber *nicht* als Dienst Fabric Services implementiert.

- Externe Watchdogs, die die Ressource von *außerhalb* der Dienst Fabric Cluster (z. B. Überwachung Service wie Gomez) Prüfpunkt.

> [AZURE.NOTE] Außerhalb des im Feld wird der Cluster mit Integritätsberichte gesendet werden von den Systemkomponenten aufgefüllt. Weitere Informationen finden Sie am [verwenden System Verwendungsberichte zur Behandlung dieses Problems](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). Die Benutzerberichte müssen gesendet werden, auf [Systemzustand Personen](service-fabric-health-introduction.md#health-entities-and-hierarchy) , die bereits vom System erstellt wurden.

Einmal können die Integrität reporting, dass klar ist, Entwurf, Integritätsberichte einfach gesendet werden. Sie können [FabricClient](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.aspx) Bericht Gesundheit Wenn Cluster nicht [sichere](service-fabric-cluster-security.md) oder wenn der Fabric-Client admininistratorberechtigungen verfügt. Dies kann über die API mithilfe von [FabricClient.HealthManager.ReportHealth](https://msdn.microsoft.com/library/system.fabric.fabricclient.healthclient.reporthealth.aspx), über PowerShell oder durch REST erfolgen. Konfiguration Regler Stapel Berichte zum Verbessern der Leistung.

> [AZURE.NOTE] Bericht Gesundheit ist synchron, und nur die Überprüfung Arbeit clientseitig darstellt. Die Fakultät, dass der Bericht durch den Dienststatus Client akzeptiert wird oder die `Partition` oder `CodePackageActivationContext` Objekte bedeutet nicht, dass es im Speicher angewendet wird. Es ist asynchrone gesendet und oftmals mit anderen Berichten zusammengefasst. Die Verarbeitung auf dem Server kann immer noch ein Fehler auftreten: die Anzahl der Sequenz veraltete sein kann, wurde die Entität, an dem der Bericht angewendet werden muss usw. gelöscht haben,.

## <a name="health-client"></a>Gesundheit client
Die Verwendungsberichte werden im Speicher Dienststatus über einen Client Gesundheit gesendet, die innerhalb der Fabric Client verfügbar ist. Der Dienststatus-Client kann mit den folgenden konfiguriert werden:

- **HealthReportSendInterval**: die Verzögerung zwischen der Zeit, die der Bericht wird an den Client hinzugefügt und die Uhrzeit, es, um die Integrität Store gesendet wird. Für jeden Bericht auf Stapel Berichte in einer einzelnen Nachricht, anstatt beim Senden einer Nachricht verwendet. Die Batchverarbeitung verbessert die Leistung. Standard: 30 Sekunden.

- **HealthReportRetrySendInterval**: das Intervall, sendet der Dienststatus Client kumulierten Gesundheit erneut, Berichte zum Systemzustand Store. Standard: 30 Sekunden.

- **HealthOperationTimeout**: das Timeout für einen Berichtnachricht, die mit den Dienststatus Store. Wenn eine Nachricht abgelaufen ist, wiederholt der Dienststatus Client, bis der Dienststatus Store bestätigt, dass der Bericht verarbeitet wurde. Standard: zwei Minuten.

> [AZURE.NOTE] Wenn die Berichte gestapelter sind, der Client Fabric muss aktiv bleiben für mindestens die HealthReportSendInterval, um sicherzustellen, dass sie gesendet werden. Wenn die Nachricht verloren geht oder der Dienststatus Store nicht werden, aufgrund einer vorübergehenden Fehler angewendet kann, der Fabric-Client muss aktiv bleiben mehr zu dieser Möglichkeit, versuchen Sie erneut zu verleihen.

Die Pufferung auf dem Client nimmt die Eindeutigkeit der Berichte zu berücksichtigen. Beispielsweise einer bestimmten schlechten Reporter 100 Berichten pro Sekunde auf Eigenschaft für die betreffende Entität Bericht erstellt wird, werden die Berichte mit der letzten Version ersetzt. Nur ein solcher Bericht ist in der Warteschlange Client höchstens vorhanden. Wenn Batchverarbeitung konfiguriert ist, ist die Anzahl der Berichte, die dem Dienststatus speichern nur eine pro Intervall senden aus. Dieser Bericht ist der letzte hinzugefügten Bericht, der im aktuellen Zustand der Entität angibt.
Können alle Konfigurationsparameter werden angegeben, wann `FabricClient` wird erstellt, indem Sie [FabricClientSettings](https://msdn.microsoft.com/library/azure/system.fabric.fabricclientsettings.aspx) mit den gewünschten Werten für den Systemzustand bezogene Einträge übergeben.

Im folgenden einen Fabric-Client erstellt und gibt an, dass die Berichte gesendet werden soll, wenn sie hinzugefügt werden. Zeitlimit und Fehlern, die wiederholt werden können, erfolgen Wiederholungsversuche alle 40 Sekunden.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Gleichen Parameter können angegeben werden, wenn eine Verbindung zu einem Cluster über PowerShell erstellt wird. Die folgenden startet eine Verbindung mit einem lokalen Cluster:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

> [AZURE.NOTE] Um sicherzustellen, dass nicht autorisierte Dienste Gesundheit vor der Elemente im Cluster nicht Berichten, kann der Server für die Anfragen nur von gesicherten Clients annehmen konfiguriert werden. Die `FabricClient` für Berichte Sicherheit muss aktiviert werden sollen, die Kommunikation mit dem Cluster (z. B. mit Kerberos oder Zertifikat Authentifizierung) verwendet. Weitere Informationen zum [Clustersicherheit](service-fabric-cluster-security.md).

## <a name="report-from-within-low-privilege-services"></a>Melden von innerhalb von geringen Berechtigungen services
Aus innerhalb der Dienst Fabric-Dienste, die nicht Administratorzugriff auf den Cluster, verfügen Sie können melden Dienststatus auf Elemente im aktuellen Kontext durch `Partition` oder `CodePackageActivationContext`.

- Verwenden Sie für zustandsloser Dienste [IStatelessServicePartition.ReportInstanceHealth](https://msdn.microsoft.com/library/system.fabric.istatelessservicepartition.reportinstancehealth.aspx) in der aktuellen Dienstinstanz einen Bericht ein.

- Verwenden Sie für dynamische Dienste [IStatefulServicePartition.ReportReplicaHealth](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.reportreplicahealth.aspx) Berichten des aktuellen Replikat ein.

- Verwenden Sie [IServicePartition.ReportPartitionHealth](https://msdn.microsoft.com//library/system.fabric.iservicepartition.reportpartitionhealth.aspx) , um auf die aktuelle Partition Entität aufzuzeichnen.

- Verwenden Sie [CodePackageActivationContext.ReportApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportapplicationhealth.aspx) , um auf der aktuellen Anwendung aufzuzeichnen.

- Verwenden der [CodePackageActivationContext.ReportDeployedApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth.aspx) Berichten der aktuellen Anwendung auf dem aktuellen Knoten bereitgestellt.

- Verwenden der [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth.aspx) Berichten ein Service-Paket für die aktuelle Anwendung auf dem aktuellen Knoten bereitgestellt.

> [AZURE.NOTE] Intern, die `Partition` und die `CodePackageActivationContext` halten einen Dienststatus-Client der mit Standardeinstellungen konfiguriert ist. Die gleichen Aspekte erläutert für [Gesundheit Client](service-fabric-report-health.md#health-client) anwenden – Berichte werden zusammengefasst und auf einen Zeitgeber unter gesendet wird, sodass die Objekte haben die Möglichkeit zum Senden des Berichts beibehalten werden soll.

## <a name="design-health-reporting"></a>Entwerfen Gesundheit reporting
Zum Generieren von Berichten mit hoher Qualität der erste Schritt besteht darin, die Konditionen finden, die die Integrität des Diensts auswirken können. Bedingung, die Kennzeichnung Probleme mit dem Dienst oder Cluster helfen können, wenn es beginnt – oder sogar, bevor Sie ein Problem geschieht – potenziell kann speichern Milliarden Dollar. Zu den Vorteilen gehören kleiner unten Zeit, weniger Nacht Stunden aufgewendet Untersuchung läuft und Reparieren von Problemen und höher Kunden zufrieden.

Nachdem Sie die Konditionen identifiziert werden, müssen Watchdog Autoren ermitteln die beste Möglichkeit, diese für Saldo zwischen Verwaltungsaufwand und Nützlichkeit überwachen. Betrachten Sie beispielsweise einen Dienst, der komplexe Berechnungen unterstützt, die einige temporären Dateien auf einer Freigabe verwenden. Watchdog kann überwachen und die freigeben, um sicherzustellen, dass genügend Speicherplatz verfügbar ist. Es konnte für Benachrichtigungen über Datei-oder Verzeichnis warten. Es konnte Bericht eine Warnung aus, wenn ein im Vorfeld Schwellenwert erreicht wird und ein Fehler gemeldet, wenn die Freigabe voll ist. Eine Warnung, könnte ein System reparieren bereinigen ältere Dateien in der Freigabe starten. Klicken Sie auf einen Fehler zurück konnte das Replikat Dienst ein Systems reparieren auf einen anderen Knoten verschoben werden. Beachten Sie, wie die Bedingung Status in Bezug auf Gesundheit beschrieben sind: den Status der Bedingung, die fehlerfrei angesehen werden kann oder fehlerhaft (Warnung oder Fehler).

Nachdem Sie die Überwachung Details festgelegt sind, muss der Autor eines Watchdog herausfinden, wie der Watchdog implementiert wird. Wenn die Konditionen aus innerhalb des Diensts bestimmt werden können, kann der Watchdog Teil der überwachten Dienst selbst sein. Beispielsweise kann der Code die Verwendung freigeben aktivieren, und melden sich mithilfe eines lokalen Fabric Clients jedes Mal, wenn versucht wird, eine Datei zu schreiben. Der Vorteil von diesem Ansatz ist, dass reporting einfach ist. Sie müssen sorgfältig um zu verhindern, dass Watchdog-Fehlern die Funktionalität Service durch beeinträchtigen.

Berichterstattung innerhalb ist überwachte Dienst nicht immer eine Option. Watchdog innerhalb der Dienst möglicherweise nicht die Konditionen erkennen können. Es möglicherweise nicht die Logik oder Daten, die die Ermittlung. Der Aufwand für die Bedingungen für die Überwachung möglicherweise hoch. Die Konditionen auch möglicherweise nicht speziell für einen Dienst, aber stattdessen Interaktionen zwischen Diensten auswirken. Eine weitere Möglichkeit besteht darin Watchdogs der Cluster als separate Prozesse enthalten. Die Watchdogs überwachen den Bedingungen und den Bericht einfach ohne Auswirkung auf die wichtigsten Dienste in keiner Weise. Beispielsweise könnten diese Watchdogs als zustandsloser Dienste in der gleichen Anwendung, die auf allen Knoten oder auf den gleichen Knoten wie der Dienst bereitgestellt implementiert werden.

Manchmal ist im Cluster Watchdog keine Option entweder. Ist die überwachte Bedingung die Verfügbarkeit oder Funktionalität des Diensts anzuzeigen, empfiehlt es sich, dass die Watchdogs an derselben Stelle wie die Benutzer-Clients. Vorhanden ist, können sie die Vorgänge auf die gleiche Weise testen Benutzer angerufen werden. Sie können beispielsweise Watchdog haben, die außerhalb der Cluster verfügbar ist und Anfragen zum Dienst Probleme, und klicken Sie dann überprüft des Wartezeit und der Richtigkeit des Ergebnisses. (Für einen Dienst Rechner zurückgeben beispielsweise 2 + 2 4 in einer angemessenen Zeitspanne?)

Nachdem Sie die Details Watchdog fertig gestellt haben, sollten Sie auf eine Quelle ID entscheiden, die es eindeutig. Wenn mehrere Watchdogs desselben Typs im Cluster beständig, entweder müssen sie auf verschiedenen Personen Berichten oder, wenn sie auf die betreffende Entität Berichten, müssen sie sicherstellen, dass die Datenquellen-ID oder die Eigenschaft nicht identisch ist. Auf diese Weise können ihre Berichte verwendet werden. Die Eigenschaft des Berichts Gesundheit sollte die überwachte Bedingung erfassen. (Für die oben genannten Beispiel könnte die Eigenschaft **ShareSize**sein.) Wenn Sie mehrere Berichte auf die gleiche Bedingung angewendet wird, sollte die Eigenschaft einige dynamische Informationen enthalten, Berichte verwendet werden können zu zulässt. Wenn mehrere Freigaben überwacht werden müssen, kann beispielsweise den Namen der Eigenschaft **ShareSize-Freigabename**sein.

> [AZURE.NOTE] Der Dienststatus Store sollte *nicht* verwendet werden, um Statusinformationen zu behalten. Nur Dienststatus-bezogene Informationen sollte als Dienststatus, gemeldet werden diese Informationen wirkt sich auf die Gesundheit Auswertung einer Entität. Der Dienststatus Store wurde nicht als eine allgemeine Store konzipiert. Gesundheit Auswertung Logik verwendet, um alle Daten in der Integritätsstatus aggregieren. Senden von Informationen für die Gesundheit (wie Statusberichte mit einer Integritätsstatus OK) von nicht verbundenen wirkt sich nicht auf des aggregierten Integritätsstatus, aber sie können die Leistung des Systemzustands Speichers negativ beeinflussen.

Nächstes ist die Entität Bericht auf. In den meisten Fällen, ist dies offensichtlich, basierend auf der Bedingung. Sie sollten die Entität mit beste mögliche Genauigkeit auswählen. Wenn eine Bedingung wirkt sich auf alle Replikate in einer Partition, melden Sie sich auf der Partition, die sich nicht auf den Dienst. Es gibt Ecke Fälle, in dem mehr Aufwand, durch erforderlich ist. Wenn die Bedingung wirkt sich auf eine Entität, wie etwa ein Replikat, aber der Wunsch besteht darin, die Bedingung für mehr als die Dauer der Replikat Nutzungsdauer, gekennzeichnet haben, und klicken Sie dann auf der Partition gemeldet werden sollen. Andernfalls, wenn das Replikat gelöscht wird, werden alle Berichte zugeordnet aus dem Store bereinigt. Dies bedeutet, dass Autoren Watchdog die Gültigkeitsdauer der Entität und den Bericht auch anzustellen müssen. Sie müssen deutlich zu sagen, wenn ein Bericht soll aus einem Speicher (beispielsweise bereinigt werden, wenn ein Fehler gemeldet wird, klicken Sie auf eine Entität nicht mehr gültig).

Sehen wir uns ein Beispiel, die an die Punkte zusammengeführt werden, die ich beschrieben. Erwägen Sie eine Fabric Service-Anwendung zusammengesetzt ein master dynamische beständigen Dienst und sekundären zustandsloser Dienste auf allen Knoten (eine sekundäre Diensttyp für jeden Vorgang) bereitgestellt. Das Master-Shape hat eine Verarbeitung, das durch sekundäre auszuführende Befehle enthält. Der sekundäre ausführen eingehenden Anfragen und wieder Bestätigung Signale senden. Einer Bedingung, die überwacht werden kann, ist die Länge der Verarbeitungswarteschlange master. Wenn die Länge der master Warteschlange einen Schwellenwert erreicht hat, wird eine Warnung gemeldet. Die Warnung gibt an, dass der sekundäre die Last verarbeiten können. Wenn die maximale Länge der Warteschlange erreichen und Befehle gelöscht werden, wird ein Fehler gemeldet, wie der Dienst nicht behoben werden kann. Die Berichte können auf die Eigenschaft **QueueStatus**sein. Der Watchdog befindet sich innerhalb des Diensts und regelmäßig auf die primäre Masterreplikat gesendet wird. Die Time to live beträgt zwei Minuten, und es wird regelmäßig alle 30 Sekunden gesendet. Wenn die primäre nach unten geht, wird der Bericht automatisch Store bereinigt. Ist das Replikat Dienst nach oben, aber es blockiert, oder gibt es andere Probleme, der Bericht läuft ab, im Speicher Dienststatus. In diesem Fall wird die Entität am Fehler ausgewertet.

Eine weitere Bedingung, die überwacht werden können ist Aufgabe Ausführung dauert. Das Master-Shape verteilt Aufgaben an, um die sekundäre ausgehend von der Vorgangsart. Je nach den Entwurf konnte das Master-Shape der sekundäre für Vorgangsstatus Umfrage. Es konnte auch warten, bis sich sekundäre Signale zurück Bestätigung zu senden, wenn sie fertig sind. Im zweiten Fall müssen sorgfältig werden Situationen erkennen, in dem Die sekundäre / en verloren gegangen sind. Eine Möglichkeit ist für das Master-Shape einer Pinganfrage in denselben sekundäre, senden, das ihren Status wieder sendet. Wenn kein Status eingeht, wird das Master-Shape hält es mit einem Fehler und plant Sie den Vorgang berücksichtigen. Dieses Verhalten wird davon ausgegangen, dass die Aufgaben Idempotent sind.

Die überwachte Bedingung kann als Warnhinweise übersetzt werden, erfolgt die Aufgabe nicht in einer bestimmten Zeit (**t1**, beispielsweise 10 Minuten). Wenn die Aufgabe in Zeit (**t2**, beispielsweise 20 Minuten) nicht abgeschlossen ist, kann die überwachte Bedingung als Fehler übersetzt werden. Die Berichterstattung kann auf verschiedene Weise erfolgen:

- Das primäre Masterreplikat Berichte regelmäßig auf sich selbst. Sie können eine Eigenschaft für alle anstehenden Aufgaben in der Warteschlange haben. Wenn mindestens eine Aufgabe länger dauert, wird der Berichtstatus der Eigenschaft **PendingTasks** eine Warnung oder ein Fehler, je nach Bedarf. Wenn es keine anstehenden Aufgaben sind oder alle Aufgaben ausführen Schritte, ist der Berichtstatus OK aus. Die Vorgänge sind beständigen. Wenn die primäre fällt aus, kann neu heraufgestufte primären weiterhin ordnungsgemäß melden.

- Ein anderer Watchdog-Prozess (in der Cloud oder externe) überprüft die Vorgänge (von außerhalb, basierend auf den gewünschten Taskergebnis) um festzustellen, ob sie abgeschlossen sind. Berücksichtigen sie die Schwellenwerte nicht, wird ein Bericht master-Dienst gesendet. Ein Bericht wird auch für jeden Vorgang gesendet, die den Bezeichner Vorgang wie **PendingTask + TaskId**enthält. Berichte, die nur auf fehlerhaften Zustände gesendet werden soll. Legen Sie die Zeit, live in wenigen Minuten, und markieren die Berichte, wenn sie ablaufen, um sicherzustellen, dass zum Aufräumen entfernt werden soll.

- Wenn länger dauert als erwartet, um Sie auszuführen-Berichte des sekundären, der eine Aufgabe ausgeführt wird. Diese Berichte auf die Instanz des Diensts der Eigenschaft **PendingTasks**. Der Bericht identifiziert die Instanz des Diensts, die Probleme hat, aber es nicht die Stelle, an der die Instanz beendet Situation erfassen. Die Berichte werden dann bereinigt. Es konnte sekundäre-Dienst gemeldet werden. Wenn die sekundäre den Vorgang abgeschlossen ist, löscht die zweite Instanz des Berichts aus dem Speicher. Der Bericht erfassen nicht die Situation, wo die Bestätigungsnachricht geht verloren, und die Aufgabe aus der Sicht der Master-Shape nicht abgeschlossen ist.

Jedoch die Berichte in den oben beschriebenen Fällen fertig ist, werden die Berichte in der Anwendung Gesundheit erfasst werden, wenn Gesundheit ausgewertet wird.

## <a name="report-periodically-vs-on-transition"></a>Bericht in regelmäßigen Abständen im Vergleich zu auf Übergang
Mithilfe von Gesundheit reporting Modell können Watchdogs Berichte in regelmäßigen Abständen, oder klicken Sie auf Übergänge senden. Wird empfohlen, für Watchdog-Berichte in regelmäßigen Abständen, da der Code viel einfacher und nicht ist zu Fehlern. Die Watchdogs müssen bemüht werden so einfach wie möglich, um Fehler zu vermeiden, die falsche Berichte auslösen. Falsche *fehlerhaften* Berichte beeinflussen, Testen der Systemzustand und Szenarien basierend auf Gesundheit, einschließlich Upgrades. Falsche *fehlerfrei* Berichte Ausblenden von Problemen im Cluster, die nicht gewünscht wird.

Für periodische Berichte kann der Watchdog mit einem Zeitgeber implementiert werden. Der Watchdog kann auf Timer-Rückruf überprüfen Sie den Status und Senden eines Berichts basierend auf dem aktuellen Status. Es gibt keine müssen finden Sie unter welcher Bericht schon einmal gesendet wurde, oder alle Optimierungen im Hinblick auf messaging durchführen. Der Dienststatus-Client hat Batchverarbeitung Logik, mit der Leistung helfen. Während der Dienststatus Client beibehalten, wiederholt den Vorgang intern bis der Bericht durch den Dienststatus Store bestätigt wird oder der Watchdog einen neueren Bericht mit Entität, Eigenschaft und Quelle generiert.

Klicken Sie auf Übergänge Reporting erfordert sorgfältige Behandlung von Bundesstaat. Der Watchdog überwacht einige Bedingungen und Berichte, wenn nur die Bedingungen ändern. Der Vorteil von diesem Ansatz ist, dass weniger Berichte erforderlich sind. Ist, die Logik der der Watchdog komplex ist. Der Watchdog muss die Bedingungen oder die Berichte verwalten, damit sie überprüft werden können, um Zustandsänderungen zu ermitteln. Bei einem Failover sorgfältig vor, wenn ein Bericht gesendet wird, kann nicht gesendet worden zuvor (in der Warteschlange, aber noch nicht an den Dienststatus Store gesendet werden). Die Anzahl der Sequenz muss größerem. Wenn dies nicht der Fall ist, werden die Berichte als veraltet zurückgewiesen. In Ausnahmefällen, bei dem Datenverlust getätigt wurde, kann die Synchronisierung zwischen den Status des Melders und den Status des Speichers Gesundheit erforderlich sein.

Klicken Sie auf Übergänge Reporting ist sinnvoll für auf sich selbst, bis reporting Services `Partition` oder `CodePackageActivationContext`. Wenn das lokale Objekt (Replikat oder bereitgestellte Dienst Paket / Anwendung bereitgestellt) wird entfernt, werden auch alle zugehörigen Berichten entfernt. Diese automatische Aufräumen lockert die Notwendigkeit der Synchronisierung zwischen Reporter und Gesundheit Store. Wenn der Bericht für die übergeordnete Partition oder übergeordneten Anwendung ist, muss auf Failover sorgfältig werden veraltete Berichte in den Dienststatus Store zu vermeiden. Logik muss hinzugefügt werden, um den richtigen Status verwalten, und deaktivieren Sie den Bericht Store, wenn Sie nicht mehr benötigt.

## <a name="implement-health-reporting"></a>Implementieren der Dienststatus reporting
Nachdem Sie die Details Entität und Bericht deaktiviert sind, kann die Integritätsberichte senden über die API, PowerShell oder REST vorgenommen werden.

### <a name="api"></a>API
Wenn Sie über die API melden möchten, müssen Sie zum Erstellen eines Berichts Gesundheit speziell für den Entitätstyp, die sie melden möchten. Geben Sie dem Bericht einem Gesundheit Client. Alternativ Erstellen einer Gesundheit und übergeben, um Methoden zur Meldung auf korrigieren `Partition` oder `CodePackageActivationContext` Berichten des aktuellen Einheiten.

Im folgenden Beispiel wird die periodische Berichte aus der Watchdog innerhalb der Cluster. Der Watchdog überprüft, ob eine externe Ressource innerhalb eines Knotens aus zugegriffen werden kann. Die Ressource ist eine Servicemanifest innerhalb der Anwendung erforderlich. Wenn die Ressource nicht verfügbar ist, können die anderen Diensten in der Anwendung weiterhin richtig. Daher wird der Bericht alle 30 Sekunden auf Entität Paket bereitgestellte Dienst gesendet.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShell
Senden Sie Verwendungsberichte mit * *Senden-ServiceFabric*EntityType*HealthReport **.

Im folgenden Beispiel wird die periodische Berichte zur CPU-Werte auf einem Knoten. Die Berichte alle 30 Sekunden gesendet werden soll, und sie haben eine Uhrzeit in zwei Minuten live. Wenn sie ablaufen, hat der Reporter Probleme, damit der Knoten am Fehler ausgewertet wird. Wenn die CPU einen Schwellenwert ist, enthält der Bericht Integritätsstatus eines Warnung aus. Wenn die CPU einen Schwellenwert für mehr als die konfigurierte Zeit bleibt, wird es als ein Fehler gemeldet. Andernfalls sendet die Reporter Integritätsstatus eines OK.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Im folgenden Beispiel wird Berichte eine vorübergehende Warnung ein Replikat. Es wird zuerst die Partitions-ID, und klicken Sie dann die Replikat-ID für den Dienst, die, dem Sie interessiert. Danach sendet einen Bericht aus **PowershellWatcher** der Eigenschaft **ResourceDependency**. Der Bericht ist relevante nur zwei Minuten, und wird Sie automatisch aus dem Speicher entfernt.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>REST
Senden Sie Verwendungsberichte Verwendung von REST mit POST-Anfragen, die auf die gewünschte Entität wechseln und im Textkörper Beschreibung die Integrität des Berichts. Beispielsweise finden Sie unter REST [Cluster Integritätsberichte](https://msdn.microsoft.com/library/azure/dn707640.aspx) oder [Dienst Integritätsberichte](https://msdn.microsoft.com/library/azure/dn707640.aspx)zu senden. Alle Elemente werden unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Basierend auf den Dienststatus-Daten, können Autoren Dienst und Cluster/Anwendung Administratoren Möglichkeiten, nutzen die Informationen vorstellen. Beispielsweise können sie Benachrichtigungen einrichten basierend auf den Integritätsstatus schwerwiegende Problemen abgefangen, bevor er Ausfall soll. Administratoren können auch reparieren Systeme so einrichten automatisch Probleme zu beheben.

[Einführung in Service Fabric Gesundheit Überwachung](service-fabric-health-introduction.md)

[Dienst Fabric-Integritätsberichte anzeigen](service-fabric-view-entities-aggregated-health.md)

[So Berichten und Kontrollkästchen-Dienststatus](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Verwenden Sie Verwendungsberichte System zur Behandlung dieses Problems](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Überwachen und Dienste lokal diagnostizieren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade der Fabric Service-Anwendung](service-fabric-application-upgrade.md)
