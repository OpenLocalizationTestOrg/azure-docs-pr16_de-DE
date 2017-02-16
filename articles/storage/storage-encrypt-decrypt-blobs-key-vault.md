<properties
    pageTitle="Lernprogramm: Verschlüsseln und Entschlüsseln Blobs in Azure-Taste Tresor mit Microsoft Azure-Speicher | Microsoft Azure"
    description="In diesem Lernprogramm führt Sie durch das Verschlüsseln und Entschlüsseln eines BLOBs clientseitige Verschlüsselung für Microsoft Azure-Speicher mit Azure-Taste Tresor mit."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Lernprogramm: Verschlüsseln Sie und Entschlüsseln Sie Blobs in Azure-Taste Tresor mit Microsoft Azure-Speicher

## <a name="introduction"></a>Einführung

In diesem Lernprogramm wird erläutert, wie vornehmen clientseitige Speicher Verschlüsselung mit Azure-Taste Tresor wiederzuverwenden. Es führt Sie durch das Verschlüsseln und Entschlüsseln eines BLOBs in einer Console-Anwendung, die mit diesen Technologien.

**Geschätzte Dauer:** 20 Minuten

Überblick über die Taste Tresor Azure erhalten finden Sie unter [Neuigkeiten Azure-Taste Tresor?](../key-vault/key-vault-whatis.md).

