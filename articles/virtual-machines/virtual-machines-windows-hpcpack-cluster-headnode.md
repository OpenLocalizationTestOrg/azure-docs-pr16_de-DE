<properties
 pageTitle="Erstellen einen HPC Pack am Knoten eine Azure-virtuellen Computer | Microsoft Azure"
 description="Informationen Sie zum Azure-Portal und dem Modell zur Bereitstellung von Ressourcenmanager verwenden, um einen am Microsoft HPC Pack-Knoten in einer Azure-virtuellen Computer erstellen."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Erstellen Sie den am Knoten einer HPC Pack Cluster in einer Azure virtueller Computer mit einem Bild Marketplace


Verwenden eines [Microsoft HPC Pack virtuellen Computern Bild](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) aus dem Azure Marketplace und Azure-Portal um zu den am Knoten der HPC-Cluster zu erstellen. Diese Abbildung HPC Pack virtueller Computer basiert auf Windows Server 2012 R2 Datacenter mit HPC Pack 2012 R2 Update 3 vorinstalliert ist. Verwenden Sie diesen am Knoten für eine Nachweis des Konzepts Bereitstellung von HPC Pack in Azure. Sie können Sie berechnen Knoten mit dem Cluster auszuführenden HPC Auslastung hinzufügen.



>[AZURE.TIP]Um eine vollständige HPC Pack Cluster in Azure bereitstellen, die am und berechnen Knoten enthält, wird empfohlen, dass Sie eine automatisierte Methode verwenden. Optionen gehören das [Bereitstellungsskript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) und die [HPC Pack Cluster für Windows Auslastung](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) Ressourcenmanager Vorlage. Weitere Vorlagen finden Sie unter [HPC Pack Cluster Optionen in Azure](virtual-machines-windows-hpcpack-cluster-options.md) . 


## <a name="planning-considerations"></a>Hinweise zur Planung

Wie in der folgenden Abbildung gezeigt wird, stellen Sie den am HPC Pack-Knoten in der Active Directory-Domäne in einem Azure-virtuellen Netzwerk bereit.

![HPC Pack am Knoten][headnode]

* **Active Directory-Domäne** : das HPC Pack am Knoten zu einer Active Directory-Domäne in Azure Vorbereitung HPC Dienste des virtuellen Computers verknüpft werden muss. Wie in diesem Artikel, um die Bereitstellung des Konzepts, eine Berechtigung zum dargestellt können Sie den virtuellen Computer heraufstufen, die Sie für den am Knoten als Domänencontroller erstellen vor dem Starten der HPC-Dienste. Eine weitere Möglichkeit besteht darin, eine separate Domänencontroller und Gesamtstruktur in Azure, der den am Knoten virtueller Computer teilnehmen an.

* **Azure virtuelles Netzwerk** – Wenn Sie das Modell zur Bereitstellung von Ressourcenmanager mithilfe den am Knoten bereitstellen, angeben oder erstellen ein Azure-virtuelles Netzwerk. Sie verwenden das virtuelle Netzwerk aus, wenn Sie den am Knoten zu einer vorhandenen Active Directory-Domäne beitreten müssen. Sie benötigen sie später berechnen Knoten virtuellen Computern zum Cluster hinzuzufügen.

    
## <a name="steps-to-create-the-head-node"></a>Schritte zum Erstellen des am Knotens

Im folgenden werden die allgemeinen Schritte zum Azure-Portal zu verwenden, um eine Azure-virtuellen Computer für den HPC Pack am Knoten mit dem Modell zur Bereitstellung von Ressourcenmanager erstellen. 


