<properties 
    pageTitle="Modellieren von Daten in Azure DocumentDB | Microsoft Azure" 
    description="Lernen Sie Modellieren von Daten für DocumentDB, ein Dokument die NoSQL-Datenbank." 
    keywords="Modellieren von Daten"
    services="documentdb" 
    authors="kiratp" 
    manager="jhubbard" 
    editor="mimig1" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/05/2016" 
    ms.author="kipandya"/>

#<a name="modeling-data-in-documentdb"></a>Modellieren von Daten in DocumentDB#
Während Schema frei-Datenbanken wie Azure-DocumentDB erleichtern sollte wirklich leicht zu Änderungen an Ihr Datenmodell zweifache Sie immer noch einige Zeit an etwa Ihre Daten aufwenden. 

Wie geht Daten gespeichert werden? Wie geht Ihrer Anwendung abrufen und Abfragen von Daten? Ist der Anwendung extra fett lesen oder Schreiben beanspruchen? 

Nach dem Lesen dieses Artikels, werden Sie die folgenden Fragen beantworten:

- Wie sollte ich zu einem Dokument in einer Datenbank Dokument vorstellen?
- Was ist datenmodellierung und warum sollte ich umsteigen? 
- Wie unterscheidet sich Modellierung von Daten in einer Datenbank Dokument in einer relationalen Datenbank?
- Wie werden gehen Sie wie folgt Daten Beziehungen in einer nicht relationalen Datenbank ausgedrückt?
- Wenn betten Daten, und wenn ich Verknüpfen mit Daten?

##<a name="embedding-data"></a>Einbetten von Daten##
Beim Starten von Modellieren von Daten in einem Dokument abgelegt, wie z. B. DocumentDB, versuchen Sie, Ihre-Einheiten als **eigenständige Dokumente** in JSON dargestellt zu behandeln.

Bevor wir in kümmern zu viel weiter lassen Sie uns ein paar Schritte ausführen wieder und sehen Sie sich wie wir etwas in einer relationalen Datenbank, einen Betreff modellieren möglicherweise viele von uns bereits vertraut sind. Im folgende Beispiel wird gezeigt, wie eine Person in einer relationalen Datenbank gespeichert werden kann. 

![Das relationale Modell](./media/documentdb-modeling-data/relational-data-model.png)

Bei der Arbeit mit relationalen Datenbanken haben wir Inhalt dieser beigetragen normalisieren normalisieren, standardisiert werden soll.

Normalisieren von Daten in der Regel umfasst das Aufzeichnen einer Entität, z. B. einer Person, und Sie zu diskrete Datenelementen aufteilen. Im oben genannten Beispiel kann eine Person mehrere Kontakt detaillierten Datensätze als auch mehrere Adressdatensätze haben. Wir gerade einen Schritt weiter gehen und unterteilen Kontaktdetails nach weiteren extrahieren allgemeine Felder wie einen Typ. Für die Adresse gleichen, besitzt jeder Datensatz hier einen Typ wie *Start* oder *Business* 

Die er begleitet lokale beim Normalisieren von Daten zu **vermeiden redundanten Daten zu speichern** , klicken Sie auf jeden Datensatz ist und beziehen sich lieber auf Daten. In diesem Beispiel müssen um eine Person, mit ihren Kontaktinformationen und Adressen, lesen Sie VERKNÜPFUNGEN zu verwenden, um die Daten zur Laufzeit effektiv zu aggregieren.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Aktualisieren einer einzelnen Person mit ihren Kontaktinformationen und Adressen erfordert Schreiboperationen in vielen verschiedenen einzelne Tabellen. 

Jetzt werfen Sie einen Blick auf wie wir dieselben Daten wie eine unabhängige Entität in einer Datenbank Dokument modellieren möchten.
        
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {            
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email: "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ] 
    }

Mit dem oben genannten Ansatz wir nun **denormalisierte** Person aufzeichnen Where haben wir **eingebettete** alle Informationen im Zusammenhang mit dieser Person, wie etwa ihre Kontaktinformationen und Adressen zu einem einzelnen JSON-Dokument.
Darüber hinaus, da wir nicht über ein festes Schema beschränkt sind haben wir die Flexibilität Möglichkeiten wie Kontaktdetails verschiedener Formen vollständig Probleme. 

