<properties
 pageTitle="Developer Guide - Steuern des Zugriffs auf IoT Hub | Microsoft Azure"
 description="Azure IoT Hub Entwicklertools Leitfaden - zum Steuern des Zugriffs auf IoT Hub und Verwalten der Sicherheit"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Steuern des Zugriffs auf IoT-Hub

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden die Optionen zum Sichern Ihrer IoT-Hub an. IoT Hub verwendet *Berechtigungen* zum Zugriff auf jede IoT Hub Endpunkte gewähren. Berechtigungen beschränken Sie den Zugriff auf einen IoT Hub basierend auf Funktionalität.

In diesem Artikel werden:

- Anderen Berechtigungen können Sie eine app Gerät oder Back-End-Hub Ihrer IoT Zugriff auf gewähren.
- Der Authentifizierungsprozess und der Token verwendet, um Berechtigungen zu überprüfen.
- Informationen zum Bereich von Anmeldeinformationen ein, um den Zugriff auf bestimmte Ressourcen zu beschränken.
- IoT Hub Unterstützung für das x. 509-Zertifikate.
- Benutzerdefiniertes Gerät Authentifizierungsmechanismen, die vorhandene Gerät Identität Register oder Authentifizierung Phishingversuchen verwenden.

### <a name="when-to-use"></a>Verwenden von

Sie müssen die entsprechende Berechtigungen für den Zugriff auf die Endpunkte IoT Hub. Beispielsweise muss ein Gerät ein Token mit Sicherheitsanmeldeinformationen zusammen mit jeder Nachricht, die an IoT Hub beinhalten.

## <a name="access-control-and-permissions"></a>Steuerung des Benutzerzugriffs und Berechtigungen

