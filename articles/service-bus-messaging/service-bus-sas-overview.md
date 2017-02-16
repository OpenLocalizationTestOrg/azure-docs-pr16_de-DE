<properties
    pageTitle="Freigegebene Access Signaturen Übersicht | Microsoft Azure"
    description="Was Access Signaturen freigegeben sind, wie sie funktionieren, und wie sie Knoten, PHP und c# verwendet."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Freigegebene Access-Signaturen

*Freigegebene Access-Signaturen* () Werden die primäre Sicherheit dafür, Dienstbus, einschließlich der Ereignis Hubs, vermittelte messaging (Warteschlangen und Themen) und messaging weitergeleitet. In diesem Artikel wird erläutert, freigegeben Access Signaturen, wie sie funktionieren und wie sie auf eine Weise Plattform-unabhängige verwendet.

## <a name="overview-of-sas"></a>Übersicht über SA-

Freigegebene Access Signaturen handelt es sich um ein Authentifizierungsmechanismus basierend auf secure Hashes SHA-256 oder URIs. SAS ist eine extrem leistungsfähige Methode, die von allen Dienstbus-Diensten verwendet wird. Tatsächlich SAS besteht aus zwei Komponenten: einer *Zugriffsrichtlinie freigegeben* und eine *Signatur für freigegeben Access* (häufig als *token*bezeichnet).

