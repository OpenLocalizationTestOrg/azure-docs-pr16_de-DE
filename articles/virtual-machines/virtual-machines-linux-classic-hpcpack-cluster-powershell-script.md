<properties
   pageTitle="PowerShell-Skript zum Bereitstellen von Linux HPC Cluster | Microsoft Azure"
   description="Führen Sie ein Powershellskript zum Bereitstellen eines Linux HPC Pack Clusters in Azure-virtuellen Computern"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Erstellen Sie einen Linux hohen Performance computing (HPC) Cluster mit HPC Pack IaaS Bereitstellungsskript

Führen Sie die Bereitstellung HPC Pack IaaS PowerShell-Skript, um eine vollständige HPC Cluster für Linux Auslastung in Azure-virtuellen Computern bereitstellen. Der Cluster besteht aus einer Active Directory-beigetreten am Knoten mit Windows Server und Microsoft HPC Pack und berechnen Knoten, die eine von der Linux-Versionen von HPC Pack unterstützt ausgeführt werden. Wenn Sie einen HPC Pack Cluster in Auslastung für Windows Azure bereitstellen möchten, finden Sie unter [Windows HPC Cluster mit HPC Pack IaaS Bereitstellungsskript erstellen](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Eine Ressourcenmanager Azure-Vorlage können Sie auch einen HPC Pack Cluster bereitstellen. Ein Beispiel finden Sie unter [Erstellen einer HPC Cluster mit Linux Knoten zu berechnen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Beispiel-Konfigurationsdatei

Die folgende Konfigurationsdatei erstellt eine neue Domänencontroller und Domänengesamtstruktur und einen HPC Pack Cluster 1 am mit lokalen Datenbanken und 10 Linux berechnen Knoten wiederkehrenden bereitstellt. Die Cloud-Dienste werden direkt in den Speicherort Ostasien erstellt. Die Datenverarbeitungsknoten Linux werden in 2-Cloud-Diensten und 2 Speicherkonten (d. h. _MyLnxCN-0001_ bis _MyLnxCN-0005_ in _MyLnxCNService01_ und _mylnxstorage01_) und _MyLnxCN-0006_ zu _MyLnxCN 0010_ in _MyLnxCNService02_ und _mylnxstorage02_erstellt. Die Datenverarbeitungsknoten werden von einem OpenLogic CentOS Version 7.0 Linux Bild erstellt. 

Ersetzen durch eigene Werte für den Namen Ihres Abonnements und das Konto und Service-Namen ein.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Behandlung von Problemen

* **Fehler "VNet nicht vorhanden"** – Wenn Sie das Bereitstellungsskript HPC Pack IaaS mehrere Cluster in Azure gleichzeitig unter ein Abonnement für die Bereitstellung ausführen, eine oder mehrere Bereitstellungen mit den Fehler fehlschlagen "VNet *VNet\_Namen* ist nicht vorhanden".
Wenn dieser Fehler auftritt, führen Sie das Skript für die Bereitstellung fehlgeschlagene erneut aus.

* **Problem beim Zugriff auf das Internet aus dem Azure virtuelle Netzwerk** – Wenn Sie einen HPC Pack Cluster mit einem neuen Domain Controller mithilfe des Bereitstellungsskripts, oder erstellen Sie manuell einen am Knoten virtueller Computer zu Domänencontroller heraufstufen, können Sie auch die virtuellen Computern in das Azure virtuelle Netzwerk mit dem Internet verbinden Probleme auftreten. Dies kann auftreten, wenn ein Weiterleitung DNS-Server wird automatisch auf die Domänencontroller konfiguriert haben und dieser Weiterleitung DNS-Server nicht ordnungsgemäß auflösen.

    Zum Umgehen dieses Problems, melden Sie sich bei der Domänencontroller und manuell entfernen der Einstellung für die Weiterleitung Konfiguration oder einen gültigen Weiterleitung DNS-Server zu konfigurieren. Klicken Sie hierzu im Server-Manager auf **Extras** >
    **DNS-Einträge** auf DNS-Manager zu öffnen, und doppelklicken Sie dann auf **Weiterleitung**.
    
## <a name="next-steps"></a>Nächste Schritte

* Finden Sie Informationen zu den unterstützten Linux Verteilung Verschieben von Daten und Aufträge zu einem Cluster HPC Pack mit Linux weiterleiten Knoten berechnen [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .
* Lernprogramme, die das Skript für einen Cluster erstellen und Ausführen einer Linux HPC Arbeitsbelastung verwenden, finden Sie unter:
    * [Linux Datenverarbeitungsknoten in Azure NAMD mit Microsoft HPC Pack ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Linux Datenverarbeitungsknoten in Azure OpenFOAM mit Microsoft HPC Pack ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Ausführen von Stern-CCM + mit Microsoft HPC Pack unter Linux Knoten in Azure berechnen](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