Abrufen eines Datensatzes abgeschlossen Person aus der Datenbank ist jetzt ein einzelnes Vorgang für eine einzelne Sammlung und für ein einzelnes Dokument lesen. Aktualisieren eines Datensatzes Person mit ihren Kontaktinformationen und Adressen, wird auch ein einzelnes Schreibvorgang für ein einzelnes Dokument.

Denormalisieren von Daten, indem Sie möglicherweise eine Anwendung weniger Abfragen und Updates für häufige Vorgänge abgeschlossen ausgeben müssen. 

###<a name="when-to-embed"></a>Wann einbetten

Verwenden Sie den eingebettete Daten im Allgemeinen Modelle Situationen:

- Es gibt **enthält** Beziehungen zwischen Elementen ein.
- Es gibt **eine zu wenig** Beziehungen zwischen Elementen ein.
- Eingebettete Daten ist vorhanden, die **selten ändert**.
- Es ist eingebettet Datenbereiche wird nicht **ohne Grenze**vergrößert werden sollen.
- Es gibt eingebettete Daten, die **ganzzahligen** mit Daten in einem Dokument ist.

> [AZURE.NOTE] Denormalisierte Datenmodelle bieten in der Regel eine bessere Leistung der **Lesen** .

###<a name="when-not-to-embed"></a>Wann nicht einbetten

Während der Faustregel in einer Datenbank Dokument ist alles Denormalisieren und alle Daten in ein einzelnes Dokument einbetten, kann dies zu einigen Situationen führen, die vermieden werden sollte.

Nehmen Sie diese JSON-Codeausschnitt in Anspruch.

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

Möglicherweise ist wie der eine Beitrag Entität mit eingebetteten Kommentaren wurden Wenn wir eine typische Blog oder CMS, System modeling aussehen würde. Das Problem mit den in diesem Beispiel ist, dass das Kommentare Array **ungebundener**, d. h., es gibt keine (praktische) hinsichtlich der Anzahl von Kommentaren, die eine einzelne Beitrag haben kann. Dies wird ein Problem werden, wenn die Größe des Dokuments erheblich wächst konnte.

> [AZURE.TIP] Dokumente im DocumentDB besitzen eine maximale Größe. Weitere Informationen dazu finden Sie in [DocumentDB Grenzwerte](documentdb-limits.md)auf.

Wenn die Größe des Dokuments die Möglichkeit zum Übertragen von Daten über das Kabel als auch lesen und aktualisieren das Dokument, klicken Sie unter Skalieren wächst beeinträchtigt werden.

In diesem Fall wäre es besser, erwägen Sie das folgende Modell.
        
    Post document:
    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

Dieses Modell verfügt über die letzten drei auf dem Beitrag selbst, also ein Array mit einer festen eingebettete Kommentaren gebunden diesmal. Die anderen Kommentare werden Stapeln von 100 Kommentare gruppiert und in separate Dokumente gespeichert. Die Größe des Stapels wurde als 100 ausgewählt wurde, da unsere fiktive Anwendung der Benutzer jeweils 100 Kommentare zu laden kann.  

Eine andere Stelle, an der das Einbetten von Daten nicht wohl ist Fall ist die eingebetteten Daten über Dokumente häufig verwendet werden und werden häufig geändert werden. 

Nehmen Sie diese JSON-Codeausschnitt in Anspruch.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

Einer Person vorhandenen Portfolio darstellen. Wir haben die vorhandenen Informationen in jedem Dokument Portfolio einbetten. In einer Umgebung, in dem verwandte Daten häufig geändert hat, soll wie eine Aktie trading Anwendung, die Einbetten von die sich häufig ändernden bedeutet, dass Sie jedes Mal, wenn eine Aktie gehandelt wird ständig jedes Dokument Portfolio aktualisieren.

Vordefinierte *Zaza* viele hundert Mal an einem Tag gehandelt werden kann, und Tausende von Benutzern möglicherweise *Zaza* auf deren Portfolio. Mit einem Datenmodell wie genannten wir hätten Tausende von Dokumenten Portfolio oft aktualisieren wird nicht jeden Tag zu einem System führenden, die nicht sehr gut skalieren. 

##<a name="a-idreferareferencing-data"></a><a id="Refer"></a>Bezüge auf Daten##

