<properties 
    pageTitle="Teilen und Zusammenführen Sicherheitskonfiguration | Microsoft Azure" 
    description="Einrichten von X409 Zertifikate für die Verschlüsselung" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Sicherheitskonfiguration von Teilen und Zusammenführen  

Um den geteilten/Zusammenführen-Dienst verwenden, müssen Sie die Sicherheit ordnungsgemäß konfigurieren. Der Dienst ist Bestandteil des flexible skalieren Features von Microsoft Azure SQL-Datenbank. Weitere Informationen finden Sie unter [flexible skalieren Teilen und Zusammenführen Dienst Lernprogramm](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Konfigurieren von Zertifikaten

Zertifikate werden auf zwei Arten konfiguriert. 

1. [So konfigurieren Sie das SSL-Zertifikat](#To-Configure-the-SSL#Certificate)
2. [So konfigurieren Sie Client-Zertifikate](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Beziehen von Zertifikaten

Zertifikate können von öffentlichen Zertifizierungsstellen (Zertifizierungsstellen) oder aus dem [Windows-Zertifikat-Dienst](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx)abgerufen werden. Hierbei handelt es sich um die bevorzugten Methoden, Zertifikate erhalten.

Wenn diese Optionen nicht verfügbar sind, können Sie **selbstsignierten Zertifikaten**generieren.
 
## <a name="tools-to-generate-certificates"></a>Tools zum Generieren von Zertifikaten

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [Pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Die Tools ausführen

* Aus einer Entwicklertools Befehlszeile für Visual Studios, finden Sie unter [Visual Studio-Eingabeaufforderungsfenster](http://msdn.microsoft.com/library/ms229859.aspx) 

    Wenn installiert haben, wechseln Sie zu:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Abrufen von WDK [Windows 8.1: Herunterladen Kits und Tools](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>So konfigurieren Sie das SSL-Zertifikat
Ein Zertifikat einer Zertifizierungsstelle ist erforderlich, um die Kommunikation verschlüsseln und Authentifizieren des Servers. Wählen Sie die am besten für die folgenden drei Szenarien, und führen Sie alle zugehörigen Schritte aus:

### <a name="create-a-new-self-signed-certificate"></a>Erstellen eines neuen selbstsignierten Zertifikats

1.    [Erstellen eines selbstsignierten Zertifikats](#Create-a-Self-Signed-Certificate)
2.    [Erstellen von PFX-Datei für selbstsignierten SSL-Zertifikats](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [Hochladen von SSL-Zertifikat zum Cloud-Dienst](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [Aktualisieren von SSL-Zertifikat im Konfigurations-Dienstdatei](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [Importieren von SSL-Zertifizierungsstelle](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Verwenden Sie ein vorhandenes Zertifikat aus dem Zertifikatspeicher
1. [Exportieren von SSL-Zertifikat Zertifikat Store](#Export-SSL-Certificate-From-Certificate-Store)
2. [Hochladen von SSL-Zertifikat zum Cloud-Dienst](#Upload-SSL-Certificate-to-Cloud-Service)
3. [Aktualisieren von SSL-Zertifikat im Konfigurations-Dienstdatei](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>So verwenden Sie ein vorhandenes Zertifikat in eine PFX-Datei

1. [Hochladen von SSL-Zertifikat zum Cloud-Dienst](#Upload-SSL-Certificate-to-Cloud-Service)
2. [Aktualisieren von SSL-Zertifikat im Konfigurations-Dienstdatei](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>So konfigurieren Sie Client-Zertifikate
Client-Zertifikate sind Anfragen zum Dienst für die Authentifizierung erforderlich. Wählen Sie die am besten für die folgenden drei Szenarien, und führen Sie alle zugehörigen Schritte aus:

### <a name="turn-off-client-certificates"></a>Deaktivieren der Client-Zertifikate
1.    [Deaktivieren von Zertifikaten basierende Client-Authentifizierung](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Emission neuen selbstsignierten Clientzertifikaten
1.    [Erstellen einer selbstsignierten Zertifizierungsstelle](#Create-a-Self-Signed-Certification-Authority)
2.    [Hochladen von CA Zertifikat, um die Cloud-Dienst](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Aktualisieren von CA Zertifikat im Konfigurations-Dienstdatei](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Problem Client-Zertifikate](#Issue-Client-Certificates)
5.    [Erstellen von PFX-Dateien für Client-Zertifikate](#Create-PFX-files-for-Client-Certificates)
6.    [Client-Zertifikat importieren](#Import-Client-Certificate)
7.    [Kopieren der Zertifikatfingerabdruck Client](#Copy-Client-Certificate-Thumbprints)
8.    [Konfigurieren der zulässigen Clients in der Konfiguration Dienstdatei](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Verwenden Sie vorhandene Zertifikate
1.    [Suchen nach öffentlichen Zertifizierungsstellenschlüssel](#Find-CA-Public Key)
2.    [Hochladen von CA Zertifikat, um die Cloud-Dienst](#Upload-CA-certificate-to-cloud-service)
3.    [Aktualisieren von CA Zertifikat im Konfigurations-Dienstdatei](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Kopieren der Zertifikatfingerabdruck Client](#Copy-Client-Certificate-Thumbprints)
5.    [Konfigurieren der zulässigen Clients in der Konfiguration Dienstdatei](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Konfigurieren von Client Zertifikat Zertifikatssperrüberprüfung](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Zugelassene IP-Adressen

Zugriff auf die Endpunkte kann auf bestimmte Bereiche von IP-Adressen beschränkt werden.

## <a name="to-configure-encryption-for-the-store"></a>Konfigurieren der Verschlüsselung für den Speicher

Ein Zertifikat ist erforderlich, um die Verschlüsselung der Anmeldeinformationen, die im Metadatenspeicher gespeichert werden. Wählen Sie die am besten für die folgenden drei Szenarien, und führen Sie alle zugehörigen Schritte aus:

### <a name="use-a-new-self-signed-certificate"></a>Verwenden Sie ein neues selbst signiertes Zertifikat.

1.     [Erstellen eines selbstsignierten Zertifikats](#Create-a-Self-Signed-Certificate)
2.     [Erstellen Sie für selbstsignierten Verschlüsselungszertifikats PFX-Datei](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Hochladen Sie Verschlüsselungszertifikats zum Cloud-Dienst](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Aktualisieren von Verschlüsselungszertifikats in Konfiguration Dienstdatei](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Verwenden Sie ein vorhandenes Zertifikat aus dem Zertifikatspeicher

1.     [Exportieren von Verschlüsselungszertifikats Zertifikat Store](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Hochladen Sie Verschlüsselungszertifikats zum Cloud-Dienst](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Aktualisieren von Verschlüsselungszertifikats in Konfiguration Dienstdatei](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Verwenden Sie ein vorhandenes Zertifikat in eine PFX-Datei

1.     [Hochladen Sie Verschlüsselungszertifikats zum Cloud-Dienst](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Aktualisieren von Verschlüsselungszertifikats in Konfiguration Dienstdatei](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Der Standard-Konfiguration

Die standardmäßige Konfiguration verweigert alle Zugriff auf den HTTP-Endpunkt. Dies ist die empfohlene Einstellung, da die Anforderungen an diese Endpunkte vertrauliche Informationen wie Datenbankbenutzers durchführen können.
Standardmäßig werden alle Zugriff auf den HTTPS-Endpunkt zugelassen. Mit dieser Einstellung kann weiter beschränkt werden.

### <a name="changing-the-configuration"></a>Ändern der Konfiguration

In die Gruppe von Access Steuerelement Regeln, die auf anwenden und Endpunkt konfiguriert die **<EndpointAcls>** Abschnitt **Konfiguration Dienstdatei**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Die Regeln in eine Access-Steuerelementgruppe sind so konfiguriert, dass einer <AccessControl name=""> Abschnitt der Dienstkonfigurationsdatei. 

Das Format wird in Network Access Control Lists Dokumentation erläutert.
Damit nur IP-Adressen im Bereich 100.100.0.0 zu 100.100.255.255 auf den HTTPS-Endpunkt zugreifen können, würde die Regeln beispielsweise wie folgt aussehen:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>DOS-Dienst (verhindern)

Es gibt zwei verschiedenen Verfahren zum Erkennen und verhindern, dass der DOS Angriffen unterstützt:

*    Einschränken der Anzahl der gleichzeitige Anforderungen pro remote-Host (standardmäßig deaktiviert)
*    Einschränken der Zinsfuß Access pro remote-Host (klicken Sie auf als Standard)

Diese basieren auf die Features in dynamische IP-Sicherheit in IIS weiteren dokumentierten. Wenn dieser Konfiguration ändern Beachten Sie die folgenden Faktoren:

* Das Verhalten von Proxys und Network Address Translation Geräte über die Informationen remote-host
* Jede Anforderung an eine Ressource in die Web-Rolle gilt (z. B. geladen Skripts, Bilder, usw.)

## <a name="restricting-number-of-concurrent-accesses"></a>Einschränken der Anzahl gleichzeitiger Zugriffe

Die Einstellungen, die dieses Verhalten konfigurieren sind:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Ändern von DynamicIpRestrictionDenyByConcurrentRequests auf True, wenn dieser Schutz aktivieren.

## <a name="restricting-rate-of-access"></a>Zinsfuß Zugriff einschränken

Die Einstellungen, die dieses Verhalten konfigurieren sind:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Konfigurieren der Antwort auf eine abgelehnte Anforderung

Die folgende Einstellung konfiguriert die Antwort auf die Anforderung einer abgelehnten:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Finden Sie in der Dokumentation für dynamische IP-Sicherheit in IIS zu anderen unterstützten Werten.

## <a name="operations-for-configuring-service-certificates"></a>Vorgänge für das Konfigurieren der Dienstzertifikate
In diesem Thema wird nur zu Referenzzwecken. Führen Sie die Konfigurationsschritte in:

* Konfigurieren des SSL-Zertifikats
* Konfigurieren von Clientzertifikaten

## <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats
Ausführen:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

So passen Sie an

*    -n mit dem Dienst-URL. Platzhalter ("CN = * .cloudapp .net") und alternative Namen ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") unterstützt werden.
*    e - mit Ablaufdatum des Zertifikats ein sicheres Kennwort erstellen und angeben, wenn Sie dazu aufgefordert werden.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Erstellen von PFX-Datei für ein selbst signiertes Zertifikat Zertifizierungsstelle

Ausführen:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Geben Sie Ihr Kennwort ein, und klicken Sie dann exportieren Sie Zertifikat mit den folgenden Optionen:
* Ja, privaten Schlüssel exportieren
* Alle erweiterten Eigenschaften exportieren

## <a name="export-ssl-certificate-from-certificate-store"></a>Exportieren von SSL-Zertifikat Zertifikat Store

* Suchen nach Zertifikat
* Klicken Sie auf Aktionen -> alle Vorgänge -> Exportieren...
* Exportieren des Zertifikats in ein. PFX-Datei mit folgenden Optionen:
    * Ja, privaten Schlüssel exportieren
    * Falls möglich, alle Zertifikate in den Zertifizierungspfad einbeziehen * alle erweiterte Eigenschaften exportieren

## <a name="upload-ssl-certificate-to-cloud-service"></a>Hochladen von SSL-Zertifikat auf Cloud-Dienst

Hochladen mit den vorhandenen Serverzertifikat oder generiert. PFX-Datei mit dem Key SSL-Paar:

* Geben Sie das Kennwort schützen von Informationen zum privaten Schlüssel

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Aktualisieren von SSL-Zertifikat im Konfigurations-Dienstdatei

Aktualisieren Sie den Fingerabdruckwert für die folgende Einstellung in der Dienstkonfigurationsdatei, mit dem Fingerabdruck des Zertifikats Cloud-Dienst geladen:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Importieren von SSL-Zertifizierungsstelle

Gehen Sie folgendermaßen in allen Konto/Computer, die mit dem Dienst kommunizieren vor:

* Doppelklicken Sie auf die. CER-Datei in Windows-Explorer
* Klicken Sie im Dialogfeld Zertifikat auf Zertifikat installieren...
* Importieren von Zertifikat in den Trusted Root Zertifizierungsstellen-Speicher

## <a name="turn-off-client-certificate-based-authentication"></a>Deaktivieren von Zertifikaten basierende Client-Authentifizierung

Nur-Client Zertifikaten basierende Authentifizierung unterstützt wird, und deaktivieren können für den öffentlichen Zugriff auf die Endpunkte, es sei denn, andere Methoden direkte (z. B. Microsoft Azure Virtual Network) sind.

Ändern Sie diese Einstellungen falsch in der Dienstdatei Konfiguration, um das Feature zu deaktivieren:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Kopieren Sie dann denselben Fingerabdruck wie das SSL-Zertifikat der CA Zertifikat Einstellung an:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Erstellen einer selbstsignierten Zertifizierungsstelle
Führen Sie die folgenden Schritte aus, um ein selbst signiertes Zertifikat als Zertifizierungsstelle fungieren zu erstellen:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

So anzupassen

*    e - mit dem Ablaufdatum Zertifizierung


## <a name="find-ca-public-key"></a>Suchen nach öffentlichen Schlüssel Zertifizierungsstelle

Alle Clientzertifikate müssen von einer vom Dienst vertrauenswürdigen Zertifizierungsstelle ausgestellt wurden. Suchen Sie den öffentlichen Schlüssel an die Zertifizierungsstelle, die den Client Zertifikate ausgestellt, die für die Authentifizierung verwendet werden, damit es in der Cloud-Dienst hochladen abgelegt werden.

Wenn die Datei mit dem öffentlichen Schlüssel nicht verfügbar ist, werden exportieren Sie diese aus dem Store Zertifikat:

* Suchen nach Zertifikat
    * Suchen nach ein Client-Zertifikat von derselben Zertifizierungsstelle ausgestellt
* Doppelklicken Sie auf das Zertifikat.
* Wählen Sie die Registerkarte Zertifizierungspfad, klicken Sie im Dialogfeld Zertifikat.
* Doppelklicken Sie auf den Pfad Zertifizierungsstelle.
* Erstellen von Notizen Eigenschaften der Zertifikat.
* Schließen Sie das Dialogfeld ' **Zertifikat** ' ein.
* Suchen nach Zertifikat
    * Suchen Sie nach der oben genannten Zertifizierungsstelle.
* Klicken Sie auf Aktionen -> alle Vorgänge -> Exportieren...
* Exportieren des Zertifikats in ein. CER mit den folgenden Optionen:
    * **Nein, privaten Schlüssel nicht exportieren**
    * Alle Zertifikate falls möglich in den Zertifizierungspfad einbeziehen.
    * Exportieren Sie alle erweiterte Eigenschaften ein.

## <a name="upload-ca-certificate-to-cloud-service"></a>Zertifikat zum Cloud-Dienst hochladen

Hochladen mit den vorhandenen Serverzertifikat oder generiert. CER-Datei mit dem öffentlichen Schlüssel CA.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Update CA Zertifikat im Konfigurations-Dienstdatei

Aktualisieren Sie den Fingerabdruckwert für die folgende Einstellung in der Dienstkonfigurationsdatei, mit dem Fingerabdruck des Zertifikats Cloud-Dienst geladen:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Aktualisieren Sie den Wert für die folgende Einstellung mit denselben Fingerabdruck:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Clientzertifikate ausgeben

Jede Person, die den Zugriff auf Dienste berechtigt sollte ein Client-Zertifikat ausgestellt für setzt sein ausschließlich verwenden und sollten auswählen, dass setzt sein sicheres Kennwort zum Schützen des zugehörigen privaten Schlüssels besitzen. 

Die folgenden Schritte müssen auf dem gleichen Computer ausgeführt werden, in dem das selbst signierte Zertifikat von Zertifizierungsstelle generiert und gespeichert wurde:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Anpassen:

* n - für eine ID, die an den Client, der mit diesem Zertifikat authentifiziert wird
* e - mit Ablaufdatum des Zertifikats
* MyID.pvk und MyID.cer mit eigenen Dateinamen für dieses Clientzertifikat

Dieser Befehl fordert für ein Kennwort erstellt, und klicken Sie dann auf einmal verwendet werden. Verwenden Sie ein sicheres Kennwort ein.

## <a name="create-pfx-files-for-client-certificates"></a>Erstellen von PFX-Dateien für Kunden Zertifikate

Führen Sie für jedes Clientzertifikat generierten zu können aus:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Anpassen:

    MyID.pvk and MyID.cer with the filename for the client certificate

Geben Sie Ihr Kennwort ein, und klicken Sie dann exportieren Sie Zertifikat mit den folgenden Optionen:

* Ja, privaten Schlüssel exportieren
* Alle erweiterten Eigenschaften exportieren
* Wählen Sie die Person, die dieses Zertifikat ausgestellt wird, sollte das Kennwort exportieren

## <a name="import-client-certificate"></a>Client-Zertifikat importieren

Jede Person, für wen ein Client-Zertifikat ausgestellt wurde, sollten Paar aus auf den Computern der Schlüssel importieren, die er zur Kommunikation mit dem Dienst verwendet wird:

* Doppelklicken Sie auf die. PFX-Datei in Windows-Explorer
* Zertifikat importieren in der persönlich speichern mit mindestens diese Option:
    * Schließt alle erweiterten Eigenschaften aktiviert

## <a name="copy-client-certificate-thumbprints"></a>Kopieren der Zertifikatfingerabdruck client
Jede Person, für wen ein Client-Zertifikat ausgestellt wurde, wie folgt vor um erhalten den Fingerabdruck des setzt sein Zertifikat, das zur Konfigurationsdatei hinzugefügt werden:
* Certmgr.exe ausführen
* Wählen Sie aus der Registerkarte Persönliche Einstellungen
* Doppelklicken Sie auf das Client-Zertifikat für die Authentifizierung verwendet werden
* Wählen Sie im Dialogfeld Zertifikat, das geöffnet wird, auf der Registerkarte Details
* Sicherstellen Sie, dass alle anzeigt anzeigen
* Wählen Sie das Feld namens Fingerabdruck in der Liste
* Kopieren Sie den Wert von der Fingerabdruck **Löschen nicht sichtbare Unicode-Zeichen vor der letzten Ziffer** Löschen aller Leerzeichen

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Konfigurieren von zulässige Kunden in der Konfiguration Dienstdatei

Aktualisieren Sie den Wert für die folgende Einstellung in der Konfiguration Dienstdatei mit einer durch Trennzeichen getrennte Liste der Fingerabdrücke von Client-Zertifikate, die Zugriff auf den Dienst zulässig:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Konfigurieren von Client Zertifikat Zertifikatssperrüberprüfung

Die Standardeinstellung wird bei der Zertifizierungsstelle Client Zertifikat Sperrstatus nicht überprüft. Ändern Sie zum Aktivieren der Prüfungen, wenn Zertifizierungsstelle, die den Client Zertifikate ausgestellt solche Prüfungen unterstützt folgende Einstellung mit einer der Werte in der X509RevocationMode-Enumeration definiert:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Erstellen von PFX-Datei für selbstsignierten Verschlüsselungszertifikaten

Führen Sie für eine Verschlüsselungszertifikats:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Anpassen:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Geben Sie Ihr Kennwort ein, und klicken Sie dann exportieren Sie Zertifikat mit den folgenden Optionen:
*    Ja, privaten Schlüssel exportieren
*    Alle erweiterten Eigenschaften exportieren
*    Sie benötigen das Kennwort ein, wenn das Zertifikat in der Cloud-Dienst hochladen.

## <a name="export-encryption-certificate-from-certificate-store"></a>Exportieren von Verschlüsselungszertifikats Zertifikat Store

*    Suchen nach Zertifikat
*    Klicken Sie auf Aktionen -> alle Vorgänge -> Exportieren...
*    Exportieren des Zertifikats in ein. PFX-Datei mit folgenden Optionen: 
  *    Ja, privaten Schlüssel exportieren
  *    Falls möglich, alle Zertifikate in den Zertifizierungspfad einbeziehen 
*    Alle erweiterten Eigenschaften exportieren

## <a name="upload-encryption-certificate-to-cloud-service"></a>Hochladen von Verschlüsselungszertifikats in Cloud-Dienst

Hochladen mit den vorhandenen Serverzertifikat oder generiert. PFX-Datei für die Verschlüsselung:

* Geben Sie das Kennwort schützen von Informationen zum privaten Schlüssel

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Aktualisieren von Verschlüsselungszertifikats in Konfiguration Dienstdatei

Aktualisieren des Fingerabdruckwerts, der die folgenden Einstellungen in der Konfiguration Dienstdatei mit dem Fingerabdruck des Zertifikats Cloud-Dienst geladen:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Allgemeine Zertifikatvorgänge

* Konfigurieren des SSL-Zertifikats
* Konfigurieren von Clientzertifikaten

## <a name="find-certificate"></a>Suchen nach Zertifikat

Gehen Sie folgendermaßen vor:

1. Führen Sie mmc.exe ein.
2. Datei-Snap-in hinzufügen/entfernen >...
3. Wählen Sie **Zertifikate**aus.
4. Klicken Sie auf **Hinzufügen**.
5. Wählen Sie den Speicherort Zertifikat aus.
6. Klicken Sie auf **Fertig stellen**.
7. Klicken Sie auf **OK**.
8. Erweitern Sie **Zertifikate**aus.
9. Erweitern Sie Zertifikat Store.
10. Erweitern Sie den Zertifikat untergeordneten Knoten aus.
11. Wählen Sie ein Zertifikat in der Liste aus.

## <a name="export-certificate"></a>Exportieren von Zertifikaten
Das **Zertifikat Export-Assistent**:

1. Klicken Sie auf **Weiter**.
2. Wählen Sie **Ja**, klicken Sie dann auf **den privaten Schlüssel exportieren**.
3. Klicken Sie auf **Weiter**.
4. Wählen Sie das gewünschte Ausgabeformat für die Datei ein.
5. Aktivieren Sie die gewünschten Optionen.
6. Aktivieren Sie **das Kennwort**ein.
7. Geben Sie ein sicheres Kennwort ein, und bestätigen Sie es.
8. Klicken Sie auf **Weiter**.
9. Geben Sie ein, oder suchen Sie einem Dateinamen, wo das Zertifikat gespeichert (verwenden Sie ein. PFX-Erweiterung).
10. Klicken Sie auf **Weiter**.
11. Klicken Sie auf **Fertig stellen**.
12. Klicken Sie auf **OK**.

## <a name="import-certificate"></a>Zertifikat importieren

Das Zertifikat Textimport-Assistenten:

1. Wählen Sie den Speicherort aus.

    * Wählen Sie **Aktuelle Benutzer** aus, wenn nur Prozesse weiterhin ausgeführt wird, klicken Sie unter aktuelle Benutzer auf den Dienst zugreifen
    * Wählen Sie **Lokalen Computer** aus, wenn andere Prozesse auf diesem Computer auf den Dienst zugreifen
2. Klicken Sie auf **Weiter**.
3. Wenn Sie Daten aus einer Datei importieren, bestätigen Sie den Dateipfad.
4. Beim Importieren von ein. PFX-Datei:
    1.     Geben Sie das Kennwort schützen von privaten Schlüssel
    2.     Wählen Sie Optionen für den Import aus.
5.     Wählen Sie "Ort" Zertifikate in folgendem Speicher
6.     Klicken Sie auf **Durchsuchen**.
7.     Wählen Sie den gewünschten Speicher aus.
8.     Klicken Sie auf **Fertig stellen**.
       
    * Wenn der vertrauenswürdige Stammzertifizierungsstelle Store ausgewählt wurde, klicken Sie auf **Ja**.
9.     Klicken Sie auf **OK** , klicken Sie auf alle Dialogfelder.

## <a name="upload-certificate"></a>Hochladen des Zertifikats

Im [Azure-Portal](https://portal.azure.com/)

1. Wählen Sie die **Cloud-Dienste**.
2. Wählen Sie aus der Cloud-Dienst.
3. Klicken Sie im oberen Menü auf **Zertifikate**.
4. Klicken Sie auf der Leiste am unteren Rand auf **Hochladen**.
5. Wählen Sie die Zertifikatsdatei aus.
6. Es ist ein. PFX-Datei, geben Sie das Kennwort für den privaten Schlüssel.
7. Nachdem abgeschlossen ist, kopieren Sie den Fingerabdruck des Zertifikats aus den neuen Eintrag in der Liste aus.

## <a name="other-security-considerations"></a>Weitere Sicherheitsaspekte
 
Die SSL-Einstellungen in diesem Dokument beschriebenen Verschlüsseln der Kommunikation zwischen dem Dienst und seinen Clients aus, wenn der HTTPS-Endpunkt verwendet wird. Dies ist seit Anmeldeinformationen für den Zugriff auf wichtige und potenziell andere vertrauliche Informationen in die Kommunikation enthalten sind. Beachten Sie jedoch, dass der Dienst weiterhin besteht, internen Status, einschließlich der Anmeldeinformationen, in dem ihre internen Tabellen in der Microsoft Azure SQL-Datenbank, die Sie in Ihrem Microsoft Azure-Abonnement für Metadaten-Speicher bereitgestellt haben. Die Datenbank als Teil des die folgende Einstellung in Ihrem Dienstkonfigurationsdatei definiert wurde (. CSCFG-Datei): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

In dieser Datenbank gespeicherte Anmeldeinformationen verschlüsselt sind. Jedoch als bewährte Methode, sicherstellen Sie, dass sowohl Web- und Worker Rollen von der Bereitstellung von Diensten auf dem neuesten Stand gehalten werden und sichere, sobald sie verfügen über Zugriff auf die Datenbank Metadaten und das Zertifikat zum Verschlüsseln und Entschlüsseln des gespeicherten Anmeldeinformationen. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
