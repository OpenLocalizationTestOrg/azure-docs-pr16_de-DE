<properties 
    pageTitle="DocumentDB hierarchische Ressourcenmodell und Konzepte | Microsoft Azure" 
    description="Informationen Sie zu DocumentDBs hierarchische Modell Datenbanken, Sammlungen, benutzerdefinierte Funktion (UDFs), Dokumente, die Berechtigungen zum Verwalten von Ressourcen und vieles mehr."
    keywords="Hierarchische Modell, Documentdb, Azure, Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>DocumentDB hierarchische Ressourcenmodell und Konzepte

Die Datenbank-Personen, die DocumentDB verwaltet werden als **Ressourcen**bezeichnet. Jede Ressource wird durch einen logischen URI eindeutig identifiziert. Sie können mit den Ressourcen mithilfe von standardmäßigen HTTP-Verben, Anforderung/Antwort Kopf- und Statuscodes interagieren. 

Lesen Sie in diesem Artikel, können Sie die folgenden Fragen beantworten ausführen:

- Was ist DocumentDBs Ressourcenmodell?
- Was sind definiert Ressourcen im Gegensatz zu benutzerdefinierten Ressourcen?
- Wie adressiere ich eine Ressource?
- Wie arbeite ich mit Websitesammlungen?
- Wie arbeite ich mit gespeicherten Prozeduren, Trigger und User Defined Functions, (UDFs)?

## <a name="hierarchical-resource-model"></a>Hierarchische Ressourcenmodell
Wie das folgende Diagramm veranschaulicht, besteht Sätze von Ressourcen unter einem Datenbankkonto, die jeweils über eine logische und unveränderliche URI adressiert hierarchische DocumentDB **Ressourcenmodell** aus. Eine Reihe von Ressourcen wird ein **feed** in diesem Artikel genannt werden. 

>[AZURE.NOTE] DocumentDB bietet eine hochgradig effizienten TCP-Protokoll der RESTful auch in deren Kommunikationsmodell, über den [.NET Client SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)verfügbar ist.

![DocumentDB hierarchische Ressourcenmodell][1]  
**Hierarchische Ressourcenmodell**   

Zum Arbeiten mit Ressourcen zu starten, müssen Sie Ihr Abonnement Azure mit [einer DocumentDB Datenbank-Konto erstellen](documentdb-create-account.md) . Eine Datenbankkonto kann bestehen aus einer Reihe von **Datenbanken**, jede mit mehreren **Websitesammlungen**, von denen jede wiederum enthalten **gespeicherten Prozeduren, ausgelöst wird, UDFs, Dokumente** und zugehörigen **Anlagen** (Funktion). Eine Datenbank weist auch **Benutzer**, jede mit einer Reihe von **Berechtigungen** zum Zugriff auf Websitesammlungen, gespeicherten Prozeduren, Triggern, UDFs, Dokumente oder Anlagen verknüpft ist. Während der Datenbanken, Benutzer, Berechtigungen und Websitesammlungen Ressourcen mit bekannten Schemas System definiert sind, Dokumente und Anlagen enthalten zufällige, benutzerdefinierte JSON-Inhalt.  

