Wenn Sie die URL einer freigegebenen Access Signatur (SAS), die Ihnen den Zugriff auf Ressourcen in einem Speicherkonto gewährt verfügen, können Sie die SAS in einer Verbindungszeichenfolge. Da die SAS auf dem URI die erforderlichen Informationen für die Authentifizierung der Anforderung umfasst, stellt der SAS URI das Protokoll, der Endpunkt und die erforderlichen Anmeldeinformationen auf die Ressource zuzugreifen.

Um eine Verbindungszeichenfolge erstellen eine freigegebenen Access-Signatur enthält, geben Sie die Zeichenfolge in folgendem Format ein:

    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    SharedAccessSignature=sasToken

Jeden Dienstendpunkt ist optional, obwohl die Verbindungszeichenfolge mindestens eine enthalten muss.

>[AZURE.NOTE] Verwendung von HTTPS mit einem SAS wird als bewährte Methode empfohlen.
>
>Wenn Sie eine SAS in einer Verbindungszeichenfolge in einer Konfigurationsdatei angeben, müssen Sie Sonderzeichen in der URL codiert werden soll.

### <a name="service-sas-example"></a>Dienst SAS-Beispiel

Hier ist ein Beispiel für eine Verbindungszeichenfolge mit einem Dienst SAS für Blob-Speicher ein:

    BlobEndpoint=https://storagesample.blob.core.windows.net;SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D

Und so sieht ein Beispiel für dieselbe Verbindungszeichenfolge mit Codierung von Sonderzeichen:

    BlobEndpoint=https://storagesample.blob.core.windows.net;SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D

### <a name="account-sas-example"></a>Konto SAS-Beispiel

Hier ist ein Beispiel für eine Verbindungszeichenfolge, die ein Konto SAS für Blob und Dateispeicher enthält. Beachten Sie, dass die Endpunkte für beide Dienste angegeben werden:

    BlobEndpoint=https://storagesample.blob.core.windows.net;
    FileEndpoint=https://storagesample.file.core.windows.net;
    SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl

Und so sieht ein Beispiel für die gleiche Verbindungszeichenfolge mit URL-Codierung:

    BlobEndpoint=https://storagesample.blob.core.windows.net;
    FileEndpoint=https://storagesample.file.core.windows.net;
    SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl