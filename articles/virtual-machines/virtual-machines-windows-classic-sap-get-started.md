<properties
   pageTitle="Verwenden von SAP auf Windows-virtuellen Computern | Microsoft Azure"
   description="Informationen zum Verwenden von SAP auf Windows-virtuellen Computern (virtuellen Computern) in Microsoft Azure löschen"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Verwenden von SAP auf Windows-virtuellen Computern in Azure

Cloud Computing eines häufig verwendeten Ausdrucks ist, das mehr Wichtigkeit innerhalb der IT-Branche von kleinen Unternehmen bis zu groß und internationalen Unternehmen hervorruft. Microsoft Azure ist die Cloud Services-Plattform von Microsoft seiner ein breites Spektrum der neuen mögliche Werte. Jetzt können Kunden schnell bereitstellen und die Bereitstellung von Applications heben als Cloud-Dienste, damit sie nicht auf technische oder Budgetierung Einschränkungen beschränkt sind. Statt eine Zeit und Ihr Budget in Hardware-Infrastruktur, können Unternehmen über die Anwendung, von Geschäftsprozessen und deren Vorteile für Kunden und Benutzer konzentrieren.

Mit Microsoft Azure-virtuellen Computern bietet Microsoft eine umfassende Infrastruktur als Plattform Service (IaaS). SAP NetWeaver-basierten Anwendungen werden auf Azure virtuellen Computern (IaaS) unterstützt werden. Die folgenden Whitepapers beschreiben, wie Planung und Implementierung von Applications SAP NetWeaver basierend auf Windows-virtuellen Computern in Azure. Sie können auch Applikationen SAP NetWeaver basierend auf [Linux virtuellen Computern](virtual-machines-linux-classic-sap-get-started.md)implementieren.

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver auf Azure - HA

Titel: SAP NetWeaver auf Azure - Cluster SAP ASCS/SCS-Instanzen mithilfe von Windows Server Failovercluster auf Azure mit SIOS DataKeeper

Zusammenfassung: ' dieses Dokument beschreibt, wie SIOS DataKeeper verwenden, um eine hoch verfügbare SAP-ASCS/SCS-Konfiguration auf Azure einzurichten. SAP Loss ihre zentrale Fehler Komponenten wie SAP-ASCS/SCS oder Warteschlange Replikations-Diensten mit Windows Server Failoverclusterkonfigurationen, die freigegebene Datenträger erfordern. Diese SAP-Komponenten sind unverzichtbar für die Funktionalität der SAP-System. Daher muss hoher Verfügbarkeit Funktionen setzen werden an Ort, um sicherzustellen, dass diese Komponenten ein Fehlers eines Servers oder eines virtuellen Computers als erledigt mit Windows-Cluster Konfigurationen für softwareunabhängige und Hyper-V-Umgebungen bewältigen können. Ab August 2015 kann nicht für sich selbst Azure freigegebene Datenträger, die für Windows erforderlich wäre hoch verfügbaren Konfigurationen für diese kritischen SAP-Komponenten erforderlichen basierend auf bereitstellen. Mithilfe des Produkts DataKeeper durch SIOS, können jedoch Windows Server Failoverclusterkonfigurationen Bedarf für SAP-ASCS/SCS auf der Azure IaaS-Plattform erstellt werden. Dieses Dokument beschreibt das Installieren einer Windows Server-Failoverclusterkonfiguration freigegebenen Datenträger von SIOS Datakeeper in Azure bereitgestellt und systematisch zu Schritt aus. Das Papier wird in Konfigurationen Details Azure, Windows und SAP auf dem der hohen Verfügbarkeit-Konfiguration auf optimale Weise arbeiten, erläutert. Das Papier ergänzen die SAP-Installationsdokumentation und SAP-Notizen, die die primären Ressourcen für Installationen und -Bereitstellungen mit SAP-Software auf bestimmten Plattformen darstellen.

Aktualisierte: August 2015

[Dieses Handbuch jetzt herunterladen](http://go.microsoft.com/fwlink/?LinkId=613056)
