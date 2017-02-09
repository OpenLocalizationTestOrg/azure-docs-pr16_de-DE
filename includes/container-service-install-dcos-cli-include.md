<properties
   pageTitle="Installieren Sie das DC/Betriebssystem CLI | Microsoft Azure"
   description="Installieren Sie das DC/Betriebssystem CLI."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="DC/OS, Azure Container, Micro-Dienste"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Dies ist für die Arbeit mit ACS DC, OS-basierten Cluster. Es ist nicht erforderlich für ACS-basierten Punktschwarms Cluster Aktion ein.

[Verbinden mit Ihren Cluster DC, OS-basierten ACS](../articles/container-service/container-service-connect.md)zuerst. Nachdem Sie dies getan haben, können Sie die DC/OS CLI auf dem Clientcomputer mit den folgenden Befehlen installieren:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Wenn Sie eine ältere Version des Python verwenden, werden Sie möglicherweise einige "InsecurePlatformWarnings" fest. Sie können diese ignorieren.

Um ohne Neustart Ihrer Shell anzufangen, führen Sie zu können aus:

```bash
source ~/.bashrc
```

Dieser Schritt werden beim Starten von neuer Schalen nicht erforderlich.

Jetzt können Sie bestätigen, dass die CLI installiert ist:

```bash
dcos --help
```