<properties
    pageTitle="Clientseitige Verschlüsselung mit Python für Microsoft Azure-Speicher | Microsoft Azure"
    description="Der Azure-Speicher Client-Bibliothek für Python unterstützt Verschlüsselung clientseitige für maximale Sicherheit für Ihre Applikationen Azure-Speicher."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Clientseitige Verschlüsselung mit Python für Microsoft Azure-Speicher

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>(Übersicht)

Der [Azure-Speicher-Client-Bibliothek für Python](https://pypi.python.org/pypi/azure-storage) unterstützt Verschlüsselung von Daten in Clientanwendungen vor dem Hochladen in Azure-Speicher und Entschlüsseln von Daten beim Herunterladen auf den Client.

>[AZURE.NOTE] Die Azure-Speicher Python-Bibliothek ist in der Vorschau.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Verschlüsseln und Entschlüsseln über den Umschlag-Methode
Führen Sie die Prozesse Verschlüsselung und Entschlüsseln die Umschlag Methode aus.

### <a name="encryption-via-the-envelope-technique"></a>Verschlüsselung über den Umschlag-Methode
Verschlüsselung über den Umschlag Methode funktioniert wie folgt:

1.  Die Azure-Speicher-Client-Bibliothek generiert einen Schlüssel für Verschlüsselung von Inhalten (CEK), der einen one einmalig verwenden symmetrischen Schlüssel ist.

2.  Benutzerdaten ist mit dieser CEK verschlüsselt.

3.  Die CEK wird dann umschlossen () mit den wichtigsten Schlüssel verschlüsselt (KEK). Die KEK kann wird durch eine Schlüssel-ID identifiziert und eine asymmetrische Key Paar oder einen symmetrischen Schlüssel, die lokal verwaltet wird.
Die Speicher-Client-Bibliothek selbst weist nie Zugriff auf KEK. Die Bibliothek Ruft den wichtigsten Umbruch Algorithmus, der von der KEK bereitgestellt wird. Benutzerdefinierte Anbieter für Key Umbruch/Entpacken verwenden, falls gewünscht, können Benutzer auswählen.

4.  Die verschlüsselten Daten werden dann in der Azure-Speicherdienst hochgeladen werden. Der Umgebrochene Schlüssel sowie einige weitere Verschlüsselung Metadaten wird als Metadaten (auf ein Blob) gespeichert oder interpoliert mit der verschlüsselten Daten (Warteschlangennachrichten und Tabelle Einheiten).

### <a name="decryption-via-the-envelope-technique"></a>Entschlüsseln über den Umschlag-Methode
Entschlüsseln über den Umschlag Methode funktioniert wie folgt:

1.  Die Client-Bibliothek wird davon ausgegangen, dass der Benutzer die Taste Verschlüsselungsschlüssels (KEK) lokal verwaltet wird. Der Benutzer muss nicht den bestimmten Schlüssel kennen, der für die Verschlüsselung verwendet wurde. Stattdessen kann einer Key Auflösung, der anderen wichtigen Bezeichner in Tasten aufgelöst wird, einrichten und verwendet werden.

2.  Die Clientbibliothek-downloads für die verschlüsselten Daten zusammen mit Verschlüsselung Material, die auf den Dienst gespeichert ist.

3.  Umgebrochene Inhalt Verschlüsselungsschlüssels (CEK) ist, und klicken Sie dann mit der Taste Verschlüsselung allein stehenden (entschlüsselt) wichtiger (KEK). Hier wird erneut, die Client-Bibliothek nicht auf KEK zugreifen. Ruft Sie einfach Entpacken Algorithmus der benutzerdefinierte Anbieter.

4.  Der Inhalt Verschlüsselungsschlüssels (CEK) wird dann zum Entschlüsseln der verschlüsselten Benutzerdaten.

## <a name="encryption-mechanism"></a>Verfahren für Verschlüsselung  
Die Speicher-Client-Bibliothek wird [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) um Benutzerdaten verschlüsseln. Insbesondere [Verschlüsselung Block verketten (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) Modus mit AES. Jede-service funktioniert etwas anders ausgeführt, sodass jeder von ihnen hier besprochen wird.

### <a name="blobs"></a>BLOBs
Die Client-Bibliothek unterstützt die Verschlüsselung nur gesamte Blobs aktuell. Insbesondere wird Verschlüsselung unterstützt, wenn Benutzer die **Erstellen**verwenden * Methoden. Für Downloads, beide abgeschlossen und Bereich Downloads werden unterstützt, und Parallelisierung Upload und Download zur Verfügung.

Bei der Verschlüsselung die Clientbibliothek generiert eine zufällige Initialisierung Vektor (IV) von 16 Bytes, zusammen mit einem zufällige Inhalt Schlüssel (CEK) 32 Bytes und Umschlag Verschlüsselung BLOB-Daten mithilfe dieser Informationen ausführen. Der Umgebrochene CEK und einige zusätzliche Verschlüsselung Metadaten sind wie Metadaten zusammen mit der verschlüsselten Blob-Dienst BLOB-dann gespeichert.

>[AZURE.WARNING] Wenn Sie bearbeiten oder eigene Metadaten für das Blob hochladen, müssen Sie sicherstellen, dass diese Metadaten gewahrt wird. Wenn Sie neue Metadaten ohne diese Metadaten hochladen, der Umgebrochene CEK, IV und sonstige Metadaten verloren und der BLOB-Inhalt werden nie einer abrufbaren erneut.

Herunterladen eines verschlüsselten BLOBs umfasst den Inhalt des gesamten Blob mithilfe der **erste**abrufen * Komfort Methoden. Der Umgebrochene CEK ausgepackt und zusammen mit der IV (in diesem Fall als Blob-Metadaten gespeichert) verwendet wird, um die entschlüsselten Daten an den Benutzer zurückzugeben.

Herunterladen eines beliebigen Textbereichs (**Abrufen*** Methoden mit Bereich Parametern übergebene) im verschlüsselten Blob umfasst Einstellen des Bereichs von Benutzer bereitgestellt werden, um eine kleine zusätzliche Datenmenge abrufen, die den angeforderten Bereich erfolgreich entschlüsseln verwendet werden können.

Blockieren Blobs und Seitenblobs können nur verschlüsselt/entschlüsselt dieses Schema sein. Es wird derzeit nicht unterstützt für Blobs Datenbankschutz anfügen.

### <a name="queues"></a>Warteschlangen
Da Warteschlangennachrichten von einem beliebigen Format werden können, definiert die Clientbibliothek ein benutzerdefiniertes Format, das Initialization Vektor (IV) und der verschlüsselten Inhalt Verschlüsselungsschlüssels (CEK) in den Nachrichtentext enthält.

Bei der Verschlüsselung die Clientbibliothek generiert einen zufälligen IV von 16 Bytes sowie eine zufällige CEK 32 Bytes und führt Umschlag Verschlüsselung Warteschlange Nachricht Text anhand dieser Informationen. Der Umgebrochene CEK und einige zusätzliche Verschlüsselung Metadaten werden dann an die Warteschlange verschlüsselte Nachricht hinzugefügt. Klicken Sie auf den Dienst wird diese geänderte Nachricht (siehe unten) gespeichert.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Der Umgebrochene Schlüssel ist während entschlüsseln aus der Warteschlange Nachricht extrahiert und unverpacktem. Die IV ist auch aus der Warteschlange Nachricht extrahiert und zusammen mit dem allein stehenden Schlüssel verwendet werden, um die Daten der Warteschlange Nachricht entschlüsseln. Beachten Sie, dass die Verschlüsselung Metadaten gedrückt, während sie das 64 KB Limit für eine Nachricht Warteschlange berücksichtigt, der Einfluss verwaltbare sollten kleine (unter 500 Byte) ist.

### <a name="tables"></a>Tabellen
Die Client-Bibliothek unterstützt die Verschlüsselung der Elementeigenschaften für einfügen und Ersetzungsvorgänge.

>[AZURE.NOTE] Zusammenführen wird derzeit nicht unterstützt. Da eine Teilmenge der Eigenschaften möglicherweise zuvor mit einem anderen Schlüssel verschlüsselt sind, führt einfach die neuen Eigenschaften zusammenführen und aktualisieren die Metadaten Datenverlust. Zusammenführen von entweder erfordert zusätzliche Service-Anrufe, die bereits vorhandene Entität vom Dienst zu lesen, oder verwenden einen neuen Product Key pro Eigenschaft, beide über eignen sich nicht für die Leistung zu verbessern.

Tabelle Daten Verschlüsselung funktioniert wie folgt aus:

1.  Benutzer geben Sie die Eigenschaften verschlüsselt werden.

2.  Die Client-Bibliothek generiert eine zufällige Initialisierung Vektor (IV) von 16 Bytes zusammen mit einem zufällige Inhalt Schlüssel (CEK) 32 Bytes für jede Person, und führt Umschlag Verschlüsselung auf die einzelnen Eigenschaften verschlüsselt werden, indem Sie einen neuen IV pro Eigenschaft abgeleitet. Die verschlüsselte Eigenschaft wird als binäre Daten gespeichert werden.

3.  Der Umgebrochene CEK und einige zusätzliche Verschlüsselung Metadaten werden als zwei zusätzliche reservierte Eigenschaften dann gespeichert. Die erste reserviert Eigenschaft (\_ClientEncryptionMetadata1) ist eine Zeichenfolgeneigenschaft, die Informationen zu IV, Version und Umgebrochene Schlüssel enthält. Die zweite reserviert Eigenschaft (\_ClientEncryptionMetadata2) ist eine binäre Eigenschaft, die Informationen zu den Eigenschaften enthält, die verschlüsselt sind. Die Informationen in dieser Eigenschaft zweiten (\_ClientEncryptionMetadata2) selbst ist verschlüsselt.

4.  Aufgrund von diese zusätzlichen reservierten Eigenschaften für die Verschlüsselung erforderlich ist können Benutzer jetzt nur 250 benutzerdefinierte Eigenschaften anstelle von 252 verfügen. Die Gesamtgröße der Entität muss kleiner als 1 MB.

    Beachten Sie, dass nur die Zeichenfolgeneigenschaften verschlüsselt werden können. Wenn Sie andere Arten von Eigenschaften werden verschlüsselt werden, müssen sie in Zeichenfolgen konvertiert werden. Die verschlüsselten Zeichenfolgen werden vom Dienst als binäre Eigenschaften gespeichert, und sie wieder in Zeichenfolgen konvertiert (unformatierte Zeichenfolgen, nicht EntityProperties mit Typ EdmType.STRING) nach entschlüsseln.

    Für Tabellen, sowie die Verschlüsselungsrichtlinie müssen Benutzer die Eigenschaften verschlüsselt werden angeben. Dies ist möglich, indem Sie entweder diese Eigenschaften in TableEntity Objekte, dessen Typ auf EdmType.STRING speichern und festlegen, um WAHR oder Festlegen der Encryption_resolver_function auf das Objekt Tableservice verschlüsseln. Eine Verschlüsselung Auflösung ist eine Funktion, die eine Partitionsschlüssel, Zeilenschlüssel und Eigenschaftsname verwendet und gibt einen Boolean zurück, der angibt, ob die Eigenschaft verschlüsselt werden soll. Bei der Verschlüsselung wird die Clientbibliothek diese Informationen verwendet, um zu entscheiden, ob eine Eigenschaft beim Schreiben zur Übertragung verschlüsselt werden soll. Die Stellvertretung sieht auch die Möglichkeit der Logik vor wie Eigenschaften verschlüsselt werden. (Beispielsweise ist X, dann verschlüsseln Eigenschaft A; andernfalls verschlüsseln Eigenschaften A und b) Beachten Sie, dass es nicht erforderlich, diese Informationen beim Lesen oder Abfragen von Personen zu machen.

### <a name="batch-operations"></a>Stapel Vorgänge
Verschlüsselungsrichtlinie gilt für alle Zeilen in den Stapel. Die Client-Bibliothek wird eine neue zufällige IV und zufällige CEK pro Zeile intern im Stapel generieren. Benutzer können auch andere Eigenschaften für jeden Vorgang im Stapel verschlüsseln dieses Verhalten in die Verschlüsselung Auflösung definieren auswählen.
Wenn ein Stapel durch die Methode Tableservice batch() als Kontextmanager erstellt wird, wird der Tableservice des Verschlüsselungsrichtlinie automatisch auf den Stapel angewendet werden. Ein Stapel explizit durch Aufrufen des Konstruktors erstellt wird, muss die Verschlüsselungsrichtlinie als Parameter übergeben und Links für die Gültigkeitsdauer des Stapels unverändert.
Beachten Sie, dass Personen verschlüsselt sind, wie sie in den Stapel mit den Stapel der Verschlüsselungsrichtlinie (Elemente sind nicht zum Zeitpunkt der Commit des Stapels mithilfe der Tableservice des Verschlüsselungsrichtlinie verschlüsselt) eingefügt werden.

### <a name="queries"></a>Abfragen
Um Abfrage durchzuführen, müssen Sie einen Key Auflösung angeben, die alle Schlüssel im Resultset auflösen kann. Wenn eine Entität im Abfrageergebnis enthaltenen mit einem Anbieter aufgelöst werden kann, wird die Clientbibliothek ein Fehler ausgelöst. Für jede Abfrage, die Seite Projektionen Server ausführt, wird die Clientbibliothek fügen Sie die Inhalte Verschlüsselung Metadateneigenschaften (\_ClientEncryptionMetadata1 und \_ClientEncryptionMetadata2) standardmäßig auf die ausgewählten Spalten.

>[AZURE.IMPORTANT] Achten Sie auf diese wichtigen Punkte, wenn die clientseitige Verschlüsselung verwendet:
>
>- Verwenden Sie beim Lesen aus oder Schreiben in einer verschlüsselten Blob gesamte Blob Upload Befehle und Bereich/ganze Blob Download Befehle. Vermeiden Sie das Schreiben in einer verschlüsselten Blob mit Protokoll Operationen wie blockieren setzen, Blockliste setzen, Schreiben von Seiten oder Löschen von Seiten; Andernfalls können Sie beschädigter verschlüsselte Blob und nicht gelesen werden.
>
>- Für Tabellen vorhanden ist eine ähnliche Einschränkung aus. Achten Sie darauf, dass keine verschlüsselte Eigenschaften aktualisieren, ohne die Verschlüsselung Metadaten zu aktualisieren.
>
>- Wenn Sie auf die verschlüsselte Blob Metadaten festgelegt, überschreiben Sie möglicherweise die Verschlüsselung-bezogene Metadaten für Verschlüsselung erforderlich ist, da das Festlegen von Metadaten nicht erweiterbar ist. Dies gilt auch für Momentaufnahmen. Vermeiden Sie es, Metadaten beim Erstellen einer Momentaufnahme einer verschlüsselten Blob. Wenn Metadaten festgelegt werden muss, achten Sie darauf, dass Sie zum Aufrufen der Methode **Get_blob_metadata** zuerst, um die aktuellen Verschlüsselung Metadaten zu erhalten, und vermeiden Sie gleichzeitige schreibt ein, während Metadaten festgelegt wird.
>
>- Aktivieren Sie die Kennzeichnung **Require_encryption** auf das Dienstobjekt für Benutzer, die nur in Verbindung mit verschlüsselten Daten arbeiten soll. Weitere Informationen finden Sie unter.

Die Speicher-Client-Bibliothek erwartet die bereitgestellten KEK und Key Auflösung, die folgende Schnittstelle implementiert wird. [Azure Schlüssel Tresor](https://azure.microsoft.com/services/key-vault/) Unterstützung für Python KEK Management steht noch aus und wird in dieser Bibliothek nach Beendigung integriert werden.


## <a name="client-api--interface"></a>Client-API / Schnittstelle
Nachdem ein Speicher-Service-Objekt (d. h. Blockblobservice) erstellt wurde, der Benutzer kann Werte zuweisen die Felder, die eine Verschlüsselungsrichtlinie bilden: Key_encryption_key, Key_resolver_function, und Require_encryption. Benutzer können nur KEK, bieten nur eine Auflösung oder beides. Key_encryption_key ist die grundlegende Art des Schlüssels, die mit einer Key-ID gekennzeichnet wird und die Logik für den Textumbruch/Entpacken bereitstellt. Key_resolver_function dient zum Auflösen von einer Taste während des Prozesses entschlüsseln. Es gibt eine gültige KEK eine Schlüssel-ID angegeben. Dies bietet Benutzern die Möglichkeit, zwischen mehreren Tasten auszuwählen, die an mehreren Speicherorten verwaltet werden.

Die KEK muss die folgenden Methoden, um Daten zu verschlüsseln implementieren:
- wrap_key(cek): umgebrochen wird der angegebene CEK (Byte) mit einem Algorithmus der Auswahl des Benutzers. Der Umgebrochene Schlüssel zurückgegeben.
- get_key_wrap_algorithm(): Gibt den Verschlüsselungsalgorithmus an Tasten umbrechen.
- get_kid(): Gibt die Zeichenfolge Key-Id für diese KEK.
Die KEK muss die folgenden Methoden, um Daten zu entschlüsseln implementieren:
- Unwrap_key (Cek, Algorithmus): Gibt die allein stehenden Form von den angegebenen CEK mit dem Algorithmus Zeichenfolge angegeben.
- get_kid(): Gibt eine Zeichenfolge Key-Id für diese KEK.

Die wichtige Auflösung muss mindestens eine Methode implementieren, die ausgehend von einer Schlüssel-Id, die entsprechenden KEK Implementieren der oben genannten Schnittstelle zurückgibt. Nur diese Methode ist, die Eigenschaft Key_resolver_function auf das Dienstobjekt zugewiesen werden soll.

- Für die Verschlüsselung der Schlüssel wird immer verwendet, sowie das Fehlen eines Schlüssels führt zu einem Fehler Fehlern.
- Zum Entschlüsseln:
    - Die wichtige Auflösung wird aufgerufen, wenn den Schlüssel abgerufen angegeben. Die Auflösung angegeben, jedoch verfügt nicht über eine Zuordnung für den Key Bezeichner enthält, wird ein Fehler ausgelöst.
    - Auflösung nicht angegeben, jedoch ein Schlüssel angegeben ist, wird der Schlüssel verwendet, seinen Bezeichner entspricht den erforderlichen Key Bezeichner enthält. Wenn der Bezeichner nicht übereinstimmt, wird ein Fehler ausgelöst.

      In den Beispielen Verschlüsselung in azure.storage.samples <fix URL>führen Sie eine ausführlichere End-to-End-Szenario für Blobs, Queues und Tabellen vor.
        Beispiel-Implementierungen KEK und Key Auflösung werden jeweils in den Beispieldateien als KeyWrapper und KeyResolver bereitgestellt.

### <a name="requireencryption-mode"></a>RequireEncryption Modus
Benutzer können optional einen Modus des Vorgangs aktivieren, in dem alle Uploads und Downloads verschlüsselt werden müssen. In diesem Modus tritt Versuche, Daten keine Verschlüsselungsrichtlinie hochladen oder Herunterladen von Daten, die nicht auf den Dienst verschlüsselt sind auf dem Client. Die Kennzeichnung **Require_encryption** auf das Dienstobjekt steuert dieses Verhalten.

### <a name="blob-service-encryption"></a>BLOB-Service-Verschlüsselung
Legen Sie die Verschlüsselung Felder für die Richtlinie auf das Objekt Blockblobservice. Allen weiteren Aufgaben werden von der Clientbibliothek intern behandelt werden.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Die Verschlüsselung der service
Legen Sie die Verschlüsselung Felder für die Richtlinie auf das Objekt Queueservice. Allen weiteren Aufgaben werden von der Clientbibliothek intern behandelt werden.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Tabelle Service-Verschlüsselung
Zusätzlich zum Erstellen einer Verschlüsselungsrichtlinie und diese Anforderung Optionen festlegen, müssen Sie angeben einer **Encryption_resolver_function** auf das **Tableservice**oder das Attribut verschlüsseln auf die EntityProperty festlegen.

### <a name="using-the-resolver"></a>Verwenden die Auflösung

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Verwenden von Attributen
Wie zuvor erwähnt, möglicherweise eine Eigenschaft für die Verschlüsselung gekennzeichnet werden, indem Sie es in einem Objekt EntityProperty speichern und das Feld verschlüsseln festlegen.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Verschlüsselung und Leistung
Beachten Sie, dass Ihre Daten ergibt zusätzliche Leistung Verwaltungsaufwand Speicher Datenbankschutz werden soll. Inhalt Schlüssel und IV generiert werden muss, muss der Inhalt selbst verschlüsselt werden und zusätzliche Metadaten formatiert und hochgeladen werden muss. Dieser Aufwand variieren je nach der Menge der verschlüsselten Daten. Es empfiehlt sich, dass Kunden immer deren Anwendung für Leistung bei der Entwicklung testen.

## <a name="next-steps"></a>Nächste Schritte

- Herunterladen der [Azure-Speicher Clientbibliothek für PyPi Java-Paket](https://pypi.python.org/pypi/azure-storage)
- Herunterladen der [Azure-Speicher-Client-Bibliothek für Python Quellcode aus GitHub](https://github.com/Azure/azure-storage-python)
