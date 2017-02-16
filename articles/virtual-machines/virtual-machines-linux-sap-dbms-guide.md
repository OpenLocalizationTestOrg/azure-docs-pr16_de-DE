<properties
   pageTitle="SAP NetWeaver auf Linux virtuellen Computern (virtuellen Computern) – Bereitstellungshandbuch DBMS | Microsoft Azure"
   description="SAP NetWeaver auf Linux virtuellen Computern (virtuellen Computern) – Bereitstellungshandbuch DBMS"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>SAP NetWeaver auf Azure-virtuellen Computern (virtuellen Computern) – Bereitstellungshandbuch DBMS

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[Dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver auf Linux virtuellen Computern (virtuellen Computern) – Bereitstellungshandbuch DBMS) [Dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Zwischenspeichern für virtuellen Computern und virtuelle Festplatten) [Dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID) [Dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure-Speicher) [Dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Struktur einer Bereitstellung RDBMS) [Dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (hohen Verfügbarkeit und Wiederherstellung mit Azure-virtuellen Computern) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85AF-73666a6f7268 (SQL Server 2012 SP1 CU4 und höher) [Dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 und frühere Versionen) [Dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (mit einer SQL Server-Bilder aus Microsoft Azure Marketplace) [Dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (allgemeiner SQL Server für SAP auf Azure Zusammenfassung) [Dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Einzelheiten zu SQL Server-RDBMS) [Dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Speicherkonfiguration) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Sichern und Wiederherstellen) [Dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Sicherung für sichern und Wiederherstellen der Leistung) [Dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (andere) [Dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[Deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver auf Linux virtuellen Computern (virtuelle Computer) – Bereitstellungshandbuch) [Deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP-Ressourcen) [Deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Szenario 1: Bereitstellen eines virtuellen Computers aus Azure Marketplace für SAP) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Szenario 2: Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild für SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 () Szenario 3: Verschieben eines virtuellen Computers aus lokalen Verwenden einer nicht GRG Azure-virtuellen mit SAP) [Deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Bereitstellung Szenarien von virtuellen Computern für SAP auf Microsoft Azure) [Deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Bereitstellen von Azure PowerShell-Cmdlets) [Deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (herunterladen und importieren SAP relevanten PowerShell-Cmdlets) [Deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (teilnehmen virtueller Computer in der lokalen Domäne - nur Windows) [Deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [Bereitstellung Leitfaden 4,4]: Virtual-Machines-Linux-SAP-Deployment-Guide.MD#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (herunterladen, installieren und Aktivieren von Azure-virtuellen Computer-Agent) [Deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [Deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [Deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfigurieren Azure erweiterte Überwachung Erweiterung für SAP) [Deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness überprüfen Azure erweiterte Überwachung für SAP) [Deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (integritätsprüfung für Azure Überwachung Infrastruktur Konfiguration) [Deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (weiteren Azure Überwachung Infrastruktur für SAP Problembehandlung)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[Planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver auf Linux virtuellen Computern (virtuellen Computern) – Planung und Implementierung Führungslinie) [Planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Ressourcen) [Planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High Availability (HA) und Disaster Wiederherstellung (DR) für SAP NetWeaver auf Azure virtuellen Computern ausgeführt) [Planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (hohe Verfügbarkeit für SAP-Anwendungsserver) [Planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (mithilfe von Autostart für SAP-Instanzen) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (nur Cloud - virtuellen Computern Bereitstellungen in Azure ohne Abhängigkeiten Netzwerk Kunden lokal) [Planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross lokale - Bereitstellung von einzelne oder mehrere SAP-virtuellen Computern in Azure mit dem lokalen Netzwerk vollständig integriert wird der Anforderung) [Planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure Regionen) [Planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fehlerstrukturanalyse Domänen) [Planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Domänen Upgrade) [Planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure Verfügbarkeit Sets) [Planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (das Microsoft Azure virtuellen Computern Konzept) [Planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Speicher) [Planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Verschieben eines virtuellen Computers aus lokalen auf Azure mit einem Datenträger nicht GRG) [Planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Bereitstellen eines virtuellen Computers mit einem bestimmten Kunden-Image) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Vorbereitung zum Verschieben eines virtuellen Computers aus lokalen in Azure einem Datenträger nicht GRG) [ Planning-Guide-5.2.2]:Virtual-Machines-Linux-SAP-Planning-Guide.MD#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Vorbereitung für die Bereitstellung eines virtuellen Computers mit einem bestimmten Kunden-Image für SAP) [Planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Vorbereiten virtueller Computer mit SAP für Azure) [Planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Unterschied zwischen ein Azure Datenträger und Azure Abbildung) [Planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (eine virtuelle Festplatte aus lokalen in Azure hochladen) [Planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Kopieren Datenträger zwischen Azure-Speicherkonten) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (virtueller Computer/virtuelle Festplatte Struktur für SAP-Bereitstellungen) [Planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Einstellung Automatische Bereitstellung für angefügten Datenträger) [Planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (virtueller einzelne Computer mit SAP NetWeaver Demo-Schulung-Szenario) [Planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Bereitstellung von SAP-Instanzen von Cloud-Only Konzepte) [Planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Überwachung Lösung für SAP) [Planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Speicher)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

Dieses Handbuch ist Bestandteil der Dokumentation auf implementieren und Bereitstellen von SAP-Software auf Microsoft Azure. Bevor Sie dieses Handbuch gelesen haben, lesen Sie [Planung und Implementierung Leitfaden] [Planungshandbuch]. Dieses Dokument beschreibt die Bereitstellung von verschiedenen relationalen Datenbank Management Systeme (RDBMS) und verwandte Produkte in Kombination mit SAP auf Microsoft Azure virtuellen Computern (virtuellen Computern) unter Verwendung der Azure-Infrastruktur als Funktionen eines Service (IaaS).

Das Papier ergänzen die SAP-Installationsdokumentation und SAP-Notizen, die die primären Ressourcen für Installationen und -Bereitstellungen mit SAP-Software auf bestimmten Plattformen darstellen

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Allgemeine Informationen
In diesem Kapitel verbundenen Aspekte des Ausführens SAP DBMS-Systeme Azure-virtuellen Computern eingeführt werden. Es gibt einige Verweise auf bestimmte DBMS-Systeme in diesem Kapitel. Stattdessen werden nach diesem Kapitel bestimmte DBMS-Systeme in diesem Artikel behandelt.

### <a name="definitions-upfront"></a>Definitionen vorab
Im gesamten Dokument verwenden wir die folgenden Begriffe:

* IaaS: Infrastructure as a Service.
* PaaS: Plattform als Dienst.
* SaaS: Software als Dienst.
* SAP-Komponente: eine einzelne SAP-Anwendung wie ECC, BW, Lösung Manager oder EP.  SAP-Komponenten können auf herkömmliche ABAP oder Java-Technologien oder eine Anwendung nicht NetWeaver-basierten wie Business Objects basieren.
* SAP-Umgebung: eine oder mehrere SAP-Komponenten gruppiert logisch, wie etwa Entwicklung, QAS, Schulung, DR oder Herstellung eine Geschäftsfunktion auszuführen.
* SAP-Querformat: Dies bezieht sich auf die gesamten SAP-Anlagen in eines Kunden IT Querformat. Die SAP-Querformat umfasst alle Herstellung und nicht Herstellung Umgebungen.
* SAP-System: Die Kombination von DBMS Layer und Anwendung Layer, z. B. einen SAP-ERP-Entwicklung-System, die Überprüfung des SAP BW, SAP-CRM Herstellung System usw.. In Azure-Bereitstellungen ist es nicht unterstützt, um diese beiden Ebenen zwischen lokalen und Azure dividieren. Dies bedeutet, die entweder ein SAP-System ist bereitgestellt lokalen, oder sie die in Azure bereitgestellt wird. Sie können jedoch die unterschiedlichen Systemen von einer SAP-Querformat in Azure oder lokalen bereitstellen. Beispielsweise konnte Sie die SAP-CRM Entwicklung bereitstellen und Systeme in Azure jedoch die SAP-CRM Herstellung System lokal testen.
* Nur Cloud-Bereitstellung: eine Bereitstellung, in dem das Azure-Abonnement nicht über eine Website-zu-Standort oder ExpressRoute Verbindung Infrastruktur des lokalen verbunden ist. Gemeinsam sind Azure Dokumentation folgenden Arten von Bereitstellungen auch als 'Nur Cloud'-Bereitstellungen beschrieben. Virtuellen Computern befinden, die bei dieser Methode können über die Internet- und öffentlichen Internet Endpunkte zugewiesen, die mit den virtuellen Computern in Azure zugegriffen werden. Der lokalen Active Directory (AD) und DNS-Einträge in Azure in diesen Abfragearten Bereitstellungen nicht erweitert ist. Die virtuellen Computern sind daher nicht Teil der lokalen Active Directory. Hinweis: Nur Cloud-Bereitstellungen in diesem Dokument werden als abgeschlossen SAP-Landschaften definiert, die ausschließlich in Azure ohne Erweiterung von Active Directory oder mit einer namensauflösung von in öffentlichen Cloud aus lokalen ausgeführt werden. Nur Cloud-Konfigurationen werden nicht unterstützt, für die genutzten SAP- oder Konfigurationen, in denen SAP-STMS oder andere Ressourcen lokal zwischen SAP-Systemen auf Azure und Ressourcen lokal gehosteten verwendet werden müssen.
* Cross lokale: Beschreibt ein Szenario, in dem virtuellen Computern ein Azure-Abonnement bereitgestellt werden, die zwischen Standorten, mit mehreren Standorten oder ExpressRoute Konnektivität zwischen dem lokalen Datacenter(s) und Azure aufweist. Gemeinsam Azure-Dokumentation die folgenden Arten von Bereitstellungen werden auch als Cross lokale Szenarien beschrieben. Der Grund für die Verbindung besteht darin, lokale Domänen zu erweitern, lokalen Active Directory und lokale DNS-Einträge in Azure. Die lokale Querformat erweiterten den Azure Anlagen des Abonnements. Probleme dieser Erweiterung an, können die virtuellen Computern Teil der lokalen Domäne sein. Der lokalen Domäne Domänen-Benutzer können auf die Server zugreifen und Dienste auf diese virtuellen Computern (wie DBMS Services) ausgeführt werden können. Kommunikation und Namen Auflösung zwischen virtuellen Computern bereitgestellt lokalen, und virtuellen Computern in Azure bereitgestellt ist möglich. Wir erwarten, dass diese Option, um die am häufigsten verwendeten Szenario für die Bereitstellung von SAP-Anlagen auf Azure werden. Finden Sie unter [dieser] [ vpn-gateway-cross-premises-options] Artikel und [dieser] [ vpn-gateway-site-to-site-create] Weitere Informationen.

> [AZURE.NOTE] Cross lokale Bereitstellungen von SAP-Systeme, wo Azure virtuellen Computern ausgeführt werden SAP-Systemen Mitglieder einer lokalen Domäne sind, werden für die Herstellung SAP-Betriebssystemen unterstützt. Cross lokale Konfigurationen werden unterstützt, für die Bereitstellung von Webparts oder SAP-Landschaften in Azure abzuschließen. Auch die vollständige SAP-Querformat in Azure ausgeführt erforderlich ist, müssen diese virtuellen Computern, Teil der lokalen Domäne und anzeigen. In früheren Versionen der Dokumentation erörtert Hybrid-IT-Szenarien, wo der Begriff 'Hybrid' in der Faktentabelle Stamm ist, dass ein Kreuz lokale Konnektivität zwischen lokalen und Azure vorhanden ist. In diesem Fall also 'Hybrid' auch die virtuellen Computern in Azure lokalen Active Directory gehören.

Einige Microsoft-Dokumentation beschreibt Cross lokale Szenarien etwas anders, vor allem für DBMS HA Konfigurationen. Dokumente, die Cross lokale Szenario nur kocht nach unten, bis ein (ExpressRoute) Konnektivität zwischen Standorten oder private Probleme und darauf, dass die SAP-Querformat zwischen lokalen und Azure verteilt werden im Zusammenhang mit bei die SAP.

### <a name="resources"></a>Ressourcen
Die folgenden Handbücher sind für das Thema der SAP-Bereitstellungen auf Azure verfügbar:

* [SAP NetWeaver auf Azure-virtuellen Computern (virtuellen Computern) – Planung und Implementierung Führungslinie] [Planungshandbuch]
* [SAP NetWeaver auf Azure-virtuellen Computern (virtuellen Computern) – Bereitstellungshandbuch] [Bereitstellungshandbuch]
* [SAP NetWeaver auf Azure-virtuellen Computern (virtuelle Computer) – Bereitstellungshandbuch für DBMS (dieses Dokument)] [Dbms-Leitfadens]

Die folgenden SAP-Hinweise beziehen sich auf das Thema der SAP auf Azure:

| Anzahl der Notiz   | Titel
|------------|--------
| [1928533] | SAP-Anwendungen auf Azure: unterstützte Produkte und Azure-virtuellen Computer Dateitypen
| [2015553] | Klicken Sie auf Microsoft Azure SAP: Vorkenntnisse unterstützen
| [1999351] | Problembehandlung bei erweiterten Azure Überwachung für SAP
| [2178632] | Kennzahlen für SAP auf Microsoft Azure Überwachung Schlüssel
| [1409604] | Virtualisierung unter Windows: Erweiterte Überwachung
| [2191498] | SAP auf Linux mit Azure: Erweiterte Überwachung
| [2039619] | SAP-Anwendungen auf Microsoft Azure mithilfe der Oracle-Datenbank: unterstützte Produkte und Versionen
| [2233094] | DB6: SAP-Anwendungen auf Azure mithilfe von IBM DB2 für Linux, UNIX und Windows – zusätzliche Informationen
| [2243692] | Klicken Sie auf Microsoft Azure (IaaS) virtueller Computer Linux: SAP-Lizenz Probleme
| [1984787] | SUSE LINUX Enterprise Server 12: Installation von Notizen
| [2002167] | Red Hat Enterprise Linux 7.x: Installation und Upgrade

Sie können auch nachlesen [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) enthält, die alle SAP-Notizen für Linux.

Sie sollten Kenntnisse über die Microsoft Azure-Architektur und wie Microsoft Azure-virtuellen Computern bereitgestellt und betrieben werden müssen. Weitere Informationen finden Sie hier <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Wir Microsoft Azure-Plattform werden **nicht** als eine Service (PaaS)-Angebote der Microsoft Azure-Plattform besprochen. Dieses Dokument ist zum Ausführen einer Datenbank Managementsystem (DBMS) in Microsoft Azure virtuellen Computern (IaaS) so, wie Sie das DBMS in Ihrer lokalen Umgebung ausführen möchten. Datenbankfunktionen Funktionalitäten zwischen diesen zwei angeboten unterscheiden sich deutlich und dürfen nicht von miteinander gemischt werden. Siehe auch: <https://azure.microsoft.com/services/sql-database/>

Da wir IaaS besprochen werden, werden im Allgemeinen unter Windows, Linux und DBMS Installation und Konfiguration im Wesentlichen identisch mit einer beliebigen virtuellen Computern oder leeren Metall Computer würden Sie lokal installieren. Es gibt jedoch einige Architektur und System Management Implementierung Entscheidungen die verschiedenen gestellt werden, wenn IaaS verwendet. Der Zweck dieses Dokuments ist erläutert, die speziell Architektur und System Management Unterschiede, denen Sie bei Verwendung von IaaS für vorbereitet werden müssen.

Im Allgemeinen sind die allgemeinen Bereiche Differenzen, die in diesem Artikel erläutert wird:

* Planen der korrekte virtueller Computer/virtuelle Festplatte Entwurf SAP-Systeme, um sicherzustellen, dass Sie die entsprechenden Daten haben Datei Layout und erzielen Sie genügend IOPS für Ihre Arbeitsbelastung.
* Netzwerk-Aspekte beim IaaS verwenden.
* Bestimmte Datenbankfunktionen zu verwenden, um das Datenbanklayout zu optimieren.
* Sichern und Wiederherstellen von Aspekte in IaaS.
* Verwenden von verschiedenen Arten von Bildern für die Bereitstellung.
* Hohe Verfügbarkeit in Azure IaaS.

## <a name="a-name65fa79d6-a85f-47ee-890b-22e794f51a64astructure-of-a-rdbms-deployment"></a><a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktur einer RDBMS Bereitstellung
Geben Sie in der Reihenfolge nach diesem Kapitel, ist es erforderlich, zu verstehen, was in [dieses] [Bereitstellung Leitfaden 3] Kapitel [Bereitstellungshandbuch] [Bereitstellungshandbuch] dargestellt wurde. Kenntnisse in Bezug auf die anderen virtuellen Computer-Reihe und deren Unterschiede und Differenzen Azure Standard- und Premium-Speicher sollte bekannt, bevor Sie dieses Kapitel und verstanden werden.

Bis März 2015 wurden Azure-virtuellen Festplatten die enthalten ein Betriebssystem auf 127 GB Größe beschränkt. Diese Einschränkung haben im März 2015 (für Weitere Informationen Kontrollkästchen <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ) aufgehoben. Dort auf virtuellen Festplatten kann, enthält das Betriebssystem, das die gleiche Größe wie andere virtuelle Festplatte aufweisen. Wir bevorzugen dennoch weiterhin eine Struktur der Bereitstellung, wo das Betriebssystem, DBMS und tatsächlichen SAP-Binärdateien von Datenbankdateien getrennt werden. Daher erwarten wir, dass SAP-Systeme in Azure virtuellen Computern ausgeführt wird, die Basis virtuellen Computers (oder virtuelle Festplatte) mit dem Betriebssystem installiert, Datenbank Management ausführbare Systemdateien und SAP-Programmdateien hat. Die DBMS Daten- und Protokolldateien Dateien werden in Azure-Speicher (Standard oder Premium Speicher) in separaten virtuellen Festplatte Dateien gespeichert und als logische Datenträger, die das Betriebssystem Azure Originalbild virtuellen Computer angefügt. 

Je nach Nutzung der Azure Standard oder Premium Speicher (z. B. mithilfe der DS-Serie oder GS-Serie virtuellen Computern) es werden andere Kontingente in Azure die beschrieben werden [hier][virtual-machines-sizes]. Beim Planen Ihrer Azure-virtuellen Festplatten, müssen Sie die beste Abstimmung der Kontingente für Folgendes zu finden:

* Die Anzahl der Datendateien.
* Die Anzahl der virtuellen Festplatten, die die Dateien enthalten.
* Die von einer einzelnen virtuellen IOPS von Kontingenten.
* Der Datendurchsatz pro virtuelle Festplatte.
* Die Anzahl der zusätzlichen virtuellen Festplatten möglich pro virtueller Speicher.
* Der Gesamtdurchsatz Speicher kann ein virtuellen Computers bereitstellen.
 
Azure wird ein Kontingent IOPS pro virtuelle Festplatte Laufwerk erzwingen. Diese Kontingente sind für virtuelle Festplatten gehostet wird, klicken Sie auf Standard Azure und Premium Speicher unterschiedlich. E/a-Wartezeiten werden auch zwischen den beiden Speichertypen mit Premium Speicher Vorführung Faktoren bessere e/a-Wartezeiten sehr unterschiedlich sein. Die verschiedenen Arten von virtuellen Computer verfügen über eine eingeschränkte Anzahl von virtuelle Festplatten, die Sie anfügen können. Eine andere Einschränkung ist, dass nur bestimmte virtueller Computer-Typen Azure Premium Storage nutzen können. Dies bedeutet, dass es sich bei die Entscheidung für einen bestimmten Typ von virtuellen Computer nicht nur durch die CPU- und Anforderungen, sondern auch durch die IOPS Wartezeit und Datenträger Durchsatz Anforderungen eingeleitet werden möglicherweise, die mit der Anzahl von virtuellen Festplatten oder den Typ des Premium-Datenträger in der Regel skaliert werden. Insbesondere mit Premium-Speicher möglicherweise auch die Größe einer virtuellen vorgegeben wird, um die Anzahl von IOPS und Durchsatz, die durch die einzelnen virtuelle Festplatte erreicht werden muss.

Die Fakultät der globalen IOPS Prozentsatzes die Anzahl der virtuellen Festplatten bereitgestellt, und die Größe der virtuellen Computer alle miteinander verbunden sind, möglicherweise eine Azure Konfiguration von ein SAP-System als seine lokalen Bereitstellung unterschiedlich sein. Die Grenzwerte IOPS pro LUN werden können, die in lokale Bereitstellungen normalerweise konfiguriert. Während die mit Azure-Speicher Höchstgrenzen feste oder wie Premium Speicher je nach Typ der Festplatte sind. So finden Sie unter Lokale Bereitstellungen wir Kundenkonfigurationen der Datenbankserver die vielen verschiedene Datenmengen für spezielle ausführbare Dateien wie SAP und die DBMS spezielle Datenmengen für temporäre Datenbanken oder Tabelle Leerzeichen verwenden. Wenn solche einem lokalen System in Azure verschoben wird möglicherweise es dazu führen, dass mögliche IOPS Bandbreite verschwendet durch eine virtuelle Festplatte für ausführbare Dateien oder Datenbanken, die keine oder nicht viele IOPS durchführen verschwenden. Daher sollten in Azure-virtuellen Computern die DBMS und SAP-Programmdateien auf dem Datenträger OS installiert werden, falls möglich.

Die Platzierung von Datenbankdateien und Protokolldateien und den Typ der Azure-Speicher verwendet, sollten durch IOPS, Wartezeit und Durchsatz Anforderungen definiert werden. Damit genügend IOPS für das Transaktionsprotokoll haben, können Sie notwendig werden, nutzen Sie mehrerer virtueller Festplatten für die Transaktionsprotokolldatei oder verwenden Sie einen größeren Premium Speicher Datenträger. In diesem Fall eine einfach erstellten würde eine Software RAID (z. B. Windows-Speicher Ressourcenpool für Windows oder MDADM und LVM (logischer Datenträger) für Linux) mit den virtuellen Festplatten das Protokoll enthalten wird.

___

> ![Windows][Logo_Windows] Windows
>
> Laufwerk D:\ eine Azure-virtuellen Computer ist nicht beibehalten der von einigen lokalen Festplatten, klicken Sie auf den Berechnungsknoten Azure unterstützt wird. Da es nicht beibehalten wird, bedeutet dies, dass an den Inhalt der D:\ Laufwerk vorgenommenen Änderungen geht verloren, wenn Sie der virtuellen Computer neu gestartet wird. Mit "Alle Änderungen" Meine wir gespeicherten Dateien, Verzeichnisse erstellt, Applikationen installiert.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure-virtuellen Computern bereitstellen automatisch eines Laufwerks am /mnt/resource also eine nicht permanente Laufwerk gesicherten von lokalen Festplatten auf dem Berechnungsknoten Azure. Da es nicht beibehalten wird, bedeutet dies, dass auf Inhalte an /mnt/resource vorgenommenen Änderungen geht verloren, wenn Sie der virtuellen Computer neu gestartet wird. Mit vornehmen Meine wir gespeicherten Dateien, Verzeichnisse erstellt, Applikationen installiert.

___

Abhängig von der Azure-virtuellen Computer-Serie, die lokalen Festplatten in den Berechnungsknoten anzeigen unterschiedliche Performance der wie kategorisiert werden kann:

* A0-A7: Sehr eingeschränkter Leistung. Nicht verwendbar für alles jenseits Datei der Windows-Seite
* A8-A11: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz
* D-Serie: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz
* DS-Serie: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz
* G-Serie: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz
* GS-Serie: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz

Oben genannten Aussagen sind auf die Typen virtueller Computer anwenden, die mit SAP zertifiziert sind. Die virtuellen Computer-Reihe mit hervorragende IOPS und Durchsatz qualifizieren für die Nutzung, indem Sie einige Features DBMS wie Tempdb oder temporäre Tabelle Leerzeichen.

### <a name="a-namec7abf1f0-c927-4a7c-9c1d-c7b5b3b7212facaching-for-vms-and-vhds"></a><a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Cache für virtuellen Computern und virtuelle Festplatten
Wenn wir diese Datenträger/virtuellen Festplatten über das Portal oder erstellen Wenn wir hochgeladene virtuellen Festplatten auf virtuellen Computern bereitstellen, können wir auswählen, ob der Datenverkehr zwischen den virtuellen Computer und diese virtuelle Festplatten befindet sich im Azure-Speicher zwischengespeichert werden. Azure Standard- und Premium-Speicher verwenden zwei verschiedene Technologien für diesen Cache. In beiden Fällen können den Cache selbst Datenträger auf denselben Laufwerken von dem temporären Datenträger (D:\ unter Windows) oder unter Linux /mnt/resource den virtuellen Computer verwendeten gesichert werden sollte.
 
Für den standardmäßigen Azure-Speicher sind die möglichen Cache:

* Kein Zwischenspeichern
* Lese-Cache
* Lesen und Schreiben von Zwischenspeichern

Um konsistente und deterministisch Leistung zu erzielen, sollten Sie festlegen das Zwischenspeichern auf Standard Azure-Speicher für alle virtuellen Festplatten mit **DBMS Zusammenhang Datendateien, Protokolldateien und Abstand: Tabelle ''**. Das Zwischenspeichern von den virtuellen Computer kann mit der standardmäßigen bleiben.

Für Azure Premium Speicher vorhanden sind die folgenden Optionen zum Zwischenspeichern aus:

* Kein Zwischenspeichern
* Lese-Cache

Empfehlungen für Azure Premium Speicher zur Nutzung der SAP-Datenbank **Zwischenspeichern für Datendateien zu lesen** ist und **kein Zwischenspeichern für die VHD(s) der Log Datei(en)**ignoriert.

### <a name="a-namec8e566f9-21b7-4457-9f7f-126036971a91asoftware-raid"></a><a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Software-RAID
Wie bereits oben erwähnt, müssen Sie die Anzahl der für die Datenbankdateien über die Anzahl der virtuellen Festplatten, die Sie konfigurieren können, benötigt IOPS Saldo, und geben Sie die maximale IOPS pro virtuelle Festplatte oder Premium Speicher Datenträger eine Azure-virtuellen Computer bereitgestellt wird. Die IOPS Laden über virtuelle Festplatten behandelt einfachsten eine Software-RAID über die verschiedenen virtuellen Festplatten zu erstellen. Platzieren Sie dann eine Anzahl von Datendateien des SAP-DBMS der LUNs aus der Software RAID darzustellen. Hängt von den Anforderungen, sollten Sie erwägen Sie die Verwendung von Premium Speicher sowie seit zwei von drei verschiedene Premium-Datenträger bieten höhere IOPS Kontingent als virtuelle Festplatten basierend auf Standard-Speicher. Neben wesentlichen bessere e/a-Wartezeit von Azure Premium Speicher bereitgestellt. 

Dasselbe gilt für das Transaktionsprotokoll der verschiedenen DBMS Systeme. Mit zahlreiche sie einfach nur weitere Nachverfolgungsprotokolldateien hinzufügen hilft nicht, da die DBMS-Systeme jeweils nur in eine Datei schreiben. Wenn höhere IOPS Sätzen als ein einzelnes Standard erforderlich sind, die Speicher basierend virtuelle Festplatte umgehend, Sie können über mehrere Standard-Speicher virtueller Festplatten versehen oder einen größeren Premium Speicher Datenträgertyp, der jenseits höhere IOPS Raten auch Faktoren unteren Wartezeit für das Schreiben e/a in der Transaktionslog bietet können müssen.
 
Situationen, in der mit einer Software bevorzugen würden Azure-Bereitstellungen erfahrener RAID sind:

* Transaktion Log/wiederholen Log erfordern mehr IOPS als Azure für eine einzelne virtuelle Festplatte bereitgestellt. Wie zuvor erwähnt dies durch die Erstellung einer LUNs über mehrere virtuelle Festplatten mithilfe einer Software-RAID gelöst werden kann.
* Ungleiche e/a-Arbeitsbelastung Verteilung über die verschiedenen Datendateien der SAP-Datenbank. In diesen Fällen kann eines einer Datendatei betätigen das Kontingent lieber häufig auftreten. Während die anderen Datendateien kein auch in der Nähe des Kontingents IOPS von einer einzelnen virtuellen wiedergegeben werden. Die einfachste Lösung werden in diesem Fall erstellen eine LUN über mehrere virtuelle Festplatten mithilfe einer Software-RAID. 
* Sie wissen nicht, was die genaue e/a-Arbeitsbelastung pro Datendatei ist und nur daran zu erinnern, was die generelle IOPS Arbeitsbelastung gegen das DBMS ist. Am einfachsten, führen Sie ist zum Erstellen einer LUN mithilfe einer Software-RAID. Die Summe der Kontingente für mehrere virtuelle Festplatten hinter dieser LUN sollte die bekannte IOPS Rate dann erfüllen.

___

> ![Windows][Logo_Windows] Windows
>
> Verwendung von Windows Server 2012 oder höhere Speicher Leerzeichen empfiehlt sich, da es effizienter als Windows Striping mit älteren Versionen von Windows ist. Bitte beachten Sie müssen möglicherweise die Windows-Speicherpools und Speicher Leerzeichen von PowerShell-Befehlen zu erstellen, wenn Sie als Betriebssystem Windows Server 2012 zu verwenden. Die PowerShell-Befehlen <https://technet.microsoft.com/library/jj851254.aspx> finden Sie hier

> 
> ![Linux][Logo_Linux] Linux
>
> Nur MDADM und LVM (logische Volume Manager) werden zum Erstellen einer Software-RAID auf Linux unterstützt. Weitere Informationen finden Sie in den folgenden Artikeln:
>
> * [Konfigurieren von Software-RAID auf Linux] [ virtual-machines-linux-configure-raid] (für MDADM)
> * [Konfigurieren Sie eine Linux virtuellen Computers in Azure LVM][virtual-machines-linux-configure-lvm]


___

Überlegungen für die Nutzung von virtuellen Computer-Serie, die in der Regel arbeiten mit Azure Premium Speicher können sind:

* Erfordert für e/a-Wartezeiten, die dicht an was SAN/NAS-Geräte senden.
* Bei Bedarf für Faktoren bessere e/a-Wartezeit als Standard Azure-Speicher vorführen können.
* Geben Sie höhere IOPS pro virtueller Computer als was mit mehreren Standard-Speicher virtueller Festplatten anhand eines bestimmten virtuellen Computers erreicht werden konnte.

Da der zugrunde liegende Azure-Speicher jede virtuelle Festplatte mit mindestens drei Speicherknoten, einfachen RAID 0 Striping verwendet werden kann repliziert. Es gibt keine RAID5 oder RAID1 implementiert werden muss.

### <a name="a-name10b041ef-c177-498a-93ed-44b3441ab152amicrosoft-azure-storage"></a><a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure-Speicher
Microsoft Azure-Speicher werden Basis virtuellen Computers (mit BS) und virtuelle Festplatten oder BLOBs mit mindestens 3 separaten Speicherknoten gespeichert. Beim Erstellen eines Kontos Speicher ist eine der Auswahlmöglichkeiten des Schutzes, wie hier dargestellt:

![Geo-Replikation für Speicher Azure-Konto aktiviert][dbms-guide-figure-100]

Azure-Speicher lokale Replikation (lokal Redundant) bietet Schutz vor Datenverlust durch Infrastruktur Fehler, der einige Kunden leisten könnten bereitstellen. Wie oben gezeigt, dass es gibt 4 unterschiedliche Optionen eine fünfte wird eine Variation einer der ersten drei. Diese genauer anschauen, können wir unterscheiden:

* **Premium lokal redundante Speicher (LRS)**: Azure Premium Speicher bietet leistungsfähige und niedrig Wartezeiten Datenträger Unterstützung für virtuellen Computern ich/O-Auslastung Auslastung ausgeführt werden. Es gibt 3 Replikate der Daten innerhalb der gleichen Azure Datencenter eines Bereichs Azure ein. Die Exemplare kann ich in verschiedenen Fehlerstrukturanalyse und Domänen Upgrade (für Konzepte finden Sie unter [Dies] [planning-Leitfaden-3,2] Kapitel in [Guide][planning-guide]) planen. Bei einer Kopie der Daten, außer Betrieb aufgrund von Speicher Knoten Fehler oder Ausfall der Festplatte vertraut wird ein neues Replikat automatisch generiert.
* **Lokal redundante Speicher (LRS)**: In diesem Fall 3 Replikate der Daten innerhalb der gleichen Azure Datencenter eines Bereichs Azure vorhanden sind. Die Exemplare kann ich in verschiedenen Fehlerstrukturanalyse und Domänen Upgrade (für Konzepte finden Sie unter [Dies] [planning-Leitfaden-3,2] Kapitel in [Guide][planning-guide]) planen. Bei einer Kopie der Daten, außer Betrieb aufgrund von Speicher Knoten Fehler oder Ausfall der Festplatte vertraut wird ein neues Replikat automatisch generiert. 
* **Geo redundante Speicher (GRS)**: In diesem Fall eine asynchrone Replikation, die eine zusätzliche 3 Replikate der Daten in einem anderen Azure Region-Feeds wird in den meisten Fällen in der gleichen geografische Region (wie North Europa und Westen Europa) also vorhanden ist. 3 zusätzliche Replikations Dadurch werden, dass in Summe 6 Replikate vorhanden sind. Eine Variation Dies ist eine Ergänzung, in dem die Daten in der Geo repliziert Azure-Region zum Lesen zur (Lesezugriff Geo Redundant) verwendet werden.
* **Zone redundante Speicher (ZRS)**: In diesem Fall 3 Replikate Daten bleiben in der gleichen Azure Region. Wie [diese] erläutert kann Kapitel [planning-Leitfaden-3,1] [Planungshandbuch] [-Planungshandbuch] ein Azure Region eine Reihe von Rechenzentren in der Nähe sein. Im Falle LRS würde die Replikate über den verschiedenen Datencentern verteilt, die eine Azure Region vornehmen.

Weitere Informationen finden Sie [hier][storage-redundancy].
 
> [AZURE.NOTE] DBMS-Bereitstellungen wird die Verwendung von redundanten Speicher Geo nicht empfohlen
>
> Azure-Speicher Geo-Replikation ist asynchrone. Replikation der einzelnen virtuellen Festplatten zu eines einzelnen virtuellen Computers bereitgestellt werden Sperren Schritt nicht synchronisiert. Daher ist es nicht geeignet DBMS Dateien repliziert, die anderen virtuellen Festplatten angefallen sind, oder anhand einer Software-RAID Grundlage mehrerer virtueller Festplatten bereitgestellt. DBMS-Software erfordert, dass die beständigen Festplattenspeicher genauer über die unterschiedlichen LUNs und zugrunde liegenden Datenträger/virtuelle Festplatten/bestehen synchronisiert ist. Software DBMS verwendet verschiedenen Verfahren zum Sequenz EA schreiben Aktivitäten, und ein DBMS meldet, dass das Ziel der Replikations Festplattenspeicher beschädigt ist, wenn diese auch durch ein paar Millisekunden geändert werden. Eine wirklich eine Datenbankkonfiguration mit einer Datenbank über mehrere virtuelle Festplatten Geo repliziert gestreckt können, muss solche eine Replikation daher mit Datenbank bedeutet und Funktionalität ausgeführt werden. Eine sollten nicht auf dem Azure Speicher Geo-Replikation zum Ausführen dieser Aufgabe verlassen. 
>
> Das Problem ist am einfachsten, mit einem System Beispiel erläutert. Angenommen, Sie haben ein SAP-System in Azure 8 virtuellen Festplatten Datendateien des DBMS sowie eine virtuelle Festplatte, enthält die Transaktionsprotokolldatei mit wiederkehrenden hochgeladen werden. Jeder von 9 diese virtuelle Festplatten wird Daten in eine konsistente Methode entsprechend den DBMS geschrieben sein, ob die Daten in die Daten oder Transaktion Protokolldateien geschrieben werden.
>
> In die Daten zu ordnungsgemäß Geo Replikation anordnen und ein Datenbankbild einheitlich Verwalten der Inhalt der alle neun virtuelle Festplatten nutzen, müssen Sie genau in der Reihenfolge Geo repliziert werden die e/a-Vorgänge in der neun anderen virtuellen Festplatten ausgeführt wurden. Azure-Speicher Geo-Replikation lässt nicht, so deklarieren Sie Abhängigkeiten zwischen virtuellen Festplatten. Dies bedeutet, dass Microsoft Azure Storage Geo-Replikation über die Fakultät wissen nicht, dass der Inhalt in diese neun anderen virtuellen Festplatten miteinander verwandt sind, und die Änderungen Daten konsistent sind nur bei der Replikation in der Reihenfolge der e/a-Vorgänge über die 9 virtuellen Festplatten Vorkommnissen.
>
> Neben der Wahrscheinlichkeit Hoch, dass die Bilder im Szenario Geo repliziert kein Datenbankbild einheitlich bieten wird auch vorliegt Leistung beeinträchtigt, die mit Geo redundante Speicherung zeigt die Leistung stark beeinflussen, können. Verwenden Sie diese Art von Speicherredundanz nicht für DBMS Typ Auslastung, um in Zusammenfassung.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Zuordnen von virtuellen Festplatten in Azure-virtuellen Computern Speicher Dienstkonten
Ein Azure-Speicher-Konto ist nicht nur eine administrative erstellen, sondern auch einen Betreff der Einschränkungen. Während die Einschränkungen abweicht, ob Azure Standardkonto Speicher oder ein Azure Premium Speicherkonto besprochen. Der genaue Stärken und Schwächen aufgelisteten [hier][storage-scalability-targets]
 
Daher für standardmäßigen Azure-Speicher es wichtig ist zu beachten, es gibt eine Beschränkung auf die IOPS pro Speicher-Konto (Zeile Summe anfordern Zins [im]Artikel mit[storage-scalability-targets]). Darüber hinaus besteht eine anfängliche maximal 100 Speicherkonten pro Azure Abonnement (Stand Juli 2015). Daher wird empfohlen, IOPS von virtuellen Computern zwischen mehreren Speicherkonten bei Verwendung der standardmäßigen Azure-Speicher abzuwägen. Während ein einzelnes virtuellen Computers idealerweise eine Speicherkonto falls möglich verwendet. Also wenn wir über DBMS Bereitstellungen sprechen, in dem jede virtuelle Festplatte, die auf Standard Azure-Speicher gehostet wird Kontingent erreicht erreicht haben könnten, sollten Sie nur 30-40 virtuelle Festplatten pro Azure-Speicherkonto bereitstellen, die Standard Azure-Speicher verwendet. Andererseits, wenn Sie Azure Premium Speicher nutzen und die Datenbank große Datenmengen speichern möchten, können Sie im Hinblick auf IOPS fein möglicherweise. Aber ein Azure Premium Speicher-Konto ist Weise eingeschränkter Datenmengen als Azure Standardkonto Speicher. Daher können Sie nur eine eingeschränkte Anzahl von virtuellen Festplatten in einem Azure Premium Speicherkonto drücken Sie anschließend den Lautstärke Datengrenzwert bereitstellen. Bei der Reaktionszeiten Ende eines Kontos Azure-Speicher als "Virtuelle SAN" enthält, die Funktionen in IOPS und/oder Kapazität eingeschränkt. Der Vorgang bleibt daher wie lokale Bereitstellungen, um das Layout von der virtuellen Festplatten der verschiedenen SAP-Systeme über verschiedene 'Imaginärteil SAN Geräte' oder des Azure-Speicherkonten definieren.
 
Für den standardmäßigen Azure-Speichers empfiehlt es sich nicht Speicher aus verschiedenen Speicherkonten, falls möglich eines einzelnen virtuellen Computers darstellen.

Während der DS oder der Azure-virtuellen Computern GS-Serie verwenden, ist es möglich, Bereitstellen virtueller Festplatten aus Azure Standard-Speicher-Konten und Premium Speicherkonten. Verwenden von Fällen wie Sicherungskopien in Standard-Speicher schreiben gesicherten virtuellen Festplatten während DBMS Daten Probleme und Protokolldateien Premium Speichermenge mir fallen, wo solche Speicherprodukten genutzt werden konnte. 

Basierend auf Kunden-Bereitstellungen und Testen ungefähr 30 bis 40 virtuellen Festplatten mit Daten und Protokolldateien Datenbank auf einem einzelnen Azure Speicher Standardkonto mit zulässigen Leistung bereitgestellt werden können. Wie zuvor schon erwähnt, wird die Einschränkung eines Kontos Azure Premium Speicher wahrscheinlich einige über ausreichend Datenkapazität, die sie enthalten kann und nicht IOPS sein.

Als erfordert mit SAN Geräte lokal Freigabe einige Überwachung und um schließlich Engpässe eines Azure-Speicher-Kontos zu erkennen. Überwachung-Erweiterung für SAP- und Azure-Portal Azure sind Tools, die zum Auffinden von beschäftigt Azure-Speicher-Konten, die nicht optimalen EA Leistung vorführen werden möglicherweise verwendet werden können.  Wenn diese Situation, dass es wird empfohlen erkannt wird, beschäftigt virtuellen Computern an ein anderes Konto der Azure-Speicher zu verschieben. Lizenzinformationen finden Sie im [Bereitstellungshandbuch] [Bereitstellungshandbuch], ausführliche Informationen zu den Überwachungsfunktionen SAP-Host aktivieren.

Ein anderer Artikel Zusammenfassen von bewährte Verfahren für Standard Azure-Speicher und Azure Standard-Speicherkonten <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx> finden Sie hier
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Verschieben von virtuellen Computern DBMS aus Azure Standard-Speicher zu Azure Premium Storage bereitgestellt
Wir auftreten einiger Szenarien, die Sie als Kunden ein bereitgestellten virtuellen Computers von Standard Azure-Speicher in Azure Premium Speicher zu wechseln möchten. Dies ist ohne physisch Verschieben der Daten nicht möglich. Es gibt mehrere Methoden zum Erreichen des Ziels ein:

* Sie könnten einfach alle virtuellen Festplatten, Basis virtuelle Festplatte als auch Daten virtuelle Festplatten in ein neues Azure Premium Speicherkonto kopieren. Sehr oft haben Sie die Anzahl der virtuellen Festplatten in Standard Azure-Speicher nicht aus den Grund, dass Sie die Lautstärke Daten erforderlich ausgewählt. Aber Sie, dass viele virtuelle Festplatten aufgrund der IOPS erforderlich. Nun Wechsel zu Azure Premium Speicher konnte Sie weniger virtuelle Festplatten erzielen Sie den einige IOPS-Durchsatz mit Möglichkeit wechseln. In Standard Azure-Speicher Sie für die verwendeten Daten und nicht die Datenträgergröße der nominal Zahlen, die Anzahl der virtuellen Festplatten nicht im Hinblick auf Kosten unerheblich, der Fakultät zugewiesen. Mit Azure Premium Speicher, möchten Sie jedoch für die Größe des nominal bezahlen. Versuchen Sie deshalb Großteil den Kunden die Anzahl der Azure-virtuellen Festplatten im Speicher Premium bei der Anzahl den IOPS-Durchsatz erzielen erforderlich notwendigen belassen. Ja, entscheiden die meisten Kunden gegen die Möglichkeit einer einfachen 1:1 kopieren.
* Wenn Sie noch nicht bereitgestellt, bereitgestellt eine einzelne virtuellen, die eine Sicherungskopie der Datenbank der SAP-Datenbank enthalten kann. Nach der Sicherung Sie heben Sie die Bereitstellung aller virtuellen Festplatten, einschließlich der virtuellen Festplatte mit der Sicherung und kopieren Sie die Basis virtuelle Festplatte und die virtuelle Festplatte mit der Sicherung in ein Konto Azure Premium Storage. Klicken Sie dann den virtuellen Computer basierend auf der Basis virtuellen Festplatte bereitstellen und die virtuelle Festplatte mit der Sicherung bereitstellen. Jetzt erstellen Sie weitere leeren Premium-Datenträger, für den virtuellen Computer, mit denen die Datenbank in wiederherstellen. Dies wird davon ausgegangen, dass das DBMS Pfade als Teil des Wiederherstellungsvorgangs auf die Daten und Protokolldateien ändern kann.
* Eine weitere Möglichkeit ist eine Variation des ehemalige Prozesses, wo Sie einfach kopieren Sie die Sicherung virtuelle Festplatte in Azure Premium Storage und das anhand eines virtuellen Computers, das Sie neu bereitgestellt und installiert.
* Der vierte Möglichkeit, die Sie auswählen möchten, wenn Sie bedürftige so ändern Sie die Anzahl der Datendateien der Datenbank haben. In diesem Fall müssen Sie eine SAP-System einheitlichen Kopie mit den Import/Export durchführen. Setzen Sie die Dateien in einer virtuellen exportieren, die in einem Azure Premium Speicherkonto kopiert wird und Zuordnen eines virtuellen Computers, mit denen Sie den Importprozesse ausgeführt werden. Kunden verwenden diese Möglichkeit, vor allem, wenn sie die Anzahl der Datendateien verkleinern möchten.

### <a name="deployment-of-vms-for-sap-in-azure"></a>Bereitstellung von virtuellen Computern für SAP in Azure
Microsoft Azure bietet mehrere Methoden zum Bereitstellen von virtuellen Computern und die zugehörigen Festplatten. Somit ist es sehr wichtig, die Unterschiede kennen, da Vorbereitungen von der virtuellen Maschinen hängt von der Möglichkeit der Bereitstellung abweichen. Im Allgemeinen nähere wir die Szenarien in die folgenden Kapitel beschrieben.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Bereitstellen eines virtuellen Computers aus dem Azure Marketplace
Möchten Sie eine Microsoft oder 3rd Party bereitgestellten Bild aus dem Azure Marketplace Ihrer virtuellen Computer bereitstellen. Nachdem Sie Ihre virtuellen Computer in Azure bereitgestellt, führen Sie dieselben Richtlinien und Tools für die SAP-Software innerhalb der virtuellen Computer zu installieren, wie Sie in einer lokalen Umgebung tun würden. Für die Installation der Software SAP-innerhalb des Azure-virtuellen Computers, empfiehlt sich SAP und Microsoft zum Hochladen und Speichern der SAP-Installationsmedien Azure-virtuellen Festplatten oder zum Erstellen einer Azure-virtuellen Computer als 'Dateiserver' die enthält alle erforderlichen SAP-Installationsmedien arbeiten.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Bereitstellen eines virtuellen Computers mit einem bestimmten GRG-Image des Kunden
Aufgrund von bestimmter Patch Anforderungen hinsichtlich des Workflowversion OS oder DBMS möglicherweise bereitgestellten Bilder in der Azure Marketplace nicht auf Ihre Bedürfnisse anpassen. Daher müssen Sie zum Erstellen eines virtuellen Computers verwenden eigener 'Privat' OS/DBMS virtueller Computer Bilder die danach mehrmals bereitgestellt werden kann. Um Kopien solche ein 'Privat' Bild vorzubereiten, muss das Betriebssystem auf dem lokalen virtuellen Computer GRG werden. Lizenzinformationen finden Sie im [Bereitstellungshandbuch] [Bereitstellungshandbuch], Details zum generalize eines virtuellen Computers.

Wenn Sie bereits SAP-Inhalte in Ihrem lokalen virtuellen Computer (besonders für Systeme der Ebene 2) installiert haben, können Sie die SAP-Systemeinstellungen anpassen, nach die Bereitstellung von den Azure-virtuellen Computer über die Instanz umbenennen Verfahren von SAP-Software Provisioning Manager (SAP-Hinweis [1619720]) unterstützt werden. Andernfalls können Sie die SAP-Software später nach der Bereitstellung von den Azure-virtuellen Computer installieren.
 
Zum Zeitpunkt der Inhalt von verwendeten Datenbank SAP-Anwendung können Sie entweder Generieren des Inhalts frisch SAP-Installation oder können Sie Ihre Inhalte in Azure importieren, indem Sie eine virtuelle Festplatte mit einer Sicherungskopie der Datenbank DBMS oder Nutzung der Funktionen des DBMS direkt Sicherung in Microsoft Azure-Speicher. In diesem Fall könnten Sie vorbereiten auch mit der DBMS Daten- und Protokolldateien Dateien lokal virtuelle Festplatten und importieren diese als Datenträger in Azure. Aber die Übertragung von DBMS-Daten, die in Azure aus lokalen geladen werden in diesem Fall funktioniert über virtuelle Festplatte Datenträger, die lokal vorbereitet werden müssen.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Verschieben eines virtuellen Computers zu Azure aus lokalen einem Datenträger nicht GRG
Sie möchten ein bestimmtes SAP-System von lokalen Azure (heben Sie und die UMSCHALTTASTE) wechseln. Dies kann die virtuelle Festplatte die enthält das Betriebssystem, die SAP-Binärdateien und tatsächlichen DBMS Binärdateien sowie die virtuellen Festplatten mit den Daten, und melden Sie sich Dateien des DBMS in Azure hochladen erfolgen. In behalten entgegen Szenario #2 obigen Sie Hostname, SAP-SID und SAP-Benutzerkonten in den Azure-virtuellen Computer sie in der lokalen Umgebung konfiguriert wurden. Daher ist es nicht erforderlich, die Verallgemeinern des Bilds. Diesem Fall wendet hauptsächlich für Cross lokale Szenarien, wo ein Teil der SAP-Querformat lokalen und Webparts auf Azure ausgeführt wird.

## <a name="a-name871dfc27-e509-4222-9370-ab1de77021c3ahigh-availability-and-disaster-recovery-with-azure-vms"></a><a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Hohe Verfügbarkeit und Wiederherstellung mit Azure-virtuellen Computern
Azure bietet die folgenden Funktionalitäten High Availability (HA) und Disaster Wiederherstellung (DR) welche verschiedenen Komponenten anwenden, die wir für SAP- und DBMS Bereitstellungen verwenden würden

### <a name="vms-deployed-on-azure-nodes"></a>Virtueller Computer auf Azure-Knoten bereitgestellt
Die Azure-Plattform bietet keine Features wie Live Migration für bereitgestellte virtuelle Computer. Dies bedeutet ist es Wartung erforderlich auf einem Servercluster auf dem ein virtuellen Computers bereitgestellt wird, muss der virtuellen Computer beendet und neu gestartet. Wartung in Azure mit sogenannte Upgrade Domänen in Zuordnungseinheiten des Servers ausgeführt. Um jeweils nur eine Aktualisierung-Domäne wird aufrechterhalten. Während ein solcher Neustart ist vorhanden, eine Unterbrechung der Dienst während der virtuellen Computer fahren, die Wartung erfolgt und virtuellen Computer neu gestartet. Die meisten Anbieter von DBMS bieten jedoch hohe Verfügbarkeit und Wiederherstellung-Funktionen, die schnell neu die DBMS Dienste auf einem anderen Knoten gestartet wird, wenn der primäre Knoten nicht verfügbar ist. Die Azure-Plattform bietet Funktionen zum Verteilen von virtuellen Computern, Speicherplatz und andere Dienste Azure auf Domänen aktualisieren, um sicherzustellen, dass geplante Wartung oder Infrastruktur Fehlern nur eine kleine Gruppe von virtuellen Computern oder Diensten auswirken können.  Bei sorgfältiger Planung ist es möglich, Verfügbarkeit Ebenen vergleichbar mit lokalen Infrastruktur zu erzielen.

Microsoft Azure Verfügbarkeit Sätze sind eine logische Gruppierung von virtuellen Computern oder Dienste, die virtuellen Computern und andere Dienste: Damit ist sichergestellt werden verschiedene Fehlerstrukturanalyse und Aktualisieren von Domänen in einem Cluster verteilt, so, dass es würde nur einen Knoten war(en) zu einem beliebigen Zeitpunkt (Lesen [diese] [ virtual-machines-manage-availability] Artikel weitere Details).

Es muss nach Zweck konfiguriert sein, wenn paralleles sich virtuelle Computer wie hier dargestellt:

![Definition für DBMS HA Konfigurationen Verfügbarkeit festlegen][dbms-guide-figure-200]

Wenn wir hoch verfügbare Konfigurationen DBMS Bereitstellungen (unabhängig von den einzelnen DBMS HA Funktionen verwendet) erstellen möchten, durchzuführen die DBMS virtuellen Computern:

* Hinzufügen der virtuellen Computern mit dem gleichen Azure virtuelle Netzwerk (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* Die virtuellen Computern der Konfiguration HA sollten auch im selben Subnetz. Mit einer namensauflösung von zwischen den verschiedenen Subnets ist nicht möglich, in der Cloud nur Bereitstellungen, funktionieren nur IP-Auflösung. Mithilfe von Website-zu-Standort oder ExpressRoute Konnektivität für Cross lokale Bereitstellungen, wird ein Netzwerk mit mindestens ein Subnetz bereits eingerichtet werden. Mit einer namensauflösung von erfolgt entsprechend der lokalen AD-Richtlinien und Netzwerk-Infrastruktur. 
[Kommentar]: <>  (MSSedusch erledigen Test immer noch true in der Cloud)

#### <a name="ip-addresses"></a>IP-Adressen
Es wird dringend empfohlen, die virtuellen Computern für Hostadapterkonfigurationen robuste so einrichten. Verlassen auf IP-Adressen behoben werden die HA Partner innerhalb der HA-Konfiguration ist nicht in Azure zuverlässigen, es sei denn, statische IP-Adressen verwendet werden. Es gibt zwei "Beenden" Konzepte in Azure ein:

* Fahren Sie bis Azure-Portal oder Azure PowerShell-Cmdlet beenden-AzureRmVM: In diesem Fall des virtuellen Computers ruft war(en) und Aufheben der Reservierung. Ein Azure-Konto wird nicht mehr für diese virtuellen Computer belastet, damit die einzigen Gebühren anfallen, die für die Speicherung verwendet werden. Jedoch, wenn die private IP-Adresse der Schnittstelle nicht statischen war, die IP Address aufgehoben, und es ist nicht sichergestellt, dass die Schnittstelle der alten IP Address erneut nach einem Neustart des dem virtuellen Computer zugewiesen wird. Das Beenden nach unten durchführen, über das Azure-Portal oder durch Einwahl beenden-AzureRmVM wird die Freigabe zugewiesenen automatisch ausgeführt werden. Wenn Sie keine zu Deallocat verwenden der Computer beenden-AzureRmVM - StayProvisioned 
* Wenn Sie den virtuellen Computer aus einem OS Pegel beenden, erhält der virtuellen Computer fahren und nicht aufheben der Reservierung. Jedoch wird in diesem Fall ein Azure-Konto weiterhin für den virtuellen Computer trotz der Fakultät in Rechnung gestellt, dass war(en) ist. In diesem Fall bleiben die Zuweisung der IP-Adresse zu beendet eines virtuellen Computers erhalten. Beenden den virtuellen Computer aus in wird nicht automatisch Freigabe zugewiesenen erzwingen.

Auch für Cross lokale Szenarien werden standardmäßig eine war(en) und Freigabe zugewiesenen Duplikat Zuordnung IP-Adressen aus dem virtuellen Computer bedeutet, dass auch wenn lokale Richtlinien in DHCP-Einstellungen unterscheiden. 

* Die Ausnahme ist eine Netzwerk-Schnittstelle als beschrieben [hier]eine statische IP-Adresse zuweist[virtual-networks-reserved-private-ip].
* In diesem Fall bleibt die IP-Adresse feste, wie die Schnittstelle nicht gelöscht wird.

> [AZURE.IMPORTANT] Um die gesamte Bereitstellung einfache und überschaubare beibehalten möchten, empfiehlt es sich löschen zum Einrichten der virtuellen Computern Partnerschaften in einem DBMS HA oder DR-Konfiguration in Azure auf eine Weise, die zwischen den verschiedenen virtuellen Computern verbindet eine funktionsfähige namensauflösung vorliegt.
 
## <a name="deployment-of-host-monitoring"></a>Bereitstellung von Host für die Überwachung
Für produktiv Verwendung von SAP Anwendungen in Azure virtuellen Computern erfordert SAP die Möglichkeit zum Überwachen von Daten aus den Azure-virtuellen Computern ausgeführt physischen Hosts Host abrufen. Eine bestimmte Ebene von SAP-HostAgent Patch kann erforderlich sein, mit dem diese Funktion in SAPOSCOL und SAP-HostAgent. Die genauen Patchebene wird im SAP-Hinweis [1409604]beschrieben.

Die Einzelheiten bezüglich der Bereitstellung von Komponenten, die Hostdaten SAPOSCOL und SAPHostAgent vorführen und die Verwaltung des Lebenszyklus dieser Komponenten, finden Sie unter der [Bereitstellungshandbuch] [Bereitstellungshandbuch]

## <a name="a-name3264829e-075e-4d25-966e-a49dad878737aspecifics-to-microsoft-sql-server"></a><a name="3264829e-075e-4d25-966e-a49dad878737"></a>Einzelheiten zu Microsoft SQL Server

### <a name="sql-server-iaas"></a>SQL Server IaaS
Beginnend mit Microsoft Azure, können Sie einfach Ihre vorhandene SQL Server entwickelte auf Windows Server-Plattform zu Azure virtuellen Computern migrieren. SQL Server auf virtuellen Computers können Sie die Gesamtkosten der Bereitstellung, Verwaltung und Wartung der Enterprise Umfang Applications zu reduzieren, indem Sie einfach diesen Anwendungen in Microsoft Azure migrieren. Mit SQL Server in einer Azure-virtuellen Computern können Administratoren und Entwickler weiterhin verwenden, die gleichen Entwicklung und Verwaltungstools, die lokale verfügbar sind. 

> [AZURE.IMPORTANT] Bitte beachten Sie, dass wir also eine Plattform als ein Dienstangebot der Microsoft Azure-Plattform Microsoft Azure SQL-Datenbank nicht besprochen werden. Die Diskussion in diesem Dokument wird zur Ausführung von SQL Server-Produkt, für lokale Bereitstellungen in Azure virtuellen Computern, die Infrastruktur als eine Funktion Dienst von Azure Nutzung verzeichnet ist. Datenbankfunktionen Funktionalitäten zwischen diesen zwei angeboten unterscheiden und dürfen nicht von miteinander gemischt werden. Siehe auch: <https://azure.microsoft.com/services/sql-database/>
 
Es wird dringend empfohlen, um zu überprüfen, [diese] [ virtual-machines-sql-server-infrastructure-services] Dokumentation, bevor Sie fortfahren.

In den folgenden Abschnitten werden Textstellen Teile der Dokumentation unter den oben angegebenen Link zusammengefasster und angegeben werden. Besonderheiten in Bezug auf SAP sind ebenfalls angegeben, und einige Konzepte ausführlicher beschrieben. Es wird jedoch dringend empfohlen, für die Arbeit durch die Dokumentation über ersten vor dem Lesen der bestimmten SQL Server-Dokumentation.

Es gibt einige SQL Server in IaaS spezifische Informationen, die Sie, bevor Sie fortfahren kennen sollten:

* **Virtuellen Computern Vereinbarung zum SERVICELEVEL**: Es wird eine Vereinbarung zum SERVICELEVEL für virtuellen Computern ausgeführt in Azure die hier zu finden sind: <https://azure.microsoft.com/support/legal/sla/>  
* **SQL-Version unterstützt**: wir Unterstützung für SAP-Kunden, SQL Server 2008 R2 und höher auf Microsoft Azure virtuellen Computern. Frühere Versionen werden nicht unterstützt. Überprüfen Sie diese allgemeinen [Support-Anweisung](https://support.microsoft.com/kb/956893) Weitere Details an. Bitte beachten Sie, dass im Allgemeinen SQL Server 2008 auch von Microsoft unterstützt wird. Jedoch durch signifikante Funktionalität für SAP die eingeführt wurde mit SQL Server 2008 R2, SQL Server 2008 R2 die Mindestversion für SAP. Beachten Sie die SQL Server 2012 und 2014 mit tieferen Integration in dem Szenario IaaS (wie Sichern direkt gegen Azure-Speicher) erweitert haben. Daher beschränken wir in diesem Dokument auf SQL Server 2012 und 2014, mit deren aktuelle Patch-Level für Azure ein.
* **Unterstützung von Features SQL**: am häufigsten SQL Server-Features werden auf Microsoft Azure-virtuellen Maschinen unterstützt, mit einigen Ausnahmen. **SQL Server-Failoverclustering Verwenden von freigegebenen Festplatten wird nicht unterstützt**.  Verteilt Technologien wie Datenbank Spiegelung, AlwaysOn Verfügbarkeit Gruppen, Replikation, Log Liefer- und Dienst Bank in einem einzigen Azure Bereich unterstützt werden. SQL Server AlwaysOn wird ebenfalls zwischen verschiedenen Azure Regionen unterstützt, wie hier beschrieben: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Überprüfen Sie die [Support-Anweisung](https://support.microsoft.com/kb/956893) für weitere Details an. Ein Beispiel zum Bereitstellen einer Konfigurations AlwaysOn in [dieser] [ virtual-machines-workload-template-sql-alwayson] Artikel. Sehen Sie sich das bewährte Methoden dokumentierten [hier] auch,[virtual-machines-sql-server-infrastructure-services] 
* **SQL-Leistung**: Wir sind sicher, dass Microsoft Azure gehosteten virtuellen Computern sehr gut im Vergleich zu anderen öffentlichen Cloud Virtualisierung Angebote, aber einzelne Ergebnisse führt variieren können. Schauen Sie sich [diese] [ virtual-machines-sql-server-performance-best-practices] Artikel.
* **Verwenden von Bildern aus Azure Marketplace**: die schnellste Möglichkeit zum Bereitstellen eines neuen Microsoft Azure-virtuellen Computers besteht darin, verwenden ein Bild aus dem Azure Marketplace. Bilder in der Azure Marketplace, die SQL Server enthalten sind. Die Bilder, auf dem SQL Server bereits installiert ist, können sofort für Applikationen SAP NetWeaver verwendet werden. Der Grund ist, dass die Standardsortierreihenfolge von SQL Server installiert ist, in diese Bilder und nicht die Sortierung von SAP NetWeaver-Systemen erforderlich. Solche Bilder verwenden möchten, aktivieren Sie die Schritte beschrieben, die in Kapitel [mit einer SQL Server-Bilder aus Microsoft Azure Marketplace] [Dbms Leitfaden 5.6]. 
* Checken Sie [Details Preise](https://azure.microsoft.com/pricing/) für Weitere Informationen aus. Die [SQL Server 2012 Lizenzierung](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) und [SQL Server 2014 Lizenzierung Führungslinie](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) werden auch eine wichtige Ressource.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>SQL Server-Konfigurations-Richtlinien für SAP-bezogene SQL Server-Installationen in Azure-virtuellen Computern

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Empfehlungen auf virtuellen Computer/virtuelle Festplatte Struktur für SAP Zusammenhang SQL Server-Bereitstellungen
Gemäß der allgemeinen Beschreibung, SQL Server-Programmdateien ansässig oder in dem Systemlaufwerk des des virtuellen Computers Basis virtuelle Festplatte installiert werden soll (Laufwerk C:\).  In der Regel können die meisten der SQL Server-System-Datenbanken auf hoher Ebene von SAP NetWeaver Arbeitsbelastung verwendet werden. Daher können die Systemdatenbanken von SQL Server (Master, Msdb und Model) auf das Laufwerk C:\ sowie bleiben. Eine Ausnahme könnte Tempdb, weshalb im Fall von einigen SAP ERP und alle BW Auslastung, erfordern möglicherweise entweder höhere Daten oder e/a-Vorgänge Datenträgers die nicht in der ursprünglichen virtuellen passt. Für diese Systeme sollte die folgenden Schritte ausgeführt werden:

* Verschieben Sie die primäre Tempdb Datendatei(en) auf demselben logischen Laufwerk als die primäre Daten Datei(en) aus der SAP-Datenbank ein.
* Fügen Sie alle zusätzlichen Tempdb Datendateien jeder anderen logischen Laufwerke mit einer Datendatei der Datenbank SAP-Benutzer hinzu.
* Fügen Sie das logische Laufwerk, das der Benutzer-Datenbank-Protokolldatei enthält die Protokolldatei Tempdb hinzu.
* Klicken Sie auf die berechnen Knoten Tempdb Daten- und Protokolldateien Dateien **ausschließlich für Typen virtueller Computer verwenden, die lokale SSDs** möglicherweise auf das Laufwerk D:\ platziert werden. Es möglicherweise dennoch empfohlen werden mehrere Tempdb-Datendateien verwendet. Beachten Sie, dass Laufwerke D:\ unterscheiden basierend auf dem virtuellen Computer.
 
Diese Konfigurationen aktivieren Tempdb beanspruchen mehr Platz als das Systemlaufwerk Möglichkeit zu bieten, ist. Akzeptieren, um die Größe der richtigen Tempdb ermittelt haben, kann eine Tempdb Größen vorhandenen Betriebssystemen überprüfen die lokale ausgeführt. Darüber hinaus würde eine solche Konfiguration IOPS Zahlen anhand der Tempdb aktivieren, die mit dem Systemlaufwerk bereitgestellt werden kann. Erneut, können Systeme, die lokal ausgeführt werden für e/a-Arbeitsbelastung gegen Tempdb überwachen, sodass Sie abgeleitet werden die IOPS-Zahlen, die Sie erwarten auf Ihre Tempdb verwendet werden.

Eine virtueller Computer Konfiguration der SQL Server mit einer SAP-Datenbank ausgeführt wird und der Tempdb Daten und Tempdb Protokolldatei klicken Sie auf das Laufwerk D:\ platziert sind aussehen würde:
 
![Die Verweiskonfiguration von Azure Neuerung für SAP][dbms-guide-figure-300]

Achten Sie darauf, dass das Laufwerk D:\ unterschiedliche Größen richtet sich nach dem Typ virtueller Computer verfügt. Abhängig von der Größe Vorbedingung Tempdb möglicherweise Sie auf Paar Tempdb Daten und Protokolldateien durch die SAP-Daten und der Protokolldateien Datenbankdateien in Fällen zwangsweise, wo D:\ Laufwerk zu klein ist.

#### <a name="formatting-the-vhds"></a>Formatierung der virtuellen Festplatten
Für SQL Server blockieren der NTFS Größe für virtuelle Festplatten mit SQL Server-Daten und Protokolldateien sollten 64 KB sein. Es ist nicht erforderlich, um das Laufwerk D:\ formatieren aus. Dieses Laufwerk enthält vorformatierten.

Um sicherzustellen, dass das Wiederherstellen und Erstellen von Datenbanken nicht die Datendateien Initialisierung aktiviert ist, indem Sie ein fokussieren den Inhalt der Dateien, sollte eine sicherstellen, dass der Kontext des Benutzers, die, den SQL Server-Dienst, in ausgeführt wird, eine bestimmte Berechtigung verfügt. Normalerweise müssen Benutzer in der Gruppe Windows-Administrator folgende Berechtigungen. Wenn Sie der SQL Server-Dienst im Kontext Benutzers, der nicht von Windows - Administrator-Benutzer ausgeführt wird, müssen Sie diesem Benutzer die Berechtigung 'Volume-Wartungsaufgaben ausführen' zuweisen.  Sehen Sie die Details in diesem Microsoft Knowledge Base-Artikel: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Auswirkung der Datenbank Komprimierung
In Konfigurationen, e Bandbreite eine Beschränkung verwendet werden kann, kann jeder Measure die IOPS reduziert hilfreich die Arbeitsbelastung dehnen, das eine in einem Szenario IaaS wie Azure ausführen kann. Daher wird, wenn noch nicht geschehen ist, Anwenden von SQL Server-Seite Komprimierung dringend empfohlen von Microsoft und SAP-vor dem Hochladen einer vorhandenen SAP-Azure Datenbanken.
 
Datenbank Komprimierung ausführen vor dem Hochladen auf Azure empfohlen wird aus zwei Gründen angegeben:

* Die Menge der Daten hochgeladen werden ist geringer.
* Die Dauer der Ausführung Komprimierung ist kürzer, unter der Voraussetzung, dass eine sicheres Hardware mit mehr CPUs oder höhere e/a-Bandbreite oder weniger e/a-Wartezeit lokal verwenden kann.
* Kleinere Datenbank möglicherweise zu weniger Kosten für die Zuordnung auf dem Datenträger führen.

Datenbank Komprimierung funktioniert auch in einer Azure-virtuellen Computern wie lokal. Weitere Informationen zum Komprimieren von einem vorhandenen SAP-SQL Server Datenbank überprüfen Sie hier: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>SQLServer 2014 – speichern Datenbank Dateien direkt auf Azure Blog Speicherung
SQL Server 2014 öffnet die Möglichkeit zum Speichern von Datenbankdateien direkt auf Azure Blob-Speicher ohne den Wrapper für eine virtuelle Festplatte, um diese an. Besonders bei der Verwendung von standardmäßigen Azure-Speicher oder kleineren virtueller Computer Typen dadurch die Szenarien, wo Sie die Grenzwerte von durch eine begrenzte Anzahl von virtuellen Festplatten erzwungen würde IOPS umgehen können, die auf bestimmte kleiner virtueller Computer bereitgestellt werden können. Dies funktioniert für Benutzer-Datenbanken, jedoch nicht für Systemdatenbanken von SQL Server. Es funktioniert auch für Daten und Protokolldateien von SQL Server. Wenn Sie eine SAP-SQL Server-Datenbank bereitstellen möchten auf diese Weise statt 'Umbruch' es in virtuellen Festplatten, wenden Sie sich bitte beachten Sie Folgendes beachten:

* Das Speicher-Konto verwendet Anforderungen wie eine, mit dem Bereitstellung des virtuellen Computer SQL Server, in ausgeführt wird der gleichen Azure Region sein.
* Aspekte, die zuvor im Hinblick auf virtuellen Festplatten über verschiedene Azure-Speicherkonten verteilen aufgeführt anwenden für diese Methode der Bereitstellung sowie Bedeutet, dass die Anzahl der e/a-Vorgänge gegen die Grenzwerte für das Konto ein Azure-Speicher.
[Kommentar]: <>  (MSSedusch erledigen aber Hiermit wird verwendet Netzwerkbandbreite und nicht Speicher Bandbreite verfügbar, es nicht?)

Details zu dieser Art von Bereitstellung sind hier aufgeführt: <https://msdn.microsoft.com/library/dn385720.aspx>
 
SQL Server-Datendateien direkt auf Azure Premium Speicher speichern, müssen Sie eine minimale Patch-Version von SQL Server 2014 haben, wie hier aufgeführt: <https://support.microsoft.com/kb/3063054>. Speichern von SQL Server-Datendateien auf Standard Azure-Speicher funktioniert mit dem die veröffentlichte Version von SQL Server 2014. Sehr dieselben Patches enthalten jedoch eine andere Reihe von Updates, die die direkte Verwendung von Azure BLOB-Speicher für SQL Server-Datendateien und Sicherungen Zuverlässigkeit zu erhöhen. Wir empfehlen daher diese Patches im Allgemeinen verwenden.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 Puffer Ressourcenpool Erweiterung
SQL Server 2014 eingeführt, ein neues Feature, das Puffer Ressourcenpool Erweiterung bezeichnet wird. Dieses Feature erweitert die Puffer Ressourcenpool von SQL Server, die mit einem zweiten Ebene Cache gespeichert ist, die durch lokale SSDs eines Servers oder virtuellen Computer unterstützt wird. Auf diese Weise können eine größere Datenmenge für arbeiten 'im Speicher' beibehalten. Im Vergleich zu den Zugriff auf Standard Azure-Speicher ist der Zugriff auf die Erweiterung der Puffer Ressourcenpool das auf lokale SSDs von einer Azure-virtuellen Computer gespeichert ist viele Faktoren schneller.  Daher könnte Nutzung von dem lokalen Laufwerk D:\ virtueller Computer Typen, die verfügen hervorragende IOPS und Durchsatz, eine sehr sinnvolle Methode entlasten gegen Azure-Speicher IOPS und seine Verbesserung der Reaktionszeiten von Abfragen. Dies gilt besonders, wenn Premium Speicher nicht verwenden. Bei Premium-Speicher und die Verwendung des Caches Premium Azure Lesen auf dem Berechnungsknoten werden wie empfohlen für Datendateien, keine große Unterschiede erwartet. Grund ist, dass die lokalen Festplatten Computeknoten beide Caches (SQL Server Puffer Ressourcenpool Erweiterung und Premium Speicher Lese-Cache) verwenden.
Weitere Details zu diesen Funktionen, aktivieren Sie diese Dokumentation: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Sicherung/Wiederherstellung Aspekte für SQL Server
Wenn Sie SQL Server in Azure bereitstellen, die Ihre Sicherung Methode überprüft werden muss. Auch wenn das System kein System produktiv ist, muss die SAP-Datenbank von SQL Server gehosteten regelmäßig gesichert werden. Da Azure-Speicher drei Bilder gespeichert sind, ist eine Sicherung jetzt in Bezug auf einen Absturz Speicher compensating weniger wichtig. Der Priorität Grund für die Verwaltung eines richtigen Sicherung und Wiederherstellung Plans ist mehr, die Sie durch die Bereitstellung von Point-in-Time-Wiederherstellungsfunktionen logischen/manueller Fehler kompensieren können. Somit ist das Ziel zu entweder Sicherungskopien verwenden Sie zum Wiederherstellen der Datenbank an einem bestimmten Punkt in Uhrzeit oder die Sicherungskopien in Azure verwenden, um ein anderes System durch Kopieren die vorhandene Datenbank zu starten. Sie können beispielsweise Wiederherstellen einer Sicherung in einer 3-Ebenen-System-Setup des gleichen Systems aus einer 2-Ebenen SAP-Konfiguration übertragen.

Es gibt drei verschiedene Arten, um zusätzliche SQL-Datenbankserver an Azure-Speicher ein:
 
1. SQL Server 2012 CU4 und höher können systembedingt zusätzliche Datenbanken zu einer URL. Dies wird in der [neuen Funktionen in SQL Server 2014 – Teil 5 – Sicherung und Wiederherstellung Erweiterungen](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx)Blog ausführlich beschrieben. Finden Sie in Kapitel [SQL Server 2012 SP1 CU4 und höher] [Dbms Leitfaden 5.5.1].
1. SQL Server-Versionen vor SQL 2012 CU4 können zum Sichern in eine virtuelle Festplatte und verschieben in den Stream Schreiben in Richtung ein Azure-Speicherort, der so konfiguriert wurde, eine Umleitung Funktionalität verwenden. Finden Sie unter Kapitel [SQL Server 2012 SP1 CU3 und früheren Versionen] [Dbms Leitfaden 5.5.2].
1. Die letzte Methode ist zur Durchführung einer herkömmlichen SQL Server-Sicherung auf einem Datenträger-Gerät virtuelle Festplatte Befehls.  Dies ist mit der lokalen Bereitstellungsmuster identisch und wird nicht in diesem Dokument ausführlich erläutert.

#### <a name="a-name0fef0e79-d3fe-4ae2-85af-73666a6f7268asql-server-2012-sp1-cu4-and-later"></a><a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 und höher
Diese Funktionalität ermöglicht Ihnen für die direkt Sicherung Azure BLOB-Speicher. Ohne diese Methode müssen Sie in anderen Azure-virtuellen Festplatten sichern, die virtuellen Festplatte und IOPS Kapazität nutzen möchten. Die Idee ist im Grunde folgt:
 
 ![Verwenden von SQL Server 2012-Sicherung zu einem Microsoft Azure BLOB-Speicher][dbms-guide-figure-400]

Der Vorteil ist in diesem Fall, dass eine nicht virtuelle Festplatten zum Speichern von SQL Server-Sicherungskopien auf aufwenden muss. Daher müssen Sie weniger virtuelle Festplatten zugewiesen haben, und die gesamte Bandbreite von VHD IOPS für Daten-und Protokolldateien verwendet werden kann. Bitte beachten Sie, dass die maximale Größe von einer Sicherung auf bis zu 1 TB beschränkt ist, wie es im Abschnitt "Einschränkungen" in diesem Artikel beschrieben: <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Wenn die Sicherungsdatei Größe, trotz mit SQL Server-Sicherung Komprimierung 1 TB in übersteigen würde Größe, die Funktionalität beschrieben, die in Kapitel [SQL Server 2012 SP1 CU3 und früheren Versionen] [Dbms-Leitfaden-5.5.2] in diesem Dokument verwendet werden muss.

[Verwandte Dokumentation](https://msdn.microsoft.com/library/dn449492.aspx) zur Beschreibung der Wiederherstellung von Sicherungskopien auf Azure Blob-Speicher-Datenbanken empfehlen nicht direkt aus Azure BLOB-Speicher wiederherstellen, ist die Sicherungskopien > 25 GB. In diesem Artikel empfohlen basiert einfach Leistungsfähigkeit und nicht aufgrund funktionsübergreifendes Einschränkungen. Daher können unterschiedliche Situationen auf Basis von Fall anfallen.

Wie diese Art der Sicherung einrichten und genutzt Dokumentation finden Sie in [diesem](https://msdn.microsoft.com/library/dn466438.aspx) Lernprogramm
 
Ein Beispiel für die Reihenfolge der Schritte kann gelesen werden [können](https://msdn.microsoft.com/library/dn435916.aspx).

Automatisieren von Sicherungskopien, ist es der höchsten Wichtigkeit, um sicherzustellen, dass die BLOBs für jede Sicherung anders lauten. Andernfalls überschrieben werden, und die Wiederherstellung Kette ist unterbrochen.
 
Damit kann nicht zum Kombinieren von Elementen zwischen den 3 verschiedenen Arten von Sicherungskopien empfiehlt sich die verschiedene Containern unterhalb des Speicherkontos für Sicherungskopien erstellen. Die Container konnte durch virtueller Computer nur oder nach virtuellen Computer und Sicherung Typ werden. Das Schema konnte aussehen:
 
 ![Verwenden von SQL Server 2012-Sicherung in Microsoft Azure-Speicher BLOB – unterschiedlichen Container unter separaten Speicherkonto][dbms-guide-figure-500]

Im Beispiel oben würde die Sicherungskopien nicht in der gleichen Speicherkonto ausgeführt werden, in dem die virtuellen Computern bereitgestellt werden. Ein neues Speicherkonto speziell für die Sicherungskopien wäre. Innerhalb der Speicherkonten wäre verschiedene Container mit einer Matrix vom Typ der Sicherung und der Name des virtuellen Computers erstellt. Solche Segmentierung wird das verwalten die Sicherung von den anderen virtuellen Computern erleichtern.

Die BLOBs schreibt eine direkt die Sicherungskopien, um die Anzahl der virtuellen Festplatten eines virtuellen Computers nicht hinzugefügt werden. Daher konnte eine den Höchstwert von virtuellen Festplatten bereitgestellt werden, der für die Daten der VM SKU maximieren und Transaktion Protokolldatei und noch ausgeführt werden, eine Sicherung anhand eines Containers Speicher. 

#### <a name="a-namef9071eff-9d72-4f47-9da4-1852d782087basql-server-2012-sp1-cu3-and-earlier-releases"></a><a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 und früheren Versionen
Dieser erste Schritt, die Sie ausführen müssen, um eine Sicherung direkt gegen Azure-Speicher zu erreichen wäre die MSI-Datei herunterladen, die mit [diesem](https://www.microsoft.com/download/details.aspx?id=40740) Artikel KBA verknüpft ist.
 
Download der X64 Installation-Datei und in der Dokumentation. Die Datei wird ein Programm namens installiert: 'Microsoft SQL Server-Sicherung an Microsoft Azure-Tool'. Lesen Sie sorgfältig die Dokumentation des Produkts.  Das Tool funktioniert im Wesentlichen wie folgt:

* Von der Seite SQL Server Speicherort für die Sicherungskopie der SQL Server auf einem Datenträger definiert ist (verwenden Sie nicht das Laufwerk D:\ für diese).
* Das Tool können Sie zum Definieren von Regeln zu den verschiedene Arten von Sicherungskopien in andere Azure-Speicher Container direkten verwendet werden können.
* Sobald die Regeln vorhanden sind, wird das Tool Umleiten des Schreibstreams eines die Sicherung auf einen der virtuellen Festplatten/Datenträger zum Speicherort Azure zuvor definiert.
* Das Tool verlassen eine kleine Stub-Datei ein paar KB Größe auf die für den SQL Server definiert wurde virtuelle Festplatte/Datenträger Sicherung. **Diese Datei darf im Speicherort bleiben, da sie Sie erneut aus Azure-Speicher Wiederherstellen benötigt wird.**
    * Wenn Sie die Stub-Datei (z. B. durch Verlust der Speicherung Medien, die die Stub-Datei enthalten) verloren haben und Sie die Möglichkeit haben, mit einem Microsoft Azure-Speicher Konto sichern, können Sie die Stub-Datei mit Microsoft Azure Storage wiederherstellen, indem Sie herunterladen aus dem Speichercontainer, in dem er platziert wurde. Sie sollten die Stub-Datei in einem Ordner auf dem lokalen Computer platzieren Sie dann, in dem das Tool zu erkennen und Hochladen auf den gleichen Container mit demselben Verschlüsselungskennwort, wenn Verschlüsselung mit der ursprünglichen Regel konfiguriert ist. 

Dies bedeutet, dass das Schema für neuere Versionen von SQL Server wie oben beschrieben in Ort sowie für SQL Server-Versionen platziert werden kann nicht direkte Adresse einen Azure-Speicherort zugelassen werden.
 
Diese Methode sollte nicht verwendet werden mit neuere Versionen von SQL Server die zugrunde liegende systembedingt von gegen Azure-Speicher unterstützen. Eine Ausnahme bilden, Schwächen der systemeigenen Sicherung in Azure native Sicherung Ausführung in Azure blockiert werden.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Andere Verfahren, um, zusätzliche SQL Server-Datenbanken
Um zusätzliche Datenbanken in anderer Weise besteht darin, weitere virtuelle Festplatten eines virtuellen Computers, mit denen Sie zum Speichern der Sicherungskopien auf anfügen. In diesem Fall müssen Sie möchten sicherstellen, dass die virtuellen Festplatten nicht vollständige ausgeführt werden. Wenn dies der Fall ist, müssen Sie würde heben Sie die Bereitstellung der virtuellen Festplatte und Mal so zu sagen 'archivieren' und durch eine neue, leere virtuelle Festplatte ersetzen. Wenn Sie nach unten, dass der Pfad zugreifen, diese virtuelle Festplatten in separaten Azure-Speicher-Konten von denen beibehalten werden soll, die die virtuellen Festplatten durch die Datenbankdateien.

Eine zweite Möglichkeit besteht darin, ein großes virtuellen Computers verwenden, die viele virtuelle Festplatten angefügt werden können. Z. B. D14 mit 32VHDs. Verwenden Sie Speicher Leerzeichen, um eine flexible Umgebung zu erstellen, könnten Sie Freigaben erstellen, die dann als Sicherung Ziele für die verschiedenen DBMS-Servern verwendet werden.
 
Einige bewährte Methoden haben dokumentierten [hier](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) auch. 

#### <a name="performance-considerations-for-backupsrestores"></a>Leistungsaspekte für Sicherungskopien/Wiederherstellung
Wie softwareunabhängige Bereitstellungen ist Sicherung und Wiederherstellung Leistung wie viele Datenmengen parallel gelesen werden können und was der Durchsatz die Datenmengen möglicherweise abhängig. Darüber hinaus kann die CPU-Auslastung durch Komprimierung verwendet eine wesentliche Rolle auf virtuellen Computern mit einfach bis zu 8 CPU-Threads wiedergeben. Daher kann eine darstellen:

* Weniger die Anzahl der virtuellen Festplatten zum Speichern der Daten verwendet Dateien, je kleiner der Gesamtdurchsatz lesen.
* Je kleiner die Anzahl der CPU auf dem virtuellen Computer, je stärker den Einfluss der Komprimierung threads.
* Die weniger Ziele (BLOBs oder virtuelle Festplatten), der Sicherung in den kleineren den Durchsatz zu schreiben.
* Je kleiner der virtueller Computer Größe, je kleiner das Durchsatz Speicherkontingent schreiben und Lesen von Azure-Speicher. Unabhängig von, ob die Sicherungskopien direkt auf Azure Blob gespeichert sind, oder gibt an, ob sie in virtuellen Festplatten gespeichert sind, die in Azure Blobs erneut gespeichert werden.

Wenn Sie Microsoft Azure BLOB-Speicher als Sicherung Ziel in neuere Versionen verwenden, sind Sie auf nur eine URL-Ziel für jede bestimmte Sicherung festlegen beschränkt.
 
Aber bei Verwendung von 'Microsoft SQL Server-Sicherung an Microsoft Azure-Tool' in älteren Versionen können Sie mehr als eine Datei Ziel definieren. Mit mehr als ein Ziel kann die Sicherung skalieren und der Durchsatz der Sicherung höher ist. Dies würde dann mehrere Dateien als auch in der Azure-Speicher Konto führen. In unseren Tests mit implementiert mehrere Datei Ziele eine auf jeden Fall den Durchsatz erreichen kann, die eine mit der Sicherungsdatei Erweiterungen erreichen konnte aus SQL Server 2012 SP1 CU4 auf. Sie sind auch nicht durch die Beschränkung 1 TB wie in die native Sicherung in Azure blockiert.

Weiter, denken Sie daran, der Durchsatz ist auch hängt vom Standort des Kontos Speicher Azure Sie sich für die Sicherung verwenden. Das Speicher-Benutzerkonto in einem anderen Bereich zu suchen, als die virtuellen Computern, in ausgeführt werden eine Vorstellung möglicherweise. Z. B. können Sie die Konfiguration virtueller Computer in "Westen"-Europa ausgeführt, aber das Speicher-Konto, mit denen Sie in den Hintergrund von gegen in Nordamerika-Europa setzen. Die natürlich hat Einfluss auf die Sicherungsmediendurchsatz und wahrscheinlich keinen Durchsatz von 150 MB pro Sekunde generieren, wie es scheint in Fällen möglich sein, in dem die Zielspeicher und die virtuellen Computern im gleichen Landes-/ Datencenter ausgeführt werden.

#### <a name="managing-backup-blobs"></a>Verwalten von Sicherung BLOBs
Es ist eine Vorbedingung zum Verwalten der Sicherungskopien auf Ihrer eigenen ein. Da der Annahme ist, dass viele Blobs durch Ausführen von häufig verwendeten Transaktion Log Sicherungskopien erstellt werden, kann diese Blobs Verwaltung einfach im Portal Azure überlastet. Daher ist es recommendable, eine Azure-Speicher-Explorer zu nutzen. Es gibt mehrere gute diejenigen verfügbar die kann ein Konto Azure-Speicher verwalten

* Microsoft Visual Studio mit Azure SDK installiert (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure-Speicher-Explorer (<https://azure.microsoft.com/downloads/>)
* 3rd Party-tools

[Kommentar]: <>  (In der Cloud noch nicht unterstützt.)
[Kommentar]: <>  (### Azure-virtuellen Computer Sicherung)
[Kommentar]: <>  (Virtuellen Computern in das SAP-System können mithilfe von Funktionen Azure-virtuellen Computern Sicherung gesichert werden. Azure-virtuellen Computern Sicherung früh im Jahr 2015 eingeführt haben und in der Zwischenzeit wird ein standardmäßiges Verfahren zum Sichern eines vollständigen virtuellen Computers in Azure. Azure Sicherung speichert die Sicherungskopien in Azure und ermöglicht eine Wiederherstellung eines virtuellen Computers.) 
[Kommentar]: <> (virtuellen Computern, die ausführen Datenbanken konsistent ebenfalls gesichert werden können, wenn die Systeme DBMS unterstützt die Windows-VSS (Volume Dienstfehler - <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>) wie z. B. SQL Server. Also mit Azure-virtuellen Computer Sicherung könnte eine Möglichkeit, um eine wiederherstellbare Sicherung einer SAP-Datenbank zu gelangen. Bedenken Sie jedoch das basierend auf Azure-virtuellen Computer Sicherungskopien, Point-in-Time-Datenbanken wiederhergestellt wird, nicht möglich ist. Daher empfiehlt zur Sicherung der Datenbanken mit DBMS-Funktionen auf Azure-virtuellen Computer Sicherung angewiesen.) [Kommentar]: <> (um zu kennenzulernen Azure-virtuellen Computern Sicherung Bitte beginnen Sie hier <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="a-name1b353e38-21b3-4310-aeb6-a77e7c8e81c8ausing-a-sql-server-images-out-of-the-microsoft-azure-marketplace"></a><a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Verwenden einer SQL Server-Bilder aus Microsoft Azure Marketplace
Microsoft bietet virtuellen Computern in der Azure Marketplace die Versionen von SQL Server bereits enthalten. Dies kann für SAP-Benutzer, die Lizenzen für SQL Server und Windows benötigen, eine Verkaufschance zu bedecken die Notwendigkeit der Lizenzen im Grunde durch dreht von virtueller Computer mit SQL Server bereits installiert sein. Solche Bilder für SAP verwenden möchten, müssen unter folgenden Aspekten vorgenommen werden:

* Erfassen von die SQL Server-Versionen nicht Auswertung höhere Kosten als nur eines 'Nur Windows' virtuellen Computers aus Azure Marketplace bereitgestellt. Finden Sie in folgenden Artikeln Preise vergleichen: <https://azure.microsoft.com/pricing/details/virtual-machines/> und <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* Sie können nur SQL Server-Versionen verwenden, die von SAP, wie SQL Server 2012 unterstützt werden.
* Sortierreihenfolge des SQL Server-Instanz, in dem die virtuellen Computern angeboten in Azure Marketplace installiert ist ist nicht die Sortierung, die SAP NetWeaver auszuführenden SQL Server-Instanz ist erforderlich. Sie können die Sortierreihenfolge ändern, obwohl mit der erfahren Sie, wie im folgenden Abschnitt.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Ändern die SQL Server-Sortierung von Microsoft Windows/SQL Server virtueller Computer
Da die SQL Server-Bilder in der Azure Marketplace nicht die Sortierung verwendet das von Applications SAP NetWeaver benötigt wird eingerichtet sind, muss es unmittelbar nach der Bereitstellung geändert werden. Für SQL Server 2012 kann dies die folgenden Schritte aus, sobald der virtuellen Computer bereitgestellt wurde, und ein Administrator wird an den bereitgestellten virtuellen Computer anmelden:

* Öffnen Sie ein Windows-Befehl 'als Administrator'.
* Wechseln Sie zu c:\Programme c:\Programme\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Führen Sie den Befehl: Setup.exe/quiet /ACTION = REBUILDDATABASE/InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> ist das Konto, die als Administratorkonto definiert wurde, wenn Sie den virtuellen Computer zum ersten Mal durch den Katalog bereitstellen.

Dieser Vorgang sollte nur ein paar Minuten dauern. Damit sichergestellt ist, ob der Schritt mit dem richtigen Ergebnis eingestellt, nach oben, führen Sie die folgenden Schritte aus:

* Öffnen Sie SQL Server Management Studio.
* Öffnen Sie ein Abfragefenster.
* Führen Sie den Befehl Sp_helpsort in master SQL Server-Datenbank.

Das gewünschte Ergebnis aussehen sollte:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Wenn dies nicht das Ergebnis ist, beenden Sie die Bereitstellung von SAP und untersuchen Sie, warum der Befehl einrichten nicht wie erwartet funktioniert hat. Bereitstellung von SAP NetWeaver Applikationen auf SQL Server-Instanz mit anderen SQL Server-Codepages als den erwähnt über **nicht** unterstützt wird.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server hoher Verfügbarkeit für SAP in Azure
Wie weiter oben in diesem Dokument erwähnt, ist es nicht möglich freigegebenen Speicher erstellen, der für die Verwendung der Funktionalitäten hohen Verfügbarkeit ältesten SQL Server erforderlich ist. Diese Funktion würde mindestens zwei Instanzen von SQL Server in einer Windows Server Failover Cluster (WSFC) mit einem freigegebenen Datenträger für den Benutzer-Datenbanken (und schließlich Tempdb) installieren. Dies ist die Zeit, lang standard hohen Verfügbarkeit Methode, die auch von SAP unterstützt wird. Da Azure freigegebenen Speicher unterstützt, können keine Konfigurationen für SQL Server hohe Verfügbarkeit mit einer freigegebenen Datenträger-Konfiguration realisiert werden. Viele anderen Methoden, hohen Verfügbarkeit ist jedoch weiterhin möglich sind und werden in den folgenden Abschnitten beschrieben.

[Kommentar]: <>  (Artikel ist immer noch Refering zu ASM)
[Kommentar]: <>  (Bevor Sie die anderen bestimmten hohen Verfügbarkeit Technologien verwendbar für SQL Server in Azure, ein sehr gute Dokument Weitere Details und Zeiger [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions] ermöglicht vorhanden ist)

#### <a name="sql-server-log-shipping"></a>SQLServer anmelden Versand
Eine der Methoden der hohen Verfügbarkeit ist Liefer-zu SQL Server-Protokoll. Wenn die virtuellen Computern, die Teilnahme an der Konfigurations HA über das Arbeiten mit einer namensauflösung von verfügen, besteht kein Problem, und die Einrichtung in Azure nicht unterscheidet sich von einem beliebigen einrichten, die lokal ausgeführt wird. Es empfiehlt sich nicht auf IP-Auflösung nur verlassen. Überprüfen Sie im Hinblick auf Log Liefer- und die Grundsätze um Log Liefer-Einrichten dieser Dokumentation:

<https://technet.Microsoft.com/library/ms187103.aspx>
 
Um eine hohe Verfügbarkeit wirklich zu erreichen, muss eine die virtuellen Computern bereitstellen, die innerhalb einer solchen Log Liefer-Konfiguration innerhalb der gleichen Azure Verfügbarkeit festgelegt sein sind.

#### <a name="database-mirroring"></a>Spiegeln von Datenbanken
Datenbank: Spiegelung als unterstützte SAP (Siehe SAP-Hinweis [965908]) abhängig Failoverpartner in der Verbindungszeichenfolge SAP definieren. Für die Fälle Cross lokale wird davon ausgegangen, dass die zwei virtuellen Computern befinden sich in derselben Domäne und Kontext des Benutzers zwei SQL Server-Instanzen unter ausgeführt werden auch die Domänen-Benutzer sind und über die erforderlichen Berechtigungen in der verbindet zwei SQL Server-Instanzen. Daher unterscheidet sich die Einrichtung von in Azure-Datenbank Spiegelung nicht zwischen einer normalen lokalen Setup/Konfiguration.

Seit nur Cloud-Bereitstellungen ist die einfachste Methode, dass eine andere Domäne einrichten in Azure diese DBMS virtuellen Computern (und idealerweise dedizierten SAP-virtuellen Computern) haben innerhalb einer Domäne ein.

Wenn Sie eine Domäne nicht möglich ist, eine können Sie auch Zertifikate für die Datenbank, die Endpunkte Spiegelung, wie hier beschrieben: <https://technet.microsoft.com/library/ms191477.aspx>

Ein Lernprogramm zu ansetzen Spiegeln von Datenbanken in Azure kann, finden Sie hier: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>AlwaysOn
Wie AlwaysOn unterstützt wird für SAP lokal (Siehe SAP-Hinweis [1772688]), und es wird unterstützt, wenn Sie in Kombination mit SAP in Azure verwendet werden. Die Fakultät, dass Sie nicht freigegebene Datenträger in Azure erstellen können bedeutet nicht, dass eine eine Konfiguration AlwaysOn Windows Server Failover Cluster (WSFC) zwischen verschiedenen virtuellen Computern erstellen kann. Dies bedeutet nur, dass Sie nicht die Möglichkeit haben, verwenden Sie einen freigegebenen Datenträger als ein Quorum in der Cluster-Konfiguration verfügen. Daher können Sie eine AlwaysOn WSFC Konfiguration in Azure erstellen und wählen Sie einfach nicht die Quorumtyp, die freigegebenen Datenträger nutzt. Azure Bereitstellung der Umgebung werden diese virtuelle Computer in Beheben der virtuellen Computern anhand des Namens und der virtuellen Computern in derselben Domäne vorliegen sollten. Dies gilt für nur Azure und Cross lokale Bereitstellungen. Es gibt einige Besonderheiten zu beachten, um die Bereitstellung von SQL Server Verfügbarkeit Gruppe Zuhörer (nicht mit der Verfügbarkeit von Azure festlegen verwechselt werden), da Azure zu diesem Zeitpunkt nicht zulässt Objekt AD/DNS-einfach zu erstellen, wie es lokale möglich ist. Daher sind einige andere Installationsschritte erforderlich sind, um das spezifische Verhalten von Azure zu vermeiden.

Einige Punkte, die mit einer Verfügbarkeit Gruppe Zuhörer sind:

* Verwendung einer Verfügbarkeit Gruppe Zuhörer ist nur als Gast OS, der den virtuellen Computer mit Windows Server 2012 oder Windows Server 2012 R2 möglich. Für Windows Server 2012 müssen Sie sicherstellen, dass das Patch angewendet wird: <https://support.microsoft.com/kb/2854082> 
* Für Windows Server 2008 R2 dieser Patch ist nicht vorhanden und AlwaysOn muss auf die gleiche Weise wie eine Datenbank Spiegelung verwendet werden, indem Sie in der Zeichenfolge Verbindungen Failoverpartner (über den SAP-default.pfl Parameter Datenbanken/mss/Server – fertig finden Sie unter SAP-Hinweis [965908]).
* Bei Verwendung eines Verfügbarkeit Gruppe Zuhörer müssen die Datenbank virtuellen Computern mit einem dedizierten Lastenausgleich verbunden sein. Mit einer namensauflösung von in der Cloud nur Bereitstellungen müssten entweder alle virtuellen Computern der ein SAP-System (Anwendungsserver, DBMS Servers und (A) SCS) werden im gleichen virtuellen Netzwerk gar nicht oder nur aus einer SAP-Anwendungsebene die Wartung der Datei Etc\host akzeptieren, um die Namen virtueller Computer gelöst SQL Server virtueller Computer zu erhalten. Um zu vermeiden, dass Azure neue IP-Adressen in Fällen zuweisen ist, beide virtuellen Computern übrigen war(en) werden, eine sollte zuweisen statische IP-Adressen im Netzwerk-Schnittstellen die virtuellen Computern in der Konfiguration AlwaysOn (Definieren einer statischen IP-Adresse wird beschrieben, in [dieser] [ virtual-networks-reserved-private-ip] Artikel) [Kommentar]: <> (alten Blogs) [Kommentar]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Es gibt spezielle Schritte erforderlich beim Erstellen der Stelle, an der der Cluster eine spezielle IP-Adresse benötigt WSFC Cluster-Konfigurations zugewiesen, da Azure mit ihre aktuelle Funktionalität dem Clusternamen die IP-Adresse zuweisen möchten während der Erstellung des Knotens Cluster auf. Dies bedeutet, dass ein manueller Schritt ausgeführt werden muss, um eine andere IP-Adresse die Cluster zuzuweisen.
* Die Verfügbarkeit Gruppe Zuhörer soll in Azure mit TCP/IP Endpunkten erstellt werden, die mit den virtuellen Computern zugewiesen sind, die primären und sekundären Replikate der Verfügbarkeit Gruppe ausgeführt werden.
* Möglicherweise gibt es eine müssen diese Endpunkte mit ACLs secure.

[Kommentar]: <>  (Erledigen alten blog)
[Kommentar]: <>  (Die detaillierten Schritte und Ihren Schutz der Installation von eine AlwaysOn-Konfiguration auf Azure sind am besten vertraut, wenn Sie über die zusammengehörenden verfügbar [Here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups] durchlaufen)
[Kommentar]: <>  (Vorkonfiguriert AlwaysOn Setup über den Azure Katalog < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[Kommentar]: <>  (Erstellen einer Verfügbarkeit Gruppe Zuhörer lässt sich am besten in [this][virtual-machines-windows-classic-ps-sql-int-listener] Lernprogramm)
[Kommentar]: <>  (Sichern von Netzwerkendpunkte mit ACLs werden am besten erläutert, hier:)
[Kommentar]: <>  (* < Https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[Kommentar]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[Kommentar]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[Kommentar]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Es ist möglich, eine SQL Server AlwaysOn Availability Group über verschiedener ebenfalls Azure Regionen bereitzustellen. Dieses Feature wird die Nutzung der Azure-VNet Vnet-Konnektivität ([Weitere Details][virtual-networks-configure-vnet-to-vnet-connection]).
[Kommentar]: <> (erledigen alten Blog) [Kommentar]: <> (das Setup von SQL Server AlwaysOn Verfügbarkeit von Gruppen in diesem Fall wird hier beschrieben: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Zusammenfassung auf hohen Verfügbarkeit von SQL Server in Azure
Wenn die Fakultät, Azure-Speicher wird den Inhalt geschützt ist, Sie besteht eine kleiner Grund, klicken Sie auf ein Bild Tastaturkürzel Standby bestehen. Dies bedeutet, dass das hohen Verfügbarkeit Szenario muss nur gescannt mit den folgenden Fällen:

* Ausfall des den virtuellen Computer als Ganzes aufgrund einer Wartung auf dem Servercluster in Azure oder aus anderen Gründen
* Softwareprobleme in SQL Server-Instanz
* Schutz vor manuelle zurück, wobei Ruft Daten gelöscht und Point-in-Time-Wiederherstellung erforderlich ist

Betrachtet übereinstimmende Technologien, die eine, die die ersten beiden Fällen können, indem Sie Datenbankspiegeln oder AlwaysOn abgedeckt werden, sagen kann, während der dritte Groß-/Kleinschreibung nur durch Protokollieren und versenden abgedeckt werden kann.

Sie benötigen, die komplexere Einrichtung von AlwaysOn, im Vergleich zu spiegeln von Datenbanken mit Vorteile von AlwaysOn abzuwägen. Wie können diese Vorteile aufgeführt sein:

*   Lesbare sekundäre Replikate.
*   Sicherungskopien von sekundären Replikaten.
*   Eine bessere Skalierbarkeit.
*   Mehr als eine sekundäre Replikate.

### <a name="a-name9053f720-6f3b-4483-904d-15dc54141e30ageneral-sql-server-for-sap-on-azure-summary"></a><a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Allgemeine SQL Server für SAP auf Azure Zusammenfassung
Es gibt viele Empfehlungen im Zusammenhang mit diesem Leitfaden, und es wird empfohlen, dass Sie ihn mehrmals lesen, bevor Planung der Bereitstellung von Azure. In der Regel durch, halten Sie sich an den obersten zehn allgemeine DBMS auf Azure bestimmte Punkte:

[Kommentar]: <>  (2.3 höheren Durchsatz als was? Als eine virtuelle Festplatte?)
1. Verwenden Sie die neueste Version der DBMS, wie SQL Server-2014, die meisten Vorteile in Azure enthält. Dies ist für SQL Server SQL Server 2012 SP1 CU4 also auch das Feature für die Sicherung aller gegenüber den Azure-Speicher. Jedoch in Verbindung mit SAP wir empfehlen Ihnen mindestens SQL Server 2014 SP1 CU1 oder SQL Server 2012 SP2 und die neueste Ausschneiden.
1. Planen Sie sorgfältig Ihre SAP-System Querformat in Azure, die Datei Datenlayout und Azure Einschränkungen abzuwägen ein:
    * Nicht zu viele virtuelle Festplatten, haben Sie aber genug, um sicherzustellen, dass Sie Ihre erforderlichen IOPS erreichen können.
    * Denken Sie daran, dass auch IOPS pro Azure-Speicherkonto beschränkt sind und Speicherkonten innerhalb jeder Azure-Abonnement beschränkt sind ([Weitere Informationen hierzu][azure-subscription-service-limits]). 
    * Nur Streifen über virtuelle Festplatten, wenn Sie benötigen, um einen höheren Durchsatz zu erzielen.
1. Installieren von Software nie oder alle Dateien, die auf das Laufwerk D:\ Beibehaltung erfordern, wie es nicht dauerhafte ist und nichts auf diesem Laufwerk bei einem Windows-Neustart verloren setzen.
1. Verwenden Sie keine Azure-virtuellen Festplatte Zwischenspeichern für standardmäßigen Azure-Speicher.
1. Verwenden Sie keine Azure Geo repliziert Speicherkonten aus.  Verwenden Sie lokal redundante für DBMS Auslastung.
1. Verwenden des Herstellers Ihres DBMS HA/DR-Lösung Datenbankdaten repliziert.
1. Immer mit einer Auflösung von Namen zu verwenden, verlassen Sie sich nicht auf IP-Adressen.
1. Verwenden Sie die höchste Datenbank Komprimierung möglich. Für SQL Server ist dies Seite Komprimierung.
1. Achten Sie mit SQL Server-Bilder aus dem Azure Marketplace. Wenn Sie eine SQL Server verwenden, müssen Sie die Sortierung Instanz vor der Neuinstallation alle NetWeaver SAP-System daran ändern.
1. Installieren Sie und konfigurieren Sie der SAP-Host Überwachung für Azure, wie in [Bereitstellungshandbuch] [Bereitstellungshandbuch] beschrieben.

## <a name="specifics-to-sap-ase-on-windows"></a>Einzelheiten zu SAP-ASE unter Windows
Beginnend mit Microsoft Azure, können Sie einfach Ihre vorhandene ASE SAP-Anwendung zu Azure virtuellen Computern migrieren. SAP-ASE in einer virtuellen Computern können Sie die Gesamtkosten der Bereitstellung, Verwaltung und Wartung der Enterprise Umfang Applications zu reduzieren, indem Sie einfach diesen Anwendungen in Microsoft Azure migrieren. Mit SAP-ASE in einer Azure-virtuellen Computern können Administratoren und Entwickler weiterhin verwenden, die gleichen Entwicklung und Verwaltungstools, die lokale verfügbar sind.

Es wird eine Vereinbarung zum SERVICELEVEL für den Azure virtuellen Computern die hier zu finden sind: <https://azure.microsoft.com/support/legal/sla>

Wir sind sicher, dass Microsoft Azure gehosteten virtuellen Computern sehr gut im Vergleich zu anderen öffentlichen Cloud Virtualisierung Angebote, aber einzelne Ergebnisse ausführen soll variieren können. SAP-Größe SAPS Zahlen von den verschiedenen SAP zertifizierte virtueller Computer SKUs in einer separaten SAP-Hinweis [1928533]bereitgestellt wird.

Anweisungen und Empfehlungen hinsichtlich der Verwendung von Azure-Speicher, Bereitstellung von SAP-virtuellen Computern oder für die Überwachung von SAP-gelten-Bereitstellungen mit SAP-ASE in Verbindung mit SAP-Anwendungen wie in diesem Dokument die ersten vier Kapitel erwähnt.

### <a name="sap-ase-version-support"></a>SAP-Version ASE-Unterstützung 
SAP aktuell unterstützt SAP-ASE Version 16,0 zur Verwendung mit SAP-Business Suite-Produkten. Alle Updates für SAP-ASE Server oder JDBC und ODBC-Treiber mit SAP-Business Suite-Produkten verwendet werden allein über SAP Service Marketplace bei bereitgestellt werden: <https://support.sap.com/swdc>.

Wie für Installationen lokalen Laden Sie Updates für den SAP-ASE Server, oder die JDBC und ODBC-Treiber nicht direkt aus Sybase-Websites. Ausführliche Informationen zu Patches, die für die Verwendung mit SAP-Business Suite Produkte lokal und in Azure-virtuellen Computern unterstützt werden finden Sie unter den folgenden SAP-Notizen:

* [1590719]
* [1973241]
 
Allgemeine Informationen zum Ausführen von SAP-Business-Suite auf SAP-ASE finden Sie in der [SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Richtlinien für die Konfiguration von SAP-ASE für SAP Zusammenhang ASE SAP-Installationen in Azure-virtuellen Computern

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktur der ASE SAP-Bereitstellung
Gemäß der allgemeinen Beschreibung, SAP-ASE Programmdateien ansässig oder in dem Systemlaufwerk des des virtuellen Computers Basis virtuelle Festplatte installiert werden sollen (Laufwerk c:\). Die meisten Datenbanken ASE SAP-System und Tools in der Regel werden von SAP NetWeaver Arbeitsbelastung nicht wirklich harte eingesetzt werden. Daher können die System und Tools Datenbanken (Master-Shape, Modell, Saptools, Sybmgmtdb, Sybsystemdb) auf den C:\drive als auch bleiben. 

Eine Ausnahme könnte die temporäre Datenbank mit allen Arbeitstabellen und temporäre SAP-ASE, die bei einigen SAP ERP und alle BW Auslastung erfordern möglicherweise entweder höhere Daten oder e/a-Vorgänge Datenträgers die nicht in der ursprünglichen virtuellen Basis virtuelle Festplatte passt erstellte Tabellen (Laufwerk c:\).
 
Je nach der Version des SAPInst/SWPM verwendet, um das System zu installieren möglicherweise die Datenbank enthalten:

* Eine einzelne SAP-ASE temporäre, die bei der Installation von SAP-ASE erstellt wird
* Ein SAP-ASE Tempdb der Installation von SAP-ASE und eine zusätzliche Saptempdb erstellt, die SAP-Installationsroutine erstellte
* Ein SAP-ASE Tempdb erstellt SAP-ASE und einer weiteren Tempdb, die manuell (z. B. folgenden SAP-Hinweis [1752266]) erstellt wurden, um ERP/BW bestimmte Tempdb erfüllen installieren.

Bei bestimmten ERP oder alle BW Auslastung ist es sinnvoll, hinsichtlich der Systemleistung die Tempdb Geräte für die darüber hinaus erstellten Tempdb (durch SWPM oder manuell) auf einem anderen Laufwerk als C: beibehalten\. ist keine zusätzliche Tempdb sie vorhanden ist, wird empfohlen, eine (SAP-Hinweis [1752266]) erstellen.

Für solche Systeme sollten die folgenden Schritte für die darüber hinaus erstellten Tempdb ausgeführt werden:

* Verschieben Sie das erste Tempdb Gerät zum ersten Gerät der SAP-Datenbank
* Jeder der virtuellen Festplatten, enthält eine Geräten der SAP-Datenbank fügen Sie Tempdb Geräte hinzu

Bei dieser Konfiguration kann Tempdb entweder beanspruchen mehr Platz als das Systemlaufwerk Möglichkeit zu bieten, ist. Als Referenz kann eine Tempdb Gerät Größen vorhandenen Betriebssystemen überprüfen, die lokal ausgeführt. Oder eine solche Konfiguration IOPS Zahlen anhand der Tempdb, die mit dem Systemlaufwerk zur Verfügung gestellt werden kann nicht aktivieren möchten. Systeme, die lokal ausgeführt werden können erneut verwendet werden, e/a-Arbeitsbelastung gegen Tempdb überwachen.

Setzen Sie alle SAP-ASE Geräte nie auf das Laufwerk D:\ von den virtuellen Computer. Dies gilt auch für die Tempdb, auch wenn die Objekte in der Datenbank Tempdb gespeichert sind nur temporär sind.

#### <a name="impact-of-database-compression"></a>Auswirkung der Datenbank Komprimierung
In Konfigurationen, e Bandbreite eine Beschränkung verwendet werden kann, kann jeder Measure die IOPS reduziert hilfreich die Arbeitsbelastung dehnen, das eine in einem Szenario IaaS wie Azure ausführen kann. Es wird daher dringend empfohlen, um sicherzustellen, dass SAP-ASE Komprimierung verwendet wird, bevor Sie Hochladen einer vorhandenen SAP-Datenbank in Azure.

Ausführen der Komprimierung vor dem Hochladen auf Azure, wenn es nicht bereits implementiert wird empfohlen wird von aus den folgenden Gründen angegeben:

* Die Datenmenge in Azure hochgeladen werden ist unteren
* Die Dauer der Ausführung Komprimierung ist kürzer, unter der Voraussetzung, dass eine sicheres Hardware mit mehr CPUs oder höhere e/a-Bandbreite oder weniger e/a-Wartezeit lokal verwenden können
* Kleinere Datenbank möglicherweise zu weniger Kosten für die Zuordnung auf dem Datenträger führen.

Daten und LOB Komprimierung funktionieren in einen virtuellen in Azure virtuellen Computern gehostet, wie lokalen bedeutet. Aktivieren Sie SAP-Hinweis [1750510], weitere Details zum Überprüfen, ob Komprimierung bereits in einer vorhandenen ASE SAP-Datenbank verwendet wird.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Verwenden von DBACockpit zum Überwachen der Datenbankinstanzen
Für SAP-Systemen, die SAP-ASE als Datenbankplattform verwenden, kann der DBACockpit zugegriffen werden als eingebettete Browserfenster in Transaktion DBACockpit oder Webdynpro. Die vollständige Funktionalität zum Überwachen und Verwalten der Datenbank ist jedoch in der Implementierung Webdynpro nur die DBACockpit zur Verfügung.

Als sind mit lokalen Systemen mehrere Schritte erforderlich alle SAP NetWeaver-Funktion verwendet, die durch die Webdynpro-Implementierung von der DBACockpit zu aktivieren. Folgen Sie SAP-Hinweis [1245200] zum Aktivieren der Verwendung von Webdynpros und erforderlichen diejenigen generieren. Wenn folgen die Anweisungen in der obigen Notizen konfigurieren Sie auch die Internet-Kommunikation-Manager (Icm) sowie die Ports für http und Https-Verbindungen verwendet werden soll. Die Standardeinstellung für http sieht wie folgt aus:

> ICM/server_port_0 = Protokoll = HTTP, PORT = 8000, PROCTIMEOUT = 600, TIMEOUT = 600
>
> ICM/server_port_1 = Protokoll = HTTPS und PORT 443$ $, PROCTIMEOUT = 600, TIMEOUT = 600 =

und die Links in Transaktion generiert DBACockpit sieht ungefähr wie folgt aus:

> https://`<fullyqualifiedhostname`>: 44300/Sap/bc/Webdynpro/Sap/Dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/Sap/bc/Webdynpro/Sap/Dba_cockpit

Je nachdem, welcher ist ob und wie die Azure-virtuellen Computern Hostinganbieter das SAP-System über Standort-zu-Standort verbunden ist mit mehreren Standorten oder ExpressRoute (Cross lokale Bereitstellung) Sie sicherstellen, dass die ICM müssen verwenden einen vollqualifizierten Hostnamen, der aufgelöst werden kann auf dem Computer, in dem Sie versuchen, öffnen Sie die DBACockpit aus. Finden Sie unter SAP-Hinweis [773830] zu verstehen, wie ICM den vollqualifizierten Hostnamen je nach Profilparameter bestimmt, und richten Sie Parameter Icm/Host_name_full explizit, sofern dies erforderlich.

Wenn Sie den virtuellen Computer in einem Szenario Cloud nur ohne Cross lokale Konnektivität zwischen lokalen und Azure bereitgestellt haben, müssen Sie eine öffentliche IP-Adresse und eine Domainlabel definieren. Das Format von den öffentlichen DNS-Namen für den virtuellen Computer sieht dann wie folgt aus:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Weitere Details im Zusammenhang mit den DNS-Namen finden Sie [hier][virtual-machines-azurerm-versus-azuresm].

Festlegen von SAP-Profil Parameter Icm/Host_name_full auf der DNS-Name, der den Azure-virtuellen Computer den Link möglicherweise ähnlich wie aus:

> Https://mydomainlabel.westeurope.cloudapp.NET:44300/Sap/bc/Webdynpro/Sap/dba_cockpit

> Http://mydomainlabel.westeurope.cloudapp.NET:8000/Sap/bc/Webdynpro/Sap/dba_cockpit

In diesem Fall müssen Sie sicherstellen:

* Fügen Sie eingehende Regeln zur Azure-Portal, für die TCP/IP Ports verwendet zur Kommunikation mit ICM Netzwerk Sicherheitsgruppe hinzu
* Fügen Sie eingehende Regeln an der Konfiguration der Windows-Firewall für die TCP/IP Ports verwendet zur Kommunikation mit dem ICM hinzu

Für eine automatisierte importiert, der alle Korrekturen zur Verfügung empfiehlt es sich um regelmäßig die Sammlung Korrektur SAP-Notiz anwendbar Ihrer SAP-Version anzuwenden:

* [1558958]
* [1619967]
* [1882376]

Weitere Informationen zu DBA Cockpit für SAP-ASE finden Sie im folgenden SAP-Notizen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Sicherung/Wiederherstellung Aspekte für SAP-ASE
Wenn Sie SAP-ASE in Azure bereitstellen, die Ihre Sicherung Methode überprüft werden muss. Auch wenn das System kein System produktiv ist, muss die SAP-Datenbank von SAP-ASE gehostet wird regelmäßig gesichert werden. Da Azure-Speicher drei Bilder gespeichert sind, ist eine Sicherung jetzt in Bezug auf einen Absturz Speicher compensating weniger wichtig. Der wichtigste Grund für die Verwaltung eines richtigen sichern und Wiederherstellen Plans ist mehr, die Sie durch die Bereitstellung von Point-in-Time-Wiederherstellungsfunktionen logischen/manueller Fehler kompensieren können. Somit ist das Ziel zu entweder Sicherungskopien verwenden Sie zum Wiederherstellen der Datenbank an einem bestimmten Punkt in Uhrzeit oder die Sicherungskopien in Azure verwenden, um ein anderes System durch Kopieren die vorhandene Datenbank zu starten. Sie können beispielsweise Wiederherstellen einer Sicherung in einer 3-Ebenen-System-Setup des gleichen Systems aus einer 2-Ebenen SAP-Konfiguration übertragen.

Sichern und Wiederherstellen einer Datenbank in Azure funktioniert genauso wie lokal. Finden Sie SAP-Notizen:

* [1588316]
* [1585981]

Ausführliche Informationen zum Erstellen von Abbild Konfigurationen und Planung Sicherungskopien. Je nach verwendetem Strategie und Anforderungen, die Sie konfigurieren können, sichert und der Protokolldateien zum Datenträger auf eine von der vorhandenen virtuellen Festplatten oder zum Hinzufügen einer weiteren virtuellen Festplatte für die Sicherung.  Klicken Sie zum Verringern der Gefahr von Datenverlusten im Falle eines Fehlers empfiehlt es sich um eine virtuelle Festplatte verwenden, in dem kein Datenbank-Gerät befindet.

Zusätzlich zu Daten – und der BRANCHENDATEN bietet Komprimierung SAP-ASE auch Komprimierung. Um weniger Platz für die Datenbank und der Protokolldateien speichert empfiehlt es sich Komprimierung verwenden. Finden Sie SAP-Hinweis [1588316] Weitere Informationen. Komprimieren die Sicherung ist entscheidend zum Verringern der Datenmenge übertragen werden, wenn Sie beabsichtigen, Sicherungskopien oder virtuelle, enthält die Sicherungsdatei speichert aus Azure virtuellen Computers zu lokalen Festplatten herunterladen.

Verwenden Sie Laufwerk D:\ nicht als Datenbank- oder Protokolldateien Abbild Ziel aus.

#### <a name="performance-considerations-for-backupsrestores"></a>Leistungsaspekte für Sicherungskopien/Wiederherstellung
Wie softwareunabhängige Bereitstellungen ist Sicherung und Wiederherstellung Leistung wie viele Datenmengen parallel gelesen werden können und was der Durchsatz die Datenmengen möglicherweise abhängig. Darüber hinaus kann die CPU-Auslastung durch Komprimierung verwendet eine wesentliche Rolle auf virtuellen Computern mit einfach bis zu 8 CPU-Threads wiedergeben. Daher kann eine darstellen:

* Weniger die Anzahl der virtuellen Festplatten verwendet wird, um die Geräte Datenbank speichern die kleinere Gesamtdurchsatz im Lesemodus.
* Je kleiner die Anzahl der CPU-Suchthreads, auf dem virtuellen Computer, je stärker den Einfluss der Komprimierung
* Der Sicherung, den kleineren den Durchsatz sowie das Schreiben in das weniger Ziele (Streifen Verzeichnisse durchsuchen, virtuellen Festplatten)

Klicken Sie zum Erhöhen der Anzahl der Ziele zu schreiben, dass es gibt zwei Optionen, die je nach Ihren Anforderungen verwendet/kombiniert werden können:

* Das Sicherungszielvolume Striping über mehrere bereitgestellten virtuellen Festplatten zur Verbesserung des IOPS-Durchsatzes auf dem aufgeteilten volume
* Erstellen einer Konfigurations des SAP-ASE Ebene die mehr als ein Zielverzeichnis wird verwendet, um das Abbild zu schreiben

Ein Volume über mehrere bereitgestellten virtuellen Festplatten Striping weist zuvor in diesem Handbuch beschrieben wurde. Weitere Informationen zur Verwendung mehrerer Verzeichnisse in die Konfiguration des SAP-ASE finden Sie in der Dokumentation auf gespeicherte Prozedur Sp_config_dump dem verwendet wird, um die Konfiguration der [Sybase InfoCenter für](http://infocenter.sybase.com/help/index.jsp)erstellen.

### <a name="disaster-recovery-with-azure-vms"></a>Wiederherstellung mit Azure-virtuellen Computern

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Datenreplikation mit SAP-Sybase Replikationsserver
Mit den SAP-Sybase Replikation Server (SRS) SAP-ASE bietet eine warme standby-Lösung Datenbanktransaktionen asynchrone an einem entfernten Standort übertragen. 

Installation und zum Betrieb von SRS funktioniert auch funktional in einen virtuellen Azure-virtuellen Computern Dienste gehostet wird, wie lokalen bedeutet.

ASE HADR über SAP-Replikationsserver ist mit einer späteren Version geplant. Es wird mit getestet und zur Microsoft Azure Plattformen freigegeben werden, sobald sie verfügbar ist.

## <a name="specifics-to-sap-ase-on-linux"></a>Einzelheiten zu SAP-ASE unter Linux

Beginnend mit Microsoft Azure, können Sie einfach Ihre vorhandene ASE SAP-Anwendung zu Azure virtuellen Computern migrieren. SAP-ASE in einer virtuellen Computern können Sie die Gesamtkosten der Bereitstellung, Verwaltung und Wartung der Enterprise Umfang Applications zu reduzieren, indem Sie einfach diesen Anwendungen in Microsoft Azure migrieren. Mit SAP-ASE in einer Azure-virtuellen Computern können Administratoren und Entwickler weiterhin verwenden, die gleichen Entwicklung und Verwaltungstools, die lokale verfügbar sind.

Für die Bereitstellung von Azure-virtuellen Computern es ist wichtig, die offizielle SLAs wissen, welche die hier zu finden sind: <https://azure.microsoft.com/support/legal/sla>

SAP-Ziehpunkts Informationen sowie eine Liste von SAP-zertifiziert virtueller Computer SKUs werden im SAP-Hinweis [1928533]bereitgestellt werden. Weitere Dokumente für Azure-virtuellen Computern Ziehpunkt SAP <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> und hier <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx> finden Sie hier

Anweisungen und Empfehlungen hinsichtlich der Verwendung von Azure-Speicher, Bereitstellung von SAP-virtuellen Computern oder für die Überwachung von SAP-gelten-Bereitstellungen mit SAP-ASE in Verbindung mit SAP-Anwendungen wie in diesem Dokument die ersten vier Kapitel erwähnt.

Die beiden folgenden SAP-Hinweise umfassen allgemeine Informationen zu ASE unter Linux und ASE in der Cloud an:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Unterstützung für SAP-ASE-Version 
SAP aktuell unterstützt SAP-ASE Version 16,0 zur Verwendung mit SAP-Business Suite-Produkten. Alle Updates für SAP-ASE Server oder JDBC und ODBC-Treiber mit SAP-Business Suite-Produkten verwendet werden allein über SAP Service Marketplace bei bereitgestellt werden: <https://support.sap.com/swdc>.

Wie für Installationen lokalen Laden Sie Updates für den SAP-ASE Server, oder die JDBC und ODBC-Treiber nicht direkt aus Sybase-Websites. Ausführliche Informationen zu Patches, die für die Verwendung mit SAP-Business Suite Produkte lokal und in Azure-virtuellen Computern unterstützt werden finden Sie unter den folgenden SAP-Notizen:

* [1590719]
* [1973241]
 
Allgemeine Informationen zum Ausführen von SAP-Business-Suite auf SAP-ASE finden Sie in der [SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Richtlinien für die Konfiguration von SAP-ASE für SAP Zusammenhang ASE SAP-Installationen in Azure-virtuellen Computern

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktur der ASE SAP-Bereitstellung
Gemäß der allgemeine Beschreibung werden ASE SAP-Programmdateien ansässig oder in Root-Dateisystem von den virtuellen Computer (/sybase) installiert. Die meisten Datenbanken ASE SAP-System und Tools in der Regel werden von SAP NetWeaver Arbeitsbelastung nicht wirklich harte eingesetzt werden. Daher können die System und Tools Datenbanken (Master-Shape, Modell, Saptools, Sybmgmtdb, Sybsystemdb) auf Root-Dateisystem auch bleiben. 

Eine Ausnahme könnte die temporäre Datenbank mit allen Arbeitstabellen und temporäre Tabellen erstellte SAP-ASE, die bei einigen SAP ERP und alle BW Auslastung erfordern möglicherweise entweder höhere Daten oder e/a-Vorgänge Datenträgers die nicht in der ursprünglichen virtuellen OS Datenträger passt.
 
Je nach der Version des SAPInst/SWPM verwendet, um das System zu installieren möglicherweise die Datenbank enthalten:

* Eine einzelne SAP-ASE temporäre, die bei der Installation von SAP-ASE erstellt wird
* Ein SAP-ASE Tempdb der Installation von SAP-ASE und eine zusätzliche Saptempdb erstellt, die SAP-Installationsroutine erstellte
* Ein SAP-ASE Tempdb erstellt SAP-ASE und einer weiteren Tempdb, die manuell (z. B. folgenden SAP-Hinweis [1752266]) erstellt wurden, um ERP/BW bestimmte Tempdb erfüllen installieren.

Bei einem bestimmten ERP oder alle BW Auslastung sinnvoll es, hinsichtlich der Leistung, bleiben die Tempdb Geräte für die darüber hinaus erstellten Tempdb (durch SWPM oder manuell) auf einem separaten Dateisystem durch einen einzelnen Azure Datenträger oder ein Linux RAID mehrerer Azure Daten Datenträger dargestellt werden kann. Wenn keine zusätzliche Tempdb vorhanden wird empfohlen, um einen SAP-Hinweis ( [1752266]) zu erstellen.

Für solche Systeme sollten die folgenden Schritte für die darüber hinaus erstellten Tempdb ausgeführt werden:

* Verschieben Sie das erste Tempdb Verzeichnis in das erste Dateisystem der SAP-Datenbank
* Hinzufügen von Tempdb Verzeichnisse zu den einzelnen der virtuellen Festplatten, enthält ein Dateisystem der SAP-Datenbank

Bei dieser Konfiguration kann Tempdb entweder beanspruchen mehr Platz als das Systemlaufwerk Möglichkeit zu bieten, ist. Als Referenz kann eine Tempdb Directory Größen vorhandenen Betriebssystemen überprüfen, die lokal ausgeführt. Oder eine solche Konfiguration IOPS Zahlen anhand der Tempdb, die mit dem Systemlaufwerk zur Verfügung gestellt werden kann nicht aktivieren möchten. Systeme, die lokal ausgeführt werden können erneut verwendet werden, e/a-Arbeitsbelastung gegen Tempdb überwachen.

Setzen Sie alle SAP-ASE Verzeichnisse nie auf/mnt oder /mnt/resource von den virtuellen Computer. Dies gilt auch für die Tempdb, auch wenn die Objekte in der Datenbank Tempdb gespeichert sind nur temporäre da/mnt oder /mnt/resource eine standardmäßige Azure-virtuellen Computer temporären Speicherplatz ist, der nicht beständigen ist. Weitere Details im Azure-virtuellen temp-Arbeitsspeicher finden Sie in [diesem Artikel][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Auswirkung der Datenbank Komprimierung
In Konfigurationen, e Bandbreite eine Beschränkung verwendet werden kann, kann jeder Measure die IOPS reduziert hilfreich die Arbeitsbelastung dehnen, das eine in einem Szenario IaaS wie Azure ausführen kann. Es wird daher dringend empfohlen, um sicherzustellen, dass SAP-ASE Komprimierung verwendet wird, bevor Sie Hochladen einer vorhandenen SAP-Datenbank in Azure.

Ausführen der Komprimierung vor dem Hochladen auf Azure, wenn es nicht bereits implementiert wird empfohlen wird von aus den folgenden Gründen angegeben:

* Die Datenmenge in Azure hochgeladen werden ist unteren
* Die Dauer der Ausführung Komprimierung ist kürzer, unter der Voraussetzung, dass eine sicheres Hardware mit mehr CPUs oder höhere e/a-Bandbreite oder weniger e/a-Wartezeit lokal verwenden können
* Kleinere Datenbank möglicherweise zu weniger Kosten für die Zuordnung auf dem Datenträger führen.

Daten und LOB Komprimierung funktionieren in einen virtuellen in Azure virtuellen Computern gehostet, wie lokalen bedeutet. Aktivieren Sie SAP-Hinweis [1750510], weitere Details zum Überprüfen, ob Komprimierung bereits in einer vorhandenen ASE SAP-Datenbank verwendet wird. Siehe auch SAP-Hinweis [2121797] Weitere Informationen zum Datenbank Komprimierung.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Verwenden von DBACockpit zum Überwachen der Datenbankinstanzen
Für SAP-Systemen, die SAP-ASE als Datenbankplattform verwenden, kann der DBACockpit zugegriffen werden als eingebettete Browserfenster in Transaktion DBACockpit oder Webdynpro. Die vollständige Funktionalität zum Überwachen und Verwalten der Datenbank ist jedoch in der Implementierung Webdynpro nur die DBACockpit zur Verfügung.

Als sind mit lokalen Systemen mehrere Schritte erforderlich alle SAP NetWeaver-Funktion verwendet, die durch die Webdynpro-Implementierung von der DBACockpit zu aktivieren. Folgen Sie SAP-Hinweis [1245200] zum Aktivieren der Verwendung von Webdynpros und erforderlichen diejenigen generieren. Wenn folgen die Anweisungen in der obigen Notizen konfigurieren Sie auch die Internet-Kommunikation-Manager (Icm) sowie die Ports für http und Https-Verbindungen verwendet werden soll. Die Standardeinstellung für http sieht wie folgt aus:

> ICM/server_port_0 = Protokoll = HTTP, PORT = 8000, PROCTIMEOUT = 600, TIMEOUT = 600
>
> ICM/server_port_1 = Protokoll = HTTPS und PORT 443$ $, PROCTIMEOUT = 600, TIMEOUT = 600 =

und die Links in Transaktion generiert DBACockpit sieht ungefähr wie folgt aus:

> https://`<fullyqualifiedhostname`>: 44300/Sap/bc/Webdynpro/Sap/Dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/Sap/bc/Webdynpro/Sap/Dba_cockpit

Je nachdem, welcher ist ob und wie die Azure-virtuellen Computern Hostinganbieter das SAP-System über Standort-zu-Standort verbunden ist mit mehreren Standorten oder ExpressRoute (Cross lokale Bereitstellung) Sie sicherstellen, dass die ICM müssen verwenden einen vollqualifizierten Hostnamen, der aufgelöst werden kann auf dem Computer, in dem Sie versuchen, öffnen Sie die DBACockpit aus. Finden Sie unter SAP-Hinweis [773830] zu verstehen, wie ICM den vollqualifizierten Hostnamen je nach Profilparameter bestimmt, und richten Sie Parameter Icm/Host_name_full explizit, sofern dies erforderlich.

Wenn Sie den virtuellen Computer in einem Szenario Cloud nur ohne Cross lokale Konnektivität zwischen lokalen und Azure bereitgestellt haben, müssen Sie eine öffentliche IP-Adresse und eine Domainlabel definieren. Das Format von den öffentlichen DNS-Namen für den virtuellen Computer sieht dann wie folgt aus:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Weitere Details im Zusammenhang mit den DNS-Namen finden Sie [hier][virtual-machines-azurerm-versus-azuresm].

Festlegen von SAP-Profil Parameter Icm/Host_name_full auf der DNS-Name, der den Azure-virtuellen Computer den Link möglicherweise ähnlich wie aus:

> Https://mydomainlabel.westeurope.cloudapp.NET:44300/Sap/bc/Webdynpro/Sap/dba_cockpit
> 
> Http://mydomainlabel.westeurope.cloudapp.NET:8000/Sap/bc/Webdynpro/Sap/dba_cockpit

In diesem Fall müssen Sie sicherstellen:

* Fügen Sie eingehende Regeln zur Azure-Portal, für die TCP/IP Ports verwendet zur Kommunikation mit ICM Netzwerk Sicherheitsgruppe hinzu
* Fügen Sie eingehende Regeln an der Konfiguration der Windows-Firewall für die TCP/IP Ports verwendet zur Kommunikation mit dem ICM hinzu

Für eine automatisierte importiert, der alle Korrekturen zur Verfügung empfiehlt es sich um regelmäßig die Sammlung Korrektur SAP-Notiz anwendbar Ihrer SAP-Version anzuwenden:

* [1558958]
* [1619967]
* [1882376]

Weitere Informationen zu DBA Cockpit für SAP-ASE finden Sie im folgenden SAP-Notizen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Sicherung/Wiederherstellung Aspekte für SAP-ASE
Wenn Sie SAP-ASE in Azure bereitstellen, die Ihre Sicherung Methode überprüft werden muss. Auch wenn das System kein System produktiv ist, muss die SAP-Datenbank von SAP-ASE gehostet wird regelmäßig gesichert werden. Da Azure-Speicher drei Bilder gespeichert sind, ist eine Sicherung jetzt in Bezug auf einen Absturz Speicher compensating weniger wichtig. Der wichtigste Grund für die Verwaltung eines richtigen sichern und Wiederherstellen Plans ist mehr, die Sie durch die Bereitstellung von Point-in-Time-Wiederherstellungsfunktionen logischen/manueller Fehler kompensieren können. Somit ist das Ziel zu entweder Sicherungskopien verwenden Sie zum Wiederherstellen der Datenbank an einem bestimmten Punkt in Uhrzeit oder die Sicherungskopien in Azure verwenden, um ein anderes System durch Kopieren die vorhandene Datenbank zu starten. Sie können beispielsweise Wiederherstellen einer Sicherung in einer 3-Ebenen-System-Setup des gleichen Systems aus einer 2-Ebenen SAP-Konfiguration übertragen.

Sichern und Wiederherstellen einer Datenbank in Azure funktioniert genauso wie lokal. Finden Sie SAP-Notizen:

* [1588316]
* [1585981]

Ausführliche Informationen zum Erstellen von Abbild Konfigurationen und Planung Sicherungskopien. Je nach verwendetem Strategie und Anforderungen, die Sie konfigurieren können, sichert und der Protokolldateien zum Datenträger auf eine von der vorhandenen virtuellen Festplatten oder zum Hinzufügen einer weiteren virtuellen Festplatte für die Sicherung.  Klicken Sie zum Verringern der Gefahr von Datenverlusten im Falle eines Fehlers wird empfohlen, eine virtuelle Festplatte verwenden, in dem keine Datenbankdatei/Verzeichnis befindet.

Zusätzlich zu Daten – und der BRANCHENDATEN bietet Komprimierung SAP-ASE auch Komprimierung. Um weniger Platz für die Datenbank und der Protokolldateien speichert empfiehlt es sich Komprimierung verwenden. Finden Sie SAP-Hinweis [1588316] Weitere Informationen. Komprimieren die Sicherung ist entscheidend zum Verringern der Datenmenge übertragen werden, wenn Sie beabsichtigen, Sicherungskopien oder virtuelle, enthält die Sicherungsdatei speichert aus Azure virtuellen Computers zu lokalen Festplatten herunterladen.

Verwenden Sie die Azure-virtuellen Computer temporärer Speicherplatz/mnt oder /mnt/resource nicht als Datenbank- oder Protokolldateien Abbild Ziel.

#### <a name="performance-considerations-for-backupsrestores"></a>Leistungsaspekte für Sicherungskopien/Wiederherstellung
Wie softwareunabhängige Bereitstellungen ist Sicherung und Wiederherstellung Leistung wie viele Datenmengen parallel gelesen werden können und was der Durchsatz die Datenmengen möglicherweise abhängig. Darüber hinaus kann die CPU-Auslastung durch Komprimierung verwendet eine wesentliche Rolle auf virtuellen Computern mit einfach bis zu 8 CPU-Threads wiedergeben. Daher kann eine darstellen:

* Weniger die Anzahl der virtuellen Festplatten verwendet wird, um die Geräte Datenbank speichern die kleinere Gesamtdurchsatz im Lesemodus.
* Je kleiner die Anzahl der CPU-Suchthreads, auf dem virtuellen Computer, je stärker den Einfluss der Komprimierung
* Die weniger Ziele (Linux-Software-RAID, virtuellen Festplatten), der Sicherung in den kleineren den Durchsatz zu schreiben

Klicken Sie zum Erhöhen der Anzahl der Ziele zu schreiben, dass es gibt zwei Optionen, die je nach Ihren Anforderungen verwendet/kombiniert werden können:

* Das Sicherungszielvolume Striping über mehrere bereitgestellten virtuellen Festplatten zur Verbesserung des IOPS-Durchsatzes auf dem aufgeteilten volume
* Erstellen einer Konfigurations des SAP-ASE Ebene die mehr als ein Zielverzeichnis wird verwendet, um das Abbild zu schreiben

Ein Volume über mehrere bereitgestellten virtuellen Festplatten Striping weist zuvor in diesem Handbuch beschrieben wurde. Weitere Informationen zur Verwendung mehrerer Verzeichnisse in die Konfiguration des SAP-ASE finden Sie in der Dokumentation auf gespeicherte Prozedur Sp_config_dump dem verwendet wird, um die Konfiguration der [Sybase InfoCenter für](http://infocenter.sybase.com/help/index.jsp)erstellen.

### <a name="disaster-recovery-with-azure-vms"></a>Wiederherstellung mit Azure-virtuellen Computern

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Datenreplikation mit SAP-Sybase Replikationsserver
Mit den SAP-Sybase Replikation Server (SRS) SAP-ASE bietet eine warme standby-Lösung Datenbanktransaktionen asynchrone an einem entfernten Standort übertragen. 

Installation und zum Betrieb von SRS funktioniert auch funktional in einen virtuellen Azure-virtuellen Computern Dienste gehostet wird, wie lokalen bedeutet.

ASE HADR über SAP-Replikationsserver wird zu diesem Zeitpunkt nicht unterstützt. Es möglicherweise mit getestet und in der Zukunft zur Microsoft Azure Plattformen freigegeben werden.

## <a name="specifics-to-oracle-database-on-windows"></a>Einzelheiten zu Oracle-Datenbank unter Windows
Seit des Jahrs 2013 wird die Oracle-Software von Oracle auf Microsoft Windows Hyper-V und Azure ausführen unterstützt. Lesen Sie diesen Artikel, um weitere Details zum allgemeinen Support von Windows Hyper-V und Azure Abrufen von Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Begonnen, die allgemeine Unterstützung wird das jeweiligen Szenario von SAP Anwendungen Nutzung von Oracle-Datenbanken ebenfalls unterstützt. Details werden in diesen Teil des Dokuments den Namen.

### <a name="oracle-version-support"></a>Unterstützung für Oracle-Version
Alle Details Oracle und entsprechende OS-Versionen, die unterstützt werden, für die Ausführung von SAP auf Oracle auf Azure virtuellen Computern finden Sie in den folgenden SAP-Hinweis [2039619]

Allgemeine Informationen zum Ausführen von SAP-Business-Suite auf Oracle finden Sie auf SCN: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle-Konfigurationsrichtlinien für SAP-Installationen in Azure-virtuellen Computern

#### <a name="storage-configuration"></a>Speicherkonfiguration
Nur Instanz, die mit NTFS formatierten Datenträger Oracle unterstützt wird. Basierend auf dem Datenträger virtuelle Festplatte NTFS Dateisystem müssen alle Datenbankdateien gespeichert werden. Diese virtuelle Festplatten zu den Azure-virtuellen Computer bereitgestellt sind und basieren auf Azure Seite BLOB-Speicher (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Beliebige Netzwerklaufwerke oder remote Freigaben wie Azure Dateidienste:
 
* <https://Blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://Blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/Persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
Oracle-Datenbankdateien werden **nicht** unterstützt!

Mithilfe von Azure-virtuellen Festplatten basierend auf Azure Seite BLOB-Speicher, anwenden die Aussagen in diesem Dokument in Kapitel [Zwischenspeichern für virtuellen Computern und virtuelle Festplatten] [Dbms Leitfaden 2.1] und [Microsoft Azure Storage] [Dbms Leitfaden 2.3] auf Bereitstellungen mit der Oracle-Datenbank

Wie weiter oben in der allgemeinen Teil des Dokuments erläutert, vorhanden sein, Kontingente für IOPS Durchsatz für Azure-virtuellen Festplatten. Die genauen Kontingente werden je nach Typ virtueller Computer verwendet. Eine Liste der Typen von virtuellen Computer mit ihrer Quote finden Sie [hier][virtual-machines-sizes]

Wenn Sie um die unterstützten Typen von Azure-virtuellen Computer zu identifizieren, Näheres SAP-Hinweis [1928533]

Solange das Kontingent für die aktuelle IOPS pro Datenträger die Anforderungen erfüllt, ist es möglich, die DB-Dateien auf einer einzelnen bereitgestellten Azure virtuellen Festplatte zu speichern. 

Wenn mehr IOPS erforderlich sind, wird es dringend empfohlen, die Fenster Speicherpools (nur in Windows Server 2012 verfügbar und höher) oder Windows-Striping für Windows 2008 R2 verwenden, um eine große logisches Gerät über mehrere bereitgestellten virtuelle Festplatte Datenträger zu erstellen. Siehe auch Kapitel [Software RAID] [Dbms Leitfaden 2.2] dieses Dokuments. Dieser Ansatz vereinfacht den Verwaltungsaufwand, um den Speicherplatz zu verwalten und vermieden werden der Leistung zum manuell Verteilen von Dateien auf mehreren bereitgestellten virtuellen Festplatten.

#### <a name="backup--restore"></a>Sichern / Wiederherstellen
Für die Sicherung / wiederherstellen von Funktionen, die SAP BR * Tools für Oracle werden auf die gleiche Weise wie auf standard Windows-Serverbetriebssysteme und Hyper-V unterstützt. Oracle Wiederherstellung Manager (RMAN) wird ebenfalls für Sicherungskopien aus, und Wiederherstellen von Datenträger unterstützt.

#### <a name="high-availability"></a>Hohe Verfügbarkeit
[Kommentar]: <>  (Link verweist auf ASM)
Hohe Verfügbarkeit und Ausfall wird Oracle Data Guard unterstützt. Details finden Sie in [dieser] [ virtual-machines-windows-classic-configure-oracle-data-guard] Dokumentation.

#### <a name="other"></a>Andere
Weitere allgemeinen Themen wie Azure Verfügbarkeit Sätze oder SAP-Überwachung anwenden, wie in diesem Dokument für die Bereitstellung von virtueller Computer mit der Oracle-Datenbank die ersten drei Kapitel beschrieben.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Einzelheiten für die SAP MaxDB-Datenbank unter Windows

### <a name="sap-maxdb-version-support"></a>SAP MaxDB Version Support
SAP aktuell unterstützt SAP MaxDB Version 7.9 zur Verwendung mit SAP NetWeaver basierenden Produkte in Azure. Alle Updates für SAP MaxDB Server oder JDBC und ODBC-Treiber mit SAP NetWeaver basierenden Produkte verwendet werden, werden allein durch SAP Service Marketplace am <https://support.sap.com/swdc>bereitgestellt.
Allgemeine Informationen über das Ausführen von SAP NetWeaver auf SAP MaxDB kann finden Sie unter <https://scn.sap.com/community/maxdb>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Unterstützte Versionen von Microsoft Windows und Azure-virtuellen Computer-Typen für SAP MaxDB DBMS
Unterstützte Microsoft Windows-Version für SAP MaxDB DBMS auf Azure, finden Sie unter:

* [SAP-Produkt Verfügbarkeit Matrix (PAM)][sap-pam]
* SAP-Hinweis [1928533]

Es wird dringend empfohlen, um die neueste Version des Betriebssystems Microsoft Windows, verwenden Sie die Microsoft Windows 2012 R2 ist.

### <a name="available-sap-maxdb-documentation"></a>Verfügbare SAP MaxDB Dokumentation
Sie können die aktualisierte Liste der SAP MaxDB Dokumentation in den folgenden SAP-Hinweis [767598] gefunden.
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Richtlinien für die Konfiguration von SAP MaxDB für SAP-Installationen in Azure-virtuellen Computern

#### <a name="a-nameb48cfe3b-48e9-4f5b-a783-1d29155bd573astorage-configuration"></a><a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Speicherkonfiguration
Azure-Speicher bewährte Methoden für SAP MaxDB führen Sie die allgemeinen Ratschläge erwähnt in Kapitel [Struktur einer Bereitstellung RDBMS] [Dbms Leitfaden 2].

> [AZURE.IMPORTANT] Wie andere Datenbanken gewissen SAP MaxDB ermöglicht Daten-und Protokolldateien. Es ist jedoch in SAP MaxDB Terminologie des richtigen Ausdrucks "Volumen" (nicht "Datei"). Es gibt beispielsweise SAP MaxDB Datenmengen und Log Datenmengen. Gehen Sie wie folgt nicht verwechselt werden diese mit OS Datenträger. 

Kurz gesagt müssen Sie:

* Einrichten des Kontos des Azure-Speicher, die die SAP MaxDB ermöglicht Daten- und Protokolldateien Datenmengen (d. h. Dateien) auf **Lokale redundante Speicher (LRS)** als in festgelegten Kapitel [Microsoft Azure Storage] [Dbms Leitfaden 2.3] enthält.
* Trennen Sie den Pfad EA für SAP MaxDB Datenmengen (d. h. Dateien) aus dem Pfad EA für Protokoll Datenmengen (d. h. Dateien). Dies bedeutet, dass SAP MaxDB Datenbestände (d. h. Dateien) auf einem logischen Laufwerk installiert sein und SAP MaxDB Log Datenmengen (d. h. Dateien) auf einem anderen logischen Laufwerk installiert werden müssen.
* Legen Sie die richtigen Datei Zwischenspeichern für jede Azure Blob, je nachdem, ob Sie es für SAP MaxDB ermöglicht Daten- oder Protokolldateien Datenträger (d. h. Dateien) verwenden, und gibt an, ob Sie Azure Standard oder Premium Azure-Speicher, wie in Kapitel [Zwischenspeichern für virtuelle Computer] beschrieben [Dbms Leitfaden 2.1].
* Solange das aktuelle IOPS Kontingent pro Datenträger die Anforderungen erfüllt, ist es möglich, speichern alle Datenbestände auf einer einzelnen bereitgestellten Azure-virtuellen und auch alle Datenbank Log Datenmengen auf einem anderen einzelnen bereitgestellten Azure virtuellen Festplatte speichern.
* Wenn mehr IOPS und/oder Leerzeichen erforderlich sind, wird es dringend empfohlen, die Microsoft-Fenster Speicherpools (nur in Microsoft Windows Server 2012 verfügbar und höher) oder Microsoft Windows Striping für Microsoft Windows 2008 R2 verwenden, um eine große logisches Gerät über mehrere bereitgestellten virtuelle Festplatte Datenträger zu erstellen. Siehe auch Kapitel [Software RAID] [Dbms Leitfaden 2.2] dieses Dokuments. Dieser Ansatz vereinfacht den Verwaltungsaufwand, um den Speicherplatz zu verwalten und vermieden werden der Leistung der manuellen Verteilen von Dateien über mehrere bereitgestellten virtuellen Festplatten.
* Die höchsten IOPS-Anforderung können Azure Premium Speicher, Sie die DS-Serie und GS-Serie virtuellen Computern verfügbar ist.

![Die Verweiskonfiguration von Azure Neuerung für SAP MaxDB DBMS][dbms-guide-figure-600]

#### <a name="a-name23c78d3b-ca5a-4e72-8a24-645d141a3f5dabackup-and-restore"></a><a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Sichern und Wiederherstellen
Wenn Sie SAP MaxDB in Azure bereitstellen zu können, müssen Sie Ihre Sicherung Methode lesen. Auch wenn das System kein System produktiv ist, muss die SAP-Datenbank von SAP MaxDB gehostet wird regelmäßig gesichert werden. Da Azure-Speicher drei Bilder gespeichert sind, ist eine Sicherung jetzt im Hinblick auf den Schutz Ihres Systems vor Speicher Fehler und weitere wichtige Betrieb oder administrative Fehler weniger wichtig. Der wichtigste Grund für die Verwaltung einer ordnungsgemäßen sichern und Wiederherstellungsplan ist, sodass Sie logische oder manuellen Fehlern durch Bereitstellung von Funktionen, die Wiederherstellung Zeitpunkt in kompensieren können. Damit das Ziel besteht darin, entweder Sicherungskopien zum Wiederherstellen der Datenbank zu einem bestimmten Punkt in Uhrzeit oder die Sicherungskopien in Azure zum Startwert für ein anderes System durch Kopieren die vorhandene Datenbank verwenden. Sie können beispielsweise Wiederherstellen einer Sicherung in einer 3-Ebenen-System-Setup des gleichen Systems aus einer 2-Ebenen SAP-Konfiguration übertragen.

Sichern und Wiederherstellen einer Datenbank in Azure funktioniert genauso wie für lokale Systeme, daher können Sie standard SAP MaxDB Sicherung und Wiederherstellung Tools verwenden, die in einem der SAP MaxDB Dokumentation Dokumente aufgeführt, die im SAP-Hinweis [767598]beschrieben werden. 

#### <a name="a-name77cd2fbb-307e-4cbf-a65f-745553f72d2caperformance-considerations-for-backup-and-restore"></a><a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Leistungsaspekte für Sicherung und Wiederherstellung
Wie softwareunabhängige Bereitstellungen ist sichern und Wiederherstellen Leistung abhängig, wie viele Datenmengen in Parallel und den Durchsatz die Datenmengen gelesen werden können. Darüber hinaus kann die CPU-Auslastung durch Komprimierung verwendet eine wesentliche Rolle auf virtuellen Computern mit bis zu 8 CPU-Threads wiedergeben. Daher kann eine darstellen:

* Weniger die Anzahl der virtuellen Festplatten verwendet, um die Datenbank-Geräte, je niedriger der Gesamtdurchsatz gelesen speichern
* Je kleiner die Anzahl der CPU-Suchthreads, auf dem virtuellen Computer, je stärker den Einfluss der Komprimierung
* Die weniger Ziele (Streifen Verzeichnisse durchsuchen, virtuellen Festplatten), der Sicherung, je niedriger den Durchsatz sowie das Schreiben in

Es gibt zwei Optionen zur Verfügung, die Sie möglicherweise bei gleichzeitiger, je nach Ihren Anforderungen verwenden, können, klicken Sie zum Erhöhen der Anzahl der Ziele sowie das Schreiben in:

* Separate Datenmengen gilt, für die Sicherung
* Das Sicherungszielvolume Striping über mehrere bereitgestellten virtuellen Festplatten zur Verbesserung des IOPS-Durchsatzes auf die aufgeteilten Datenträger
* Probleme separaten dedizierten logischen Datenträger Geräten:
    * SAP MaxDB Sicherung Datenmengen (d. h. Dateien)
    * SAP MaxDB Datenbestände (d. h. Dateien)
    * SAP MaxDB Log Datenmengen (d. h. Dateien)

Ein Volume über mehrere bereitgestellten virtuellen Festplatten Striping weist in Kapitel [Software RAID] [Dbms Leitfaden 2.2] dieses Dokuments beschrieben wurden. 

#### <a name="a-namef77c1436-9ad8-44fb-a331-8671342de818aother"></a><a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Andere
Weitere allgemeinen Themen wie Azure Verfügbarkeit Sätze oder SAP-Cmdlets für die Überwachung gelten auch wie in diesem Dokument aus, für die Bereitstellung von virtuellen Computern mit SAP MaxDB Datenbank die ersten drei Kapitel beschrieben.
Andere Einstellungen SAP MaxDB-spezifische sind transparent zu Azure-virtuellen Computern und in andere Dokumente aufgeführt in SAP-Hinweis [767598] und diese Notizen SAP-beschrieben:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Einzelheiten für SAP-LiveCache unter Windows

### <a name="sap-livecache-version-support"></a>SAP-LiveCache Version Support
Minimale Version des SAP-LiveCache unterstützt in Azure virtuellen Computern ist **SAP-LC/LCAPPS 10.0 SP 25** einschließlich **LiveCache 7.9.08.31** und **25 LCA-Generator**, freigegeben für **EhP 2 für SAP-SCM 7.0** oder höher.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Unterstützte Versionen von Microsoft Windows und Azure-virtuellen Computer-Typen für SAP-LiveCache DBMS
Unterstützte Microsoft Windows-Version für SAP-LiveCache auf Azure finden Sie unter:

* [SAP-Produkt Verfügbarkeit Matrix (PAM)][sap-pam]
* SAP-Hinweis [1928533]

Es wird dringend empfohlen, um die neueste Version des Betriebssystems Microsoft Windows, verwenden Sie die Microsoft Windows 2012 R2 ist. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP-LiveCache Konfigurationsrichtlinien für SAP-Installationen in Azure-virtuellen Computern

#### <a name="recommended-azure-vm-types"></a>Empfohlene Azure-virtuellen Computer-Typen
Wie LiveCache SAP-Anwendung, die riesige Berechnungen durchführt, weist die Stärke und die Geschwindigkeit von RAM und CPU einen Haupt-Einfluss auf die Leistung von SAP-LiveCache. 

Für die Azure-virtuellen Computer-Typen von SAP (SAP-Hinweis [1928533]) unterstützt werden alle virtuelle CPU-Ressourcen für den virtuellen Computer reserviert von dedizierten physische CPU-Ressourcen des Hypervisors unterstützt. Keine Bereitstellung überproportional vieler (und daher keine induzierten Risikos für die CPU-Ressourcen) erfolgt.

Auf ähnliche Weise ist für alle Azure-virtuellen Computer Instanz Typen von SAP unterstützt, Speicher virtueller Computer, dass das Arbeitsspeichers – Bereitstellung überproportional vieler (übermäßige Zusicherung), z. B. zugeordneten (100 %) nicht verwendet wird.

Aus dieser Perspektive ist es dringend empfohlen, verwenden Sie den neuen D-Serie oder DS-Serie (in Kombination mit Azure Premium Speicher) Azure-virtuellen Computer ein solange 60 % schnellere Prozessoren als die A-Serie ist. Der höchsten RAM und CPU-Auslastung können Sie G-Serie und GS-Serie (in Kombination mit Azure Premium Speicher) virtuellen Computern mit der neuesten Intel® Xeon® E5 v3 Prozessoren, die doppelte Speicherkapazität und viermal die einfarbige Zustand Laufwerk Speicherung (SSDs) D/DS-Serie haben.

#### <a name="storage-configuration"></a>Speicherkonfiguration
Wie SAP-LiveCache auf SAP MaxDB Technologie basiert, optimale alle Azure-Speicher Vorgehensweisen erwähnt für SAP MaxDB in Kapitel [Speicherkonfiguration] [Dbms Leitfaden 8.4.1] auch für SAP-LiveCache gültig sind. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Dedizierte Azure-virtuellen für LiveCache Computer
Wie SAP-LiveCache intensiv berechnete Power verwendet, wird für produktiv Verwendung empfohlen auf eine dedizierte Azure-virtuellen Computern bereitstellen. 
 
![Azure-virtuellen Computer für LiveCache für produktiv Anwendungsfall-dedizierten][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Sichern und Wiederherstellen
Sichern und wiederherstellen, einschließlich Leistungsfähigkeit sind bereits beschrieben, in die relevanten SAP MaxDB Kapitel [sichern und Wiederherstellen] [Dbms Leitfaden 8.4.2] und [Leistungsaspekte für Sicherung und Wiederherstellung] [Dbms Leitfaden 8.4.3]. 

#### <a name="other"></a>Andere
Alle anderen allgemeinen Themen werden in dem betreffenden SAP MaxDB [Dies] [Dbms Leitfaden 8.4.4] Kapitel bereits beschrieben. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Einzelheiten für den Inhalt SAP-Server auf Windows
Inhalt SAP-Server ist eine separate, serverbasierten Komponente Inhalt wie z. B. elektronische Dokumente in verschiedenen Formaten gespeichert. SAP-Content Server erfolgt über die Entwicklung von Technologie und verwendete Cross-Anwendung für alle SAP-Anwendungen werden soll. Es wird in einem gesonderten System installiert. Typische Inhalt ist Schulung Material- und Dokumentation von Knowledge Warehouse oder technischen Zeichnungen aus der MySAP PLM Dokument Management System. 

### <a name="sap-content-server-version-support"></a>Unterstützung für SAP-Content Server-Version
SAP unterstützt derzeit aus:

* **SAP-Content Server** mit Version **6.50 (und höher)**
* **SAP MaxDB Version 7.9**
* **Microsoft IIS (Internet Information Server) Version 8.0 (und höher)**

Es wird dringend empfohlen, die neueste Version von SAP-Content Server, die zum Zeitpunkt der Schreiben dieses Dokument **6.50 SP4**ist, und die neueste Version von **Microsoft IIS 8.5**verwenden. 

Überprüfen Sie die neueste unterstützten Versionen von SAP-Content Server und Microsoft IIS im [SAP-Produkt Verfügbarkeit Matrix (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Microsoft Windows und Azure-virtuellen Computer Datentypen für SAP-Content Server unterstützt
Unterstützte Windows-Version für SAP-Content Server auf Azure finden Sie finden Sie unter:

* [SAP-Produkt Verfügbarkeit Matrix (PAM)][sap-pam]
* SAP-Hinweis [1928533]

Es wird dringend empfohlen, um die neueste Version von Microsoft Windows zu verwenden, die zum Zeitpunkt des Schreiben dieses Dokument ist von **Windows Server 2012 R2**.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Richtlinien für die Konfiguration von SAP-Content Server für SAP-Installationen in Azure-virtuellen Computern

#### <a name="storage-configuration"></a>Speicherkonfiguration 
Wenn Sie mit SAP-Content Server zum Speichern von Dateien in der Datenbank SAP MaxDB konfigurieren, alle Azure-Speicher Methoden empfohlene erwähnt für SAP MaxDB in Kapitel [Speicherkonfiguration] [Dbms Leitfaden 8.4.1] gelten auch für das SAP-Content Server-Szenario. 

Wenn Sie zum Speichern von Dateien im Dateisystem Content SAP-Server konfigurieren, empfiehlt es sich ein dediziertes logisches Laufwerk verwenden. Mit Speicher Leerzeichen können Sie auch logische Datenträgergröße und IOPS-Durchsatz erhöhen, wie unter in Kapitel [Software RAID] [Dbms Leitfaden 2.2] beschrieben. 

#### <a name="sap-content-server-location"></a>SAP-Inhalt Serverspeicherorts
SAP-Content Server muss bereitgestellt werden, in der gleichen Azure Region und Azure VNET, wo das SAP-System bereitgestellt wird. Sie können entscheiden, ob Sie Content Server SAP-Komponenten eine dedizierte Azure-virtuellen Computers oder des gleichen virtuellen Computers, auf dem das SAP-System läuft, bereitstellen möchten. 
 
![Dedizierte Azure virtueller Computer für SAP-Content Server][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP-Cache Serverspeicherorts
Die SAP-Cache-Server ist eine zusätzliche Server-basierte Komponente auf lokal (zwischengespeicherte) Dokumente zugreifen können. Die SAP-Cache-Server speichert die Dokumente von einem SAP-Content-Server. Dies ist Netzwerkverkehr optimieren, wenn Dokumente mehr als einmal von anderen Speicherorten abgerufen werden müssen. In der Regel besteht darin, dass die SAP-Cache-Server physisch in der Nähe der Client sein, der Zugriff auf den SAP-Cache-Server. 

Hier haben Sie zwei Optionen:

1. **Client ist ein Back-End-SAP-system** Wenn eine Back-End-SAP-System Zugriff auf SAP-Content Server konfiguriert ist, ist das SAP-System ein Client. Wie in der gleichen Region Azure – im gleichen Datencenter Azure – SAP-System, und SAP-Content Server bereitgestellt werden sind sie physisch nahe. Es ist daher nicht erforderlich, dass ein dedizierter SAP-Cache-Server. SAP-UI-Clients (SAP-Benutzeroberfläche oder Web Browser) greifen direkt auf das SAP-System, und das SAP-System Dokumente vom SAP-Content Server abgerufen.
1. **Client ist einem lokalen browser** SAP-Content Server können direkt über den Webbrowser zugegriffen konfiguriert werden. In diesem Fall ist ein Webbrowser ausführen lokalen – ein Client von SAP-Content Server an. Lokale Datacenter und Azure Datacenter unterschiedlichen physischen Standorten (idealerweise nahe miteinander) befinden. Datencenters lokalen ist mit Azure über Azure Standort-zu-Standort VPN oder ExpressRoute verbunden. Obwohl beide Optionen sichere VPN-Netzwerk-Verbindung Azure anbieten möchten, bietet Website-zu-Standort-Verbindung kein Netzwerk Bandbreite und Wartezeit Vereinbarung zum SERVICELEVEL zwischen der lokalen Datacenter und Azure Datencenter. Beschleunigen von Zugriff auf Dokumente, können Sie eine der folgenden Aktionen ausführen:
    1. SAP-Cache-Server lokal installieren, und schließen, um die lokalen Browser (Option [this][dbms-guide-900-sap-cache-server-on-premises] Abbildung)
    1. Konfigurieren von Azure ExpressRoute, der eine schnelle und mit geringer Verzögerung dedizierte Verbindung zwischen lokalen Datacenter und Azure Datacenter bietet.
 
![Option zum Installieren von SAP-Cache-Server lokal][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Sichern / Wiederherstellen
Wenn Sie die SAP-Content Server zum Speichern von Dateien in der Datenbank SAP MaxDB konfigurieren, werden die Sicherung und Wiederherstellung Verfahren und Leistung Aspekte bereits beschrieben in SAP MaxDB Kapitel [sichern und Wiederherstellen] [Dbms Leitfaden 8.4.2] und Kapitel [Leistungsaspekte für Sicherung und Wiederherstellung] [Dbms Leitfaden 8.4.3]. 

Wenn Sie den Inhalt SAP-Server zum Speichern von Dateien im Dateisystem zu konfigurieren ist Möglichkeit eine manuelle Sicherung/wiederherstellen der gesamte Dateistruktur ausführen, in dem die Dokumente befinden. Ähnlich wie SAP MaxDB Sicherung und Wiederherstellung, empfiehlt es sich um einen dedizierten Datenträger Sicherung Zweck durchzuführen. 

#### <a name="other"></a>Andere
Andere bestimmten SAP-Content Server-Einstellungen sind für Azure-virtuellen Computern transparent und in verschiedene Dokumente und SAP-Notizen beschrieben:

* <https://Service.SAP.com/contentserver> 
* SAP-Hinweis [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Einzelheiten zu IBM DB2 LUW unter Windows
Mit Microsoft Azure können Sie einfach eine vorhandene SAP-Anwendung unter IBM DB2 für Linux, UNIX und Windows (LUW) auf Azure-virtuellen Computern migrieren. Mit SAP auf IBM DB2 für LUW können Administratoren und Entwickler weiterhin verwenden, die denselben Entwicklung und -Verwaltungstools die lokale verfügbar sind.
Allgemeine Informationen zum Ausführen von SAP-Business-Suite auf IBM DB2 für LUW finden Sie in der SAP-Community-Netzwerk (SCN) am <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Weitere Informationen und Updates zu SAP auf DB2 für LUW auf Azure finden Sie unter SAP-Hinweis [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 für Linux, UNIX und Windows-Version-Support
SAP auf IBM DB2 für LUW auf Microsoft Azure-virtuellen Computern Services ist ab dem DB2 Version 10.5 unterstützt.

Informationen zu unterstützten SAP-Produkte und Azure-virtuellen Computer Typen Näheres SAP-Hinweis [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 für Linux, UNIX und Windows-Konfigurationsrichtlinien für SAP-Installationen in Azure-virtuellen Computern

#### <a name="storage-configuration"></a>Speicherkonfiguration
Basierend auf dem Datenträger virtuelle Festplatte NTFS Dateisystem müssen alle Datenbankdateien gespeichert werden. Diese virtuelle Festplatten zu den Azure-virtuellen Computer bereitgestellt werden und in Azure Seite BLOB-Speicher (<https://msdn.microsoft.com/library/azure/ee691964.aspx>) basieren. Beliebige Netzwerklaufwerke oder remote Freigaben wie die folgenden Dateidienste Azure- **nicht** unterstützte Datenbankdateien sind: 

* <https://Blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://Blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/Persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
Wenn Sie Azure-virtuellen Festplatten basierend auf der Seite BLOB-Speicher Azure verwenden, die Aussagen in diesem Dokument in Kapitel [Struktur einer Bereitstellung RDBMS] [Dbms Leitfaden 2] auch auf nur eine Übersicht über die IBM DB2-Datenbank LUW angewendet. 

Wie weiter oben in der allgemeinen Teil des Dokuments erläutert, vorhanden sein, Kontingente für IOPS Durchsatz für Azure-virtuellen Festplatten. Die genauen Kontingente richtet sich nach dem virtuellen Computer verwendet. Eine Liste der Typen von virtuellen Computer mit ihrer Quote finden Sie [hier][virtual-machines-sizes].

Solange das Kontingent für die aktuelle IOPS pro Datenträger ausreichend ist, ist es möglich, alle Datenbankdateien auf einem einzelnen bereitgestellten Azure virtuellen Festplatte gespeichert werden. 

Für Performance finden Aspekte auch Kapitel "Daten Sicherheit und Leistung Aspekte für Datenbank Verzeichnissen" im SAP-Installation Führungslinien.

Sie können auch können Windows Speicherpools (nur in Windows Server 2012 verfügbar und höher) oder Windows-Striping für Windows 2008 R2 Sie wie im Kapitel [Software RAID] [Dbms Leitfaden 2.2] dieses Dokuments beschrieben um eine großes logisches Gerät über mehrere bereitgestellten virtuelle Festplatte Datenträger zu erstellen.
Für Datenträger, der die DB2-Speicherpfade für Ihre SAP-Daten und Saptmp Verzeichnisse enthält müssen Sie eine physische Festplatte Sector Größe 512 KB angeben. Wenn Windows Speicherpools verwenden zu können, müssen Sie die Speicherpools manuell über den Befehl Linie Benutzeroberfläche mit dem Parameter erstellen "-LogicalSectorSizeDefault". Weitere Informationen finden Sie unter <https://technet.microsoft.com/library/hh848689.aspx>.

#### <a name="backuprestore"></a>Sicherung und Wiederherstellung
Die Sicherung und Wiederherstellung Funktionalität für IBM DB2 für LUW wird auf die gleiche Weise wie auf standard Windows-Serverbetriebssysteme und Hyper-V unterstützt.

Sie müssen sicherstellen, dass Sie eine gültige Datenbank Sicherung Strategie angeordnet haben. 

Sicherung und Wiederherstellung Leistung hängt wie softwareunabhängige Bereitstellungen aus wie viele Datenmengen parallel gelesen werden können und was der Durchsatz die Datenmengen möglicherweise. Darüber hinaus kann die CPU-Auslastung durch Komprimierung verwendet eine wesentliche Rolle auf virtuellen Computern mit einfach bis zu 8 CPU-Threads wiedergeben. Daher kann eine darstellen:

* Weniger die Anzahl der virtuellen Festplatten verwendet wird, um die Geräte Datenbank speichern die kleinere Gesamtdurchsatz im Lesemodus.
* Je kleiner die Anzahl der CPU-Suchthreads, auf dem virtuellen Computer, je stärker den Einfluss der Komprimierung
* Weniger Ziele (Streifen Verzeichnisse durchsuchen, virtuellen Festplatten), der Sicherung, je niedriger den Durchsatz sowie das Schreiben in

Klicken Sie zum Erhöhen der Anzahl der Ziele sowie das Schreiben in zwei Optionen je nach Ihren Anforderungen verwendet/kombiniert sind möglich:

* Das Sicherungszielvolume Striping über mehrere bereitgestellten virtuellen Festplatten zur Verbesserung des IOPS-Durchsatzes auf dem aufgeteilten volume
* Verwenden mehr als ein Zielverzeichnis die Sicherung zu schreiben

#### <a name="high-availability-and-disaster-recovery"></a>Hohe Verfügbarkeit und Wiederherstellung
Microsoft Cluster Server-(MSCS-)Clusters wird nicht unterstützt.

DB2 hohen Verfügbarkeit Wiederherstellung (HADR) wird unterstützt. Den virtuellen Computern der HA-Konfiguration über das Arbeiten mit einer namensauflösung von verfügen, wird die Einrichtung in Azure nicht aus jedem Setup unterscheiden, die lokal ausgeführt wird. Es empfiehlt sich nicht auf IP-Auflösung nur verlassen.

Verwenden Sie keine Azure Store Geo-Replikation aus. Weitere Informationen finden Sie in Kapitel [Microsoft Azure Storage] [Dbms Leitfaden 2.3] und Kapitel [hohe Verfügbarkeit und Wiederherstellung mit Azure-virtuellen Computern] [Dbms Leitfaden 3].

#### <a name="other"></a>Andere
Weitere allgemeinen Themen wie Azure Verfügbarkeit Sätze oder SAP-Überwachung anwenden, wie in diesem Dokument für die Bereitstellung von virtueller Computer mit IBM DB2 für LUW, die auch die ersten drei Kapitel beschrieben. 

Finden Sie auch Kapitel [allgemeiner SQL Server für SAP auf Azure Zusammenfassung] [Dbms Leitfaden 5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Einzelheiten zu IBM DB2 LUW unter Linux
Mit Microsoft Azure können Sie einfach eine vorhandene SAP-Anwendung unter IBM DB2 für Linux, UNIX und Windows (LUW) auf Azure-virtuellen Computern migrieren. Mit SAP auf IBM DB2 für LUW können Administratoren und Entwickler weiterhin verwenden, die denselben Entwicklung und -Verwaltungstools die lokale verfügbar sind. Allgemeine Informationen zum Ausführen von SAP-Business-Suite auf IBM DB2 für LUW finden Sie in der SAP-Community-Netzwerk (SCN) am <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Weitere Informationen und Updates zu SAP auf DB2 für LUW auf Azure finden Sie unter SAP-Hinweis [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 für Linux, UNIX und Windows-Version-Support
SAP auf IBM DB2 für LUW auf Microsoft Azure-virtuellen Computern Services wird ab DB2 Version 10.5 unterstützt.

Informationen zu unterstützten SAP-Produkte und Azure-virtuellen Computer Typen Näheres SAP-Hinweis [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 für Linux, UNIX und Windows-Konfigurationsrichtlinien für SAP-Installationen in Azure-virtuellen Computern

#### <a name="storage-configuration"></a>Speicherkonfiguration
Auf einem Datenträger virtuelle Festplatte Grundlage Dateisystem müssen alle Datenbankdateien gespeichert werden. Diese virtuelle Festplatten zu den Azure-virtuellen Computer bereitgestellt werden und in Azure Seite BLOB-Speicher (<https://msdn.microsoft.com/library/azure/ee691964.aspx>) basieren.
Beliebige Netzwerklaufwerke oder remote Freigaben wie die folgenden Dateidienste Azure- **nicht** unterstützte Datenbankdateien sind:

* <https://Blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://Blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/Persisting-Connections-to-Microsoft-Azure-Files.aspx>

Wenn Sie Azure-virtuellen Festplatten basierend auf der Seite BLOB-Speicher Azure verwenden, die Aussagen in diesem Dokument in Kapitel [Struktur einer Bereitstellung RDBMS] [Dbms Leitfaden 2] auch auf nur eine Übersicht über die IBM DB2-Datenbank LUW angewendet.

Wie weiter oben in der allgemeinen Teil des Dokuments erläutert, vorhanden sein, Kontingente für IOPS Durchsatz für Azure-virtuellen Festplatten. Die genauen Kontingente richtet sich nach dem virtuellen Computer verwendet. Eine Liste der Typen von virtuellen Computer mit ihrer Quote finden Sie [hier][virtual-machines-sizes].

Solange das Kontingent für die aktuelle IOPS pro Datenträger ausreichend ist, ist es möglich, alle Datenbankdateien auf einem einzelnen bereitgestellten Azure virtuellen Festplatte gespeichert werden.

Für Performance finden Aspekte auch Kapitel "Daten Sicherheit und Leistung Aspekte für Datenbank Verzeichnissen" im SAP-Installation Führungslinien.

Sie können auch können LVM (logische Volume Manager) oder MDADM Sie wie im Kapitel [Software RAID] [Dbms Leitfaden 2.2] dieses Dokuments beschrieben um eine großes logisches Gerät über mehrere bereitgestellten virtuelle Festplatte Datenträger zu erstellen.
Für Datenträger, der die DB2-Speicherpfade für Ihre SAP-Daten und Saptmp Verzeichnisse enthält müssen Sie eine physische Festplatte Sector Größe 512 KB angeben.

#### <a name="backuprestore"></a>Sicherung und Wiederherstellung
Die Sicherung und Wiederherstellung Funktionalität für IBM DB2 für LUW wird auf die gleiche Weise wie standard Linux Installation lokal auf unterstützt.

Sie müssen sicherstellen, steht Ihnen eine gültige Datenbank Sicherung Strategie angeordnet.

Sicherung und Wiederherstellung Leistung hängt wie softwareunabhängige Bereitstellungen aus wie viele Datenmengen parallel gelesen werden können und was der Durchsatz die Datenmengen möglicherweise. Darüber hinaus kann die CPU-Auslastung durch Komprimierung verwendet eine wesentliche Rolle auf virtuellen Computern mit einfach bis zu 8 CPU-Threads wiedergeben. Daher kann eine darstellen:

* Weniger die Anzahl der virtuellen Festplatten verwendet wird, um die Geräte Datenbank speichern die kleinere Gesamtdurchsatz im Lesemodus.
* Je kleiner die Anzahl der CPU-Suchthreads, auf dem virtuellen Computer, je stärker den Einfluss der Komprimierung
* Die weniger Ziele (Streifen Verzeichnisse durchsuchen, virtuellen Festplatten), der Sicherung, je niedriger den Durchsatz sowie das Schreiben in

Klicken Sie zum Erhöhen der Anzahl der Ziele sowie das Schreiben in zwei Optionen je nach Ihren Anforderungen verwendet/kombiniert sind möglich:

* Das Sicherungszielvolume Striping über mehrere bereitgestellten virtuellen Festplatten zur Verbesserung des IOPS-Durchsatzes auf dem aufgeteilten volume
* Verwenden mehr als ein Zielverzeichnis die Sicherung zu schreiben

#### <a name="high-availability-and-disaster-recovery"></a>Hohe Verfügbarkeit und Wiederherstellung
DB2 hohen Verfügbarkeit Wiederherstellung (HADR) wird unterstützt. Den virtuellen Computern der HA-Konfiguration über das Arbeiten mit einer namensauflösung von verfügen, wird die Einrichtung in Azure nicht aus jedem Setup unterscheiden, die lokal ausgeführt wird. Es empfiehlt sich nicht auf IP-Auflösung nur verlassen.

Verwenden Sie keine Azure Store Geo-Replikation aus. Weitere Informationen finden Sie in Kapitel [Microsoft Azure Storage] [Dbms Leitfaden 2.3] und Kapitel [hohe Verfügbarkeit und Wiederherstellung mit Azure-virtuellen Computern] [Dbms Leitfaden 3].

#### <a name="other"></a>Andere
Weitere allgemeinen Themen wie Azure Verfügbarkeit Sätze oder SAP-Überwachung anwenden, wie in diesem Dokument für die Bereitstellung von virtueller Computer mit IBM DB2 für LUW, die auch die ersten drei Kapitel beschrieben.

Finden Sie auch Kapitel [allgemeiner SQL Server für SAP auf Azure Zusammenfassung] [Dbms Leitfaden 5.8].