Ja, Einbetten von Daten für vielen Fällen gut funktioniert, aber es ist klar, dass Szenarien vorhanden sind, wenn Ihre Daten Denormalisieren weitere Probleme unterbindet lohnt. Daher was tun wir jetzt? 

Relationale Datenbanken sind nicht die einzige Stelle, wo Sie Beziehungen zwischen Elementen erstellen können. In einer Datenbank Dokument haben Sie Informationen in einem Dokument an, die tatsächlich Daten in anderen Dokumenten beziehen. Nun bin ich nicht auch eine Minute Allgemein, dass wir erstellen Systeme, die in DocumentDB einer relationalen Datenbank oder eine beliebige andere Dokument Datenbank besser geeignet sein sollte, aber einfache Beziehungen sind in Ordnung und können sehr hilfreich sein. 

In der nachstehenden JSON wir haben entschieden, die im Beispiel eines vorhandenen Portfolios aus einer früheren Version verwenden, jedoch diesmal des vorhandenen Elements auf der Portfolio nicht einbetten, sondern gemeint. Auf diese Weise bei vorhandenen Änderung des Elements häufig im Verlauf des Tages das Dokument nur, das aktualisiert werden soll, ist der einzelnen vorhandenen Dokument. 

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }
    
    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }
    

Eine sofortige Nachteil dabei ist jedoch ist eine Anwendung erforderlichen Informationen für jede Aktie angezeigt, die beim Anzeigen einer Person Portfolio gehalten wird; In diesem Fall müssen Sie möchten mehrere Schleifen an die Datenbank, laden die Informationen für jedes vorhandenen Dokument vornehmen. Hier haben wir eine Entscheidung der Effizienz von Schreibvorgängen, die häufig im Verlauf des Tages erfolgen, aber wiederum auf gelesen Vorgänge, die potenziell weniger Einfluss auf die Leistung von diesem System gefährdet vorgenommen.

> [AZURE.NOTE] Normalisiert Datenmodelle **können weitere Schleifen erforderlich** , auf dem Server.

### <a name="what-about-foreign-keys"></a>Wissenswertes zu Fremdschlüssel
Da es gibt es zurzeit keine Konzepts einer Einschränkung, Fremdschlüssel oder sonstige alle zwischen Dokument Beziehungen, die Sie in Dokumenten enthalten sind im Grunde "schwachen Links" und werden durch die Datenbank selbst nicht überprüft werden. Wenn Sie möchten sicherstellen, dass die Daten, die ein Dokument verweisen wird, auf tatsächlich vorhanden ist, müssen Sie dies in Ihrer Anwendung oder bestimmte serverseitige Trigger oder gespeicherte Prozeduren auf DocumentDB vornehmen.

###<a name="when-to-reference"></a>Wann verweisen
Verwenden Sie im Allgemeinen standardisierte Daten wann Modelle:

- **1: n -** Beziehung darstellt.
- **M: n -** Beziehungen der darstellt.
- Verknüpfte Daten **häufig geändert wird**.
- Referenzierte Daten könnte **ungebundener**.

> [AZURE.NOTE] In der Regel normalisieren bietet eine bessere Leistung der **Schreiben** .

###<a name="where-do-i-put-the-relationship"></a>Wo setzen Sie die Beziehung
Das Wachstum der Beziehung hilft bestimmen, welche Dokument, dem Sie den Bezug zu speichern.

Wenn Sie die folgenden JSON betrachten wir die Herausgeber und Bücher Modelle.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "DocumentDB 101" }
    {"id": "2", "name": "DocumentDB for RDBMS Users" }
    {"id": "3", "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure DocumentDB" }
    ...
    {"id": "1000", "name": "Deep Dive in to DocumentDB" }

Wenn die Anzahl der Bücher pro Publisher mit eingeschränkter geometrischen klein ist, sein klicken Sie dann speichern den Bezug Adressbuch innerhalb des Dokuments Publisher hilfreich. Jedoch ist die Anzahl der Bücher pro Publisher ungebundener würde dann dieses Datenmodell zu veränderbare, wachsende Arrays, wie im obigen Beispiel Publisher Dokument führen. 

