<properties 
    pageTitle="Konfigurieren eine Verbindungszeichenfolge zur Azure-Speicher | Microsoft Azure"
    description="Konfigurieren einer Verbindungszeichenfolge mit einer Firma Azure-Speicher. Eine Verbindungszeichenfolge enthält die Informationen zum Authentifizieren des Zugriffs mit einem Speicherkonto aus Ihrer Anwendung zur Laufzeit erforderlich sind."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Konfigurieren Sie die Verbindungszeichenfolgen Azure-Speicher

## <a name="overview"></a>(Übersicht)

Eine Verbindungszeichenfolge enthält die Authentifizierungsinformationen Zugriff auf Daten in einem Konto Azure-Speicher aus Ihrer Anwendung zur Laufzeit erforderlich. Sie können eine Verbindungszeichenfolge zu konfigurieren:

- Verbinden Sie mit Azure Speicheremulator.
- Zugriff auf ein Speicherkonto in Azure.
- Zugriff auf die angegebenen Ressourcen in Azure über eine Signatur freigegebenen Access (SAS).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Speichern der Verbindungszeichenfolge

Ihrer Anwendung müssen die Verbindungszeichenfolge zur Laufzeit Anfragen versucht, Azure-Speicher für die Authentifizierung zugreifen. Sie haben eine Reihe von Optionen für das Speichern der Verbindungszeichenfolge aus:

- Für die Ausführung einer Anwendung auf dem Desktop oder auf einem Gerät, können Sie in die Verbindungszeichenfolge Speichern einer `app.config `Datei oder eines `web.config` Datei. Fügen Sie die Verbindungszeichenfolge zum Abschnitt **AppSettings** .
- Für eine Anwendung, die in einer Azure-Cloud-Dienst ausgeführt wird können Sie Ihre Verbindungszeichenfolge [Azure Service Konfiguration Schemadatei (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx)speichern. Fügen Sie die Verbindungszeichenfolge zum Abschnitt **ConfigurationSettings** der Dienstkonfigurationsdatei ein.
- Sie können auch die Verbindungszeichenfolge direkt in Ihrem Code verwenden. In den meisten Fällen empfiehlt jedoch Ihre Konfigurationszeichenfolge in einer Konfigurationsdatei zu speichern.

Speichern der Verbindungszeichenfolge innerhalb einer Konfigurationsdatei erleichtert die Verbindungszeichenfolge zum Wechseln zwischen Speicheremulator und ein Konto Azure-Speicher in der Cloud zu aktualisieren. Sie müssen nur die Verbindungszeichenfolge für Ihre zielumgebung verweisen bearbeiten.

Die [Microsoft Azure-Konfigurations-Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) -Klasse können zum Zugreifen auf Ihre Verbindungszeichenfolge zur Laufzeit unabhängig davon, wo die Anwendung ausgeführt wird.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Erstellen einer Verbindungszeichenfolge zur Speicheremulator

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Weitere Informationen zu Speicheremulator finden Sie unter [Verwendung der Speicheremulator Azure für Entwicklung und Testen](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Erstellen einer Verbindungszeichenfolge mit einer Firma Azure-Speicher

Verwenden Sie zum Erstellen einer Verbindungszeichenfolge bei Ihrem Konto Azure-Speicher das Format der Verbindungszeichenfolge darunter ein. Angeben, ob Sie die Verbindung mit dem Speicherkonto über HTTPS (empfohlen) oder HTTP, ersetzen `myAccountName` mit dem Namen Ihrer Speicher-Konto, und Ersetzen `myAccountKey` mit Ihrem Konto Zugriffstaste:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Beispielsweise wird die Verbindungszeichenfolge ähnlich wie im folgenden Beispiel Verbindungszeichenfolge aus:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure-Speicher unterstützt HTTP- und HTTPS in einer Verbindungszeichenfolge. mit HTTPS wird jedoch dringend empfohlen.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Erstellen einer Verbindungszeichenfolge mithilfe einer freigegebenen Access-Signatur

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Erstellen einer Verbindungszeichenfolge auf einen Endpunkt explizite Speicher

Sie können die Endpunkte explizit in der Verbindungszeichenfolge anstelle von der Standardendpunkte angeben. Um eine Verbindungszeichenfolge erstellen, die einen expliziten Endpunkt gibt an, geben Sie den vollständigen Dienst-Endpunkt für jeden Dienst, einschließlich der Protokollspezifikation (HTTPS (empfohlen) oder HTTP) im folgenden Format an:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Ein Szenario, in dem Sie einen expliziten Endpunkt angeben möchten, ist, wenn Sie eine benutzerdefinierte Domäne Ihre Blob-Speicher-Endpunkt zugeordnet haben. In diesem Fall können Sie Ihre benutzerdefinierten Endpunkt für Blob-Speicher in der Verbindungszeichenfolge angeben und optional die Standardendpunkte für den anderen Dienst angeben, wenn die Anwendung sie verwendet.

Hier finden Beispiele für gültige Verbindungszeichenfolgen, die einen expliziten Endpunkt für den Dienst Blob angeben:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Der Endpunktwert, der in der Verbindungszeichenfolge aufgeführt ist wird verwendet, um die Anforderung URIs zum Dienst Blob erstellen und unveränderlich bleiben Form von einem beliebigen URIs, die dem Code zurückgegeben werden.

Beachten Sie, dass, wenn Sie einen Endpunkt aus der Verbindungszeichenfolge weglassen entscheiden, klicken Sie dann Sie nicht die Verbindungszeichenfolge zu verwenden, um Daten in diesen Dienst über den Code zugreifen können.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Erstellen einer Verbindungszeichenfolge mit einem Suffix Endpunkt

Zum Erstellen einer Verbindungszeichenfolge für Speicherdienst in Regionen oder Instanzen mit anderen Endpunkt Suffixe, z. B. für Azure China oder Azure Governance, verwenden Sie das folgenden Format der Verbindungszeichenfolge aus. Gibt an, ob Sie mit dem Speicherkonto über HTTP oder HTTPS verbinden, ersetzen möchten `myAccountName` ersetzen durch den Namen Ihres Kontos Speicher, `myAccountKey` mit Ihrem Konto Access-Taste gedrückt, und klicken Sie auf Ersetzen `mySuffix` mit dem Suffix URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Beispielsweise sollte die Verbindungszeichenfolge ähnlich wie die folgende Verbindungszeichenfolge aus:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Analysieren einer Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Nächste Schritte

- [Verwenden Sie für Test- und Azure Speicheremulator](storage-use-emulator.md)
- [Azure-Speicher-Explorers](storage-explorers.md)
- [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md)
