<properties
    pageTitle="Installieren Sie auf einem Windows-virtuellen Computer MongoDB | Microsoft Azure"
    description="Erfahren Sie, wie MongoDB einer Azure virtuellen Computers erstellt, mit dem klassischen Bereitstellungsmodell unter Windows Server zu installieren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>MongoDB auf einem Windows-virtuellen Computer installieren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zum Installieren und verwenden das Modell zur Bereitstellung von Ressourcenmanager MongoDB konfiguriert haben, finden Sie [in diesem Artikel](virtual-machines-windows-classic-install-mongodb.md)aus.

[MongoDB] [ MongoDB] ist eine beliebte Source-öffnen, leistungsfähige NoSQL-Datenbank. In diesem Artikel schrittweise Anleitung zum Erstellen eines Windows Server virtuellen Computers (virtueller Computer) über das [Azure klassischen Portal][AzurePortal]. Erstellen, und fügen Sie einen Datenträger an den virtuellen Computer vor dem Installieren und Konfigurieren von MongoDB ein. Wenn Sie eine vorhandene virtueller Computer in Azure, die Sie verwenden möchten verfügen, können Sie direkt zu [Installieren und Konfigurieren von MongoDB](#install-and-run-mongodb-on-the-virtual-machine)springen.


## <a name="create-a-virtual-machine-running-windows-server"></a>Erstellen einer virtuellen Computern unter Windows Server

Befolgen Sie diese Anweisungen zum Erstellen eines virtuellen Computers.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Fügen Sie einen Endpunkt für MongoDB beim Erstellen des virtuellen Computers hinzu, und konfigurieren Sie ihn wie folgt: Nennen Sie es als **Mongo**, **TCP** als Protokoll verwenden, und legen Sie die öffentlichen und privaten Ports auf **27017**.

## <a name="attach-a-data-disk"></a>Fügen Sie einen Datenträger
Fügen Sie einen Datenträger Speicher des virtuellen Computers zum Bereitstellen und dann Initialisierung es so, dass er von Windows verwendet werden können. Wenn Sie bereits über einen Datenträger verfügen, können Sie die vorhandenen Datenträger anfügen, oder Sie können einen leeren Datenträger anfügen.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Klicken Sie auf der Festplatte Anweisungen finden Sie unter "so: Initialisierung einen neuen Datenträger in WindowsServer" in [so einen Datenträger Daten auf einem Windows-Computer installieren](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Installieren und Ausführen von MongoDB des virtuellen Computers

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Zusammenfassung
In diesem Lernprogramm haben Sie zum Erstellen einer virtuellen Computern unter Windows Server, Remote zu verbinden, und fügen Sie einen Datenträger aus.  Außerdem haben Sie erfahren, wie installieren und Konfigurieren von MongoDB auf dem Windows-basierten virtuellen Computer. Sie können nun MongoDB auf dem Windows-basierten virtuellen Computer zugreifen, indem Sie die folgenden erweiterten Themen in der [Dokumentation MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
