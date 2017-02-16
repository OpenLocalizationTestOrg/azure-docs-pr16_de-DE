<properties
    pageTitle="Verwenden von freigegebenen Access Signaturen (SAS) | Microsoft Azure"
    description="Lernen Sie delegieren des Zugriffs auf Azure-Speicher Ressourcen, einschließlich Blobs, Queues, Tabellen und Dateien, die mit gemeinsamen Zugriff Signaturen (SAS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="tamram"/>



# <a name="using-shared-access-signatures-sas"></a>Verwenden von freigegebenen Access Signaturen (SAS)

## <a name="overview"></a>(Übersicht)

Verwenden einer freigegebenen Access-Signatur ist (SAS) eine effiziente Möglichkeit zum Erteilen von beschränkter Zugriffs auf Objekte in Ihr Speicherkonto mit anderen Clients, ohne zu Ihren kontoschlüssel verfügbar zu machen. In Teil 1 dieses Lernprogramms auf freigegebenen Access Signaturen wir bieten einen Überblick über das Modell SAS und SAS bewährte Methoden überprüfen.

Zusätzliche Code Beispiele für die Verwendung von SAS über die hier aufgeführten hinausgehen finden Sie unter [Erste Schritte mit Azure BLOB-Speicher in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) und weitere Beispiele, die in der Bibliothek [Azure Codebeispielen](https://azure.microsoft.com/documentation/samples/?service=storage) zur Verfügung. Sie können herunterladen die Stichprobe Applikationen und führen Sie sie, oder Durchsuchen Sie den Code auf GitHub.

## <a name="what-is-a-shared-access-signature"></a>Was ist eine Signatur gemeinsamen Zugriff?

Eine Signatur freigegebenen Access bietet delegierten Zugriff auf Ihr Speicherkonto Ressourcen. Mit einem SAS können Sie Clients Zugriff auf Ressourcen in Ihrem Speicherkonto gewähren ohne Ihre Keys Konto freizugeben. Dies ist der wichtigste Punkt der Verwendung von freigegebenen Access Signaturen in Ihrer Anwendung &mdash; einer SAS ist ein sicheres Verfahren zum Speicherressourcen gemeinsam, ohne Kompromisse bei Ihrem Konto Tasten.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

Eine SAS bietet Ihnen präzise steuern, welche Art von Access Sie Clients, die die SAS gewähren, einschließlich haben:

- Das Intervall, über dem die SAS gültige, einschließlich der Startzeit und der Uhrzeit der Ablauf ist.
- Die Berechtigungen durch die SAS. Beispielsweise möglicherweise eine SAS für ein Blob erteilen einem Benutzer – lesen und Schreibberechtigungen für diese Blob aber nicht Berechtigungen löschen.
- Eine optionale IP-Adresse oder der IP-Adressen von denen Azure-Speicher die SAS akzeptiert. Beispielsweise könnten Sie einen Bereich von IP-Adressen, die zu Ihrer Organisation gehören angeben. Dadurch wird eine andere Maßeinheit der Sicherheit für Ihre SAS.
- Das Protokoll, das Azure-Speicher die SAS akzeptiert. Dieser optionalen Parameter können zum Einschränken des Zugriffs auf Clients HTTPS verwenden.

## <a name="when-should-you-use-a-shared-access-signature"></a>Wann sollten Sie eine freigegebene Access-Signatur verwenden?

Wenn Sie Zugriff auf Ihr Speicherkonto Ressourcen zu einem Client angeben, die mit dem kontoschlüssel nicht vertrauenswürdig möchten, können Sie einem SAS verwenden. Ihre Speicher Konto Keys gehören sowohl einen primären und sekundären Schlüssel, beide über administrativen Zugriff auf Ihr Konto und alle Ressourcen darin gewähren. Verfügbarmachen einer Ihr Konto Schlüssel wird Ihr Konto die Möglichkeit bösartiger oder fahrlässigen verwenden. Freigegebene Access Signaturen bieten eine sichere Alternative, die andere Clients zum Lesen, schreiben und Löschen von Daten in Ihr Speicherkonto entsprechend den Berechtigungen, die Ihnen erteilt haben und ohne Notwendigkeit der kontoschlüssel ermöglicht.

Häufig, wo eine SAS hilfreich ist, wird ein Dienst, in dem Benutzer lesen und Schreiben ihre eigenen Daten bei Ihrem Speicherkonto. In einem Szenario, in dem ein Speicherkonto Benutzerdaten gespeichert, gibt es zwei typische entwurfmustern:


1\. Clients hochladen und Herunterladen von Daten über eine Front-End-Proxy-Dienst, in dem die Authentifizierung durchgeführt. Diese Front-End-Proxy-Dienst hat den Vorteil, dass die Überprüfung des Business Regeln, aber für große Datenmengen umfangreicher Transaktionen oder erstellen einen Dienst, der bei Bedarf entsprechend skalieren kann möglicherweise teure oder schwierig.

![SAS-Speicher-her-Proxy-service][sas-storage-fe-proxy-service]

2\. ein kompakten Dienstes authentifiziert den Client, je nach Bedarf, und klicken Sie dann generiert eine SAS. Nachdem der Client die SAS erhält, können sie direkt mit den Berechtigungen, die durch die SAS und für das Intervall zulässig, indem Sie die SAS definiert Ressourcen für Speicher-Konto zugreifen. Die SAS reduziert die Notwendigkeit der routing aller Daten über den Front-End-Proxydienst an.

![SAS-Anbieter Speicherdienst][sas-storage-provider-service]

Viele praktisches Dienste möglicherweise verwenden Sie eine Kombination aus den folgenden zwei Verfahren, je nach dem Szenario verbindet, mit einigen Daten verarbeitet und über die Front-End-Proxy überprüft werden, während andere Daten gespeichert und/oder direkt mit SAS gelesen werden.

Darüber hinaus müssen Sie einen SAS Authentifizierung das Quellobjekt in einem Kopiervorgang in bestimmten Szenarien verwenden:

- Beim Kopieren eines BLOBs in ein anderes Blob, der sich in einem anderen Speicherkonto befindet, müssen Sie einen SAS verwenden, mit das Quelle Blob authentifiziert. Mit Version 2015-04-05 können Sie optional eine SAS als auch das Ziel-Blob-Authentifizierung verwenden.
- Beim Kopieren einer Datei in einer anderen Datei, die in einem anderen Speicherkonto befindet, müssen Sie einen SAS verwenden, mit die Quelldatei authentifiziert. Mit Version 2015-04-05 können Sie optional eine SAS verwenden, mit die Zieldatei auch authentifiziert.
- Beim Kopieren eines BLOBs in einer Datei oder einer Datei in ein Blob, müssen Sie einen SAS verwenden, mit das Quellobjekt authentifiziert, selbst wenn die Quell- und Zielfelder Objekte innerhalb der gleichen Speicherkonto gespeichert sind.

## <a name="types-of-shared-access-signatures"></a>Typen von freigegebenen Access Signaturen

Version 2015-04-05 der Azure-Speicher führt eine neue Art von freigegebenen Access-Signatur, das Konto SAS. Sie können jetzt zwei Arten von freigegebenen Access Signaturen erstellen:

- **Konto SAS.** Dem Konto SAS Stellvertretungen Zugriff auf Ressourcen in einer oder mehreren der Speicherdienste. Alle Vorgänge, die über einen SAS zur Verfügung stehen auch über ein Konto SAS. Darüber hinaus können Sie mit dem Konto SAS, Zugriff auf Vorgänge delegieren, die für einen bestimmten Dienst, z. B. **Get-Set Diensteigenschaften** und **Erhalten Service Stats**gelten. Sie können auch Zugriffsrechte für Stellvertretung zum Lesen, schreiben und Löschvorgängen auf Blob-Container, Tabellen, Warteschlangen und Dateifreigaben, die mit einem Dienst SAS nicht zulässig sind. Finden Sie unter [ein Konto SAS bauen](https://msdn.microsoft.com/library/mt584140.aspx) ausführliche Informationen zum Erstellen von SAS-Konto-Token.

- **Dienst SAS.** Der Dienst SAS delegiert Zugriff auf eine Ressource in nur einem der Speicherdienste: der Dienst Blob, Warteschlange, Tabelle oder Datei. Finden Sie unter [Erstellen einer SAS Dienst](https://msdn.microsoft.com/library/dn140255.aspx) und dem [Dienst SAS Beispiele](https://msdn.microsoft.com/library/dn140256.aspx) ausführliche Informationen zu bauen Token SAS-Dienst.

## <a name="how-a-shared-access-signature-works"></a>Funktionsweise von eine freigegebenen Access-Signatur

Eine freigegebene Access-Signatur ist ein signierte URI, der verweist auf eine oder mehrere Speicherressourcen und enthält ein Token, die einen besonderen Satz von Abfrageparameter enthält. Das Token zeigt an, wie die Ressourcen vom Client zugegriffen werden können. Einer der Abfrageparameter, die Signatur, die SAS-Parameter ist und mit dem kontoschlüssel signiert. Diese Signatur wird von Azure-Speicher mit der SAS authentifiziert verwendet.

Hier ist ein Beispiel für einen SAS URI, mit den Ressourcen-URI und dem SAS Token: 

![SAS-Speicher-uri][sas-storage-uri]

Beachten Sie, dass das Token SAS eine Zeichenfolge, die auf dem Client generiert wird (siehe Abschnitt [SAS Beispiele](#sas-examples) unter für Codebeispielen). Das von der Speicher-Client-Bibliothek generierte SAS Token wird von Azure-Speicher in keiner Weise nicht nachverfolgt. Sie können eine unbegrenzte Anzahl von SAS Token clientseitig erstellen.

Wenn ein Client als Teil der Anforderung einer SAS URI Azure-Speicher bereitstellt, überprüft der Dienst die SAS-Parameter und die Signatur, um sicherzustellen, dass sie für die Authentifizierung der Anforderung gültig ist. Wenn Sie der Dienst, die überprüft, ob die Signatur ist gültig, und klicken Sie dann die Anforderung authentifiziert wird. Andernfalls wird die Anforderung mit dem Fehlercode 403 (Verboten) abgelehnt.


## <a name="shared-access-signature-parameters"></a>Parameter für freigegebene Access-Signatur

Das Konto SAS und Dienst SAS Token einige allgemeine Parameter enthalten, und auch ein paar Parameter akzeptieren, die unterscheiden.

### <a name="parameters-common-to-account-sas-and-service-sas-tokens"></a>Allgemeine Konto SAS Parameter und Dienst SAS Token

- **API-version** Ein optionaler Parameter, der angibt, die Speicher-Service-Version zu verwenden, um die Anfrage auszuführen.
- **Service-version** Ein erforderlicher Parameter, der angibt, die Speicher Service-Version zum Authentifizieren der Anforderung verwendet werden soll.
- **Startzeit.** Dies ist die Uhrzeit, an der die SAS gültig wird. Die Startzeit für eine Signatur gemeinsamen Zugriff ist optional. Wenn nicht angegeben, ist die SAS ab sofort. Muss mit einem speziellen UTC ("Z") h. 1994 in UTC (Coordinated Universal Time), ausgedrückt-11-05T13:15:30Z.
- **Der Ablaufzeit.** Dies ist die Zeit, nach der die SAS nicht mehr gültig ist. Bewährte Methoden empfiehlt sich, dass Sie eine Ablaufzeit für eine SAS angeben oder einer gespeicherten Zugriffsrichtlinie zuordnen. Muss mit einem speziellen UTC ("Z") h. 1994 in UTC (Coordinated Universal Time), ausgedrückt-11-05T13:15:30Z (Weitere siehe unten).
- **Berechtigungen.** Die Berechtigungen auf die SAS angegebenen anzugeben, welche Vorgänge der Client gegen die Speicherressource mit den SAS ausführen kann. Verfügbare Berechtigungen unterscheiden sich für ein Konto SAS und einem Dienst SAS.
- **IP.** Ein optionaler Parameter, die eine IP-Adresse oder einen Bereich von IP-Adressen von Azure angibt (Siehe den Abschnitt [Routing Sitzung Konfigurationszustand](../expressroute/expressroute-workflows.md#routing-session-configuration-state) für Express-Routing) aus dem Anfragen akzeptieren.
- **Protokoll.** Ein optionaler Parameter, der angibt, das Protokoll zulässig für eine Anforderung. Mögliche Werte sind sowohl in HTTPS HTTP (Https, http), also den Standardwert oder HTTPS nur (Https). Beachten Sie, dass HTTP nicht nur einen zulässigen Wert ist.
- **Signatur.** Die Signatur wird anhand der anderen Parameter als Teil Token angegeben, und klicken Sie dann verschlüsselt erstellt. Hiermit wird die SAS authentifiziert.

### <a name="parameters-for-an-account-sas-token"></a>Parameter für ein Konto SAS token

- **Dienst oder Dienste.** Ein Konto SAS kann den Zugriff auf eine oder mehrere der Speicherdienste vergeben. Beispielsweise können Sie ein Konto SAS erstellen, die Stellvertretungen des Zugriffs auf den Dienst Blob und den Dateinamen. Oder Sie können eine SAS, dass Stellvertretungen Zugriff auf alle vier (Blob, Warteschlange, Tabelle und Datei) services erstellen.
- **Speicher Ressourcentypen zur Verfügung.** Ein Konto SAS gilt eine oder mehrere Klassen Speicherressourcen, anstatt eine bestimmte Ressource an. Sie können ein Konto den Zugriff auf übertragen SAS erstellen:
    - Servicelevel-APIs, die für die Ressource Speicher Konto bezeichnet werden. Beispiele für **Eigenschaften der Get-Set-Dienst**, **Dienst Stats abrufen**und **Liste Container/Warteschlangen/Tabellen/Freigaben**.
    - Container Ebene APIs, die für jeden Dienst anhand der Containerobjekte bezeichnet werden: BLOB-Container, Warteschlangen, Tabellen und Dateifreigaben. Beispiele: **Container erstellen/löschen**, **Warteschlange erstellen/löschen**, **Tabelle erstellen/löschen**, **Erstellen/Löschen freigeben**und **Liste Blobs/Dateien und Verzeichnisse**.
    - Auf Objektebene APIs, die Blobs, Warteschlangennachrichten, Tabelle Elemente und Dateien bezeichnet werden. Beispielsweise **Blob setzen**, **Abfrage Entität**, **Nachrichten erhalten**und **Datei erstellen**.

### <a name="parameters-for-a-service-sas-token"></a>Parameter für ein Dienst SAS token

- **Speicherressource.** Für die Sie den Zugriff mit einem Dienst vergeben können Speicherressourcen gehören SAS:
    - Container und blobs
    - Dateifreigaben und Dateien
    - Warteschlangen
    - Tabellen und Bereiche der Tabelle Einheiten.

## <a name="examples-of-sas-uris"></a>Beispiele für SAS-URIs

Hier ist ein Beispiel für einen Dienst bietet SAS-URI Lese- und Schreibberechtigungen in ein Blob ein. Die Tabelle unterteilt jeden Teil des URIS zu verstehen, wie sie zu der SAS beiträgt aus:

    https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D

Namen|SAS Anteil|Beschreibung
---|---|---
BLOB-URI|https://myAccount.BLOB.Core.Windows.NET/sascontainer/sasblob.txt |Die Adresse des Blob. Beachten Sie, dass mit HTTPS wird dringend empfohlen.
Version von services|PAP = 2015-04-05|Für Speicher Version 2012-02-12 Services und später für diesen Parameter zeigt die zu verwendende Version an.
Startzeit|Mo = 2015-04-29T22 % 3A18 % 3A26Z|In UTC-Zeit angegeben. Wenn Sie die SAS sofort gültig sein möchten, lassen Sie die Startzeit.
Ablaufzeit|SE = 2015-04-30T02 % 3A23 % 3A26Z|In UTC-Zeit angegeben.
Ressource|SR = b|Die Ressource ist ein Blob.
Berechtigungen|SP = Rw|Die Berechtigungen, die durch die SAS Read (r) einbeziehen, und Schreiben (w).
IP-Bereich|SIP = 168.1.5.60-168.1.5.70|Die von IP-Adressen, aus denen eine Anforderung akzeptiert wird.
Protokoll|SPR = Https|Nur mit HTTPS Anfragen zulässig sind.
Signatur|SIG = Z "% 2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk" 3D|Zugriff auf die Blob-Authentifizierung verwendet. Die Signatur ist ein HMAC über eine Zeichenfolge-zu-Zeichen und Schlüssel mit dem SHA256-Algorithmus berechnet, und klicken Sie dann unter Verwendung von Base64-Codierung codiert.

Und so sieht ein Beispiel für ein Konto SAS, die die gleichen allgemeinen Parameter auf dem Token verwendet. Da diese Parameter über beschrieben sind, werden sie hier nicht beschrieben. Nur die Parameter, die für das Konto aus, das SAS in der folgenden Tabelle beschriebenen spezifisch sind.

    https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B

Namen|SAS Anteil|Beschreibung
---|---|---
Ressourcen-URI|https://myAccount.BLOB.Core.Windows.NET/?RESTYPE=Service&Comp=Properties|Der Blob-Endpunkt, mit den Parametern zum Abrufen von Eigenschaften (wenn mit GET genannt) oder Festlegen von Eigenschaften (wenn mit aufgerufen wird).
Services|ss = bf|Die SAS gilt die Dienste Blob und Datei
Ressourcentypen|SRT s =|Die SAS gilt Servicelevel Vorgänge aus.
Berechtigungen|SP = Rw|Die Berechtigungen erteilen Zugriff zum Lesen und Schreiben Vorgänge an.  

Vorausgesetzt, dass die Berechtigungen auf das Dienstalter beschränkt sind, sind barrierefreien Vorgänge mit dieser SAS **Erhalten BLOB-Diensteigenschaften** (gelesen) und **BLOB-Diensteigenschaften festlegen** (Schreiben). Jedoch konnte mit einer anderen Ressource URI das gleiche SAS Token auch verwendet werden den Zugriff auf **Erste BLOB-Dienst Stats** (gelesen) übertragen.

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>Steuern von einem SAS mit einer gespeicherten Zugriffsrichtlinie ##

Eine Signatur gemeinsamen Zugriff kann zwei Arten ausführen:

- **Ad-hoc-SAS:** Beim Erstellen einer ad-hoc-SAS Startzeit, Ablaufzeit und Berechtigungen für die SAS werden alle angegeben auf dem SAS-URI (oder impliziert, in den Fall, in dem Startzeit ausgelassen wird). Dieses Typs SA-möglicherweise als ein SAS-Konto oder einem Dienst SAS erstellt werden.

- **SAS mit gespeicherten Zugriffsrichtlinie:** Eine gespeicherte Zugriffsrichtlinie für eine Ressource Container - Blob Container, Tabelle, Warteschlange oder Dateifreigabe - definiert ist und zum Verwalten der Einschränkungen für eine oder mehrere gemeinsamen Zugriff Signaturen verwendet werden kann. Wenn Sie eine gespeicherte Zugriffsrichtlinie einem SAS zuordnen, erbt die SAS Einschränkungen - Startzeit, Ablaufzeit und Berechtigungen – für die gespeicherte Zugriffsrichtlinie definiert ist.

>[AZURE.NOTE] Derzeit muss ein Konto SAS eine ad-hoc-SAS sein. Gespeicherte Access, die Richtlinien für Konto SAS noch nicht unterstützt werden.

Der Unterschied zwischen den beiden Formularen ist wichtig für ein Key Szenario: Sperrung. Eine SAS ist einer URL, sodass alle Personen, die die SAS erhält, unabhängig davon, wer zunächst angefordert verwenden kann. Wenn eine SAS öffentlich veröffentlicht wird, kann es von allen Benutzern in der Welt verwendet werden. Eine SAS, die verteilt ist ist gültig, bis eine der vier Punkte geschieht:

1.  Die auf die SAS angegebene Ablaufzeit erreicht ist.
2.  Klicken Sie auf die gespeicherte Zugriffsrichtlinie optimiert die SAS angegebene Zeitspanne Ablauf erreicht ist (Wenn eine gespeicherte Zugriffsrichtlinie verwiesen wird, und wenn es sich um eine Ablaufzeit angibt). Dies kann entweder auftreten, da das Intervall abgelaufen ist, oder Sie die gespeicherten Zugriffsrichtlinie damit einem Ablaufdatum in der Vergangenheit, also eine Methode, um die SAS widerrufen geändert haben.
3.  Die gespeicherte Zugriffsrichtlinie optimiert die SAS wird welche ist eine weitere Möglichkeit, die SAS widerrufen, gelöscht. Beachten Sie, dass, wenn Sie die gespeicherten Zugriffsrichtlinie mit demselben Namen neu erstellen, alle vorhandene SAS Token erneut gültige entsprechend den Berechtigungen, die gespeicherten Zugriffsrichtlinie (vorausgesetzt, die nicht den Ablaufzeit auf die SAS verstrichen) zugeordnet. Wenn Sie die SAS widerrufen festlegen möchten, müssen Sie einen anderen Namen verwenden, wenn Sie die Zugriffsrichtlinie mit einem Ablaufdatum in der Zukunft neu erstellen.
4.  Der kontoschlüssel, der zum Erstellen der SAS verwendet wurde neu erstellt.  Beachten Sie, dass Hiermit werden alle Komponenten der Anwendung, die mit diesem Konto-Key fehlschlägt authentifiziert, bis sie aktualisiert werden, um entweder die anderen gültigen Konto-Taste oder die Taste neu generierten Konto verwenden.

>[AZURE.IMPORTANT] URI eine freigegebenen Access-Signatur zugeordnet ist, die zum Erstellen der Signatur verwendete Konto-Taste und die zugehörigen Speicherorten eine Zugriffsrichtlinie, (falls vorhanden). Wenn keine gespeicherten Zugriffsrichtlinie angegeben ist, ist die einzige Möglichkeit zum Widerrufen einer freigegebenen Access-Signatur Konto Key ändern.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>Aus einer Clientanwendung mit einem SAS-Authentifizierung

Ein Client, die im Besitz eines SA-ist können die SAS zum Authentifizieren einer Anforderung mit einem Speicherkonto, für die sie nicht die Tasten Konto besitzen. Eine SAS kann in einer Verbindungszeichenfolge enthalten oder direkt aus der entsprechenden oder Methode verwendet werden.

### <a name="using-a-sas-in-a-connection-string"></a>Verwenden einer SAS in einer Verbindungszeichenfolge

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>Verwenden einer SAS in einen Konstruktor oder eine Methode

Viele Speicher Client Bibliothek Konstruktoren und überladene Methoden bieten SAS-Parameter auf.  

Beispielsweise wird hier einen SAS URI verwendet einen Bezug auf einen Blockblob zu erstellen. Die SAS bietet die einzigen Anmeldeinformationen für die Anforderung erforderlich. Block Blob Bezug wird für einen Schreibvorgang verwendet:

    string sasUri = 
        "https://storagesample.blob.core.windows.net/sample-container/sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D&se=2016-10-18T21%3A51%3A37Z&sp=rcw"
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

    // Create operation: Upload a blob with the specified name to the container.
    // If the blob does not exist, it will be created. If it does exist, it will be overwritten.
    try
    {
        MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        msWrite.Position = 0;
        using (msWrite)
        {
            await blob.UploadFromStreamAsync(msWrite);
        }

        Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        if (e.RequestInformation.HttpStatusCode == 403)
        {
            Console.WriteLine("Create operation failed for SAS {0}", sasUri);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }


## <a name="best-practices-for-using-sas"></a>Bewährte Methoden für die Verwendung von SAS

Wenn Sie in Ihrer Anwendung gemeinsamen Zugriff Signaturen verwenden, müssen Sie zwei mögliche Risiken bewusst sein:

- Wenn eine SAS offengelegt werden, kann es durch jeden verwendet werden, die, erhält die potenziell zu Ihrem Speicherkonto beeinträchtigen kann.
- Wenn eine SAS zur Verfügung gestellt eine Clientanwendung läuft ab und die Anwendung kann nicht zum Abrufen eines neuen SAS aus dem Dienst und dann der Anwendungsfunktionalität möglicherweise beeinträchtigt werden.  

Die folgenden Informationen zur Verwendung von freigegebenen Access Signaturen helfen Ihnen diese Risiken abzuwägen.

1. **Immer HTTPS verwenden** , zum Erstellen einer SAS oder einem SAS verteilen.  Wenn eine SAS über HTTP übertragen und abgefangen wird, werden Angreifer Durchführung einer Mann in zweiter Angriffen lesen die SAS und verwenden sie dann wie der gewünschte Benutzer potenziell vertrauliche Daten beeinträchtigen oder das Beschädigung von Daten durch den Benutzer bösartiger sein kann, können.
2. **Verwiesen Sie gespeicherte Access-Richtlinien mögliche werden.** Gespeicherte Access-Richtlinien bieten Ihnen die Option, um Berechtigungen zu widerrufen, ohne die Speicher Konto Tasten neu erstellen.  Festlegen des Ablaufdatums auf diese eine sehr viel Zeit (oder Infinite) sein, und stellen Sie sicher, dass es regelmäßig aktualisiert wird, um weiter weg in die Zukunft.
3. **Verwenden Sie übrige Ablaufzeiten auf einer ad-hoc-SAS an.** Auf diese Weise selbst wenn eine SAS versehentlich, gefährdet wird werden nur realisierbar für eine kurze Zeitdauer. Diese Methode ist besonders wichtig, wenn Sie eine gespeicherte Zugriffsrichtlinie verweisen können. Dieses Verfahren können auch die Menge der Daten zu beschränken, die in einer Blob geschrieben werden können, indem Sie die Zeit, die darauf hochladen zur Verfügung.
4. **Haben Sie Clients automatisch erneuert wird die SAS bei Bedarf an.** Clients sollten die SAS auch vor der erwarteten Ablauf erneuern, um Zeit für Wiederholungsversuche zulassen, wenn der Dienst bereitstellen die SAS nicht verfügbar ist.  Wenn Ihre SAS vorgesehen ist nicht erforderlich, möglicherweise nicht erwartungsgemäß die SAS erneuert werden und Sie dann für eine kleine Anzahl von sofort, kurzlebige Vorgänge innerhalb der angegebene Ablauf Zeitraum abgeschlossen sein soll erwartet werden, verwendet werden.  Jedoch, wenn Sie Client, die regelmäßig Anfragen über SAS vornehmen verfügen, ist, kommt klicken Sie dann die Möglichkeit, Ablauf ins Spiel.  Im Vordergrund besteht darin, die Notwendigkeit der SAS kurzlebig (wie bereits erwähnt) sind abzuwägen mit müssen Sie sicherstellen, dass der Client Erneuerung Frühester anfordert genügend Unterbrechung aufgrund der Ablauf vor dem erfolgreiche Erneuerung SAS zu vermeiden.
5. **Achten Sie darauf, dass mit SAS beginnt.** Wenn Sie die Startzeit für eine SAS auf **jetzt**dann aufgrund Uhr Schiefe (Unterschiede bei aktuelle Uhrzeit nach unterschiedlichen Computern) festlegen, möglicherweise Fehler zeitweise in den ersten Minuten beobachtet werden.  Im Allgemeinen Festlegen der Startzeit vor mindestens 15 Minuten werden oder nicht festgelegt es überhaupt, welche werden sofort in allen Fällen gültig erleichtern.  Im Allgemeinen gilt auch für Ablaufzeit als auch: Denken Sie daran, dass Sie bis zu 15 Minuten Zeit in Richtung auf eine beliebige Anforderung Schiefe beobachten können.  Beachten Sie die maximale Dauer für ein, die nicht mit eine gespeicherten Zugriffsrichtlinie verweist SAS beträgt 1 Stunde und alle Richtlinien angeben Laufzeit von mehr als, tritt ein Fehler für eine Version REST vor 2012-02-12-Clients.
6.  **Seien Sie spezifisch für die Ressource zugegriffen werden.** Typische aus Sicherheitsgründen ist einen Benutzer mit den minimalen erforderlichen Berechtigungen bereitstellen.  Wenn ein Benutzer nur Lesezugriff für eine einzelne Entität benötigt, erteilen Sie ihnen Lesezugriff auf die Einheit und nicht lesen/schreiben/löschen Zugriff auf alle Elemente.  Auf diese Weise können, Risiko der SA-gefährdet wird, wie die SAS Stromverbrauch in die Hände eines Angreifers hat.
7.  **Grundlegendes zu, dass es sich bei Ihrem Konto bei jeder Verwendung, einschließlich, die erledigt mit SAS Abrechnung wird.** Wenn Sie Schreibzugriff auf ein Blob bereitstellen, kann der Benutzer auswählen, um ein Blob 200 GB hochzuladen.  Wenn Sie diese ebenfalls Lesezugriff erteilt haben, können herunterladen 10 Mal, damit verbundenen 2TB in Ausgang Kosten für Sie auch.  Bieten Sie erneut, eingeschränkte Berechtigungen, um zu verringern des Potenzials autorisierte Benutzer ein.  Verwenden Sie kurzlebige SAS verringern dieses Risiko (aber beachten Sie Zeit auf die Anfangsuhrzeit Schiefe) ein.
8.  **Überprüfen von Daten mithilfe von SAS geschrieben.** Beachten Sie bei eine Clientanwendung Daten in Ihr Speicherkonto schreibt, beachten Sie, dass es können Probleme mit diesen Daten werden. Wenn die Anwendung erfordert, dass die Daten werden überprüft oder autorisiert, bevor er verwendet werden kann, sollten Sie diese Validierung durchführen, nachdem die Daten geschrieben werden und bevor sie von der Anwendung verwendet wird. Diese Methode bietet Schutz auch vor Beschädigung oder bösartiger von Daten von einem Benutzer, der die SAS ordnungsgemäß erworben oder von einem Benutzer eine verlorene SAS ausnutzen to your Account geschrieben werden.
9. **Verwenden Sie nicht immer SAS ein.** In einigen Fällen empfiehlt sich Risiko im Zusammenhang mit einem bestimmten Vorgang für Ihr Speicherkonto SAS jedoch.  Für Operationen, erstellen Sie einen mittleren Ebene-Dienst, der mit Ihrem Speicherkonto schreibt nach Durchführung Business Regel Überprüfung, Authentifizierung und Überwachung. Manchmal ist es auch zum Verwalten des Zugriffs auf andere Weise einfacher. Beispielsweise, wenn Sie alle Blobs in einem Container öffentlich lesbar zu machen möchten, umso den Container Öffentlichkeit, anstatt, die eine SAS auf jeder Client für Access.
10. **Verwenden Sie Speicher Analytics um zu Ihrer Anwendung zu überwachen.** Protokollierung und Kennzahlen können Sie eine beliebige Sammlung Fehlern bei der Authentifizierung aufgrund von einem Ausfall in Ihrem Anbieter-Dienst SAS oder mit dem unbeabsichtigtes Entfernen einer gespeicherten Zugriffsrichtlinie beobachten. Finden Sie weitere Informationen der [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) .

## <a name="sas-examples"></a>Beispiele für SAS

Nachstehend sind einige Beispiele für beide Arten von freigegebenen Access Signaturen Konto SAS und SAS service.

Um diese Beispiele ausführen zu können, müssen Sie herunterladen, und diese Pakete verweisen:

- [Azure-Speicher-Client-Bibliothek für .NET](http://www.nuget.org/packages/WindowsAzure.Storage), Version 6.x oder höher (SAS-Konto verwenden zu können).
- [Azure-Konfigurations-Manager](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

Finden Sie weitere Beispiele, die zeigen, wie Sie erstellen und Testen einer SAS [Azure Codebeispielen für Speicher](https://azure.microsoft.com/documentation/samples/?service=storage)aus.

### <a name="example-create-and-use-an-account-sas"></a>Beispiel: Erstellen Sie und verwenden Sie ein Konto SAS

Im folgenden Code wird erstellt ein Konto SAS, die für die Dienste Blob und Datei gültig ist, und erhalten den Kunden aus, Berechtigungen lesen, schreiben und Listenberechtigungen Servicelevel APIs Zugriff auf. Das Konto SAS schränkt das Protokoll auf HTTPS, damit die Anforderung mit HTTPS ausgeführt werden muss.

    static string GetAccountSASToken()
    {
        // To create the account SAS, you need to use your shared key credentials. Modify for your account.
        const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

        // Create a new access policy for the account.
        SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
            {
                Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
                Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
                ResourceTypes = SharedAccessAccountResourceTypes.Service,
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
                Protocols = SharedAccessProtocol.HttpsOnly
            };

        // Return the SAS token.
        return storageAccount.GetSharedAccessSignature(policy);
    }

Um das Konto SAS auf Servicelevel APIs für den Blob-Dienst zuzugreifen, erstellen Sie ein Blob-Client-Objekt mithilfe der-SAS- und den Endpunkt des BLOB-Speicher für Ihr Speicherkonto ein.

    static void UseAccountSAS(string sasToken)
    {
        // Create new storage credentials using the SAS token.
        StorageCredentials accountSAS = new StorageCredentials(sasToken);
        // Use these credentials and the account name to create a Blob service client.
        CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
        CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

        // Now set the service properties for the Blob client created with the SAS.
        blobClientWithSAS.SetServiceProperties(new ServiceProperties()
        {
            HourMetrics = new MetricsProperties()
            {
                MetricsLevel = MetricsLevel.ServiceAndApi,
                RetentionDays = 7,
                Version = "1.0"
            },
            MinuteMetrics = new MetricsProperties()
            {
                MetricsLevel = MetricsLevel.ServiceAndApi,
                RetentionDays = 7,
                Version = "1.0"
            },
            Logging = new LoggingProperties()
            {
                LoggingOperations = LoggingOperations.All,
                RetentionDays = 14,
                Version = "1.0"
            }
        });

        // The permissions granted by the account SAS also permit you to retrieve service properties.
        ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
        Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
        Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
        Console.WriteLine(serviceProperties.HourMetrics.Version);
    }

### <a name="example-create-a-stored-access-policy"></a>Beispiel: Erstellen einer gespeicherten Zugriffsrichtlinie

Im folgende Code erstellt eine gespeicherten Zugriffsrichtlinie eines Containers. Sie können die Zugriffsrichtlinie verwenden, um Einschränkungen für einen Dienst SAS auf den Container oder deren Blobs anzugeben.

    private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
    {
        // Create a new shared access policy and define its constraints.
        // The access policy provides create, write, read, list, and delete permissions.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request. 
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
                SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
        };

        // Get the container's existing permissions.
        BlobContainerPermissions permissions = await container.GetPermissionsAsync();

        // Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        await container.SetPermissionsAsync(permissions);
    }

### <a name="example-create-a-service-sas-on-a-container"></a>Beispiel: Erstellen Sie einen SAS-Dienst an einen container

Mit dem folgende Code erstellt ein SAS eines Containers. Wenn Sie der Namen einer vorhandenen gespeicherten Zugriffsrichtlinie bereitgestellt wird, ist die Richtlinie die SAS zugeordnet. Wenn keine gespeicherten Zugriffsrichtlinie bereitgestellt wird, erstellt der Code in einer Ad-hoc-SAS für den Container.

    private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
    {
        string sasContainerToken;

        // If no stored policy is specified, create a new access policy and define its constraints.
        if (storedPolicyName == null)
        {
            // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and 
            // to construct a shared access policy that is saved to the container's shared access policies. 
            SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
            {
                // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request. 
                // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
                Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
            };

            // Generate the shared access signature on the container, setting the constraints directly on the signature.
            sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

            Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
            Console.WriteLine();
        }
        else
        {
            // Generate the shared access signature on the container. In this case, all of the constraints for the
            // shared access signature are specified on the stored access policy, which is provided by name.
            // It is also possible to specify some constraints on an ad-hoc SAS and others on the stored access policy.
            sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

            Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
            Console.WriteLine();
        }

        // Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }


### <a name="example-create-a-service-sas-on-a-blob"></a>Beispiel: Erstellen eines Diensts SAS auf ein blob

Mit dem folgende Code erstellt ein Blob ein SAS. Wenn Sie der Namen einer vorhandenen gespeicherten Zugriffsrichtlinie bereitgestellt wird, ist die Richtlinie die SAS zugeordnet. Wenn keine gespeicherten Zugriffsrichtlinie bereitgestellt wird, wird der Code eine Ad-hoc-SAS auf dem Blob erstellt.

    private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
    {
        string sasBlobToken;

        // Get a reference to a blob within the container.
        // Note that the blob may not exist yet, but a SAS can still be created for it.
        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

        if (policyName == null)
        {
            // Create a new access policy and define its constraints.
            // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and 
            // to construct a shared access policy that is saved to the container's shared access policies. 
            SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
            {
                // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request. 
                // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
                Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
            };

            // Generate the shared access signature on the blob, setting the constraints directly on the signature.
            sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

            Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
            Console.WriteLine();
        }
        else
        {
            // Generate the shared access signature on the blob. In this case, all of the constraints for the
            // shared access signature are specified on the container's stored access policy.
            sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

            Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
            Console.WriteLine();
        }

        // Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }





## <a name="conclusion"></a>Abschluss ##

Freigegebene Access Signaturen eignen sich für die Bereitstellung von Clients, auf denen keine der kontoschlüssel eingeschränkter Berechtigungen bei Ihrem Speicherkonto.  Als solche sind sie ein wesentlicher Bestandteil der Sicherheitsmodell für jede andere Anwendung Azure-Speicher.  Wenn Sie die hier aufgeführten best Practices folgen, können Sie SAS verwenden, um größere Flexibilität des Zugriffs auf Ressourcen in Ihrem Speicherkonto bereitzustellen, ohne dass die Sicherheit Ihrer Anwendung beeinträchtigt.

## <a name="next-steps"></a>Nächste Schritte ##

- [Erste Schritte mit auf Windows Azure-Datei-Speicher](storage-dotnet-how-to-use-files.md)
- [Anonyme Lesezugriff auf Container und Blobs verwalten](storage-manage-access-to-resources.md)
- [Delegieren des Zugriffs mit einer freigegebenen Access-Signatur](http://msdn.microsoft.com/library/azure/ee395415.aspx)
- [Einführung in die Tabelle und Warteschlange SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)


[sas-storage-fe-proxy-service]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png
[sas-storage-provider-service]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png
[sas-storage-uri]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png