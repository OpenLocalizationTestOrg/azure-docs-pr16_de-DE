<properties
   pageTitle="Verteilte Transaktionen über Clouddatenbanken"
   description="Übersicht über flexible Datenbanktransaktionen mit SQL Azure-Datenbank"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Verteilte Transaktionen über Clouddatenbanken

Flexible Datenbanktransaktionen für SQL Azure-Datenbank (SQL DB) können Sie zum Ausführen von Transaktionen, die mehrere Datenbanken in der SQL-Datenbank umfassen. Flexible Datenbanktransaktionen für SQL-DB stehen für .NET Applications mithilfe von ADO.NET und mit der vertrauten programming Erfahrung mit den Klassen [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) integrieren. Um die Bibliothek zu erhalten, finden Sie unter [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

Lokal erforderlich bei der Fall normalerweise Microsoft Distributed Transaktion Coordinator (MSDTC) ausgeführt. Da MSDTC nicht für Plattform-als-Service-Anwendung in Azure verfügbar ist, wurde die Möglichkeit zum Koordinieren von verteilter Transaktionen jetzt direkt in SQL DB integriert. Applikationen können die Verbindung zu einer SQL-Datenbank, um verteilte Transaktionen zu starten, und eine der Datenbanken wird die verteilte Transaktion transparent koordinieren, wie in der folgenden Abbildung gezeigt. 

  ![Verteilte Transaktionen mit Azure SQL-Datenbank mithilfe von Transaktionen flexible Datenbanken ][1]

## <a name="common-scenarios"></a>Häufige Szenarien

Flexible Datenbanktransaktionen für SQL-DB können Anwendungen in mehrere verschiedene SQL-Datenbanken gespeicherten Daten atomaren ändern. Die Vorschau befasst sich clientseitige Entwicklung Erfahrung in c# und .NET. Eine serverseitige Erfahrung mit T-SQL ist für einen späteren Zeitpunkt geplant.  
Flexible Datenbanktransaktionen richtet die folgenden Szenarien:

* Mehrere datenbankanwendungen in Azure: mit diesem Szenario Daten ist vertikal aufgeteilt über mehrere Datenbanken in der SQL-Datenbank so, dass verschiedene Arten von Daten auf anderen Datenbanken befinden. Einige Vorgänge erfordern Änderungen an Daten, die in zwei oder mehr Datenbanken vorhanden ist. Die Anwendung verwendet flexible Datenbanktransaktionen, um die Änderungen über Datenbanken koordinieren und Unteilbarkeit sicherzustellen.

* Sharded datenbankanwendungen in Azure: mit diesem Szenario verwendet die Datenebene die [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md) oder Self-Sharding um die Daten horizontal über viele Datenbanken in der SQL-Datenbank aufzuteilen. Ein herausragender Anwendungsfall-ist das atomare Änderungen bei der Änderungen Mandanten Intervalls für eine sharded mit mehreren Mandanten Anwendung ausführen. Denken Sie zum Beispiel zur Übertragung von einem Mandanten in ein anderes beide, die auf anderen Datenbanken. Ein zweite Fall ist abgestimmte Sharding um Kapazität Anforderungen für eine große Mandanten zu aufzunehmen, die wiederum in der Regel setzt voraus, dass einige atomare Vorgänge muss erstreckt mehrere Datenbanken für den gleichen Mandanten verwendet. Ein dritter Fall ist atomaren Updates auf Daten verweisen, die über Datenbanken repliziert werden. Atomare, durchgeführte, Vorgänge entlang dieser Zeilen können jetzt über mehrere Datenbanken mit Preview koordiniert werden.
Flexible Datenbanktransaktionen verwenden zwei Phasen Commit ausführen, um Atomizität über Datenbanken sicherzustellen. Es ist gut für Transaktionen, die weniger als 100 Datenbanken jeweils in einer einzigen Transaktion. Diese Grenzwerte werden nicht erzwungen, aber eine erwarten Leistung und Erfolg Sätzen für flexible Datenbanktransaktionen beeinträchtigt, wenn diese Grenzwerte überschreiten.


## <a name="installation-and-migration"></a>Installation und migration

Die Funktionen für flexible Datenbanktransaktionen in der SQL-Datenbank sind über Updates an die Bibliotheken .NET System.Data.dll und System.Transactions.dll bereitgestellt. Die DLL-Dateien stellen Sie sicher, dass die zwei Phasen Commit erforderlichenfalls Ablauf verwendet wird. Um zur Entwicklung von Applications mit flexible Datenbanktransaktionen zu starten, installieren Sie [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) oder einer späteren Version aus. Bei der Ausführung auf eine frühere Version von .NET Framework tritt Transaktionen zu einer verteilten Transaktion Höherstufen, und es wird eine Ausnahme ausgelöst.

Nach der Installation können Sie die verteilte Transaktion APIs in System.Transactions mit Verbindungen mit SQL DB verwenden. Wenn Sie vorhandene MSDTC Applikationen Verwendung dieser APIs haben, einfach neu zu erstellen Ihrer vorhandenen Applikationen für .NET 4.6 nach der Installation von der 4.6.1 Framework. Wenn Ihre Projekte 4.6 .NET adressieren, werden diese automatisch die aktualisierten DLL-Dateien aus der neuen Version von Framework verwenden und verteilte Transaktion API in Kombination mit Verbindungen mit SQL DB Anrufe ist jetzt erfolgreich.

Denken Sie daran, dass keine flexible Datenbanktransaktionen erfordern MSDTC installieren. Flexible Datenbanktransaktionen werden stattdessen direkt nach und in SQL DB verwaltet. Dadurch wird Cloudszenarien erheblich vereinfacht, da eine Bereitstellung von MSDTC nicht zur Verwendung von verteilter Transaktionen mit SQL DB erforderlich ist. Abschnitt 4 erläutert ausführlicher flexible Datenbanktransaktionen und erforderlichen .NET Framework zusammen mit der Cloud Applications in Azure bereitstellen.

## <a name="development-experience"></a>Erfahrung in der Entwicklung

### <a name="multi-database-applications"></a>Mehrere datenbankanwendungen

Der folgende Code verwendet die vertraute programming Erfahrung mit .NET System.Transactions. Die TransactionScope-Klasse stellt eine umgebende Transaktion in .NET her. ("Umgebende Transaktion" ist eine, das im aktuellen Thread verfügbar ist.) Alle Verbindungen geöffnet innerhalb der TransactionScope teilnehmen Transaktion. Wenn Sie andere Datenbanken teilnehmen, wird die Transaktion automatisch zu einer verteilten Transaktion somit. Das Ergebnis der Transaktion wird durch das Festlegen des Bereichs zum Angeben eines Commits ausführen gesteuert.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Sharded datenbankanwendungen
 
Flexible Datenbanktransaktionen für SQL-DB unterstützen koordinieren verteilte Transaktionen, die Stelle, an der Sie mit der OpenConnectionForKey Methode der Clientbibliothek flexible Datenbank um Verbindungen für einen skalierten, Daten zu öffnen, auch Ebene. Erwägen Sie die Fälle, in denen Sie über mehrere verschiedene Sharding Schlüsselwerte für Änderungen Konsistenz sichergestellt ist müssen. Verbindungen mit der Schlüsselwerte für die verschiedenen Sharding hosten mehrere Shards hinweg sind vermittelte OpenConnectionForKey verwenden. Im allgemeinen Fall können Verbindungen zu anderen mehrere Shards hinweg sein, Transaktionen Garantien Sicherstellung eine verteilte Transaktion erfordert. Im folgenden Beispiel veranschaulicht diese Vorgehensweise. Es wird davon ausgegangen, dass eine Variable namens Shardmap zum Darstellen einer Karte Shard aus der flexible Datenbank-Client-Bibliothek verwendet wird:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>.NET-Installation für Azure Cloud Services

Azure bietet mehrere Angebote für Host .NET Applications. Ein Vergleich der verschiedenen Angebote steht in [Azure-App-Verwaltungsdienst, Cloud-Diensten und virtuellen Computern Vergleich](../app-service-web/choose-web-site-cloud-service-vm.md). Wenn der Gast OS des Angebots kleiner als .NET 4.6.1 für flexible Transaktionen erforderlich ist, müssen Sie den Gast OS auf 4.6.1 aktualisieren. 

Für Azure App Services sind Upgrades auf Gast-BS derzeit nicht unterstützt. Azure virtuellen Computern einfach melden Sie sich bei dem virtuellen Computer, und führen Sie das Installationsprogramm für die neuesten .NET Framework. Für Azure Cloud Services müssen Sie die Installation von eine neuere Version von .NET in die Startaufgaben beim der Bereitstellung aufnehmen möchten. Konzepte und Schritte sind in [.NET zu installieren, klicken Sie auf eine Rolle der Cloud-Dienst](../cloud-services/cloud-services-dotnet-install-dotnet.md)beschrieben.  

Beachten Sie, dass das Installationsprogramm für .NET 4.6.1 möglicherweise mehr temporären Speicher während der Bootstrappingprozess auf Azure Cloud Services als das Installationsprogramm für .NET 4.6 erfordert. Um eine erfolgreiche Installation sicherzustellen, müssen Sie vergrößern temporären Speicher für Ihren Azure-Cloud-Dienst in der Datei ServiceDefinition.csdef im Abschnitt LocalResources und der umgebungseinstellungen der Aufgabe Autostart, wie im folgenden Beispiel gezeigt:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Transaktionen über mehrere Server

Flexible Datenbanktransaktionen werden auf verschiedene logische Server in Azure SQL-Datenbank unterstützt. Wenn Transaktionen logischen Server-Grenzen überschreiten, müssen der beteiligten Server zunächst in einem gemeinsamen Kommunikation Beziehung eingegeben werden. Sobald die Kommunikation Beziehung eingerichtet wurde, kann eine Datenbank in eine der beiden Server flexible Transaktionen mit Datenbanken vom anderen Server teilnehmen. Mit Transaktionen, die mehr als zwei logische Servern dauern muss eine Beziehung Kommunikation für alle Paare von logischen Servern angeordnet werden.

Verwenden Sie die folgenden PowerShell-Cmdlets Cross-Server-Kommunikation Beziehungen Transaktionen flexible Datenbanken verwalten:

* **Neu-AzureRmSqlServerCommunicationLink**: mit diesem Cmdlet verwenden, um eine neue Beziehung der Kommunikation zwischen zwei logischen Servern in Azure SQL-Datenbank zu erstellen. Die Beziehung ist symmetrischen, d. h., beide Server und den anderen Transaktionen initiieren können.
* **Get-AzureRmSqlServerCommunicationLink**: Verwenden Sie dieses Cmdlet zum Abrufen von vorhandenen Kommunikation Beziehungen und ihre Eigenschaften.
* **Entfernen-AzureRmSqlServerCommunicationLink**: Verwenden Sie zum Entfernen einer vorhandenen Beziehung für die Kommunikation mit diesem Cmdlet. 


## <a name="monitoring-transaction-status"></a>Transaktionsstatus für die Überwachung

Verwenden Sie dynamische Management Ansichten (DMVs) in der SQL-Datenbank Monitorstatus und den Fortschritt Ihrer laufenden flexible Datenbanktransaktionen. Alle im Zusammenhang mit Transaktionen DMVs sind relevant für verteilte Transaktionen in der SQL-Datenbank. Sie können die entsprechende Liste mit DMVs Hier finden: [Transaktion verknüpfte dynamische Management Ansichten und Funktionen (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Diese DMVs sind besonders hilfreich:

* **sys.dm\_Tran\_aktiven\_Transaktionen**: Listen aktuell aktive Transaktionen und deren Status. Die Spalte UOW (Arbeitseinheit) kann die verschiedenen untergeordneten Transaktionen zu identifizieren, die die gleichen verteilte Transaktion angehören. Alle Transaktionen innerhalb derselben verteilte Transaktion führen denselben UOW Wert an. Finden Sie weitere Details der [DMV Dokumentation](https://msdn.microsoft.com/library/ms174302.aspx) .
* **sys.dm\_Tran\_Datenbank\_Transaktionen**: bietet zusätzliche Informationen zu Transaktionen, wie z. B. Platzierung der Transaktion im Protokoll. Finden Sie weitere Details der [DMV Dokumentation](https://msdn.microsoft.com/library/ms186957.aspx) .
* **sys.dm\_Tran\_Sperren**: Stellt Informationen über das Sperren, die zurzeit von laufenden Transaktionen verwendet werden. Finden Sie weitere Details der [DMV Dokumentation](https://msdn.microsoft.com/library/ms190345.aspx) .

## <a name="limitations"></a>Einschränkungen 

Die folgenden Einschränkungen gelten aktuell für flexible Datenbanktransaktionen in der SQL-Datenbank:

* Es werden nur Transaktionen über Datenbanken in der SQL-Datenbank unterstützt. Andere [X / Open XA](https://en.wikipedia.org/wiki/X/Open_XA) Ressourcenanbieter und außerhalb DB SQL-Datenbanken können nicht flexible Datenbanktransaktionen teilnehmen. Das heißt, flexible Datenbanktransaktionen lokal SQL Server und Azure SQL-Datenbanken erstreckt nicht möglich. Bei verteilten Transaktionen lokal weiterhin MSDTC verwenden. 
* Nur-Client-koordiniert Transaktionen von einer werden unterstützt. Serverseitige Unterstützung für T-SQL, z. B. verteilt Transaktion beginnen ist geplanten, aber noch nicht verfügbar. 
* Nur Datenbanken auf Azure SQL-DB V12 werden unterstützt.
* Transaktionen für WCF-Dienste werden nicht unterstützt. Angenommen, Sie verfügen über ein WCF-Dienst-Methode, die eine Transaktion ausgeführt wird. Trennen den Anruf innerhalb eines Transaktionsbereichs wird als eine [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception)fehl.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Verwenden Sie noch nicht flexible Datenbankfunktionen für Ihre Azure Applications? Schauen Sie sich unsere [Wegweiser für die Dokumentation](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Für Fragen bitte erreichen Sie uns im [Forum SQL-Datenbank](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) und für Featureanfragen, bitte im [Forum "Feedback" SQL-Datenbank](https://feedback.azure.com/forums/217321-sql-database/)hinzuzufügen.

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



