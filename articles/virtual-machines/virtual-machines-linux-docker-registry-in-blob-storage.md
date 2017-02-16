<properties 
  pageTitle="Bereitstellen von eigene Private Docker Registrierung auf Azure | Microsoft Azure"
  description="Beschreibt, wie Sie Docker Registrierung Ihrer Bilder Container Azure BLOB-Speicher-Dienst hosten."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Bereitstellen von eigene Private Docker Registrierung auf Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



Dieses Dokument beschreibt, welche Docker private Registry ist und zeigt, wie Sie ein Docker Registrierung 2.0 Container Bild mit einer privaten Docker-Registrierung klicken Sie auf Microsoft Azure mit Azure BLOB-Speicher bereitstellen können.

Dieses Dokument setzt voraus:

1. Sie erfahren, wie Sie Docker verwenden und Docker Bilder gespeichert haben. (Nicht? [Erfahren Sie mehr über Docker](https://www.docker.com))
2. Sie haben einen Server mit Docker-Engine installiert. (Nicht? [Schnell auf Azure tun.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Was ist eine Private Docker Registrierung?

Um Ihre Sammelartikeleinheit Applications in der Cloud zu versenden einer Docker Container Image Erstellen und es dort speichern, sodass sie durch sich selbst und andere verwendet werden kann. 

Beim Erstellen eines Bilds Container, und versenden ihn an der Cloud einfach ist, ist es eine Herausforderung generierten Bilds zuverlässig speichern. Aus diesem Grund bietet Docker aufgerufen [Docker Hub] zentralen Dienst[ docker-hub] zum Speichern von Container Bilder in der Cloud und können Sie jederzeit mit diesen Bildern Container erstellen.

Obwohl die [Docker Hub] [ docker-hub] ist ein kostenpflichtiges Dienst zum Speichern Ihrer privaten Anwendung Containers Bilder, Docker Hinsicht Entwickler Anforderungen und stellt ein offener Quelle Toolset zum Speichern Ihrer Bilder in Ihrem eigenen private Docker Registrierung hinter einer Firewall oder lokalen ohne dafür im öffentliche Internet.
Da Azure Blob-Speicher leicht zu sichern ist, können Sie schnell verwenden sie zum Erstellen und verwenden eine private Docker Registrierung in Azure, die Sie selbst steuern.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Warum sollten Sie eine Docker Registrierung auf Azure hosten?

Klicken Sie auf Microsoft Azure Ihrer Docker Registrierungs-Instanz hostet, und speichern Ihre Bilder auf Azure BLOB-Speicher, können Sie mehrere Vorteile lassen:

**Sicherheit:** Ihre Bilder Docker lassen Azure Rechenzentren, Sie nicht, damit diese im öffentliche Internet nicht überschreiten, als wären Sie Docker Hub verwendet haben.
  
**Leistung:** Ihre Bilder Docker werden als Ihre Applikationen innerhalb derselben Datacenter oder Region gespeichert. Dies bedeutet, dass die Bilder schneller und weitere zuverlässig im Vergleich zu Docker Hub herausgezogen werden.

**Zuverlässigkeit:** Mithilfe von Microsoft Azure BLOB-Speicher können Sie viele Speicher Eigenschaften wie hohe Verfügbarkeit, Redundanz, Premium-Speicher (SSDs), verwenden Sie usw. vornehmen.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Konfigurieren von Docker Registrierung Azure BLOB-Speicher verwenden

(Es wird empfohlen, lesen Sie die [Docker Registrierung 2.0-Dokumentation][Registrierungs-Dokumente] , bevor Sie fortfahren.)

Sie können [Konfigurieren] [ registry-config] der Registrierung Docker auf zwei verschiedene Arten.
Sie können:

1. Verwenden einer `config.yml` Datei. In diesem Fall müssen Sie zum Erstellen eines separaten Docker Bilds über `registry` Bild.
2. Überschreiben Sie die standardmäßige Konfigurationsdatei bis Umgebungsvariablen: Ruft Dinge ohne erstellen und Verwalten von einem separaten Docker Bild.

Aus Gründen der Vereinfachung folgt in diesem Thema Option 2, die Umgebungsvariablen verwenden.

Zum Ausführen Instanz einer Docker Registrierung der:

* Mithilfe von Ihrem Speicher Azure-Konto die Bilder gespeichert
* hört des virtuellen Computers Port 5000
* ist keine Authentifizierung konfiguriert (nicht empfohlen, siehe Anmerkung unten)

Sie müssen den folgenden Befehl aus Docker in Ihrem Bash Terminal ausführen (ersetzen `<storage-account>` und `<storage-key>` mit Ihren Anmeldeinformationen):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Nachdem Sie der Befehl beendet, sehen Sie den Container, Ihre private Docker Registrierungs-Instanz hostet, durch Ausführen der `docker ps` Befehl auf dem Host:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Konfigurieren der Sicherheit für die Registrierung Docker fällt nicht in diesem Dokument aus, und die Registrierung werden jeder Benutzer ohne Authentifizierung standardmäßig zugreifen, wenn Sie den Port an den Port Registrierung klicken Sie auf den Endpunkt des virtuellen Computers zu öffnen oder Lastenausgleich, wenn Sie oben auf den Befehl "Bereitstellung" verwenden.
>
> Lesen Sie bitte die [Docker Registrierung konfigurieren] [ registry-config] Dokumentation erfahren, wie der Registrierung Instanz und Ihre Bilder zu sichern.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Registrierung eingerichtet haben, ist es Zeit, wechseln sie einige weitere verwenden. Beginnen Sie mit der Docker [Registrierungs-Dokumente]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[Registrierungs-Dokumente]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
