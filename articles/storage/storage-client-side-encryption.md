<properties
    pageTitle="Clientseitige Verschlüsselung mit .NET für Microsoft Azure-Speicher | Microsoft Azure"
    description="Der Azure-Speicher Client-Bibliothek für .NET unterstützt clientseitige Verschlüsselung und Integration in Azure-Taste Tresor für maximale Sicherheit für Ihre Applikationen Azure-Speicher."
    services="storage"
    documentationCenter=".net"
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


# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Clientseitige Verschlüsselung und Azure Key Tresor für Microsoft Azure-Speicher

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>(Übersicht)

Der [Azure-Speicher-Client-Bibliothek für .NET Nuget-Paket](https://www.nuget.org/packages/WindowsAzure.Storage) unterstützt Verschlüsselung von Daten in Clientanwendungen vor dem Hochladen in Azure-Speicher und Entschlüsseln von Daten beim Herunterladen auf den Client. Die Bibliothek unterstützt Integration mit [Azure Schlüssel Tresor](https://azure.microsoft.com/services/key-vault/) auch für Speicher-Konto Key-Verwaltung.

Ein schrittweises Lernprogramm, das führt Sie durch das Verfahren Datenbankschutz Blobs clientseitige Verschlüsselung und Azure-Taste Tresor verwenden, finden Sie unter [Verschlüsseln und Entschlüsseln Blobs in Azure Schlüssel Tresor mit Microsoft Azure-Speicher](storage-encrypt-decrypt-blobs-key-vault.md).

Clientseitige Verschlüsselung mit Java finden Sie unter [Clientseitige Verschlüsselung mit Java für Microsoft Azure-Speicher](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Verschlüsseln und Entschlüsseln über den Umschlag-Methode

Führen Sie die Prozesse Verschlüsselung und Entschlüsseln die Umschlag Methode aus.

### <a name="encryption-via-the-envelope-technique"></a>Verschlüsselung über den Umschlag-Methode

Verschlüsselung über den Umschlag Methode funktioniert wie folgt:

1. Die Azure-Speicher-Client-Bibliothek generiert einen Schlüssel für Verschlüsselung von Inhalten (CEK), der einen one einmalig verwenden symmetrischen Schlüssel ist.
2. Benutzerdaten ist mit dieser CEK verschlüsselt.
3. Die CEK wird dann umschlossen () mit den wichtigsten Schlüssel verschlüsselt (KEK). Der KEK wird durch eine Schlüssel-ID identifiziert und werden ein Paar für asymmetrische Schlüssel oder einen symmetrischen Schlüssel und können lokal verwaltete oder möglich in Azure-Taste Depots gespeichert.

    Die Speicher-Client-Bibliothek selbst weist nie Zugriff auf KEK. Die Bibliothek Ruft den wichtigsten Umbruch Algorithmus, der Taste Tresor genannt ist. Benutzerdefinierte Anbieter für Key Umbruch/Entpacken verwenden, falls gewünscht, können Benutzer auswählen.

4. Die verschlüsselten Daten werden dann in der Azure-Speicherdienst hochgeladen werden. Der Umgebrochene Schlüssel sowie einige weitere Verschlüsselung Metadaten wird als Metadaten (auf ein Blob) gespeichert oder interpoliert mit der verschlüsselten Daten (Warteschlangennachrichten und Tabelle Einheiten).

### <a name="decryption-via-the-envelope-technique"></a>Entschlüsseln über den Umschlag-Methode

Entschlüsseln über den Umschlag Methode funktioniert wie folgt:

1. Die Client-Bibliothek wird davon ausgegangen, dass der Benutzer die Taste Verschlüsselungsschlüssels (KEK) lokal oder in Azure-Taste Depots verwalten. Der Benutzer muss nicht den bestimmten Schlüssel kennen, der für die Verschlüsselung verwendet wurde. Stattdessen kann eine Key Auflösung der anderen wichtigen Bezeichner in Tasten aufgelöst wird eingerichtet und verwendet.
2. Die Clientbibliothek-downloads für die verschlüsselten Daten zusammen mit Verschlüsselung Material, die auf den Dienst gespeichert ist.
3. Umgebrochene Inhalt Verschlüsselungsschlüssels (CEK) ist, und klicken Sie dann mit der Taste Verschlüsselung allein stehenden (entschlüsselt) wichtiger (KEK). Hier wird erneut, die Client-Bibliothek nicht auf KEK zugreifen. Ruft Sie einfach das benutzerdefinierte oder Entpacken Algorithmus Schlüssel Tresor-Anbieter.
4. Der Inhalt Verschlüsselungsschlüssels (CEK) wird dann zum Entschlüsseln der verschlüsselten Benutzerdaten.

## <a name="encryption-mechanism"></a>Verfahren für Verschlüsselung

Die Speicher-Client-Bibliothek wird [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) um Benutzerdaten verschlüsseln. Insbesondere [Verschlüsselung Block verketten (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) Modus mit AES. Jede-service funktioniert etwas anders ausgeführt, sodass jeder von ihnen hier besprochen wird.

### <a name="blobs"></a>BLOBs

Die Client-Bibliothek unterstützt die Verschlüsselung nur gesamte Blobs aktuell. Insbesondere Verschlüsselung unterstützt wird, wenn Benutzer die **UploadFrom** verwenden* Methoden oder die * *OpenWrite** Methode. Für Downloads, beide abgeschlossen und Bereich Downloads unterstützt werden.

Bei der Verschlüsselung die Clientbibliothek generiert eine zufällige Initialisierung Vektor (IV) von 16 Bytes, zusammen mit einem zufällige Inhalt Schlüssel (CEK) 32 Bytes und Umschlag Verschlüsselung BLOB-Daten mithilfe dieser Informationen ausführen. Der Umgebrochene CEK und einige zusätzliche Verschlüsselung Metadaten sind wie Metadaten zusammen mit der verschlüsselten Blob-Dienst BLOB-dann gespeichert.

> [AZURE.WARNING] Wenn Sie bearbeiten oder eigene Metadaten für das Blob hochladen, müssen Sie sicherstellen, dass diese Metadaten gewahrt wird. Wenn Sie hochladen neue Metadaten ohne diese Metadaten, die Umgebrochene CEK, IV und sonstige Metadaten verloren und der BLOB-Inhalt werden nie einer abrufbaren erneut.

Herunterladen eines verschlüsselten BLOBs umfasst den Inhalt des gesamten Blob mithilfe der **DownloadTo**abrufen*/**BlobReadStream** Komfort Methoden. Der Umgebrochene CEK ausgepackt und zusammen mit der IV (in diesem Fall als Blob-Metadaten gespeichert) verwendet wird, um die entschlüsselten Daten an den Benutzer zurückzugeben.

Herunterladen eines beliebigen Textbereichs (**DownloadRange*** Methoden) im verschlüsselten Blob umfasst Einstellen des Bereichs von Benutzer bereitgestellt werden, um eine kleine zusätzliche Datenmenge abrufen, die den angeforderten Bereich erfolgreich entschlüsseln verwendet werden können.

Alle Typen blob (Blobs blockieren, Seite Blobs und Anfügen Blobs) können verschlüsselt/entschlüsselt werden dieses Schema verwenden.

### <a name="queues"></a>Warteschlangen

Da Warteschlangennachrichten von einem beliebigen Format werden können, definiert die Clientbibliothek ein benutzerdefiniertes Format, das Initialization Vektor (IV) und der verschlüsselten Inhalt Verschlüsselungsschlüssels (CEK) in den Nachrichtentext enthält.

Bei der Verschlüsselung die Clientbibliothek generiert einen zufälligen IV von 16 Bytes sowie eine zufällige CEK 32 Bytes und führt Umschlag Verschlüsselung Warteschlange Nachricht Text anhand dieser Informationen. Der Umgebrochene CEK und einige zusätzliche Verschlüsselung Metadaten werden dann an die Warteschlange verschlüsselte Nachricht hinzugefügt. Klicken Sie auf den Dienst wird diese geänderte Nachricht (siehe unten) gespeichert.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Der Umgebrochene Schlüssel ist während entschlüsseln aus der Warteschlange Nachricht extrahiert und unverpacktem. Die IV ist auch aus der Warteschlange Nachricht extrahiert und zusammen mit dem allein stehenden Schlüssel verwendet werden, um die Daten der Warteschlange Nachricht entschlüsseln. Beachten Sie, dass die Verschlüsselung Metadaten gedrückt, während sie das 64 KB Limit für eine Nachricht Warteschlange berücksichtigt, der Einfluss verwaltbare sollten kleine (unter 500 Byte) ist.

### <a name="tables"></a>Tabellen

Die Client-Bibliothek unterstützt die Verschlüsselung der Elementeigenschaften für einfügen und Ersetzungsvorgänge.

>[AZURE.NOTE] Zusammenführen wird derzeit nicht unterstützt. Da eine Teilmenge der Eigenschaften möglicherweise zuvor mit einem anderen Schlüssel verschlüsselt sind, führt einfach die neuen Eigenschaften zusammenführen und aktualisieren die Metadaten Datenverlust. Zusammenführen von entweder erfordert zusätzliche Service-Anrufe, die bereits vorhandene Entität vom Dienst zu lesen, oder verwenden einen neuen Product Key pro Eigenschaft, beide über eignen sich nicht für die Leistung zu verbessern.

Tabelle Daten Verschlüsselung funktioniert wie folgt aus:  

1. Benutzer geben Sie die Eigenschaften verschlüsselt werden.
2. Die Client-Bibliothek generiert eine zufällige Initialisierung Vektor (IV) von 16 Bytes zusammen mit einem zufällige Inhalt Schlüssel (CEK) 32 Bytes für jede Person, und führt Umschlag Verschlüsselung auf die einzelnen Eigenschaften verschlüsselt werden, indem Sie einen neuen IV pro Eigenschaft abgeleitet. Die verschlüsselte Eigenschaft wird als binäre Daten gespeichert werden.
3. Der Umgebrochene CEK und einige zusätzliche Verschlüsselung Metadaten werden als zwei zusätzliche reservierte Eigenschaften dann gespeichert. Die erste reservierte Eigenschaft (_ClientEncryptionMetadata1) ist eine Zeichenfolgeneigenschaft, die Informationen zu IV, Version und Umgebrochene Schlüssel enthält. Die zweite reservierte Eigenschaft (_ClientEncryptionMetadata2) ist eine binäre Eigenschaft, die Informationen zu den Eigenschaften enthält, die verschlüsselt sind. Die Informationen in dieser zweiten Eigenschaft (_ClientEncryptionMetadata2) ist selbst verschlüsselt.
4. Aufgrund von diese zusätzlichen reservierten Eigenschaften für die Verschlüsselung erforderlich ist können Benutzer jetzt nur 250 benutzerdefinierte Eigenschaften anstelle von 252 verfügen. Die Gesamtgröße der Entität muss kleiner als 1 MB.

Beachten Sie, dass nur die Zeichenfolgeneigenschaften verschlüsselt werden können. Wenn Sie andere Arten von Eigenschaften werden verschlüsselt werden, müssen sie in Zeichenfolgen konvertiert werden. Die verschlüsselten Zeichenfolgen werden vom Dienst als binäre Eigenschaften gespeichert, und sie wieder in Zeichenfolgen konvertiert, nach dem entschlüsseln.

Für Tabellen, sowie die Verschlüsselungsrichtlinie müssen Benutzer die Eigenschaften verschlüsselt werden angeben. Dies kann entweder ein Attribut [EncryptProperty] (für POCO juristische Personen, die von TableEntity abgeleitet) oder eine Verschlüsselung Auflösung in angeben Anforderung Optionen erfolgen. Eine Verschlüsselung Auflösung ist eine Stellvertretung, die eine Partitionsschlüssel, Zeilenschlüssel und Eigenschaftsname verwendet und gibt einen Boolean zurück, der angibt, ob die Eigenschaft verschlüsselt werden soll. Bei der Verschlüsselung wird die Clientbibliothek diese Informationen verwendet, um zu entscheiden, ob eine Eigenschaft beim Schreiben zur Übertragung verschlüsselt werden soll. Die Stellvertretung sieht auch die Möglichkeit der Logik vor wie Eigenschaften verschlüsselt werden. (Beispielsweise ist X, dann verschlüsseln Eigenschaft A; andernfalls verschlüsseln Eigenschaften A und b) Beachten Sie, dass es nicht erforderlich, diese Informationen beim Lesen oder Abfragen von Personen zu machen.

### <a name="batch-operations"></a>Stapel Vorgänge

In Stapel Vorgänge wird die gleiche KEK über alle Zeilen in dieser Vorgang Stapel verwendet werden, da die Clientbibliothek nur ein einziges Optionsobjekt (und daher einer Richtlinie/KEK) ermöglicht pro Stapel Vorgang. Die Client-Bibliothek intern einen neuen zufällige IV und zufällige CEK pro Zeile in den Stapel generiert jedoch. Benutzer können auch andere Eigenschaften für jeden Vorgang im Stapel verschlüsseln dieses Verhalten in die Verschlüsselung Auflösung definieren auswählen.

### <a name="queries"></a>Abfragen

Um Abfrage durchzuführen, müssen Sie einen Key Auflösung angeben, die alle Schlüssel im Resultset auflösen kann. Wenn eine Entität im Abfrageergebnis enthaltenen mit einem Anbieter aufgelöst werden kann, wird die Clientbibliothek ein Fehler ausgelöst. Für jede Abfrage, die serverseitige Projektionen ausführt, wird die Clientbibliothek die spezielle Verschlüsselung Metadateneigenschaften (_ClientEncryptionMetadata1 und _ClientEncryptionMetadata2) hinzufügen standardmäßig die ausgewählten Spalten.

## <a name="azure-key-vault"></a>Azure Key Tresor

Azure-Taste Tresor hilft cryptographic Tasten schützen und Kennwörter von Cloudanwendungen und Dienste verwendet. Mithilfe von Azure-Taste Tresor können Benutzer Schlüssel und Kennwörter (z. B. Authentifizierung, Speicher Konto Tasten, Daten Verschlüsselung Tasten,. verschlüsseln. PFX-Dateien und Kennwörter) mithilfe von Tasten, die durch Hardware Security Module (HSMs) geschützt werden. Weitere Informationen finden Sie unter [Neuigkeiten Azure-Taste Tresor?](../key-vault/key-vault-whatis.md).

Die Speicher-Client-Bibliothek verwendet die Taste Tresor Core-Bibliothek, um ein gemeinsamer Rahmen über Azure bereitstellen, für die Verwaltung von Tasten. Benutzer erhalten auch den zusätzlichen Vorteil der Nutzung der Schlüssel Tresor Extensions-Bibliothek. Die Extensions-Bibliothek bietet nützliche Funktionen, um einfache und nahtlose symmetrisch/RSA lokale und Key Cloud-Anbieter und mit Aggregation und Zwischenspeichern.

### <a name="interface-and-dependencies"></a>Benutzeroberfläche und Abhängigkeiten

Es gibt drei Schlüssel Tresor-Paketen:

- Microsoft.Azure.KeyVault.Core enthält die IKey und IKeyResolver. Es ist ein kleines Paket mit keine Abhängigkeiten. Die Speicher-Client-Bibliothek für .NET, die sie als Abhängigkeit definiert.
- Microsoft.Azure.KeyVault enthält den Taste Tresor REST-Client.
- Microsoft.Azure.KeyVault.Extensions enthält Code der Erweiterung, die Implementierung von cryptographic Algorithmen und eine RSAKey und eine SymmetricKey enthält. Es hängt von den Namespaces Core und KeyVault und stellt die Funktionalität zum Definieren einer aggregieren Auflösung (Wenn Sie möchten, dass Benutzer mehrere wichtige Anbieter verwenden) und eine Zwischenspeichern Key Auflösung. Zwar Speicher-Client-Bibliothek nicht direkt auf diese-Paket abhängt Wenn Benutzer Azure-Taste Tresor verwenden, um ihre Schlüssel speichern oder verwenden Sie die Taste Tresor Erweiterungen zu nutzen den lokalen cloud cryptographic Provider möchten, benötigen sie dieses Paket.

Schlüssel Tresor für wertvolle Master Tasten vorgesehen ist und Drosselung Grenzwerte pro Schlüssel Tresor dienen mit diesem berücksichtigen. Wenn Sie clientseitige Verschlüsselung mit Schlüssel Tresor durchführen zu können, sollten das bevorzugte Modell symmetrische Master Schlüssel als vertrauliche Informationen in Schlüssel Tresor gespeichert und Cache lokal verwenden. Benutzer müssen die folgenden Schritte ausführen:

1. Erstellen Sie einen geheimen offline, und klicken Sie auf-Taste Tresor hochladen Sie können.
2. Verwenden Sie das Kennwort des Basis Bezeichner als Parameter, zum Beheben der aktuellen Version der Schlüssel für die Verschlüsselung und diese Informationen lokal zwischenspeichern. Verwenden Sie zum Zwischenspeichern von CachingKeyResolver; Benutzer werden nicht erwartet implementieren eigene Logik Zwischenspeichern.
3. Verwenden Sie die Auflösung Zwischenspeichern als Eingabe beim Erstellen der Verschlüsselungsrichtlinie.

Weitere Informationen zur Verwendung von Key Tresor finden Sie in den [Codebeispielen Verschlüsselung](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Bewährte Methoden

Unterstützung für Verschlüsselung ist nur in der Bibliothek Speicher Client für .NET verfügbar. Windows Phone und Windows-Runtime unterstützt nicht aktuell Verschlüsselung.

>[AZURE.IMPORTANT] Achten Sie auf diese wichtigen Punkte, wenn die clientseitige Verschlüsselung verwendet:
>
>- Verwenden Sie beim Lesen aus oder Schreiben in einer verschlüsselten Blob gesamte Blob Upload Befehle und Bereich/ganze Blob Download Befehle. Vermeiden Sie das Schreiben in einer verschlüsselten Blob mit Protokoll Operationen wie blockieren setzen, Blockliste setzen, Schreiben von Seiten, Löschen von Seiten oder blockieren Anfügen; Andernfalls können Sie beschädigter verschlüsselte Blob und nicht gelesen werden.
>- Für Tabellen vorhanden ist eine ähnliche Einschränkung aus. Achten Sie darauf, dass keine verschlüsselte Eigenschaften aktualisieren, ohne die Verschlüsselung Metadaten zu aktualisieren.
>- Wenn Sie auf die verschlüsselte Blob Metadaten festgelegt, überschreiben Sie möglicherweise die Verschlüsselung-bezogene Metadaten für Verschlüsselung erforderlich ist, da das Festlegen von Metadaten nicht erweiterbar ist. Dies gilt auch für Momentaufnahmen. Vermeiden Sie es, Metadaten beim Erstellen einer Momentaufnahme einer verschlüsselten Blob. Wenn Metadaten festgelegt werden muss, achten Sie darauf, dass Sie zum Aufrufen der Methode **FetchAttributes** zuerst, um die aktuellen Verschlüsselung Metadaten zu erhalten, und vermeiden Sie gleichzeitige schreibt ein, während Metadaten festgelegt wird.
>- Aktivieren Sie die Eigenschaft **RequireEncryption** in der Standardoptionen für die Anfrage für Benutzer, die nur in Verbindung mit verschlüsselten Daten arbeiten soll. Weitere Informationen finden Sie unter.


## <a name="client-api--interface"></a>Client-API / Schnittstelle

Beim Erstellen eines Objekts EncryptionPolicy, können Benutzer nur einen Schlüssel (implementieren IKey), bieten nur eine Auflösung (implementieren IKeyResolver) oder beides. IKey ist die grundlegende Art des Schlüssels, die mit einer Key-ID gekennzeichnet wird und die Logik für den Textumbruch/Entpacken bereitstellt. IKeyResolver dient zum Auflösen von einer Taste während des Prozesses entschlüsseln. Eine ResolveKey-Methode, die eine IKey angegebenen eine Schlüssel-ID gibt definiert. Dies bietet Benutzern die Möglichkeit, zwischen mehreren Tasten auszuwählen, die an mehreren Speicherorten verwaltet werden.

- Für die Verschlüsselung der Schlüssel wird immer verwendet, sowie das Fehlen eines Schlüssels führt zu einem Fehler Fehlern.
- Zum Entschlüsseln:
    - Die wichtige Auflösung wird aufgerufen, wenn den Schlüssel abgerufen angegeben. Die Auflösung angegeben, jedoch verfügt nicht über eine Zuordnung für den Key Bezeichner enthält, wird ein Fehler ausgelöst.
    - Auflösung nicht angegeben, jedoch ein Schlüssel angegeben ist, wird der Schlüssel verwendet, seinen Bezeichner entspricht den erforderlichen Key Bezeichner enthält. Wenn der Bezeichner nicht übereinstimmt, wird ein Fehler ausgelöst.

Eine ausführlichere End-to-End-Szenario für Blobs, Queues und Tabellen sowie Integration Schlüssel Tresor führen Sie die [Verschlüsselung Beispiele](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) vor.

### <a name="requireencryption-mode"></a>RequireEncryption Modus

Benutzer können optional einen Modus des Vorgangs aktivieren, in dem alle Uploads und Downloads verschlüsselt werden müssen. In diesem Modus tritt Versuche, Daten keine Verschlüsselungsrichtlinie hochladen oder Herunterladen von Daten, die nicht auf den Dienst verschlüsselt sind auf dem Client. Die Eigenschaft **RequireEncryption** des Objekts Optionen Anforderung steuert dieses Verhalten. Wenn die Anwendung alle Objekte in Azure-Speicher verschlüsselt wird, können Sie die Eigenschaft **RequireEncryption** auf die Anfrage Standardoptionen für den Dienst Client-Objekt festlegen. Legen Sie beispielsweise **CloudBlobClient.DefaultRequestOptions.RequireEncryption** auf **Wahr** Verschlüsselung für alle Blob-Vorgänge, die über das Clientobjekt durchgeführt werden muss.

### <a name="blob-service-encryption"></a>BLOB-Service-Verschlüsselung

Erstellen Sie ein **BlobEncryptionPolicy** Objekt, und legen Sie sie in den Optionen Anforderung (pro API oder auf einer Clientebene mithilfe von **DefaultRequestOptions**). Allen weiteren Aufgaben werden von der Clientbibliothek intern behandelt werden.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Upload the encrypted contents to the blob.
    blob.UploadFromStream(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    MemoryStream outputStream = new MemoryStream();
    blob.DownloadToStream(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Die Verschlüsselung der service

Erstellen Sie ein **QueueEncryptionPolicy** Objekt, und legen Sie sie in den Optionen Anforderung (pro API oder auf einer Clientebene mithilfe von **DefaultRequestOptions**). Allen weiteren Aufgaben werden von der Clientbibliothek intern behandelt werden.


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
    queue.AddMessage(message, null, null, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);

### <a name="table-service-encryption"></a>Tabelle Service-Verschlüsselung

Zusätzlich zum Erstellen einer Verschlüsselungsrichtlinie und diese Anforderung Optionen festlegen, müssen Sie angeben einer **EncryptionResolver** in **TableRequestOptions**, oder legen Sie das Attribut [EncryptProperty] für die Entität aus.

#### <a name="using-the-resolver"></a>Verwenden die Auflösung


    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    {
        EncryptionResolver = (pk, rk, propName) =>
        {
            if (propName == "foo")
            {
                return true;
            }
            return false;
        },
        EncryptionPolicy = policy
    };

    // Insert Entity
    currentTable.Execute(TableOperation.Insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    {
        EncryptionPolicy = policy
    };

    TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
    TableResult result = currentTable.Execute(operation, retrieveOptions, null);

#### <a name="using-attributes"></a>Verwenden von Attributen

Wie erwähnt, wenn die Entität TableEntity implementiert, können die Eigenschaften mit dem Attribut [EncryptProperty] anstelle der **EncryptionResolver**versehen werden.

    [EncryptProperty]
    public string EncryptedProperty1 { get; set; }

## <a name="encryption-and-performance"></a>Verschlüsselung und Leistung

Beachten Sie, dass Ihre Daten ergibt zusätzliche Leistung Verwaltungsaufwand Speicher Datenbankschutz werden soll. Inhalt Schlüssel und IV generiert werden muss, muss der Inhalt selbst verschlüsselt werden und zusätzliche Metadaten formatiert und hochgeladen werden müssen. Dieser Aufwand variieren je nach der Menge der verschlüsselten Daten. Es empfiehlt sich, dass Kunden immer deren Anwendung für Leistung bei der Entwicklung testen.

## <a name="next-steps"></a>Nächste Schritte

- [Lernprogramm: Verschlüsseln Sie und Entschlüsseln Sie Blobs in Azure-Taste Tresor mit Microsoft Azure-Speicher](storage-encrypt-decrypt-blobs-key-vault.md)
- Herunterladen der [Azure-Speicher-Client-Bibliothek für .NET NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.Storage)
- Herunterladen der Azure-Taste Tresor NuGet [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [Client](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/)und [Erweiterungen](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) -Paketen  
- Besuchen Sie die [Azure-Taste Vaulting Dokumentation](../key-vault/key-vault-whatis.md)