|Ressource   |Beschreibung
|-----------|-----------
|Datenbankkonto   |Eine Datenbankkonto ist eine Reihe von Datenbanken und einen festen Betrag Blob-Speicher für Anlagen (Funktion) zugeordnet. Sie können eine oder mehrere Datenbankkonten mit Ihrem Abonnement Azure erstellen. Weitere Informationen finden Sie auf unserer [Seite Preise](https://azure.microsoft.com/pricing/details/documentdb/).
|Datenbank   |Eine Datenbank ist ein logischer Container Dokument Speicher über Websitesammlungen aufgeteilt. Es ist ebenfalls ein Benutzercontainer.
|Benutzer   |Die logischen Namespace für Bereichsdefinition Berechtigungen. 
|Berechtigung |Ein Autorisierungstoken eines Benutzers für den Zugriff auf eine bestimmte Ressource zugeordnet ist.
|Websitesammlung |Eine Auflistung ist ein Container für JSON-Dokumente und die Logik der zugehörigen JavaScript. Eine Auflistung ist eine berechenbare Entität, wobei die [Kosten](documentdb-performance-levels.md) durch die Leistung Ebene der Websitesammlung zugeordnet bestimmt. Websitesammlungen können einen oder mehrere Partitionen/Server umfassen und zum Behandeln von praktisch unbegrenzte Datenmengen Speicher oder Durchsatz skalieren können.
|Gespeicherte Prozedur   |Anwendungslogik geschrieben in JavaScript mit einer Websitesammlung registriert und überführen innerhalb der Datenbank-Engine ausgeführt wird.
|Auslösen    |Anwendungslogik geschrieben in JavaScript vor oder nach Abschluss eines einfügen, ersetzen oder löschen.
|UDFS    |Die Logik in JavaScript geschrieben. UDFs können Sie einen benutzerdefinierte Abfrageoperator modellieren und damit auch die Core DocumentDB Abfragesprache erweitern.
|Dokument   |Benutzerdefinierte (beliebigen) JSON-Inhalt. Standardmäßig kein Schema muss definiert werden, noch müssen sekundäre Indizes für alle Dokumente, die einer Websitesammlung hinzugefügt bereitgestellt werden.
|(Preview) Anlage   |Eine Anlage ist eine spezielle Unterlage Verweise und zugehörigen Metadaten für externe Blob-Medien. Der Entwickler kann festlegen, dass des BLOBs von DocumentDB verwaltet oder mit einem externen Blob-Dienstanbieter wie etwa OneDrive, Dropbox usw. zu speichern. 


## <a name="system-vs-user-defined-resources"></a>System im Vergleich zu benutzerdefinierten Ressourcen
Ressourcen wie Datenbankkonten, Datenbanken, Sammlungen, Benutzer, Berechtigungen, gespeicherten Prozeduren, Triggern und UDFs - alle haben eine feste Schema und werden Systemressourcen bezeichnet. Im Gegensatz dazu Ressourcen wie Dokumente und Anlagen haben keine Einschränkungen auf dem Schema und sind Beispiele für benutzerdefinierte Ressourcen. In DocumentDB, System und die benutzerdefinierten Ressourcen dargestellt und als Standard-kompatible JSON verwaltet werden. Alle Ressourcen, System oder benutzerdefinierte Verben, haben die folgenden allgemeinen Eigenschaften.

> [AZURE.NOTE] Beachten Sie, dass alle System Eigenschaften in einer Ressource generiert eine Unterstrichs (_) in den JSON-Darstellung vorangestellt werden.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschaft</strong></p></td>
            <td valign="top"><p><strong>Benutzer festgelegt werden oder vom System generiert werden?</strong></p></td>
            <td valign="top"><p><strong>Zweck</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>System erzeugt</p></td>
            <td valign="top"><p>Vom System generierte, eindeutigen und hierarchische Bezeichner für die Ressource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>System erzeugt</p></td>
            <td valign="top"><p>eTag der Ressource für die Steuerung der vollständigen Parallelität erforderlich ist</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>System erzeugt</p></td>
            <td valign="top"><p>Zuletzt aktualisierte Timestamp der Ressource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>System erzeugt</p></td>
            <td valign="top"><p>Eindeutige adressiert URI der Ressource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID</p></td>
            <td valign="top"><p>System erzeugt</p></td>
            <td valign="top"><p>Benutzerdefinierte eindeutigen Namen der Ressource (mit den gleichen Partition Schlüsselwert). Wenn der Benutzer kein Personalnummer angegeben ist, wird eine Id vom System generiert wird.</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Über das Netzwerk Darstellung von Ressourcen
DocumentDB keine unterstützten geschützten Erweiterungen zu den JSON-Standardberechtigungen oder besonderen Codierungen verlangen; Es funktioniert mit standard kompatible JSON-Dokumente.  
 
### <a name="addressing-a-resource"></a>Adressieren einer Ressource
Alle Ressourcen werden URI adressiert. Der Wert der Eigenschaft **_self** einer Ressource stellt den relativen URI der Ressource. Das Format des URIS umfasst die /\<feed\>/ {_rid} Pfadsegmente:  

|Wert der _self |Beschreibung
|-------------------|-----------
|/DBS   |Feed Datenbanken unter einer Datenbankkonto
|/DBS/ {DB-Name}  |Datenbank mit der Id mit dem Wert {DB-Name} übereinstimmen
|/colls/ /DBS/ {DB-Name}   |Feed Websitesammlungen unter einer Datenbank
|/DBS/ {DB-Name} /colls/ {CollName} |Websitesammlung mit einer Id mit dem Wert {CollName} übereinstimmen
|/DBS/ {DB-Name} /colls/ {CollName} / Dokumente    |Feed Dokumente unter einer Websitesammlung
|/DBS/ {DB-Name} /colls/ {CollName} /docs/ {DocId}    |Dokument mit der Personalnummer mit dem Wert {Doc} übereinstimmen
|/DBS/ {DB-Name} / Users /   |Feed der Benutzer unter einer Datenbank
|/ Users /DBS/ {DB-Name} / {Benutzer-ID}   |Benutzer mit der Id mit dem Wert {User} übereinstimmen
|/ Users /DBS/ {DB-Name} / {Benutzer-ID} / Berechtigungen   |Feed Berechtigungen unter einem Benutzerkonto
|/DBS/ {DB-Name} / Users / {Benutzer-ID} /permissions/ {PermissionId}    |Berechtigung eine ID, die mit dem Wert {Berechtigung} übereinstimmen
  
Jede Ressource weist einen eindeutigen definierten Namen über die ID-Eigenschaft verfügbar gemacht. Hinweis: für Dokumente, wenn der Benutzer kein Personalnummer, angegeben wird unsere unterstützten SDKs automatisch eine eindeutige Id für das Dokument generiert. Die Id ist eine benutzerdefinierte Zeichenfolge, bis zu 256 Zeichen ein, die innerhalb des Kontexts von einer bestimmten übergeordnete Ressource eindeutig ist. 

Jede Ressource weist auch einen System generiert hierarchische Resource Identifier (auch als eine RID bezeichnet), die über die Eigenschaft _rid verfügbar ist. Die RID codiert die gesamte Hierarchie einer bestimmten Ressource, und es wird eine geeignete interne Darstellung verwendet, um die referenzielle Integrität zu erzwingen, verteilt. Die RID innerhalb einer Datenbankkonto eindeutig ist, und es wird intern von DocumentDB verwendet, für das effiziente routing ohne Cross Partition Suchvorgänge. Die Werte der _self und die _rid Eigenschaften sind alternative und kanonische Darstellungen von einer Ressource an. 

Die REST-APIs DocumentDB unterstützen Adressieren von Ressourcen und das Weiterleiten von Besprechungsanfragen, indem Sie sowohl die Id als auch die _rid Eigenschaften.

## <a name="database-accounts"></a>Datenbankkonten
Sie können eine oder mehrere DocumentDB Datenbankkonten mithilfe Ihres Abonnements Azure bereitstellen.

Sie können das [Erstellen und Verwalten von DocumentDB Datenbank-Konten](documentdb-create-account.md) über das Azure-Portal unter [http://portal.azure.com/](https://portal.azure.com/). Erstellen und Verwalten von einem Datenbankkonto erfordert Administratorzugriff und kann nur unter Ihrem Abonnement Azure ausgeführt werden. 

### <a name="database-account-properties"></a>Eigenschaften von Benutzerkonten
Sie können als Teil der Bereitstellung und Verwalten eines Datenbankkonto konfigurieren und lesen die folgenden Eigenschaften:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschaftsname</strong></p></td>
            <td valign="top"><p><strong>Beschreibung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Konsistenz-Richtlinie</p></td>
            <td valign="top"><p>Legen Sie diese Eigenschaft so konfigurieren Sie die Konsistenz Standardstufe für alle Websitesammlungen unter Ihrer Datenbankkonto an. Sie können die Konsistenz Ebene mit der Anfrage [X-ms-Konsistenz-Ebene] Kopfzeile für jeden Anforderung außer Kraft setzen. <p><p>Beachten Sie, dass diese Eigenschaft nur auf die <i>benutzerdefinierte Ressourcen</i>angewendet wird. Alle definierte Ressourcen zur Unterstützung von konfigurierten System liest/Abfragen mit signifikante Konsistenz.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Autorisierung Tasten</p></td>
            <td valign="top"><p>Hierbei handelt es sich um die primären und sekundären Master und schreibgeschützt Tasten, die Administratorzugriff auf alle Ressourcen unter dem Datenbankkonto bereitstellen.</p></td>
        </tr>
    </tbody>
</table>

Beachten Sie, dass zusätzlich bereitgestellt, konfigurieren und Verwalten von Ihrem Datenbankkonto vom Azure-Portal, Sie können auch programmgesteuert erstellen und Verwalten von DocumentDB Datenbankkonten mithilfe von [Azure DocumentDB REST APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) als auch [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Datenbanken
Eine DocumentDB Datenbank ist ein logischer Container, der eine oder mehrere Websitesammlungen und Benutzer aus, wie in der folgenden Abbildung gezeigt. Sie können eine beliebige Anzahl von Datenbanken unter einem DocumentDB Datenbankkonto beschränkt Angebot erstellen.  

![Datenbank-Konto und Websitesammlungen hierarchische Modell][2]  
**Eine Datenbank ist ein logischer Container von Benutzern und Websitesammlungen**

Eine Datenbank kann praktisch unbegrenzte Dokument-Speicher von Websitesammlungen, die die Transaktion Domänen für die darin enthaltenen Dokumente bilden aufgeteilt enthalten. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Flexible Skalieren einer Datenbank DocumentDB
Eine DocumentDB Datenbank ist standardmäßig – von wenigen GB bis hin zu Petabyte SSD gesicherten Speicherung von Dokumenten und bereitgestellte Durchsatz flexible. 

Im Gegensatz zu einer Datenbank in herkömmlichen RDBMS wird eine Datenbank im DocumentDB nicht auf einem einzelnen Computer beschränkt. Mit DocumentDB wie Ihrer Anwendung Maßstab werden sollen vergrößert muss, können Sie mehrere Websitesammlungen und/oder Datenbanken erstellen. Tatsächlich um, verschiedene erste Anwendungen von Drittherstellern in Microsoft DocumentDB mit Maßen Consumer seit verwenden durch Erstellen von sehr großen DocumentDB Datenbanken jedes enthaltenden Tausende von Websitesammlungen mit TB Speicherkapazität des Dokuments. Sie können vergrößert oder verkleinert werden eine Datenbank durch Hinzufügen oder Entfernen von Websitesammlungen, um Ihrer Anwendung Skala zu erfüllen. 

Sie können eine beliebige Anzahl von Websitesammlungen in einer Datenbank unterliegen Angebot erstellen. Jede Websitesammlung enthält SSD gesicherten Speicher und Durchsatz für Sie je nach der ausgewählten Leistung Ebene bereitgestellt.

Eine Datenbank DocumentDB ist auch ein Container von Benutzern. Ein Benutzer in aktivieren, wird für eine Reihe von Berechtigungen, die abgestimmte Autorisierung und Zugriff auf Websitesammlungen, Dokumente und Anlagen stellt einen logischen Namespace.  
 
Wie bei anderen Ressourcen im Modell Ressource DocumentDB Datenbanken erstellt werden können, ersetzt, gelöscht, lesen oder einfach mit [Azure DocumentDB REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder den [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)aufgelistet. DocumentDB garantiert signifikante Konsistenz zum Lesen oder Abfragen der Metadaten einer Datenbank Ressource an. Löschen einer Datenbank automatisch stellt sicher, dass Sie eine der darin enthaltenen Benutzer oder Websitesammlungen zugreifen können.   

## <a name="collections"></a>Websitesammlungen
Eine Auflistung von DocumentDB ist ein Container für Ihre Dokumente JSON. Eine Auflistung ist auch eine Einheit der Anzahl der Dezimalstellen für Transaktionen und Abfragen. 

### <a name="elastic-ssd-backed-document-storage"></a>Flexible SSD gesicherten Speicherung von Dokumenten
Eine Auflistung ist systembedingt flexible – automatisch wächst und verkleinert beim Hinzufügen oder Entfernen von Dokumenten. Websitesammlungen sind logische Ressourcen und physische Partitionen oder Servern umfassen können. Die Anzahl der Partitionen in einer Sammlung wird durch DocumentDB ausgehend von der Größe und der bereitgestellte Durchsatz Ihrer Websitesammlung bestimmt. Jeder Partition in DocumentDB hat eine bestimmte SSD gesicherten Speicher zugeordnet und hohen Verfügbarkeit repliziert. Verwaltung der Anwendungsverzeichnispartition vollständig von Azure DocumentDB verwaltet wird, und Sie haben keinen zum Schreiben von Code komplexe oder verwalten Ihre Partitionen. DocumentDB Websitesammlungen sind **praktisch unbegrenzte** in Bezug auf die Speicherung und Durchsatz. 

### <a name="automatic-indexing-of-collections"></a>Automatische Indizierung von Websitesammlungen
DocumentDB ist ein WAHR Schema frei Datenbanksystem. Es wird davon ausgegangen oder keine Schema für die JSON-Dokumente erforderlich. Beim Hinzufügen von Dokumenten zu einer Websitesammlung, DocumentDB automatisch indiziert werden können, und für Ihre Abfrage verfügbar sind. Automatische Indizieren von Dokumenten ohne Schema oder sekundäre Indizes ist eine wichtige Funktion von DocumentDB und durch Schreiben optimiert, ohne Sperren und Log-strukturierten Index Wartung Techniken aktiviert ist. DocumentDB unterstützt längeren Lautstärke äußerst schnell schreibt während des weiterhin bedienen konsistente Abfragen. Sowohl Dokument und Index-Speicher werden verwendet, um die Speicherung von jeder Websitesammlung verbraucht zu berechnen. Sie können die Speicherung und Leistung Kompromisse Indizierung durch Konfigurieren der Indizierung für eine Websitesammlung zugeordnet steuern. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Konfigurieren der Richtlinie für Indizierung einer Websitesammlung
Die Indizierung Richtlinien für jede Websitesammlung ermöglicht Ihnen, die Leistung und Speicher vor-und Nachteile Indizierung zugeordnet. Folgenden Optionen stehen Ihnen als Teil der Indizierungskonfiguration:  

-   Wählen Sie aus, ob die Sammlung automatisch alle Dokumente oder nicht indiziert. Standardmäßig werden alle Dokumente automatisch indiziert. Sie können auswählen, deaktivieren die automatische Indizierung und selektives nur bestimmte Dokumente auf den Index hinzufügen. Sie können umgekehrt Selektives nur bestimmte Dokumente auszuschließen. Sie können dies erreichen, indem der automatischen Eigenschaft wahr oder falsch, klicken Sie auf die IndexingPolicy einer Websitesammlung sein, und verwenden die Kopfzeile [X-ms-Indexingdirective] Anforderung beim Einfügen, ersetzen oder Löschen eines Dokuments.  
-   Wählen Sie aus, ob ein- oder Ausschließen bestimmter Pfade oder Mustern in Ihren Dokumenten aus dem Index. Sie können dies durch die Einstellung IncludedPaths und ExcludedPaths auf die IndexingPolicy einer Websitesammlung Hilfethemas erzielen. Sie können auch die Speicherung und Leistung Kompromisse für Bereichs- und Hash Abfragen nach bestimmten Pfad Mustern konfigurieren. 
-   Wählen zwischen synchroner (konsistent) und asynchrone (aus-) Index Updates. Standardmäßig wird der Index synchron auf jede einfügen, ersetzen oder Löschen eines Dokuments in der Auflistung aktualisiert. Dadurch werden die Abfragen die gleichen Ebene der Konsistenz des Dokuments lautet wie berücksichtigt. Während der DocumentDB schreiben optimiert ist und unterstützt längere Datenmengen Dokument schreibt zusammen mit synchroner Indexwartung und konsistente Abfragen erstellen, können Sie bestimmte Websitesammlungen, um deren Index verzögert aktualisieren konfigurieren. Verzögerte Indizierung steigert die Leistung von schreiben weiteren und eignet sich optimal für Massen Aufnahme Szenarien hauptsächlich gelesen überladene Websitesammlungen.

Die Indizierung Richtlinie kann durch Ausführen von einer sich in der Websitesammlung geändert werden. Dies kann erreicht werden entweder über den [Client SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx), [Azure-Portal](https://portal.azure.com) oder die [Azure DocumentDB REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Eine Auflistung Abfragen
Dokumente in einer Websitesammlung können beliebige Schemas haben und Sie können Dokumente in einer Sammlung ohne Angabe eines Schemas oder sekundäre Indizes vorab Abfragen. Sie können die Sammlung der [DocumentDB SQL-Syntax](https://msdn.microsoft.com/library/azure/dn782250.aspx), die Rich hierarchische, relationale bereitstellt, und räumliche Operatoren und Erweiterbarkeit über JavaScript-basierte UDFs Abfragen. JSON-Grammatik ermöglicht modeling JSON-Dokumente als Strukturen mit Etiketten als die Knoten. Dies ist sowohl durch automatischen Indizierung Techniken für die DocumentDB als auch DocumentDBs SQL-Dialekt genutzt werden. Die DocumentDB-Abfragesprache umfasst drei wichtigsten Aspekte:   

1.  Eine kleine Gruppe von Abfragevorgänge, die die hierarchische Abfragen und Projektionen einschließlich der Baumstruktur zugeordnet werden soll. 
2.  Eine Teilmenge der relationalen Vorgänge einschließlich Komposition, filtern, Projektionen, Aggregate und Self Verknüpfungen. 
3.  Reines JavaScript-basierte UDFs, die Arbeit mit (1) und (2).  

Das Modell DocumentDB Abfrage versucht, ein Gleichgewicht zwischen Funktionalität, Effizienz und Vereinfachung. Die Datenbank-Engine DocumentDB systembedingt kompiliert und die SQL-Abfrage-Anweisungen ausgeführt. Sie können eine Auflistung mithilfe der [Azure DocumentDB REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eine [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)Abfragen. .NET SDK verfügt über ein LINQ-Anbieter.

> [AZURE.TIP] Sie können DocumentDB testen und unser Dataset in den [Abfrage-Umgebung](https://www.documentdb.com/sql/demo)SQL-Abfragen ausführen.

### <a name="multi-document-transactions"></a>Dokument mit mehreren Transaktionen.
Datenbanktransaktionen bieten eine sichere und vorhersehbar programming Modell für den Umgang mit gleichzeitige geänderten Daten. In RDBMS besteht die traditionelle Möglichkeit zum Schreiben von Geschäftslogik zu **gespeicherten Prozeduren** und/oder **Trigger** schreiben, und versenden sie mit dem Datenbankserver für die Ausführung von Transaktionen. RDBMS ist der Anwendungsprogrammierer angefordert zwei unterschiedlichen Sprachen behandelt: 

- (Ausgelegt) Anwendung Programmiersprache (z. B. JavaScript, Python, c#, Java usw.)
- T-SQL, die Transaktionen Programmiersprache, die direkt von der Datenbank ausgeführt wird

Aufgrund von deren Tiefe Verpflichtung JavaScript und JSON direkt in die Datenbank-Engine enthält DocumentDB ein intuitives programming Modell JavaScript-basierte Anwendungslogik direkt auf die Websitesammlungen in Bezug auf die gespeicherten Prozeduren und Triggern ausgeführt. Dadurch wird für beide der folgenden Aktionen aus:

- Effiziente Implementierung gleichzeitige Verarbeitung steuern, Wiederherstellung, automatische Indizierung der Diagramme JSON-Objekt direkt in die Datenbank-engine
- So bin Ausdrücken Fluss Steuerelements, Variable Bereichsdefinition, Zuordnung und Behandlung Primitives mit Datenbanktransaktionen direkt im Hinblick auf die Programmiersprache JavaScript-integration

JavaScript-Logik registriert Ebene einer Websitesammlung kann dann Datenbankvorgänge an den Dokumenten der angegebenen Websitesammlung ausgeben. DocumentDB umschließt implizit das JavaScript-basierte gespeicherten Prozeduren und Trigger innerhalb einer umgebende ACID-Transaktionen mit Snapshot-Isolation über Dokumente in einer Websitesammlung. Im Verlauf der Ausführung, wenn das JavaScript eine Ausnahme auslöst, wird die gesamte Transaktion abgebrochen. Das sich daraus ergebende programming Modell ist eine sehr einfache noch leistungsfähige. JavaScript-Entwickler erhalten ein "dauerhaftes" programming Modell gleichzeitiger Verwendung ihrer vertrauten Konstrukte und Bibliothek Primitives.   

Die Möglichkeit zur Ausführung von JavaScript direkt in die Datenbank-Engine im gleichen Adressbereich als dem Pufferpool ermöglicht leistungsfähige und Ausführung von Transaktionen der Datenbankoperationen für die Dokumente einer Websitesammlung. Darüber hinaus DocumentDB Datenbank-Engine verpflichtet sich eine Tiefe auf das JSON und JavaScript beseitigt jede Impedanz Abweichung zwischen der Systeme vom Typ der Anwendung und der Datenbank.   

Nach dem Erstellen einer Websitesammlungs, können Sie gespeicherte Prozeduren, Trigger und UDFs mit einer Websitesammlung mithilfe der [Azure DocumentDB REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eine [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)registrieren. Nach der Registrierung können verweisen und ausführen können. Erwägen Sie die folgende gespeicherte Prozedur vollständig in JavaScript geschrieben, der folgenden Code verwendet zwei Argumente (Adressbuch Namen und Name des Autors) und erstellt ein neues Dokument, für ein Dokument Abfragen und anschließend aktualisiert wird – alle innerhalb einer implizit ACID-Transaktion. Wenn eine JavaScript-Ausnahme ausgelöst wird, wird Sie jederzeit während der Ausführung die gesamte Transaktion abgebrochen.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Der Client kann "oben JavaScript-Logik in der Datenbank für die Ausführung von Transaktionen über HTTP POST verschicken". Weitere Informationen zur Verwendung von HTTP-Methoden finden Sie unter [Rest Interaktionen mit DocumentDB Ressourcen](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Beachten Sie, dass keine System Typenkonflikt, keine "Oder Zuordnung" oder Code Generation magische erforderlich, da die Datenbank systembedingt JSON und JavaScript versteht, vorhanden ist.   

Interagieren mit einer Websitesammlung und die Dokumente in einer Websitesammlung über eine klar definierte Objektmodell, das im aktuellen Kontext für die Websitesammlung verfügbar macht, gespeicherten Prozeduren und Triggern.  

Websitesammlungen in DocumentDB erstellt werden können, gelöscht, lesen oder einfach mit entweder aufgelistet, die [REST-APIs von Azure DocumentDB](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eine [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB enthält immer signifikante Konsistenz zum Lesen oder Abfragen der Metadaten einer Websitesammlung. Beim Löschen einer Sammlung automatisch gewährleistet, dass Sie keine Dokumente, Anlagen, gespeicherten Prozeduren, Triggern zugreifen und UDFs in ihr enthaltenen.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Gespeicherte Prozeduren, Trigger und Benutzer benutzerdefinierte Funktionen (UDFs)
Wie im vorherigen Abschnitt beschrieben, können Sie die Anwendungslogik direkt innerhalb einer Transaktion innerhalb der Datenbank-Engine ausgeführt schreiben. Die Logik der vollständig in JavaScript geschrieben werden kann und wie eine gespeicherte Prozedur, Trigger oder UDFs erstellt werden kann. Der JavaScript-Code in einer gespeicherten Prozedur oder eines Triggers kann einfügen, ersetzen, löschen, lesen oder Abfrage Dokumente innerhalb einer Websitesammlung. Auf der anderen Seite kann nicht der JavaScript-Code in UDFs einfügen, ersetzen oder Löschen von Dokumenten. UDFs Auflisten der Dokumente Resultset einer Abfrage und erstellen ein weiteres Resultset. Für mehrere Mandanten erzwingt DocumentDB einer Ressource-Governance strict Reservierung basierend auf. Jede gespeicherte Prozedur, Trigger oder UDFs erhält eine feste Quantum Betriebssystem Ressourcen seine Arbeit erledigen. Darüber hinaus, gespeicherten Prozeduren, Triggern oder zu UDFs nicht verknüpfen mit externen JavaScript-Bibliotheken und werden gesperrt, wenn sie die Ressource Budgets, die ihnen zugewiesenen überschreiten. Sie können registrieren, Aufheben der Registrierung gespeicherten Prozeduren, Triggern oder zu UDFs mit einer Websitesammlung mithilfe der REST-APIs.  Bei der Registrierung wird eine gespeicherte Prozedur, Trigger oder UDFs vorab kompiliert und gespeichert Byte-Code später ausgeführt wird. Im folgende Abschnitt beschreiben, wie Sie verwenden die DocumentDB JavaScript-SDK zu erfassen, ausführen und Aufheben der Registrierung eine gespeicherte Prozedur, Trigger und UDFs. Das JavaScript SDK ist ein einfacher Wrapper über die [REST-APIs DocumentDB](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Registrieren eine gespeicherte Prozedur
Registrierung einer gespeicherten Prozedur erstellt eine neue gespeicherte Prozedur Ressource eine Auflistung über HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Ausführen einer gespeicherten Prozedur
Ausführung einer gespeicherten Prozedur ist durch die Ausgabe einer HTTP POST anhand einer vorhandenen gespeicherten Prozedur Ressource durch Übergabe von Parametern an das Verfahren im Hauptteil Anforderung abgeschlossen.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Aufheben der Registrierung einer gespeicherten Prozedur
Aufheben der Registrierung von einer gespeicherten Prozedur erfolgt einfach durch eine HTTP DELETE anhand einer vorhandenen gespeicherten Prozedur Ressource ausgeben.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registrieren eines vor dem Triggers
Registrierungsinformationen eines Triggers ist durch Erstellen einer neuen Ressource der Trigger auf einer Websitesammlung über HTTP POST abgeschlossen. Sie können angeben, ob der Trigger einer Prä ist oder einen Beitrag Trigger und den Typ des Vorgangs können (z. B. erstellen, ersetzen, löschen oder alle) zugeordnet werden.   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Ausführen eines vor dem Triggers
Ausführung eines Triggers abgeschlossen ist, indem Sie den Namen eines vorhandenen Triggers zum Zeitpunkt der der Beitrag/sich/löschen Anforderung einer Ressource Dokument über der Anfrage-Header angeben.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Aufheben der Registrierung vor dem trigger
Aufheben der Registrierung eines Triggers erfolgt einfach über eine HTTP DELETE anhand einer vorhandenen Trigger Ressource ausgeben.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registrieren UDFs
Registrierung von UDFs ist durch Erstellen einer neuen UDFs Ressource für eine Websitesammlung über HTTP POST abgeschlossen.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>UDFs als Teil der Abfrage ausführen
UDFs kann als Teil der SQL-Abfrage angegeben werden und dient als eine Möglichkeit, um das Herzstück [SQL-Abfragesprache der DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx)zu erweitern.

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Aufheben der Registrierung von UDFs 
Aufheben der Registrierung von UDFs erfolgt einfach durch eine HTTP DELETE anhand einer vorhandenen UDFs Ressource ausgeben.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Obwohl die oben angegebenen Codeausschnitte Erfassung (POST), zum Aufheben der Registrierung (sich), gelesen/Liste (GET) und Ausführung (POST) über das [DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js)angezeigt wurden, können Sie auch die [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder anderen [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx). 

## <a name="documents"></a>Dokumente
Sie können einfügen, ersetzen, löschen, lesen, auflisten und Abfragen zufällige JSON-Dokumenten in einer Websitesammlung. DocumentDB keine Schema verlangen und sekundären Indizes zur Unterstützung von Abfragen von Dokumenten in einer Websitesammlung sind nicht erforderlich.   

Wird eine wirklich geöffneten Datenbank-Dienst, DocumentDB nicht Lager-eine spezialisierte Datentypen (z. B. Uhrzeit) oder bestimmte Codierungen für JSON-Dokumente. Beachten Sie, dass DocumentDB nicht, alle Inhalten JSON-Konventionen erforderlich ist auf die Beziehungen zwischen verschiedenen Dokumenten Sicherungssystems; die SQL-Syntax DocumentDB bietet sehr leistungsfähige hierarchische und relationalen Abfrage, dass Operatoren, um die Abfrage und Projektdokumente ohne spezielle Anmerkungen oder müssen Beziehungen zwischen Dokumente mit Sicherungssystems Eigenschaften unterschieden.  
 
Als können mit anderen Ressourcen, Dokumente, ersetzt, gelöschten, finden Sie hier, aufgezählten und einfach mithilfe von REST-APIs oder den [Client SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)abgefragte erstellt werden. Löschen ein Dokument sofort frei von das Kontingent für alle Anlagen geschachtelte entspricht. Die Ebene gelesen Konsistenz Dokumente gelten Konsistenz Richtlinien für die Datenbankkonto an. Dieser Richtlinie kann auf Basis pro Anforderung je nach Daten Konsistenz Anforderungen Ihrer Anwendung überschrieben werden. Bei der Abfrage Dokumente folgt die Konsistenz finden Sie hier den Indizierung Modus legen Sie für die Websitesammlung. Für "konsistent" resultiert dies des Kontos Konsistenz Richtlinie aus. 

## <a name="attachments-and-media"></a>Anlagen und Medien
>[AZURE.NOTE] Anlage und Medien-Ressourcen werden Features Preview.
 
DocumentDB können Sie binäre Blobs/Medien mit DocumentDB oder eigene remote-Media-Store zu speichern. Sie können die Metadaten der Medien in Bezug auf eine spezielle Dokument mit dem Namen der Anlage darstellen. Anlage in DocumentDB ist eine spezielle (JSON)-Dokument, das das Media-Blob-an anderer Stelle aufbewahrt verweist auf. Eine Anlage ist einfach ein spezielle Dokument, das die Metadaten von einer in einer remote-Media-Speicher gespeicherte Medien (z. B. Speicherort, Autor usw.) erfasst werden. 

Erwägen Sie eine sozialen Lesebereich Anwendung, die DocumentDB verwendet, um freihandanmerkungen zu speichern, und Metadaten, einschließlich der Kommentare hervorgehoben, Textmarken, Bewertungen, gefällt mir/Communitysite usw. nach einem e-Mail-Adressbuch eines Benutzers zugeordnet ist.   

-   Der Inhalt des Adressbuchs selbst befindet sich an die Speicherung Medien entweder als Teil des DocumentDB Datenbank-Konto oder einem remote-Media-Speicher zur Verfügung. 
-   Eine Anwendung möglicherweise des Benutzers Metadaten als distinct Dokument speichern – z. B. Helmuts Metadaten für Mappe1 in einem Dokument /colls/joe/docs/book1 optimiert gespeichert ist. 
-   Inhaltsseiten von einem angegebenen Buch eines Benutzers auf Anlagen werden gespeichert, unter der entsprechenden Dokument z. B. /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 usw.. 

Beachten Sie, dass die oben aufgeführten Beispiele geeignet Ids verwenden, um die Ressourcenhierarchie zu vermitteln. Ressourcen können Sie über die REST-APIs über eindeutige Ressourcen-Ids zugreifen. 

Für das Medium, die vom DocumentDB verwaltet wird, werden die Eigenschaft _media der Anlage auf das Medium durch den URI verweisen. Stellen Sie sicher DocumentDB zum Garbage sammeln die Medien, wenn alle ausstehenden Verweise abgelegt werden. DocumentDB automatisch generiert die Anlage, wenn Sie das neue Medium hochladen und füllt die _media auf die neu hinzugefügten Medien verweisen. Wenn Sie zum Speichern der Medien in einem remote-BLOB-Speicher von Ihnen (z. B. OneDrive, Azure-Speicher DropBox usw.) verwalteten auswählen, können Sie weiterhin Anlagen in Bezug auf das Medium verwenden. In diesem Fall werden Sie die Anlage zu erstellen und seine Eigenschaft _media zu füllen.   

Wie bei allen anderen Ressourcen Anlagen erstellt werden können, ersetzt, gelöscht, lesen oder einfach mithilfe von REST-APIs oder eine Client-SDKs aufgelistet. Wie bei Dokumenten gelten die Ebene gelesen Konsistenz von Anlagen Konsistenz Richtlinien für die Datenbankkonto an. Dieser Richtlinie kann auf Basis pro Anforderung je nach Daten Konsistenz Anforderungen Ihrer Anwendung überschrieben werden. Bei der Abfrage für Anlagen folgt die Konsistenz finden Sie hier den Indizierung Modus legen Sie für die Websitesammlung. Für "konsistent" resultiert dies des Kontos Konsistenz Richtlinie aus. 
 
## <a name="users"></a>Benutzer
Ein Benutzer DocumentDB stellt einen logischen Namespace zum Gruppieren von Berechtigungen. Ein Benutzer DocumentDB möglicherweise ein Benutzer in einem Identität-System oder einer vordefinierten Anwendungsrolle entsprechen. Für DocumentDB stellt ein Benutzer einfach eine Abstraktion ein Satzes von Berechtigungen unter einer Datenbank zu gruppieren.   

Für die Durchführung mehrere Mandanten in der Anwendung, können Sie Benutzer in DocumentDB der entspricht erstellen, die tatsächliche Benutzer oder den Mandanten der Anwendung. Anschließend können Sie die Berechtigungen für einen bestimmten Benutzer erstellen, die die Access-Kontrolle über die verschiedenen Websitesammlungen, Dokumente, Anlagen, usw. entsprechen.   

Wie Ihre Applikationen mit Ihrem Wachstum zu skalieren müssen, können Sie Ihre Daten kann auf unterschiedliche Weise Shard übernehmen. Sie können jeden Benutzer wie folgt modellieren:   

-   Jedem Benutzer wird eine Datenbank zugeordnet.
-   Jeder Benutzer ordnet einer Websitesammlung. 
-   Dokumente, die mehrere Benutzer entspricht eine dedizierte Auflistung wechseln. 
-   Dokumente, die mehreren Benutzern entsprechen wechseln Sie zu einer Gruppe von Websitesammlungen.   

Unabhängig davon, die bestimmte Sharding Strategie ausgewählt haben, können Sie Ihre tatsächlichen Benutzer als Benutzer in der Datenbank DocumentDB modellieren und fein detaillierte Berechtigungen für jeden Benutzer zuordnen.  

![Benutzer Websitesammlungen][3]  
**Sharding Strategien und Modellierung Benutzer**

Wie alle anderen Ressourcen Benutzer in DocumentDB erstellt werden können, ersetzt, gelöscht, lesen oder einfach mithilfe von REST-APIs oder eine Client-SDKs aufgelistet. DocumentDB enthält immer signifikante Konsistenz zum Lesen oder Abfragen die Metadaten eines Benutzers einer Ressource. Lohnt sich darauf hin, dass automatisch Löschen eines Benutzers wird sichergestellt, dass Sie keine darin enthaltenen Berechtigungen zugreifen können. Obwohl die DocumentDB das Kontingent über die Berechtigungen als Teil der gelöschten Benutzer im Hintergrund freigibt, die gelöschten Berechtigungen steht sofort erneut für Ihre Verwendung.  

## <a name="permissions"></a>Berechtigungen
Aus Access Steuerelement-Perspektive Ressourcen wie z. B. Datenbankkonten, Datenbanken, Benutzer und Berechtigungen *administrativen* Ressourcen gelten, da diese Administratorrechte erforderlich sind. Andererseits, sind Ressourcen, einschließlich der Websitesammlungen, Dokumente, Anlagen, gespeicherten Prozeduren, Triggern und UDFs ausgelegte unter einer bestimmten Datenbank und *Anwendungsressourcen*angesehen. Entsprechend der zwei Arten von Ressourcen und die Rollen, die darauf (nämlich Administrator und Benutzer) zugreifen, das Autorisierungsmodell definiert zwei Arten von *Tastenkombinationen*: *Master* und *Ressourcenschlüssel*. Die Taste Master-Shape ist ein Teil des Kontos Datenbank und wird die Entwicklertools (oder Administrator) bereitgestellt wird, die das Datenbankkonto bereitgestellt. Master-Shape Schlüssel hat Administrator-Semantik, da zum Autorisieren des Zugriffs auf administrative und Anwendung Ressourcen verwendet werden können. Ein Ressourcenschlüssel ist dagegen eine detaillierte Zugriffstaste, die Zugriff auf eine *bestimmte* Ressource der Anwendung ermöglicht. Auf diese Weise erfasst es die Beziehung zwischen dem Benutzer einer Datenbank und den Berechtigungen, die der Benutzer für eine bestimmte Ressource (z. B. Auflistung, Dokument, Anlage, gespeicherte Prozedur, Trigger oder UDFs) verfügt.   

Die einzige Möglichkeit, einen Ressource Key zu erhalten, wird durch Erstellen einer Berechtigungsstufe Ressource unter einem bestimmten Benutzer. Beachten Sie, dass zum Erstellen oder Abrufen einer Berechtigungsstufe, eine Master-Shape-Taste in der Kopfzeile Autorisierung vorzulegen. Eine Ressource über die Berechtigung verbindet der Ressource, deren Zugriff und der Benutzer. Nach dem Erstellen einer Ressource über die Berechtigung, muss der Benutzer nur die Taste zugeordneten Ressourcen präsentieren, um auf die entsprechende Ressource zugreifen. Daher kann ein Ressourcenschlüssel als eine logische und kompakten Darstellung der Ressource über die Berechtigung angezeigt werden.  

Wie bei allen anderen Ressourcen Berechtigungen in DocumentDB erstellt werden können, ersetzt, gelöscht, lesen oder einfach mithilfe von REST-APIs oder eine Client-SDKs aufgelistet. DocumentDB enthält immer signifikante Konsistenz zum Lesen oder Abfragen der Metadaten einer Berechtigung. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Arbeiten mit Ressourcen mithilfe von HTTP-Befehlen in [Rest Interaktionen mit DocumentDB Ressourcen](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

