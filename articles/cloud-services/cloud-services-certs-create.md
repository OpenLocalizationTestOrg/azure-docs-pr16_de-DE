<properties 
    pageTitle="Cloud Services und Management Zertifikate | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Verwenden von Zertifikaten mit Microsoft Azure" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Übersicht über Zertifikate für Azure Cloud Services
Zertifikate werden in Azure Cloud Services ([Service Zertifikate](#what-are-service-certificates)) und für die Authentifizierung bei der Verwaltung API ([Management Zertifikate](#what-are-management-certificates) bei Verwendung des Azure klassischen Portal und nicht Cloud) verwendet. Dieses Thema bietet eine allgemeine Übersicht über beider Arten Zertifikat zum [Erstellen](#create) und [Bereitstellen](#deploy) , um Azure.

Mithilfe von Zertifikaten in Azure sind x. 509 v3-Zertifikate und können mit einem anderen vertrauenswürdigen Zertifikat signiert werden, oder sie können selbstsignierten sein. Ein selbst signiertes Zertifikat durch einen eigenen Ersteller und aus diesem Grund angemeldet ist, sind standardmäßig nicht vertrauenswürdig. Die meisten Browser können dies ignorieren. Selbstsignierte Zertifikaten sollte nur von selbst beim Entwickeln und Testen Ihre Cloud-Diensten verwendet werden. 

Von Azure verwendete Zertifikate können einem privaten oder einen öffentlichen Schlüssel enthalten. Zertifikate haben einen Fingerabdruck, der bietet eine Möglichkeit, diese in eine Möglichkeit eindeutig identifizieren. Dieses Fingerabdrucks wird in der Azure [Konfigurationsdatei](cloud-services-configure-ssl-certificate.md) zur welches Zertifikat identifizieren ein Clouddienst verwendet werden sollen. 

## <a name="what-are-service-certificates"></a>Was sind Zertifikate Service?
Dienstzertifikate werden angefügt, um cloud Services und sicheren Kommunikation an und von den Dienst aktivieren. Beispielsweise, wenn Sie eine Webrolle bereitgestellt haben, möchten Sie ein Zertifikat bereitstellen, die einen verfügbar gemachten HTTPS-Endpunkt authentifiziert werden kann. Dienstzertifikate, in der Definition Ihrer Service definiert werden automatisch mit dem virtuellen Computer bereitgestellt, die eine Instanz von Ihrer Rolle ausgeführt wird. 

Sie können den Dienstzertifikate Azure klassischen Portal entweder mithilfe des klassischen Azure-Portals oder mithilfe der Service Management-API hochladen. Dienstzertifikate sind mit einem bestimmten Cloud-Dienst verbunden sind und eine Bereitstellung in der Definition Dienstdatei zugewiesen.

Dienstzertifikate aus Ihrer Dienste separat verwaltet werden können, und möglicherweise von anderen Personen verwaltet werden. Ein Entwickler kann beispielsweise ein Service-Paket hochladen, das auf ein Zertifikat verweist, die ein IT-Manager zuvor auf Azure hochgeladen wurde. Ein IT-Manager können Sie verwalten und ändern die Konfiguration des Diensts ohne ein neues Servicepaket hochladen, dass das Zertifikat erneuern. Dies ist möglich, da der logische Name für das Zertifikat und seine Store Name und Speicherort in der Dienstdatei Definition angegeben sind, während der Fingerabdruck des Zertifikats in der Konfiguration Dienstdatei angegeben ist. Um das Zertifikat zu aktualisieren, ist es nur ein neues Zertifikat hochladen, und ändern Sie den Fingerabdruckwert in der Konfiguration Dienstdatei erforderlich.

## <a name="what-are-management-certificates"></a>Was sind Management Zertifikate?
Verwaltung von Zertifikaten können Sie mit der Verwaltung von Azure klassischen bereitgestellten API authentifizieren. Viele Programme und Tools (z. B. Visual Studio oder das Azure SDK) werden diese Zertifikate zum Automatisieren von Konfiguration und Bereitstellung von verschiedenen Azure Services verwenden. Diese wirklich beziehen sich nicht in die cloud Services. 

>[AZURE.WARNING] Sei vorsichtig! Diese Arten von Zertifikaten zulassen jede Person, die authentifiziert mit ihnen das Abonnement zu verwalten, denen, dem Sie zugeordnet sind. 

### <a name="limitations"></a>Einschränkungen
Es gibt maximal 100 Management Zertifikate pro Abonnement aus. Es gibt auch maximal 100 Management Zertifikate für alle Abonnements unter einer bestimmten Dienstadministrator Benutzer-ID an. Wenn die Benutzer-ID für das Kontoadministrator bereits verwendet wurde, um 100 Management Zertifikate hinzufügen, und es eine Notwendigkeit mehr Zertifikate ist, können Sie einen gemeinsame Administrator aus, um die zusätzliche Zertifikate hinzufügen hinzufügen. 

Vor dem Hinzufügen von mehr als 100 Zertifikate, angezeigt, wenn Sie ein vorhandenes Zertifikat wiederverwenden können. Verwenden von Co-Administratoren hinzugefügt Zertifikat Management Prozesses potenziell überflüssige Komplexität.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Erstellen eines neuen selbstsignierten Zertifikats
Sie können alle Tool erstellt ein selbst signiertes Zertifikat, solange diese Einstellungen beachtet werden:

* Ein x. 509-Zertifikat.
* Enthält einen privaten Schlüssel.
* Für Key Exchange (PFX-Datei) erstellt.
* Betreff-Name muss die Zugriff auf die Clouddienst verwendete Domäne übereinstimmen. 
    > Eine SSL-Zertifikat für die cloudapp.net (oder für alle Azure Zusammenhang) Domäne kann nicht abgerufen werden; der Name des Zertifikats Betreff muss den benutzerdefinierten Domänennamen verwendet, um die Anwendung zugreifen übereinstimmen. Zum Beispiel **contoso.net**, nicht **contoso.cloudapp.net**.
* Mindestens 2048-Bit-Verschlüsselung.
* **Nur Zertifikat Dienst**: clientseitige Zertifikat muss im *persönlichen* Zertifikats Store befinden.

Es gibt zwei einfache Methoden, erstellen ein Zertifikat unter Windows, mit der `makecert.exe` Programm oder IIS.

### <a name="makecertexe"></a>MakeCert.exe

Dieses Programm ist veraltet und nicht mehr hier beschrieben. Lesen Sie [diese MSDN-Artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968) für Weitere Informationen.

### <a name="powershell"></a>PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Wenn Sie eine IP-Adresse statt einer Domäne das Zertifikat verwenden möchten, verwenden Sie die IP-Adresse in den Parameter - DNS-Name.


Wenn Sie dieses [Zertifikat mit Verwaltungsportal](../azure-api-management-certs.md)verwenden möchten, in eine **CER** -Datei exportieren:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)

Es gibt viele Seiten im Internet, die mit IIS dazu Deckblatt aus. [Hier](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) ist eine großartige ich festgestellt, dass ich denke auch erklärt. 

### <a name="java"></a>Java
Sie können Java erstellen Sie [ein Zertifikat](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate)verwenden.

### <a name="linux"></a>Linux
[Dieser](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) Artikel beschreibt, wie Sie Zertifikate mit SSH zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

[Hochladen das Dienstzertifikat zum klassischen Azure-portal](cloud-services-configure-ssl-certificate.md) (oder das [Azure-Portal](cloud-services-configure-ssl-certificate-portal.md)).

Hochladen eines [Management API Zertifikat](../azure-api-management-certs.md) klassischen Azure-Portal an.

>[AZURE.NOTE] Azure-Portal verwenden Management Zertifikate nicht auf die API zugreifen, sondern Benutzerkonten verwendet.
