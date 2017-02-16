#<a name="data-management-and-business-analytics"></a>Datenverwaltung und Business Analytics

Verwalten und Analysieren von Daten in der Cloud ist nur so wichtig, wie es an anderer Stelle ist. Damit Sie dies tun können, bietet Azure verschiedenste Technologien für die Arbeit mit relationalen und nicht relationalen Daten an. In diesem Artikel werden die Optionen. 

##<a name="table-of-contents"></a>Inhaltsverzeichnis      

- [BLOB-Speicher](#blob)
- [Ausführen eines DBMS in einer virtuellen Computern](#dbinvm)
- [SQL-Datenbank](#sqldb)
    - [Synchronisieren der SQL-Daten](#datasync)
    - [SQL Berichterstattung Daten mithilfe von virtuellen Computern](#datarpt)
- [Table Storage](#tblstor)
- [Hadoop](#hadoop)

## <a name="a-nameblobablob-storage"></a><a name="blob"></a>BLOB-Speicher

Das Wort "Blob" wird die Abkürzung für "BLOB", und es genau beschrieben, welche ein Blob ist: eine Sammlung von binäre Informationen. Obwohl sie einfach sind, sind noch Blobs sehr nützlich sein. [Abbildung 1](#Fig1) zeigt die Grundlagen des Azure BLOB-Speicher.

<a name="Fig1"></a>![Diagramm eines Blobs][blobs]
 
**Abbildung 1: Azure Blob-Speicher speichert binäre Daten - Blobs - im Container.**

Wenn Blobs verwenden möchten, erstellen Sie zuerst ein Azure- *Speicher-Konto*ein. Als Teil Geben Sie an, in denen die Objekte gespeichert werden, die mit diesem Konto erstellten Azure Datencenter. Wo sie sich befindet, gehört jedes Blob erstellten auf einige Container in Ihr Speicherkonto. Um ein Blob zuzugreifen, stellt eine Anwendung eine URL mit dem Formular an:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*Container*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; ist ein eindeutiger Bezeichner zugewiesen, wenn Sie ein neues Speicherkonto erstellt wurde, während &lt; *Container* &gt; und &lt; *BlobName* &gt; sind die Namen von einem bestimmten Container und ein Blob innerhalb des Containers. 

Azure bietet zwei verschiedene Arten von Blobs. Die Auswahlmöglichkeiten sind:

- *Blockieren* Blobs, von die jede bis zu 200 GB an Daten enthalten kann. Wie der Name bereits sagt, wird ein Blob blockieren in einigen Anzahl Blöcke unterteilt. Bei einem beim Übertragen eines BLOBs blockieren Fehler, kann die erneute Übertragung mit das gesamte Blob erneut zu senden, statt des letzten Zeitraums fortsetzen. Blockieren Blobs sind ein ganz allgemein Ansatz für Speicher, und sie noch heute die am häufigsten verwendeten BLOB-Typ befinden.

- *Seitenblobs, die als bei einem TB groß sein kann.* Seitenblobs für direkten Zugriff vorgesehen sind und daher jeweils in einige Seitenanzahl aufgeteilt werden. Eine Anwendung ist zum Lesen und Schreiben einzelne Seiten zufällig im Blob kostenlos. In Azure virtuellen Computern verwenden beispielsweise virtuellen Computern, die Sie erstellen Seitenblobs als permanenten Speicher für OS Datenträger und Datenfestplatten.

Ob Sie Blobs blockieren oder Seitenblobs auswählen, können Applikationen BLOB-Daten auf verschiedene Arten zugreifen. Die folgenden: Optionen

- Direkt über eine RESTful (d. h., HTTP-basierte) Protokoll zugreifen. Sowohl Azure Applications und externen Anwendungen, einschließlich apps lokal, ausführen können diese Option verwenden.
- Mithilfe der Azure-Speicher-Client-Bibliothek bietet die eine weitere Entwicklertools geeignete Schnittstelle über die unformatierten Rest Blob Access Protocol. Erneut können sowohl Azure Applications und externen Applikationen Blobs mit dieser Bibliothek zugreifen.
- Verwenden von Azure Laufwerke, eine Option, mit der Anwendung Azure Seitenblob als ein lokales Laufwerk mit einem NTFS-Dateisystem zu behandeln. Mit der Anwendung sieht eine einfache Windows-Dateisystem mit standard File i/o zugegriffen das Seitenblob. Tatsächlich liest und schreibt an der zugrunde liegenden Seitenblob gesendet werden, die das Laufwerk Azure implementiert. 

Zum Schutz vor Hardware-Fehlern und die Verfügbarkeit verbessern, wird jede Blob auf drei Computern in einer Azure Datacenter repliziert. Das Schreiben in ein Blob aktualisiert alle der drei Kopien, damit spätere liest inkonsistente Ergebnisse angezeigt werden. Sie können auch angeben, dass eine Blob-Daten in einer anderen Azure Datacenter in der gleichen Geo jedoch mindestens 500 Meilen abwesend kopiert werden sollen. Diese kopieren, aufgerufen *Geo-Replikation*, innerhalb weniger Minuten eines Updates für dieses Blob geschieht, und es ist sinnvoll, für die Wiederherstellung.

Daten in Blobs können auch über das Azure *Content Delivery Network (CDN)*verfügbar gemacht werden. Durch Zwischenspeichern Kopien von BLOB-Daten am Dutzende Server auf der ganzen Welt, kann das CDN beschleunigen Zugriff auf Informationen, die wiederholt zugegriffen werden kann. 

Einfache, wie sie sind, sind Blobs in vielen Situationen die richtige Wahl. Speichern und streaming video und audio sind offensichtlich Beispiele, wie Sicherungskopien und andere Arten von Daten zu archivieren. Entwickler können beliebige unstrukturierten Daten aufnehmen, die sie mögen auch Blobs. Gibt es eine einfache Möglichkeit zum Speichern und binäre Daten zugreifen kann überraschend hilfreich sein.


## <a name="a-namedbinvmarunning-a-dbms-in-a-virtual-machine"></a><a name="dbinvm"></a>Ausführen eines DBMS in einer virtuellen Computern

Viele Clientanwendungen aufsetzen heute auf eine Art von Datenbank Managementsystem (DBMS). Relationale Systeme wie SQL Server sind die am häufigsten verwendeten Wahlmöglichkeiten ab, doch nicht relationalen Vorgehensweisen, bekannt als *NoSQL* Technologien beliebter jeden Tag. Um Cloud Applikationen verwenden diese Daten Verwaltungsoptionen lassen, Azure-virtuellen Computern können Sie ein DBMS ausführen (relationale oder NoSQL) in einen virtuellen Computer. [Abbildung 2](#Fig2) zeigt, wie dies mit SQL Server sieht aus.

<a name="Fig2"></a>![Diagramm der SQLServer auf virtuellen Computers][SQLSvr-vm]
 
**Abbildung 2: Azure-virtuellen Computern ermöglicht dem ein DBMS ausgeführt wird, in einen virtuellen Computer mit Beibehaltung von Blobs bereitgestellt.**

Für Entwickler und Datenbank-Administratoren sieht dieses Szenario ähnlich wie mit der gleichen Software in ihrem eigenen Rechenzentrum aus. Im hier gezeigten Beispiel z. B. nahezu alle SQL Server Funktionen verwendet werden können, und haben Sie vollen administrativen Zugriff auf das System. Sie können auch den Zuständigkeitsbereich Datenbankserver natürlich verwalten, als wäre sie lokal ausgeführt wurden.

[Abbildung 2](#Fig2) angezeigt wird, auf der lokalen Festplatte des den virtuellen Computer gespeichert werden soll, wenn der Server ausgeführt, in wird die Datenbanken angezeigt. Im Hintergrund wird jedoch jede diese Datenträger in einer Azure Blob geschrieben. (Es ist ähnlich wie bei der Verwendung von einem SANs in Ihrem eigenen Datencenter, mit einer Blob ähnlich wie eine LUN fungiert.) Wie mit einem beliebigen Azure Blob, die darin enthaltenen Daten innerhalb eines Datencenters dreimal repliziert werden, und wenn Sie dies zu einem anderen Datacenter in derselben Region Geo repliziert anfordern. Es ist es möglich, Optionen wie Spiegelung für verbesserte Zuverlässigkeit SQL Server-Datenbank zu verwenden. 

Eine weitere Möglichkeit zur Verwendung von SQL Server in einen virtuellen besteht im Erstellen einer Hybrid-Anwendungs, in dem die Daten auf Azure verfügbar ist, während die Logik der lokalen ausgeführt wird. Beispielsweise möglicherweise dies aussagekräftig an mehreren Speicherorten oder auf verschiedenen mobilen Geräten ausgeführt Applications dieselben Daten verwenden müssen. Um Kommunikation zwischen die Cloud-Datenbank und lokale Logik einfacher zu gestalten, können eine Organisation Azure-virtuellen Netzwerk Sie eine Verbindung virtuelles privates Netzwerk (VPN) zwischen einer Azure Datacenter und einem eigenen lokalen Datacenter erstellen.


## <a name="a-namesqldbasql-database"></a><a name="sqldb"></a>SQL-Datenbank

Für viele Personen ist Ausführen eines DBMS in einen virtuellen die erste Option, die für die Verwaltung von strukturierte Daten in der Cloud Gedanke aus. Es ist nicht die einzige Wahl, jedoch noch immer die beste Option dar. In einigen Fällen ist die Verwaltung von Daten mithilfe einer Plattform als einen Ansatz Service (PaaS) sinnvoller. Azure bietet eine PaaS Technologie SQL-Datenbank, in dem Sie die Aktion für relationale Daten können. [Abbildung 3](#Fig3) zeigt diese Option. 

<a name="Fig3"></a>![Diagramm der SQL-Datenbank][SQL-db]
 
**Abbildung 3: SQL-Datenbank bietet einen freigegebenen PaaS relationale Speicher.**

SQL-Datenbank nicht einzelnen Kunden in einem eigenen physischen Instanz von SQL Server sehr. In diesem Fall bietet es einen Dienst mit mehreren Mandanten einer logischen SQL-Datenbankserver für jeden Kunden. Alle Kunden freigeben der berechnen und Speicherkapazität, der der Dienst bereitstellt. Und wie bei Blob-Speicher auf drei separaten Computern innerhalb einer Azure Datencenters, zugewiesen Ihrer Datenbanken integrierten hohen Verfügbarkeit aller Daten in der SQL-Datenbank gespeichert.

Zur Anwendung sieht die SQL-Datenbank ähnlich wie SQL Server aus. Applikationen können Ausführen von SQL-Abfragen in relationalen Tabellen, gespeicherte T-SQL-Prozeduren verwenden und Ausführen von Transaktionen über mehrere Tabellen hinweg. Und da Applikationen SQL-Datenbank mit dem Tabular Data Stream (TDS)-Protokoll Zugriff auf SQL Server, verwendet dasselbe Protokoll zugreifen können sie mit Daten mithilfe von Entität Framework, ADO.NET, JDBC und andere vertraut Daten-Access-Schnittstellen funktionieren. 

Aber da SQL-Datenbank auf einen Clouddienst in Azure Data Center ausgeführt wird, müssen Sie nicht physischen Aspekte des Systems, wie z. B. Datenträgerverwendung zu verwalten. Sie benötigen keine Gedanken Software aktualisieren oder andere Low-Level Verwaltungsaufgaben behandeln. Jede Kundenorganisation steuert weiterhin seine eigenen Datenbanken, natürlich, einschließlich ihrer Schemas und Benutzernamen, aber viele der banalen administrativen Aufgaben, die für Sie erledigt werden. 

Während der SQL-Datenbank viel SQL Server Applications aussieht, wird nicht es genau wie ein DBMS auf einer physischen oder virtuellen Computern ausgeführt anders. Da es für freigegebene Hardware ausgeführt wird, variiert die Leistung von allen seiner Kunden auf die Hardware Auslastung. Dies bedeutet, dass die Leistung von, sagen Sie eine gespeicherte Prozedur in SQL-Datenbank von einem Tag zur nächsten variieren kann. 

Heute, können mit SQL-Datenbank Sie eine Datenbank mit bis zu 150 Gigabyte zu erstellen. Wenn Sie mit größeren Datenbanken arbeiten müssen, bietet der Dienst eine Option namens *Föderation*. Hierzu erstellt ein Datenbankadministrator zwei oder mehr *Föderation Mitglieder*, von denen jede einer separaten Datenbank mit einem eigenen Schemas ist. Daten ist auf diese Mitglieder verteilt, die häufig als *Sharding*, für jedes Element einen eindeutigen *Schlüssel Föderation*zugewiesen bezeichnet wird. Eine Anwendung Probleme SQL-Abfragen für diese Daten mit dem Föderation Key, der das Element Föderation die Abfrage identifiziert sollte adressieren. Dadurch wird eine herkömmliche relationale Ansatz mit große Datenmengen mit. Es gibt wie immer vor-und Nachteile. weder Abfragen noch Transaktionen können beispielsweise Föderation Mitglieder umfassen. Wenn eine relationale PaaS-Diensts die beste Option dar ist, und diese Kompromisse zulässig sind, mit SQL-Föderation kann jedoch eine gute Lösung.

SQL-Datenbank kann von Applications Azure oder an anderer Stelle, z. B. in Ihrem lokalen Datacenter ausgeführt verwendet werden. Dies ist es sinnvoll für Applikationen Cloud, die relationale Daten benötigen, als auch lokale Anwendungen, die Daten in der Cloud speichern nutzbringend können. Eine mobile Anwendung hängen eventuell von SQL-Datenbank zum Verwalten von freigegebener relationaler Daten, beispielsweise als möglicherweise Inventory-Anwendung, die an mehrere Händler auf der ganzen Welt ausgeführt wird.

Denken SQL-Datenbank löst ein Problem offensichtlich (und wichtige): Wenn sollten Sie SQL Server im eines virtuellen Computers und besser geeignet ist ein SQL-Datenbank ausführen? Wie gewohnt, es gibt vor-und Nachteile, und daher über die erforderliche Vorgehensweise besser sind, hängt von Ihren Anforderungen. 

Eine einfache Methode, um darüber nachdenken ist zum Anzeigen der SQL-Datenbank als neue Anwendungsmöglichkeiten, solange SQL Server auf einen virtuellen besser geeignet ist, wenn Sie eine vorhandene Anwendung in der Cloud lokalen verschieben. Sie können auch bei dieser Entscheidung mehr abgestimmte anzuordnen, jedoch aussehen hilfreich sein. SQL-Datenbank beträgt beispielsweise einfacher zu verwenden, da es minimal ist, Setup und Verwaltung. Aber mit SQL Server in einen virtuellen kann weitere vorhersehbar Leistung – es ist nicht freigegebener - Dienst und auch unterstützt größere nicht-verbundene Datenbanken als SQL-Datenbank. SQL-Datenbank enthält immer noch, integrierte Replikation der Daten und Verarbeitung, effektiv Sie eine hohe Verfügbarkeit DBMS mit wenig Arbeit zugewiesen. Während der SQL Server Sie mehr Kontrolle und eine etwas umfassendere Reihe von Optionen bietet, ist die SQL-Datenbank einfacher, erheblich weniger Arbeit ordnungsgemäß zu verwalten.

Schließlich ist es wichtig, darauf hinzuweisen, dass SQL-Datenbank nicht nur PaaS Datendienst Azure verfügbar ist. Microsoft-Partner bieten auch die anderen Optionen. Angenommen, bietet ClearDB eine MySQL PaaS Geschenk, während Cloudant eine Option NoSQL verkauft. Datendienste PaaS sind die richtige Lösung in vielen Situationen und somit dieser Ansatz zur datenverwaltung ist ein wichtiger Azure.


### <a name="a-namedatasyncasql-data-sync"></a><a name="datasync"></a>Synchronisieren der SQL-Daten

Während der SQL-Datenbank drei Kopien der einzelnen Datenbanken in einem einzigen Azure Rechenzentrum unterhält, wird nicht es automatisch Daten zwischen Azure Rechenzentren repliziert. In diesem Fall bietet es sich um SQL Daten synchronisieren, einem Dienst, den Sie dazu verwenden können. [Abbildung 4](#Fig4) zeigt, wie folgt aussieht.

<a name="Fig4"></a>![Diagramm der SQL-Daten synchronisieren][SQL-datasync]
 
**Abbildung 4: SQL Daten synchronisieren synchronisiert Daten in der SQL-Datenbank mit Daten in anderen Azure und lokale Rechenzentren.**

Wie das Diagramm angezeigt wird, können die Daten synchronisieren SQL Daten an unterschiedlichen Standorten synchronisieren. Nehmen Sie an, dass Sie eine Anwendung in mehreren Azure Rechenzentren, z. B. mit Daten in SQL-Datenbank ausführen. SQL-Daten synchronisieren können Sie die Daten synchronisiert bleiben. Synchronisieren der SQL-Daten können auch Daten zwischen einer Azure Datacenter und einer Instanz von SQL Server ausgeführt wird, in einem lokalen Rechenzentrum synchronisieren. Dies möglicherweise hilfreich, eine lokale Kopie der Daten von Applications lokal verwendet, und eine Cloud-Kopie von Applications auf Windows Azure ausgeführte verwendet. Und auch wenn es nicht in der Abbildung dargestellt ist, SQL-Daten synchronisieren können auch verwendet werden Synchronisieren von Daten zwischen SQL-Datenbank und SQL Server ausgeführt wird, in einen virtuellen Azure oder an anderer Stelle.

Synchronisierung bidirektionale werden kann, und Sie ermitteln, genau, welche Daten synchronisiert werden und wie häufig erfolgt. (Synchronisierung zwischen Datenbanken nicht atomaren ist, – es ist jedoch immer mindestens einige Verzögerung). Und Einrichten der Synchronisierung mit SQL Daten synchronisieren, doch es verwendet wird, ist vollständig Konfiguration leistungsgesteuert; Es gibt keinen Code zu schreiben.


### <a name="a-namedatarptasql-data-reporting-using-virtual-machines"></a><a name="datarpt"></a>SQL Berichterstattung Daten mithilfe von virtuellen Computern

Nachdem Sie eine Datenbank Daten enthält, wird natürlich wahrscheinlich Berichte mithilfe dieser Daten erstellen möchten. Azure ausgeführt werden kann SQL Server Reporting Services (SSRS) in Azure virtuellen Computern, die funktional dem lokalen SQL Server Reporting Services ausgeführt wird. Dann können Sie SSRS zum Ausführen von Berichten auf Daten in einer SQL Azure-Datenbank gespeichert.  [Abbildung 5](#Fig5) zeigt, wie der Prozess funktioniert.

<a name="Fig5"></a>![Diagramm der SQL-Berichterstattung][SQL-report]
 
**Abbildung 5: SQL Server Reporting Services ausgeführt in einer Azure-virtuellen Computern bietet reporting Services für die Daten in der SQL-Datenbank. .**

Bevor ein Benutzer einen Bericht sehen kann, definiert jemand dieses Berichts (Schritt 1) aussehen sollte. Mit SSRS eines virtuellen Computers, dies kann mithilfe von zwei Tools: SQL Server Data Tools, Teil von SQL Server 2012 oder Vorgänger, Business Intelligence (BI) Development Studio. Wie bei SSRS, werden diese Berichtsdefinitionen in der Sprache (RDL) ausgedrückt. Nachdem die RDL-Dateien für einen Bericht erstellt wurden, werden diese in einen virtuellen Computer in der Cloud (Schritt 2) hochgeladen werden. Die Definition des Berichts kann nun verwenden.

Ein Benutzer der Anwendung greift als Nächstes auf den Bericht (Schritt 3) zu. Die Anwendung übergibt diese Anforderung der SSRS VM (Schritt 4), die SQL-Datenbank oder anderen Datenquellen, die Daten zu erhalten, es muss, Kontakte (Schritt 5). SSRS verwendet diese Daten, und die relevanten RDL-Dateien des Berichts (Schritt 6), klicken Sie dann gerendert gibt den Bericht mit der Anwendung (Schritt 7), die sie für den Benutzer (Schritt 8) anzeigt.

Einbetten eines Berichts in einer Anwendung, das hier gezeigte Szenario nicht nur die Option. Es kann auch zum Anzeigen von Berichten im Berichts-Manager des virtuellen Computers, SharePoint des virtuellen Computers oder auf andere Weise. Berichte können mit einem Bericht mit einem Link zu einem anderen auch kombiniert werden.

Ein Azure-virtuellen Computers SSRS bietet Ihnen umfassende Funktionalität als reporting-Lösung in der Cloud. Berichte können alle Datenquellen von SSRS unterstützt. Applikationen und Berichte können eingebettetem Code oder Assemblys zur Unterstützung von benutzerdefinierter Verhalten einbeziehen. Ausführung von Berichten und Rendern sind schnell, da der Server Melden von Inhalten und -Engine zusammen auf demselben virtuellen Server ausgeführt werden.



## <a name="a-nametblstoratable-storage"></a><a name="tblstor"></a>Table Storage

Relationale Daten ist in vielen Situationen nützlich, aber es ist nicht immer die richtige Wahl. Wenn die Anwendung schnellen und einfachen Zugriff auf sehr großen Mengen von grob strukturierte Daten benötigt, beispielsweise eine relationale Datenbank funktioniert möglicherweise nicht gut. Wahrscheinlich eine bessere Option, um eine NoSQL Technologie handelt.

Azure Table Storage ist ein Beispiel für diese Art von NoSQL Ansatz. Entgegen dem Namen unterstützt Tabellenspeicher keine standardmäßige relationale Tabellen. In diesem Fall bietet es einen *Schlüssel/Wert speichern*, wird eine Reihe von Daten mit einem bestimmten Schlüssel, und klicken Sie dann ganz eine Anwendung zugreifen können, die Daten sogenannte können, indem Sie die Taste. [Abbildung 6](#Fig6) veranschaulicht die Grundlagen.

<a name="Fig6"></a>![Diagramm Tabellenspeicher][SQL-tblstor]
 
**Abbildung 6: Azure Table Storage ist ein Schlüssel/Wert-Speicher, der schnellen und einfachen Zugriff auf große Mengen von Daten enthält.**

Wie Blobs für jede Tabelle ein Azure-Speicher-Konto zugeordnet ist. Tabellen werden auch ähnlich wie Blobs, mit einer URL des Formulars bezeichnet.

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*Tabellenname*&gt;

Wie in die Abbildung dargestellt, wird jede Tabelle in einigen Anzahl von Partitionen, unterteilt jeweils auf einem separaten Computer gespeichert werden kann. (Dies ist eine Form von Sharding, wie bei SQL Föderation.) Sowohl Azure Applications-Anwendung, die an anderer Stelle ausgeführt können eine Tabelle mit den Rest OData-Protokoll oder der Azure-Speicher-Client-Bibliothek zugreifen.

Jede in einer Tabelle enthält einige Anzahl der *Einheiten*, jede mit bis zu 255 *Eigenschaften*. Jede Eigenschaft hat einen Namen, einen Typ (z. B. Binärzahl, Bool, DateTime, Int oder Zeichenfolge) und ein Wert an. Im Gegensatz zu relationale Speicher in diesen Tabellen haben keine feste Schema und damit andere Personen in der gleichen Tabelle können Eigenschaften mit unterschiedlichen Datentypen enthalten. Eine Entität möglicherweise müssen Sie nur eine Zeichenfolgeneigenschaft, die einen Namen ein, beispielsweise zwar eine andere Person in der gleichen Tabelle zwei Int-Eigenschaften, die mit einer Kunden-ID-Zahl und eine Bonität verfügt enthält.

Wenn Sie um eine bestimmte Entität innerhalb einer Tabelle zu identifizieren, stellt eine Anwendung dieser Entität-Taste. Der Schlüssel besteht aus zwei Teilen: ein *Partitionsschlüssel* , angibt, einer bestimmten Partition und ein *Zeile-Taste* , eine Entität innerhalb der Partition angibt. [Abbildung 6](#Fig6)beispielsweise der Client fordert die Entität mit Partitionsschlüssel A und Zeile 3-Taste, und Tabellenspeicher gibt die Entität, einschließlich aller darin enthaltenen Eigenschaften.

Mithilfe dieser Struktur können Tabellen groß sein – eine einzelne Tabelle kann bis zu 100 TB Daten - enthalten, und es ermöglicht schnellen Zugriff auf die darin enthaltenen Daten. Es wird jedoch auch Einschränkungen. Es gibt beispielsweise keine Unterstützung für Transaktionen Updates, die Tabellen oder sogar Partitionen in einer einzelnen Tabelle umfassen. Eine Reihe von Updates zu einer Tabelle kann nur in einer atomaren Transaktion gruppiert werden, wenn alle Beteiligten die Objekte in der gleichen Partition sind. Es ist auch keine Möglichkeit, eine Tabelle Abfragen basierend auf dem Wert der Formulareigenschaften, es gibt auch Unterstützung für Verknüpfungen über mehrere Tabellen hinweg. Und im Gegensatz zu relationalen Datenbanken Tabellen keine Unterstützung für gespeicherte Prozeduren.

Azure Table Storage ist eine gute Wahl für Applikationen, die benötigen, schnell und effizient auf große Mengen von grob strukturierte Daten zuzugreifen. Eine Internet-Anwendung, die Profilinformationen für zahlreiche Benutzer speichert möglicherweise beispielsweise Tabellen verwenden. Schneller Zugriff ist in diesem Fall wichtig, und die Anwendung möglicherweise nicht den ganzen Leistungsumfang von SQL. Einrichten dieser Funktionen zu gewinnen Geschwindigkeit und Größe zugewiesen kann manchmal sinnvoll ist und somit Tabellenspeicher nur die richtige Lösung für einige Probleme ist.


## <a name="a-namehadoopahadoop"></a><a name="hadoop"></a>Hadoop

Organisationen haben Daten Lager Jahrzehnten erstellt wurde. Diese Sammlungen von Informationen in den meisten Fällen relationalen Tabellen gehörende Kehrmatrix zulassen, dass Personen, die mit arbeiten und Informationen aus Daten auf verschiedene Weise. Mit SQL Server ist es beispielsweise üblich, Tools, wie etwa SQL Server Analysis Services dazu verwenden.

Aber nehmen Sie an, dass die Analyse nicht relationaler Daten geschehen soll. Ihre Daten möglicherweise viele Formen annehmen: Informationen aus Sensoren oder RFID Kategorien, Protokolldateien in Serverfarmen von Webanwendungen erzeugten Clickstream-Daten Bilder aus medizinischen Diagnose Geräte und vieles mehr. Diese Daten möglicherweise auch sehr groß, zu groß, um effektiv mit einem herkömmlichen Datawarehouse verwendet werden. Große Daten wie folgt, seltene vor wenigen Jahren sind jetzt ganz allgemeine geworden.

Um diese Art von große Daten analysieren, wurde unsere Branche weitgehend auf einer einzigen Lösung zusammengeführt: die Open Source-Technologie Hadoop. Hadoop führt in einem Cluster aus physischen oder virtuellen Computern, Zuweisen von der Daten, an denen, die es auf diesen Computern arbeitet, und parallel verarbeiten. Die weitere Computer muss Hadoop verwenden, die schneller können sie durchführen, welchen es arbeiten ist wie folgt.

Diese Art von Problem ist ideal für die öffentliche Cloud. Statt der Pflege einer lokalen Servern, die CPU möglicherweise viel von der Zeit, Hadoop ausgeführt, in der Cloud Sie erstellen kann (und Bezahlung für) nur virtuellen Computern nur bei Bedarf. Sogar wird mehrere und vieles mehr große Daten, die Sie mit Hadoop analysieren möchten erstellt in der Cloud, speichern Sie die Probleme beim bewegen es in. Damit Sie diese Synergien nutzen können, bietet Microsoft einen Hadoop-Dienst auf Azure. [Abbildung 7](#Fig7) zeigt die wichtigsten Komponenten diesen Dienst an.

<a name="Fig7"></a>![Hadoop Diagramm][hadoop]

**Abbildung 7: Hadoop auf Azure führt MapReduce Einzelvorgänge aus, die Daten in mehrere virtuelle Computer mit Parallel verarbeiten.**

Hadoop auf Azure um zu verwenden, bitten Sie zuerst dieser Cloudplattform zum Erstellen eines Clusters Hadoop angibt, virtuellen Computern, die Sie benötigen. Einrichten von einem Hadoop Cluster selbst ist eine komplexe Aufgabe, und ist sinnvoll, also lassen Sie Azure. Wenn Sie damit fertig sind mit dem Cluster, beenden Sie ihn. Es gibt keine bezahlen berechnen Ressourcen, die Sie verwenden müssen.

Eine Hadoop-Anwendung, einen *Auftrag*, der so genannte verwendet ein Modell Programmierung als *MapReduce*bezeichnet. Wie in die Abbildung dargestellt, wird die Logik für ein Projekt MapReduce gleichzeitig über vieler virtueller Computer ausgeführt. Verarbeiten von Daten parallel, kann Hadoop Daten sehr viel schneller als Lösungen mit einem einzelnen Computer analysieren.

Auf Azure ist die Daten einer MapReduce Position auf funktioniert in der Regel im BLOB-Speicher gespeichert. Hadoop erwarten jedoch MapReduce Aufträge Daten in der *Hadoop Distributed Datei System (HDFS)*gespeichert werden. HDFS ähnelt Blob-Speicher einige Methoden ist. Es repliziert Daten auf mehreren physischen Servern, beispielsweise. Statt diese Funktionalität erreichen, macht Hadoop auf Azure BLOB-Speicher über die HDFS-API, stattdessen wie in der Abbildung dargestellt. Während die Logik in einem Auftrag MapReduce geht davon aus, dass sie einfache HDFS Dateien zugreifen ist, arbeitet der Auftrag wirklich mit Daten darauf von Blobs gestreamt. Und zur Unterstützung des Fall, in dem mehrere Aufträge für dieselben Daten ausgeführt werden, auch Hadoop auf Azure erlauben Kopieren von Daten aus Blobs in vollständigen HDFS in den virtuellen Computern ausgeführt. 

MapReduce Aufträge werden häufig in Java heute, einen Ansatz geschrieben, den Hadoop auf Azure unterstützt. Microsoft hat auch Unterstützung beim Erstellen von MapReduce Aufträge in anderen Sprachen, einschließlich c#, F- und JavaScript hinzugefügt. Ziel ist es, diese Technologie große Daten einer größeren Gruppe von Entwicklern leichter zugänglich machen.

Zusammen mit HDFS und MapReduce umfasst Hadoop andere Technologien, die Personen Analysieren von Daten ohne Schreiben eines Auftrags MapReduce selbst. Schwein beträgt beispielsweise einer allgemeinen Programmiersprache zum Analysieren großer Datenverlustvorfalls zwar Struktur eine SQL-ähnliche Sprache aufgerufen HiveQL bietet entwickelt. Schwein, und Struktur generieren tatsächlich MapReduce Einzelvorgänge, die HDFS Daten zu verarbeiten, aber sie diese Komplexität von ihren Benutzern ausblenden. Beide werden mit Hadoop auf Azure bereitgestellt.

Microsoft bietet auch einen HiveQL Treiber für Excel. Verwenden ein Excel-add-in, können Business Analysten HiveQL Abfragen (und somit MapReduce Aufträge) direkt in Excel erstellen und dann verarbeitet und Visualisieren der Ergebnisse mithilfe von PowerPivot und anderer Excel-Tools. Hadoop auf Azure enthält auch andere Technologien wie der Computer learning Bibliotheken Mahout, das Diagramm Mining System Pegasus und mehr.

Große Datenanalyse ist wichtig, und somit auch Hadoop wichtig ist. Durch die Bereitstellung Hadoop als verwalteter Dienst auf Azure, sowie Links zu vertraute Tools wie Excel Ziel Microsoft diese Technologie bei einer größeren Gruppe von Benutzern zugänglich ist.

Alle Arten von Daten ist breiterer, wichtig. Dies ist, warum Azure eine Reihe von Optionen für die Verwaltung und Business Analytics von Daten enthält. Jeden Anwendung, die Sie erstellen gerade, ist es wahrscheinlich finden Sie etwas in dieser Cloudplattform, die funktionieren für Sie.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