Sie können auf folgende Weise [Berechtigungen](#iot-hub-permissions) erteilen:

* **Hub Ebene freigegebene Access-Richtlinien**. Freigegebene Richtlinien können eine beliebige Kombination von [Berechtigungen](#iot-hub-permissions)erteilen. Sie können die Richtlinien im [Portal Azure]definieren[lnk-management-portal], oder programmgesteuert mithilfe des [IoT Hub-Anbieter für Ressourcen REST-APIs][lnk-resource-provider-apis]. Ein neu erstellter IoT Hub weist die folgenden Standardrichtlinien:

    - **Iothubowner**: Richtlinie mit allen Berechtigungen.
    - **Dienst**: die Richtlinie mit der Berechtigung ServiceConnect.
    - **Gerät**: Richtlinie mit der Berechtigung DeviceConnect.
    - **RegistryRead**: die Richtlinie mit der Berechtigung RegistryRead.
    - **RegistryReadWrite**: Richtlinie mit RegistryRead und RegistryWrite Berechtigungen.


* **Pro Gerät Sicherheitsanmeldeinformationen**. Jede IoT Hub enthält ein [Gerät Identität Registrierung][lnk-identity-registry]. Für jedes Gerät in dieser Registrierung können Sie die Sicherheitsanmeldeinformationen konfigurieren, die auf die entsprechende Gerät Endpunkte ausgelegte **DeviceConnect** -Berechtigungen erteilen.

Angenommen, in eine normale IoT-Lösung:

- Die Verwaltungskomponente Gerät verwendet die Richtlinie *RegistryReadWrite* .
- Die Ereignis Prozessor-Komponente verwendet die Richtlinie *Dienst* an.
- Die Laufzeit Geräte Business Logik Komponente verwendet die Richtlinie *Dienst* an.
- Einzelne Geräte verbinden mit Anmeldeinformationen, die in den IoT-Hub Identität Registrierung gespeichert.

## <a name="authentication"></a>Authentifizierung

Azure IoT Hub gewährt Zugriff auf Endpunkte, indem Sie ein Token vor der gemeinsamen Zugriffsrichtlinien und Gerät Identität Registrierung Sicherheitsanmeldeinformationen zu bestätigen.

Anmeldeinformationen, wie beispielsweise symmetrische Schlüssel werden nie über das Kabel gesendet.

> [AZURE.NOTE] Der Anbieter für Ressourcen Azure IoT Hub ist wie alle Anbieter im [Ressourcenmanager Azure]sind über Ihre Azure-Abonnement gesichert[lnk-azure-resource-manager].

Weitere Informationen zum Erstellen und Verwenden von Sicherheitstokens, finden Sie unter [IoT Hub Sicherheitstokens][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokoll websitedetails

Jedes unterstütztes Protokoll, beispielsweise MQTT, AMQP und HTTP, Nachrichtenübermittlung mithilfe auf verschiedene Arten von Token aus.

Bei der Verwendung von MQTT weist das Paket verbinden die Geräte-ID als ClientId, {Iothubhostname} / {Geräte-ID} in das Feld Username und ein SAS Token in das Feld Kennwort ein. {Iothubhostname} sollte der vollständige CName eines IoT Hub (z. B. contoso.azure-devices.net).

Bei Verwendung von [AMQP][lnk-amqp], IoT Hub unterstützt [Nur SASL] [ lnk-sasl-plain] und [AMQP Ansprüche-basierten-Sicherheit][lnk-cbs].

Wenn Sie AMQP Ansprüche-basierte-Sicherheit verwenden, gibt an, wie diese Token übertragen der standardmäßigen.

Der **Benutzername** sind für SASL-nur möglich:

* `{policyName}@sas.root.{iothubName}`Wenn Hub Ebene Token verwenden zu können.
* `{deviceId}@sas.{iothubname}`Wenn Gerät ausgelegte Token verwenden zu können.

In beiden Fällen enthält das Kennwortfeld das Token in [Sicherheitstokens IoT Hub]beschriebenen[lnk-sas-tokens].

HTTP implementiert Authentifizierung durch ein gültiges Token in der **Autorisierung** Anforderung Kopfzeile einfügen.

#### <a name="example"></a>Beispiel

Benutzername (Geräte-ID wird beachtet):`iothubname.azure-devices.net/DeviceId`

Kennwort (SAS mit Gerät Explorer generieren):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] Die [Azure IoT Hub SDKs] [ lnk-sdks] Token automatisch generieren, die bei der Verbindung mit dem Dienst. In einigen Fällen die SDKs aller Protokolle oder die Authentifizierungsmethoden nicht unterstützt.

### <a name="special-considerations-for-sasl-plain"></a>Für SASL nur Besonderheiten

Bei Verwendung von SASL nur mit AMQP, ein Client Herstellen einer Verbindung mit einem IoT-Hub können ein einzelnes Token für jede TCP-Verbindung. Wenn die Sicherheitstoken abläuft, wird die TCP-Verbindung trennt die Verbindung mit dem Dienst und eine erneute Verbindung auslöst. Dieses Verhalten, ist zwar nicht für eine Anwendung Back-End-Komponente problematisch großer Schaden für eine Anwendung auf dem Gerät aus den folgenden Gründen:

*  Gateways werden in der Regel für viele Geräte verbinden. Bei der Verwendung von SASL nur müssen sie eine distinct TCP-Verbindung für jedes Gerät herstellen einer Verbindung mit einem IoT-Hub zu erstellen. Dieses Szenario erheblich erhöht den Verbrauch von Power und Netzwerke Ressourcen und erhöht die Wartezeit jeder Verbindung Gerät.
* Geräte mit eingeschränkten Ressourcen werden durch die höhere Nutzung von Ressourcen nach jeder token Ablauf wiederherstellen beeinträchtigt.

## <a name="scope-hub-level-credentials"></a>Bereich Hub Ebene Anmeldeinformationen

Sie können Sicherheit auf Benutzerebene-Hub Richtlinien einen Bereich durch Token mit einer eingeschränkten Ressource URI erstellen. Der Endpunkt zum Senden von Gerät-Cloud-Nachrichten auf einem Gerät beträgt beispielsweise **/devices/ {Geräte-ID} / Nachrichten/Ereignisse**. Sie können auch eine Hub Ebene freigegebenen Zugriffsrichtlinie mit **DeviceConnect** Berechtigungen verwenden, ein Token anmelden, deren ResourceURI **/devices/ {Geräte-ID}**ist. Dieser Ansatz erstellt ein Token, das nur zum Senden von Nachrichten im Auftrag Gerät **Geräte-ID**verwendet werden kann.

Dieses Verfahren ähnelt dem das [Ereignis Hubs Publisher-Richtlinie][lnk-event-hubs-publisher-policy], und ermöglicht Ihnen das benutzerdefinierte Authentifizierungsmethoden implementieren.

## <a name="security-tokens"></a>Sicherheitstokens

IoT Hub verwendet Sicherheitstokens Authentifizierung Geräte und Dienste, damit keine Tasten bei der Übertragung gesendet. Darüber hinaus werden Sicherheitstokens zeitliche Gültigkeit und den Umfang eingeschränkt. [Azure IoT Hub SDKs] [ lnk-sdks] Token automatisch generieren, ohne eine spezielle Konfiguration erforderlich ist. Einige Szenarien erfordern jedoch den Benutzer generieren und Sicherheitstokens direkt verwenden. Hierzu gehören die direkte Verwendung der MQTT, AMQP oder HTTP Flächen oder der Implementierung des Musters token Service, wie in den [benutzerdefinierten Geräteauthentifizierung]erläutert[lnk-custom-auth].

IoT Hub können auch Geräte mit IoT Hub authentifizieren mit [x. 509-Zertifikate][lnk-x509]. 

### <a name="security-token-structure"></a>Sicherheit token Struktur
Verwenden Sie Sicherheitstokens Zeit begrenzt Zugriff auf Geräte und Dienste auf bestimmte Funktionen IoT Hub gewähren. Um sicherzustellen, dass nur autorisierte Geräte und Dienste eine Verbindung herstellen können, müssen Sicherheitstokens mithilfe eines freigegebenen Richtlinie Zugriffstaste oder einen symmetrischen Schlüssel gespeichert mit einer Identität Gerät in der Registrierung Identität angemeldet sein.

Ein Token angemeldet mit einem gemeinsamen Zugriff Richtlinie Key gewährt Zugriff auf alle Funktionen, die die freigegebenen Zugriffsberechtigungen für die Richtlinie zugeordnet. Ein mit einem Gerät-Identität symmetrischen Schlüssel signierten Token gewährt andererseits, nur die Berechtigung **DeviceConnect** für die zugeordnete Gerät Identität.

Das Sicherheitstoken weist das folgende Format:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Dies sind die erwarteten Werte aus:

| Wert | Beschreibung |
| ----- | ----------- |
| {Signatur} | Ein HMAC-SHA256 Signaturzeichenfolge der Form: `{URL-encoded-resourceURI} + "\n" + expiry`. **Wichtig**: der Schlüssel ist aus base64 decodiert und als Schlüssel verwendet, um die HMAC-SHA256 Berechnung ausführen. |
| {ResourceURI} | URI-Präfix (nach Segment) der Endpunkte, die mit diesem Token, beginnend mit Hostname der IoT Hub (keine Protocol) zugegriffen werden kann. Beispielsweise`myHub.azure-devices.net/devices/device1` |
| {Ablauf} | UTF8 Zeichenfolgen für die Anzahl der Sekunden seit der Epoche 00:00:00 UTC auf 1. Januar 1970. |
| {URL-codierte-ResourceURI} | Niedriger case-URL-codierte der Ressource Kleinbuchstaben URI |
| {PolicyName} | Der Name der freigegebenen Zugriffsrichtlinie auf dem dieses Token verweist. Im Falle von Verweisen auf Anmeldeinformationen in der Registrierung Gerät Token ist nicht vorhanden. |

**Hinweis Präfix**: das URI-Präfix von Segment und nicht von Zeichen berechnet wird. Beispielsweise `/a/b` ist ein Präfix für `/a/b/c` aber für keine `/a/bc`.

Im folgenden Node.js-Codeausschnitt wird eine Funktion namens **GenerateSasToken** , die das Token aus den Eingaben berechnet `resourceUri, signingKey, policyName, expiresInMins`. In den nächsten Abschnitten ausführlich erläutert die unterschiedlichen Eingaben für die verschiedenen token verwenden Fälle Initialisierung.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Vergleich wird entspricht Python Code ein Sicherheitstoken generiert:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Da die zeitliche Gültigkeit der Token auf Computern IoT Hub überprüft wird, ist es wichtig, dass die Nullpunktdrift auf die Uhr des Computers, der das Token generiert minimal sein.

### <a name="use-sas-tokens-in-a-device-client"></a>Verwenden von SAS Token in einem Gerät client

Es gibt zwei Methoden zum **DeviceConnect** Berechtigungen mit IoT Hub mit Sicherheitstoken zu erhalten: mithilfe eines [symmetrischen Gerät Key aus Registrierung Identität Gerät](#use-a-symmetric-key-in-the-identity-registry)oder mithilfe eines [freigegebenen Richtlinie Zugriffstaste](#use-a-shared-access-policy).

Beachten Sie, dass alle Geräte verfügbare Funktionen von Entwurf auf Endpunkten mit Präfix verfügbar gemacht wird `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Die einzige Möglichkeit, dass ein bestimmtes Gerät IoT Hub authentifiziert, ist den Gerät Identität symmetrischen Schlüssel verwenden. In Fällen muss eine freigegebenen Zugriffsrichtlinie verwendet wird, um Zugriff auf Gerät Funktionen, die Lösung die Komponente das Sicherheitstoken als vertrauenswürdig Unterkomponente ausgeben berücksichtigen.

Die Endpunkte Gerät zugänglichen sind (ohne Rücksicht auf das Protokoll):

| Endpunkt | Funktionalität |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Senden Sie Gerät-Cloud-Nachrichten. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Empfangen Sie Cloud-zu-Gerät. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Verwenden Sie einen symmetrischen Schlüssel in der Registrierung Identität

Wenn Sie ein Gerät-Identität symmetrischen Schlüssel generieren, die ein Token das PolicyName mit (`skn`) nicht Bestandteil der Token angegeben wird.

Beispielsweise sollte ein Token erstellt, um den Zugriff auf alle Funktionen von Gerät die folgenden Parameter haben:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Bei der Anmeldung Schlüssel: alle symmetrischen Schlüssel für die `{device id}` Identität,
* Kein Vorname Richtlinie
* jederzeit Ablauf.

Ein Beispiel für die Verwendung der oben angegebenen Knoten-Funktion wäre:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Das Ergebnis, wodurch der Zugriff auf alle Funktionen Gerät1 gewährt, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Es ist möglich, eine sichere Token mithilfe des Tools für .NET [Gerät Explorer]generieren[lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Verwenden einer freigegebenen Zugriffsrichtlinie

Wenn Sie ein Token aus einer freigegebenen Zugriffsrichtlinie zu erstellen, benennen Sie die Richtlinie Feld `skn` muss festgelegt werden, auf den Namen der Richtlinie verwendet. Außerdem ist es erforderlich, dass die Richtlinie die **DeviceConnect** die Berechtigung erteilt.

Die beiden wichtigsten Szenarien für die Verwendung von freigegebenen Richtlinien Zugriff auf Gerät Funktionen sind:

* [Cloud-Protokoll Gateways][lnk-endpoints],
* [Token Services] [ lnk-custom-auth] verwendet, um benutzerdefinierte Authentifizierung Phishingversuchen implementieren.

Da die freigegebenen Zugriffsrichtlinie potenziell Zugriff Verbindung als jedem Gerät gewähren kann, ist es wichtig, die richtige Ressource URI verwenden, wenn Sicherheitstoken zu erstellen. Dies ist besonders wichtig für token Dienste, die das Token mit einem bestimmten Gerät mit den Ressourcen-URI Gültigkeitsbereich festgelegt haben. Dieser Punkt ist für Protokoll Gateways, weniger relevant, wie sie bereits Datenverkehr für alle Geräte eingesetzt werden.

Beispielsweise würden ein Tokens-Diensts mithilfe der zuvor erstellten freigegebenen Richtlinie mit der Bezeichnung **Gerät** ein Token mit den folgenden Parametern erstellen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Bei der Anmeldung Schlüssel: eine der Schlüssel aus den `device` Richtlinie,
* Name der Richtlinie: `device`,
* jederzeit Ablauf.

Ein Beispiel für die Verwendung der oben angegebenen Knoten-Funktion wäre:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Das Ergebnis, wodurch der Zugriff auf alle Funktionen Gerät1 gewährt, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Ein Gateway Protocol könnten das gleiche Token für alle Geräte einfach Festlegen der Ressource URI verwenden, um `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Verwenden des Sicherheitstokens von Servicekomponenten

Die Komponenten können nur Sicherheitstokens mithilfe von freigegebenen Access-Richtlinien die entsprechenden Berechtigungen erteilen, wie zuvor beschrieben generieren.

Dies sind die Service-Funktionen für die Endpunkte verfügbar gemacht werden:

| Endpunkt | Funktionalität |
| ----- | ----------- |
| `{iot hub host name}/devices` | Erstellen, aktualisieren, abrufen und Löschen von Identitäten Gerät. |
| `{iot hub host name}/messages/events` | Empfangen Sie Gerät Cloud. |
| `{iot hub host name}/servicebound/feedback` | Erhalten Sie Feedback für Cloud-zu-Gerät Nachrichten. |
| `{iot hub host name}/devicebound` | Senden Sie Cloud-zu-Gerät-Nachrichten. |

Beispielsweise würden ein Dienst generieren, die mit den zuvor erstellten freigegebenen Richtlinie mit der Bezeichnung **RegistryRead** ein Token mit den folgenden Parametern erstellen:

* Ressourcen-URI: `{IoT hub name}.azure-devices.net/devices`,
* Bei der Anmeldung Schlüssel: eine der Schlüssel aus den `registryRead` Richtlinie,
* Name der Richtlinie: `registryRead`,
* jederzeit Ablauf.

    Varianz Endpunkt = "myhub.azure-devices.net/devices";   Varianz PolicyName = 'Gerät';   Var PolicyKey = '...'.

    Varianz Token = GenerateSasToken (Endpunkt, PolicyKey, PolicyName, 60)

Das Ergebnis, das alle Gerät Identitäten Lesezugriff gewähren möchten, wäre:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Unterstützte x. 509-Zertifikate

Alle x 509-Zertifikat können Sie ein Gerät mit IoT Hub authentifizieren. Dies umfasst:

-   **Ein vorhandenes x. 509-Zertifikat**. Möglicherweise müssen Sie ein Gerät bereits ein x. 509-Zertifikat zugeordnet. Das Gerät kann dieses Zertifikat Authentifizierung mit IoT Hub verwenden.

-   **Ein automatisch generiertes und selbstsignierten X-509 Zertifikat**. Eine Gerätehersteller oder für Bereitsteller kann diese Zertifikate generieren und speichern die entsprechenden privaten Schlüssel (und das Zertifikat) auf dem Gerät. Tools wie [OpenSSL] können[ lnk-openssl] und [Windows-SelfSignedCertificate] [ lnk-selfsigned] -Programm für diesen Zweck.

-   **Zertifizierungsstelle signiertes x 509-Zertifikat**. Sie können auch ein x. 509-Zertifikat generiert und signiert von einer Zertifizierungsstelle (CA) verwenden, identifizieren ein Gerät und einem Gerät mit IoT Hub authentifizieren.

Ein Gerät möglicherweise entweder eine x 509-Zertifikat oder ein Sicherheitstoken Authentifizierung, jedoch nicht beide verwenden.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Registrieren Sie sich ein x. 509-Client-Zertifikat für ein Gerät

Das [Azure IoT Service SDK für C#-] [ lnk-service-sdk] (Version 1.0.8+) unterstützt Registrieren eines Geräts, die ein x. 509-Client-Zertifikat für die Authentifizierung verwendet. Andere APIs wie Importieren/Exportieren von Geräten unterstützt auch x. 509-Client-Zertifikate.

### <a name="c-support"></a>C\# Support

Die Klasse **RegistryManager** stellt eine programmgesteuerten Möglichkeit, ein Gerät zu registrieren. Insbesondere ermöglichen die Methoden **AddDeviceAsync** und **UpdateDeviceAsync** ein Benutzers zum Registrieren und aktualisieren ein Gerät in der Registrierung Iot Hub Geräts Identität. Diese beiden Methoden akzeptieren eine Instanz **Gerät** als Eingabe. Die **Gerät** Klasse enthält eine **Authentifizierung** -Eigenschaft, die Benutzer primären und sekundären x. 509-Zertifikatfingerabdruck angeben kann. Der Fingerabdruck stellt einen SHA-1-Hash des x. 509-Zertifikats (gespeichert mit binäre DER-Codierung). Benutzer können die Option zur Angabe eines primären Fingerabdrucks oder einer sekundären Fingerabdruck oder beides. Primären und sekundären Fingerabdrücke werden unterstützt, um das Zertifikat Rollover Szenarien zu behandeln.

> [AZURE.NOTE] IoT Hub ist nicht erforderlich oder das gesamte x. 509-Client-Zertifikat nur den Fingerabdruck gespeichert.

Hier ist ein Beispiel C\# Codeausschnitt auf einem Gerät mit einem x. 509-Client-Zertifikat zu registrieren:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Verwenden Sie ein x. 509-Client-Zertifikat während Runtime-Vorgänge

Das [Azure IoT Gerät SDK für .NET] [ lnk-client-sdk] (Version 1.0.11+) unterstützt die Verwendung von x. 509-Client-Zertifikate.

### <a name="c-support"></a>C\# Support

Die Klasse **DeviceAuthenticationWithX509Certificate** unterstützt die Erstellung von  **DeviceClient** Instanzen mit einem x. 509-Client-Zertifikat.

Hier ist ein Beispiel-Codeausschnitt:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Benutzerdefiniertes Geräteauthentifizierung

Können die IoT Hub [Gerät Identität Registrierung] [ lnk-identity-registry] pro Gerät Sicherheitsanmeldeinformationen konfigurieren und Steuerelement mit [Token][lnk-sas-tokens]. Jedoch, wenn eine Lösung IoT erheblichen bereits in einem benutzerdefinierten Gerät Identität Registrierung und/oder Authentifizierung-Schema verfügt, können Sie diese vorhandene Infrastruktur mit IoT Hub integrieren durch Erstellen eines *Sicherheitstokens Dienst*. Auf diese Weise können Sie andere Features IoT in Ihre Lösung.

Ein token Dienst ist eine benutzerdefinierte Cloud-Dienst. Verwendet eine IoT Hub *freigegebenen Zugriffsrichtlinie* mit **DeviceConnect** Berechtigungen zum *Gerät ausgelegte* Token zu erstellen. Diese Token aktivieren ein Geräts für die Verbindung zu Ihrem IoT-Hub an.

  ![Schritte des Musters token service][img-tokenservice]

Dies sind die wichtigsten Schritte des Musters token Service:

1. Erstellen Sie eine Zugriffsrichtlinie IoT Hub freigegeben mit **DeviceConnect** Berechtigungen für Ihre IoT-Hub an. Sie können diese Richtlinie erstellen, in der [Azure-Portal] [ lnk-management-portal] oder programmgesteuert. Der token Dienst verwendet dieser Richtlinie sich die Token erstellt wird.
2. Wenn Sie ein Gerät muss Ihre IoT Hub zugreifen, fordert er ein signierten Token aus dem token Dienst an. Das Gerät kann authentifiziert mit Ihrer benutzerdefinierten Gerät Identität Registrierung/Authentifizierung des Farbschemas, Identität des Geräts festzustellen, die Token Dienst verwendet wird, um das Token zu erstellen.
3. Token Dienst gibt ein Token zurück. Das Token wird erstellt, mit `/devices/{deviceId}` als `resourceURI`, mit `deviceId` als das Gerät authentifiziert wird. Die token Service mithilfe die freigegebenen Zugriffsrichtlinie das Token erstellt.
4. Das Gerät verwendet das Token direkt mit dem IoT-Hub an.

> [AZURE.NOTE] Können Sie die Klasse .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] oder die Java-Klasse [IotHubServiceSasToken] [ lnk-java-sas] ein Token in Ihrem Dienst token zu erstellen.

Der Dienst token kann die token Ablaufdatum wie gewünscht festlegen. Wenn die Sicherheitstoken abläuft, trennt der IoT Hub Verbindung mit dem Gerät aus. Das Gerät muss dann ein neues Token aus dem token Dienst anfordern. Wenn Sie eine kurze Ablauf Uhrzeit verwenden, wird dies die Last auf das Gerät und der token Dienst erhöht.

Für ein Gerät mit Ihrem Hub verbinden, müssen Sie weiterhin diese Hinzufügen zur Registrierung IoT Hub Gerät Identität – Obwohl das Gerät ein Token und nicht auf einem Geräteschlüssel Verbindung verwendet wird. Daher können Sie weiterhin pro Gerät Access-Steuerelements zu verwenden, indem Sie aktivieren oder Deaktivieren von Gerät Identitäten in der [Registrierung von IoT Hub Identität] [ lnk-identity-registry] Wenn das Gerät authentifiziert mit einem Token. Dadurch werden die Risiken Ihrer Verwendung Token mit langen Ablauf Zeiten aus.

### <a name="comparison-with-a-custom-gateway"></a>Vergleich mit einem benutzerdefinierten gateway

Das token Service Muster ist die empfohlene Vorgehensweise zum Implementieren eines benutzerdefinierten Identität Registrierung/Authentifizierung des Farbschemas mit IoT-Hub an. Es wird empfohlen, weil IoT Hub weiterhin die meisten der Lösung Datenverkehr behandeln. Es gibt jedoch auch Fälle, in dem das Schema benutzerdefinierte Authentifizierung also mit dem Protokoll ineinander greifen, dass ein Dienst verarbeitet den Datenverkehr (*benutzerdefinierten Gateways*) erforderlich ist. Ein Beispiel dafür ist [Sicherheit TLS (Transport Layer) und vorinstallierten Schlüssel (PSKs)][lnk-tls-psk]. Weitere Informationen finden Sie unter [Protokollgateway] [ lnk-protocols] Thema.

## <a name="reference-topics"></a>Verweis Themen:

Die folgenden Verweis Themen bieten weitere Informationen zum Steuern des Zugriffs auf Ihre IoT Hub.

## <a name="iot-hub-permissions"></a>IoT Hub Berechtigungen

Die folgende Tabelle listet die Berechtigungen, die Sie zum Steuern des Zugriffs auf Ihre IoT-Hub verwenden können.

| Berechtigung            | Notizen |
| --------------------- | ----- |
| **RegistryRead**      | Gewährt Zugriff auf die Registrierung des Geräts Identität zu lesen. Weitere Informationen finden Sie unter [Gerät Identität Registrierung][lnk-identity-registry]. |
| **RegistryReadWrite** | Gewährt Lese- und Schreibzugriff auf die Registrierung des Geräts Identität. Weitere Informationen finden Sie unter [Gerät Identität Registrierung][lnk-identity-registry]. |
| **ServiceConnect**    | Gewährt Zugriff auf die cloud-Dienst zugängliche Kommunikation und Überwachung Endpunkte. Es wird beispielsweise über die Berechtigung zum Back-End Clouddienste Gerät Cloud empfangen, Cloud-zu-Gerät Nachrichten zu senden und die entsprechenden Übermittlung Danksagungen abrufen gewährt. |
| **DeviceConnect**     | Erteilt den Zugriff auf Gerät zugänglichen Kommunikationsendpunkte. Es wird beispielsweise über die Berechtigung zum Gerät-Cloud-Nachrichten senden und Empfangen von Nachrichten Cloud-zu-Gerät gewährt. Diese Berechtigung wird von Geräten verwendet. |

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Andere Verweis in der Developer Guide Themen:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschrieben, die verschiedene Endpunkte, die jeder IoT Hub für Laufzeit und Management Vorgänge macht.
- [Beschränkung und Kontingente] [ lnk-quotas] beschreibt die Kontingente, die beziehen sich auf den Dienst IoT Hub und das Drosselung Verhalten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub Geräte und Dienste SDKs] [ lnk-sdks] Listet die verschiedenen Sprache SDKs Sie eine zu verwenden, wenn Sie das Gerät und der Dienst Clientanwendungen, die Interaktion mit einem IoT Hub entwickeln.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] die Abfragesprache Sie Informationen über IoT Hub Ihr Gerät im Vergleich, Methoden und Aufträge abgerufen können werden.
- [Support IoT Hub MQTT] [ lnk-devguide-mqtt] enthält weitere Informationen über die Unterstützung von IoT Hub für das Protokoll MQTT.

## <a name="next-steps"></a>Nächste Schritte

Jetzt Sie zum Steuern des Zugriffs IoT Hub vertraut gemacht haben, können Sie in den folgenden Themen Developer Guide interessiert wenden:

- [Verwenden Sie Gerät im Vergleich zum Synchronisieren von Zustand und Konfigurationen][lnk-devguide-device-twins]
- [Um eine direkte Methode auf einem Gerät aufzurufen][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, können Sie die folgenden IoT Hub Lernprogramme interessant sein:

- [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]
- [Informationen zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT-Hub][lnk-c2d-tutorial]
- [Wie IoT Hub Gerät-Cloud-Nachrichten verarbeitet][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
