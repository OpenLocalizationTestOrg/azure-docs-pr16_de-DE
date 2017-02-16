<properties
   pageTitle="Interagieren mit Service Fabric Cluster mit CLI | Microsoft Azure"
   description="So verwenden Azure CLI Interaktion mit einem Dienst Fabric cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Verwendung der Azure-CLI Interaktion mit einem Dienst Fabric Cluster

Sie können mit dem Dienst Fabric Cluster aus Linux Computern mithilfe der Azure CLI unter Linux interagieren.

Dieser erste Schritt ist Get die neueste Version der CLI aus der Git Rep und in Ihrem Pfad mit den folgenden Befehlen einrichten:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Bei jedem Befehl es unterstützt, können Sie den Namen des Befehls, erhalten Hilfe für diesen Befehl eingeben. Automatische Vervollständigung ist für die Befehle unterstützt. Beispielsweise bietet der folgende Befehl Sie Hilfe für alle Anwendungsbefehle. 

```sh
 azure servicefabric application 
```

Sie können die Hilfe zu einem bestimmten Befehl, wie im folgenden Beispiel gezeigt filtern:

```sh
 azure servicefabric application  create
```

Klicken Sie zum Aktivieren der automatischen Vervollständigung in der CLI führen Sie die folgenden Befehle:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Die folgenden Befehle zum Cluster verbinden, und zeigen Sie die Knoten im Cluster:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Wenn Sie benannte Parameter verwenden, und suchen, was sie sind, können Sie eingeben – nach einem Befehl helfen. Beispiel:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Bei der Verbindung mit einer Umgebung mit mehreren Computern Cluster von einem Computer **, der nicht Teil der Cluster**verwenden Sie den folgenden Befehl aus:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Ersetzen Sie die Kategorie PublicIPorFQDN mit den Real-IP oder FQDN je nach Bedarf an. Bei der Verbindung mit einer Umgebung mit mehreren Computern Cluster von einem Computer **, Teil der Cluster**verwenden Sie den folgenden Befehl aus:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Sie können PowerShell oder CLI Interaktion mit Ihrem Linux Dienst Fabric Cluster über das Azure-Portal erstellt. 

**Vorsicht:** Diese Cluster nicht sicher sind, auf diese Weise können Sie von Ihrem ein-Feld Öffnen werden, durch die öffentliche IP-Adresse im Cluster Manifest hinzufügen.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Verwendung der Azure-CLI für die Verbindung zu einem Dienst Fabric Cluster

Die folgenden Azure CLI-Befehle beschrieben, wie die Verbindung zu einem sicheren Cluster. Die Zertifikatdetails müssen ein Zertifikat auf dem Clusterknoten übereinstimmen.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Wenn Ihr Zertifikat Zertifizierungsstellen (Zertifizierungsstellen) vorhanden ist, müssen Sie den Parameter – ca-Zertifikat-Path wie im folgenden Beispiel hinzufügen: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Wenn Sie mehrere Zertifizierungsstellen verfügen, verwenden Sie ein Komma als Trennzeichen verwendet.
 
Wenn der allgemeine Name im Zertifikat nicht den Verbindungsendpunkt übereinstimmt, könnten, verwenden Sie den Parameter `--strict-ssl` zum Umgehen der Überprüfung Siehe in den folgenden Befehl aus: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Wenn Sie die Überprüfung CA überspringen möchten, könnten Sie hinzufügen den – nicht autorisiert ablehnen Parameter wie in den folgenden Befehl aus: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Nachdem Sie eine Verbindung herstellen, sollten Sie andere CLI-Befehle für die Interaktion mit dem Cluster ausführen sein. 

## <a name="deploying-your-service-fabric-application"></a>Bereitstellen der Fabric Service-Anwendung

Führen Sie die folgenden Befehle zu kopieren, zu registrieren, und Starten der Fabric Anwendungsdienst an:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Aktualisieren Ihrer Anwendung

Der Vorgang ist ähnlich wie der [Prozess in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Erstellen, kopieren, registrieren und Projektstammverzeichnis Ihrer Anwendung erstellt. Wenn Ihre Anwendungsinstanz Fabric heißt: / MySFApp und den Typ MySFApp, wäre die Befehle wie folgt:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Nehmen Sie die Änderung an Ihrer Anwendung, und erstellen Sie den geänderten Dienst neu.  Aktualisieren Sie die geänderten Dienst Manifestdatei (ServiceManifest.xml) mit den aktualisierten Versionen für den Dienst (und Code oder Config oder Daten nach Bedarf). Ändern Sie auch Anwendungsmanifest (ApplicationManifest.xml) mit den aktualisierten Versionsnummer für die Anwendung, und der geänderte Dienst.  Nun kopieren und Registrieren der aktualisierten Anwendungs mithilfe der folgenden Befehle:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Nun können Sie die Aktualisierung der Anwendung mit dem folgenden Befehl starten:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Sie können nun die Aktualisierung der Anwendung mit umfassende überwachen. In wenigen Minuten würde die Anwendung wurden aktualisiert.  Auch können Sie eine aktualisierte app mit einem Fehler versuchen und aktivieren Sie automatische Funktion Service-Struktur.

## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopieren des Anwendungspakets erfolgreich nicht

Überprüfen Sie, ob `openssh` installiert ist. Standardmäßig ist nicht Ubuntu-Desktop installiert. Installieren Sie es mit dem folgenden Befehl aus:

```
 sudo apt-get install openssh-server openssh-client**
```

Wenn das Problem weiterhin besteht, versuchen Sie PAM für ssh durch Ändern der Datei **Sshd_config** mit den folgenden Befehlen deaktivieren:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Wenn das Problem weiterhin besteht, vergrößern Sie die Anzahl der ssh Sitzungen durch Ausführen der folgenden Befehle:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Mithilfe von Tasten für ssh Authentifizierung (im Gegensatz zu Kennwörtern) noch nicht unterstützt wird (da die Plattform ssh verwendet, um Pakete kopieren), verwenden Sie stattdessen also Kennwortauthentifizierung.


## <a name="next-steps"></a>Nächste Schritte

Einrichten der Entwicklungsumgebung und eine Fabric Service-Anwendung zu einem Linux Cluster bereitstellen.
