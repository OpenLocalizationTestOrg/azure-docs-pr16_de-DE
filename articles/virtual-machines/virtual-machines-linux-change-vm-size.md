<properties
   pageTitle="So ändern Sie die Größe einer Linux VM | Microsoft Azure"
   description="Informationen zum Vergrößern oder Verkleinern eines virtuellen Computers Linux durch Ändern der Größe des virtuellen Computer."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>So ändern Sie die Größe einer Linux VM

## <a name="overview"></a>(Übersicht) 

Nachdem Sie einen virtuellen Computer (virtueller Computer) bereitstellen, können Sie skalieren den virtuellen Computer nach oben oder nach unten durch Ändern der [Größe des virtuellen Computer][vm-sizes]. In einigen Fällen müssen Sie zuerst den virtuellen Computer freigeben. Dies kann geschehen, wenn die neue Größe nicht auf dem Cluster Hardware verfügbar ist, die den virtuellen Computer gehostet wird.

In diesem Artikel wird das Ändern der Größe einer Linux VM mithilfe der [Azure CLI][azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.


## <a name="resize-a-linux-vm"></a>Ändern der Größe eines Linux virtuellen Computers 

Führen Sie zum Ändern der Größe eines virtuellen Computers die folgenden Schritte aus.

1. Führen Sie den folgenden CLI-Befehl ein. Dieser Befehl listet die Größen virtueller Computer, die auf dem Cluster Hardware verfügbar sind, der virtuellen Computer gehostet wird.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Wenn die gewünschte Größe nicht aufgeführt ist, führen Sie den folgenden Befehl zum Ändern der Größe des virtuellen Computer an.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    Während dieses Vorgangs startet der virtuellen Computer neu. Nach dem Neustart werden Ihre vorhandene OS und Datenträger Daten zugeordnet werden. Nichts temporären geht verloren.

    Verwenden der `--enable-boot-diagnostics` Option ermöglicht [Boot Diagnose][boot-diagnostics], die Anmeldung bei Fehlern, die im Zusammenhang mit Start.

3. Wenn die gewünschte Größe nicht aufgeführt ist führen Sie die folgenden Befehle den virtuellen Computer freigeben, andernfalls, ihre Größe zu ändern, und starten Sie den virtuellen Computer neu.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Freigeben von den virtuellen Computer frei auch alle dynamischen IP-Adressen, die den virtuellen Computer zugewiesen ist. Der Datenträger OS und Daten sind hiervon nicht betroffen.
   
## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Skalierbarkeit mehrere Instanzen von virtuellen Computer ausführen und skalieren. Weitere Informationen finden Sie unter [Skalieren automatisch Linux-Computern in einer virtuellen Computern Skalierung festlegen][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md