# <a name="securing-your-iot-deployment"></a>Sichern Ihrer IoT-Bereitstellung

Dieser Artikel enthält die nächste Detailebene für Schutz der Infrastruktur IoT Azure-basierten Internet der Dinge (IoT). Klicken Sie auf Implementierung Ebene Details zum Konfigurieren und Bereitstellen von jeder Komponente verweist. Darüber hinaus Vergleiche und Auswahlmöglichkeiten zwischen verschiedenen verschiedenen Methoden.

Sichern der Bereitstellung Azure IoT kann in den folgenden drei Sicherheitsbereichen unterteilt werden:

- **Sicherheit von Geräten**: das Gerät IoT sichern, während sie in der Natur bereitgestellt wird.
- **Verbindung Sicherheit**: alle Daten, die zwischen dem Gerät IoT und IoT Hub übertragen sichergestellt ist, vertrauliche und vor unbefugtem.
- **Cloud-Sicherheit**: bietet eine Möglichkeit, um Daten zu schützen, während sie durch geht, und in der Cloud gespeichert ist.

![Drei Sicherheitsbereichen][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Sicheres Gerät bereitgestellt und Authentifizierung

Die Azure IoT Suite sichert IoT Geräte mit den folgenden beiden Methoden an:

- Durch die Bereitstellung eines eindeutigen Identität Keys (Sicherheitstokens) für jedes Gerät, die vom Gerät zur Kommunikation mit dem IoT Hub verwendet werden können.

- Mithilfe von in-Gerät- [x 509-Zertifikat] [ lnk-x509] und private Schlüssel als eine bedeutet, dass das Gerät an den IoT Hub authentifiziert. Diese Authentifizierungsmethode wird sichergestellt, dass der private Schlüssel auf dem Gerät außerhalb des Geräts zu einem beliebigen Zeitpunkt, unbekannt ist eine höhere Sicherheit bereitstellen.

Die Sicherheit token Methode bietet Authentifizierung für jeden Anruf, indem Sie das Gerät an IoT Verteiler durch Zuordnen des symmetrischen Schlüssels für die einzelnen Aufrufe. X. 509-basierten Authentifizierung ermöglicht die Authentifizierung von einem Gerät IoT auf der physischen Ebene als Teil des Betriebs Verbindung TLS. Die Sicherheit Token-basierte Methode kann ohne das x. 509-Authentifizierung verwendet werden, also ein Muster weniger sicher. Die Auswahl zwischen den beiden Methoden wird hauptsächlich von wie sicher Gerät Authentifizierung muss werden und Verfügbarkeit von sicheren Speicher auf dem Gerät (um den privaten Schlüssel sicher zu speichern) vorgegeben.

## <a name="iot-hub-security-tokens"></a>Sicherheitstokens IoT-Hub

IoT Hub verwendet Sicherheitstokens Authentifizierung Geräte und Dienste, damit keine Tasten im Netzwerk gesendet. Darüber hinaus werden Sicherheitstokens zeitliche Gültigkeit und den Umfang eingeschränkt. Automatisch generieren Azure IoT Hub SDKs Token ohne eine spezielle Konfiguration erforderlich ist. Einige Szenarien erfordern jedoch den Benutzer generieren und Sicherheitstokens direkt verwenden. Hierzu gehören die direkte Verwendung der MQTT, AMQP oder HTTP Flächen oder der Implementierung des Musters token Service.

Weitere Details auf die Struktur des Sicherheitstokens und deren Verwendung finden Sie in den folgenden Artikeln:

-   [Sicherheit token Struktur][lnk-security-tokens]
-   [Verwenden von SAS Token als ein Gerät][lnk-sas-tokens]

Jede IoT Hub verfügt über ein [Gerät Identität Registrierung] [ lnk-identity-registry] Ressourcen pro Gerät im Dienst, beispielsweise eine Warteschlange zu erstellen, die Flight Cloud-zu-Gerät-Nachrichten, enthält und den Zugriff auf die Endpunkte Gerät zugänglichen verwendet werden können. Die IoT Hub Identität Registrierung ermöglicht ein sicheres Speichern von Gerät Identitäten und Sicherheitsschlüssel für eine Lösung. Person oder Gruppen von Gerät Identitäten können auf eine Liste oder eine Sperrliste hinzugefügt werden vollständige Kontrolle über Gerät zugreifen. Die folgenden Artikeln finden Sie weitere Informationen zur Struktur der Registrierung Identität Gerät und Vorgänge unterstützt.

[IoT Hub unterstützt Protokolle wie MQTT, AMQP und HTTP-][lnk-protocols]. Jedes dieser Protokolle verwenden Sicherheitstoken vom Gerät IoT an IoT Verteiler anders:

- AMQP: SASL EINFARBIGEN und AMQP anspruchsbasierte Sicherheit ({policyName}@sas.root.{iothubName} bei Hub Ebene Token; {Geräte-ID} bei Gerät ausgelegte Token).

- MQTT: Verbinden Paket verwendet {Geräte-ID} als {ClientId}, {IoThubhostname} / {Geräte-ID} in das Feld **Username** und ein SAS Token in das Feld **Kennwort** ein.

- HTTP: Gültiges Token ist in der Kopfzeile der Autorisierung anfordern.

IoT Hub Gerät Identität Registrierung kann pro Gerät Sicherheitsanmeldeinformationen konfigurieren und Steuerelement verwendet werden. Jedoch, wenn eine Lösung IoT erheblichen bereits in einem [benutzerdefinierten Gerät Identität Registrierung und/oder Authentifizierung Schema]verfügt[lnk-custom-auth], in eine vorhandene Infrastruktur mit IoT Hub durch Erstellen einer token Service integriert werden.

### <a name="x509-certificate-based-device-authentication"></a>X. 509-Zertifikat-basierten Geräteauthentifizierung

Die Verwendung eines [x 509-Zertifikat Gerät-basierten] [ lnk-use-x509] und deren zugeordneten Paar zu öffentlichen und privaten ermöglicht zusätzliche Authentifizierung auf der physischen Ebene. Den privaten Schlüssel sicher auf dem Gerät gespeichert ist und kann nicht außerhalb des Geräts ermittelt. Das x. 509-Zertifikat enthält Informationen über das Gerät, beispielsweise Geräte-ID, und andere organisationsinterne Details. Mit den privaten Schlüssel ist eine Signatur des Zertifikats generiert.

Auf hoher Ebene Gerät provisioning Fluss:

- Zuordnen eines Bezeichners für eine physische Gerät – Gerät Identität und/oder x 509-Zertifikat, das das Gerät während Gerät Fertigung oder Inbetriebnahme zugeordnet ist.

- Erstellen Sie einen entsprechenden Identität Eintrag IoT Hub – Gerät Identität und zugehörigen Geräteinformationen in der Registrierung IoT Hub Gerät aus.

- X. 509-Zertifikatfingerabdruck sicher in IoT Hub Gerät Registrierung zu speichern.

### <a name="root-certificate-on-device"></a>Quadratwurzel Zertifikat für Gerät

Beim Herstellen einer sicheren TLS-Verbindungs IoT-Hub an, authentifiziert das Gerät IoT IoT Hub verwenden ein Zertifikat einer Stammzertifizierungsstelle Teil des Geräts SDK also an. Für den Client C SDK befindet sich das Zertifikat unter dem Ordner "\\c\\Zertifikate" im Stammverzeichnis der Repo. Obwohl diese Stammzertifikate langer Lebensdauer sind, diese weiterhin möglicherweise ablaufen oder gesperrt werden. Das Gerät sind möglicherweise nicht später eine Verbindung IoT-Hub (oder einen anderen Clouddienst) herstellen können, ist es keine Möglichkeit, das Zertifikat auf dem Gerät aktualisieren. Gibt es eine Möglichkeit, das Zertifikat Stammverzeichnis aktualisieren, sobald das Gerät IoT bereitgestellt wurde, wird dieses Risiko effektiv verringern.

## <a name="securing-the-connection"></a>Sichern der Verbindungs

Internet-Verbindung zwischen dem Gerät IoT und IoT Hub ist gesicherter Zeitstandard der Sicherheit TLS (Transport Layer). Azure IoT unterstützt [TLS 1.2][lnk-tls12], TLS 1.1 und TLS 1.0, in dieser Reihenfolge. Unterstützung für TLS 1.0 wird nur für Abwärtskompatibilität bereitgestellt. Es wird empfohlen, TLS 1.2 verwenden, da sie die meisten Sicherheit bietet.

Azure IoT Suite unterstützt die folgenden Verschlüsselung Suites in dieser Reihenfolge an.

| Ziffernfolge | Länge |
|--------------|--------|
| TLS\_ECDHE\_RSA\_mit\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (EQ. EA 7680 Bits RSA) | 256    |
| TLS\_ECDHE\_RSA\_mit\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (EQ. EA 3072 Bits RSA) | 128    |
| TLS\_ECDHE\_RSA\_mit\_AES\_256\_CBC\_SHA (0xc014) secp384r1 ECDH (EQ. EA 7680 Bits RSA)           | 256    |
| TLS\_ECDHE\_RSA\_mit\_AES\_128\_CBC\_SHA (0xc013) secp256r1 ECDH (EQ. EA 3072 Bits RSA)           | 128    |
| TLS\_RSA\_mit\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_mit\_AES\_128\_GCM\_SHA256 (0x9C MACHINE_CHECK_EXCEPTION) | 128    |
| TLS\_RSA\_mit\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_mit\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_mit\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_mit\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_mit\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Sichern der cloud

Azure IoT Hub ermöglicht die Definition von [Richtlinien für Access-Steuerelement] [ lnk-protocols] für jeden Sicherheitsschlüssel. Die folgenden Satzes von Berechtigungen verwendet, um zu den einzelnen IoT-Hub Endpunkte Zugriff gewähren. Berechtigungen beschränken Sie den Zugriff auf einen IoT Hub basierend auf Funktionalität.

- **RegistryRead**. Die Registrierung des Geräts Identität Lesezugriff gewährt. Weitere Informationen finden Sie unter [Gerät Identität Registrierung][lnk-identity-registry].

- **RegistryReadWrite**. Gewährt Lese- und Schreibzugriff auf die Registrierung des Geräts Identität. Weitere Informationen finden Sie unter [Gerät Identität Registrierung][lnk-identity-registry].

- **ServiceConnect**. Gewährt Zugriff auf die cloud-Dienst zugängliche Kommunikation und Überwachung Endpunkte. Es wird beispielsweise über die Berechtigung zum Back-End Clouddienste Gerät Cloud empfangen, Cloud-zu-Gerät Nachrichten zu senden und die entsprechenden Übermittlung Danksagungen abrufen gewährt.

- **DeviceConnect**. Erteilt den Zugriff auf Gerät zugänglichen Kommunikationsendpunkte. Es wird beispielsweise über die Berechtigung zum Gerät-Cloud-Nachrichten senden und Empfangen von Nachrichten Cloud-zu-Gerät gewährt. Diese Berechtigung wird von Geräten verwendet.

Es gibt zwei Methoden zum **DeviceConnect** Berechtigungen mit IoT Hub mit [Sicherheitstoken]zu erhalten[lnk-sas-tokens]: mit einem Gerät Identitätsschlüssel oder einer freigegebenen Richtlinie Zugriffstaste. Darüber hinaus ist zu beachten, dass alle Geräte verfügbare Funktionen von Entwurf auf Endpunkten mit Präfix verfügbar gemacht wird `/devices/{deviceId}`.

[Service-Komponenten können nur Sicherheitstokens generieren] [ lnk-service-tokens] mithilfe von freigegebenen-Richtlinien die entsprechenden Berechtigungen erteilen.

Azure IoT Hub und andere Dienste, die Teil der Lösung eventuell zulassen Verwaltung der Benutzer mit der Azure-Active Directory.

Aufgenommen von Azure IoT Hub Daten können von einer Vielzahl von Diensten wie Azure Stream Analytics und Azure Blob-Speicher genutzt werden. Diese Dienste ermöglichen Management-Zugriff. Erfahren Sie mehr über diese Dienste und die verfügbaren Optionen unten:

- [Azure DocumentDB][lnk-docdb]: kein skalierbare Datenbank vollständig indiziert Dienst für teilweise strukturierte Daten, die Metadaten für die Geräte verwaltet werden Sie bereitgestellt, z. B. Attributen, Konfiguration und Sicherheitseigenschaften. DocumentDB bietet leistungsfähige mit hohem Durchsatz Verarbeitung, Schema-unabhängige Indizierung von Daten und eine umfangreiche SQL-Abfrage-Oberfläche.

- [Azure Stream Analytics][lnk-asa]: in Echtzeit in der Cloud, mit dem Sie sich schnell zu entwickeln und Bereitstellen einer Lösung Analytics-LC, um in Echtzeit Einsichten von Geräten, Sensoren, Infrastruktur und Anwendungen für eine Steigerung, Verarbeitung Stream. Die Daten aus diesem Dienst vollständig verwaltete können auf eine beliebige Volume zu skalieren und weiterhin gleichzeitig hohen Durchsatz, geringe Wartezeit und Stabilität.

- [Azure App Services][lnk-appservices]: eine Cloud leistungsfähige Web- und mobile-apps, die Daten an anderer Stelle; Verbindung erstellen in der Cloud oder lokal. Erstellen von ansprechenden für iOS, Android und Windows mobile-apps. Mit der Software als a Service (SaaS) und Enterprise-Clientanwendungen mit einem Out-of-Box-Konnektivität auf Dutzende Cloud-basierte Services und Enterprise Applications integrieren. Fehlercode in Ihre bevorzugten Sprache und IDE (.NET, NodeJS, PHP, Python oder Java) Web apps und APIs schneller als je zuvor erstellen.

- [Logik Apps][lnk-logicapps]: der Logik Apps-Features der App-Verwaltungsdienst Azure hilft, Ihre IoT Lösung für Ihre vorhandene Line-of-Business-Systeme integrieren und Workflow-Prozesse automatisieren. Logik Apps ermöglicht Entwicklern das Design Workflows, die von einem Trigger anfangen, und führen Sie eine Reihe von Schritten – Regeln und Aktionen, die leistungsfähige Verbinder zu verwenden, mit Ihrer Geschäftsprozesse integriert werden soll. Logik Apps bietet Out-of-Box-Verbindung zu einem riesigen Netz von SaaS, Cloud-basierte und lokale Applications.

- [Azure Blob-Speicher][lnk-blob]: zuverlässigen, preisgünstige Cloud-Speicher für die Daten, die Ihren Geräten in der Cloud zu senden.

## <a name="conclusion"></a>Abschluss

In diesem Artikel Übersicht Implementierung Ebene Details für das Entwerfen und Bereitstellen einer IoT Infrastruktur Azure IoT verwenden. Konfigurieren jede Komponente benutzerspezifisch secure ist in der gesamten IoT Infrastruktur Schutz. Einige die verfügbaren Azure IoT Auswahlmöglichkeiten beim Entwurf bieten Flexibilität und Wahlmöglichkeiten; jede Auswahl haben jedoch Auswirkungen Sicherheit. Es wird empfohlen, dass jede dieser Optionen durch eine Bewertung Risiko/Kosten ausgewertet werden.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