Überblick über clientseitige Verschlüsselung für Azure-Speicher erhalten finden Sie unter [clientseitige Verschlüsselung und Azure Schlüssel Tresor für Microsoft Azure-Speicher](storage-client-side-encryption.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramms abgeschlossen haben, müssen Sie Folgendes:

- Ein Konto Azure-Speicher
- Visual Studio 2013 oder höher
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Clientseitige Verschlüsselung im Überblick

Übersicht über clientseitige Verschlüsselung für Azure-Speicher finden Sie unter [clientseitige Verschlüsselung und Azure Schlüssel Tresor für Microsoft Azure-Speicher](storage-client-side-encryption.md)

Hier ist eine kurze Beschreibung der Funktionsweise der clientseitigen Verschlüsselung aus:

1. Der Azure-Speicher-Client SDK erzeugt einen Schlüssel für Verschlüsselung von Inhalten (CEK), der einen one einmalig verwenden symmetrischen Schlüssel ist.
2. Kundendaten ist mit dieser CEK verschlüsselt.
3. Die CEK wird dann umschlossen () mit den wichtigsten Schlüssel verschlüsselt (KEK). Der KEK wird durch eine Schlüssel-ID identifiziert und werden ein Paar für asymmetrische Schlüssel oder einen symmetrischen Schlüssel und können lokal verwaltete oder möglich in Azure-Taste Tresor gespeichert. Der Speicher-Client selbst weist nie Zugriff auf die KEK. Ruft Sie einfach den Taste Umbruch Algorithmus, der Taste Tresor genannt ist. Verwenden von benutzerdefinierten Anbieter für Schlüssel Umbruch/Entpacken sie bei Bedarf können Kunden wählen.
4. Die verschlüsselten Daten werden dann in der Azure-Speicherdienst hochgeladen werden.


## <a name="set-up-your-azure-key-vault"></a>Richten Sie Ihre Azure-Taste Tresor
Um dieses Lernprogramms fortsetzen möchten, müssen Sie führen Sie die folgenden Schritte aus, die in die [Erste Schritte mit Azure Schlüssel Tresor](../key-vault/key-vault-get-started.md)Lernprogramm beschriebenen:

- Erstellen eines Key Tresor.
- Hinzufügen eines Schlüssels oder geheim zum Key Tresor an.
- Registrieren Sie sich eine Anwendung mit Azure Active Directory.
- Autorisieren Sie die Anwendung die Taste oder geheim verwenden.

Notieren Sie die ClientID und ClientSecret, die generiert wurden, wenn Sie eine Anwendung mit Azure Active Directory registrieren.

Die wichtigsten Tresor erstellen Sie beide Schlüssel. Wir für die restlichen des Lernprogramms angenommen, dass Sie die folgenden Namen verwendet haben: ContosoKeyVault und TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Erstellen Sie eine mit Paketen und AppSettings

Erstellen Sie in Visual Studio eine neue.

Fügen Sie in der Paket-Manager-Konsole erforderlichen Nuget-Pakete hinzu.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


Fügen Sie AppSettings App.Config hinzu.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Fügen Sie den folgenden `using` Anweisungen und überprüfen Sie, dass einen Verweis auf System.Configuration zum Projekt hinzufügen.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Fügen Sie eine Methode, um ein Token an Ihrer Anwendung Konsole zu gelangen.

Die folgende Methode wird von Key Tresor Klassen verwendet, die für den Zugriff auf Ihre Key Tresor authentifizieren müssen.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Access-Speicher und die Taste Tresor in Ihr Programm

Fügen Sie in der Main-Funktion den folgenden Code ein.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Key Tresor Objekt Datenmodellen
>
>Es ist wichtig zu verstehen, dass es tatsächlich zwei Schlüssel Tresor Objektmodelle gibt berücksichtigen: eine basiert auf die REST-API (KeyVault Namespace) und anderen handelt es sich um eine Erweiterung für die Verschlüsselung der clientseitige.

> Der Schlüssel Tresor Client interagiert mit die REST-API und versteht JSON Web Tasten und Kennwörter für die zwei Arten von Aktionen, die im Schlüssel Tresor enthalten sind.

> Die Taste Tresor Erweiterungen sind Klassen, die scheint, die speziell für die clientseitige Verschlüsselung in Azure-Speicher erstellt. Eine Benutzeroberfläche für Schlüssel (IKey) und Klassen auf Grundlage des Konzepts der ein Schlüssel Auflösung enthalten. Es gibt zwei Implementierungen der IKey die benötigten Informationen: RSAKey und SymmetricKey. Sie waren jetzt mit Maßnahmen übereinstimmen, die in einem Tresor Schlüssel enthalten sind, zu diesem Zeitpunkt sind jedoch unabhängig Klassen (, damit die Taste und abgerufen vom Client Tresor Schlüssel geheim IKey nicht implementieren).


## <a name="encrypt-blob-and-upload"></a>Verschlüsseln Blob und Hochladen
Fügen Sie den folgenden Code ein, um ein Blob verschlüsseln und bei Ihrem Konto Azure-Speicher hochladen. Die verwendete Methode **ResolveKeyAsync** gibt eine IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Im folgenden sehen einen Screenshot vom [Klassischen Azure-Portal](https://manage.windowsazure.com) für ein Blob, der mithilfe von clientseitige Verschlüsselung mit einem Schlüssel Tresor gehörende Kehrmatrix Schlüssel verschlüsselt wurde. Die **Schlüssel-ID** -Eigenschaft ist der URI für den Schlüssel im Schlüssel Tresor, der als der KEK fungiert. Die Eigenschaft **EncryptedKey** enthält die verschlüsselte Version der CEK.

![Screenshot mit Blob-Metadaten, die die Verschlüsselung Metadaten enthält.][1]

> [AZURE.NOTE] Wenn Sie sich den Konstruktor BlobEncryptionPolicy anschauen, sehen Sie sich, dass er einen Schlüssel und/oder eine Auflösung akzeptiert werden kann. Beachten Sie, bis zu diesem Zeitpunkt Sie können eine Auflösung für die Verschlüsselung verwenden, da es derzeit keinen standardmäßigen Schlüssel unterstützen.



## <a name="decrypt-blob-and-download"></a>Blob entschlüsseln und herunterladen
Entschlüsseln ist wirklich, wenn mithilfe der Auflösung Klassen sinnvoll. Die ID des Schlüssels für die Verschlüsselung verwendet ist der Blob in ihren Metadaten, zugeordnet, daher besteht kein Grund zum Abrufen des Schlüssels und beachten Sie die Zuordnung zwischen Schlüssel und Blob. Sie müssen lediglich sicherstellen, dass der Schlüssel im Schlüssel Tresor bleibt.   

Der private Schlüssel RSA-Schlüssel bleibt in Tresor-Taste, sodass zum Entschlüsseln ausgeführt, aus den Blob-Metadaten, die die CEK enthält die verschlüsselte-Taste zum Entschlüsseln an Schlüssel Tresor gesendet wird.

Fügen Sie vor, um die Blob entschlüsseln, das Sie gerade hochgeladen habe.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Es gibt ein paar andere Arten von Server, um die Key-Verwaltung, einschließlich erleichtern: AggregateKeyResolver und CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Verwenden Sie die Taste Tresor Kennwörter
Die Möglichkeit, verwenden Sie einen geheimen mit clientseitig Verschlüsselung ist über die SymmetricKey-Klasse, da ein Geheimnis im Wesentlichen einen symmetrischen Schlüssel ist. Aber wie bereits erwähnt, ein Geheimnis in Schlüssel Tresor nicht genau zu einer SymmetricKey zugeordnet. Es gibt ein paar Punkte zu verstehen:


- Die Taste in einer SymmetricKey hat eine feste Länge sein: 128, 192, 256, 384 oder 512 Bits.
- Die Taste in einer SymmetricKey sollte Base64-codierte sein.
- Ein Schlüssel Tresor Geheimnis, die als ein SymmetricKey verwendet werden muss einen Inhaltstyp der "Application/Octet-Stream" im Schlüssel Tresor aufweisen.

Hier ist ein Beispiel in PowerShell erstellen Sie einen geheimen in Tresor-Taste, die als ein SymmetricKey verwendet werden kann.
Hinweis: Der hartcodierte Wert $key, wendet nur Demo Zweck ab. In Ihrem eigenen Code möchten Sie diesen Schlüssel generieren.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

In Ihrer Anwendung Console können Sie den gleichen Anruf als vor diesen Schlüssel als eine SymmetricKey abgerufen.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

Das war's auch. Einfacheres!

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Microsoft Azure-Speicher mit c# finden Sie unter [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Weitere Informationen zu den REST Blob-API finden Sie unter [BLOB-Dienst REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Für die neuesten Informationen zu Microsoft Azure-Speicher wechseln Sie zum [Microsoft Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/).


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