1. Wenn Sie eine neue Active Directory-Struktur in Azure mit eigenen Domänencontroller virtuelle Computer erstellen möchten, ist eine Möglichkeit zum Verwenden einer [Vorlage Ressourcenmanager](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Für eine einfache Nachweis des Konzepts Bereitstellung ist es fein überspringen Sie diesen Schritt und den am Knoten virtueller Computer selbst als Domänencontroller konfigurieren. Diese Option wird später in diesem Artikel beschrieben.
    
2. Klicken Sie auf die [HPC Pack 2012 R2 auf Windows Server 2012 R2 Seite](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) in der Azure Marketplace klicken Sie auf **virtuellen Computern erstellen**. 

3. Im Portal auf der Seite **HPC Pack 2012 R2 unter Windows Server 2012 R2** wählen Sie das Modell zur Bereitstellung von **Ressourcenmanager** aus, und klicken Sie dann auf **Erstellen**.

    ![HPC Pack image][marketplace]

4. Verwenden des Portals konfigurieren Sie die Einstellungen, und erstellen den virtuellen Computer an. Wenn Sie neu bei Azure sind, führen Sie das Lernprogramm [virtuellen Computer Windows Azure-Portal erstellen](virtual-machines-windows-hero-tutorial.md). Für eine Nachweis des Konzepts Bereitstellung können in der Regel übernehmen Sie den Standardwert oder empfohlene Einstellungen.

    >[AZURE.NOTE]Wenn Sie den am Knoten zu einer vorhandenen Active Directory-Domäne in Azure teilnehmen möchten, stellen Sie sicher, dass Sie das virtuelle Netzwerk für diese Domäne angeben, wenn Sie den virtuellen Computer zu erstellen.
       
4. Nach dem Erstellen des virtuellen Computer und der virtuellen Computer ausgeführt wird, [eine Verbindung mit dem virtuellen Computer herstellen](virtual-machines-windows-connect-logon.md) , indem Sie Remotedesktop. 

5. Teilnehmen an den virtuellen Computer zu einer vorhandenen Domänengesamtstruktur oder erstellen Sie eine Domänengesamtstruktur des virtuellen Computers selbst zu.

    * Wenn Sie den virtuellen Computer in ein Azure-virtuellen Netzwerk mit einer vorhandenen Domäne erstellt, teilnehmen an den virtuellen Computer in der Gesamtstruktur mithilfe der standard-Server-Manager oder Windows PowerShell-Tools. Klicken Sie dann neu starten.

    * Wenn Sie den virtuellen Computer in ein neues virtuelle Netzwerk (ohne eine vorhandene Domänengesamtstruktur) erstellt haben, können hochstufen Sie den virtuellen Computer als Domänencontroller. Verwenden Sie standard Schritte zum Installieren und konfigurieren die Rolle aus Active Directory-Domänendiensten auf dem am Knoten. Die detaillierten Schritte finden Sie unter [Installieren Sie eine neue Windows Server 2012 Active Directory-Gesamtstruktur](https://technet.microsoft.com/library/jj574166.aspx).

5. Nach der virtuellen Computer ausgeführt wird und zum Active Directory-Struktur verknüpft ist, beginnen Sie die HPC Pack Dienste wie folgt:

    ein. Verbinden Sie mit dem am Knoten virtueller Computer mit einer Domänenkonto, ein Mitglied der Gruppe Administratoren ist. Verwenden Sie beispielsweise das Administratorkonto, die eingerichtet werden, wenn Sie den am Knoten virtueller Computer erstellt haben.

    b. Starten Sie für eine standardmäßige am Knotenkonfiguration Windows PowerShell als Administrator aus, und geben Sie Folgendes:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Sie können mehrere Minuten für die HPC Pack-Dienste zu starten.

    Geben Sie zusätzliche am Knoten Konfiguration, um Optionen zum `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Nächste Schritte

* Sie können jetzt mit der am Knotens im Cluster HPC Pack arbeiten. Beispielsweise starten Sie HPC Cluster Manager zu, und führen Sie die [Aufgabenliste der Bereitstellung](https://technet.microsoft.com/library/jj884141.aspx).
* Wenn Sie vergrößern möchten berechnen Cluster Kapazität bei Bedarf, fügen Sie [Azure Burst-Knoten](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) in einen Cloud-Dienst hinzu. 

* Versuchen Sie es auf dem Cluster eine Arbeitsbelastung Test ausgeführt. Ein Beispiel finden Sie unter HPC Pack- [Leitfaden für erste Schritte](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
