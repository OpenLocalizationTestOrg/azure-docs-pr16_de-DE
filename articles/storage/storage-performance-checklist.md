<properties
    pageTitle="Azure Speicher Leistung und Skalierbarkeit Checkliste | Microsoft Azure"
    description="Checkliste für bewährte Methoden für die Verwendung mit Azure-Speicher in leistungsfähigen Anwendungen entwickeln."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Microsoft Azure-Speicher Leistung und Skalierbarkeit Checkliste

## <a name="overview"></a>(Übersicht)
Microsoft hat eine Anzahl von bewährte Methoden zur Verwendung dieser Services leistungsstarke entwickelt seit der Veröffentlichung der Dienste Microsoft Azure-Speicher und in diesem Artikel dient zum Konsolidieren von der von ihnen besonders wichtig in einer Liste Checkliste-Schreibweise. Der Zweck dieses Artikels besteht darin, überprüfen Sie, ob die verwendeten bewährter Methoden mit Azure-Speicher Entwickler Hilfe und dabei unterstützen sollen, andere bewährten Methoden identifizieren, die Sie eingeführt berücksichtigen sollten. In diesem Artikel nicht versucht, jede mögliche Optimierung der Leistung und Skalierbarkeit Deckblatt – es ausgeschlossen sind diejenigen, die in deren Einfluss klein oder nicht gestreut verfügbar sind. Wenn das Verhalten der Anwendung während des Entwurfs regressionsgleichung werden kann, ist es sinnvoll, beachten frühzeitig, um Designs zu vermeiden, die auf Leistungsprobleme ausgeführt werden soll.  

Jeder Anwendungsentwickler mit Azure Storage dauert die Zeit, lesen Sie diesen Artikel, und überprüfen Sie, dass vertretene Anwendung einzelnen aufgeführten bewährten Methoden befolgt.  

## <a name="checklist"></a>Checkliste
In diesem Artikel werden die bewährten Methoden in die folgenden Gruppen organisiert. Bewährte Methoden anwendbar:  

-   Alle Azure Storage Services (Blobs, Tabellen, Warteschlangen und Dateien)
-   BLOBs
-   Tabellen
-   Warteschlangen  

