<properties
    pageTitle="Installieren von MySQL einer OpenSUSE virtuellen Computers | Microsoft Azure"
    description="Sie lernen, wie auf einem Computer OpenSUSE Linux VMirtual in Azure MySQL installieren."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Installieren von MySQL auf einen virtuellen Computer mit OpenSUSE Linux in Azure

[MySQL] [ MySQL] ist eine beliebte, Open Source SQL-Datenbank. In diesem Lernprogramm erfahren Sie, wie Erstellen eines virtuellen Computers OpenSUSE Linux ausgeführt, und dann MySQL installieren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Erstellen eines virtuellen Computers mit OpenSUSE Linux

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Installieren und Ausführen von MySQL des virtuellen Computers

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Nächste Schritte
Details zu MySQL, finden Sie in der [MySQL-Dokumentation][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

