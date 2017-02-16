<properties
    pageTitle="Beheben von einer SQL-Verbindung zurück, die vorübergehenden Fehler | Microsoft Azure"
    description="Informationen Sie zum Behandeln von Problemen mit, diagnose und verhindern, dass eine SQL-Verbindung oder vorübergehende Fehler in Azure SQL-Datenbank. "
    keywords="SQL-Verbindung, Verbindungszeichenfolge, Verbindungsprobleme, vorübergehenden Fehler, Verbindung zurück"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Behandeln von Problemen mit, diagnose und verhindern, dass SQL-Verbindung und vorübergehenden Fehlern für SQL-Datenbank

Dieser Artikel beschreibt, wie zu verhindern, Problembehandlung, diagnose und zu verringern, Verbindung und vorübergehenden Fehlern, die Ihre Clientanwendung vorgefunden wird bei einer Interaktion mit Azure SQL-Datenbank. Informationen Sie zum Konfigurieren Logik Wiederholungsversuche, erstellen die Verbindungszeichenfolge und andere Verbindungseinstellungen anpassen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Vorübergehende Fehler (vorübergehenden Fehler)

Ein vorübergehender Fehler - auch vorübergehende Fehlerstrukturanalyse - verfügt über eine Ursache, die bald selbst aufgelöst wird. Eine gelegentliche vorübergehende Fehler verursacht Wenn Azure System schnell Hardware-Ressourcen zum besseren Lastenausgleich verschiedene Auslastung wechselt. Die meisten dieser neu konfiguriert Ereignisse häufig in weniger als 60 Sekunden abgeschlossen. Während dieses Zeitraums neu konfiguriert müssen Sie möglicherweise Verbindungsprobleme mit Azure SQL-Datenbank. Herstellen einer Verbindung mit Azure SQL-Datenbank Applikationen sollte diese vorübergehende Fehler erwartet, durch die Implementierung von "Wiederholen" Logik in ihren Code statt einbinden, die von Benutzern als Anwendungsfehler zu behandeln erstellt werden.