Wechsel um ein bisschen Elemente ergibt eines Modells, die steht noch immer auf die gleichen Daten aber jetzt vermieden werden diese großen veränderbaren Sammlungen.

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }
    
    Book documents: 
    {"id": "1","name": "DocumentDB 101", "pub-id": "mspress"}
    {"id": "2","name": "DocumentDB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure DocumentDB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive in to DocumentDB", "pub-id": "mspress"}

Im Beispiel oben haben wir die Sammlung ungebundene ablegen, auf das Publisher-Dokument. Stattdessen einfach müssen ein Verweis auf den Herausgeber für jedes Dokument Adressbuch.

###<a name="how-do-i-model-manymany-relationships"></a>Wie modellieren ich die m: n-Beziehung?
In einer relationalen Datenbank werden *m: n* -Beziehungen häufig mit Join-Tabellen, erstellt, die nur Datensätze aus anderen Tabellen miteinander zu verbinden. 

![Verknüpfen von Tabellen](./media/documentdb-modeling-data/join-table.png)

Sie können möglicherweise dazu verleitet repliziert das gleiche mithilfe von Dokumenten und erstellen ein Datenmodell, die etwa wie folgt aussieht.

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }
    
    Book documents:
    {"id": "b1", "name": "DocumentDB 101" }
    {"id": "b2", "name": "DocumentDB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure DocumentDB" }
    {"id": "b5", "name": "Deep Dive in to DocumentDB" }
    
    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Dies funktioniert. Jedoch würde entweder einen Autor mit ihren Büchern geladen oder einem Buch geladen, mit deren Autor immer mindestens zwei weitere Abfragen an die Datenbank erfordern. Eine Abfrage, um das Verknüpfen Dokument, und klicken Sie dann eine andere Abfrage das eigentliche Dokument zu verknüpfenden abgerufen werden sollen. 

Wenn sämtliche dieser Verknüpfungstabelle durchführt wird durch das Kleben zusammen zwei Datenelementen, legen Sie dann nicht warum sie vollständig?
Beachten Sie Folgendes aus.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}
    
    Book documents: 
    {"id": "b1", "name": "DocumentDB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "DocumentDB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure DocumentDB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in to DocumentDB", "authors": ["a2"]}

Jetzt, wenn ich einen Autor vorkam, ich sofort wissen, welche ihnen verfassten Bücher und umgekehrt, wenn ich ein Adressbuch Dokument geladen hätte ich würde die Ids eines kennen die wer. Dies speichert die temporären Abfrage anhand der degressiven zu Verknüpfung Tabelle die Nummer des Servers den Schleifen, die eine Anwendung zum treffen hat. 

##<a name="a-idwrapupahybrid-data-models"></a><a id="WrapUp"></a>Hybrid-Datenmodellen##
Wir haben nun vergeblich einbetten (oder Denormalisieren) und verweisende (oder Normalisieren) Daten für jedes deren Vorzüge und jeder Kompromisse haben, wie wir gesehen haben. 

Es wird nicht immer entweder als haben oder Dinge mischen etwas von erschrockene werden nicht. 

Basierend auf bestimmten Verwendungsmuster und Auslastung, die möglicherweise Fälle, in denen eingebettet mischen, Ihrer Anwendungs referenzierte Daten ist sinnvoll, und konnte den Lead einfacher Anwendung Logik mit weniger Server Schleifen beibehalten, jedoch eine gute Ebene der Leistung.

Berücksichtigen Sie die folgende JSON. 

    Author documents: 
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",     
        "countOfBooks": 3,
        "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "http://....png"}
            {"profile": "http://....png"}
            {"large": "http://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "http://....png"}
        ]
    }
    
    Book documents:
    {
        "id": "b1",
        "name": "DocumentDB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "http://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "DocumentDB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
        ]
    }

Hier haben wir (hauptsächlich) gefolgt eingebettete Modell, in dem Daten aus anderen Personen werden im Dokument auf oberster Ebene eingebettet ist, aber andere Daten verwiesen werden. 

