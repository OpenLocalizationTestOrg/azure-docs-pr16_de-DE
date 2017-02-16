## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurieren der Anwendungs Zugriff auf Azure-Speicher

Es gibt zwei Methoden zum Authentifizieren Ihrer Anwendungs auf Speicherdienste zuzugreifen:

- Freigegebene Schlüssel: Freigegebene Schlüssel verwenden nur zu Testzwecken
- Freigegebene Access-Signatur (SAS): Verwenden Sie SAS für Applikationen Herstellung

### <a name="shared-key"></a>Gemeinsamer Schlüssel
Authentifizierung mit freigegebenem Schlüssel bedeutet, dass eine Anwendung Storage Services Zugriff auf Ihren Kontonamen und kontoschlüssel verwendet wird. Im Sinne schnell mit so verwenden Sie diese Bibliothek verwenden wir Authentifizierung anhand vorinstallierter Schlüssel erste Schritte.

> [AZURE. Warnung (nur verwenden Authentifizierung anhand vorinstallierter Schlüssel zu Testzwecken!)] Ihren Kontonamen und kontoschlüssel, die Lese-und Schreibzugriff auf das zugeordnete Speicherplatz Konto gewähren möchten, werden jede Person, die Ihre app downloads verteilt. Dies ist eine gute **nicht** üben, wie Sie Ihren Schlüssel gefährdet von Datenbankobjekten Clients Probleme riskieren.

Bei Verwendung der freigegebenen Schlüssel Authentifizierung, erstellen Sie eine [Verbindungszeichenfolge](../articles/storage/storage-configure-connection-string.md). Die Verbindungszeichenfolge besteht aus:  

- Die **DefaultEndpointsProtocol** - können Sie HTTP oder HTTPS auswählen. Mit HTTPS wird jedoch dringend empfohlen.
- **Kontoname** - den Namen Ihres Kontos Speicher
- Die **Kontoschlüssel** - klicken Sie im [Portal Azure](https://portal.azure.com)navigieren Sie zu Ihrem Speicherkonto, und klicken Sie auf die **Tasten** -Symbol, um diese Informationen zu suchen.
- (Optional) **EndpointSuffix** - dieser Wert wird für Speicher-Services in Regionen mit anderen Endpunkt Suffixe, wie China Azure oder Azure Governance.

Hier ist ein Beispiel der Verbindungszeichenfolge mithilfe der freigegebenen Schlüssel Authentifizierung ein:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Freigegebene Access Signaturen (SAS)
Bei einer mobilen Anwendung empfiehlt sich für die Authentifizierung einer Anforderung von einem Client anhand der Azure-Speicherdienst mithilfe einer freigegebenen Access Signatur (SAS). SAS können Sie eine den Clientzugriff auf eine Ressource für einen angegebenen Zeitraum, mit einem zuvor festgelegten Berechtigungen erteilen.
Als Besitzer des Kontos Speicher müssen Sie Generieren einer SAS für mobile Clients zu nutzen. Um die SAS generieren, sollten Sie wahrscheinlich einen separaten Dienst zu schreiben, der die SAS, an den Kunden geliefert werden generiert. Zu Testzwecken können Sie den [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com) oder der [Azure-Portal](https://portal.azure.com) zu einem SAS generieren verwenden. Wenn Sie die SAS erstellen, können Sie angeben, den Zeitraum, für den die SAS gültig ist, und die Berechtigungen, die die SAS an dem Kunden gewährt.

Im folgenden Beispiel wird veranschaulicht, wie mit Microsoft Azure-Speicher-Explorer einem SAS generiert wird.

1. Wenn Sie nicht bereits geschehen, [Installieren Sie die Microsoft Azure-Speicher-Explorer](http://storageexplorer.com)

2. Verbinden Sie mit Ihr Abonnement.

3. Klicken Sie auf Ihr Konto Speicher, und klicken Sie auf der Registerkarte "Aktionen" unten links auf. Klicken Sie auf "Erste freigegebene Access Signatur" um "Verbindungszeichenfolge" für Ihre SAS generieren.

4. Hier ist ein Beispiel einer Verbindungszeichenfolge SAS, dass gewährt Lese- und am Dienst, Container und Objektebene für den Blob-Dienst für das Konto ein Speicher Schreibberechtigungen ein.

  `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Wie Sie sehen können, wenn mit einem SAS, sind Sie nicht Ihren kontoschlüssel in Ihrer Anwendung verfügbar machen. Weitere Informationen finden Sie Informationen zu-SAS- und bewährte Methoden für die Verwendung von SAS Auschecken [freigegeben Access Signaturen: Grundlegendes zu SAS-Modell](../articles/storage/storage-dotnet-shared-access-signature-part-1.md).