Wenn Ihr Clientprogramm ADO.NET verwendet wird, wird das Programm zu dem vorübergehenden Fehler durch das Auslösen der eine **SqlException**mitgeteilt. Die Eigenschaft **Zahl** mit der Liste der vorübergehende Fehler am oberen Rand des Themas verglichen werden kann: [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbindung im Vergleich zu den Befehl

Die Verbindung wird die SQL-Verbindung zu wiederholen oder diese erneut, je nach den folgenden einzurichten:

* **Ein vorübergehender Fehler auftritt, während eine Verbindung testen**: die Verbindung nach für einige Sekunden verzögern wiederholt werden soll.

* **Ein vorübergehender Fehler auftritt, während eine SQL-Abfrage-Befehl**: der Befehl sollte nicht sofort wiederholt. In diesem Fall sollte nach einer kurzen Verzögerung die Verbindung werden frisch hergestellt. Und dann auf der Befehl wiederholt werden kann.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Wiederholen Sie Logik für vorübergehende Fehler


Clientprogramme, die gelegentlich einen vorübergehenden Fehler auftreten, sind weitere robuste, sofern sie "Wiederholen" Logik enthalten.


Wenn Ihr Programm mit Azure SQL-Datenbank über eine 3rd Party Middleware kommuniziert werden soll, führen Fragen Sie mit dem Lieferanten ab, ob die Middleware "Wiederholen" Logik für vorübergehende Fehler enthält.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Grundsätze für "Wiederholen"


- Beim Versuch, eine Verbindung zu öffnen sollte wiederholt werden, ist der Fehler vorübergehende.


- Eine SQL SELECT-Anweisung, die mit einer vorübergehenden Fehler fehlschlägt sollte nicht direkt wiederholt werden.
 - In diesem Fall stellen Sie frische Verbindung her, und wiederholen Sie dann auf auswählen.


- Wenn eine SQL-UPDATE-Anweisung mit einer vorübergehenden Fehler fehlschlägt, sollte frische Verbindung hergestellt werden, bevor die Aktualisierung wiederholt wird.
 - Die Logik "Wiederholen" muss stellen Sie sicher, dass der gesamte Datenbanktransaktion abgeschlossen oder die gesamte Transaktion zurückgesetzt wird.


#### <a name="other-considerations-for-retry"></a>Andere Aspekte für "Wiederholen"


- Sehr Patienten mit lange Zeitintervalle zwischen seine Wiederholungsversuche kann eine Batchdatei, die automatisch nach Arbeitszeiten gestartet wird, und das vor heute, morgen vervollständigt, haben.


- Ein Benutzer Benutzeroberflächen-Programm sollte personenbezogenen häufig nach zu lang eine Wartezeit berücksichtigt werden.
 - Jedoch darf die Lösung jeder wenige Sekunden zu wiederholen, da dieser Richtlinie das System mit Anfragen überfluten kann.


#### <a name="interval-increase-between-retries"></a>Erhöhen der Intervall zwischen Wiederholungsversuche



Es empfiehlt sich, dass Sie für 5 Sekunden, bevor Sie Ihre erste "Wiederholen verzögern". Wiederholen nach einer kurzen Verzögerung kürzer als 5 Sekunden Risiken überfluten Cloud-Dienst. Bei jeder nachfolgenden Wiederholung die Verzögerung sollte wesentlich, wächst bis zu einem Maximum von 60 Sekunden.

Eine Beschreibung des *Zeitraums blockieren* für Clients, die ADO.NET verwenden ist in [SQL Server Verbindung Verbindungspooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)verfügbar.

Möglicherweise möchten Sie auch eine maximale Anzahl der Wiederholungsversuche festlegen, bevor Sie das Programm selbst beendet wird.


#### <a name="code-samples-with-retry-logic"></a>Mit "Wiederholen" Logik Codebeispielen


Mit "Wiederholen" Logik, in einer Vielzahl von Sprachen, Codebeispielen stehen in:

- [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testen Sie Ihre Logik "Wiederholen"


Klicken Sie zum Testen der Logik Wiederholungsversuche müssen simulieren oder dazu führen, dass einen Fehler nicht behoben werden können, während das Programm weiterhin ausgeführt wird.


##### <a name="test-by-disconnecting-from-the-network"></a>Testen Sie, indem Sie vom Netzwerk trennen


Eine Möglichkeit besteht darin, testen Sie Ihre Logik "Wiederholen" wird auf den Clientcomputer aus dem Netzwerk trennen, während das Programm ausgeführt wird. Der Fehler werden:

- **SqlException.Number** = 11001
- Meldung: "keine solcher Host bekannt"


Als Teil der ersten "Wiederholen" Versuch das Programm die Rechtschreibfehler korrigieren und dann versuchen, eine Verbindung herzustellen.


Damit praktische angezeigt wird, trennen Sie Ihren Computer aus dem Netzwerk, bevor Sie Ihr Programm starten. Klicken Sie dann erkennt Ihr Programm, bei dem das Programm, zur Laufzeit Parameter an:

1. Hinzufügen von 11001 vorübergehend zu seiner Liste der Fehler als vorübergehend zu berücksichtigen.
2. Versuchen Sie die erste Verbindung wie gewohnt ein.
3. Nachdem der Fehler abgefangen wird, entfernen Sie 11001 aus der Liste aus.
4. Anzeigen von Benutzer eine Meldung mit dem, in dem Netzwerk den Computer anzuschließen.
 - Anhalten weitere Ausführung mithilfe der Methode **Console.ReadLine** oder ein Dialogfeld mit der Schaltfläche OK. Der Benutzer drücken die EINGABETASTE, nachdem der Computer mit dem Netzwerk verbunden.
5. Versuchen Sie erneut, eine Verbindung herzustellen, Erfolg erwartet.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Testen Sie, indem Sie den Datenbanknamen Tippfehler, beim Herstellen einer Verbindung


Das Programm kann den Benutzernamen, bevor der erste Verbindungsversuch absichtlich geschrieben. Der Fehler werden:

- **SqlException.Number** = 18456
- Meldung: "Fehler bei der Anmeldung für Benutzer 'WRONG_MyUserName'."


Als Teil der ersten "Wiederholen" Versuch das Programm die Rechtschreibfehler korrigieren und dann versuchen, eine Verbindung herzustellen.


Damit praktische angezeigt wird, kann Ihr Programm ein Parameters zur Laufzeit erkannt, bei dem das Programm:

1. Hinzufügen von 18456 vorübergehend zu seiner Liste der Fehler als vorübergehend zu berücksichtigen.
2. Fügen Sie absichtlich 'WRONG_' auf den Namen des Benutzers ein.
3. Nachdem der Fehler abgefangen wird, entfernen Sie 18456 aus der Liste aus.
4. Entfernen Sie 'WRONG_' aus den Benutzernamen ein.
5. Versuchen Sie erneut, eine Verbindung herzustellen, Erfolg erwartet.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlConnection Parameter zur erneuten Verbindung


Wenn Ihr Clientprogramm mit Azure SQL-Datenbank mithilfe von .NET Framework **System.Data.SqlClient.SqlConnection**Klasse verbindet, sollten Sie .NET 4.6.1 oder höher, sodass Sie die Verbindung "Wiederholen" Feature nutzen können. Details des Features sind [hier](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Wenn Sie die [Verbindungszeichenfolge](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) für das Objekt **SqlConnection** erstellen, sollten Sie die Werte zwischen den folgenden Parametern koordinieren:

- ConnectRetryCount &nbsp; &nbsp; *(Standardwert ist 1. Bereich ist 0 bis 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(Standardwert ist 1 Sekunde. Bereich ist 1 bis 60.)*
- Connection Timeout &nbsp; &nbsp; *(der Standardwert liegt nach 15 Sekunden. Bereich ist 0 bis 2147483647)*


Insbesondere sollte der ausgewählten Werte den folgenden Gleichheit True vornehmen:

- Connection Timeout = ConnectRetryCount * ConnectionRetryInterval

Angenommen, wenn die Anzahl der = 3 und Intervall = 10 Sekunden ein Timeout von, nur 29 Sekunden nicht ganz das System genügend Zeit für seine 3rd und abgeschlossen "Wiederholen" am Herstellen einer Verbindung gewähren möchten: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbindung im Vergleich zu den Befehl


Die Parameter **ConnectRetryCount** und **ConnectRetryInterval** zulassen, dass Ihr **SqlConnection** -Objekt, das den Verbindungsvorgang zu wiederholen, ohne ein anderer Benutzer oder Ihre Hilfe Ihr Programm, wie etwa die Steuerung an Ihr Programm zurückgegeben. Die Wiederholungsversuche können in den folgenden Situationen auftreten:

- Aufrufen der mySqlConnection.Open-Methode
- Aufrufen der mySqlConnection.Execute-Methode

Es gibt eine Besonderheit aus. Wenn ein vorübergehender Fehler auftritt, während die *Abfrage* ausgeführt wird, Ihre **SqlConnection** -Objekt ist nicht wiederholen Sie den Verbindungsvorgang und es natürlich keine Wiederholung Ihrer Abfrage. **SqlConnection** prüft jedoch sehr schnell die Verbindung vor dem Senden Ihrer Abfrage für die Ausführung. Wenn das Kontrollkästchen Symbolleiste ein Verbindungsproblem erkennt, wiederholt **SqlConnection** den Verbindungsvorgang. Wenn der erneute Versuch erfolgreich ist, wird Sie Abfrage für die Ausführung gesendet.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Werden ConnectRetryCount mit der Anwendung "Wiederholen" Logik kombiniert?

Nehmen Sie an, dass die Anwendung Logik robuste benutzerdefinierten Wiederholungsversuche verfügt. Sie können den Verbindungsvorgang 4 Mal wiederholen. Wenn Sie **ConnectRetryInterval** und **ConnectRetryCount** hinzufügen = 3 zur Verbindungszeichenfolge, erhöhen Sie die Anzahl der "Wiederholen" bis 4 * 3 = 12 Wiederholungsversuche. Sie können keine solche eine großen Anzahl von Wiederholungsversuche davon.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Verbindungen mit SQL Azure-Datenbank

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>: Verbindungszeichenfolge Verbindung


Die Verbindungszeichenfolge zum Herstellen einer Verbindung mit Azure SQL-Datenbank notwendigen unterscheidet sich leicht von der Zeichenfolge zum Herstellen einer Verbindung mit Microsoft SQL Server aus. Sie können die Verbindungszeichenfolge aus dem [Azure-Portal](https://portal.azure.com/)für die Datenbank kopieren.


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Der Verbindung: IP-Adresse


Sie müssen den SQL-Datenbankserver zum Annehmen von Kommunikation über die IP-Adresse des Computers ein, die Ihr Clientprogramm hostet konfigurieren. Dazu bearbeiten die Firewalleinstellungen über das [Azure-Portal](https://portal.azure.com/)an.


Wenn Sie vergessen, die IP-Adresse zu konfigurieren, wird Ihr Programm mit einer praktischen Fehlermeldung auftreten, die besagt die notwendigen IP-Adresse.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Weitere Informationen finden Sie unter: [wie: Konfigurieren der Firewall-Einstellungen auf SQL-Datenbank](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Verbindung: Ports


Normalerweise müssen Sie nur um sicherzustellen, dass Anschluss 1433 für ausgehende Kommunikation, auf dem Computer geöffnet ist, auf dem Sie Clientprogramm befindet.


Beispielsweise können die Windows-Firewall auf dem Host, wenn Ihr Clientprogramm auf einem Windows-Computer gehostet wird, Sie Anschluss 1433 öffnen:


1. Öffnen Sie die Systemsteuerung
2. &gt;Alle Steuerelement Systemsteuerungselemente
3. &gt;Windows-Firewall
4. &gt;Erweiterte Einstellungen
5. &gt;Ausgehende Regeln
6. &gt;Aktionen
7. &gt;Neue Regel


Wenn Ihr Clientprogramm auf eine Azure-virtuellen Computern (virtueller Computer) gehostet wird, sollten Sie lesen:<br/>[Ports jenseits 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).


Hintergrundinformationen zur Konfiguration des Ports und IP-Adresse finden Sie unter: [Firewall Azure SQL-Datenbank](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Verbindung: ADO.NET 4.6.1


Wenn Ihr Programm ADO.NET-Klassen wie **System.Data.SqlClient.SqlConnection** wird verwendet, um zu Azure SQL-Datenbank herstellen, es empfiehlt sich, dass Sie .NET Framework, Version 4.6.1 verwenden oder höher.


ADO.NET 4.6.1:

- Für SQL Azure-Datenbank ist verbesserter Zuverlässigkeit beim Öffnen einer Verbindungs mithilfe der Methode **SqlConnection.Open** . Die **Open** -Methode übernimmt jetzt optimale Leistung "Wiederholen" Verfahren in Antwort auf vorübergehenden Fehler, für bestimmte Fehler in der Verbindung vorgegebenen Zeit.
- Unterstützt das Verbindungspooling. Dies umfasst eine effiziente Überprüfung, die das Connection-Objekt, das sie Ihr Programm bietet fungiert.



Wenn Sie ein Verbindungsobjekt aus einem Verbindungspool verwenden, wird empfohlen, dass das Programm die Verbindung vorübergehend schließen, wenn es nicht sofort zu verwenden. Öffnen eine Verbindung erneut ist nicht teure ist die Möglichkeit, eine neue Verbindung erstellen.


Wenn Sie ADO.NET 4.0 oder einer früheren Version, es empfiehlt sich, dass die Aktualisierung auf die neueste ADO.NET.

- Ab November 2015 können Sie [ADO.NET 4.6.1 herunterladen](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnose

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnose: Testen Sie, ob Dienstprogramme herstellen kann


Wenn Ihr Programm weiß nicht, die Verbindung zu Azure SQL-Datenbank herstellen, wird einer Diagnoseprotokollen Option versuchen, eine Verbindung mit einem Programm Programm herstellen. Idealerweise sollte das Programm verbinden, mithilfe derselben Bibliothek, die Ihr Programm verwendet.


Klicken Sie auf jedem Windows-Computer können Sie diese Dienstprogramme versuchen:

- SQL Server Management Studio (ssms.exe), das über ADO.NET her.
- Sqlcmd.exe, die über [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)her.


Nachdem die Verbindung hergestellt wurde, testen Sie, ob eine kurze wählen Sie SQL-Abfrage funktioniert.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnose: Überprüfen Sie die Ports öffnen


Angenommen, Sie davon ausgehen, dass aufgrund von Problemen mit Port weiß versucht, eine Verbindung nicht sind. Auf Ihrem Computer können Sie ein Programm ausgeführt, der auf den Portkonfigurationen Berichte.


Klicken Sie auf Linux möglicherweise die folgenden Dienstprogramme hilfreich sein:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Ändern Sie den Beispielwert benutzerspezifisch Ihre IP-Adresse ein.)


Unter Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) möglicherweise hilfreich sein. Hier ist eine Beispiel für Ausführung, die die Situation Port auf einem Server Azure SQL-Datenbank abgefragt, und die auf einem Laptopcomputer ausgeführt wurde:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnose: Der Fehler protokollieren


Ein wiederkehrender Problem ist manchmal am besten durch Erkennung von einem allgemeinen Muster über mehrere Tage oder Wochen Diagnose.


Ihren Kunden kann in einer Diagnose helfen, indem Sie die Protokollierung alle auftretenden Fehler auf. Sie können möglicherweise die Protokolleinträge mit Fehlerdaten zu koordinieren, die SQL Azure-Datenbank selbst intern protokolliert.


Enterprise-Bibliothek 6 (EntLib60) bietet verwalteter Klassen zur Unterstützung bei der Protokollierung:

- [5 - so einfach wie das Deaktivieren ein Protokoll fallen: Verwenden des Protokollierung Application Blocks](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnose: Untersuchen Sie Systemprotokolle Fehler


Hier sind einige Transact-SQL-SELECT-Anweisungen an die Abfrage Protokolle des Fehlers und andere Informationen.


| Abfrage mit log | Beschreibung |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | Die Ansicht [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) bietet Informationen zu einzelnen Ereignissen, einschließlich einige, die vorübergehende Fehler oder Konnektivitätsfehlern auftreten können.<br/><br/>Sie können die Werte **Start_time** oder **End_time** idealerweise entsprechen den Informationen, wenn Ihr Clientprogramm Probleme aufgetreten sind.<br/><br/>**Tipp:** Sie müssen mit der Ausführung dieser **master** -Datenbank verbinden. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | Die Ansicht [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) bietet zusammengefasster zählt Ereignis Typ, für die zusätzliche Diagnose.<br/><br/>**Tipp:** Sie müssen mit der Ausführung dieser **master** -Datenbank verbinden. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnose: Suchen nach Problem Ereignisse im Protokoll SQL-Datenbank


Sie können für die Einträge zum Problem Ereignisse in das Protokoll Azure SQL-Datenbank suchen. Probieren Sie die folgende Transact-SQL-SELECT-Anweisung in der **master** -Datenbank:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Einige zurückgegebenen Zeilen aus sys.fn_xe_telemetry_blob_target_read_file


Als Nächstes wird eine zurückgegebene Zeile könnte folgendermaßen aussehen. Null-Werte dargestellt sind häufig nicht null in anderen Zeilen.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Enterprise-Bibliothek 6


Enterprise-Bibliothek 6 (EntLib60) ist ein Framework .NET Klassen, die hilft Ihnen implementieren robuste Clients Cloud-Dienste, von denen der Azure SQL-Datenbank-Dienst ist. Sie können auf den jeweiligen Bereich, in dem EntLib60 unterstützen können, erste Vorsichtsmaßnahmen dedizierte Themen finden:

- [Enterprise-Bibliothek 6 – April 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


"Wiederholen" Logik für den Umgang mit vorübergehende Fehler ist ein Bereich, in dem EntLib60 helfen können:

- [4 - Perseverance, der alle Triumphs geheim: Verwenden des vorübergehenden Fehlerbehandlung Application Blocks](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] Beispiel-Quellcode für EntLib60 steht für öffentliche [herunterladen](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft hat keine Pläne, weitere Features Updates oder Wartung Updates zu Funktionen machen.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 Klassen für vorübergehende Fehler, und versuchen Sie es erneut


Die folgenden EntLib60 Klassen sind besonders hilfreich für Logik Wiederholungsversuche. Alle Dies sind oder werden außerdem unter den Namespace des **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*In den namespace* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** Klasse
 - **ExecuteAction** -Methode


- **ExponentialBackoff** Klasse


- **SqlDatabaseTransientErrorDetectionStrategy** Klasse


- **ReliableSqlConnection** Klasse
 - **ExecuteCommand** -Methode


In den Namespace des **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** Klasse

- **NeverTransientErrorDetectionStrategy** Klasse


Hier sind Links zu Informationen über EntLib60 aus:

- Kostenlose [Buch Download: Developer's Guide to Microsoft Enterprise-Bibliothek, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)

- Bewährte Methoden: [allgemeine Anleitung wiederholen](../best-practices-retry-general.md) weist eine hervorragende ausführliche Erläuterung von Logik Wiederholungsversuche.

- NuGet Download von [Enterprise Library - vorübergehende Fehlerstrukturanalyse-Handling Anwendungsblock 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Die Protokollierung blockieren


- Die Protokollierung blockieren ist eine hochgradig flexible und konfigurierbare Lösung, die Sie können:
 - Erstellen und Speichern von Nachrichten protokollieren in einer Vielzahl von Speicherorte.
 - Kategorisieren und Filtern von Nachrichten.
 - Sammeln Sie kontextbezogene Informationen, die nützlich für das Debuggen und Tracing und für die Überwachung und allgemeine Protokollierung Anforderungen sind.


- Die Protokollierung blockieren fasst die Protokollierung Funktionalität vom Ziel Log, damit der Anwendungscode konsistent sind, unabhängig vom Standort und den Typ des Speichers Ziel Protokollierung ist.


Details finden Sie unter: [5: als einfach als fallen deaktivieren ein Protokoll: Verwenden der Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient Methode-Quellcode


Als Nächstes wird von der Klasse **SqlDatabaseTransientErrorDetectionStrategy** C#-Quellcode für die Methode **IsTransient** . Der Quellcode wird erläutert, welche Fehler betrachtet wurden vorübergehende und wiederholen Sie den Vorgang, bis zum April 2013 vorgestellt.

Zahlreiche **//comment** Linien wurden entfernt aus diesen kopieren, um Lesbarkeit hervorzuheben.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Nächste Schritte

- Andere allgemeine Problembehandlung Azure SQL-Datenbank Verbindung, finden Sie unter [Behandeln von Verbindungsproblemen mit Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md).

- [SQL Server-Verbindung Verbindungspooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Erneuter* ist, dass eine Apache 2.0 allgemeine Wiederholung Bibliothek, in **Python**, um den Vorgang des Hinzufügens von "Wiederholen" Verhalten zu fast alles zu vereinfachen geschrieben lizenziert.](https://pypi.python.org/pypi/retrying)