|Fertig|  Bereich|   Kategorie|   Frage
|----|------|-----------|-----------
||Alle Dienste| Skalierbarkeit Ziele|[Dient Ihrer Anwendung Annäherung an den Skalierbarkeit Ziele vermeiden?](#subheading1)
||Alle Dienste| Skalierbarkeit Ziele|[Soll die Ihrer Benennungskonvention besser Lastenausgleich aktivieren?](#subheading47)
||Alle Dienste| Netzwerke| [Habe Client-Seite Geräte ausreichend große Bandbreite und geringe Wartezeit erzielen die Leistung erforderlich?](#subheading2)
||Alle Dienste| Netzwerke| [Gehen Sie wie folgt Clients auf Geräten hoch genug Qualität über verfügen einen Link?](#subheading3)
||Alle Dienste| Netzwerke| [Befindet sich die Clientanwendung "am" im Speicher-Konto?](#subheading4)
||Alle Dienste| Verteilen von Inhalten|   [Verwenden Sie eine CDN zur Verteilung von Inhalten?](#subheading5)
||Alle Dienste| Direkter Clientzugriff|   [Sind Sie direkten Zugriff auf Speicher statt Proxy dürfen-SAS- und CORS verwenden?](#subheading6)
||Alle Dienste| Zwischenspeichern|    [Ist der Anwendung Zwischenspeichern von Daten, die wiederholt verwendet wird und die Änderungen selten?](#subheading7)
||Alle Dienste| Zwischenspeichern|    [Batchverarbeitung die Anwendung, die Updates (clientseitige Zwischenspeichern, und klicken Sie dann in größere hochladen)?](#subheading8)
||Alle Dienste| .NET Konfiguration| [Haben Sie Ihren Kunden, um eine ausreichende Anzahl der aktiven Verbindungen verwenden konfiguriert?](#subheading9)
||Alle Dienste| .NET Konfiguration| [Haben Sie .NET um eine ausreichende Anzahl von Threads verwenden konfiguriert?](#subheading10)
||Alle Dienste| .NET Konfiguration| [Verwenden Sie .NET 4.5 oder höher, wurde die Garbagecollection verbessert?](#subheading11)
||Alle Dienste| Parallelität|    [Haben Sie sichergestellt, dass Parallelism ordnungsgemäß begrenzt ist, dass Sie entweder die Clientfunktionen oder der Skalierbarkeit Ziele überladen nicht?](#subheading12)
||Alle Dienste| Tools|  [Sind Sie mit der neuesten Version von Microsoft Clientbibliotheken und Tools zur Verfügung gestellt?](#subheading13)
||Alle Dienste| Wiederholungsversuche|    [Sind Sie mit einer exponentiellen Backoff Richtlinie für begrenzungsebene Fehlern und Zeitlimit wiederholen?](#subheading14)
||Alle Dienste| Wiederholungsversuche|    [Ist der Anwendung vermeiden von Wiederholungsversuche nicht Aufforderung zum Wiederholen Fehler?](#subheading15)
||BLOBs|    Skalierbarkeit Ziele|    [Haben Sie eine große Anzahl von Clients, die Zugriff auf ein einzelnes Objekt gleichzeitig?](#subheading46)
||BLOBs|    Skalierbarkeit Ziele|    [Ist eine Anwendung innerhalb des Bandbreite oder Vorgänge Skalierbarkeit Ziels für ein einzelnes Blob für Ihr Unternehmen?](#subheading16)
||BLOBs|    Kopieren von Blobs|  [Sind kopieren Sie Blobs effizient?](#subheading17)
||BLOBs|    Kopieren von Blobs|  [Verwenden Sie AzCopy für Massenkopien von Blobs?](#subheading18)
||BLOBs|    Kopieren von Blobs|  [Verwenden Sie Azure importieren/exportieren sehr große Datenmengen übertragen?](#subheading19)
||BLOBs|    Verwenden von Metadaten|   [Speichern Sie häufig verwendete Metadaten zu Blobs in ihren Metadaten?](#subheading20)
||BLOBs|    Schnelles hochladen| [Wenn Sie versuchen, eine Blob schnell hochladen, werden Sie Blöcke parallel hochladen?](#subheading21)
||BLOBs|    Schnelles hochladen| [Wenn Sie versuchen, viele Blobs schnell hochladen, werden Sie Blobs parallel hochladen?](#subheading22)
||BLOBs|    Korrigieren von BLOB-Typ|  [Verwenden Sie Seitenblobs oder blockieren Blobs bei Bedarf?](#subheading23)
||Tabellen|   Skalierbarkeit Ziele|    [Sind Sie der Skalierbarkeit Ziele für Personen pro Sekunde Annäherung an?](#subheading24)
||Tabellen|   Konfiguration|  [Verwenden Sie für Ihre Tabelle Anfragen JSON?](#subheading25)
||Tabellen|   Konfiguration|  [Haben Sie Nagle deaktiviert für optimale Leistung von kleinen Anfragen?](#subheading26)
||Tabellen|   Tabellen und Partitionen|  [Haben Sie Ihre Daten ordnungsgemäß aufgeteilt?](#subheading27)
||Tabellen|   Wichtiges Partitionen| [Vermeiden Sie nur Anfügen und nur vorangestellt Muster?](#subheading28)
||Tabellen|   Wichtiges Partitionen| [Werden Ihre fügt-Updates sind viele partitionsübergreifend verteilt?](#subheading29)  
||Tabellen|   Abfragebereich|    [Haben Sie Ihr Schema für Punkt-Abfragen, in den meisten Fällen verwendet werden soll, und Tabellenabfragen nur in Ausnahmefällen verwendet werden dürfen entworfen?](#subheading30)
||Tabellen|   Einer Abfrage|  [Führen Sie Ihre Abfragen in der Regel nur Scan und Zurückgeben von Zeilen, die in Ihrer Anwendung verwendet wird?](#subheading31)
||Tabellen|   Einschränkenden zurückgegebenen Daten| [Verwenden Sie filtern zu vermeiden, zurückgeben Einheiten, die nicht benötigt werden?](#subheading32)
||Tabellen|   Einschränkenden zurückgegebenen Daten| [Verwenden Sie Projektion zur Vermeidung Zurückgeben von Eigenschaften, die nicht benötigt werden?](#subheading33)
||Tabellen|   Denormaliserung|    [Haben Sie die Daten so, dass Sie beim Abrufen von Daten nicht effizient Abfragen oder aus mehreren gelesen Anforderungen vermeiden denormalisiert?](#subheading34)
||Tabellen|   Einfügen, aktualisieren und löschen|   [Sind Sie Batchverarbeitung anfordert, müssen Transaktionen oder auch zur gleichen Zeit Schleifen verringern durchgeführt werden?](#subheading35)
||Tabellen|   Einfügen, aktualisieren und löschen|   [Vermeiden Sie beim Abrufen einer Entität einfach, um festzustellen, ob aufrufen, einfügen oder aktualisieren?](#subheading36)
||Tabellen|   Einfügen, aktualisieren und löschen|   [Haben Sie speichern Datenreihe, die häufig abgerufen wird zusammen in einer einzelnen Entität als Eigenschaften anstelle von mehreren Personen berücksichtigt?](#subheading37)
||Tabellen|   Einfügen, aktualisieren und löschen|   [Für Personen, die immer zusammen abgerufen und in Stapeln (z. B. Zeitdaten Reihe) geschrieben werden können, haben Sie die Verwendung von Blobs anstelle von Tabellen berücksichtigt?](#subheading38)
||Warteschlangen|   Skalierbarkeit Ziele|    [Sind Sie der Skalierbarkeit Ziele für Nachrichten pro Sekunde Annäherung an?](#subheading39)
||Warteschlangen|   Konfiguration|  [Haben Sie Nagle deaktiviert für optimale Leistung von kleine Anfragen?](#subheading40)
||Warteschlangen|   Nachrichtengröße|   [Sind Ihre Nachrichten komprimieren steigern die Leistung der Warteschlange?](#subheading41)
||Warteschlangen|   Massen abrufen|  [Rufen Sie mehrere Nachrichten in einem Vorgang "Abrufen"?](#subheading42)
||Warteschlangen|   Häufigkeit von Umfragen|  [Werden Sie häufig genug abrufen, um die angenommene Wartezeit Ihrer Anwendung zu verringern?](#subheading43)
||Warteschlangen|   Aktualisieren der Nachricht| [Verwenden Sie zum Speichern des Vorgangsfortschritts in Bearbeitung von Nachrichten, vermeiden, müssen die gesamte Nachricht erneut verarbeiten, falls ein Fehler auftritt UpdateMessage?](#subheading44)
||Warteschlangen|   Architektur|   [Verwenden Sie Warteschlangen die gesamte Anwendung besser skalierbar nach langer Auslastung außerhalb des kritischen Wegs planmäßigen und dann unabhängig voneinander skalieren?](#subheading45)


##<a name="a-nameallservicesaall-services"></a><a name="allservices"></a>Alle Dienste
Dieser Abschnitt listet die bewährte Methoden, die für die Verwendung der Dienste Azure-Speicher (Blobs, Tabellen, Warteschlangen oder Dateien) verfügbar sind.  

###<a name="a-namesubheading1ascalability-targets"></a><a name="subheading1"></a>Skalierbarkeit Ziele
Jede der Dienste Azure Storage verfügt über Skalierbarkeit Ziele für Kapazität (GB), Transaktion Zins und Bandbreite. Wenn die Anwendung Ansätze oder beliebige Ziele Skalierbarkeit überschreitet, können sie höhere Transaktion Wartezeiten oder begrenzungsebene auftreten. Wenn eine Speicherdienst Ihrer Anwendung Steuerung, beginnt der Dienst "503 Server ist ausgelastet" oder "500 Vorgang Timeout" Fehlercodes für einige Speichertransaktionen zurückgeben. In diesem Abschnitt werden den allgemeinen Ansatz zum Umgang mit Skalierbarkeit Ziele und Bandbreite Skalierbarkeit Ziele im besonderen. Höher Abschnitte, die Speicherdienste für einzelne behandelt diskutieren Skalierbarkeit Ziele im Zusammenhang mit diesem bestimmten Dienst:  

-   [BLOB Bandbreite und Abfragen pro Sekunde](#subheading16)
-   [Tabelle Elemente pro Sekunde](#subheading24)
-   [Warteschlangennachrichten pro Sekunde](#subheading39)  

####<a name="a-namesub1bandwidthabandwidth-scalability-target-for-all-services"></a><a name="sub1bandwidth"></a>Bandbreite Skalierbarkeit Ziel für alle Dienste
Zum Zeitpunkt der schreiben sind die Bandbreitenziele in den USA für ein Speicherkonto Geo redundante (GRS) 10 GB/s pro Sekunde (Gbps) für eingehende (Daten mit dem Speicherkonto gesendet) und 20 Gbps für Ausgang (Daten aus dem Speicherkonto gesendet). Für ein Speicherkonto lokal redundante (LRS) sind die Grenzwerte höher – 20 Gbps für eingehende und 30 Gbps für Ausgang.  Internationale Bandbreite Beschränkungen möglicherweise niedriger und auf unserer [Skalierbarkeit Ziele Seite](http://msdn.microsoft.com/library/azure/dn249410.aspx)gefunden werden können.  Weitere Informationen zu den Speicheroptionen Redundanz finden Sie unter den Links unten [Nützlichen Ressourcen](#sub1useful) .  

####<a name="what-to-do-when-approaching-a-scalability-target"></a>Was zu tun ist, wenn ein Ziel Skalierbarkeit Annäherung an.
Wenn eine Anwendung die Ziele Skalierbarkeit für ein einzelnes Speicherkonto ansteht, erwägen Sie eingeführt, eine der folgenden Vorgehensweisen:  

-   Überprüfen Sie die Arbeitsbelastung, bei dem eine Anwendung zu erreichen oder überschreiten das Ziel Skalierbarkeit. Können Sie es anders mit weniger Bandbreite oder Kapazität oder weniger Transaktionen entwerfen?
-   Wenn eine Anwendung eines der Ziele Skalierbarkeit überschreiten darf, sollten Sie mehrere Speicherkonten und Partition Ihrer Anwendungsdaten über diese mehrere Speicherkonten erstellen. Wenn Sie dieses Muster verwenden, werden Sie sicherstellen, dass eine Anwendung entwerfen, sodass Sie mehr Speicherkonten in der Zukunft für den Lastenausgleich hinzufügen können. Jede Azure-Abonnement kann bis zu 100 Speicherkonten haben, Zeitpunkt des Schreibvorgangs.  Speicherkonten können ebenfalls keine Kosten als Ihre Verwendung im Hinblick auf die gespeicherten Daten, Transaktionen oder übertragene Daten.
-   Wenn die Anwendung die Bandbreitenziele erreicht, erwägen Sie das Komprimieren von Daten im Client, die Bandbreite erforderlich, um die Daten in der Speicherdienst senden zu verringern.  Beachten Sie, dass während dies möglicherweise Bandbreite gespeichert haben und verbessern die Leistung des Netzwerks, es auch einige Nachteile negative haben kann.  Sie sollten die Leistung durch diese aufgrund die weitere Verarbeitung von Anforderungen für das Komprimieren und Dekomprimieren von Daten im Client ausgewertet werden. Darüber hinaus kann Speicherns komprimierte Daten erschweren von Problemen, da es schwieriger zu gespeicherte Daten mithilfe der standard-Tools angezeigt werden konnte.
-   Wenn die Anwendung der Skalierbarkeit Ziele erreicht werden, müssen sicherstellen Sie, dass Sie eine exponentielle Backoff für Wiederholungsversuche verwenden (siehe [Wiederholungsversuche](#subheading14)).  Es empfiehlt sich zu vergewissern, dass Sie fast nie der Skalierbarkeit Ziele (mithilfe einer der oben angegebenen Methoden), aber dies stellt sicher, dass eine Anwendung nur schnell, wiederholen, wird nicht begrenzungsebene schlechter vornimmt.  

####<a name="useful-resources"></a>Nützlichen Ressourcen
Die folgenden Links bieten zusätzlichen Details auf Skalierbarkeit Ziele:
-   Informationen zu Skalierbarkeit Zielgruppen finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md) .
-   Finden Sie unter [Azure-Speicher Replikation](storage-redundancy.md) und den Blogbeitrag [Azure Redundanz Speicheroptionen und Lesezugriff Geo redundante Speicherung](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) Informationen Redundanz Speicheroptionen.
-   Aktuelle Informationen Preise für Azure-Dienste finden Sie unter [Azure Preise](https://azure.microsoft.com/pricing/overview/).  

###<a name="a-namesubheading47apartition-naming-convention"></a><a name="subheading47"></a>Partition Benennungskonvention
Azure-Speicher verwendet bereichsbasierten Partitionierungsschema zu skalieren und Laden Saldo das System. Partitionieren von Daten in den Bereichen und diese Bereiche laden-System verteilt werden, wird der Partitionsschlüssel verwendet. Dies bedeutet, dass Benennungskonventionen wie Lexikalische Sortierung (z. B. Msftpayroll, Msftperformance, Msftemployees usw.) oder mit Zeitstempel (log20160101, log20160102, log20160102 usw.) wird verleihen selbst auf die gemeinsame potenziell auf dem gleichen gefunden werden, bis eine Last Lastenausgleich Vorgang teilt diese sich in kleineren Bereichen. Beispielsweise können alle Blobs innerhalb eines Containers von einem einzigen Server bereitgestellt werden, bis die Belastung diese Blobs erfordert weiteren Qualifikationsprofilen Partition Bereiche. Auf ähnliche Weise eine Gruppe von leicht geladen Konten mit ihren Namen in der lexikalischen Reihenfolge angeordnet möglicherweise von einem einzigen Server bereitgestellt werden, bis die Belastung eine oder alle der folgenden Konten festlegen, dass diese auf mehrere Partitionen Server aufgeteilt werden. Jede Lastenausgleich Vorgang kann die Wartezeit von Speicher Anrufe während des Importvorgangs auswirken. Des Systems Möglichkeit, einen plötzlichen Paketburst Datenverkehr auf eine Partition behandeln wird durch einen einzelnen Partitionsserver der Skalierbarkeit beschränkt, bis der Lastenausgleich Vorgang springt in und gleicht Partition Key Bereich aus.  

Sie können einige bewährte Methoden zum Verringern der Häufigkeit von solche Vorgänge folgen.  

-   Überprüfen Sie die Benennungskonvention, die Sie für Konten, Container, Blobs, Tabellen und Warteschlangen, genau zu verwenden. Erwägen Sie das voranstellen Kontonamen mit einem 3-stelliges Hash mit einer hashing-Funktion, die Ihren Anforderungen am besten passt.  
-   Wenn Sie Ihre Daten mit einem Zeitstempel oder einer numerischen Bezeichnern organisieren, müssen Sie sicherstellen, dass Sie ein Muster im schreibgeschützten anfügen (oder nur vorangestellt) Datenverkehr nicht verwenden. Diese Muster eignen sich nicht für einen Zellbereich-basierten Partitionierung System und konnte Lead für den gesamten Verkehr einer Partition vertraut und das System von effektiv begrenzen Lastenausgleich. Beispielsweise wenn Sie täglichen, die ein Blob-Objekt mit einem Zeitstempel wie JJJJMMTT verwenden verfügen, wird Klicken Sie dann der gesamten Datenverkehr für diesen Vorgang tägliche auf ein einziges Objekt weitergeleitet die in einem Server Einzelpartition realisiert ist. Prüfen Sie, ob die pro Blob Grenzwerte pro Partition Grenzwerte Ihren Anforderungen, und können Sie diesen Vorgang in mehreren Blobs unterteilen, falls erforderlich. Wenn Sie eine Reihe von Zeitdaten in Ihren Tabellen speichern, geleitet sämtliche der Datenverkehr werden konnte auf ähnliche Weise auf die letzte Teil der wichtigsten Namespace. Wenn Sie mit einem Zeitstempel oder einer numerischen IDs verwenden müssen, Präfix Präfix die Id, die mit einem 3-stelliges Hash oder, im Falle werden im Webpart Sekunden der Zeitangabe wie Ssyyyymmdd. Wenn auflisten und Abfragen Vorgänge regelmäßig ausgeführt werden, wählen Sie eine hashing-Funktion, die die Anzahl der Abfragen zu beschränken, wird ein. In anderen Fällen möglicherweise eine zufällige Präfix ausreichend.  
-   Weitere SOSP Papier, lesen Sie weitere Informationen über das Partitionierungsschema in Azure-Speicher verwendet werden [können](http://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

###<a name="networking"></a>Netzwerke
Während die API Angelegenheit anruft, haben oft die physische Netzwerk Einschränkungen der Anwendung wesentlichen Einfluss auf die Leistung. Im folgenden werden einige Einschränkungen, die Benutzer auftreten können beschrieben.  

####<a name="client-network-capability"></a>Client-Netzwerk-Funktion
#####<a name="a-namesubheading2athroughput"></a><a name="subheading2"></a>Durchsatz
Für die Bandbreite ist das Problem oft die Funktionen von dem Client aus. Während Sie beispielsweise ein einzelnes Speicherkonto können 10 Gbps behandeln oder mehrere der eingehende (siehe [Bandbreite Skalierbarkeit Ziele](#sub1bandwidth)), die Geschwindigkeit im Netzwerk in eine Instanz von "Klein" Azure Worker-Rolle ist nur in der Lage, etwa 100 MB /. Größere Azure Instanzen haben NICs mit größerer Kapazität, damit Sie eine größere Instanz oder weitere virtuellen Computers verwenden berücksichtigen sollten, wenn Sie höhere Netzwerk Grenzwerte aus einem einzigen Computer benötigen. Wenn Sie den Zugriff auf eine Speicherdienst aus einer auf lokale Anwendung, und klicken Sie dann die gleiche Regel gilt: die Netzwerkfunktionen, der das Client-Gerät und die Netzwerkkonnektivität zum Speicherort Azure verstehen und entweder bauen Sie sie als erforderlich oder Entwerfen Ihrer Anwendung, deren Funktionen zusammen zu arbeiten.  

#####<a name="a-namesubheading3alink-quality"></a><a name="subheading3"></a>Link Qualität
Wie bei der Netzwerkverwendung einer beliebigen, beachten Sie, dass Netzwerkprobleme, was zu Fehlern und Pakete verloren gehen effektive Durchsatz verlangsamt werden.  Mithilfe von WireShark oder NetMon möglicherweise Hilfe bei der Diagnose dieses Problems.  

#####<a name="useful-resources"></a>Nützlichen Ressourcen
Weitere Informationen zu virtuellen Computern Größen und zugewiesenen Bandbreite finden Sie unter [Windows virtueller Computer Größen](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux virtueller Computer Schriftgrade](../virtual-machines/virtual-machines-linux-sizes.md).  

####<a name="a-namesubheading4alocation"></a><a name="subheading4"></a>Speicherort
In einer beliebigen verteilten Umgebung übermittelt wird, platzieren den Kunden nahe dem Server in die optimale Leistung. Für den Zugriff auf Azure-Speicher mit den niedrigsten Wartezeit ein, wird der optimale Speicherort für Ihren Kunden innerhalb der gleichen Azure Region ein. Wenn Sie eine Azure-Website verfügen, die Azure-Speicher verwendet, sollten Sie beide Programme beispielsweise in einem einzigen Bereich (beispielsweise uns Westen oder Asien oder) suchen. Dies verringert die Wartezeit und die Kosten – zum Zeitpunkt der schreiben, Bandbreite Verwendung in einem einzigen Bereich ist kostenlos.  

Wenn Ihren Kunden, die Applikationen nicht in Azure (z. B. apps Mobilgerät oder auf lokale Enterprise Services), klicken Sie dann erneut gehostet werden platzieren das Speicherkonto in einem Gebiet nahe Geräte, die darauf zugreifen wird im Allgemeinen Wartezeit reduzieren werden. Kunden werden (z. B., einige in Nordamerika, und einige in Europa) gestreut verteilt, und erwägen Sie mehrere Speicherkonten: eine befindet sich in einer North American Region und eine in eine Europäische Region. Dadurch wird die Wartezeit für Benutzer in beiden Regionen zu verringern. Dieser Ansatz ist in der Regel leichter zu implementieren, wenn die Daten die Anwendung speichert gilt nur für einzelne Benutzer und Replikation von Daten zwischen Speicherkonten sind nicht erforderlich.  Für eine Breite Verteilung von Inhalten empfiehlt sich eine CDN – finden Sie im nächste Abschnitt Weitere Details.  

###<a name="a-namesubheading5acontent-distribution"></a><a name="subheading5"></a>Verteilen von Inhalten
In manchen Fällen muss eine Anwendung zu viele Benutzer (z. B. ein Produkt Demo Video in der Homepage einer Website verwendet werden), befindet sich in derselben oder mehrere Bereiche denselben Inhalt bedienen. In diesem Szenario sollten Sie eine Content Delivery Network (CDN) wie Azure CDN verwenden, und das CDN Azure-Speicher als den Ursprung der Daten verwenden. Im Gegensatz zu einem Konto Azure-Speicher im Bereich mit einem einzelnen vorhanden ist und nicht möglich, die Inhalt mit niedriger Wartezeit in andere Regionen vorführen verwendet Azure CDN Servern in mehreren Data Center auf der ganzen Welt an. Darüber hinaus kann einem CDN unterstützen in der Regel sehr viel höher Ausgang Grenzwerte als ein einzelnes Speicherkonto.  

Weitere Informationen zur Azure CDN finden Sie unter [Azure CDN](https://azure.microsoft.com/services/cdn/).  

###<a name="a-namesubheading6ausing-sas-and-cors"></a><a name="subheading6"></a>Verwenden von SAS- und CORS
Wenn Sie, wie z. B. JavaScript-Code in Webbrowser des Benutzers oder eine app auf dem Mobiltelefon zu Access-Daten in Azure-Speicher zu autorisieren müssen, eine Möglichkeit besteht darin, eine Anwendung im Webrolle als Proxy verwenden: das Gerät des Benutzers mit der Webrolle, die wiederum mit der Speicherdienst authentifiziert authentifiziert. Auf diese Weise können Sie vermeiden, Ihre Speicher Konto Tastenreihe unsicher Geräte verfügbar zu machen. Jedoch platziert dies eine große Verwaltungsaufwand die Web-Rolle, da alle Daten zwischen dem Gerät des Benutzers und der Speicherdienst übertragen, bis die Web-Rolle verstreichen müssen. Sie können vermeiden, mithilfe einer Webrolle als Proxy für den Speicherdienst freigegeben Access Signaturen (SAS), manchmal in Verbindung mit Cross-Origin gemeinsames Nutzen von Ressourcen Kopfzeilen (CORS). Sie können SAS des Benutzers Gerät Anfragen direkt an einen Speicherdienst über ein Token beschränkter Zugriff vornehmen erlauben. Wenn ein Benutzer ein Foto an Ihrer Anwendung hochladen möchte, kann Ihre Webrolle beispielsweise generieren und Senden an das Gerät des Benutzers ein SAS Token, die Schreibberechtigung für eine bestimmte Blob oder Container für den nächsten 30 Minuten (das SAS Token nach dem abläuft) gewährt.

Normalerweise lässt ein Browser JavaScript nicht in einer Seite von einer Website in eine Domäne für bestimmte Operationen wie "Setzen eine" in eine andere Domäne gehostet wird. Beispielsweise, wenn Sie eine Webrolle bei "contosomarketing.cloudapp.net" gehostet und Client Seite JavaScript verwenden einen Blob bei Ihrem Speicherkonto bei "contosoproducts.blob.core.windows.net," im Browser des hochladen möchten "same Origin Policy" wird dieser Vorgang verbieten. CORS ist ein Browser-Feature, das die Zieldomäne (in diesem Fall das Speicherkonto) ermöglicht an den Browser kommunizieren, dass es in der Quelldomäne (in diesem Fall die Web-Rolle) Anfragen vertraut.  

Beide der folgenden Technologien helfen Ihnen unnötige Belastung (und Engpässe) auf Ihrer Webanwendung zu vermeiden.  

####<a name="useful-resources"></a>Nützlichen Ressourcen
Weitere Informationen zu SAS, finden Sie unter [Signaturen für freigegebene Access, Teil 1: Grundlegendes zu SAS-Modell](storage-dotnet-shared-access-signature-part-1.md).  

Weitere Informationen zu CORS finden Sie unter [Cross-Origin Ressource freigeben (CORS) Unterstützung für die Azure-Speicher-Dienste](http://msdn.microsoft.com/library/azure/dn535601.aspx).  

###<a name="caching"></a>Zwischenspeichern
####<a name="a-namesubheading7agetting-data"></a><a name="subheading7"></a>Abrufen von Daten
Abrufen von Daten aus einem Dienst einmal ist im Allgemeinen besser als erste sie zweimal. Erwägen Sie das Beispiel einer MVC Web-Anwendung, die in einer Webrolle, die bereits einen Blob 50 MB aus der Speicherdienst zu dienen als Inhalte, die ein Benutzer abgerufen wurde ausgeführt. Die Anwendung konnte dann die gleiche Blob abgerufen werden jedes Mal, wenn ein Benutzer fordert sie, oder es konnte zwischengespeichert werden lokal an, um den Datenträger und die zwischengespeicherte Version für nachfolgenden benutzeranforderungen wiederverwenden. Darüber hinaus fordert jederzeit ein Benutzer die Daten, die Anwendung Problem mit einem bedingten Header für den Zeitpunkt der Änderung abzurufen, könnte die zu vermeiden, sollte des gesamten BLOBs, wenn es nicht geändert wurde. Sie können in demselben Muster für die Arbeit mit der Tabelle Elemente anwenden.  

In einigen Fällen können Sie festlegen, dass die Anwendung davon ausgehen kann, dass das Blob für kurze Zeit nach dem Abrufen gültig bleibt und während dieses Zeitraums der Anwendung nicht erforderlich ist, prüfen, ob das Blob geändert wurde.

Konfiguration, Nachschlagen und andere Daten, die von der Anwendung immer verwendet werden, sind gute Kandidaten zum Zwischenspeichern.  

Ein Beispiel für das Abrufen eines Blob-Eigenschaften zum Ermitteln von .NET mit Datums des letzten Änderung finden Sie unter [festlegen und Abrufen von Eigenschaften und Metadaten](storage-properties-metadata.md). Weitere Informationen zur bedingten Downloads finden Sie unter [Aktualisieren bedingt eine lokale Kopie eines Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx).  

####<a name="a-namesubheading8auploading-data-in-batches"></a><a name="subheading8"></a>Hochladen von Daten in Stapeln
Sie können in einigen Anwendungsszenarien Aggregieren von Daten lokal und regelmäßig Laden Sie es in einem Stapel statt jedes Datenelement sofort hochzuladen. Eine Webanwendung möglicherweise beispielsweise eine Protokolldatei Aktivitäten beibehalten: die Anwendung konnte entweder Details jeder Aktivität hochladen, wie dies als Tabellenentität geschieht (das viele Speichervorgänge erfordert), oder es konnte Aktivitätsdetails zu einer lokalen Protokolldatei speichern und dann regelmäßig hochladen aller Details über die Aktivitäten als durch Trennzeichen getrennten Datei in ein Blob. Wenn Protokolleinträge 1 KB groß ist, können Sie in einer einzigen "Blob setzen" Transaktion Tausendertrennzeichen Hochladen (Sie können einen Blob von bis zu 64 MB Größe in einer einzigen Transaktion hochladen). Natürlich, wenn auf der lokale Computer vor der Upload stürzt ab, potenziell einige Log Daten verloren: Entwickler muss Entwerfen für die Möglichkeit, Client-Gerät oder Fehlern hochladen.  Wenn die Aktivitätsdaten für Zeitspannen (nicht nur einzelne Aktivität) heruntergeladen werden müssen, werden Blobs über Tabellen empfohlen.

###<a name="net-configuration"></a>.NET Konfiguration
Bei Verwendung von .NET Framework, listet dieser Abschnitt mehrere schnelle Konfiguration Einstellungen, die Sie verwenden können, um Performance signifikant zu gestalten.  Wenn andere Sprachen verwenden, überprüfen Sie um ähnliche Konzepte in der ausgewählten Sprache zu installieren.  

####<a name="a-namesubheading9aincrease-default-connection-limit"></a><a name="subheading9"></a>Vergrößern Sie die standardmäßige Verbindung Grenzwert
In .NET wird mit dem folgende Code der standardmäßige Verbindung Grenzwert (also normalerweise 2 im Client-Umgebung oder in einer Server-Umgebung 10) und 100 erhöht. In der Regel, sollten Sie den Wert auf ungefähr der Anzahl der von der Anwendung verwendeten Threads festlegen.  

    ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  

Sie müssen die Verbindung Beschränkung vor dem Öffnen einer beliebigen Verbindungen festlegen.  

Anderen programming Sprachen finden Sie in der Sprache Dokumentation zu bestimmen, wie den Verbindung Grenzwert festgelegt werden.  

Weitere Informationen finden Sie im Blogbeitrag [Webdienste: aktiven Verbindungen](http://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

####<a name="a-namesubheading10aincrease-threadpool-min-threads-if-using-synchronous-code-with-async-tasks"></a><a name="subheading10"></a>Erhöhen Sie ThreadPool Min Threads, wenn asynchrone Aufgaben synchroner Code mit
In diesem Code wird die min Threadpoolthreads vergrößern:  

    ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  

Weitere Informationen finden Sie unter [ThreadPool.SetMinThreads-Methode](http://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

####<a name="a-namesubheading11atake-advantage-of-net-45-garbage-collection"></a><a name="subheading11"></a>Nutzen Sie die Vorteile von .NET 4.5 Garbagecollection
Verwenden Sie .NET 4.5 oder höher für die Clientanwendung leistungsverbesserungen in Garbagecollection Server nutzen.

Weitere Informationen finden Sie im Artikel [Ein Übersicht der Leistungsverbesserungen in .NET 4.5](http://msdn.microsoft.com/magazine/hh882452.aspx).  

###<a name="a-namesubheading12aunbounded-parallelism"></a><a name="subheading12"></a>Ungebundener Parallelität
Während der Parallelität hervorragend zum Leistung werden kann, achten Sie darauf, dass Sie zur Verwendung von ungebundener Parallelism (keine Beschränkung für die Anzahl der Threads und/oder parallele Anfragen) hochladen oder Herunterladen von Daten mithilfe von mehreren Kollegen, mehrere (Container, Warteschlangen oder Tabellenpartitionen) in demselben Speicherkonto Zugriff oder auf mehrere Elemente in der gleichen Partition zuzugreifen. Ist die Parallelität ungebundener, Ihrer Anwendung kann das Client-Gerät Funktionen überschreiten oder Speicher-Konto Skalierbarkeit richtet sich in längeren Wartezeiten resultierender und begrenzungsebene.  

###<a name="a-namesubheading13astorage-client-libraries-and-tools"></a><a name="subheading13"></a>Speicher-Client-Bibliotheken und Tools
Verwenden Sie immer den neuesten von Microsoft Client-Bibliotheken und Tools zur Verfügung gestellt. Zum Zeitpunkt der schreiben gibt es Client-Bibliotheken für .NET, Windows Phone, Windows-Runtime, Java und C++ sowie Vorschau Bibliotheken für andere Sprachen. Darüber hinaus hat Microsoft PowerShell-Cmdlets und Azure CLI-Befehle für die Arbeit mit Azure-Speicher veröffentlicht. Microsoft aktiv entwickelt diese Tools mit Leistung Denken Sie daran, behält diese mit den neuesten Dienstversionen auf dem neuesten Stand und damit ist sichergestellt, dass sie viele die Leistung bewährten Methoden intern behandeln.  

###<a name="retries"></a>Wiederholungsversuche
####<a name="a-namesubheading14athrottlingserverbusy"></a><a name="subheading14"></a>Begrenzungsebene/ServerBusy
In einigen Fällen wird der Speicherdienst schränken Sie möglicherweise eine Anwendung oder kann einfach möglicherweise nicht die Anforderung aufgrund einer vorübergehenden Bedingung dienen und einer Nachricht "503 Server ist ausgelastet" oder "500 Timeout" zurück.  Dies kann geschehen, wenn eine Anwendung beliebige Ziele Skalierbarkeit Annäherung an oder das System die partitionierten Daten für höhere Durchsatzraten dürfen Qualifikationsprofilen ist.  Die Clientanwendung sollte normalerweise wiederholen Sie den Vorgang, der einen derartigen Fehler verursacht: später versuchen, die gleiche Anforderung kann erfolgreich durchgeführt werden. Stellen Sie aber wenn der Speicherdienst Ihrer Anwendung eingeschränkt ist, da es Skalierbarkeit Ziele überschreitet oder auch, wenn der Dienst zur Bereitstellung der Anfrage für andere aus irgendeinem Grund nicht konnte, anspruchsvollen Wiederholungsversuche normalerweise das Problem schlechter. Daher sollten Sie eine exponentielle wieder deaktivieren (der Client Bibliotheken Standardeinstellung dieses Verhalten) verwenden. Ihrer Anwendung möglicherweise beispielsweise, versuchen Sie es erneut 2 Sekunden, und klicken Sie dann 4 Sekunden, 10 Sekunden und dann 30 Sekunden und geben dann vollständig nach oben. Dieses Verhalten führt eine Anwendung erheblich seine Last-Dienst verringert statt exacerbating Probleme.  

Beachten Sie, dass Connectivity Fehler sofort, wiederholt werden können, da sie nicht das Ergebnis des begrenzungsebene und vorübergehend sein sollen.  

####<a name="a-namesubheading15anon-retryable-errors"></a><a name="subheading15"></a>Fehler nicht Aufforderung zum Wiederholen
Die Clientbibliotheken kennen der Fehler "Wiederholen" können und welche nicht sind. Wenn Sie Ihren eigenen Code gegen die Speicherung REST-API schreiben, beachten Sie jedoch einige Fehler auftreten, die Sie nicht wiederholen sollte: z. B. 400 (Ungültige Anforderung) Antwort weist darauf hin, dass die Clientanwendung eine Anforderung, die nicht bearbeitet werden können gesendet, da es nicht in einem Formular erwarteten war. Erneutes Senden diese Anforderung führt zu Fehlern dieselbe Antwort jedes Mal, es ist jedoch keine wiederholen sie. Wenn Sie Ihren eigenen Code gegen die Speicherung REST-API schreiben, achten Sie auf der Bedeutung der Fehlercodes und die richtige Methode zum Wiederholen (oder nicht) für jede von ihnen.  

####<a name="useful-resources"></a>Nützlichen Ressourcen
Weitere Informationen zu Fehlercodes Speicher finden Sie unter [Status und Fehlercodes](http://msdn.microsoft.com/library/azure/dd179382.aspx) auf der Website Microsoft Azure.  

##<a name="blobs"></a>BLOBs
Wenden Sie zusätzlich zu den bewährten Methoden für [Alle Dienste](#allservices) beschriebenen bewährten Methoden für die folgenden speziell für den Blob-Dienst.  

###<a name="blob-specific-scalability-targets"></a>BLOB-spezifische Skalierbarkeit Ziele

####<a name="a-namesubheading46amultiple-clients-accessing-a-single-object-concurrently"></a><a name="subheading46"></a>Mehrere Clients gleichzeitig Zugriff auf ein einzelnes Objekt
Wenn Sie eine große Anzahl von Clients, die Zugriff auf ein einzelnes Objekt gleichzeitig haben, müssen Sie pro Objekt und Speicher Konto Skalierbarkeit Ziele berücksichtigen. Die genaue Anzahl von Clients, die ein einzelnes Objekt zugreifen können, die eine variieren je nach Faktoren wie die Anzahl der Clients, die das Objekt zu gleichzeitig Anfordern der Größe des Objekts, Netzwerk Bedingungen usw..

Wenn das Objekt über verteilt werden, kann einem CDN wie Bilder oder Videos von einer Website bereitgestellt und dann sollten Sie eine CDN verwenden. Finden Sie [hier](#subheading5).

In anderen Szenarien wie wissenschaftliche Simulationen, wo die Daten vertraulich sind, stehen Ihnen zwei Optionen. Die erste besteht darin, Ihre Arbeitsbelastung des Access staffeln so, dass Sie Zugriff auf das Objekt über einen Zeitraum von Zeit im Vergleich mit einer gleichzeitig zugegriffen wird. Alternativ können Sie mehrere Speicherkonten und die gesamte IOPS pro Objekt und über Speicherkonten hinweg gesteigert vorübergehend das Objekt kopieren. Eingeschränkte testen gefunden wir, dass ungefähr 25 virtuellen Computern gleichzeitig einen 100 GB Blob parallel herunterladen konnte (jedes virtuellen Computer wurde den Verwendung der 32 Download parallelisieren). Wenn Sie mit 100 Kunden benötigen Zugriff auf das Objekt, kopieren Sie sie zuerst auf ein zweites Speicherkonto und lassen Sie anschließend die ersten 50 virtuellen Computern das erste Blob und den zweiten 50 virtuelle Computer Zugriff auf das zweite Blob geführt. Ergebnisse variieren je nach Ihrer Anwendung Verhalten, damit diese während des Entwurfs getestet werden soll. 


####<a name="a-namesubheading16abandwidth-and-operations-per-blob"></a><a name="subheading16"></a>Bandbreite und Vorgänge pro Blob
Sie können lesen oder Schreiben in ein einzelnes Blob bei bis zu einem Maximum von 60 MB/Sekunde (Dies ist ungefähr 480 MB/s die die Funktionen von vielen Client Seite Netzwerken (einschließlich der physischen NIC auf dem Clientgerät) überschreiten. Darüber hinaus unterstützt ein einzelnes Blob bis zu 500 Abfragen pro Sekunde. Wenn Sie mehrere Clients, die das gleiche Blob lesen müssen und diese Grenzwerte überschritten werden kann, sollten Sie mit einem CDN für das Blob verteilen.  

Weitere Informationen zu Ziel Durchsatz für Blobs finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md).  

###<a name="copying-and-moving-blobs"></a>Kopieren und Verschieben von Blobs
####<a name="a-namesubheading17acopy-blob"></a><a name="subheading17"></a>Kopieren von Blob
Der REST-API Version 2012-02-12-Speicher eingeführt werden, die hilfreiche Möglichkeit, Blobs über Konten hinweg kopieren: eine Clientanwendung kann anweisen, der Speicherdienst zum Kopieren eines BLOBs aus einer anderen Quelle (möglicherweise in einer anderen Speicher-Konto), und lassen Sie den Dienst, der die Kopie asynchrone ausführen. Dies kann die Bandbreite für die Anwendung erforderlich, wenn Sie Daten aus anderen Speicherkonten migrieren, da Sie nicht benötigen, herunterladen und Hochladen der Daten reduziert.  

Ein Aspekt ist jedoch, dass beim Kopieren von Speicherkonten vorhanden ist keine Garantie Zeit auf, wenn der Kopiervorgang abgeschlossen wird. Wenn eine Anwendung Blob Kopie schnell Ihnen gesteuert erledigen, kann es besser, das Blob kopieren, indem sie auf einen virtuellen Computer herunterladen und Hochladen der Datei klicken Sie dann auf das Ziel sein.  Vollständige Vorhersagbarkeit in diesem Fall sicherstellen Sie, dass die Kopie erfolgt über einen virtuellen Computer ausgeführt wird, in der gleichen Azure Region, da sonst Netzwerkprobleme möglicherweise (und wird) Ihre Kopie Leistung beeinträchtigen.  Darüber hinaus können Sie den Fortschritt eine asynchrone Kopie programmgesteuert überwachen.  

Beachten Sie, dass Kopien innerhalb der gleichen Speicherkonto selbst schnell in der Regel durchgeführt werden.  

Weitere Informationen finden Sie unter [Kopieren Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).  

####<a name="a-namesubheading18ause-azcopy"></a><a name="subheading18"></a>Verwenden von AzCopy
Das Team Azure-Speicher hat eine Befehlszeile Tool "AzCopy" veröffentlicht, ist mit Massen übertragen viele Blobs an, aus und über Speicherkonten hinweg unterstützen sollen.  Dieses Tool ist für dieses Szenario optimiert und kann hohe durchstellen Sätzen erzielen.  Seine Verwendung wird für Massen hochladen, herunterladen und kopieren Szenarien empfohlen. Wenn Sie weitere Informationen zu erhalten und herunterladen, finden Sie unter [Übertragen von Daten mit den AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).  

####<a name="a-namesubheading19aazure-importexport-service"></a><a name="subheading19"></a>Azure Import/Export-Dienst
Für sehr große Datenmengen (mehr als 1TB) bietet der Azure-Speicher den Import/Export-Dienst, der für das Hochladen und Herunterladen von Blob-Speicher von Versand Festplatten kann.  Sie können Anordnen von Daten auf einem Festplattenlaufwerk und zur Upload an Microsoft senden, oder senden eine leere Festplatte an Microsoft herunterladen von Daten.  Weitere Informationen finden Sie unter [Verwenden der Microsoft Azure Import/Export-Dienst Daten in Blob-Speicher übertragen werden](storage-import-export-service.md).  Dies kann wesentlich effizienter als Hochladen/Herunterladen von diesem Handbuch von Daten über das Netzwerk sein.  

###<a name="a-namesubheading20ause-metadata"></a><a name="subheading20"></a>Verwenden von Metadaten
Der Blob-Dienst unterstützt am Besprechungsanfragen, die Metadaten für die Blob enthalten sein können. Bei Bedarf die EXIF-Daten aus einem Foto Ihrer Anwendung könnten sie beispielsweise das Foto abrufen und extrahieren Sie die Datei. Zum Speichern von Bandbreite und Verbessern der Leistung, Ihrer Anwendung konnte speichern EXIF-Daten in der-BLOB-Metadaten, wenn die Anwendung das Foto hochgeladen: Sie können dann abrufen, die die EXIF-Daten in den Metadaten mit nur einem Kopf anfordern, speichern signifikante Bandbreite und die Verarbeitungszeit erforderlich zum Extrahieren der EXIF Daten jedes Mal das Blob gelesen wird. Dies würde in Szenarien hilfreich sein, in dem Sie nur die Metadaten und nicht des gesamten Inhalts eines Blob benötigen.  Beachten Sie, dass nur 8 KB Metadaten pro Blob (der Dienst wird eine Anforderung zum Speichern von mehreren, die nicht annehmen), gespeichert werden können, damit die Daten nicht in der jeweiligen Größe passt, Sie nicht zu diesem Ansatz möglicherweise mehr.  

Ein Beispiel für so eine Blob-Metadaten, die mit .NET erhalten finden Sie unter [festlegen und Abrufen von Eigenschaften und Metadaten](storage-properties-metadata.md).  

###<a name="uploading-fast"></a>Schnelles hochladen
Um schnell Blobs hochladen, wird die erste Frage beantworten: möchten Sie eine Blob oder viele hochladen?  Verwenden Sie die unter Leitfäden, um die richtige Methode abhängig von Ihrem Szenario verwenden zu bestimmen.  

####<a name="a-namesubheading21auploading-one-large-blob-quickly"></a><a name="subheading21"></a>Eine große Blob hochladen schnell
Wenn Sie schnell ein einzelnes großes Blob hochladen, sollte die Clientanwendung seine Blöcke oder Seiten parallel (Beachten Sie die Skalierbarkeit Ziele für einzelne Blobs und das Speicherkonto als Ganzes wird) hochladen.  Beachten Sie, dass die offiziellen Microsoft bereitgestellten RTM Speicher-Client-Bibliotheken (.NET, Java) die Möglichkeit, dies zu tun haben.  Verwenden Sie für jede der Bibliotheken, die unten angegebene Objekt/Eigenschaft, um die Ebene gleichzeitige Verarbeitung festzulegen:  

-   .NET: Klicken Sie auf ein Objekt BlobRequestOptions festlegen ParallelOperationThreadCount verwendet werden.
-   Java/Android: Verwenden von BlobRequestOptions.setConcurrentRequestCount()
-   Node.js: Verwenden Sie ParallelOperationThreadCount entweder die Anfrage Optionen oder Blob-Dienst.
-   C++: Verwenden Sie die Methode blob_request_options::set_parallelism_factor.

####<a name="a-namesubheading22auploading-many-blobs-quickly"></a><a name="subheading22"></a>Viele Blobs hochladen schnell
Wenn Sie viele Blobs schnell hochladen, laden Sie Blobs parallel hoch. Dies ist schneller als einzelne Blobs nacheinander mit paralleler Block Uploads hochladen, da es mehrere partitionsübergreifend der Speicherdienst den Upload verbreitet. Ein einzelnes Blob unterstützt nur einen Durchsatz von 60 MB/Sekunde (etwa 480 MB/s). Zum Zeitpunkt der schreiben unterstützt ein LRS US-basierten Konto bis zu 20 Gbps eingehende also viel mehr als den Durchsatz durch eine einzelne Blob unterstützt.  [AzCopy](#subheading18) führt Uploads parallel standardmäßig, und es wird empfohlen, in diesem Szenario.  

###<a name="a-namesubheading23achoosing-the-correct-type-of-blob"></a><a name="subheading23"></a>Die richtige Art von Blob auswählen
Azure-Speicher unterstützt zwei Arten von Blob: *Seitenblobs* und *Blockieren* Blobs. Für ein angegebenes Verwendungsszenario wirkt sich die Wahl der BLOB-Typ die Leistung und Skalierbarkeit Ihrer Lösung. Blockieren Blobs eignen sich, wenn Sie große Datenmengen effizient hochladen möchten: z. B. möglicherweise eine Clientanwendung zum Hochladen von Fotos oder Videos zu Blob-Speicher müssen. Seitenblobs sind geeignet, wenn die Anwendung zufällige schreibt auf der Registerkarte Daten durchführen muss: z. B. Azure-virtuellen Festplatten als Seitenblobs gespeichert werden.  

Weitere Informationen finden Sie unter [Grundlegendes zu blockieren Blobs, Anfügen Blobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).  

##<a name="tables"></a>Tabellen
Wenden Sie zusätzlich zu den bewährten Methoden für [Alle Dienste](#allservices) beschriebenen bewährten Methoden für die folgenden speziell zum Dienst Tabelle ein.  

###<a name="a-namesubheading24atable-specific-scalability-targets"></a><a name="subheading24"></a>Tabelle-spezifische Skalierbarkeit Ziele
Neben die Bandbreite Einschränkungen eines gesamten Speicherkontos über Tabellen die folgenden bestimmte Skalierbarkeit Beschränkung.  Beachten Sie, dass das System Saldo Ihrer zunehmender Datenverkehr lädt, aber wenn Ihre Datenverkehr plötzlich Bursts aufweist, Sie möglicherweise nicht in diesem Handbuch Durchsatz sofort zu erhalten.  Wenn Ihr Muster Bursts aufweist, erwarten begrenzungsebene und/oder Zeitlimit bei der Burst wie der Speicher automatisch laden Salden finden Sie in der Tabelle service finden Sie unter.  Langsam im Allgemeinen verbessern verfügt über eine bessere Ergebnisse, wie Sie es für den Lastenausgleich entsprechend die Systemzeit Fortsetzung des Vorgangs aus.  

####<a name="entities-per-second-account"></a>Personen pro Sekunde (Konto)
Skalierbarkeit Beschränkung der für den Zugriff auf Tabellen beträgt bis zu 20.000 Elemente (1KB jedes) pro Sekunde für ein Konto.  Jede Person, die eingefügt wird, in der Regel aktualisiert, gelöscht oder gescanntes zählt gegen dieses Ziel.  Daher würde ein Blatt einfügen, die 100 Unternehmen enthält als 100 Einheiten zählen.  Eine Abfrage, die scannt 1000 Elemente und gibt 5, würde als 1000 Elemente zählen.  

####<a name="entities-per-second-partition"></a>Personen pro Sekunde (Partition)
Innerhalb einer Einzelpartition ist das Ziel Skalierbarkeit für den Zugriff auf Tabellen 2.000 Einheiten (1KB jedes) pro Sekunde, mit der gleichen Inventur, wie im vorherigen Abschnitt beschrieben.  

###<a name="configuration"></a>Konfiguration
In diesem Abschnitt werden mehrere schnelle Konfiguration Einstellungen, die Sie stellen Performance signifikant in der Tabelle-Dienst verwenden können:  

####<a name="a-namesubheading25ause-json"></a><a name="subheading25"></a>Verwenden von JSON
Beginnend mit Speicher-Service-Version 2013-08-15, unterstützt der Tabelle Dienst JSON anstelle des XML-basierten AtomPub-Formats für die Übertragung von Tabellendaten mit. Dies kann Nutzlast Größen verringern, indem Sie mehr als 75 % und kann die Leistung der Anwendung erheblich verbessern.

Weitere Informationen finden Sie unter dem Beitrag [Microsoft Azure Tabellen: Einführung in JSON](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) und [Nutzlast Format für die Tabelle Dienstvorgänge](http://msdn.microsoft.com/library/azure/dn535600.aspx).

####<a name="a-namesubheading26anagle-off"></a><a name="subheading26"></a>Nagle deaktivieren
Der Nagle-Algorithmus wird als eine Möglichkeit zum Verbessern der Leistung des Netzwerks stark über TCP/IP Netzwerken implementiert. Es ist jedoch nicht optimale in allen Fällen (beispielsweise hochgradig interaktive Umgebungen). Zum Azure-Speicher Nagles-Algorithmus hat einen negativen Einfluss auf die Leistung von Besprechungsanfragen in der Tabelle und Warteschlange Services und sollten nach Möglichkeit deaktivieren.  

Weitere Informationen finden Sie unter unseren Blogbeitrag [Nagles-Algorithmus in Richtung kleine Anfragen nicht geeignet ist](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx), die erläutert, warum Nagles-Algorithmus interagiert schlecht mit Tabellen und Warteschlange Anforderungen, und zeigt, wie sie in der Clientanwendung deaktivieren.  

###<a name="schema"></a>Schema
Wie Sie darstellen und Abfragen von Daten ist der größte Einfaktorielle Varianzanalyse, die die Leistung des Diensts Tabelle wirkt sich auf. Während jeder Anwendung abweicht, werden in diesem Abschnitt einige allgemeinen bewährten Methoden, die mit zusammenhängen:  

-   Tabellenentwurf
-   Effiziente Abfragen
-   Effizient Datenupdates  

####<a name="a-namesubheading27atables-and-partitions"></a><a name="subheading27"></a>Tabellen und Partitionen
Tabellen werden in Partitionen unterteilt. Jede Person in einer Partition gespeichert teilt den gleichen Partitionsschlüssel und verfügt über eine eindeutige Zeilen-Taste, um ihn innerhalb dieser Partition zu identifizieren. Partitionen bieten Vorteile aber auch Skalierbarkeit Grenzwerte vorstellen.  

-   Vorteile: Sie können Elemente in der gleichen Partition in einer einzigen, atomaren, Stapel Transaktion aktualisieren, die bis zu 100 separaten Speichervorgänge (maximal 4 MB Gesamtgröße) enthält. Wenn die gleiche Anzahl von Elementen, die abgerufen werden, können Sie auch Daten innerhalb einer Einzelpartition effizienter als Daten Abfragen, die Partitionen (für weitere Empfehlungen zu Abfragen Tabellendaten durch weiterlesen) umfasst.
-   Skalierbarkeit Grenzwert: Zugriff auf Elemente in einer einzelnen Partition gespeichert werden Lastenausgleich nicht da Partitionen atomaren an Transaktionen unterstützt. Aus diesem Grund ist das Ziel Skalierbarkeit für eine einzelne Tabellenpartition als Ganzes niedriger als für den Dienst Tabelle.  

Da diese Merkmale von Tabellen und Partitionen sollten Sie die folgenden Entwurfsprinzipien übernehmen:  

-   Daten, die Ihre Clientanwendung häufig aktualisiert oder in der gleichen logische Arbeitseinheit abgefragt sollte in der gleichen Partition befinden.  Dies ist möglicherweise weil eine Anwendung schreibt aggregieren ist oder atomare Stapel Vorgänge nutzen möchten.  Darüber hinaus können Daten in einer Einzelpartition effizienter in einer Abfrage als Daten partitionsübergreifend abgefragt werden.
-   Daten, die Ihre Clientanwendung nicht einfügen/aktualisieren oder einer Abfrage in der gleichen logische Arbeitseinheit (einzelne Abfrage oder Stapel Update) sollte in separate Partitionen befinden.  Ein wichtiger Hinweis darin, dass es keine Beschränkung der Anzahl von Partition Tasten in einer Tabelle, ist Probleme Millionen von Partition Schlüssel ist kein Problem, und hat keine Auswirkung auf die Leistung.  Beispielsweise wenn die Anwendung eine beliebte Website Anmeldung des Benutzers ist, könnte verwenden die Benutzer-Id als der Partitionsschlüssel eine gute Wahl.  

####<a name="hot-partitions"></a>Wichtiges Partitionen
Eine wichtiges Partition ist eine, die einem Prozentwert des Datenverkehrs mit einer Firma empfängt, und kann nicht werden Lastenausgleich, da es sich um eine einzelne Partition ist.  Wichtiges Partitionen werden in der Regel eine von zwei Arten erstellt:  

#####<a name="a-namesubheading28aappend-only-and-prepend-only-patterns"></a><a name="subheading28"></a>Nur angefügt werden soll, und stellen Sie nur Muster voran
Das Muster "Nur anfügen" ist eine Stelle, an der alle (oder nahezu alle) des Datenverkehrs zu einem angegebenen PK erhöht und entsprechend der aktuellen Uhrzeit wird verringert.  Ein Beispiel ist, wenn die Anwendung das aktuelle Datum als Partitionsschlüssel für Log-Daten verwendet.  Dadurch werden alle vom abnehmenden eingefügte zur letzten Partition in der Tabelle, und das System kann nicht den Lastenausgleich, da alle die schreibt an das Ende der Tabelle abgelegt werden.  Wenn der Umfang des Datenverkehrs auf dieser Partition das Ziel Partitionsebene Skalierbarkeit überschreitet, wird dann in begrenzungsebene führen.  Es empfiehlt sich, um sicherzustellen, dass Datenverkehr auf mehrere, Lastenausgleich aktivieren die Anfragen über der Tabelle gesendet wird.  

#####<a name="a-namesubheading29ahigh-traffic-data"></a><a name="subheading29"></a>Daten mit hohem Datenverkehr
Ihr Partitionierungsschema einer Einzelpartition ergibt, das nur die Daten enthält, die weit mehr als die anderen Partitionen verwendet wird, können Sie auch anzeigen, begrenzungsebene als die Partition das Ziel Skalierbarkeit für eine einzelne Partition Ansätze.  Es empfiehlt sich, um sicherzustellen, dass Ihr Partitionsschema keine Einzelpartition Annäherung an die Ziele Skalierbarkeit ergibt.  

####<a name="querying"></a>Abfragen
In diesem Abschnitt werden bewährte Methoden für die Tabelle Dienst Abfragen.  

#####<a name="a-namesubheading30aquery-scope"></a><a name="subheading30"></a>Abfragebereich
Es gibt mehrere Methoden, um den Bereich der Elemente aus, um die Abfrage angeben.  Im folgenden finden eine Informationen zur Verwendung der einzelnen.  

Im Allgemeinen vermeiden Sie scannt (größer als einzelne Einheit Abfragen), aber wenn Sie scannen müssen, versuchen Sie, Ihre Daten organisieren, damit Ihre scannt die Daten, die Sie benötigen abzurufen, ohne Scannen oder zurückgeben signifikante Mengen von Personen, die Sie nicht benötigen.  

######<a name="point-queries"></a>Punkt-Abfragen
Eine Abfrage Punkt ruft genau ein Entität ab. Dies geschieht sowohl die Zeilenschlüssel der Entität zum Abrufen der Partitionsschlüssel angibt. Diese Abfragen sind sehr effizient, und Sie sollten sie nach Möglichkeit verwenden.  

######<a name="partition-queries"></a>Partition Abfragen
Eine Abfrage Partition ist eine Abfrage, die eine Reihe von Daten abgerufen werden, das eine allgemeine Partitionsschlüssel teilt. In der Regel, gibt die Abfrage an einen Zellbereich der Schlüsselwerte für die Zeile oder einen Bereich von Werten für einige Entitätseigenschaft zusätzlich Partitionsschlüssel. Dies sind weniger effizient als Punkt Abfragen, und nur in Ausnahmefällen verwendet werden soll.  

######<a name="table-queries"></a>Tabellenabfragen
Eine Tabellenabfrage ist eine Abfrage, die eine Reihe von Personen zu holen, die allgemeine Partitionsschlüssel nicht gemeinsam an. Diese Abfragen sind nicht effizient, und vermeiden Sie, falls möglich.  

#####<a name="a-namesubheading31aquery-density"></a><a name="subheading31"></a>Einer Abfrage
Eine andere ist Abfrage Effizienz die Anzahl der Einheiten, die im Vergleich zu die Anzahl der Einheiten, die zum Ermitteln der zurückgegebene zurückgegeben. Wenn Ihre Anwendung eine Tabellenabfrage mit einem Filter für einen Eigenschaftswert durchführt, dass nur 1 % der Datenfreigaben, die Abfrage 100 Einheiten für jede eine Person prüft, es gibt. Die Tabelle Skalierbarkeit Ziele erläuterten zuvor alle beziehen sich auf die Anzahl der Einheiten gescannt, und nicht die Anzahl der Einheiten, die zurückgegeben: eine niedrige Abfrage Dichtefunktion kann einfach bewirken, dass den Tabelle-Dienst Ihrer Anwendung einschränken, da es so viele Personen, um die Entität Abrufen von Ihnen gesuchte durchsuchen muss.  Finden Sie im Abschnitt unter [Denormaliserung](#subheading34) Weitere Informationen, wie Sie dieses Problem zu vermeiden.  

#####<a name="limiting-the-amount-of-data-returned"></a>Begrenzung der zurückgegebenen Daten
######<a name="a-namesubheading32afiltering"></a><a name="subheading32"></a>Filtern
Wenn Sie wissen, dass eine Abfrage in der Clientanwendung Einheiten, die Sie nicht benötigen zurückgegeben wird, erwägen Sie mithilfe eines Filters zum Verringern der Größe der zurückgegebene festlegen. Während die Personen, die nicht an den Client zurückgegeben weiterhin in Richtung der Skalierbarkeit Grenzwerte zählen möchten, wird die Leistung Ihrer Anwendung verbessern, aufgrund der Nutzlastgröße reduzierte Netzwerk und geringere Anzahl von Personen, die Ihre Clientanwendung verarbeitet werden muss.  Finden Sie unter über Notiz auf [Einer Abfrage](#subheading31), jedoch – Skalierbarkeit Ziele beziehen sich auf die die Anzahl der Einheiten, die überprüft, damit eine Abfrage, die viele Elemente herausgefiltert weiterhin in Einschränkung, ergeben, auch wenn einige Elemente zurückgegeben werden.  

######<a name="a-namesubheading33aprojection"></a><a name="subheading33"></a>Projektion
Wenn nur eine begrenzte Anzahl der Eigenschaften für die Elemente in der Tabelle die Clientanwendung benötigt werden, können Sie die Projektion verwenden, um die Größe der zurückgegebene Datenmenge beschränken. Wie bei filtern, kann mithilfe dieser Informationen um Netzwerk laden und Client Verarbeitung zu verringern.  

#####<a name="a-namesubheading34adenormalization"></a><a name="subheading34"></a>Denormaliserung
Führen im Gegensatz zu arbeiten mit relationalen Datenbanken, die bewährten Methoden für Abfragen effizient Tabellendaten zu denormalisieren von Daten ein. D. h., duplizieren dieselben Daten in mehreren Personen (eine für jede Taste ein, die Sie verwenden können, um die Daten zu finden) zu minimieren die Anzahl der Einheiten, die eine Abfrage durchsuchen müssen, um den Daten den Client finden müssen, anstatt Scannen großen Anzahl von Personen aus, die die Daten müssen die Anwendung benötigt.  Beispielsweise sollten Sie in einer e-Commerce-Website Ordnung beide nach der Kunden-ID suchen (Gib mir des Kunden Bestellungen) und nach dem Datum (Gib mir Bestellungen an einem Datum).  In Table Storage, es empfiehlt sich, die Entität (oder ein Verweis auf ihn) speichern zweimal – einmal mit Tabellennamen PK und RK zum Suchen nach Kunde zu erleichtern-ID, einmal um zu erleichtern, suchen sie nach dem Datum.  

####<a name="insertupdatedelete"></a>Einfügen, aktualisieren und löschen
In diesem Abschnitt werden bewährte Methoden zum Ändern von Personen, die in der Tabelle Service gespeichert.  

#####<a name="a-namesubheading35abatching"></a><a name="subheading35"></a>Batchverarbeitung
Stapel Transaktionen werden als Entität Gruppe Transaktionen (ETG) in Azure-Speicher bekannt; alle Vorgänge innerhalb einer ETG müssen sich auf einer Einzelpartition in einer Tabelle befinden. Verwenden Sie nach Möglichkeit ETGs einfügen, aktualisieren und löschen stapelweise ausführen. Dies verringert die Anzahl der Schleifen in der Clientanwendung auf dem Server, verringert die Anzahl der berechenbaren Transaktionen (eine ETG zählt als eine einzelne Transaktion für Zwecke Abrechnung und kann bis zu 100 Speicher Operationen enthalten) und atomaren Updates (alle Vorgänge erfolgreich ist oder alle innerhalb einer ETG fehlschlagen) ermöglicht. Umgebungen mit hoher Wartezeiten wie mobile Geräte werden erheblich nutzbringend verwenden ETGs.  

#####<a name="a-namesubheading36aupsert"></a><a name="subheading36"></a>Upsert
Verwenden Sie nach Möglichkeit Tabelle **Upsert** -Vorgänge. Es gibt zwei Arten von **Upsert**, beide über effizienter als eine traditionelle **Einfügen** und **Aktualisieren** Vorgänge werden können:  

-   **InsertOrMerge**: Verwenden Sie diese Option, wenn Sie eine Teilmenge der Eigenschaften der Entität hochladen möchten, aber nicht sicher sind, ob die Entität bereits vorhanden ist. Wenn die Entität vorhanden ist, aktualisiert diese Anruf die Eigenschaften in der Vorgang **Upsert** enthalten, und lässt alle vorhandene Eigenschaften unverändert, wenn die Entität nicht vorhanden ist, wird, wird die neue Entität eingefügt. Dies ist ähnlich wie bei der Verwendung von Projektion in einer Abfrage, darin, dass Sie nur die Eigenschaften hochladen, die geändert werden müssen.
-   **InsertOrReplace**: Verwenden Sie diese Option, wenn Sie eine vollständig neue Entität hochladen möchten, aber Sie nicht sicher sind, ob es bereits vorhanden ist. Sie sollten dies nur verwenden, wenn Sie wissen, dass die neu hochgeladene Entität vollständig korrekt ist, da die alte Entität vollständig überschrieben wird. Angenommen, möchten Sie die Entität aktualisieren, in der aktuellen Position des Benutzers, unabhängig davon, unabhängig davon, ob die Anwendung Standortdaten für den Benutzer zuvor gespeichert wurde gespeichert; die neue Position Entität abgeschlossen ist, und Sie brauchen keine Informationen aus einer beliebigen vorherigen Entität.

#####<a name="a-namesubheading37astoring-data-series-in-a-single-entity"></a><a name="subheading37"></a>Speichern von Datenreihen in einer einzelnen Entität
Manchmal ist es eine Anwendung speichert eine Reihe von Daten, die sie häufig auf einmal abrufen muss: beispielsweise eine Anwendung möglicherweise nachverfolgen CPU-Auslastung über einen Zeitraum um ein paralleles Diagramm der Daten aus den letzten 24 Stunden dargestellt werden. Eine Möglichkeit besteht darin, eine Tabellenentität pro Stunde, mit dem jede Person, die eine bestimmte Stunde darstellt und die CPU-Auslastung für die Stunde gespeichert haben. Um diese Daten zu zeichnen, muss die Anwendung die Personen, halten die Daten aus den letzten 24 Stunden abrufen.  

Alternativ Ihrer Anwendung eine CPU-Auslastung für jede Stunde als separate Eigenschaft einer einzelnen Entität gespeichert: um jede Stunde zu aktualisieren, kann Ihrer Anwendung ein einzelnes **InsertOrMerge Upsert** Anrufs verwenden, um den Wert für die letzte Stunde aktualisieren. Um die Daten zu zeichnen, muss die Anwendung nur eine einzelne Entität anstelle von 24, ausführenden für eine sehr effiziente Abfrage (siehe oben Diskussion auf [Abfrage Umfang](#subheading30)) abzurufen.

#####<a name="a-namesubheading38astoring-structured-data-in-blobs"></a><a name="subheading38"></a>Speichern von strukturierten Daten in blobs
Manchmal strukturierte Daten scheint sollte er in Tabellen, aber Bereiche von Elementen abgerufen werden immer zusammen, und Stapel eingefügt werden kann.  Ein guter Beispiel hierfür ist eine Protokolldatei.  In diesem Fall können Sie einige Minuten von Protokollen Stapel, fügen Sie sie, und klicken Sie dann rufen Sie immer jeweils sowie einige Minuten der Protokolle.  In diesem Fall empfiehlt sich für die Leistung, Blobs anstelle von Tabellen, verwendet werden, da die Anzahl der Objekte geschrieben/zurückgegeben, sowie normalerweise die Anzahl der Anfragen, die benötigen erheblich reduziert werden können.  

##<a name="queues"></a>Warteschlangen
###<a name="a-namesubheading39ascalability-limits"></a>< einen Namen subheading39 = "></a>Skalierbarkeit Beschränkungen
Eine einzelne Warteschlange kann 2.000 Nachrichten (1 KB jedes) pro Sekunde (jede AddMessage, GetMessage und DeleteMessage zählen als hier eine Nachricht) verarbeiten. Wenn dies nicht ausreichend für eine Anwendung ist, verwenden Sie mehrerer und die Nachrichten, die auf diese verteilt.  

Aktuelle Skalierbarkeit Ziele bei [Azure Speicher Skalierbarkeit und Leistungsziele](storage-scalability-targets.md)anzeigen  

###<a name="a-namesubheading40anagle-off"></a>< einen Namen subheading40 = "></a>Nagle deaktivieren
Finden Sie im Abschnitt auf Tabelle-Konfiguration, die den Nagle-Algorithmus erläutert – Nagle-Algorithmus ist im Allgemeinen schlecht für die Leistung der Warteschlange Anfragen, und deaktivieren Sie es.  

###<a name="a-namesubheading41amessage-size"></a>< einen Namen subheading41 = "></a>Nachrichtengröße
Warteschlange Leistung und Skalierbarkeit verringert als Nachricht vergrößert. Sie sollten nur die Informationen, die der Empfänger muss in einer Nachricht platzieren.  

###<a name="a-namesubheading42abatch-retrieval"></a>< einen Namen subheading42 = "></a>Stapel abrufen
Sie können bis zu 32 Nachrichten aus einer Warteschlange in einem einzigen Vorgang abrufen. Dies kann von der Clientanwendung, besonders für Umgebungen, wie z. B. mobilen Geräten, also die Anzahl der Roundtrips verringern, mit langen Wartezeiten.  

###<a name="a-namesubheading43aqueue-polling-interval"></a>< einen Namen subheading43 = "></a>Warteschlange Abrufintervall
Die meisten Applikationen Umfrage für Nachrichten aus einer Warteschlange, die einer der größten Quellen von Transaktionen für die Anwendung werden können. Wählen Sie Ihre Abrufintervalls sorgfältig: die Anwendung der Skalierbarkeit Ziele für die Warteschlange fast veranlassen zu häufig abrufen. Ist jedoch am 200.000 Transaktionen für 0,01 US-Dollar (zum Zeitpunkt der Writing) ein einzelner Prozessor abrufen, sobald pro Sekunde für einen Monat kleiner als 15 Centbeträge also Kosten Kosten würde nicht in der Regel ein Faktor, der Ihre Auswahl des Abrufintervalls bestimmt.  

Auf dem neuesten Stand Kosteninformationen finden Sie unter [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/).  

###<a name="a-namesubheading44aupdatemessage"></a>< einen Namen subheading44 = "></a>UpdateMessage
Sie können die **UpdateMessage** zum Erhöhen des Invisibility Zeitlimits oder zum Aktualisieren von Statusinformationen einer Nachricht verwenden. Während sich leistungsfähige handelt, denken Sie daran, dass jede **UpdateMessage** Vorgang in der Zielliste Skalierbarkeit Richtung zählt. Dies kann jedoch eine sehr viel effizienter Ansatz als einen Workflow, der einen Auftrag von eine Warteschlange in die nächste fließt Probleme, wie die einzelnen Schritte des Auftrags abgeschlossen sind. Mithilfe des Vorgangs **UpdateMessage** kann die Anwendung den Auftragsstatus der Nachricht speichern und dann fortfahren, anstatt auf die Nachricht für den nächsten Schritt des Projekts erneut queuing, jedes Mal, wenn ein Schritt abgeschlossen ist.  

Weitere Informationen finden Sie im Artikel [How to: Ändern des Inhalts einer Nachricht in der Warteschlange](storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

###<a name="a-namesubheading45aapplication-architecture"></a>< einen Namen subheading45 = "></a>Architektur der Anwendung
Sie sollten Warteschlangen verwenden, um Ihrer Anwendungsarchitektur skalierbare stellen. Die folgende Liste enthält einige Verwendungsmöglichkeiten Warteschlangen Ihrer Anwendung besser skalierbar vornehmen:  

-   Sie können Warteschlangen erstellen Rückstände Arbeit für die Verarbeitung und Glätten Auslastung in Ihrer Anwendung verwenden. Beispielsweise könnten Sie von Anfragen von Benutzern zur Ausführung von stark arbeiten Prozessor, wie etwa das Ändern der Größe hochgeladene Bilder Warteschlange.
-   Sie können Warteschlangen verwenden, um Teile der Anwendung zu entkoppeln, sodass diese unabhängig voneinander zu skalieren. Beispielsweise könnten eine Web-Front-End-Umfrageergebnisse von Benutzern in einer Warteschlange zur späteren Analyse und Speicher platzieren. Sie können mehrere Instanzen von Arbeitskollegen Rolle zum Verarbeiten der Warteschlangendaten nach Bedarf hinzufügen.  

##<a name="conclusion"></a>Abschluss
In diesem Artikel erläutert einige der am häufigsten verwendeten bewährte Methoden zum Optimieren der Leistung bei Verwendung von Azure-Speicher. Wir empfehlen jeder Anwendungsentwickler in ihrer Anwendung in jeder der oben genannten Methoden bewerten und fungiert, über die empfohlene zu hohe Leistung für ihre Applikationen zu erhalten, verwenden Azure-Speicher, zu berücksichtigen sind.