Ausführliche Informationen zu freigegebenen Access Signaturen mit Dienstbus finden Sie in der [Access-Signatur freigegebene Authentifizierung mit Dienstbus](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Freigegebene Zugriffsrichtlinie

Wichtiger Hinweis zum Verständnis von SAS ist, dass alles mit einer Richtlinie beginnt. Bei jeder Richtlinie können Sie entscheiden, auf drei Angaben: **Name**, **Umfang**und **Berechtigungen**. Den **Namen** handelt es sich um; einen eindeutigen Namen in diesem Bereich. Sie können der Umfang ist es ausreichend: es den URI für die betreffende Ressource ist. Für einen Namespace Dienstbus ist der Bereich den vollqualifizierten Domänennamen (FQDN), z. B. `https://<yournamespace>.servicebus.windows.net/`.

Die verfügbaren Berechtigungen für eine Richtlinie sind weitgehend sofort verständlich:

  + Senden
  + Abhören
  + Verwalten

Nachdem Sie die Richtlinie erstellt haben, wird er einen *Primärschlüssel* und einen *Sekundärschlüssel*zugewiesen. Hierbei handelt es sich um kryptografisch Tasten. Nicht wieder verloren oder verloren gehen sie – sie können zwar immer im [Azure-Portal][]zur Verfügung. Sie können mithilfe einer der generierten Schlüssel und können Sie diese jederzeit wiederherstellen. Jedoch, wenn Sie erneut generieren oder Ändern des Primärschlüssels in der Richtlinie, alle erstellt daraus freigegeben Access Signaturen ungültig.

Wenn Sie einen Namespace Dienstbus erstellen, eine Richtlinie für den gesamten Namespace mit dem Namen **RootManageSharedAccessKey**automatisch erstellt wird, und diese Richtlinie verfügt über alle Berechtigungen. Sie Anmelden nicht als **Stamm**, damit diese Richtlinie nicht verwenden, es sei denn, es gibt einen wirklich guten Grund. Sie können weitere Richtlinien auf der Registerkarte " **Konfigurieren** " für den Namespace im Portal erstellen. Es ist wichtig, beachten Sie, dass eine einzelne Ebene in Dienstbus Struktur (Namespace, Warteschlange, Ereignis-Hub usw.) bis zu 12 beigefügt Richtlinien kann nur haben.

## <a name="shared-access-signature-token"></a>Freigegebene Access Signatur (Token)

Die Richtlinie selbst ist nicht das Access-Token für Dienstbus. Es ist das Objekt, in dem das Access-Token mit der primären oder sekundären Schlüssel generiert wird-. Das Token wird ausgelöst, indem Sie sorgfältig Erstellen einer Zeichenfolge in folgendem Format:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Wo `signature-string` ist der SHA-256 Hash für den Bereich des das Token (**Umfang** wie im vorherigen Abschnitt beschrieben) mit einer CRLF angefügt und einem Ablaufdatum (in Sekunden seit der Epoche: `00:00:00 UTC` auf 1. Januar 1970).

Der Hash sieht ähnlich wie den folgenden Pseudocode aus und gibt 32 Byte.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Nicht gehashten Werte sind in der Zeichenfolge **SharedAccessSignature** , sodass der Empfänger Sie den Hashwert mit denselben Parametern berechnen können, um sicherzustellen, dass es das gleiche Ergebnis zurück. Der URI Gibt den Bereich, und der Name des Schlüssels bezeichnet die Richtlinie zum Berechnen des Hash verwendet werden. Dies ist wichtig aus Sicherheitsgründen zu empfehlen. Wenn die Signatur, die nicht dem entsprechen der Empfänger (Dienstbus) berechnet werden, wird der Zugriff verweigert. Sie können zu diesem Zeitpunkt sicher sein, dass der Absender Zugriff auf den Schlüssel haben und die in der Richtlinie angegebenen Rechte gewährt werden sollte.

## <a name="generating-a-signature-from-a-policy"></a>Generieren von einer Signatur aus einer Richtlinie

Wie gehen Sie tatsächlich Code dazu? Sehen wir uns mit einer Reihe von Drittanbietern.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Verwenden die Access-Signatur freigegeben (auf HTTP-Ebene)
 
Nun wissen, wie freigegebene Access Signaturen für alle Personen in Dienstbus erstellen, sind Sie bereit sind, eine HTTP-POST auszuführen:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Beachten Sie, dass dies für alle Elemente funktioniert. Sie können für eine Warteschlange, Thema, Abonnement, Ereignis Hub oder Relay SAS erstellen. Wenn Sie eine Publisher-Identität für Ereignis Hubs verwenden, Sie einfach Anfügen `/publishers/< publisherid>`.

Wenn Sie einen Absender oder eine Client ein Token SAS geben, sie brauchen nicht die Taste direkt, und sie können nicht stornieren, den Hash, um es zu erhalten. Daher müssen Sie Kontrolle über was sie zugreifen können, und wie lange. Wichtiger Hinweis zu merken ist, wenn Sie den Primärschlüssel in der Richtlinie ändern, alle erstellt daraus freigegeben Access Signaturen ungültig gemacht wird.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Verwenden die Access-Signatur freigegeben (AMQP Ebene)

Im vorherigen Abschnitt wurde gezeigt, wie das SAS Token mit einer HTTP POST-Anforderung verwenden Sie zum Senden von Daten mit dem Dienst. Wie Sie wissen, können Sie zugreifen Dienstbus mithilfe der erweiterte Nachricht Queuing-Protokoll (AMQP), die die bevorzugte Protokoll Gründen der Leistung in vielen Szenarios verwendet wird. Die Verwendung der SAS token mit AMQP wird im Dokument [AMQP Claim-Based Security Version 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) beschrieben, die heute für den Entwurf seit 2013 jedoch durch Azure gut unterstützte ist.

Bevor Sie beginnen, um Daten zu Dienstbus senden, muss Herausgeber das SAS Token innerhalb einer Nachricht AMQP an einen klar definierten AMQP Knoten mit dem Namen **$cbs** senden (sie sichtbar als "Inhalte" Warteschlange zu erfassen, und überprüfen alle SAS-Token vom Dienst verwendet). Herausgeber muss das **ReplyTo** -Feld innerhalb der Nachricht AMQP angegeben werden; Hierbei handelt es sich um den Knoten in dem der Dienst für den Herausgeber, mit dem Ergebnis der token Validierung (eine einfache Anforderung/Antwort Muster zwischen Publisher und Service) zu antworten. Dieser Knoten Antworten wird erstellt "im laufenden Betrieb," zu "dynamische Erstellung des remote-Knotens" sprechen, wie in der AMQP 1.0-Spezifikation beschrieben. Sichergestellt, dass das SAS Token gültig ist, können Sie die Publisher weiter und zum Senden von Daten mit dem Dienst starten.

Die folgenden Schritte zeigen, wie das SAS Token mit AMQP Protokoll mithilfe der [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) Bibliothek zu senden. Dies ist sinnvoll, wenn Sie die offizielle Service Bus SDK verwendet werden kann (beispielsweise auf WinRT, .net Compact Framework, .net Framework Micro und Schwarzweißdruck) in C Entwicklung\#. Diese Bibliothek ist natürlich sinnvoll, besser zu verstehen, wie anspruchsbasierte Sicherheit auf oberster Ebene AMQP funktioniert, wie Sie gesehen haben, deren Funktionsweise auf HTTP-Ebene (mit einer HTTP POST-Anforderung und in der Kopfzeile "Autorisierung" gesendeten SAS Token). Wenn Sie solche umfassende Kenntnisse darüber AMQP nicht benötigen, können Sie die offizielle Service Bus SDK verwenden, mit .net Framework-bereit, mit denen sie ausgeführt werden.

### <a name="c35"></a>C & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Die `PutCbsToken()` Methode empfängt die *Verbindung* (AMQP Verbindung Klasseninstanz bereitgestellt vom [AMQP .NET Lite Library](https://github.com/Azure/amqpnetlite)), die die TCP-Verbindung mit dem Dienst und Parameters *SasToken* das SAS Token senden darstellt. 

> [AZURE.NOTE] Es ist wichtig, dass die Verbindung mit **SASL Authentifizierungsmethode legen Sie auf externe** (und nicht den standardmäßigen nur mit Benutzername und Kennwort verwendet, wenn Sie nicht das SAS Token senden müssen) erstellt wird.

Als Nächstes erstellt Herausgeber zwei AMQP Links für das SAS Token senden und Empfangen von der Antwort (token Überprüfung Ergebnis) aus dem Dienst.

Die Nachricht AMQP enthält eine Reihe von Eigenschaften und Weitere Informationen als eine einfache Meldung an. Das SAS-Token ist Hauptteil der Nachricht (mit dem entsprechenden Konstruktor). Die Eigenschaft **"ReplyTo"** festgelegt ist, auf den Knotennamen für den Empfang von des Ergebnis der Validierung auf den Hyperlink der Empfänger (Sie können den Namen ändern, wenn Sie möchten, und es vom Dienst dynamisch erstellt werden werden). Die letzten drei Anwendung/benutzerdefinierten Eigenschaften werden vom Dienst verwendet, um anzugeben, welche Art des Vorgangs muss es ausführen. Wie die CBS Entwurfsspezifikation beschrieben, müssen sie den **Namen des Vorgangs** ("sich-Token") die **Art der Token** (in diesem Fall ein "servicebus.windows.net:sastoken"), und der **"Name" der Zielgruppe** , für die das Token (die gesamte Entität) gilt.

Nach dem Senden des SAS Tokens auf den Link Absender, muss die Antwort auf den Link Empfänger Herausgeber gelesen werden. Die Antwort ist eine einfache AMQP-Nachricht mit einer Anwendungseigenschaft mit dem Namen **"Status-Code"** , die dieselben Werte wie eine HTTP-Statuscode enthalten kann. 

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Service Bus REST-API-Referenz](https://msdn.microsoft.com/library/azure/hh780717.aspx) für Weitere Informationen zu den folgenden Token SAS gebotenen können.

Weitere Informationen zu Dienstbus Authentifizierung finden Sie unter [Dienstbus Authentifizierung und Autorisierung](service-bus-authentication-and-authorization.md). 

Wenn Sie weitere Beispiele SA-in c# und JavaScript sind in [diesem Blogbeitrag](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Azure-portal]: https://portal.azure.com