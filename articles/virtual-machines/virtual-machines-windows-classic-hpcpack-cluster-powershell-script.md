<properties
   pageTitle="PowerShell-Skript zum Bereitstellen von Windows HPC Cluster | Microsoft Azure"
   description="Führen Sie ein Powershellskript zum Bereitstellen von eines Windows HPC Pack Clusters in Azure-virtuellen Computern"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Erstellen Sie ein Windows cluster High Performance computing (HPC) mit HPC Pack IaaS Bereitstellungsskript

Führen Sie die Bereitstellung HPC Pack IaaS PowerShell-Skript, um eine vollständige HPC Cluster für Windows-Auslastung in Azure-virtuellen Computern bereitstellen. Cluster besteht aus einem Active Directory-teilnehmen am Knoten mit Windows Server und Microsoft HPC Pack und zusätzliche Fenster berechnen Ressourcen, die Sie angeben. Wenn Sie einen HPC Pack Cluster in Azure für Linux Auslastung bereitstellen möchten, finden Sie unter [Linux HPC Cluster mit HPC Pack IaaS Bereitstellungsskript erstellen](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Eine Ressourcenmanager Azure-Vorlage können Sie auch einen HPC Pack Cluster bereitstellen. Beispiele finden Sie unter [Erstellen einer HPC Cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) und [Erstellen einer HPC Cluster mit einem benutzerdefinierten Symbolen Knoten zu berechnen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Konfiguration von Beispieldateien

Ersetzen Sie in den folgenden Beispielen Namen oder eigene Werte für Ihr Abonnement-Id und das Konto und Service-Namen ein.

### <a name="example-1"></a>Beispiel 1

Die folgende Konfigurationsdatei bereitstellt, einen HPC Pack Cluster, der ein am Knoten mit lokalen Datenbanken hat und fünf Knoten des Betriebssystems Windows Server 2012 R2 zu berechnen. Die Cloud-Dienste werden direkt in den Westen US-Speicherort erstellt. Der am Knoten fungiert als Domain Controller, der die Domänengesamtstruktur.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Beispiel 2

Die folgende Konfigurationsdatei bereitstellt, einen HPC Pack Cluster in einer vorhandenen Domänengesamtstruktur. Der Cluster verfügt über 1 am Knoten mit lokalen Datenbanken und 12 berechnen Knoten mit der BGInfo VM Erweiterung angewendet.
Automatische Installation von Windows-Updates ist für alle virtuellen Computern in der Domänengesamtstruktur deaktiviert. Die Cloud-Dienste werden direkt in den Speicherort Ostasien erstellt. Die Datenverarbeitungsknoten werden in drei Clouddiensten und drei Speicherkonten erstellt: _MyHPCCN-0001_ bis _MyHPCCN-0005_ in _MyHPCCNService01_ und _mycnstorage01_; _MyHPCCN-0006_ zu _MyHPCCN0010_ in _MyHPCCNService02_ und _mycnstorage02_; und _MyHPCCN-0011_ in _MyHPCCN-0012_ in _MyHPCCNService03_ und _mycnstorage03_). Die Datenverarbeitungsknoten werden aus einer vorhandenen privaten Bild von einem Knoten berechnen erfasst erstellt. Das automatische vergrößern und verkleinern Dienst aktiviert ist, mit den Standardansichten vergrößern und verkleinern Intervallen.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Beispiel 3

Die folgende Konfigurationsdatei bereitstellt, einen HPC Pack Cluster in einer vorhandenen Domänengesamtstruktur. Cluster enthält eine am Knoten, einen Datenbankserver mit einem 500 GB Daten Datenträger, 2 Bank Knoten das Betriebssystem Windows Server 2012 R2 ausführen und fünf berechnen Knoten das Betriebssystem Windows Server 2012 R2 ausführen. Cloud-Dienst MyHPCCNService wird in der Gruppe Zugehörigkeit *MyIBAffinityGroup*erstellt, und die anderen Clouddienste in der Gruppe Zugehörigkeit *MyAffinityGroup*erstellt werden. Die HPC Job Scheduler REST-API und HPC Web-Portal auf den am Knoten aktiviert sind.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Beispiel 4

Die folgende Konfigurationsdatei bereitstellt, einen HPC Pack Cluster in einer vorhandenen Domänengesamtstruktur. Der Cluster verfügt über zwei am Knoten mit lokalen Datenbanken, zwei Azure Knoten Vorlagen werden erstellt und drei Größe Medium Azure-Knoten für Azure Knoten Vorlage _AzureTemplate1_erstellt werden. Eine Skriptdatei wird auf dem am Knoten ausgeführt, nach der am Knoten konfiguriert ist.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Behandlung von Problemen


* **Fehler "VNet nicht vorhanden"** -, wenn Sie das Skript zum Bereitstellen von mehrere Cluster in Azure gleichzeitig unter ein Abonnement ausführen, eine oder mehrere Bereitstellungen mit den Fehler fehlschlagen "VNet *VNet\_Namen* ist nicht vorhanden".
Wenn dieser Fehler auftritt, führen Sie das Skript für die Bereitstellung fehlgeschlagene erneut aus.

* **Problem beim Zugriff auf das Internet aus dem Azure virtuelle Netzwerk** – Wenn Sie manuell einen am Knoten virtueller Computer zu Domänencontroller heraufstufen oder Sie einen Cluster mit einem neuen Domain Controller erstellen mithilfe des Bereitstellungsskripts, Sie können die virtuelle Computer mit dem Internet verbinden Probleme auftreten. Dieses Problem kann auftreten, wenn ein Weiterleitung DNS-Server wird automatisch auf die Domänencontroller konfiguriert haben und dieser Weiterleitung DNS-Server nicht ordnungsgemäß auflösen.

    Zum Umgehen dieses Problems, melden Sie sich bei der Domänencontroller und manuell entfernen der Einstellung für die Weiterleitung Konfiguration oder einen gültigen Weiterleitung DNS-Server zu konfigurieren. Um diese Einstellung zu konfigurieren, klicken Sie im Server-Manager auf **Extras** >
    **DNS-Einträge** auf DNS-Manager zu öffnen, und doppelklicken Sie dann auf **Weiterleitung**.

* **Zugreifen auf RDMA Netzwerk aus rechenintensiven virtuellen Computern Problem** – Wenn Sie Windows Server berechnen hinzufügen oder broker-Knoten virtuellen Computern mit einer RDMA-fähigen Größe wie A8 oder A9, Sie können diese virtuelle Computer mit dem RDMA Anwendung-Netzwerk verbinden Probleme auftreten. Ein Grund, dass dieses Problem auftritt ist, wenn die Erweiterung HpcVmDrivers nicht ordnungsgemäß installiert ist, wenn der virtuelle Computer mit dem Cluster hinzugefügt werden. Beispielsweise möglicherweise die Erweiterung in der Installation von Bundesstaat hängen werden.

    Um dieses Problem zu umgehen, überprüfen Sie zunächst den Status der Erweiterung in den virtuellen Computern aus. Wenn die Erweiterung nicht ordnungsgemäß installiert ist, versuchen Sie die Knoten aus dem HPC Cluster zu entfernen, und fügen Sie den Knoten erneut hinzu. Beispielsweise können Sie berechnen Knoten virtuellen Computern hinzufügen, indem Sie das Skript hinzufügen-HpcIaaSNode.ps1 auf dem am Knoten ausführen.
    
## <a name="next-steps"></a>Nächste Schritte

* Versuchen Sie es auf dem Cluster eine Arbeitsbelastung Test ausgeführt. Ein Beispiel finden Sie unter HPC Pack- [Leitfaden für erste Schritte](https://technet.microsoft.com/library/jj884144).

* Ein Skript für die Clusterbereitstellung und Ausführen einer HPC Arbeitsbelastung finden Sie unter [Erste Schritte mit einer HPC Pack Cluster in Azure Excel und SOA Auslastung ausführen](virtual-machines-windows-excel-cluster-hpcpack.md).

* Versuchen Sie es HPC-Sprachpaket-Tools, um zu starten, zu beenden, hinzufügen und Entfernen von Datenverarbeitungsknoten aus einem Cluster, die, den Sie erstellen. Finden Sie unter [Verwalten Knoten in einem Cluster HPC Pack in Azure zu berechnen](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Wenn Sie festlegen, dass Aufträge zum Cluster von einem lokalen Computer zu senden, finden Sie unter [übermitteln HPC Aufträge von einem lokalen Computer zu einem Cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
