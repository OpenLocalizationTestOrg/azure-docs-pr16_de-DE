<properties
 pageTitle="Hinzufügen von Burst Knoten zu einem Cluster HPC Pack | Microsoft Azure"
 description="Erfahren Sie, wie Sie einen HPC Pack Cluster in Azure bei Bedarf zu erweitern, indem er Worker Rolleninstanzen in einen Cloud-Dienst ausgeführt"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Hinzufügen von on Demand "Burst" Knoten zu einem Cluster HPC Pack in Azure



Wenn Sie eine [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) Cluster in Azure eingerichtet haben, sollten Sie eine Möglichkeit, um schnell die Cluster Kapazität nach oben oder unten skalieren, ohne Verwalten einer Gruppe von vorkonfigurierten berechnen Knoten virtuellen Computern. In diesem Artikel wird gezeigt, wie können Sie bei Bedarf "Burst" Knoten (Worker Rolleninstanzen in einen Cloud-Dienst ausgeführt) hinzufügen als berechnen von Ressourcen zu einem am Knoten in Azure. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Spitzen-Knoten][burst]

Die Schritte in diesem Artikel helfen Ihnen die Azure Knoten schnell einen cloudbasierten HPC Pack am Knoten virtueller Computer für eine Test oder Prüfung des Konzepts Bereitstellung hinzufügen. Die allgemeinen Schritte sind identisch mit den Schritten, um "in Azure Spitzen" hinzufügen Cloud Kapazität zu einem lokalen HPC Pack Cluster zu berechnen. Ein Lernprogramm finden Sie unter [Einrichten einer Hybrid Cluster mit Microsoft HPC Pack zu berechnen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Ausführliche Leitfäden und Überlegungen für die Herstellung Bereitstellungen finden Sie unter [in Azure mit Microsoft HPC Pack getrennt](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack am Knoten in einer Azure-virtuellen Computer bereitgestellt** – Sie können einen eigenständigen am Knoten virtueller Computer oder eine, das Bestandteil einer größeren Cluster ist. Zum Erstellen eines eigenständigen am Knotens finden Sie unter [Bereitstellen einer HPC Pack Kopf Knoten eine Azure-virtuellen Computer](virtual-machines-windows-hpcpack-cluster-headnode.md). Automatisierte HPC Pack Cluster Bereitstellungsoptionen finden Sie unter [Optionen zum Erstellen und Verwalten eines Windows HPC Clusters in Azure mit Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Wenn Sie das [HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) verwenden, um den Cluster in Azure zu erstellen, können Sie in der automatisierten Bereitstellung Azure Burst Knoten einbeziehen. Finden Sie in den Beispielen in diesem Artikel.

* **Azure-Abonnement** - zum Hinzufügen von Azure Knoten, Sie können das gleiche Abonnement verwendet, um den am Knoten virtueller Computer, bereitstellen oder ein anderes Abonnement (oder Abonnements) auswählen.

* **Kerne Kontingent** - möglicherweise müssen Sie das Kontingent der Kerne, erhöhen insbesondere dann, wenn Sie festlegen, dass mehrere Azure Knoten mit Multikernprozessoren Maßen bereitstellen. Um ein Kontingent, [Öffnen Sie eine Supportanfrage online Kunden](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) kostenlos zu erhöhen.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Schritt 1: Erstellen einer Cloud-Dienst und ein Speicherkonto für die Azure Knoten

Verwenden Sie den Azure klassischen Portal oder gleichwertiger Tools so konfigurieren Sie die folgenden Ressourcen, die für die Bereitstellung Ihrer Azure Knoten erforderlich sind:

* Einen neuen Azure-Cloud-Dienst
* Ein neues Konto mit Azure-Speicher

>[AZURE.NOTE] Nicht wiederverwenden eines vorhandenen Cloud-Diensts in Ihrem Abonnement. 

**Aspekte**

* Konfigurieren eines separaten Cloud-Diensts für jede Azure Knoten Vorlage, die Sie erstellen möchten. Allerdings können Sie das gleichen Speicherkonto für mehrere Knoten Vorlagen.

* Es empfiehlt sich, dass Sie in der gleichen Azure Region Cloud-Dienst und für die Bereitstellung der Speicher-Konto suchen.




## <a name="step-2-configure-an-azure-management-certificate"></a>Schritt 2: Konfigurieren von ein Zertifikat Azure management

Wenn Azure Knoten als berechnen Ressourcen hinzufügen möchten, benötigen Sie ein Zertifikat Management auf am Knoten und Upload ein entsprechendes Serverzertifikat für das Azure-Abonnement für die Bereitstellung verwendet.

In diesem Szenario können Sie das **Standardformat HPC Azure Management Zertifikat** auswählen, das HPC Pack installiert und konfiguriert automatisch auf dem am Knoten. Dieses Zertifikat ist für das Testen Zwecke und Prüfung des Konzepts Bereitstellungen hilfreich. Wenn dieses Zertifikat verwenden möchten, laden Sie Datei c:\Programme\Microsoft c:\Programme\Microsoft HPC Pack 2012\Bin\hpccert.cer aus dem am Knoten virtueller Computer mit dem Abonnement hoch. Wenn das Zertifikat im [Azure klassischen Portal](https://manage.windowsazure.com)hochladen möchten, klicken Sie auf **Einstellungen** > **Management Zertifikate**.

Weitere Optionen, um das Zertifikat Management konfigurieren finden Sie unter [Szenarien das Azure Management Zertifikat für Azure Spitzen-Bereitstellungen konfigurieren](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Schritt 3: Bereitstellen von Azure Knoten zum cluster



Die Schritte zum Hinzufügen und starten Sie Azure-Knoten in diesem Szenario werden in der Regel identisch mit den Schritten mit einer lokalen am Knoten. Weitere Informationen finden Sie in den folgenden Abschnitten in [Schritte zum Bereitstellen von Microsoft Azure-Knoten mit Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx)aus:

* Erstellen einer Vorlage für Azure Knoten

* Hinzufügen von Azure Knoten zu Windows HPC-cluster

* Starten Sie (bereitstellen) der Azure-Knoten

Nachdem Sie hinzufügen und starten Sie die Knoten, sind sie bereit für Ihre Verwendung Cluster Auftrag ausführen.

Wenn Sie beim Bereitstellen von Azure Knoten Probleme auftreten, finden Sie unter [Behandeln von Problemen mit Bereitstellungen von Azure Knoten mit Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Nächste Schritte

* Um eine der rechenintensiven Instanzgröße für die Burst-Knoten verwenden zu können, finden Sie unter der Aspekte in [zu H-Serie und berechnen ankommt A-Serie virtuellen Computern](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Wenn Sie automatisch vergrößert oder verkleinert wird Azure computing-Ressourcen entsprechend der Cluster Arbeitsbelastung möchten, finden Sie unter [Automatisches vergrößern und Verkleinern von Azure berechnen von Ressourcen in einem Cluster HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
