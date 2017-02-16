<properties
    pageTitle="Linux Gäste auf Azure Stapel | Microsoft Azure"
    description="Erfahren Sie, wie Linux-basierten virtuellen Maschinen Azure Stapel erstellen."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Bereitstellen Sie auf Stapel Azure-virtuellen Computern Linux

Sie können virtuelle Linux-Computer auf der Azure Stapel Prüfung des Konzepts ist durch Hinzufügen eines Bilds Linux-basierten in den Stapel Azure Marketplace bereitstellen. Mehrere Linux-Anbietern gewährt Bilder, die in einer Azure Stapel Prüfung des Konzepts ist hinzugefügt werden können, oder eigene erstellen.

## <a name="download-an-image"></a>Herunterladen eines Bilds

 1. Herunterladen und extrahieren ein Azure Stapel-kompatibler Bild aus den folgenden Links oder eigene vorbereiten:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Extrahieren Sie das Bild virtuelle Festplatte bei Bedarf, und [Fügen Sie das Bild zu Marketplace hinzu](azure-stack-add-vm-image.md). Stellen Sie sicher, dass die `OSType` Parameter so eingerichtet ist `Linux`.
 
 3. Nachdem Sie das Bild zu Marketplace hinzugefügt haben, ein Marketplace-Element erstellt wird und Sie können eine Linux virtuellen Computern bereitstellen.
  
## <a name="prepare-your-own-image"></a>Bereiten Sie ein eigenes Bild vor.

1. Bereiten Sie ein eigenes Bild Linux mithilfe einer der folgenden Anweisungen vor:
 - [CentOS-basierten Verteilung](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle-Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Red Hat Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Herunterladen Sie und installieren Sie der [Azure Linux-Agent](https://github.com/Azure/WALinuxAgent/)

    Der Azure Linux Agent Version 2.1.3 oder höher ist erforderlich, um Ihre Linux VM Azure Stapel bereitstellen. Viele der Verteilung bereits aufgeführten dieser Version des Agents oder höher als Paket in ihre Repositorys enthalten (in der Regel aufgerufen `WALinuxAgent` oder `walinuxagent`). Jedoch die Version des Agentenpakets Azure-ist kleiner als 2.1.3 (d. h. 2.0.18 oder niedriger), und klicken Sie dann Sie den Agent manuell installieren müssen. Die installierte Version bestimmt werden kann, aus dem Paketnamen oder durch Ausführen `/usr/sbin/waagent -version` des virtuellen Computers.

    Den folgenden Anweisungen des Linux Azure-Agents manuell installiert-

 - Laden Sie zunächst die neuesten Azure Linux-Agents aus [Github](https://github.com/Azure/WALinuxAgent/releases), Beispiel:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Entpacken des Azure-Agents:

            # tar -vzxf v2.2.0.tar.gz

 - Installieren Sie Python-setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Installieren des Azure-Agents:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Betriebssysteme mit Python 2.x und Python 3.x installiert-nebeneinander möglicherweise müssen Sie den folgenden Befehl ausführen:

        # sudo python3 setup.py install --register-service

    Weitere Informationen finden Sie unter der Azure Linux Agent [Infos](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Hinzufügen des Bilds zum Marketplace](azure-stack-add-vm-image.md). Stellen Sie sicher, dass die `OSType` Parameter so eingerichtet ist `Linux`.

4. Nachdem Sie das Bild zu Marketplace hinzugefügt haben, ein Marketplace-Element erstellt wird und Sie können eine Linux virtuellen Computern bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

[Häufig gestellte Fragen zu Azure Stapel](azure-stack-faq.md)