Wenn Sie sich das Dokument Adressbuch anschauen, sehen wir ein paar interessante Felder aus, wenn wir uns die Matrix von Autoren ansehen. Es ist ein *ID-* Feld an der das Feld, das wir verwenden angibt, um wieder zu einem Autorendokument, standard Übungsbeispiel in einem standardisierten Modell, schlagen Sie allerdings dann wir auch *Namen* und *ThumbnailUrl*. Wir nur *ID* hängen und links von der Anwendung zu erhalten, die sie benötigt zusätzliche Informationen aus dem jeweiligen Autorendokument mithilfe des "Links" haben könnten, aber daran, dass unsere Anwendung mit jeder Adressbuch angezeigt, den Namen des Autors und einer Miniaturansicht angezeigt können wir eine Schleife auf dem Server pro Adressbuch in einer Liste mit Denormalisieren **einiger** Daten aus der Autor speichern.

Sicher, wenn der Name des Autors geändert, oder sie ihre Fotos aktualisieren wir haben möchten, um eine Aktualisierung zu wechseln wollten jedes Buch sie jemals veröffentlicht für unsere Anwendung, basierend auf der Annahme, dass Autoren deren Namen sehr häufig ändern, nicht Dies ist jedoch eine Entscheidung zulässigen Entwurf.  

Im Beispiel sind **vordefinierte berechnet Aggregate** Werte für einen Vorgang gelesen teure Verarbeitung zu speichern. Im Beispiel einige der Daten in das Autorendokument eingebettet ist Daten, die zur Laufzeit berechnet werden. Jedes Mal, wenn ein neues Buch veröffentlicht wird, ist ein Adressbuch-Dokument **und** erstellt, das Feld "CountOfBooks", um einen berechneten Wert festgelegt ist, basierend auf die Anzahl der Adressbuch-Dokumente, die für einen bestimmten Autor vorhanden sind. Diese Optimierung wäre in gelesen beanspruchen Systeme gut, können wir haben Berechnungen auf schreibt ausgeführt, um liest optimieren.

Die Möglichkeit, eines Modells mit vorab berechneten Feldern wird ermöglicht, da DocumentDB **Dokument mit mehreren Transaktionen**unterstützt. Führen Sie Transaktionen über Dokumente und daher Vertreter Entwurfs entscheiden, wie etwa "Einbetten immer alles", da diese Einschränkung nicht viele NoSQL Stores möglich. Mit DocumentDB können Sie serverseitige Trigger oder gespeicherten Prozeduren, die Bücher einfügen und Aktualisieren von Autoren alle innerhalb einer ACID-Transaktion. Nachdem Sie diese nicht wünschen **haben** zum Einbetten alles in einem Dokument nur um sicherzustellen, dass Ihre Daten konsistent bleiben.

##<a name="a-namenextstepsanext-steps"></a><a name="NextSteps"></a>Nächste Schritte

Die wichtigsten lassen in diesem Artikel ist zu verstehen, dass die Daten in einer Welt Schema frei modeling als je zuvor genauso wichtig ist. 

Wie gibt es keine einzelne Möglichkeit einen Teil der Daten auf einem Bildschirm dargestellt, gibt es Möglichkeit keine einzelner Modellieren von Daten. Sie müssen zu verstehen Ihrer Anwendung und wie sie zurückgegeben werden, nutzen und die Daten zu verarbeiten. Klicken Sie dann können durch Anwenden einer einige Richtlinien für den hier gezeigten Sie festlegen zum Erstellen eines Modells, das die Anforderungen Ihrer Anwendung sofortigen Adressen. Wenn Ihre Programme ändern müssen, können Sie die Flexibilität ein Schema frei Datenbank zu nutzen, ändern und des Datenmodells problemlos weiterentwickelt, nutzen. 

Um weitere Informationen zur Azure DocumentDB des Diensts [Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/) Seite verweisen. 

Weitere Informationen zu Videogeräten Indizes in Azure DocumentDB finden Sie im Artikel auf [Indizierung Richtlinien](documentdb-indexing-policies.md).

Zu verstehen, wie in Shard von Daten aus mehreren partitionsübergreifend beziehen sich auf [Partitionierung Daten in DocumentDB](documentdb-partition-data.md). 

Und schließlich um Unterstützung für die Modellierung von Daten und Sharding für Applikationen mit mehreren Mandanten, wenden Sie sich an [eine Anwendung mit mehreren Mandanten mit Azure DocumentDB skalieren](http://blogs.msdn.com/b/documentdb/archive/2014/12/03/scaling-a-multi-tenant-application-with-azure-documentdb.aspx).
 
