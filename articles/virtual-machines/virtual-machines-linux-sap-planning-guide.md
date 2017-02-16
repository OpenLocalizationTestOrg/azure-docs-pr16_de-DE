<properties
   pageTitle="SAP NetWeaver auf Linux virtuellen Computern (virtuellen Computern) – Planung und Implementierung Führungslinie | Microsoft Azure"
   description="SAP NetWeaver auf Linux virtuellen Computern (virtuellen Computern) – Planung und Implementierung Führungslinien"
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

# <a name="sap-netweaver-on-azure-virtual-machines-vms--planning-and-implementation-guide"></a>SAP NetWeaver auf Azure-virtuellen Computern (virtuellen Computern) – Planung und Implementierung Führungslinien

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
[virtual-machines-windows-capture-image]:virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:virtual-machines-windows-generalize-vhd.md
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

Microsoft Azure ermöglicht Unternehmen Datenverarbeitung und Speicher-Ressourcen in Formularlayouts ohne lange Aufträge Zyklen abrufen. Azure-virtuellen Computern zulassen Unternehmen klassische Anwendungen bereitgestellt, wie SAP NetWeaver Applications in Azure basierend und erweitern ihre Zuverlässigkeit und Verfügbarkeit ohne weitere Ressourcen zur Verfügung lokalen. Azure-virtuellen Computern-Services unterstützt auch Cross lokale Konnektivität, womit Unternehmen aktiv Azure-virtuellen Computern in ihren lokalen Domänen, deren Wolken "Privat" und ihre SAP-System Querformat integrieren kann.
In diesem Whitepaper werden die Grundlagen der Microsoft Azure virtuellen Computern und bietet ein schrittweises Abarbeiten der Planung und Implementierung Aspekte für SAP NetWeaver Installationen in Azure und als solche sollte das Dokument lesen Sie vor der Bereitstellung von SAP NetWeaver auf Azure gestartet werden.
Das Papier ergänzen die SAP-Installationsdokumentation und SAP-Notizen, die die primären Ressourcen für Installationen und -Bereitstellungen mit SAP-Software auf bestimmten Plattformen darstellen.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="summary"></a>Zusammenfassung
Cloud Computing eines häufig verwendeten Ausdrucks ist, das mehr Wichtigkeit innerhalb der IT-Branche von kleinen Unternehmen bis zu groß und internationalen Unternehmen hervorruft.

Microsoft Azure ist die Cloud Services-Plattform von Microsoft seiner ein breites Spektrum der neuen mögliche Werte. Jetzt können Kunden schnell bereitstellen und Freigabe Bereitstellen von Applications als Dienst in der Cloud, damit sie nicht auf technische oder Budgetierung Einschränkungen beschränkt sind. Statt eine Zeit und Ihr Budget in Hardware-Infrastruktur, können Unternehmen über die Anwendung, von Geschäftsprozessen und deren Vorteile für Kunden und Benutzer konzentrieren.

Mit Microsoft Azure-virtuellen Computern Diensten bietet Microsoft eine umfassende Infrastruktur als Plattform Service (IaaS) an. SAP NetWeaver-basierten Anwendungen werden auf Azure virtuellen Computern (IaaS) unterstützt werden. Dieses Whitepaper wird beschrieben, wie Planung und Implementierung von SAP NetWeaver-basierten Anwendungen in Microsoft Azure als Plattform der Wahl.

Das Papier selbst liegt der Schwerpunkt auf zwei wesentliche Aspekte:

* Im erste Teil wird in zwei unterstützte Bereitstellungsmuster für Applikationen SAP NetWeaver basierend auf Azure beschrieben. Außerdem wird Allgemeine Behandlung von Azure mit SAP-Bereitstellungen Punkte beschrieben.
* Die zweite Komponente wird ausführlich Implementieren der zwei verschiedenen Szenarien, die im ersten Teil beschrieben.

Zusätzliche Ressourcen finden Sie unter Kapitel [Ressourcen] [planning-Leitfaden-1.2] in diesem Dokument.

### <a name="definitions-upfront"></a>Definitionen vorab
Im gesamten Dokument verwenden wir die folgenden Begriffe:

* IaaS: Infrastructure as a Service.
* PaaS: Plattform als Dienst.
* SaaS: Software als Dienst.
* Cloud: Azure Ressourcenmanager
* SAP-Komponente: eine einzelne SAP-Anwendung wie ECC, BW, Lösung Manager oder EP.  SAP-Komponenten können auf herkömmliche ABAP oder Java-Technologien oder eine Anwendung nicht NetWeaver-basierten wie Business Objects basieren.
* SAP-Umgebung: eine oder mehrere SAP-Komponenten gruppiert logisch, wie etwa Entwicklung, QAS, Schulung, DR oder Herstellung eine Geschäftsfunktion auszuführen.
* SAP-Querformat: Dies bezieht sich auf die gesamten SAP-Anlagen in eines Kunden IT Querformat. Die SAP-Querformat umfasst alle Herstellung und nicht Herstellung Umgebungen.
* SAP-System: Die Kombination von DBMS Layer und Anwendung Layer z. B. SAP-ERP-Entwicklung-System, BW SAP-Test-System, SAP-CRM Herstellung System usw... In Azure-Bereitstellungen ist es nicht unterstützt, um diese beiden Ebenen zwischen lokalen und Azure dividieren. Dies bedeutet, die entweder ein SAP-System ist bereitgestellt lokalen, oder sie die in Azure bereitgestellt wird. Sie können jedoch die unterschiedlichen Systemen von einer SAP-Querformat in Azure oder lokalen bereitstellen. Beispielsweise konnte Sie die SAP-CRM Entwicklung bereitstellen und Systeme in Azure jedoch die SAP-CRM Herstellung System lokal testen.
* Nur Cloud-Bereitstellung: eine Bereitstellung, in dem das Azure-Abonnement nicht über eine Website-zu-Standort oder ExpressRoute Verbindung Infrastruktur des lokalen verbunden ist. Gemeinsam sind Azure Dokumentation folgenden Arten von Bereitstellungen auch als 'Nur Cloud'-Bereitstellungen beschrieben. Virtuellen Computern befinden, die bei dieser Methode können über das Internet und eine öffentliche IP-Adresse und/oder einen öffentlichen DNS-Namen mit den virtuellen Computern in Azure zugewiesen zugegriffen werden. Für die Microsoft Windows lokalen Active Directory (AD) und DNS ist nicht in Azure in diesen Abfragearten Bereitstellungen erweitert. Die virtuellen Computern sind daher nicht Teil der lokalen Active Directory. Dasselbe gilt für Linux Implementierungen z. B. OpenLDAP + Kerberos verwenden.

> [AZURE.NOTE] Nur Cloud-Bereitstellungen in diesem Dokument wird definiert, wie ausschließlich in Azure ohne Erweiterung von Active Directory abgeschlossen SAP-Landschaften ausgeführt werden / OpenLDAP oder mit einer namensauflösung von aus lokalen in öffentlichen Cloud. Nur Cloud-Konfigurationen werden nicht unterstützt, für die genutzten SAP- oder Konfigurationen, in denen SAP-STMS oder andere Ressourcen lokal zwischen SAP-Systemen auf Azure und Ressourcen lokal gehosteten verwendet werden müssen.

* Cross lokale: Beschreibt ein Szenario, in dem virtuellen Computern ein Azure-Abonnement bereitgestellt werden, die zwischen Standorten, mit mehreren Standorten oder ExpressRoute Konnektivität zwischen dem lokalen Datacenter(s) und Azure aufweist. Gemeinsam Azure-Dokumentation die folgenden Arten von Bereitstellungen werden auch als Cross lokale Szenarien beschrieben. Der Grund für die Verbindung besteht darin, lokale Domänen erweitern lokalen Active Directory / OpenLDAP und lokale DNS-Einträge in Azure. Die lokale Querformat erweiterten den Azure Anlagen des Abonnements. Probleme dieser Erweiterung an, können die virtuellen Computern Teil der lokalen Domäne sein. Der lokalen Domäne Domänen-Benutzer können auf die Server zugreifen und Dienste auf diese virtuellen Computern (wie DBMS Services) ausgeführt werden können. Kommunikation und Namen Auflösung zwischen virtuellen Computern bereitgestellt lokalen und Azure bereitgestellten virtuellen Computern ist möglich. Dies ist das Szenario, das wir erwarten, dass die meisten SAP-Elemente im bereitgestellt werden.  Finden Sie unter [dieser] [ vpn-gateway-cross-premises-options] Artikel und [dieser] [ vpn-gateway-site-to-site-create] Weitere Informationen.

> [AZURE.NOTE] Cross lokale Bereitstellungen von SAP-Systeme, wo Azure virtuellen Computern ausgeführt werden SAP-Systemen Mitglieder einer lokalen Domäne sind, werden für die Herstellung SAP-Betriebssystemen unterstützt. Cross lokale Konfigurationen werden unterstützt, für die Bereitstellung von Webparts oder SAP-Landschaften in Azure abzuschließen. Auch die vollständige SAP-Querformat in Azure ausgeführt erfordert Probleme die virtuellen Computern, Teil der lokalen Domäne und Werbung / OpenLDAP. In früheren Versionen der Dokumentation erörtert Hybrid-IT-Szenarien, wo der Begriff 'Hybrid' in der Faktentabelle Stamm ist, dass ein Kreuz lokale Konnektivität zwischen lokalen und Azure vorhanden ist. Außerdem verlangt, dass die virtuellen Computern in Azure Teil der lokalen Active Directory sind / OpenLDAP.

Einige Microsoft-Dokumentation beschreibt Cross lokale Szenarien etwas anders, vor allem für DBMS HA Konfigurationen. Wenn die SAP verwandte Dokumente, die Cross lokale Szenario nur im Wesentlichen einer Standort-zu-Standort oder privat (ExpressRoute)-Konnektivität und der Fakultät, dass die SAP-Querformat zwischen lokalen und Azure verteilt werden müssen.  

### <a name="a-namee55d1e22-c2c8-460b-9897-64622a34fdffaresources"></a><a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Ressourcen
Die folgenden zusätzlichen Handbücher sind für das Thema der SAP-Bereitstellungen auf Azure verfügbar:

* [SAP NetWeaver auf Azure-virtuellen Computern (virtuellen Computern) – Planung und Implementierung Führungslinie (dieses Dokument)] [Planungshandbuch]
* [SAP NetWeaver auf Azure-virtuellen Computern (virtuellen Computern) – Bereitstellungshandbuch] [Bereitstellungshandbuch]
* [SAP NetWeaver auf Azure-virtuellen Computern (virtuellen Computern) – Bereitstellungshandbuch DBMS] [Dbms-Leitfadens]

> [AZURE.IMPORTANT] Wo möglich einen Link zu der verweisende SAP-Installation Guide verwendet wird (Bezug InstGuide-01, finden Sie unter <http://service.sap.com/instguides>). Wenn es sich um die erforderlichen Komponenten und den Installationsvorgang geht, sollte die SAP NetWeaver Installation Führungslinien immer sorgfältig, gelesen werden, wie dieses Dokument nur bestimmte Vorgänge für SAP NetWeaver Systeme, die in einem Microsoft Azure virtuellen Computer installiert behandelt.

Die folgenden SAP-Hinweise beziehen sich auf das Thema der SAP auf Azure:

| Anzahl der Notiz | Titel |
|--------------|-------|
| [1928533] | SAP-Anwendungen auf Azure: unterstützte Produkte und anpassen |
| [2015553] | Klicken Sie auf Microsoft Azure SAP: Vorkenntnisse unterstützen |
| [1999351] | Problembehandlung bei erweiterten Azure Überwachung für SAP |
| [2178632] | Kennzahlen für SAP auf Microsoft Azure Überwachung Schlüssel |
| [1409604] | Virtualisierung unter Windows: Erweiterte Überwachung |
| [2191498] | SAP auf Linux mit Azure: Erweiterte Überwachung
| [2243692] | Klicken Sie auf Microsoft Azure (IaaS) virtueller Computer Linux: SAP-Lizenz Probleme
| [1984787] | SUSE LINUX Enterprise Server 12: Installation von Notizen
| [2002167] | Red Hat Enterprise Linux 7.x: Installation und Upgrade

Sie können auch nachlesen [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) enthält, die alle SAP-Notizen für Linux.

In [diesem Artikel] können allgemeine standardmäßige Einschränkungen und maximale Schwächen Azure-Abonnements gefunden werden[azure-subscription-service-limits-subscription] 

## <a name="possible-scenarios"></a>Mögliche Szenarien
SAP wird häufig als eine, die die wichtigste Komponenten von Unternehmen angesehen. Der Architektur und Funktionen von diesen Anwendungen ist hauptsächlich sehr komplex und wichtige ist sicherzustellen, dass Sie auf Verfügbarkeit und Performance erfüllen.

Daher müssen Unternehmen denken sorgfältig darüber, welche Anwendungen in einer öffentlichen Cloud-Umgebung, unabhängig von den ausgewählten Cloud-Anbieter ausgeführt werden können.

Systemtypen von möglichen für die Bereitstellung von SAP NetWeaver Grundlage Clientanwendungen im öffentlichen Cloud Umgebungen unten aufgeführten sind:

1. Mittlerer Größe genutzten
1. Entwicklung Betriebssysteme
1. Testen Systeme
1. Prototypen Betriebssysteme
1. Learning / Demo-Betriebssysteme

Um erfolgreich SAP-Systemen in Azure IaaS oder IaaS im Allgemeinen bereitzustellen, ist es wichtig, die wesentlichen Unterschiede zwischen den Angeboten traditionelle Outsourcern oder Hoster und IaaS Angebote zu verstehen. Während der traditionelle Hoster oder Outsourcer Infrastruktur (Netzwerk, Speicher und Typ), um die Arbeitsbelastung, die ein Kunde hosten möchte anpassen wird, ist es stattdessen der Kunde muss die richtige Arbeitsbelastung für IaaS Bereitstellungen auswählen.

Als ersten Schritt müssen Kunden überprüfen Sie die folgenden Elemente:

* Die SAP unterstützt virtueller Computer Typen von Azure
* Die SAP unterstützt-Produkte/Versionen auf Azure
* Unterstützte Betriebssystem und DBMS frei, für die bestimmten SAP-Versionen in Azure
* SAPS Durchsatz von verschiedenen Azure SKUs bereitgestellt

Die Beantwortung dieser Fragen können im SAP-Hinweis [1928533]gelesen werden. 

Als zweiten Schritt müssen Azure Ressourcen- und Bandbreite Einschränkungen mit tatsächlichen Ressourcenverbrauch lokale Systeme verglichen werden. Daher müssen Kunden die verschiedenen Funktionen der Azure mit SAP im Bereich der unterstützten Typen vertraut sein:

* CPU- und Ressourcen der verschiedenen Arten von virtuellen Computer und
* IOPS Bandbreite von verschiedenen Arten von virtuellen Computer und 
* Netzwerkfunktionen, die verschiedene Typen von virtuellen Computer.

Die meisten der Daten finden Sie [hier][virtual-machines-sizes]

Denken Sie daran, die die Grenzwerte in den Link oben aufgelistet sind Grenzwerte. Es bedeutet nicht, dass die Grenzwerte für eine Ressource, z. B. IOPS unter allen Umständen bereitgestellt werden können. Eine Ausnahme bilden jedoch die CPU- und Ressourcen eines ausgewählten virtuellen Computer Typs. Für die virtuellen Computer-Typen von SAP unterstützt werden sind die CPU- und Ressourcen reservierte und als solche verfügbar zu einem beliebigen Zeitpunkt Zeitpunkt für die Ernährung in den virtuellen Computer an.

Die Microsoft Azure-Plattform wie andere IaaS Plattformen ist eine Plattform mit mehreren Mandanten. Dies bedeutet, dass zwischen Mandanten, Netzwerk- und anderen Ressourcen freigegeben wurden. Intelligente begrenzungsebene und Kontingent Logik wird verwendet, um zu verhindern, dass eine Mandanten beeinträchtigen die Leistung von einem anderen Mandanten (laut benachbarten) auf eine Weise schwerwiegenden. Obwohl versucht Logik in Azure zu bleiben, die Varianzen in Bandbreite erfahrener kleine, hochgradig freigegebenen Plattformen sind in der Regel größere Varianzen in Ressource/Bandbreite Verfügbarkeit als zahlreiche Kunden vorstellen werden verwendet, um in ihre lokale Bereitstellungen. Daher können Sie verschiedene Detailebenen Bandbreite in Bezug auf die Netzwerke oder Speicher von e/a (die Lautstärke als auch Wartezeit) Minute um Minute auftreten. Die Wahrscheinlichkeit, dass ein SAP-System auf Azure größere Varianzen als in einem lokalen System auftreten kann muss berücksichtigt werden.

Letzter Schritt ist für Verfügbarkeit Anforderungen ausgewertet werden soll. Es kann vorkommen, dass die zugrunde liegende Azure Infrastruktur muss aktualisiert wird, und die Hosts Betrieb virtueller Computer neu gestartet werden. In diesen Fällen würde virtuellen Computern ausgeführt auf diesen Hosts beenden und neu gestartet ebenfalls. Die Anzeigedauer solche Wartung nicht Core üblichen Geschäftszeiten für eine bestimmte Region abgeschlossen ist, aber das potenzielle Fenster von ein Paar, das Stunden, in denen ein Neustart ausgeführt wird ist relativ breit. Es gibt verschiedene Technologien innerhalb der Azure-Plattform, die konfiguriert werden können, um einige oder alle den Einfluss der solche Updates zu verringern. Zukünftiger Verbesserungen der Azure-Plattform DBMS und SAP-Anwendung sind so gestaltet, minimieren den Einfluss der solche neu gestartet wurde. 

Um einen SAP-System auf Azure bereitstellen zu können, klicken Sie auf die lokale Systeme Betriebssystem Datenbank SAP und Azure-Infrastruktur kann innerhalb der Ressourcen bieten passt SAP Anwendungen müssen in der SAP-Azure-Support-Matrix angezeigt werden und die mit der Verfügbarkeit von SLAs Microsoft Azure bietet arbeiten können. Wie diese Systeme identifiziert werden, müssen Sie entscheiden, klicken Sie auf eine der folgenden beiden Szenarien.

### <a name="a-name1625df66-4cc6-4d60-9202-de8a0b77f803acloud-only---virtual-machine-deployments-into-azure-without-dependencies-on-the-on-premises-customer-network"></a><a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Nur Cloud - virtuellen Computern Bereitstellungen in Azure ohne Abhängigkeiten im lokalen Kunden-Netzwerk
 
![Einzelne virtueller Computer mit SAP-Demo oder Schulung Szenario in Azure bereitgestellt][planning-guide-figure-100]

Dieses Szenario ist für Training oder Demonstrationssysteme, typische, in dem alle Komponenten von SAP- und nicht-SAP-Software innerhalb eines einzelnen virtuellen Computers installiert werden. Herstellung SAP-Systeme werden in diesem Bereitstellungsszenario nicht unterstützt. Dieses Szenario wird im Allgemeinen die folgenden Anforderungen erfüllt:

* Die virtuellen Computern selbst werden über das öffentliche Netzwerk zugegriffen. Direkte Netzwerkkonnektivität innerhalb der virtuellen Computern ausgeführt werden, mit dem lokalen Netzwerk des Unternehmens entweder Besitzer des Inhalts Demos oder Training oder den Kunden Anwendungsmöglichkeiten ist nicht erforderlich. 
* Bei mehreren virtuellen Computern, die die Training oder Demoszenario darstellt muss Netzwerk Kommunikation und Namen Auflösung zwischen den virtuellen Computern arbeiten. Kommunikation zwischen den Satz von virtuellen Computern isoliert werden, damit mehrere Sätze von virtuellen Computern nebeneinander störungsfrei bereitgestellt werden können, müssen Sie jedoch.  
* Verbindung mit dem Internet ist erforderlich für den Endbenutzer auf remote Login in mit den virtuellen Computern in Azure gehostet wird. Je nach den Gast OS, Terminal Services/RDS oder VNC/ssh dient zum Zugriff auf den virtuellen Computer, um die Schulung Aufgaben erfüllen oder die Demos ausführen. Wenn beispielsweise 3200, 3300 und 3600 können SAP-ports auch verfügbar gemacht werden, die Instanz der SAP-Anwendung von jedem Internet verbundenen Desktop zugegriffen werden kann.
* Das SAP-Systeme (und VM(s)) darstellen einer eigenständigen Szenario in Azure nur public Internet Connectivity für den Endbenutzerzugriff erfordert und eine Verbindung mit anderen virtuellen Computern in Azure sind nicht erforderlich.
* SAPGUI und einem Browser installiert und direkt auf dem virtuellen Computer ausgeführt werden. 
* Eine schnelle Zurücksetzen eines virtuellen Computers in seinem ursprünglichen Zustand und neue Bereitstellung von die ursprünglichen Zustand erneut ist erforderlich. 
* Wenn Demo und Schulung Szenarien, die in mehrere virtuelle Computer, ein lokales Active Directory realisiert werden / OpenLDAP und/oder DNS-Dienst ist für jeden Satz von virtuellen Computern erforderlich.


![Gruppe des virtuellen Computers, eine Demo oder Schulung Szenario in einen Cloud-Dienst Azure darstellt.][planning-guide-figure-200]

Es ist wichtig zu beachten, die die VM(s) in jeder der Datensätze müssen parallel, bereitgestellt werden, in dem die Namen virtueller Computer in jedem der festlegen identisch sind.

### <a name="a-namef5b3b18c-302c-4bd8-9ab2-c388f1ab3d10across-premise---deployment-of-single-or-multiple-sap-vms-into-azure-with-the-requirement-of-being-fully-integrated-into-the-on-premises-network"></a><a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Cross lokale - Bereitstellung von einzelne oder mehrere SAP-virtuellen Computern in Azure mit dem lokalen Netzwerk vollständig integriert wird der Anforderung
 
![VPN mit Standorten Konnektivität (Cross lokal)][planning-guide-figure-300]

Dieses Szenario ist ein Kreuz lokale Szenario mit vielen mögliche Bereitstellungsmustern. Es kann beschrieben werden einfach einige Teile der SAP ausgeführt lokalen Querformat und anderen Teile der SAP-Querformat auf Azure. Alle Aspekte der, dass ein Teil der SAP-Komponenten auf Azure ausgeführt werden sollte Endbenutzer transparent sein. Daher wird die SAP-Transport Korrektur System (STMS) RFC Kommunikation, drucken, Sicherheit (wie SSO), usw. für auf Windows Azure ausgeführte SAP-Systeme problemlos arbeiten. Aber das Cross lokale Szenario werden auch ein Szenario, in dem die vollständige SAP-Querformat in Azure ausgeführt wird, mit der vom Kunden Domäne und DNS-Einträge zu Azure erweitert. 

> [AZURE.NOTE] Dies ist das Szenario der für die Ausführung von produktiver SAP-Betriebssystemen unterstützt wird.

Lesen Sie [diesen Artikel] [ vpn-gateway-create-site-to-site-rm-powershell] für Weitere Informationen zu Ihrem lokalen Netzwerk Verbindung mit Microsoft Azure

> [AZURE.IMPORTANT] Wenn wir Cross lokale Szenarien zwischen Azure und lokale Kunden Bereitstellungen sprechen, wird mit der Abstufung eines gesamten SAP-Systemen gesucht. Szenarien, die sind Cross lokale Szenarien _nicht unterstützt_ werden:
> 
> * Verschiedene Ebenen von SAP Anwendungen in Methoden für die Bereitstellung von anderen ausgeführt. Z. B. Ausführen der DBMS Layer lokalen, aber die SAP-Anwendungsebene in virtuellen Computern als Azure-virtuellen Computern oder umgekehrt bereitgestellt.
> * Einige Komponenten eines SAP-Layer in Azure und einige lokale. Z. B. Teilen Instanzen der SAP-Anwendungsebene zwischen lokalen und Azure-virtuellen Computern an. 
> * Verteilung der virtuellen Computern SAP-Instanzen von einem System über mehrere Azure Regionen ausgeführt wird nicht unterstützt.
> 
> Der Grund für diese Einschränkung ist die Anforderung für ein sehr niedrig Wartezeit hohe Leistung Netzwerk innerhalb einer SAP-System, insbesondere zwischen den Anwendungsinstanzen und den Layer DBMS aus einem SAP-System.



### <a name="supported-os-and-database-releases"></a>Unterstützte OS und Datenbankversionen

* Server-Software Microsoft Azure-virtuellen Computern Dienste in diesem Artikel aufgeführt ist, die für unterstützt: <http://support.microsoft.com/kb/2721672>. 
* Unterstütztes Betriebssystemversionen Datenbank System Versionen, die in Verbindung mit SAP-Software auf Azure-virtuellen Computern Services unterstützt werden im SAP-Hinweis [1928533]beschrieben. 
* SAP-Applikationen und Versionen auf Azure-virtuellen Computern Services unterstützt werden in SAP-Hinweis [1928533]beschrieben.
* Nur 64-Bit-Bilder werden unterstützt, um als Gast virtuellen Computern in Azure für SAP-Szenarien auszuführen. Dies bedeutet auch, dass nur die 64-Bit-SAP-Applikationen und Datenbanken unterstützt werden.

## <a name="microsoft-azure-virtual-machine-services"></a>Dienste für Microsoft Azure-virtuellen Computern
Die Microsoft Azure-Plattform ist ein Internet-Skala Cloud Services-Plattform gehostet und betreiben, Microsoft Data Center. Die Plattform enthält Microsoft Azure-virtuellen Computern Dienste (Infrastruktur als Dienst oder IaaS) und eine Reihe von Rich-Plattform als Servicefunktionen eines (PaaS).

Die Azure-Plattform reduziert die Notwendigkeit betreiben Technologie und Einkäufe Infrastruktur. Verwaltung und Betrieb von Applications können, indem Sie bei Bedarf berechnen und Speicherplatz zu hosten, skalieren und Verwalten von Web-Anwendung und verbundenen Applikationen vereinfacht. Infrastructure Management ist mit einer Plattform, die für eine hohe Verfügbarkeit ausgelegt ist automatisierte und dynamische Skalierung Verwendung Anforderungen mit die Option eines Modells je nach Bedarf berechnet Preisgestaltung übereinstimmt.


 
![Positionieren der Dienste für Microsoft Azure-virtuellen Computern][planning-guide-figure-400]

Mit Azure-virtuellen Computern Diensten ist Microsoft aktivieren Sie benutzerdefinierte Server Bilder als IaaS Instanzen Azure bereitstellen (siehe Abbildung 4). Den virtuellen Computern in Azure basieren auf Hyper-V virtuelle Festplatten (virtuelle Festplatte) und können verschiedenen Betriebssysteme als Gast-Betriebssystem ausführen.

Aus einem Betrieb Perspektive bietet der Azure-virtuellen Computerdienst ähnliche Erfahrung als virtuellen Computern lokal bereitgestellt. Es hat jedoch den Vorteil, den Sie nicht benötigen, beschaffen, verwalten und die Infrastruktur verwalten. Entwickler und Administratoren haben Vollzugriff auf das Betriebssystemabbild in diesen virtuellen Computern. Administratoren können in diesen virtuellen Computern Wartung und Problembehandlung Vorgänge als auch Software Bereitstellungsaufgaben ausführen Remote anzumelden. Hinsichtlich der Bereitstellung sind die einzigen Einschränkungen, die Größen und Funktionen von Azure-virtuellen Computern. Dies ist möglicherweise nicht als fein präzise Konfiguration wie folgt lokal durchgeführt werden kann. Es gibt Wahl der virtuellen Computer-Typen, die eine Kombination aus darstellen:

* Anzahl der vCPUs,
* Arbeitsspeicher,
* Anzahl der virtuelle Festplatten, die angefügt werden kann,
* Netzwerk- und Bandbreiten.

Die Größe und Schwächen verschiedenen anderen virtuellen Computern, die Größen angeboten in einer Tabelle in [diesem Artikel] angezeigt werden können[virtual-machines-sizes]

Wie Sie erreichen können gibt es verschiedene Familien oder eine Reihe von virtuellen Computern. Zum Zeitpunkt Dez 2015 können Sie die folgenden Familien virtueller Computer unterscheiden:

* Typen von a0-A7 virtueller Computer: nicht alle betreffenden für SAP zertifiziert sind. Ersten virtuellen Computer Datenreihe, denen mit Azure IaaS eingeführt haben.
* Typen von a8-A11 virtueller Computer: High Performance computing Instanzen. Ausführung auf anderen bessere Durchführung Hosts als anderen A-Serie virtuellen Computern zu berechnen.
* Typen von D-Serie virtueller Computer: besser als A0-A7 ausführt. Nicht alle Typen virtueller Computer sind mit SAP zertifiziert.
* Typen von DS-Serie virtueller Computer: Verwenden derselben Hosts als D-Serie, aber die Verbindung zu Azure Premium Speicher hergestellt werden (siehe Kapitel [Azure Premium Speicher] [planning-Leitfaden-3.3.2] dieses Dokuments). Erneut sind nicht alle Typen von virtuellen Computer mit SAP zertifiziert.
* G-Serie virtueller Computer Typen: Typen von virtuellen Computer hoch. 
* Typen von GS-Serie virtueller Computer: wie G-Serie, aber die Option zur Verwendung von Azure Premium Speicher einschließlich (siehe Kapitel [Azure Premium Speicher] [planning-Leitfaden-3.3.2] dieses Dokuments). Wenn GS-Serie virtuellen Computern als Datenbankserver verwenden, ist es obligatorisch Premium Speicher für DB-Daten und Transaktion Protokolldateien verwenden


Die gleichen CPU und Arbeitsspeicherkonfigurationen bietet in anderen virtuellen Computer Serie. Dennoch, wenn Sie die Leistungsdurchsatz diese virtueller Computer Abmelden bei der anderen Serie Nachschlagen möglicherweise erheblich anders. Obwohl er die gleiche CPU- und Konfiguration. Grund ist, dass die zugrunde liegende Host Serverhardware bei der Einführung von den verschiedenen Arten von virtuellen Computer Durchsatz verschiedene Merkmale hatte.  Normalerweise ist die Differenz in Leistungsdurchsatz Siehe auch im Preis von den anderen virtuellen Computern übernommen.

Bitte beachten Sie, dass nicht alle anderen virtuellen Computer Reihe in jeweils der Azure Regionen (für Azure Regionen finden Sie unter nächste Kapitel) angeboten werden möglicherweise. Beachten Sie auch, dass nicht alle virtuellen Computern oder virtueller Computer-Serie für SAP zertifiziert sind.

> [AZURE.IMPORTANT] Für die Nutzung von Applications SAP NetWeaver basiert werden nur die Teilmenge der virtuellen Computer und in SAP-Hinweis [1928533] aufgeführten Konfigurationen unterstützt.

### <a name="a-namebe80d1b9-a463-4845-bd35-f4cebdb5424aaazure-regions"></a><a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure Regionen
Microsoft ermöglicht virtuellen Computern in so genannte "Azure Regionen' bereitstellen. Ein Azure Region möglicherweise eine oder mehrere Data Center, die sich in der Nähe befinden. Für die meisten geopolitischen Regionen in der Welt weist Microsoft mindestens zwei Azure Regionen an. Z. B. in Europa ist ein Azure Region "North Europa" und eine "Westen Europa". Solche zwei Azure Bereiche innerhalb eines Bereichs geopolitische werden durch relevant sind, dass Abstand getrennt, damit natürliche oder technischen Datenverluste beide Regionen Azure in derselben geopolitische Region nicht beeinträchtigen. Da Microsoft kontinuierlich Global, neue Azure Regionen in unterschiedlichen geopolitischen Regionen entwickelt wurden, wird die Anzahl der folgenden Regionen kontinuierlich wächst und zum Zeitpunkt Dez 2015 erreicht die Anzahl der 20 Azure Regionen mit zusätzlichen Bereichen, die bereits angekündigt. Als Kunde können SAP-Systemen in alle diese Bereiche, einschließlich der beiden Regionen Azure in China bereitgestellt werden. Aktuelle nach Zeitphasen bis zum Datum Informationen Azure Regionen finden Sie unter diese Website: <https://azure.microsoft.com/regions/>

### <a name="a-name8d8ad4b8-6093-4b91-ac36-ea56d80dbf77athe-microsoft-azure-virtual-machine-concept"></a><a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Des Konzepts der Microsoft Azure-virtuellen Computern
Microsoft Azure bietet eine Infrastruktur as a Service (IaaS) Lösung an Host virtuellen Computern mit ähnlichen Funktionen wie eine lokale Virtualisierung-Lösung. Sie können zum Erstellen von virtuellen Computern aus innerhalb der Azure-Portal, PowerShell oder CLI, die auch Bereitstellung und Verwaltungsfunktionen bieten.

Azure Ressourcenmanager können Sie Ihre Applikationen mithilfe einer Vorlage deklarativen bereitstellen. In einer Vorlage können Sie mehrere Dienste sowie deren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage wiederholt Ihrer Anwendung während jeder Phase des Lebenszyklus der Anwendung bereitgestellt.

Weitere Informationen zur Verwendung des Cloud-Vorlagen finden Sie hier:

* [Bereitstellen und Verwalten von virtuellen Computern mithilfe von Azure Ressourcenmanager Vorlagen und Azure CLI][virtual-machines-linux-cli-deploy-templates]
* [Verwalten von virtuellen Computern mit Azure Ressourcenmanager und PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.Microsoft.com/Documentation/Templates/>

Ein weiteres interessantes Feature ist die Möglichkeit zum Erstellen von Bildern aus virtuellen Computern, wodurch Sie bestimmte Repositorys vorbereiten, aus denen Sie können schnell Instanzen von virtuellen Computern bereitstellen die Ihren Anforderungen entsprechen sind.

Weitere Informationen zum Erstellen von Bildern aus virtuellen Computern finden Sie in [diesem Artikel][virtual-machines-linux-capture-image-resource-manager].

#### <a name="a-namedf49dc09-141b-4f34-a4a2-990913b30358afault-domains"></a><a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Fehlerstrukturanalyse-Domänen
Fehlerstrukturanalyse-Domänen darstellen physische Einheit des Fehlers, sehr eng mit der physischen Infrastruktur enthaltenen Daten Centers und während einer Fehlerstrukturanalyse-Domäne eine physische Blade oder den Shapes für Gestelle angesehen werden kann, es gibt keine direkte 1: 1-Zuordnung zwischen den beiden. 

Wenn Sie mehrere virtuelle Computer als Teil einer SAP-System in Microsoft Azure-virtuellen Computern Services bereitstellen, können Sie die Azure Fabric Controller zum Bereitstellen der Anwendung in verschiedenen Fehlerstrukturanalyse-Domänen, damit auch Besprechung die Anforderungen der Microsoft Azure Vereinbarung zum SERVICELEVEL beeinflussen. Ist jedoch die Verteilung der Fehlerstrukturanalyse Domänen über eine Azure Mengen Einheit (Sammlung von mehreren hundert Datenverarbeitungsknoten oder Speicherknoten und Netzwerke) oder die Zuordnung von virtuellen Computern an eine bestimmte Fehlerstrukturanalyse-Domäne etwas über die Sie nicht die direkte Kontrolle verfügen. Um den Azure-Struktur Controller eine Reihe von virtuellen Computern über die verschiedenen Fehlerstrukturanalyse Domänen bereitstellen umgeleitet werden sollen, müssen Sie eine Azure Verfügbarkeit festzulegen, die mit den virtuellen Computern zum Zeitpunkt der Bereitstellung zuweisen. Weitere Informationen zum Azure Verfügbarkeit Mengen finden Sie unter Kapitel [Azure Verfügbarkeit Sätze] [planning-Leitfaden-3.2.3] in diesem Dokument.

#### <a name="a-namefc1ac8b2-e54a-487c-8581-d3cc6625e560aupgrade-domains"></a><a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Aktualisieren von Domänen
Upgrade Domänen repräsentieren eine logische Einheit, die Ihnen helfen, um zu bestimmen, wie ein virtueller Computer in ein SAP-System, die von SAP-Instanzen in mehreren virtuellen Computern ausgeführt besteht, aktualisiert werden sollen. Bei eine Aktualisierung wird durchläuft die Vorgehensweise zum Aktualisieren dieser Upgrade Domänen einzeln Microsoft Azure. Durch das Verteilen von virtuellen Computern zum Zeitpunkt der Bereitstellung über verschiedene Upgrade Domänen können Sie Ihre SAP-System teilweise aus potenziellen Ausfallzeiten schützen. Um Azure die virtuellen Computern der ein SAP-System über die verschiedenen Upgrade Domänen verteilt bereitstellen zu erzwingen, müssen Sie ein bestimmtes Attribut zum Zeitpunkt der Bereitstellung der einzelnen virtuellen Computer festlegen. Fehlerstrukturanalyse-Domänen ähnlich, ist eine Azure Mengen Einheit in mehrere Upgrade Domänen unterteilt. Um den Azure-Struktur Controller eine Reihe von virtuellen Computern über die verschiedenen Upgrade Domänen bereitstellen umgeleitet werden sollen, müssen Sie eine Azure Verfügbarkeit festzulegen, die mit den virtuellen Computern zum Zeitpunkt der Bereitstellung zuweisen. Weitere Informationen Azure Verfügbarkeit Mengen finden Sie unter Kapitel [Azure Verfügbarkeit Sätze] [planning-Leitfaden-3.2.3].

#### <a name="a-name18810088-f9be-4c97-958a-27996255c665aazure-availability-sets"></a><a name="18810088-f9be-4c97-958a-27996255c665"></a>Verfügbarkeit von Azure-Sätze
Azure-virtuellen Computern innerhalb einer Azure Verfügbarkeit festgelegt wird über die anderen Fehlerstrukturanalyse und Domänen Upgrade vom Azure Fabric Controller verteilt. Der Zweck der Verteilung über einen anderen Fehlerstrukturanalyse und Aktualisieren von Domänen ist zu verhindern, dass alle virtuellen Computern der ein SAP-System im Falle Infrastruktur Wartung oder Fehler innerhalb einer Fehlerstrukturanalyse-Domäne beendet wird. Standardmäßig sind virtuellen Computern nicht Teil einer Verfügbarkeit festzulegen. Die Teilnahme in einer Verfügbarkeit Festlegen eines virtuellen Computers ist zum Zeitpunkt der Bereitstellung oder höher durch eine neu konfiguriert und erneute Bereitstellung eines virtuellen Computers definiert.

Zum besseren Verständnis des Konzepts der Azure Verfügbarkeit Sätze und der Ihrem Erwerb Verfügbarkeit Sätze Fehlerstrukturanalyse und Domänen Upgrade verknüpfen, lesen Sie [diesen Artikel][virtual-machines-manage-availability]

Verfügbarkeit definieren finden Sie unter Gruppen für Cloud über eine Vorlage Json [die Rest-api Spezifikationen](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) und suchen Sie nach "Verfügbarkeit".

### <a name="a-namea72afa26-4bf4-4a25-8cf7-855d6032157fastorage-microsoft-azure-storage-and-data-disks"></a><a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Speicher: Microsoft Azure-Speicher und die Datenfestplatten
Microsoft Azure-virtuellen Computern nutzen verschiedene Speichertypen. Bei der Implementierung SAP für Azure-virtuellen Computern Services ist es wichtig, die Unterschiede zwischen diesen zwei Hauptarten von Speicher zu verstehen:

* Nicht-beständiger, veränderliche Speicher.
* Permanenter Speicher.

Der nicht beständige Speicher direkt an den laufenden virtuellen Computern angefügt wird und befindet sich auf die berechnen Knoten selbst – die lokale Instanz Speicher (temporärer Speicher). Die Größe hängt von der Größe des virtuellen Computers zu Beginn die Bereitstellung ausgewählt. Diese Speicherungsart veränderliche ist und der Datenträger ist daher Initialisierung, wenn eine Instanz des virtuellen Computer neu gestartet wird. In der Regel befindet sich die Auslagerungsdatei für das Betriebssystem auf dem temporären Datenträger.

___

> ![Windows][Logo_Windows] Windows
>
> Temp-Laufwerk ist auf Windows-virtuellen Computern als Laufwerk D:\ in einen bereitgestellten virtuellen bereitgestellt.
>
> ![Linux][Logo_Linux] Linux
> 
> Auf Linux virtuellen Computern wird es als /mnt/resource oder/mnt bereitgestellt. Finden Sie weitere Details finden Sie hier:
> 
> * [So fügen Sie einen Datenträger zu einem Linux virtuellen Computern][virtual-machines-linux-how-to-attach-disk]
> * <http://Blogs.msdn.com/b/Mast/Archive/2013/12/07/Understanding-the-Temporary-Drive-on-Windows-Azure-Virtual-Machines.aspx>

___

Da es auf dem Hostserver selbst gespeichert erste wird, ist das jeweilige Laufwerk veränderliche. Wenn der virtuellen Computer in eine erneute Bereitstellung verschoben ist der Inhalt des Laufwerks (z. B. wegen Wartung auf dem Host oder beenden und neu starten) verloren. Daher ist es nicht die Möglichkeit, wichtigen Daten auf diesem Laufwerk speichern. Der Typ der verwendeten für diese Art von Speicher Medien unterscheidet sich zwischen den virtuellen Computer Datenreihen mit sehr unterschiedliche Leistungsmerkmale der ab Juni 2015 aussehen:

* A5-A7: Sehr eingeschränkter Leistung. Nicht empfohlen für alles jenseits Page-Datei
* A8-A11: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz.
* D-Serie: Sehr gute Leistungsmerkmale mit einigen dann Tausendertrennzeichen IOPS und > 1GB/s Durchsatz.
* DS-Serie: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz.
* G-Serie: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz.
* GS-Serie: Sehr gute Leistungsmerkmale mit einigen 10.000 IOPS und > 1GB/s Durchsatz.

Oben genannten Aussagen sind auf die Typen virtueller Computer anwenden, die mit SAP zertifiziert sind. Die virtuellen Computer-Reihe mit hervorragende IOPS und Durchsatz qualifizieren, indem Sie einige Features DBMS für nutzen. Finden Sie im [DBMS Bereitstellungshandbuch] [Dbms-Leitfadens] Weitere Details.

Microsoft Azure-Speicher bietet dauerhaften Speicher und die normalen Pegel Schutz und Redundanz auf SAN-Speicher dargestellt. Virtuelle Festplatte (virtuelle Festplatten) in den Azure-Speicher-Diensten ansässig sind Datenträger, die basierend auf Azure-Speicher. Dem lokalen OS-Datenträger (C: Windows\, Linux / (/ Entwicklung/sda1)) wird im Speicher Azure gespeichert und weitere Datenmengen/Datenträger bereitgestellt, um den virtuellen Computer abrufen, gespeichert, zu.

Es ist möglich, eine vorhandene virtuelle aus lokalen hochladen oder erstellen leeren Journaleinträge in Azure, und fügen Sie Sie zu bereitgestellten virtuellen Computern. Diese virtuelle Festplatten werden als Azure Datenträger verwiesen werden. 

Nach dem Erstellen oder Hochladen einer virtuellen in Azure-Speicher, ist es möglich, bereitstellen, und fügen Sie Sie zu einer vorhandenen virtuellen Computern und vorhandene (nicht bereitgestellt) virtuelle kopieren.

Wie diese virtuelle Festplatten beibehalten werden, sind Daten und Änderungen in diesen schützen können, wenn Neustarten und neu erstellt eine Instanz des virtuellen Computers. Auch wenn eine Instanz gelöscht wird, schützen Sie sich diese virtuelle Festplatten und erneut bereitgestellt werden können oder bei BS-Datenträger können anderen virtuellen Computern bereitgestellt werden.

Innerhalb des Netzwerks Azure-Speicher können verschiedene Redundanz Ebenen konfiguriert werden:

* Minimale Berechtigungsstufe, die ausgewählt werden kann, ist 'lokale Redundanz', was entspricht drei-Kopie der Daten in der gleichen Daten Mitte eines Bereichs Azure (siehe Kapitel [Azure Regions][planning-guide-3.1]). 
* Zone redundante Speicherung der drei Bilder auf andere Daten verteilt werden zentriert innerhalb derselben Azure Region.
* Standard-Redundanzebene ist geografische Redundanz der asynchrone den Inhalt in einem anderen 3 Bilder der Daten in einer anderen Azure Region repliziert, die in der gleichen geopolitische Region gehostet wird.

Siehe auch die Tabelle oben in diesem Artikel im Hinblick auf die verschiedenen Redundanz Optionen: <https://azure.microsoft.com/pricing/details/storage/> 

Weitere Informationen zur Azure-Speicher finden Sie hier: 

* <https://Azure.Microsoft.com/Documentation/Services/Storage/>
* <https://Azure.Microsoft.com/Services/Site-Recovery>
* <https://msdn.Microsoft.com/library/windowsazure/ee691964.aspx>
* <https://Blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-Disk-Encryption-for-Linux-and-Windows-Virtual-Machines-Public-Preview.aspx>


#### <a name="azure-standard-storage"></a>Azure Standard-Speicher
Azure Standard BLOB-Speicher wurde den Typ der verfügbaren Speicherplatz Azure IaaS veröffentlicht wurde. Es wurden IOPS Kontingente pro einzelne virtuelle Festplatte erzwungen. Erfahrener Wartezeit konnte nicht in derselben Klasse wie SAN/NAS-Geräte in der Regel für High-End-SAP-Systeme bereitgestellt lokal gehostet wird. Dennoch der standardmäßigen Azure-Speicher für mehreren hundert ausreichend erwiesen SAP-Systeme, die in der Zwischenzeit in Azure bereitgestellt.

Azure Standard-Speicher belastet wird basierend auf der tatsächlichen Daten, die gespeichert sind, die Lautstärke von Speichertransaktionen, ausgehende Datenübertragung und Redundanz Option ausgewählt. Viele virtuelle Festplatten erstellt werden, bei der maximal 1TB Größe, aber solange leer bleiben ist kostenlos. Wenn Sie dann eine virtuelle Festplatte mit 100 GB ausfüllen, werden Sie in Rechnung gestellt zum Speichern von 100GB und nicht für die nominal Größe, die, der mit die virtuellen Festplatte erstellt haben.

#### <a name="a-nameff5ad0f9-f7f4-4022-9102-af07aef3bc92aazure-premium-storage"></a><a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Premium Azure-Speicher
April 2015 eingeführt Microsoft Azure Premium Storage. Premium Speicher auf Ihrem System mit dem Ziel bereitstellen eingeführt werden:

* Bessere e/a-Wartezeit.
* Besseren Durchsatz.
* Weniger Streuung e/a-Wartezeit.

Zu diesem Zweck zahlreiche Änderungen eingeführt wurden von welche die beiden wichtigsten sind:

* Verwendung der SSD Datenträger in den Knoten Azure-Speicher
* Eine neue Lese-Cache, die durch die lokale SSD eines Knotens Azure berechnen unterstützt wird

In entgegen standardmäßigen Speicher, in dem Funktionen nicht ändern, hängt von der Größe der Festplatte (oder die virtuelle Festplatte) Premium Speicher aktuell hat 3 anderen Datenträger Kategorien, die am Ende dieses Artikels vor dem Abschnitt häufig gestellte Fragen zu gezeigt werden: <https://azure.microsoft.com/pricing/details/storage/>

Sie sehen, dass IOPS/virtuelle Festplatte und Datenträger Durchsatz/virtuelle Festplatte werden, hängt von der Größenkategorie der Datenträger

Kosten Basis bei Premium Speicher ist nicht der tatsächlichen Daten Lautstärke gehörende Kehrmatrix solche virtuellen Festplatten, aber die Größenkategorie der solche eine virtuelle Festplatte, unabhängig von der Datenmenge, die in die virtuelle Festplatte gespeichert ist.

Sie können auch virtuelle Festplatten erstellen, Premium Speichermenge, die nicht direkt in den angezeigten Größe Kategorien zuordnen. Dies kann der Fall sein, besonders, wenn virtuelle Festplatten aus standardmäßigen Speicher in Premium Speicher zu kopieren. In diesem Fall wird eine Zuordnung zur nächsten größten Premium Speicher Datenträger Option ausgeführt. 

Bitte beachten Sie, dass nur bestimmte virtueller Computer Reihe von den Azure-Speicher Premium profitieren kann. Zum Zeitpunkt Dez 2015 sind diese DS und GS-Serie aus. Die DS-Serie entspricht im Grunde der D-Serie, mit der Ausnahme, dass die DS-Serie die Möglichkeit zum Bereitstellen Premium Speicher weist Grundlage virtuelle Computer außerdem virtuelle Festplatten gehostet werden, die mit standardmäßigen Azure-Speicher. Dasselbe gilt für G-Serie im Vergleich zu GS-Serie.

Wenn Sie nur den Teil der DS-Serie virtuellen Computern in [diesem Artikel] Auschecken[ virtual-machines-sizes] Sie auch können dabei, dass Daten Lautstärke Schwächen zu Premium Speicher virtueller Festplatten auf die Genauigkeit der Ebene virtueller Computer vorhanden sind. Anderen DS-Serie oder GS-Serie virtuellen Computern haben andere Einschränkungen auch im Hinblick auf die Anzahl der virtuelle Festplatten, die bereitgestellt werden kann. Diese Grenzwerte werden in auch weiter oben erwähnten Artikel beschrieben. Aber im Wesentlichen Dies bedeutet, dass Sie z. B. 32 x P30 Datenträger/virtuellen-Festplatten zu eines einzelnen DS14 virtuellen Computers bereitstellen Sie nicht 32 x den maximalen Durchsatz eines P30 Datenträgers zugreifen können. Datensatz beschränken Sie stattdessen der maximale Durchsatz auf virtuellen Computer Ebene wie im Artikel erläutert. 

Weitere Informationen zu Premium Speicher finden Sie hier: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure-Speicher-Konten
Bei der Bereitstellung von Dienstleistungen oder virtuellen Computern in Azure muss Bereitstellung von virtuellen Festplatten und virtueller Computer Bilder in einer anderen Einheit aufgerufen Azure-Speicherkonten angeordnet werden. Bei der Planung einer Azure-bereitstellungs müssen Sie die Einschränkungen von Azure überlegt. Klicken Sie auf der 1-Seite gibt es eine begrenzte Anzahl von Speicher-Konten pro Azure-Abonnement. Zwar jedes Azure-Speicherkonto eine große Anzahl von virtuellen Festplatte Dateien enthalten kann, gibt es eine feste Einschränkung auf die gesamte IOPS pro Speicher-Konto an. Bei der Bereitstellung von Hundert SAP-virtuellen Computern mit DBMS-Systeme signifikante EA-Anrufe erstellen, empfiehlt es hohe IOPS DBMS virtuellen Computern zwischen mehreren Azure-Speicherkonten verteilen. Sie müssen sorgfältig nicht auf die aktuelle von Azure-Speicherkonten pro Abonnement überschritten. Da Speicher ein wesentlicher Bestandteil der Bereitstellung der Datenbank für ein SAP-System ist, wird dieses Konzept in das bereits verwiesen wird [DBMS Bereitstellungshandbuch] [Dbms-Leitfadens] ausführlicher erläutert.

Weitere Informationen zu Azure-Speicherkonten finden Sie in [diesem Artikel][storage-scalability-targets]. Lesen in diesem Artikel, werden Sie feststellen, dass es Unterschiede in die Einschränkungen Azure Standard-Speicher-Konten und Premium Speicherkonten sind. Wichtigsten Unterschiede sind die Menge der Daten, die in solche Speicher-Konto gespeichert werden können. In Standard-Speicher ist der Datenträger eine Größe größer als mit Premium-Speicher. Klicken Sie auf der anderen Seite ist im Speicher Standardkonto nur äußerst IOPS (siehe Spalte Total anfordern Rate), während das Azure Premium Speicherkonto keine solche Einschränkung verfügt. Wenn die Bereitstellung von SAP-Systeme, insbesondere die DBMS Server besprochen werden Details und die Ergebnisse dieser Unterschiede erläutert.

Innerhalb eines Speicher-Konto haben Sie die Möglichkeit zum Erstellen von verschiedenen Containers im Sinne organisieren und Kategorisieren von anderen virtuellen Festplatten. Diese Container werden in der Regel, z. B. separaten virtuellen Festplatten von anderen virtuellen Computern verwendet. Es gibt keine Auswirkungen Leistung bei der Verwendung von nur einem oder die mehrere Container unter einem einzigen Azure-Speicher-Konto an.

In Azure umfasst ein Namen für die virtuelle Festplatte folgende naming Verbindung, für die muss einen eindeutigen Namen für die virtuelle Festplatte in Azure bereit:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Wie schon erwähnt, dass die oben angegebenen Zeichenfolge muss eindeutig die virtuelle Festplatte zu identifizieren, die auf Azure Storage gespeichert ist.

### <a name="a-name61678387-8868-435d-9f8c-450b2424f5bdamicrosoft-azure-networking"></a><a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure Netzwerke
Microsoft Azure nun eine Netzwerkinfrastruktur bereit, wodurch die Zuordnung von allen Szenarien derer wir mit SAP-Software einreichen möchten. Die Funktionen sind:

* Zugriff von außen, direkt mit den virtuellen Computern über Windows-Terminaldienste oder ssh/VNC
* Zugriff auf Dienste und bestimmte Ports von Applications innerhalb der virtuellen Computern verwendet
* Interne Kommunikation und namensauflösung zwischen einer Gruppe von virtuellen Computern befinden, die als Azure-virtuellen Computern
* Cross lokale Konnektivität zwischen lokalen und Azure Netzwerk des Kunden
* Cross Azure Region oder Data Center Konnektivität zwischen Azure-Websites 

Weitere Informationen finden Sie hier: <https://azure.microsoft.com/documentation/services/virtual-network/>

Es gibt zahlreiche verschiedene Situationen auftreten, um Namen und IP-Auflösung in Azure konfigurieren. In diesem Dokument verlassen sich nur-Cloud-Szenarien standardmäßig von Azure DNS (im Gegensatz zum Definieren eines eigenen DNS-Diensts) ein. Es gibt auch ein neuer Azure DNS-Dienst verwendet werden kann, statt Einrichten Ihrer eigenen DNS-Servers. Weitere Informationen finden Sie in [diesem Artikel] [ virtual-networks-manage-dns-in-vnet] und auf [dieser Seite](https://azure.microsoft.com/services/dns/).

Für Cross lokale Szenarien wir davon abhängt, dass die Fakultät, die der lokalen AD/OpenLDAP/DNS über VPN oder private Verbindung in Azure erweitert wurde. Für bestimmte Szenarien wie hier beschrieben, es in einer AD/OpenLDAP Kopie in Azure installiert haben möglicherweise erforderlich.

Da networking und namensauflösung ist ein wichtiger Bestandteil der Bereitstellung Datenbank für ein SAP-System, wird dieses Konzept im [DBMS Bereitstellungshandbuch] [Dbms-Leitfadens] ausführlicher erläutert.


##### <a name="azure-virtual-networks"></a>Azure virtuelle Netzwerke

Durch ein Azure virtuelles Netzwerk aufbauen können Sie den Bereich der privaten IP-Adressen von Azure DHCP-Funktionalität zugeordnet wurden, definieren. In Cross lokale Szenarien wird definierten Bereichs die IP-Adresse dennoch reserviert werden mit DHCP von Azure. Jedoch mit einer Auflösung von Domänennamen erfolgt lokal (vorausgesetzt, dass die virtuellen Computern eine lokale Domäne gehören) und daher zu Adressen außerhalb der verschiedenen Azure-Cloud-Diensten beheben kann.

[Kommentar]: <>  (MSSedusch, die noch erforderliche? Erledigen ursprünglich ein Azure virtuelles Netzwerk wurde an eine Gruppe für die Zugehörigkeit gebunden. Ein virtuelles Netzwerk in Azure rufen ab, eingeschränkten zur Azure-Skala Einheit, die die Zugehörigkeit Gruppe zugewiesen haben. Am Ende darauf folgt an, dass das virtuelle Netzwerk auf den Ressourcen zur Verfügung, in der Azure-Skala Einheit beschränkt wurde. Dies wurde seit geändert und Azure-virtuellen Netzwerken können jetzt über mehr als eine Azure-Skala Einheit Strecken. Erfordert jedoch, dass Azure virtuelle Netzwerke sind ** nicht ** zugeordnet Gruppen die zum Zeitpunkt der Erstellung nicht mehr. Wir bereits erwähnt, dass in entgegen Empfehlungen ein Jahr vor, Sie sollten ** nicht mehr nutzen Gruppen die Azure **. Weitere Informationen finden Sie unter < Https://azure.microsoft.com/blog/regional-virtual-networks/>)

Jeder virtuellen Computern in Azure muss mit einem virtuellen Netzwerk verbunden werden.

Weitere Informationen hierzu finden Sie in [diesem Artikel] [ resource-groups-networking] und auf [dieser Seite](https://azure.microsoft.com/documentation/services/virtual-network/).

[Kommentar]: <>  (Einen Artikel, der einschließlich der OpenLDAP Thema + Cloud; MShermannd erledigen konnten nicht finden)
[Kommentar]: <>  (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [AZURE.NOTE] Standardmäßig nach der Bereitstellung eines virtuellen Computers ist können Sie die virtuelle Netzwerkkonfiguration nicht ändern. Die Einstellungen TCP/IP müssen auf dem Server Azure DHCP bleiben. Standardverhalten ist dynamische IP-Zuordnung an.

Die MAC-Adresse der Karte virtuelles Netzwerk möglicherweise z. B. nach Größe ändern und den Windows oder Linux Gast, den OS wird im Netzwerk Glückwunschkarte Abholen und wird automatisch DHCP in diesem Fall weisen Sie die IP-Adresse und DNS-Adressen, ändern.

##### <a name="static-ip-assignment"></a>Statische IP-Zuordnung
Es ist möglich, feste oder reservierte IP-Adressen virtuellen Computern innerhalb einer Azure-virtuellen Netzwerk zuzuweisen. Ausführen von den virtuellen Computern in einer Azure-virtuellen Netzwerk öffnet eine großartige Möglichkeit, diese Funktionen nutzen, wenn erforderlich oder für einige Szenarien erforderlich ist. Die IP-Zuordnung bleibt in der gesamten das Vorhandensein den virtuellen Computer, unabhängig davon, ob der virtuellen Computer Running oder war(en) ist gültig. Daher müssen Sie die Anzahl der virtuellen Computern (virtuelle Computer ausgeführt werden und beendet) berücksichtigt berücksichtigen, wenn die Bereich der IP-Adressen für das virtuelle Netzwerk definieren. Die IP-Adresse bleibt zugewiesen, bis Sie den virtuellen Computer und die zugehörigen Netzwerk-Benutzeroberfläche wird gelöscht oder bis die IP-Adresse erneut heben zugewiesen wird. Ausführliche Informationen in [diesem Artikel]finden Sie[virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Mehrere NICs pro virtueller Computer
Sie können mehrere virtuelle Netzwerk Benutzeroberflächen-Karten (vNIC) für eine Azure-virtuellen Computern definieren. Die Möglichkeit, mehrere eine haben, die Sie zum Einrichten der Netzwerkdatenverkehr starten können ist Trennung, z. B. Clientdatenverkehr über eine vNIC und Back-End-Datenverkehr weitergeleitet werden, durch eine zweite vNIC geleitet. Abhängige werden auf den Typ des virtuellen Computer es andere Einschränkungen hinsichtlich der Anzahl von eine. Genaue Details, Funktionalität und Einschränkungen finden Sie in folgenden Artikeln:
 
* [Erstellen eines virtuellen Computers mit mehreren NICs][virtual-networks-multiple-nics]
* [Bereitstellen von Multi NIC virtuellen Computern mithilfe einer Vorlage][virtual-network-deploy-multinic-arm-template]
* [Bereitstellen von Multi NIC virtuellen Computern mithilfe der PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Bereitstellen von Multi NIC virtuellen Computern mithilfe der Azure-CLI][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Konnektivität zwischen Standorten
Cross lokale ist Azure-virtuellen Computern und lokal mit transparenten und permanente VPN-Verbindung verknüpft. Es wird davon ausgegangen, die am häufigsten verwendeten SAP-Bereitstellungsmuster in Azure vorgesehen ist. Ausgegangen ist, dass die vorbereitenden Schritte und Prozessen mit SAP-Instanzen in Azure transparent funktionieren sollte. Diese bedeutet, die Sie Drucken aus diesen Systemen sollten sowie verwenden, die die SAP-Transport Management System (TMS) an den Transport aus einem Entwicklungssystem in Azure zu einem Testsystem ändert sich also bereitgestellt lokalen. In [diesem Artikel] kann weitere Dokumentation entsprechend zwischen Standorten gefunden werden[vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-Tunnelgerät
Zum Erstellen eine Verbindung zwischen Standorten (lokaler Daten-Center auf Azure Datacenter), müssen Sie Sie erhalten und Konfigurieren einer VPN-Gerät oder Routing und Service (RAS) die eingeführt wurde als Softwarekomponente mit Windows Server 2012 verwenden. 

* [Erstellen eines virtuellen Netzwerks mit einem Website-zu-Standort VPN-Verbindung mithilfe der PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Informationen zu VPN-Geräten für Standort-zu-Standort VPN-Gateway-Verbindungen][vpn-gateway-about-vpn-devices]
* [VPN-Gateway häufig gestellte Fragen][vpn-gateway-vpn-faq]

![Website-zu-Standort-Verbindung zwischen lokalen und Azure][planning-guide-figure-600]

Die obige Abbildung zeigt, dass zwei Azure-Abonnements virtueller Netzwerke in Azure IP-Adresse Unterbereiche für die Verwendung reserviert haben. Die Verbindung zwischen dem lokalen Netzwerk und Azure wird über VPN eingerichtet.

#### <a name="point-to-site-vpn"></a>Punkt-zu-Standort VPN
Punkt-zu-Standort VPN erfordert, dass jeder Clientcomputer herstellen von Verbindungen mit einem eigenen VPN in Azure. Für die SAP-Szenarien wir am gefunden werden, ist nicht Punkt-zu-Standort Connectivity praktisch. Daher werden keine weiteren Verweise auf Punkt-zu-Standort VPN-Konnektivität angegeben sein.

[Kommentar]: <>  (MSSedusch – Weitere Informationen finden Sie hier)
[Kommentar]: <>  (MShermannd erledigen Link nicht mehr gültig; Aber trotzdem Cloud wird nicht unterstützt – finden Sie unter Hyperlink weiter unten)
[Kommentar]: <>  (MSSedusch – < http://msdn.microsoft.com/library/azure/dn133798.aspx>.)
[Kommentar]: <>  (MShermannd erledigen, zeigen Sie auf der Website, die noch nicht mit der Cloud nicht unterstützt.)
[Kommentar]: <>  (MSSedusch – < Https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/>)

#### <a name="multi-site-vpn"></a>Website mit mehreren VPN
Azure bietet heutzutage auch die Möglichkeit zum Erstellen der Website mit mehreren VPN-Konnektivität für eine Azure-Abonnement. Ein einzelnes Abonnement war zuvor auf eine Website-zu-Standort VPN Verbindung beschränkt. Diese Einschränkung ist Weg mit mehreren Standorten VPN-Verbindungen für ein einzelnes Abonnement. Hierdurch mehrere Azure Region für bestimmte Abonnement durch Cross lokale Konfigurationen nutzen können.

Weitere Dokumentation finden Sie [in diesem Artikel][vpn-gateway-create-site-to-site-rm-powershell]
[Kommentar]: <> (MShermannd TODO gefunden keine Cloud Doku-Link)

#### <a name="vnet-to-vnet-connection"></a>VNet auf VNet-Verbindung
Verwenden die Website mit mehreren VPN, müssen Sie ein separates Azure-virtuellen Netzwerk in jeder der Regionen zu konfigurieren. Jedoch sehr häufig müssen Sie die Anforderung, die die Software-Komponenten in den verschiedenen Regionen miteinander kommunizieren soll. Idealerweise sollte diese Kommunikation aus einem Azure-Region auf lokale und von dort in der anderen Azure Region nicht weitergeleitet werden. Klicken Sie auf die Verknüpfung bietet Azure an, dass die Möglichkeit zum Konfigurieren einer Verbindungs von einem Azure-virtuellen Netzwerk in einem Region in ein anderes Azure-virtuellen Netzwerk in einem anderen Region gehostet. Dieses Feature heißt VNet-VNet-Verbindung. Weitere Informationen zu dieser Funktion finden Sie hier: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure--expressroute"></a>Private Verbindung mit Azure – ExpressRoute
Microsoft Azure ExpressRoute ermöglicht die Erstellung einer privaten Verbindungen zwischen Azure Data Center und den Kunden lokalen Infrastruktur oder in einer Umgebung für die gemeinsame Speicherort. ExpressRoute wird von verschiedenen MPLS angeboten (Paket aktiviert) VPN-Anbieter oder anderen Netzwerk-Dienstanbieter. ExpressRoute Verbindungen gehen Sie nicht über das öffentliche Internet. ExpressRoute Verbindungen bieten höhere Sicherheit, weitere Zuverlässigkeit über mehrere parallele Schaltkreise, höhere Geschwindigkeit und unteren Wartezeiten als normalen Verbindungen über das Internet an. 

Suchen nach Informationen zur Azure ExpressRoute und Angebote hier:

* <https://Azure.Microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.Microsoft.com/Pricing/Details/expressroute/>
* <https://Azure.Microsoft.com/Documentation/articles/expressroute-FAQs/>

Express-Routing ermöglicht mehrere Azure-Abonnements über eine ExpressRoute Verbindung aus, wie hier beschrieben. 

* <https://Azure.Microsoft.com/Documentation/articles/expressroute-HOWTO-linkvnet-ARM/> 
* <https://Azure.Microsoft.com/Documentation/articles/expressroute-HOWTO-Circuit-ARM/>


#### <a name="forced-tunneling-in-case-of-cross-premise"></a>Erzwungene Tunnel bei lokalen Cross-Installation
Für virtuelle Computer beitreten zu lokalen Domänen bis-Standorten, Punkt-zu-Standort oder ExpressRoute müssen Sie sicherstellen, dass die Internet-Proxyeinstellungen für alle Benutzer in diesen virtuellen Computern ebenfalls bereitgestellt erste sind. Standardmäßig in diesen virtuellen Computern oder ein Benutzer mit einem Browser auf das Internet zugreifen ausgeführte Software nicht über den Proxy Unternehmen wechseln möchten, aber direkt über Azure mit dem Internet verbinden möchten. Auch die Proxyeinstellung, nicht jedoch eine Lösung 100 %, um den Datenverkehr über den Proxy Unternehmen zu leiten, da es von Software und-Diensten zu prüfen, ob der Proxy zuständig ist. Wenn die Software auf dem virtuellen Computer ausgeführt wird, die nicht ausführen, oder ein Administrator bearbeitet die Einstellungen, den Datenverkehr mit dem Internet kann werden detoured erneut direkt über Azure mit dem Internet. 

Um dies zu vermeiden, können Sie mit Standorten Konnektivität zwischen lokalen und Azure Erzwungene Tunnel konfigurieren. Die detaillierte Beschreibung der Funktion Erzwungene Tunnel ist veröffentlichten hier <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/> 

Erzwungene Tunneling mit ExpressRoute ist in einer standardmäßigen Routing über die ExpressRoute BGP Peeringliste Sitzungen Werbung Kunden aktiviert.

#### <a name="summary-of-azure-networking"></a>Zusammenfassung der Azure Netzwerke
In diesem Kapitel enthaltenen viele wichtige Punkte zu Azure Networking. Hier finden Sie eine Zusammenfassung der wichtigsten Punkte:


* Azure virtuelle Netzwerke ermöglicht das Netzwerk gemäß Ihren eigenen Wünschen einrichten
* Azure virtuelle Netzwerke können genutzt werden, um IP-Adressbereiche virtuellen Computern zuweisen oder Zuweisen von feste IP-Adressen zu virtuellen Computern
* Um eine Verbindung zwischen Standorten oder Punkt-zu-Standort einrichten, müssen Sie zuerst ein Azure virtuelles Netzwerk erstellen
* Nach der Bereitstellung eines virtuellen Computers ist es nicht mehr möglich, das virtuelle Netzwerk zugeordnet werden, um den virtuellen Computer zu ändern.

### <a name="quotas-in-azure-virtual-machine-services"></a>Kontingente Azure-virtuellen Computern-Dienste
Müssen wir über die Fakultät deutlich zu sagen, dass die Speicherung und Netzwerk-Infrastruktur zwischen virtuellen Computern ausführen eine Reihe von Diensten in der Azure-Infrastruktur freigegeben werden. Und einfach wie die des Kunden Data Centers, Bereitstellung von einige der Infrastrukturressourcen zu viel führt dies zu einem gewissen Grad. Microsoft Azure-Plattform verwendet Datenträger, CPU, Netzwerk und andere Kontingente den Ressourcenverbrauch begrenzen und konsistente und deterministisch Leistung beizubehalten.  Die verschiedenen Typen der virtuellen Computer (A5, A6 usw.) verfügen über unterschiedliche Kontingente für die Anzahl der Datenträger, CPU, RAM und Netzwerk.

> [AZURE.NOTE] CPU- und Ressourcen virtueller Computer Arten von SAP unterstützt werden vorab zugewiesene auf den Hostknoten. Dies bedeutet, nachdem Sie der virtuellen Computer bereitgestellt wird, die Ressourcen auf dem Host als vom Typ virtueller Computer definiert erhältlich ist.

Bei der Planung und SAP auf Azure Lösungen Ziehpunkt müssen der Kontingente für jede Größe des virtuellen Computers berücksichtigt werden.  Die virtuellen Computer Kontingente beschrieben sind [hier][virtual-machines-sizes].

Die Kontingente beschrieben stehen für die theoretische maximale Werte.  Limit IOPS pro virtuelle Festplatte mit kleinen IOs (8kb) erreicht werden kann, aber möglicherweise möglicherweise nicht mit großen IOs (1Mb) erreicht werden.  Klicken Sie auf die Genauigkeit der einzelnen virtuellen Festplatten wird die Beschränkung IOPS erzwungen.

Als grob Entscheidungsstruktur selbst entscheiden, ob ein SAP-System in Azure-virtuellen Computern Dienste und deren Funktionen oder gibt an, ob ein vorhandenes System anders konfiguriert sein passt, damit das System auf Azure bereitstellen muss kann die folgenden Entscheidungsstruktur verwendet werden:
 
![Möglichkeit zum Bereitstellen von SAP auf Azure entscheiden Entscheidungsstruktur][planning-guide-figure-700]

**Schritt 1**: die wichtigste Informationen ist zunächst die Anforderung SAPS für ein bestimmtes SAP-System. Die SAPS Anforderungen müssen sich das Webpart DBMS und im Webpart SAP-Anwendung getrennt werden müssen auch wenn das SAP-System bereits bereitgestellten lokal in einer 2-Ebenen-Konfiguration ist. Für vorhandene Systeme können die SAPS häufig im Zusammenhang mit der Hardware verwendet bestimmt oder geschätzt basierend auf einer vorhandenen SAP-Benchmarks. Die Ergebnisse finden Sie hier: <http://global.sap.com/campaigns/benchmark/index.epx>. Für neu bereitgestellten SAP-Systeme sollten Sie über einen Ziehpunkt Übung überschritten haben die die Anforderungen SAPS des Systems bestimmt werden soll.
Siehe auch diesen Blog und angefügten Dokument für SAP-Ziehpunkt auf Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Schritt2**: für vorhandene Systeme, die Lautstärke e/a und e/a-Vorgänge pro Sekunde auf dem Server DBMS gemessen werden soll. Neu geplanten Systeme sollte die Ziehpunkte Übung für das neue System auch grobe Ideen zu den e/a-Anforderungen auf der Seite DBMS mitzuteilen. Wenn Sie unsicher sind, müssen Sie später eine Nachweis des Konzepts durchführen.

**Schritt 3**: Vergleichen die SAPS Notwendigkeit DBMS Server mit der SAPS verschiedener virtueller Computer Azure angeben können. Die Informationen auf SAPS der verschiedenen Azure-virtuellen Computer wird im SAP-Hinweis [1928533]beschrieben. Der Fokus sollten auf die DBMS VM zuerst, da die Datenbankebene die Ebene in einem NetWeaver SAP-System ist, die sich nicht in den meisten Bereitstellungen skaliert wird. Im Gegensatz dazu kann der SAP-Anwendung Layer skaliert werden. Wenn keines der SAP unterstützt Typen Azure-virtuellen Computer können die erforderlichen SAPS vorführen, die Arbeitsbelastung der geplanten SAP-System kann nicht auf Azure ausgeführt werden. Sie, die entweder das System bereitstellen müssen lokalen oder müssen Sie die Arbeitsbelastung Lautstärke für das System zu ändern.

**Schritt 4**: als dokumentierten [hier][virtual-machines-sizes], Azure erzwingt ein Kontingent IOPS pro virtuelle Festplatte unabhängige, ob Sie die standardmäßigen Speicher oder Premium Speicher verwenden. Hängt von den Typ des virtuellen Computer, ist die Anzahl der virtuellen Festplatten der bereitgestellt werden kann. Daher können Sie eine maximale Anzahl von IOPS berechnen, die mit den verschiedenen Arten von virtuellen Computer erreicht werden können. Abhängig von der Datenbank Dateilayout, können Sie virtuelle Festplatten, um die Konvertierung in ein Volumen im Gast OS versehen. Wenn die aktuelle IOPS Lautstärke eine bereitgestellte SAP-System die berechneten Grenzwerte den größten virtuellen Computer Typ Azure, und es besteht keine Möglichkeit überschreitet, mit mehr Speicher angepasst, kann die Arbeitsbelastung, der das SAP-System stark beeinträchtigt. In diesen Fällen können Sie einen Punkt erreichen, in dem Sie das System auf Azure nicht bereitstellen sollten.

**Schritt 5**: besonders in SAP Systeme, die sind lokal in der Ebene 2 Konfigurationen bereitgestellt wird, sind die Chancen, dass das System möglicherweise in einer 3-Ebenen-Konfiguration auf Azure konfiguriert sein muss. In diesem Schritt müssen Sie überprüfen, ob es ist eine Komponente in der SAP-Anwendungsebene, die nicht möglich skaliert werden und die nicht in den CPU- und Ressourcen, die die verschiedenen Typen von Azure-virtuellen Computer bieten anpassen möchten. Befindet sich tatsächlich um eine solche Komponente, können das SAP-System und seine Arbeitsbelastung in Azure bereitgestellt werden. Aber wenn Sie die SAP-Anwendungskomponenten in mehreren Azure-virtuellen Computern Skalierung können, kann das System in Azure bereitgestellt werden. 

**Schritt 6**: Wenn DBMS und SAP-Anwendung Layer Komponenten in Azure-virtuellen Computern ausgeführt werden können, muss die Konfiguration Stand definiert werden:

* Anzahl der Azure-virtuellen Computern
* Virtueller Computer Datentypen für die einzelnen Komponenten
* Anzahl der virtuellen Festplatten in DBMS VM genügend IOPS bereitstellen

## <a name="managing-azure-assets"></a>Verwalten von Azure Anlagen

### <a name="azure-portal"></a>Azure-Portal
Das Azure-Portal ist eine von drei Schnittstellen Azure-virtuellen Computer-Bereitstellungen verwalten. Die grundlegende Verwaltungsaufgaben, wie die Bereitstellung von virtuellen Computern Bilder, können über das Portal Azure vorgenommen werden. Darüber hinaus die Erstellung von Speicher-Konten, virtuelle Netzwerke und andere Azure Komponenten stehen auch Aufgaben, die im Portal Azure gut verarbeitet werden können. Jedoch lokale in Azure Funktionen wie virtuelle Festplatten aus hochladen oder Kopieren einer virtuellen in Azure Aufgaben, die entweder Tools von Drittanbietern oder Administration über PowerShell oder CLI erforderlich sind.
 
![Portal von Microsoft Azure - virtuellen Computern (Übersicht)][planning-guide-figure-800]

[Kommentar]: <>  (MSSedusch * < Https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[Kommentar]: <>  (MSSedusch * < Https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Verwaltung und Konfiguration von Aufgaben für die Instanz des virtuellen Computern sind aus innerhalb des Portals Azure möglich. 

Zusätzlich zu neu zu starten und Beenden eines virtuellen Computers können auch anfügen, trennen und Daten Datenträger für die Instanz von virtuellen Computern, die Instanz für Bild Vorbereitung erfassen und konfigurieren die Größe der Instanz des virtuellen Computers erstellen.

Das Azure-Portal stellt Grundfunktionen zum Bereitstellen und Konfigurieren von virtuellen Computern und viele andere Dienste Azure bereit. Im Portal Azure jedoch nicht alle verfügbaren Funktionen Gegenstand ist. Im Portal Azure ist es nicht möglich, wie Aufgaben ausführen:

* Hochladen von virtuellen Festplatten in Azure
* Kopieren von virtuellen Computern

[Kommentar]: <>  (MShermannd TODO Wissenswertes zu Automatisierung für SAP-virtuellen Computern Service?)
[Kommentar]: <>  (MSSedusch Bereitstellung von mehreren virtuellen Computern os in der Zwischenzeit möglich)
[Kommentar]: <>  (MSSedusch auch jede Art von Automatisierung hinsichtlich Bereitstellung ist nicht möglich mit Azure-Portal. Aufgaben wie Skripts Bereitstellung von mehreren virtuellen Computern nicht über das Azure-Portal möglich ist.) 

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Management über Microsoft Azure PowerShell-cmdlets
Windows PowerShell ist ein leistungsfähiges und extensible Framework, das stark von Kunden Bereitstellen von größere Anzahl von Systemen in Azure übernommen hat. Nach der Installation von PowerShell-Cmdlets auf Desktop, Laptop oder dedizierte Verwaltungsstation können Remote PowerShell-Cmdlets ausgeführt werden.

Der Vorgang zum Aktivieren einer lokalen Desktop oder ein Notebook für die Verwendung der Azure-PowerShell-Cmdlets und die für die Verwendung mit der Azure-Abonnements in [diesem Artikel]beschriebenen konfigurieren[powershell-install-configure]. 

Ausführlichere Schritte zum Installieren, aktualisieren und konfigurieren die Azure PowerShell-Cmdlets finden Sie auch im [Dieses Kapitel im Bereitstellungshandbuch] [Bereitstellung Leitfaden 4.1].

Customer Experience wurde bisher PowerShell (PS) sicher leistungsfähigeren Tools zum Bereitstellen von virtuellen Computern und zum Erstellen von benutzerdefinierten Schritte bei der Bereitstellung von virtuellen Computern sind. Alle Kunden SAP-Instanzen in Azure ausgeführt werden PS Cmdlets verwendet, um Verwaltungsaufgaben ergänzen, die Sie Azure-Portal führen oder sogar PS Cmdlets ausschließlich zum Verwalten ihrer Bereitstellungen in Azure verwenden. Da die Azure bestimmte Cmdlets die gleiche Benennungskonvention als die mehr als 2000 verwandte Windows-Cmdlets freigeben möchten, ist es eine einfache Aufgabe für Windows-Administratoren, diese Cmdlets zu nutzen.

Im folgenden Beispiel finden Sie unter: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[Kommentar]: <>  (Beschreiben Sie neuen CLI-Befehl wenn getestet MShermannd erledigen)
Bereitstellung von Azure Überwachung Erweiterung für SAP (finden Sie in Kapitel [Azure Überwachung Lösung für SAP] [planning-Leitfaden-9.1] in diesem Dokument) über PowerShell oder CLI nur möglich wird. Daher ist es zum Einrichten und Konfigurieren der PowerShell oder CLI beim Bereitstellen oder Verwalten von einem NetWeaver SAP-System in Azure obligatorisch.  

Wie Azure weitere Funktionen bereitstellt, werden neuen PS Cmdlets vertraut, dass erfordert ein Update Cmdlets hinzugefügt werden soll. Daher ist es sinnvoll, Azure-Downloadwebsite mindestens einmal überprüfen, wenn der Monat <https://azure.microsoft.com/downloads/> für eine neue Version der Cmdlets. Die neue Version wird nur über die ältere Version installiert werden.

Für eine allgemeine Übersicht Azure Zusammenhang PowerShell-Befehlen aktivieren Sie hier: <https://msdn.microsoft.com/library/azure/dn708514.aspx>. 

### <a name="management-via-microsoft-azure-cli-commands"></a>Management über Microsoft Azure CLI-Befehle

Für Kunden, Linux verwenden und Azure verwalten möchten, möglicherweise Ressourcen Powershell keine Option. Microsoft bietet Azure CLI als Alternative.
Die CLI Azure bietet eine Reihe von open Source Plattform-Befehle für die Arbeit mit Azure-Plattform. Die CLI Azure bietet im Wesentlichen die gleiche Funktionalität Azure-Portal gefunden.

Informationen zur Installation, Konfiguration und Verwendung von CLI finden Sie unter Befehle Azure Aufgaben

* [Installieren Sie die Azure CLI][xplat-cli]
* [Bereitstellen und Verwalten von virtuellen Computern mithilfe von Azure Ressourcenmanager Vorlagen und Azure CLI][virtual-machines-linux-cli-deploy-templates]
* [Verwenden Sie die Azure CLI für Mac, Linux und Windows mit Azure Ressourcenmanager][xplat-cli-azure-resource-manager]

Sie können auch nachlesen Kapitel [Azure CLI für Linux virtuellen Computern] [Bereitstellung-Leitfaden-4.5.2] im [Bereitstellungshandbuch] [Planungshandbuch] zum Azure CLI zu verwenden, um die Azure Überwachung Erweiterung für SAP bereitstellen.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Andere Methoden zum Bereitstellen von virtuellen Computern für SAP in Azure
In diesem Kapitel lernen Sie die verschiedenen Verfahren zum Bereitstellen eines virtuellen Computers in Azure. In diesem Kapitel werden zusätzliche Vorbereitung Verfahren sowie Handhabung von virtuellen Festplatten und virtuellen Computern in Azure behandelt.

### <a name="deployment-of-vms-for-sap"></a>Bereitstellung von virtuellen Computern für SAP
Microsoft Azure bietet mehrere Methoden zum Bereitstellen von virtuellen Computern und die zugehörigen Festplatten. Somit ist es wichtig, die Unterschiede zu verstehen, da die virtueller Computer Vorbereitungen je nach der Methode der Bereitstellung unterscheiden sich möglicherweise. Im Allgemeinen dauert wir einen Blick in den folgenden Szenarien:

#### <a name="a-name4d175f1b-7353-4137-9d2f-817683c26e53amoving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a><a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Verschieben eines virtuellen Computers zu Azure aus lokalen einem Datenträger nicht GRG
Sie möchten ein bestimmtes SAP-System von lokalen Azure wechseln. Dies kann die virtuelle Festplatte die enthält die OS, SAP-Binärdateien und DBMS Binärdateien sowie die virtuellen Festplatten mit den Daten, und melden Sie sich Dateien des DBMS in Azure hochladen erfolgen. Im Gegensatz zu anderen [Szenario #2 unten] [planning-Leitfaden-5.1.2] halten Sie die Hostname, SAP-SID und SAP-Benutzerkonten auf dem Azure-virtuellen Computer, wie sie in der lokalen Umgebung konfiguriert wurden. Daher ist es nicht erforderlich, die Verallgemeinern des Bilds. Finden Sie in Kapitel [Vorbereitung zum Verschieben eines virtuellen Computers zu Azure aus lokalen einem Datenträger nicht GRG] [planning-Leitfaden-5.2.1] dieses Dokuments für lokale vorbereitenden Schritte und beim Upload nicht GRG virtuellen Computern oder virtuelle Festplatten in Azure. Lesen Sie Kapitel [Szenario 3: Verschieben eines virtuellen Computers aus lokalen Verwenden einer nicht GRG Azure-virtuellen mit SAP] [Bereitstellung-Leitfaden-3.4] im [Bereitstellungshandbuch] [Bereitstellungshandbuch] die detaillierten Schritte der Bereitstellung von solche ein Bild in Azure.

#### <a name="a-namee18f7839-c0e2-4385-b1e6-4538453a285cadeploying-a-vm-with-a-customer-specific-image"></a><a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Bereitstellen eines virtuellen Computers mit einem bestimmten Kunden-image
Aufgrund von bestimmter Patch Anforderungen Workflowversion OS oder DBMS möglicherweise bereitgestellten Bilder in der Azure Marketplace nicht auf Ihre Bedürfnisse anpassen. Daher müssen Sie zum Erstellen eines virtuellen Computers verwenden eigener 'Privat' OS/DBMS virtueller Computer Bilder die danach mehrmals bereitgestellt werden kann. Zur Vorbereitung solche 'privaten' eines Bilds auf doppelte Einträge müssen Folgendes berücksichtigt werden:

___

> ![Windows][Logo_Windows] Windows
>
> Die Windows-Einstellungen (wie Windows-SID und Hostname) muss auf dem lokalen virtuellen Computer über den Befehl Sysprep abstrahiert/GRG.
[Kommentar]: <>  (MSSedusch > finden Sie weitere Details finden Sie hier:)
[Kommentar]: <> Erste Link  (MShermannd TODO geht Klassisch aus. Einen Azure Doku Artikel wurde nicht gefunden)
[Kommentar]: <>  (MSSedusch >< Https://azure.microsoft.com/documentation/articles/virtual-machines-create-upload-vhd-windows-server/>)
[Kommentar]: <>  (MSSedusch >< http://blogs.technet.com/b/blainbar/archive/2014/09/12/modernizing-your-infrastructure-with-hybrid-cloud-using-custom-vm-images-and-resource-groups-in-microsoft-azure-part-21-blain-barton.aspx>)
>
> ![Linux][Logo_Linux] Linux
>
> Führen Sie die Schritte in den folgenden Artikeln beschrieben, für [SUSE] [ virtual-machines-linux-create-upload-vhd-suse] oder [Red Hat] [ virtual-machines-linux-redhat-create-upload-vhd] zur Vorbereitung einer virtuellen in Azure hochgeladen werden muss.

___

Wenn Sie bereits SAP-Inhalte in Ihrem lokalen virtuellen Computer (besonders für Systeme der Ebene 2) installiert haben, können Sie die SAP-Systemeinstellungen anpassen, nach die Bereitstellung von den Azure-virtuellen Computer über die Instanz umbenennen Verfahren von SAP-Software Provisioning Manager (SAP-Hinweis [1619720]) unterstützt werden. Finden Sie unter Kapitel [Vorbereitung für die Bereitstellung eines virtuellen Computers mit einem bestimmten Kunden-Image für SAP] [planning-Leitfaden-5.2.2] und [Hochladen einer virtuellen aus lokalen in Azure] [planning-Leitfaden-5.3.2] dieses Dokuments für lokale vorbereitenden Schritte und Hochladen eines GRG virtuellen Computers zu Azure. Lesen Sie Kapitel [Szenario 2: Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild für SAP] [Bereitstellung-Leitfaden-3.3] im [Bereitstellungshandbuch] [Bereitstellungshandbuch] die detaillierten Schritte der Bereitstellung von solche ein Bild in Azure.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Bereitstellen eines virtuellen Computers aus dem Azure Marketplace
Sie möchten eine Microsoft verwenden oder 3rd Party bereitgestellten virtuellen Computer Bild aus dem Azure Marketplace Ihrer virtuellen Computer bereitstellen. Nachdem Sie Ihre virtuellen Computer in Azure bereitgestellt haben, führen Sie dieselben Richtlinien und Tools, die SAP-Software und/oder DBMS innerhalb Ihrer virtuellen Computer zu installieren, wie Sie in einer lokalen Umgebung tun würden. Ausführlichere Beschreibung der Bereitstellung finden Sie unter Kapitel [Szenario 1: Bereitstellen eines virtuellen Computers aus Azure Marketplace für SAP] [Bereitstellung-Leitfaden-3,2] in das [Bereitstellungshandbuch] [Bereitstellungshandbuch].

### <a name="a-name6ffb9f41-a292-40bf-9e70-8204448559e7apreparing-vms-with-sap-for-azure"></a><a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Vorbereiten virtueller Computer mit SAP für Azure
Vor dem Hochladen virtuellen Computern in Azure Sie sicherstellen müssen, zu der virtuellen Computern und virtuelle Festplatten bestimmte Vorschriften erfüllen. Es gibt kleine Unterschiede je nach der Bereitstellungsmethode, die verwendet wird. 

#### <a name="a-name1b287330-944b-495d-9ea7-94b83aff73efapreparation-for-moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a><a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Zum Verschieben eines virtuellen Computers aus lokalen in Azure einem Datenträger nicht GRG Vorbereitung
Eine allgemeine Bereitstellungsmethode ist zu einer vorhandenen virtuellen Computer verschieben, der ein SAP-System aus lokalen in Azure ausgeführt wird. Diesen virtuellen Computer und das SAP-System auf dem virtuellen Computer sollten in Azure führen Sie einfach mit der gleichen Hostname und sehr wahrscheinlich die gleiche SAP-SID. In diesem Fall sollte der Gast OS des virtuellen Computer nicht für mehrere Bereitstellungen GRG werden. Wenn das lokale Netzwerk in Azure erweitert haben (finden Sie in Kapitel [Cross lokale - Bereitstellung von einzelne oder mehrere SAP-virtuellen Computern in Azure mit dem lokalen Netzwerk vollständig integriert wird die Vorbedingung] [planning-Leitfaden-2.2] in diesem Dokument), und klicken Sie dann die gleichen Domänenkonten in den virtuellen Computer verwendet werden können, wie die lokal verwendet wurden. 

Anforderungen bei der Vorbereitung Ihrer eigenen Azure-virtuellen Computer Datenträger sind:

* Die virtuelle Festplatte mit dem Betriebssystem konnte ursprünglich nur eine maximale Größe von 127 GB haben. Diese Einschränkung haben Sie am Ende des März 2015 beseitigt. Nachdem Sie nun die virtuelle Festplatte mit dem Betriebssystem bis zu 1 TB groß sein kann, wenn andere Azure-Speicher sowie virtuelle Festplatte gehostet.
[Kommentar]: <>  (Prüfen, ob CLI auch in statischen umwandelt MShermannd erledigen müssen)
* Es muss sich in der festes Format virtuelle Festplatte befinden. Dynamische virtuelle Festplatten oder virtuellen Festplatten im Format VHDx werden auf Azure noch nicht unterstützt. Dynamische virtuellen Festplatten werden in statischen virtuellen Festplatten konvertiert, wenn Sie die virtuelle Festplatte mit PowerShell-Cmdlets oder CLI hochladen
* Virtuelle Festplatten bereitgestellt sind, um den virtuellen Computer und sollte zur werden in ein festes Format für virtuelle Festplatte als auch die virtuellen Computer müssen erneut in Azure bereitgestellt werden. Der gleiche Grenzwert für Größe des Datenträgers OS gilt auch Daten Datenträger aus. Virtuelle Festplatten können eine maximale Größe von 1 TB haben. Dynamische virtuellen Festplatten werden in statischen virtuellen Festplatten konvertiert, wenn Sie die virtuelle Festplatte mit PowerShell-Cmdlets oder CLI hochladen
* Fügen Sie ein anderes lokales Konto mit Administratorrechten die verwendet werden kann, indem Microsoft Support oder der zugeordnet werden kann, als Kontext für Dienste und Ausführung bis zur Bereitstellung des virtuellen Computer und besser geeignet Benutzer verwendet werden kann.
* Für die Groß-/Kleinschreibung von einem Bereitstellungsszenario nur Cloud-verwenden (finden Sie unter Kapitel [nur Cloud - virtuellen Computern Bereitstellungen in Azure ohne Abhängigkeiten Netzwerk Kunden lokalen] [planning-Leitfaden-2.1] dieses Dokuments) in Kombination mit diese Methode der Bereitstellung, Domänenkonten funktionieren möglicherweise nicht, sobald der Datenträger Azure in Azure bereitgestellt wird. Dies gilt besonders für Konten, die zum Ausführen von Diensten wie die DBMS oder SAP-Anwendungen verwendet werden. Daher müssen Sie solche Domänenkonten mit lokalen Konten virtueller Computer ersetzen, und löschen Sie die lokale Domänenkonten auf dem virtuellen Computer. Planmäßigen lokalen Domänenbenutzer im Bild virtueller Computer ist kein Problem, wenn der virtuellen Computer im Szenario Cross lokale bereitgestellt wird, wie in Kapitel [Cross lokale - Bereitstellung von einzelne oder mehrere SAP-virtuellen Computern in Azure mit dem lokalen Netzwerk vollständig integriert wird der Anforderung] beschrieben [planning-Leitfaden-2.2] in diesem Dokument.
* Wenn die Domänenkonten verwendet wurden als DBMS Benutzernamen oder Benutzer wann das System lokal und diesen virtuellen Computern ausgeführt werden in nur-Cloud-Szenarien bereitgestellt werden, müssen Benutzer der Domäne gelöscht werden. Sie müssen, um sicherzustellen, dass der lokale Administrator plus ein anderer virtueller Computer lokale Benutzer wird als Login/Benutzer in das DBMS als Administratoren hinzugefügt.
* Fügen Sie andere lokalen Konten aus, wie die für die von bestimmter Bereitstellungsszenario notwendig sein können.

___

> ![Windows][Logo_Windows] Windows
>
> In diesem Szenario ist keine Generalisierung (Sysprep) von den virtuellen Computer erforderlich, um hochzuladen und zu den virtuellen Computer auf Azure bereitstellen.
> Stellen Sie sicher, dass Laufwerk D:\ nicht ist festlegen Datenträger automatische Bereitstellung für verbundenen Laufwerke verwendet, wie in Kapitel [Einstellung Automatische Bereitstellung für angefügten Datenträger] beschrieben [planning-Leitfaden-5.5.3] in diesem Dokument.
> 
> ![Linux][Logo_Linux] Linux
>
> In diesem Szenario keine Generalisierung (Waagent-entziehen) von den virtuellen Computer hochladen und Bereitstellen von den virtuellen Computer auf Azure erforderlich ist.
> Stellen Sie sicher, dass/Mnt/Ressource nicht verwendet wird und alle Datenträger über Uuid bereitgestellt werden. Stellen Sie sicher, dass der Eintrag Startladeprogramm auch die Zuordnung Uuid-basierten widerspiegeln Schritte aus, für den Datenträger OS.

___

#### <a name="a-name57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3apreparation-for-deploying-a-vm-with-a-customer-specific-image-for-sap"></a><a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Vorbereitung für die Bereitstellung eines virtuellen Computers mit einem bestimmten Kunden-Image für SAP
Virtuelle Festplatte-Dateien, die eine GRG OS enthalten sind auch in Containern für Speicher-Konten Azure gespeichert. Sie können ein neues virtuellen Computers aus solchen ein Bild virtuelle Festplatte bereitstellen, indem Sie auf die virtuelle Festplatte als Quelle virtuelle Festplatte in Ihre Bereitstellung Vorlagendateien verweisen, wie in Kapitel beschrieben [Szenario 2: Bereitstellen eines virtuellen Computers durch ein benutzerdefiniertes Bild für SAP] [Bereitstellung-Leitfaden-3.3], [Bereitstellungshandbuch] [Bereitstellungshandbuch]. 

Anforderungen, wenn Sie ein eigenes Bild der Azure-virtuellen Computer vorbereiten sind:

* Die virtuelle Festplatte mit dem Betriebssystem konnte ursprünglich nur eine maximale Größe von 127 GB haben. Diese Einschränkung haben Sie am Ende des März 2015 beseitigt. Nachdem Sie nun die virtuelle Festplatte mit dem Betriebssystem bis zu 1 TB groß sein kann, wenn andere Azure-Speicher sowie virtuelle Festplatte gehostet.
[Kommentar]: <>  (Prüfen, ob CLI auch in statischen umwandelt MShermannd erledigen müssen)
* Es muss sich in der festes Format virtuelle Festplatte befinden. Dynamische virtuelle Festplatten oder virtuellen Festplatten im Format VHDx werden auf Azure noch nicht unterstützt. Dynamische virtuellen Festplatten werden in statischen virtuellen Festplatten konvertiert, wenn Sie die virtuelle Festplatte mit PowerShell-Cmdlets oder CLI hochladen
* Virtuelle Festplatten bereitgestellt sind, um den virtuellen Computer und sollte zur werden in ein festes Format für virtuelle Festplatte als auch die virtuellen Computer müssen erneut in Azure bereitgestellt werden. Der gleiche Grenzwert für Größe des Datenträgers OS gilt auch Daten Datenträger aus. Virtuelle Festplatten können eine maximale Größe von 1 TB haben. Dynamische virtuellen Festplatten werden in statischen virtuellen Festplatten konvertiert, wenn Sie die virtuelle Festplatte mit PowerShell-Cmdlets oder CLI hochladen
* Da alle Benutzer der Domäne registriert, wie Benutzer auf dem virtuellen Computer nicht in einem Szenario Cloud nur erscheint (finden Sie in Kapitel [Cloud nur - virtuellen Computern Bereitstellungen in Azure ohne Abhängigkeiten Netzwerk Kunden lokale] [planning-Leitfaden-2.1] dieses Dokuments), die Dienste, die mit solchen Domäne Konten funktionieren möglicherweise nicht, nachdem Sie das Bild in Azure bereitgestellt wird. Dies gilt besonders für Konten, die zum Ausführen von Diensten wie DBMS oder SAP-Applikationen verwendet werden. Daher müssen Sie solche Domänenkonten mit lokalen Konten virtueller Computer ersetzen, und löschen Sie die lokale Domänenkonten auf dem virtuellen Computer. Lokale Domänenbenutzer im Bild virtueller Computer planmäßigen möglicherweise kein Problem bei der virtuellen Computer im Szenario Cross lokal bereitgestellt wird, wie in Kapitel [Cross lokale - Bereitstellung von einzelne oder mehrere SAP-virtuellen Computern in Azure mit dem lokalen Netzwerk vollständig integriert wird der Anforderung] beschrieben [planning-Leitfaden-2.2] in diesem Dokument.
* Fügen Sie ein anderes lokales Konto mit Administratorrechten die verwendet werden können, um Microsoft Support bei Problem Untersuchungen oder der zugeordnet werden kann, als Kontext für Dienste und Ausführung bis zur Bereitstellung des virtuellen Computer und besser geeignet Benutzern verwendet werden kann.
* In nur-Cloud-Bereitstellungen und die Stelle, an der Domänenkonten als DBMS Benutzernamen oder Benutzer verwendet wurden, wenn das System lokal ausgeführt sollte die Domänenbenutzer gelöscht werden. Sie müssen, um sicherzustellen, dass der lokale Administrator plus ein anderer virtueller Computer lokale Benutzer als Login/Benutzer des DBMS als Administratoren hinzugefügt wird.
* Fügen Sie andere lokalen Konten aus, wie die für die von bestimmter Bereitstellungsszenario notwendig sein können.
* Wenn das Bild eine Installation von SAP NetWeaver enthält und Umbenennen von der Hostname aus dem ursprünglichen Namen zum Zeitpunkt der Azure-Bereitstellung ist zu rechnen, empfiehlt es sich um die neuesten Versionen der SAP-Software Provisioning Manager DVD in der Vorlage zu kopieren. Dadurch können Sie einfach die SAP-bereitgestellten umbenennen-Funktionalität verwenden, um die geänderte Hostname anpassen und/oder die SID des SAP-System innerhalb des bereitgestellten virtuellen Computer Bilds ändern, sobald eine neue Instanz gestartet wird.

___

> ![Windows][Logo_Windows] Windows
>
> Stellen Sie sicher, dass Laufwerk D:\ nicht ist festlegen Datenträger automatische Bereitstellung für verbundenen Laufwerke verwendet, wie in Kapitel [Einstellung Automatische Bereitstellung für angefügten Datenträger] beschrieben [planning-Leitfaden-5.5.3] in diesem Dokument.
> 
> ![Linux][Logo_Linux] Linux
>
> Stellen Sie sicher, dass/Mnt oder einer Ressource nicht verwendet wird und alle Datenträger über Uuid bereitgestellt werden. Für die OS Datenträger müssen gibt der Eintrag Startladeprogramm auch die Zuordnung Uuid-basierten wieder.

___

* SAP-Benutzeroberfläche (für administrative und Einrichtung Zwecke) kann eine solche Dokumentvorlage vorinstalliert werden.
* Andere Software erforderlich sind, um die virtuellen Computern erfolgreich in Cross lokale Szenarien ausführen kann installiert werden, solange diese Software mit Umbenennen von den virtuellen Computer arbeiten kann.

Wenn der virtuellen Computer werden generische und schließlich unabhängig von Konten/Benutzer nicht verfügbar ist, in dem Szenario zielgerichteten Azure-Bereitstellung ausreichend vorbereitet ist, wird der letzte Schritt der Vorbereitung des verallgemeinern solche ein Bild durchgeführt.

##### <a name="generalizing-a-vm"></a>Verallgemeinern eines virtuellen Computers 

___

[Kommentar]: <>  (MShermannd TODO müssen bessere Artikeln suchen / Doku zu verallgemeinern der virtuelle Computer für die Cloud)
> ![Windows][Logo_Windows] Windows
>
> Der letzte Schritt darin ist ein virtueller Computer mit einem Administratorkonto anmelden. Öffnen Sie ein Eingabeaufforderungsfenster Windows als 'Administrator' ein. Wechseln Sie zu ...\windows\system32\sysprep und führen Sie sysprep.exe aus.
> Es wird ein kleines Fenster angezeigt. Es ist wichtig, aktivieren Sie die Option 'Verallgemeinern' (die Standardeinstellung ist deaktiviertes), und ändern Sie die Option war(en) vom Standardwert 'Neustart', 'Beenden'. Dieses Verfahren wird davon ausgegangen, dass der Sysprep-Prozess ausgeführten lokal in der OS Gast eines virtuellen Computers ist.
> Wenn Sie das Verfahren mit eines virtuellen Computers bereits in Azure ausgeführt ausführen möchten, führen Sie die Schritte in [diesem Artikel]beschriebenen[virtual-machines-windows-capture-image].
> 
> ![Linux][Logo_Linux] Linux
>
> [Gewusst wie: Erfassen eines Linux virtuellen Computern als Vorlage Ressourcenmanager verwenden][virtual-machines-linux-capture-image-resource-manager]

___

### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Übertragen von virtuellen Computern und virtuellen Festplatten zwischen lokal in Azure
Da Hochladen von Bildern virtueller Computer und Datenträger in Azure nicht über das Azure-Portal möglich ist, müssen Sie Azure PowerShell-Cmdlets oder CLI verwenden. Eine weitere Möglichkeit ist die Verwendung des Tools 'AzCopy'. Das Tool kann virtuelle Festplatten zwischen lokalen und Azure (in beiden Richtungen) kopieren. Sie können auch virtuelle Festplatten zwischen Azure Regionen kopieren. Wenden Sie sich [Diese Dokumentation] [ storage-use-azcopy] für den Download und die Verwendung der AzCopy.

Eine dritte Alternative wäre verschiedener Drittanbieter Benutzeroberfläche Orientierung Tools verwenden. Jedoch, stellen Sie sicher, dass diese Tools Azure Seitenblobs unterstützt werden. Für unsere Zwecke müssen wir Azure Seitenblob-Speicher verwenden (werden hier die Unterschiede beschrieben: <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>). Die Tools von Azure sind auch sehr effizientes Komprimieren der virtuellen Computern und virtuelle Festplatten, die hochgeladen werden müssen. Dies ist wichtig, da diese Effizienz in Komprimierung die Dauer für das Hochladen verringert (also trotzdem je nach den Upload Link mit dem Internet aus der lokalen Einrichtung und der Azure-Bereitstellung Region ausgerichtet wird aktualisiert). Es ist eine ' wissenschaftsmesse ' Annahme, dass Hochladen eines virtuellen Computers oder virtuelle Festplatte aus europäischen Standort in den USA Azure Daten, die Centers dauert länger basiert als die gleichen virtuellen Computern/virtuelle Festplatten in der Europäischen Azure Data Center hochladen. 

#### <a name="a-namea43e40e6-1acc-4633-9816-8f095d5a7b6aauploading-a-vhd-from-on-premises-to-azure"></a><a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Hochladen einer virtuellen aus lokalen in Azure
Hochladen einer vorhandenen virtuellen Computers oder die virtuelle Festplatte aus dem lokalen Netzwerk solche eines virtuellen Computers oder virtuelle Festplatte muss erfüllen aufgeführt in Kapitel [Vorbereitung zum Verschieben eines virtuellen Computers aus lokalen in Azure einem Datenträger nicht GRG] [planning-Leitfaden-5.2.1] dieses Dokuments.

Ein virtuellen Computers muss nicht GRG-werden und in den Zustand und die Form, die es nach war(en) auf lokale weist hochgeladen werden kann. Dasselbe gilt für zusätzliche virtuelle Festplatten die: das Betriebssystem von einem beliebigen enthalten nicht zu. 

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Hochladen einer virtuellen und somit einen Datenträger Azure
In diesem Fall möchten wir Hochladen einer virtuellen Festplatte mit oder ohne eine OS in, und stellen Sie es eines virtuellen Computers als einen Datenträger oder als OS Festplatte. Dies ist ein Vorgang mit mehreren Schritten 

__PowerShell__

* Melden Sie sich Ihr Abonnement mit _Login-AzureRmAccount_
* Festlegen des Abonnements von Kontext mit _Set-AzureRmContext_ und Parameter SubscriptionId oder SubscriptionName – finden Sie unter <https://msdn.microsoft.com/library/mt619263.aspx>
* Hochladen der virtuellen Festplatte mit _Hinzufügen-AzureRmVhd_ mit einer Firma der Azure-Speicher – finden Sie unter <https://msdn.microsoft.com/library/mt603554.aspx>
* Festlegen des Datenträgers OS mit einer neuen virtuellen Computer Config die virtuelle Festplatte mit _Set-AzureRmVMOSDisk_ – finden Sie unter <https://msdn.microsoft.com/library/mt603746.aspx>
* Erstellen Sie einen neuen virtuellen Computer aus der Config virtueller Computer mit _Neu-AzureRmVM_ – finden Sie unter <https://msdn.microsoft.com/library/mt603754.aspx>
* Einen Datenträger hinzufügen, um einen neuen virtuellen Computer mit _Hinzufügen-AzureRmVMDataDisk_ – finden Sie unter <https://msdn.microsoft.com/library/mt603673.aspx>

__Azure CLI__

* Wechseln Sie zur Azure Ressourcenmanager Modus mit _Azure Config Modus Cloud_
* Melden Sie sich Ihr Abonnement mit _Azure login_
* Wählen Sie Ihr Abonnement mit _Azure-Konto einrichten `<subscription name or id` _
* Hochladen der virtuellen Festplatte mit _Speicher Azure Blob hochladen_ – finden Sie unter [Verwenden der Azure CLI mit Azure-Speicher][storage-azure-cli]
* Erstellen eines neuen virtuellen Computers angeben die hochgeladene virtuelle Festplatte als OS Datenträger mit _Azure-virtuellen Computer erstellen_ und Parameter -d
* Fügen Sie einen Datenträger hinzu, um einen neuen virtuellen Computer mit _virtueller Computer Datenträger anfügen-neu_

__Vorlage__

* Hochladen der virtuellen Festplatte mit Powershell oder Azure CLI
* Bereitstellen Sie den virtuellen Computer mit einer JSON-Vorlage verweisen auf die virtuelle Festplatte wie in [diesem Beispiel JSON Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-from-specialized-vhd/azuredeploy.json)angezeigt.

#### <a name="deployment-of-a-vm-image"></a>Bereitstellung von eines Bilds virtueller Computer
Aus dem lokalen Netzwerk eines vorhandenen virtuellen Computers oder die virtuelle Festplatte hochladen, damit Sie es als Bild Azure-virtuellen Computer verwenden können solche eines virtuellen Computers oder virtuelle Festplatte müssen in Kapitel [Vorbereitung für die Bereitstellung eines virtuellen Computers mit einem bestimmten Kunden-Image für SAP] aufgeführten erfüllen [planning-Leitfaden-5.2.2] dieses Dokuments.

* Verwenden von _Sysprep_ auf Windows oder _Waagent-entziehen_ finden Sie unter Linux zu Ihrem virtuellen Computer - generalize [Technische Referenz zu Sysprep](https://technet.microsoft.com/library/cc766049.aspx) für Windows oder [eine Linux virtuellen Computern als Vorlage Ressourcenmanager verwenden Aufzeichnen von] [ virtual-machines-linux-capture-image-resource-manager-capture] für Linux
* Melden Sie sich Ihr Abonnement mit _Login-AzureRmAccount_
* Festlegen des Abonnements von Kontext mit _Set-AzureRmContext_ und Parameter SubscriptionId oder SubscriptionName – finden Sie unter <https://msdn.microsoft.com/library/mt619263.aspx>
* Hochladen der virtuellen Festplatte mit _Hinzufügen-AzureRmVhd_ mit einer Firma der Azure-Speicher – finden Sie unter <https://msdn.microsoft.com/library/mt603554.aspx>
* Legen Sie den Datenträger OS mit einer neuen virtuellen Computer Config auf die virtuelle Festplatte mit _Set-AzureRmVMOSDisk - SourceImageUri - CreateOption FromImage_ -finden Sie unter <https://msdn.microsoft.com/library/mt603746.aspx>
* Erstellen eines neuen virtuellen Computers aus der Konfiguration virtueller Computer mit _Neu-AzureRmVM_ – finden Sie unter <https://msdn.microsoft.com/library/mt603754.aspx>

__Azure CLI__

* Verwenden von _Sysprep_ unter Windows oder _Waagent-entziehen_ finden Sie unter Linux zu Ihrer virtuellen Computer - generalize [Technische Referenz zu Sysprep](https://technet.microsoft.com/library/cc766049.aspx) für Windows oder [eine Linux virtuellen Computern als Ressourcenmanager Vorlage verwenden Aufzeichnen von] [ virtual-machines-linux-capture-image-resource-manager-capture] für Linux
* Wechseln Sie zur Azure Ressourcenmanager Modus mit _Azure Config Modus Cloud_
* Melden Sie sich Ihr Abonnement mit _Azure login_
* Wählen Sie Ihr Abonnement mit _Azure-Konto einrichten `<subscription name or id` _
* Hochladen der virtuellen Festplatte mit _Speicher Azure Blob hochladen_ – finden Sie unter [Verwenden der Azure CLI mit Azure-Speicher][storage-azure-cli]
* Erstellen Sie einen neuen virtuellen Computer angeben die hochgeladene virtuelle Festplatte wie OS Datenträger mit _Azure-virtuellen Computer erstellen_ und Parameter -F

__Vorlage__

* Verwenden von _Sysprep_ auf Windows oder _Waagent-entziehen_ finden Sie unter Linux zu Ihrem virtuellen Computer - generalize [Technische Referenz zu Sysprep](https://technet.microsoft.com/library/cc766049.aspx) für Windows oder [eine Linux virtuellen Computern als Vorlage Ressourcenmanager verwenden Aufzeichnen von] [ virtual-machines-linux-capture-image-resource-manager-capture] für Linux
* Hochladen der virtuellen Festplatte mit Powershell oder Azure CLI
* Bereitstellen Sie den virtuellen Computer mit einer JSON-Vorlage verweisen auf das Bild virtuelle Festplatte wie in [diesem Beispiel JSON-Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json)dargestellt.

#### <a name="downloading-vhds-to-on-premises"></a>Virtueller Festplatten auf lokale herunterladen
Azure Infrastruktur als Dienst ist nicht in eine Richtung der nur die Möglichkeit zum Hochladen von virtuellen Festplatten und SAP-Systeme. Sie können SAP verschieben Systeme aus Azure zurück, in der lokalen Welt auch.

Während die Uhrzeit des Downloads sein nicht die virtuellen Festplatten aktiv. Auch wenn virtuelle Festplatten die bereitstehen auf virtuellen Computern herunterladen, muss der virtuellen Computer beendet werden. Wenn Sie nur den Datenbankinhalt herunterladen möchten welcher dann verwendet werden soll, ein neues System einrichten lokalen und es ist zulässig, dass während des Downloads im Unternehmen sowie der Einrichtung des neuen Systems, das System in Azure trotzdem funktionsfähig sein kann, Sie eine lange Ausfallzeiten vermeiden konnte, indem Sie eine Sicherungskopie der komprimierten Datenbank in eine virtuelle Festplatte Durchführung und nur herunterladen diese virtuelle Festplatte statt auch den Basis virtueller OS Computer herunterzuladen.

#### <a name="powershell"></a>PowerShell

Nachdem das SAP-System beendet ist und der virtuellen Computer geschlossen wird, können das PowerShell-Cmdlet speichern-AzureRmVhd auf dem lokalen Ziel Sie die virtuelle Festplatte Datenträger wieder in die lokale Welt herunterladen. Um zu erreichen, dass Sie die URL der virtuellen Festplatte, die sich im ClipArt-benötigen müssen 'Speicher Abschnitt' des Portals Azure (müssen navigieren Sie zu dem Konto Speicher und den Speichercontainer, in dem die virtuelle Festplatte erstellt wurde), und Sie wissen, wohin die virtuelle Festplatte kopiert werden soll.

Klicken Sie dann können Sie den Befehl nutzen, indem Sie einfach den Parameter SourceUri als die URL der virtuellen Festplatte herunterladen und die LocalFilePath als physischen Speicherort der virtuellen Festplatte (einschließlich deren Namen) definieren. Der Befehl konnte aussehen:

```powerhell
Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
```

Weitere Informationen hierzu des Cmdlets speichern-AzureRmVhd überprüfen Sie hier <https://msdn.microsoft.com/library/mt622705.aspx>. 

#### <a name="cli"></a>CLI

Nachdem das SAP-System beendet ist und der virtuellen Computer geschlossen wird, können den Azure CLI Befehl Azure-Speicher Blob-Download auf dem lokalen Ziel Sie die virtuelle Festplatte Datenträger wieder in die lokale Welt herunterladen. Damit werden, benötigen Sie den Namen und den Container der virtuellen Festplatte die sich im Abschnitt' Speicher' des Portals Azure (müssen navigieren Sie zu dem Konto Speicher und den Speichercontainer, in dem die virtuelle Festplatte erstellt wurde), und Sie müssen wissen, wo die virtuelle Festplatte zu kopiert werden sollen.

Dann können Sie den Befehl nutzen, indem Sie definieren einfach den Parameter Blob und den Container der virtuellen Festplatte herunterladen und das Ziel als die physische-Zielort der virtuellen Festplatte (einschließlich deren Namen). Der Befehl konnte aussehen:

```
azure storage blob download --blob <name of the VHD to download> --container <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --destination <destination of the VHD to download> 
```

### <a name="transferring-vms-and-vhds-within-azure"></a>Übertragen von virtuellen Computern und virtuelle Festplatten in Azure

#### <a name="copying-sap-systems-within-azure"></a>Kopieren von SAP-Systemen innerhalb Azure

Ein SAP-System oder sogar ein dedizierter DBMS Server unterstützende eines SAP-Anwendung Layers wahrscheinlich besteht mehrere virtuelle Festplatten die entweder das Betriebssystem mit den Binärdateien oder die Daten und der Protokolldateien Datei(en) aus der SAP-Datenbank enthalten. Weder die Azure Funktionalität des Kopierens virtuelle Festplatten noch die Azure Funktionalität des Speicherns von virtuellen Festplatten auf einem Datenträger verfügt über eine Synchronisierung Verfahren der mehrerer virtueller Festplatten synchron snapshot würden. Daher würde der Status der der kopierten oder gespeicherten virtuellen Festplatten, selbst wenn die gegen den gleichen virtuellen Computer bereitstehen unterscheiden. Dies bedeutet, dass in der Beton Groß-/Kleinschreibung von gibt es andere Daten- und Logfile(s) in den verschiedenen virtuellen Festplatten enthalten sind, die Datenbank in das Ende inkonsistente würden. 

**Abschluss: Um kopieren oder Speichern virtueller Festplatten, die Teil einer SAP-Systemkonfiguration sind müssen Sie das SAP-System beenden und müssen Sie auch den bereitgestellten virtuellen Computer beenden. Nur dann können Sie den Satz von virtuellen Festplatten so erstellen Sie eine Kopie der SAP-System entweder in Azure oder lokalen herunterladen oder kopieren.**

Daten Datenträger können werden als virtuelle Festplatte Dateien in einem Azure-Speicher-Konto gespeichert und direkt Anfügen an einem virtuellen Computer oder als ein Bild verwendet werden. In diesem Fall wird die virtuelle Festplatte an einen anderen Ort vor Knotenwert angefügter des virtuellen Computers kopiert. Der vollständige Name der Datei virtuelle Festplatte in Azure muss innerhalb Azure eindeutig sein. Wie bereits zuvor erwähnt, ist der Name Art von einen dreiteiligen Namen, der aussieht: 

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

##### <a name="powershell"></a>PowerShell
Sie können Azure PowerShell-Cmdlets verwenden, um eine virtuelle Festplatte zu kopieren, wie in [diesem Artikel]dargestellt[storage-powershell-guide-full-copy-vhd].

##### <a name="cli"></a>CLI
Azure CLI können Sie um eine virtuelle Festplatte zu kopieren, wie in [diesem Artikel] dargestellt.[storage-azure-cli-copy-blobs]

##### <a name="azure-storage-tools"></a>Azure-Speicher-tools

* <http://azurestorageexplorer.codeplex.com/Releases/View/125870>

Es gibt auch professionelle Editionen von Azure-Speicher Explorern die hier gefunden werden können:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer> 


Die Kopie einer virtuellen innerhalb eines Speicher-Konto umfasst die akzeptiert nur ein paar Sekunden (vergleichbar mit SAN-Hardware schreiben Momentaufnahmen mit aus-kopieren und Kopie erstellen). Nachdem Sie eine Kopie haben der Datei virtuelle Festplatte können Sie Zuordnen eines virtuellen Computers oder virtuellen Computern Kopien der virtuellen Festplatte anfügen als Bild verwenden.

##### <a name="powershell"></a>PowerShell

```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption fromImage
$vm | Update-AzureRmVM
```
##### <a name="cli"></a>CLI
```
azure config mode arm 

# attach a vhd to a vm
azure vm disk attach <resource group name> <vm name> <path to vhd>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.
```

#### <a name="a-name9789b076-2011-4afa-b2fe-b07a8aba58a1acopying-disks-between-azure-storage-accounts"></a><a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Kopieren von Datenträger zwischen Azure-Speicher-Konten
Diese Aufgabe kann nicht auf dem Portal Azure ausgeführt werden. Sie können die Ise Azure PowerShell-Cmdlets, Azure CLI oder eine dritte Partei Browser-Speicher. Der PowerShell-Cmdlets oder CLI-Befehle können erstellen und Verwalten von Blobs, die die Möglichkeit, asynchrone Blobs über Speicher-Konten und über Bereiche innerhalb der Azure-Abonnement kopieren enthalten.

##### <a name="powershell"></a>PowerShell 

Kopieren von virtuellen Festplatten zwischen Abonnements ist möglich. Weitere Informationen finden Sie [in diesem Artikel][storage-powershell-guide-full-copy-vhd]. 

Der grundlegende Fluss PS Cmdlet Logik sieht wie folgt aus:

* Erstellen Sie einen Speicher Konto Kontext für das Konto der Quelle Speicher mit _Neu-AzureStorageContext_ – finden Sie unter <https://msdn.microsoft.com/library/dn806380.aspx>
* Erstellen Sie einen Speicher Konto Kontext für das Ziel-Speicher-Konto mit _Neu-AzureStorageContext_ – finden Sie unter <https://msdn.microsoft.com/library/dn806380.aspx>
* Starten Sie die Kopie mit

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Überprüfen Sie den Status der in einer Schleife mit Kopie
 
```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Anfügen der neuen virtuellen mit einem virtuellen Computer an, wie zuvor beschrieben.

[In diesem Artikel] finden Sie Beispiele[storage-powershell-guide-full-copy-vhd]

##### <a name="cli"></a>CLI
* Starten Sie die Kopie mit

```
  azure storage blob copy start --source-blob <source blob name> --source-container <source container name> --account-name <source storage account name> --account-key <source storage account key> --dest-container <target container name> --dest-blob <target blob name> --dest-account-name <target storage account name> --dest-account-key <target storage account name>
```

* Überprüfen Sie den Status aus, wenn die Kopie in einer Schleife mit

```
azure storage blob copy show --blob <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```
  
* Anfügen der neuen virtuellen mit einem virtuellen Computer an, wie zuvor beschrieben.

[In diesem Artikel] finden Sie Beispiele[storage-azure-cli-copy-blobs]

### <a name="disk-handling"></a>Behandlung von
#### <a name="a-name4efec401-91e0-40c0-8e64-f2dceadff646avmvhd-structure-for-sap-deployments"></a><a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Virtueller Computer/virtuelle Festplatte Struktur für SAP-Installationen.
Idealerweise sollte die Behandlung von der Struktur eines virtuellen Computers und den zugehörigen virtuellen Festplatten sehr einfach sein. Bei lokalen Installationen entwickelt Kunden verschiedenen Verwendungsmöglichkeiten eine Server-Installation zu strukturieren. 

* Eine Basis virtuelle Festplatte die enthält das Betriebssystem und alle Binärdateien des DBMS und/oder SAP. Seit März 2015 kann diese virtuelle Festplatte bis zu 1 TB groß anstelle von früheren Einschränkungen sein, auf die sie 127 GB beschränkt. 
* Eine oder mehrere virtuelle Festplatten die Protokolldatei DBMS der SAP-Datenbank und die Protokolldatei von der temporären Speicherbereich DBMS enthält (wenn das DBMS unterstützt werden). Wenn die Datenbank Log IOPS Anforderungen hoch sind, müssen Sie mehrere virtuelle Festplatten verteilen, um dem Volumen IOPS erreicht haben, um. 
* Eine Anzahl von virtuellen Festplatten mit einer oder zwei Datenbankdateien der SAP-Datenbank und die Zeichenfolge %Temp% DBMS Datendateien auch (wenn das DBMS unterstützt werden).

![Die Verweiskonfiguration von Azure Neuerung für SAP][planning-guide-figure-1300]

[Kommentar]: <>  (MShermannd erledigen beschreiben Linux Struktur)

___

> ![Windows][Logo_Windows] Windows
>
> Bei vielen Kunden gesehen haben wir Konfigurationen, wo, wurden beispielsweise SAP und DBMS Binärdateien nicht auf das Laufwerk c:\ installiert, in dem das Betriebssystem installiert wurde. Wenn wir wieder werden im Stammordner hat, wurde sie normalerweise, dass die Laufwerke small wurden und OS-Upgrades zusätzlichen Abstand vor 10 bis 15 Jahren erforderlich, aber es wurden verschiedene Ursachen geben. In beiden Fällen heutzutage nicht mehr oft auch nicht angewendet. Heute kann das Laufwerk c:\ auf große Mengen Laufwerken oder virtuellen Computern zugeordnet werden. Um Bereitstellungen in deren Struktur einfach zu halten, wird empfohlen, führen Sie das folgenden Bereitstellungsmuster für SAP NetWeaver Betriebssysteme in Azure
>
> Die Windows-Betriebssystem Auslagerungsdatei sollten sich auf Laufwerk D: (nicht beständige Datenträger) 
> 
> ![Linux][Logo_Linux] Linux
>
> Platzieren Sie den Linux verwendet unter/mnt/Mnt/Ressourcen auf Linux in [diesem Artikel]beschriebenen[virtual-machines-linux-agent-user-guide]. Die Datei austauschen kann in der Konfiguration der /etc/waagent.conf Linux-Agent konfiguriert sein. Fügen Sie hinzu oder ändern Sie die folgenden Einstellungen:

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Um die Änderungen zu aktivieren, müssen Sie mit der Linux-Agent neu starten.

```
sudo service waagent restart
```

Lesen Sie die SAP-Hinweis [1597355] Weitere Details auf die Dateigröße empfohlene austauschen

___

Die Anzahl der virtuellen Festplatten für die Datendateien DBMS und den Typ der Azure-Speicher auf diese virtuelle Festplatten gehostet werden verwendet, sollten durch die Anforderungen IOPS und der Wartezeit erforderlich bestimmt werden. Genaue Kontingente werden in [diesem Artikel] beschrieben.[virtual-machines-sizes]

Erfahrung mit SAP-Bereitstellungen den letzten 2 Jahren gezeigt uns, einige Lektionen, die als zusammengefasst werden können:

* IOPS den Datenverkehr in verschiedenen Datendateien ist nicht immer die gleiche da vorhandene Kundensysteme anders Datendateien, die ihre SAP-Datenbanken darstellt Größe haben möglicherweise. Daher ergebende es besser verwenden eine RAID-Konfiguration über mehrere virtuelle Festplatten um zu Datendateien zu platzieren, den, die LUNs aus denen darzustellen. Es wurden Situationen, insbesondere mit Standard Azure-Speicher, wo eine IOPS Zins Kontingents von einer einzelnen virtuellen gegen das DBMS Transaktionsprotokoll drücken. In solchen Szenarios empfiehlt sich die Verwendung von Premium Speicher oder alternativ Aggregieren mehrerer Standard-Speicher virtueller Festplatten mit einer Software-RAID.

___

> ![Windows][Logo_Windows] Windows
>
> * [Leistung bewährte Methoden für SQL Server in Azure virtuellen Computern][virtual-machines-sql-server-performance-best-practices]
> 
> ![Linux][Logo_Linux] Linux
>
> * [Konfigurieren von Software-RAID auf Linux][virtual-machines-linux-configure-raid]
> * [Konfigurieren Sie eine Linux virtuellen Computers in Azure LVM][virtual-machines-linux-configure-lvm]
> * [Azure Speicher Kennwörter und Linux e/a-Optimierungen](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)

___

* Premium Speicher ist signifikante eine bessere Leistung, insbesondere von kritischen Transaktionsprotokollen angezeigt. SAP-Szenarien, die Herstellung wie Leistung vorführen erwartet werden, wird dringend empfohlen, virtueller Computer-Serie zu verwenden, die Azure Premium Storage nutzen können.

Bedenken Sie, dass die virtuelle Festplatte die enthält das Betriebssystem, und es wird empfohlen, die SAP-Binärdateien und die Datenbank (Basis virtueller Computer) sowie, ist nicht mehr auf 127GB beschränkt. Sie können jetzt bis zu 1 TB Größe haben. Dies sollte genügend Speicherplatz bleiben alle etwa das SAP-Stapel Auftrag Protokolle erforderliche Datei sein.

Weitere Vorschläge und speziell für DBMS virtuellen Computern Weitere Informationen hierzu finden Sie in der [DBMS Bereitstellungshandbuch] [Dbms-Leitfadens]


#### <a name="disk-handling"></a>Behandlung von
In den meisten Fällen müssen Sie zusätzliche Datenträger erstellen, um die SAP-Datenbank in den virtuellen Computer bereitstellen. Erwähnten wir die Aspekte Anzahl der virtuellen Festplatten in Kapitel [virtueller Computer/virtuelle Festplatte Struktur für SAP-Bereitstellungen] [planning-Leitfaden-5.5.1] dieses Dokuments. Im Portal Azure ermöglicht Anfügen und trennen Datenträger, nachdem die Basis ein virtuellen Computers bereitgestellt wird. Der Datenträger können sein, angefügt/getrennt bei der virtuellen Computer aktiv ist und als auch wenn sie abgebrochen wird ausgeführt. Wenn Sie einen Datenträger anhängen, stellt das Azure-Portal fügen Sie einen leeren Datenträger oder einen vorhandenen Datenträger, die zu diesem Zeitpunkt nicht an einem anderen virtuellen Computer angeschlossen ist. 

**Hinweis**: virtuelle Festplatten können nur zu einem beliebigen Zeitpunkt zu einem virtuellen Computer angefügt werden.
 
![Fügen Sie an / trennen Sie Datenträger mit standardmäßigen Azure-Speichers][planning-guide-figure-1400]

Sie müssen entscheiden, ob Sie zum Erstellen einer neuen und leeren virtuellen (die im selben Speicherkonto als Basis, virtueller Computer befindet, erstellt würde) oder gibt an, ob Sie eine vorhandene virtuelle auszuwählen, die zuvor hochgeladen wurde und jetzt auf den virtuellen Computer angefügt werden soll. 

**Wichtig**: **Nicht** Zwischenspeichern Host mit standardmäßigen Azure-Speicher verwenden möchten. Sie sollten die Einstellung Host Cache der Standardwert keine belassen. Mit Azure Premium Speicher sollten Sie lesen Zwischenspeichern aktivieren, wenn die e/a-Eigenschaft wie normale e/a-Datenverkehr mit den Daten Datenbankdateien hauptsächlich gelesen wird. Bei der Datenbank Transaktionsprotokolldatei empfiehlt sich kein Zwischenspeichern.

___

> ![Windows][Logo_Windows] Windows
>
> [So fügen Sie einen Datenträger im Azure-Portal][virtual-machines-linux-attach-disk-portal]
>
> Wenn der Datenträger angeschlossen sind, müssen Sie melden Sie sich in den virtuellen Computer, um den Windows-Datenträger-Manager zu öffnen. Wenn die automatische Bereitstellung nicht aktiviert ist, wie in Kapitel [Einstellung Automatische Bereitstellung für angefügten Datenträger] empfohlen [planning-Leitfaden-5.5.3], der neu angefügte Datenträger online und initialisierte absolviert werden muss.
>
> ![Linux][Logo_Linux] Linux
>
> Wenn Datenträger angeschlossen sind, müssen Sie melden Sie sich in den virtuellen Computer und Initialisierung der Datenträger aus, wie in [diesem Artikel] beschrieben.[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]

___

Ist der Datenträger neue einen leeren Datenträger, müssen Sie auch den Datenträger zu formatieren. Wenden Sie zum Formatieren von Daten besonders für DBMS Daten und Protokolldateien die gleichen Empfehlungen wie für softwareunabhängige Bereitstellungen des DBMS.

Wie bereits in Kapitel [der Microsoft Azure virtuellen Computern Konzept] [planning-Leitfaden-3,2] erwähnt, ein Konto Azure-Speicher bietet keine unbegrenzte Ressourcen im Hinblick auf e/a-Volumen, IOPS und Daten Lautstärke. DBMS virtuellen Computern sind in der Regel am häufigsten von diesem betroffen. Möglicherweise sollten Sie ein separates Speicher-Konto für jeden virtuellen Computer verwenden, wenn Sie einige hoher e/a-Volume virtuellen Computern bereitstellen, damit bleiben innerhalb der Obergrenze des Datenträgers Azure-Speicher-Konto verfügen. Andernfalls müssen Sie sehen, wie Sie diese virtuellen Computern zwischen verschiedenen Speicher-Konten ohne Berührung Limit jedes einzelne Speicherkonto gegeneinander abwägen können. Weitere Details werden im [DBMS Bereitstellungshandbuch] [Dbms-Leitfadens] erläutert. Sie sollten auch orientieren diese Einschränkungen für reines SAP-Anwendung Server virtuellen Computern oder anderen virtuellen Computern wofür später zusätzliche virtuelle Festplatten erforderlich ist.

Ein anderes Hilfethema also für Speicher-Konten von Bedeutung ist, ob die virtuellen Festplatten in einem Konto Speicher Geo repliziert abrufen. Geo-Replikation ist aktiviert oder deaktiviert werden, klicken Sie auf der Ebene Speicher-Konto und nicht auf der Ebene virtueller Computer. Wenn Geo-Replikation aktiviert ist, sollte die virtuellen Festplatten innerhalb der Speicher-Konto in ein anderes Azure Datacenter innerhalb derselben Region repliziert werden. Bevor Sie sich entscheiden, klicken Sie auf diese, sollten Sie die folgenden Einschränkung anzustellen:

Azure Geo-Replikation lokal auf jede virtuelle Festplatte im eines virtuellen Computers funktioniert und repliziert IOs chronologisch nicht über mehrere virtuelle Festplatten in einen virtuellen Computer. Die virtuelle Festplatte, die den Basis virtueller Computer darstellt als auch für alle zusätzlichen virtuellen Festplatten, die den virtuellen Computer angefügt werden daher unabhängig voneinander repliziert. Dies bedeutet, dass es besteht keine Synchronisierung zwischen Änderungen in der anderen virtuellen Festplatten. Die Fakultät, unabhängig von der Reihenfolge der IOs repliziert werden in der sie bedeutet, dass geschrieben ist die Geo-Replikation nicht der Werte für Datenbankserver, auf denen ihre Datenbanken verteilt über mehrere virtuelle Festplatten. Zusätzlich zu den DBMS möglicherweise gibt es auch andere Programme, wo Prozesse schreiben oder Bearbeiten von Daten in anderen virtuellen Festplatten und wo ist es wichtig, die Reihenfolge der Änderungen beibehalten. Wenn dies erforderlich ist, sollte Geo-Replikation in Azure nicht aktiviert. Abhängige auf, ob Sie müssen oder möchten Geo-Replikation für eine Reihe von virtuellen Computern, aber nicht für einen weiteren Satz, können Sie bereits virtuellen Computern und die zugehörigen virtuellen Festplatten in verschiedenen Speicher-Konten kategorisieren Geo-Replikation aktiviert oder deaktiviert.

#### <a name="a-name17e0d543-7e8c-4160-a7da-dd7117a1ad9dasetting-automount-for-attached-disks"></a><a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Automatische Bereitstellung für angefügten Datenträger festlegen

___


> ![Windows][Logo_Windows] Windows
> 
> Für virtuelle Computer, die aus der eigenen Bilder oder Datenträger erstellt wurden, ist es erforderlich, um zu überprüfen, und legen Sie den Parameter automatische Bereitstellung oftmals. Wenn dieser Parameter ermöglicht den virtuellen Computer für die erneute Bereitstellung in Azure, die angefügt-bereitgestellten Laufwerke erneut automatisch bereitzustellen oder Neustart. 
> Der Parameter ist für die Bilder von Microsoft in der Azure Marketplace bereitgestellten festgelegt.
>
> Um die automatische Bereitstellung festlegen möchten, in der Dokumentation der von der Befehlszeile um ausführbare diskpart.exe hier: 
> 
> * [Befehlszeilenoptionen für DiskPart](https://technet.microsoft.com/library/cc766465.aspx)
> * [Automatische Bereitstellung](http://technet.microsoft.com/library/cc753703.aspx)
> 
> Als Administrator sollte das Windows-Befehlszeile-Fenster geöffnet werden.
> 
> Wenn der Datenträger angeschlossen sind, müssen Sie melden Sie sich in den virtuellen Computer, um den Windows-Datenträger-Manager zu öffnen. Wenn automatische Bereitstellung nicht aktiviert ist, wie in Kapitel [Einstellung Automatische Bereitstellung für angefügten Datenträger] empfohlen [planning-Leitfaden-5.5.3], die neu angefügten Lautstärke > online und initialisierte absolviert werden muss.
>
> ![Linux][Logo_Linux] Linux
>
> Sie benötigen einen leeren neu angefügten Datenträger Initialisierung in [diesem Artikel]beschriebenen[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Sie müssen außerdem die/etc/fstab neue Festplatten hinzufügen.

___


### <a name="final-deployment"></a>Endgültige Bereitstellung
Für die endgültige Bereitstellung und die genauen Schritte, insbesondere im Hinblick auf die Bereitstellung von SAP-erweiterten Überwachung Lizenzinformationen finden Sie im [Bereitstellungshandbuch] [Bereitstellungshandbuch].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Zugriff auf SAP-Systeme, die innerhalb der Azure-virtuellen Computern ausgeführt
Für nur Cloud-Szenarien empfiehlt es sich über das Internet über SAP-Oberfläche eine Verbindung zu diesen SAP-Systemen. In diesen Fällen müssen die folgenden Verfahren angewendet werden soll.

Später im Dokument werden erläutert das wichtige Szenario Herstellen einer Verbindung mit SAP-Systeme in Cross lokale Bereitstellungen denen eine Standort-zu-Standort-Verbindung (VPN-Tunnel) oder Azure ExpressRoute Verbindung zwischen dem lokalen Betriebssysteme und Azure Systemen.


### <a name="remote-access-to-sap-systems"></a>Remote-Zugriff auf SAP-Betriebssysteme

Ressourcenmanager mit Azure es gibt keine standardmäßigen Endpunkte nicht mehr wie in der vorherigen Option Klassisch aus. Alle Ports von einer Azure Cloud virtueller Computer geöffnet sind, solange:

1. Für das Subnetz oder der Schnittstelle ist keine Sicherheitsgruppe Netzwerk definiert. Netzwerkverkehr zu Azure-virtuellen Computern kann über so genannte "Netzwerk-Sicherheitsgruppen" gesichert werden. Weitere Informationen finden Sie unter [was einem Netzwerk Sicherheit Gruppe (NSG) ist?][virtual-networks-nsg]
1. Keine Azure Lastenausgleich ist für die Netzwerkschnittstelle definiert.   
 
Architektur unterscheiden sich klassisch und Cloud finden Sie in [diesem Artikel]beschriebenen[virtual-machines-azure-resource-manager-architecture].
 
#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Konfiguration von der SAP-System und SAP-Benutzeroberfläche Konnektivität für nur Cloud-Szenario
Finden Sie in diesem Artikel werden die Details zu diesem Thema beschreibt: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx> 

#### <a name="changing-firewall-settings-within-vm"></a>Ändern die Firewalleinstellungen in virtueller Computer
Möglicherweise müssen Sie zum Konfigurieren der Firewall auf Ihrem virtuellen Computern an, damit eingehenden Datenverkehr an Ihre SAP-System werden. 

___

> ![Windows][Logo_Windows] Windows
>
> Windows-Firewall in einer Azure bereitgestellten virtuellen Computer ist standardmäßig aktiviert. Möchten Sie jetzt ermöglichen, den SAP-Anschluss geöffnet werden soll, andernfalls wird der SAP-Benutzeroberfläche wird keine Verbindung hergestellt werden.
> Zweck
>
>  * Öffnen Steuerelement Panel\System und Security\Windows Firewall auf 'Erweiterte Einstellungen".
>  * Jetzt mit der rechten Maustaste auf eingehende Regeln, und wählen Sie "Neue Regel".
>  * In den folgenden Assistenten wählen zum Erstellen einer neuen Regel für 'Port'.
>  * Lassen Sie im nächsten Schritt des Assistenten die Einstellung bei TCP- und Typ den Port-Nummer, die Sie öffnen möchten. Da unsere SAP-Instanz-ID 00 ist, erstellen wir 3200 an. Wenn Ihre Instanz eine anderen Instanz Zahl enthält, sollten Ihnen definierten Port einer früheren Version auf der Grundlage der Instanz Anzahl geöffnet werden.
>  * Im nächsten Teil des Assistenten müssen Sie das Element 'Zulassen Verbindung' ausgecheckt zu lassen.
>  * Im nächsten Schritt des Assistenten müssen Sie festlegen, ob die Regel für die Domäne, Private und öffentliche Netzwerk angewendet wird. Wenden passen sie bei Bedarf an Ihre Bedürfnisse an. Jedoch müssen Herstellen einer Verbindung mit SAP-Benutzeroberfläche, von außen über das öffentliche Netzwerk, die die Regel, die mit dem öffentlichen Netzwerk angewendet haben.
>  * Im letzten Schritt des Assistenten müssen Sie benennen Sie der Regel und speichern Sie dann die Regel durch Drücken von 'Fertig stellen'
>
>  Die Regel wird sofort wirksam.
>
> ![Anschluss Regeldefinition][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Linux Bilder in der Azure Marketplace nicht die Firewall Iptables standardmäßig aktivieren, und die Verbindung zu Ihrem SAP-System arbeiten soll. Wenn Sie Iptables oder eine andere Firewall aktiviert haben, finden Sie in der Dokumentation Iptables oder verwendeten Firewall, um zuzulassen eingehenden TCP-Datenverkehr an Port 32xx (Xx ist, in dem das System Anzahl Ihrer SAP-System). 

___

#### <a name="security-recommendations"></a>Sicherheit Empfehlungen

Der SAP-Benutzeroberfläche nicht sofort an das SAP-Instanzen (Port 32xx) eine Verbindung herzustellen, die ausgeführt werden, aber zuerst erfolgen soll, über den Port für den SAP-Nachricht Serverprozess (Port 36xx) geöffnet. In der Vergangenheit wurde sehr denselben Port für den internen Kommunikation mit der Anwendungsinstanzen von dem e-Mail-Server verwendet. Um zu verhindern, dass lokale Anwendungsserver aus versehentlich Kommunikation mit einem Nachrichtenserver in Azure die interne Kommunikationsports geändert werden können. Es wird dringend empfohlen, die interne Kommunikation zwischen dem SAP-e-Mail-Server und deren Anwendungsinstanzen in eine andere Port-Nummer Betriebssystemen ändern möchten, das aus lokalen bedienen, beispielsweise eine datenbeschriftungsreihe der Entwicklung für Project testen usw. kopiert wurde haben. Dies kann mit den standardmäßigen Profilparameter erfolgen:

>   Rdisp/msserv_internal

wie in erläutert: <https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm> 

## <a name="a-name96a77628-a05e-475d-9df3-fb82217e8f14aconcepts-of-cloud-only-deployment-of-sap-instances"></a><a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Konzepte der Cloud nur Bereitstellung von SAP-Instanzen

### <a name="a-name3e9c3690-da67-421a-bc3f-12c520d99a30asingle-vm-with-sap-netweaver-demotraining-scenario"></a><a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Einzelne virtueller Computer mit SAP NetWeaver Demo-Schulung-Szenario
 
![In die Cloud Services Azure isoliert Betriebssystem der einzelnen virtuellen Computer SAP-Demo mit denselben Namen virtueller Computer,][planning-guide-figure-1700]

In diesem Szenario (siehe Kapitel [nur Cloud] [planning-Leitfaden-2.1] dieses Dokuments) implementieren wir eine typische Schulung-Demo System Szenario, in dem das vollständige Schulung-Demo Szenario innerhalb eines einzelnen virtuellen Computers enthalten ist. Angenommen, dass für die Bereitstellung erfolgt über Bildvorlagen. Angenommen auch mehrere der folgenden Demo/Training virtuellen Computern mit den virtuellen Computern mit demselben Namen bereitgestellt werden müssen.

Wird davon ausgegangen, dass Sie ein Bild virtueller Computer erstellt haben, wie beschrieben in einigen Abschnitten Kapitels [Vorbereiten virtueller Computer mit SAP für Azure] [planning-Leitfaden-5.2] in diesem Dokument.

Die Abfolge von Ereignissen zum Implementieren des Szenarios sieht wie folgt aus:

[Kommentar]: <>  (MShermannd TODO Cloud Stichprobe bereitstellen müssen / Beschreibung mithilfe von Json-Vorlage + Erläuterung bezüglich eindeutigen Namen des virtuellen Computers in der Cloud virtuelles Netzwerk)   
##### <a name="powershell"></a>PowerShell

* Erstellen einer neuen Resoure Gruppe für jede Schulung/Demo Querformat

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```

* Erstellen eines neuen Kontos mit Speicher

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Erstellen Sie ein neues virtuelles Netzwerk für jede Schulung/Demo Querformat So aktivieren Sie die Verwendung der gleichen Hostname und IP-Adressen ein. Das virtuelle Netzwerk ist durch eine Sicherheitsgruppe Netzwerk geschützt, die nur den Datenverkehr in Port 3389 zum Aktivieren des Remote Desktop-Zugriffs und den Port 22 SSH ermöglicht. 

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Erstellen Sie eine neue öffentliche IP Adresse, die zum zugreifen, die Internet des virtuellen Computers verwendet werden können

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Erstellen einer neuen Netzwerk-Oberfläche des virtuellen Computers

```powershell 
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip 
```

* Erstellen eines virtuellen Computers an. Für die Cloud nur Szenario wird jeder virtuellen Computer denselben Namen haben. Die SAP-SID SAP NetWeaver Instanzen in diesen virtuellen Computern wird ebenfalls übereinstimmen. Innerhalb der Ressourcengruppe Azure der Namen der virtuellen Computer muss eindeutig sein, aber in unterschiedlichen Azure Ressourcengruppen Sie virtuellen Computern mit demselben Namen ausführen können. Das 'Administrator' Standardkonto von Windows oder "Root" für Linux sind nicht zulässig. Daher muss eine neue Administratorbenutzernamen zusammen mit einem Kennwort definiert werden. Die Größe der virtuellen Computer muss ebenfalls definiert werden.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES" -Skus "12" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="os"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"
$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Optional können Sie weitere Datenträger hinzufügen und Wiederherstellen von Inhalten erforderlich. Denken Sie daran, dass alle Blob Namen (URLs, die Blobs) in Azure eindeutig sein müssen.

```powershell
# Optional: Attach additional data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM
```

##### <a name="cli"></a>CLI

Im folgenden Beispielcode kann unter Linux verwendet werden. Wenden Sie für Windows sich bitte verwenden Sie PowerShell oben beschriebenen oder anpassen im Beispiel zum Verwenden von "% RgName" statt $rgName und legen Sie die Umgebungsvariable mit der Windows-Befehl _festlegen_.

* Erstellen einer neuen Resoure Gruppe für jede Schulung/Demo Querformat

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
azure group create $rgName "North Europe"
```

* Erstellen eines neuen Kontos mit Speicher

```
azure storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku-name LRS $rgNameLower
```

* Erstellen Sie ein neues virtuelles Netzwerk für jede Schulung/Demo Querformat So aktivieren Sie die Verwendung der gleichen Hostname und IP-Adressen ein. Das virtuelle Netzwerk ist durch eine Sicherheitsgruppe Netzwerk geschützt, die nur den Datenverkehr in Port 3389 zum Aktivieren des Remote Desktop-Zugriffs und den Port 22 SSH ermöglicht. 

```
azure network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

azure network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
azure network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group-name SAPERPDemoNSG
```

* Erstellen Sie eine neue öffentliche IP Adresse, die zum zugreifen, die Internet des virtuellen Computers verwendet werden können

```
azure network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --domain-name-label $rgNameLower --allocation-method Dynamic
```

* Erstellen einer neuen Netzwerk-Oberfläche des virtuellen Computers

```
azure network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-name SAPERPDemoPIP --subnet-name Subnet1 --subnet-vnet-name SAPERPDemoVNet 
```

* Erstellen eines virtuellen Computers an. Für die Cloud nur Szenario wird jeder virtuellen Computer denselben Namen haben. Die SAP-SID SAP NetWeaver Instanzen in diesen virtuellen Computern wird ebenfalls übereinstimmen. Innerhalb der Ressourcengruppe Azure der Namen der virtuellen Computer muss eindeutig sein, aber in unterschiedlichen Azure Ressourcengruppen Sie virtuellen Computern mit demselben Namen ausführen können. Das 'Administrator' Standardkonto von Windows oder "Root" für Linux sind nicht zulässig. Daher muss eine neue Administratorbenutzernamen zusammen mit einem Kennwort definiert werden. Die Größe der virtuellen Computer muss ebenfalls definiert werden.

```
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn SUSE:SLES:12:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn RedHat:RHEL:7.2:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
#azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
```

* Optional können Sie weitere Datenträger hinzufügen und Wiederherstellen von Inhalten erforderlich. Achten Sie darauf, dass alle Blob Namen (URLs auf die Blobs) in Azure eindeutig sein müssen.

```
# Optional: Attach additional data disks
azure vm disk attach-new --resource-group $rgName --vm-name SAPERPDemo --size-in-gb 1023 --vhd-name datadisk
```

##### <a name="template"></a>Vorlage
Sie können auf das Repository Azure-Schnellstart-Vorlagen auf Github die Beispielvorlagen.

* [Einfache Linux virtueller Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Einfache Windows virtueller Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Virtueller Computer Bilds](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Implementieren Sie eine Reihe von virtuellen Computern, die in Azure kommunizieren müssen
Dieses Szenario Cloud nur ist ein typische Szenario für Schulung und Demo Zwecke Where der Software, die die Demo-Schulung darstellt Szenario über mehrere virtuelle Computer verteilen. Die verschiedenen Komponenten, die in den anderen virtuellen Computern installiert müssen miteinander kommunizieren. Erneut in diesem Szenario keine lokalen Netzwerk-Kommunikation oder übergreifend lokale Szenario erforderlich ist.

Dieses Szenario ist eine Erweiterung der Installation beschrieben Kapitel [virtueller einzelne Computer mit SAP NetWeaver Demo-Schulung-Szenario] [planning-Leitfaden-7.1] dieses Dokuments. In diesem Fall wird eine vorhandene Ressourcengruppe mehr virtuelle Computer hinzugefügt werden. Im folgenden Beispiel besteht aus den Schulung Querformat eines SAP-ASCS/SCS virtueller Computer, eines virtuellen Computers ein DBMS und eine Instanz der SAP-Anwendungsserver virtuellen Computer ausgeführt.

Bevor Sie dieses Szenario erstellen müssen Sie grundlegende Einstellungen wie bereits in dem Szenario vor geltend gemacht anzustellen.

#### <a name="resource-group-and-virtual-machine-naming"></a>Ressourcengruppe und Benennen von virtuellen Computern
Alle Ressourcennamen Gruppe müssen eindeutig sein. Entwickeln Sie ein eigenes naming Schema Ressourcen, z. B. `<rg-name`>-Suffix. 

Name des virtuellen Computers muss innerhalb der Ressourcengruppe eindeutig sein. 

#### <a name="setup-network-for-communication-between-the-different-vms"></a>Setup-Netzwerk für die Kommunikation zwischen den verschiedenen virtuellen Computern
 
![Festlegen von virtuellen Maschinen innerhalb einer Azure virtuelles Netzwerk][planning-guide-figure-1900]

Um zu verhindern, dass Namenskonflikten mit Klonen von der gleichen Schulung/Demo Landschaften, müssen Sie ein Azure-virtuellen Netzwerk für jede Querformat zu erstellen. DNS-namensauflösung erhalten Sie von Azure, oder Sie können Ihre eigenen DNS-Server außerhalb Azure (nicht in der nachfolgend ausführlicher behandelt) konfigurieren. In diesem Szenario konfigurieren wir nicht unsere eigenen DNS-Einträge. Für alle virtuellen Computern innerhalb einer Azure-virtuellen Netzwerk wird die Kommunikation über Hostnamen aktiviert sein. 

Gründe zum Trennen von Schulung oder Demo Landschaften durch virtuelle Netzwerke und nicht nur Ressourcengruppen sind möglich:

* Die SAP-Querformat als einrichten benötigt einen eigenen AD/OpenLDAP und Teil der einzelnen der Landschaften sein muss ein Server für die Domäne.  
* Die SAP-Querformat wie eingerichtet hat Komponenten, die für die Arbeit mit festen IP-Adressen müssen.

Weitere Details zu Azure virtuelle Netzwerke und die Definition finden Sie in [diesem Artikel][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Bereitstellen von SAP-virtuellen Computern mit dem Firmennetzwerk Connectivity (Cross lokale)

Ausführen eines SAP-Querformat, und die Bereitstellung zwischen softwareunabhängige für High-End-DBMS-Servern teilen möchten, lokalen virtualisierten Umgebungen für Anwendung Layer und kleinere 2 Ebene konfiguriert SAP-Systeme und Azure IaaS. Basis ausgegangen ist, dass SAP-Systemen innerhalb einer SAP-Querformat miteinander und viele andere im Unternehmen, unabhängig von deren Bereitstellung Formular bereitgestellt Software-Komponenten kommunizieren müssen. Es sollte keine Unterschiede formularbasierten Bereitstellung eingeführt werden, für den Endbenutzer Herstellen einer Verbindung mit SAP-Benutzeroberfläche oder eine andere Schnittstellen auch sein. Nur können diese Bedingung erfüllt sein, wenn wir die lokalen Active Directory/OpenLDAP und DNS-Dienste über an die Azure Systeme Website-zu-Website/multi vor Ort Konnektivität oder private Verbindungen wie Azure ExpressRoute erweitert haben.

Um weitere Hintergrundinformationen zu den Details der Implementierung von SAP auf Azure zu erhalten, wir empfehlen Ihnen, lesen Sie Kapitel [Konzepte der Cloud-Only Bereitstellung von SAP-Instanzen] [planning-Leitfaden-7] dieses Dokuments die erklärt, einige der Konstrukte Grundlagen von Azure und wie diese mit SAP-Anwendungen in Azure verwendet werden soll.

### <a name="scenario-of-a-sap-landscape"></a>Szenario der SAP-Umgebung

Das Cross lokale Szenario kann ungefähr wie in den folgenden Grafiken beschrieben werden:
 
![Website-zu-Standort-Konnektivität zwischen lokalen und Azure-Ressourcen][planning-guide-figure-2100]

Das oben dargestellte Szenario wird ein Szenario beschrieben, in dem der lokalen AD/OpenLDAP und erweiterten DNS-Einträge in Azure. Klicken Sie auf der Seite lokale ist ein bestimmten Bereich von IP pro Abonnement Azure reserviert. Ein Azure-virtuellen Netzwerk Azure auf wird der Bereich der IP-Adresse zugewiesen werden.

#### <a name="security-considerations"></a>Zur Sicherheit

Die Mindestanforderungen ist die Verwendung von sichere Kommunikationsprotokolle wie SSL/TLS für den Zugriff über einen Webbrowser oder VPN-basierten Verbindungen für Systemzugriff auf die Azure Dienste. Ausgegangen ist, dass Unternehmen die VPN-Verbindung zwischen dem Firmennetzwerk und Azure sehr anders verarbeitet werden. Einige Unternehmen möglicherweise blankly alle Ports öffnen. Einige andere Unternehmen sollten sehr präzise in welche Ports werden benötigten usw. zu öffnen. 

In der Tabelle unter normalen SAP werden Kommunikationsports aufgeführt. Im Wesentlichen ist es ausreichend, den SAP-Gateway Anschluss zu öffnen.

| Dienst | Portnamen | Beispiel für `<nn`> = 01 | Standardbereich (min-Max.) | Kommentar |
|---------|-----------|-------------------|-------------------------|---------|
| Verteiler | Sapdp`<nn>` finden Sie unter * | 3201 | 3200 – 3299 | SAP-Verteiler, die vom SAP-Benutzeroberfläche für Windows und Java verwendet wird |
| E-Mail-server | Sapms`<sid`> finden Sie unter ** | 3600. | kostenlose sapms`<anySID`> | SID = SAP-System-ID |
| Gateway | Sapgw`<nn`> finden Sie unter * | 3301 | frei | SAP-Gateway, für die CPIC und RFC Kommunikation |
| SAP-router | sapdp99 | 3299 | frei | Nach der Installation können nur CI (zentrale Instanz)-Dienstnamen in/etc/Services auf einen beliebigen Wert zugewiesen. |

*) Nn = Anzahl der SAP-Instanz

*) Sid = SAP-System-ID

Weitere Informationen zur Ports erforderlich für verschiedene SAP-Produkte oder Dienste von SAP-Produkten finden Sie hier <http://scn.sap.com/docs/DOC-17124>. Mit dieser sollte Sie dedizierte Ports im erforderlichen für bestimmte SAP-Produkte und Szenarien VPN-Gerät öffnen können.

Andere Sicherheitsmaßnahmen beim Bereitstellen von virtuellen Computern der Fall, zum Erstellen einer [Sicherheitsgruppe Netzwerk sein kann] [ virtual-networks-nsg] zum Definieren von Access-Regeln.

### <a name="dealing-with-different-virtual-machine-series"></a>Umgang mit anderen virtuellen Computern Reihe

Im Verlauf der letzten 12 Monate Microsoft viele weitere virtueller Computer Diagrammtypen, die entweder in die Anzahl der vCPUs, Arbeitsspeicher unterscheiden hinzugefügt oder wichtiger auf Hardware es auf ausgeführt wird. Nicht alle diese virtuellen Computern werden unterstützt, mit SAP (Typen im SAP-Hinweis [1928533]finden Sie unter Unterstützte virtueller Computer). Einige dieser virtuellen Computern ausführen auf anderen Host Hardware Generationen. Diese Host Hardware Generationen werden in der Reihenfolge der ein Azure Mengen-Einheit bereitgestellt abrufen. Bedeutet, dass Fällen auftreten, in dem die verschiedenen virtuellen Computer Größen, die Sie ausgewählt haben auf den gleichen Mengen-Einheit ausgeführt werden können. Legen Sie eine Verfügbarkeit ist in die Möglichkeit, auf der Grundlage der anderen Hardware Maßeinheiten erstrecken begrenzt.  Z. B. Wenn Sie das DBMS A5 A11 virtuellen Computern und auf die SAP-Anwendungsebene auf G-Serie virtuellen Computern ausführen möchten, möchten Sie ein einzelnes SAP-System oder anderen SAP-Systeme in unterschiedliche Verfügbarkeit bereitstellen wird.


#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Drucken auf einem lokalen Netzwerkdrucker aus SAP-Instanz in Azure
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Drucken über TCP/IP in Cross lokale Szenario


Einrichten Ihrer lokalen TCP/IP Grundlage Netzwerkdrucker ein Azure-virtuellen Computer ist insgesamt mit dieselben wie in Ihr Unternehmensnetzwerk, unter der Voraussetzung, dass Sie eine VPN-Standort-zu-Standort Tunnel oder ExpressRoute Verbindung hergestellt haben. 

___

> ![Windows][Logo_Windows] Windows
>
> Zweck
> - Einige Netzwerkdrucker im Zusammenhang mit einer Kontokonfigurations-Assistenten, der sodass sie einfach zu Ihren Drucker ein Azure-virtuellen Computer eingerichtet ist. Wenn keine Software Assistenten mit dem Drucker verteilt wurde ist die "manuelle" Methode zum Einrichten des Druckers zum Erstellen einer neuen Drucker.
> - Öffnen Control Panel -> Geräte und Drucker -> Drucker hinzufügen 
> - Wählen Sie einen Drucker mit einem TCP/IP Adresse oder Hostname hinzufügen
> - Geben Sie die IP-Adresse des Druckers
> - Drucker Port 9100 standard
> - Bei Bedarf installieren Sie den entsprechenden Druckertreiber manuell. 
> 
> ![Linux][Logo_Linux] Linux
>
> - wie für Windows nur folgen das Standardverfahren Installieren eines Netzwerkdruckers
> - Befolgen Sie die öffentlichen Linux Führungslinien für [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) oder [Red Hat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) , zum Hinzufügen eines Druckers.

___

 
![Drucken im Netzwerk][planning-guide-figure-2200]



##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Server-basierte Drucker über SMB (freigegebener Drucker) in Cross lokale Szenario

Host-basierte Drucker sind nicht Netzwerk-kompatiblen standardmäßig. Aber ein Host-basierten Drucker kann zwischen Computern in einem Netzwerk freigegeben werden, solange Sie der Drucker mit einem Computer eingeschaltet verbunden ist. Verbinden Ihrer corporate network entweder zwischen Standorten oder ExpressRoute und Freigeben Ihrer lokalen Druckers. Das SMB-Protokoll verwendet NetBIOS anstelle von DNS-Einträge als Namensdienst an. Der NetBIOS-Hostname kann von der DNS-Hostname abweichen. Der standard Fall ist, dass der NetBIOS-Hostname und der DNS-Hostname identisch sind. Die DNS-Domäne ist im Namespace NetBIOS nicht sinnvoll. Entsprechend, muss der vollqualifizierte DNS-Hostnamen aus dem DNS-Hostname und DNS-Domäne besteht nicht in dem Bereich der NetBIOS-Namen verwendet werden.

Die Druckerfreigabe wird durch einen eindeutigen Namen im Netzwerk gekennzeichnet:

* Hostname des Hosts SMB (immer erforderlich). 
* Name der Freigabe (immer erforderlich). 
* Namen der Domäne, wenn der Drucker freigeben ist, nicht in der gleichen Domäne wie SAP-System. 
* Darüber hinaus möglicherweise einen Benutzernamen und ein Kennwort Druckerfreigabe Zugriff auf erforderlich sein.

So wird es gemacht:

___

> ![Windows][Logo_Windows] Windows
>
> Freigeben Sie Ihrer lokalen Druckers.
> Öffnen Sie auf dem Azure-virtuellen Computer im Windows-Explorer, und geben Sie den Freigabenamen des Druckers aus.
> Ein Drucker Installationsassistenten führt Sie durch den Installationsvorgang.
>
> ![Linux][Logo_Linux] Linux
>
> Hier sind einige Beispiele zum Konfigurieren von Netzwerk Drucken in Linux oder ein Kapitel einschließlich Dokumentation zum Drucken in Linux. Es wird die gleiche Weise wie in einer Azure Linux virtueller Computer funktionieren, solange der virtuellen Computer Teil eines VPN ist:
>
> * SLES <_Share_or_Windows_Share Https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba)>
> * RHEL <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-printing-smb-printer.html>

___


##### <a name="usb-printer-printer-forwarding"></a>USB-Drucker (Drucker Weiterleitung) 

In Azure ist die Möglichkeit der Remote Desktop-Dienste, die Benutzern den Zugriff auf den lokalen Drucker Geräten in einer remote-Sitzung bereitstellen, nicht verfügbar.

___

> ![Windows][Logo_Windows] Windows
>
> Weitere Informationen zum Drucken mit Windows finden Sie hier: <http://technet.microsoft.com/library/jj590748.aspx>.

___

 
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integration von SAP Azure Systeme in Korrektur und Transportsystem (TMS) in Cross lokale

Die SAP-ändern und Transport System (TMS) muss zum Exportieren und Importieren von Transport Anforderung über Systeme in das Querformat konfiguriert sein. Angenommen, dass für die Entwicklung Instanzen von ein SAP-System (Entwickler) in Azure befinden, während die Qualität Assurance (f & a) und produktiv Systeme (PRD) lokal sind. Darüber hinaus wird davon ausgegangen, dass ein Verzeichnis zentralen Transport vorhanden ist.

##### <a name="configuring-the-transport-domain"></a>Konfigurieren die Domäne Transport
Konfigurieren Sie Ihre Domäne Transport auf das System aus, das Sie als Transport Domain Controller in [den Transport Domänencontroller konfigurieren](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm)beschriebenen festgelegt. Ein Systembenutzer, die, den TMSADM erstellt wird, und die erforderlichen RFC Ziel werden generiert. Sie können diesen RFC Verbindungen mit der Transaktion SM59 überprüfen. Hostname Auflösung muss über Ihre Domäne Transport aktiviert sein. 

So wird es gemacht:

* In diesem Szenario entschieden wir, dass das lokale QAS System den CTS Domänencontroller fungieren soll. Rufen Sie Transaktion STMS. Das Dialogfeld TMS angezeigt wird. Klicken Sie im Dialogfeld konfigurieren Transport Domäne wird angezeigt. (Dieses Dialogfeld wird nur angezeigt, wenn Sie eine Domäne Transport noch nicht konfiguriert haben.)
* Stellen Sie sicher, dass der Benutzer automatisch erstellte TMSADM berechtigt ist (SM59-Verbindung ABAP > -> TMSADM@E61.DOMAIN_E61 -> Details -> Utilities(M) -> Autorisierung Test). Im ersten Bildschirm Transaktion sollte STMS zeigen an, dass diese SAP-System jetzt als Controller der Domäne Transport fungiert, wie hier dargestellt:
 
![Ersten Bildschirm Transaktion STMS auf dem Domänencontroller][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Einschließlich SAP-Systeme in der Domäne Transport

Die Reihenfolge der einschließlich ein SAP-System in einer Transportregel-Domäne sieht wie folgt aus:

* Wechseln Sie zu der Transportregel-System (000-Client), und rufen Sie Transaktion STMS, Entwickler System in Azure. Wählen Sie im Dialogfeld Weitere Konfiguration aus, und fahren mit System in Domäne enthalten. Geben Sie die Domänencontroller als Ziel-Host ([Einschließlich SAP-Systeme in der Domäne Transport](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)) an. Das System ist jetzt warten, in der Domäne Transport aufgenommen werden sollen.
* Aus Gründen der Sicherheit müssen Sie dann zu der Domänencontroller zur Bestätigung Ihrer Anfrage zurückzukehren. Wählen Sie Systemüberblick und Genehmigen des Systems warten. Bestätigen Sie die Aufforderung und die Konfiguration verteilt.

Dieser SAP-System enthält jetzt die erforderlichen Informationen über alle anderen SAP-Systeme in der Domäne Transport aus. Zur gleichen Zeit die Adressdaten der neuen SAP-System werden auf alle anderen SAP-Systeme gesendet, und das SAP-System wird eingegeben in das Profil Transport des Programms Transportregel-Steuerelement. Überprüfen Sie, ob RFCs und den Zugriff auf das Verzeichnis Transport der Domäne arbeiten.

Fahren Sie mit der Konfiguration von Ihrem Transportsystem wie gewohnt, wie in der Dokumentation [ändern und Transport System](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)beschrieben.

So wird es gemacht:

* Stellen Sie sicher, dass Ihre STMS lokal richtig konfiguriert ist.
* Stellen Sie sicher, dass der Hostname des dem Transport Domänencontroller von Ihrem virtuellen Computern auf Azure und umgekehrt Visa aufgelöst werden kann.
* Anruf Transaktion STMS -> andere Konfiguration -> System in Domäne enthalten.
* Bestätigen Sie die Verbindung im auf lokale TMS System.
* Konfigurieren Sie Transport weitergeleitet, Gruppen und Ebenen wie gewohnt ein.

In-Standorten verbundenen Cross lokale Szenarien kann die Wartezeit zwischen lokalen und Azure weiterhin wesentlichen sein. Wenn wir führen Sie die Reihenfolge der Übermittlung von Objekten durch Entwicklung und Testen der Herstellung Systeme oder Denken Sie an Transport anwenden oder Pakete auf verschiedene Systeme unterstützt, einreichen Sie an, dass, hängt von den Speicherort der zentralen Transport Verzeichnis, einige Systeme Wartezeit lesen oder Schreiben von Daten im Verzeichnis zentralen Transport auftreten. Die Situation ist ähnlich dem SAP-Querformat Konfigurationen, wo sind die verschiedenen Systeme durch andere Daten Mittelpunkt mit erheblichen Abstand zwischen der Data Center verteilt.

Um solche Wartezeit umgehen und verfügen über die Systeme funktionieren schnelles in lesen und Schreiben in den oder aus dem Verzeichnis Transport können Sie die Einrichtung transportieren zwei STMS Domänen (eine lokale und eine mit den Systemen in Azure und die Transport Domänen verknüpfen. Überprüfen Sie diese Dokumentation die die Prinzipien dieses Konzept in SAP TMS erläutert: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>. 

So wird es gemacht:

* Einrichten einer Transportregel-Domäne auf jedem Standort (lokal und Azure) Transaktion STMS <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm> verwenden
* Verknüpfen Sie die Domänen mit einem Link der Domäne ein, und bestätigen Sie die Verknüpfung zwischen den beiden Domänen. 
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Verteilen Sie die Konfiguration an das verknüpfte System.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC Datenverkehr zwischen SAP-Instanzen in Azure und lokalen (Cross lokale)

RFC Datenverkehr zwischen Systeme, die lokal sind und in Azure muss arbeiten. Um eine Verbindung Anruf Transaktion SM59 in einem Quellsystem einrichten, Sie eine Verbindung RFC in Richtung des Systems Ziel definieren müssen. Die Konfiguration ist ähnlich wie die Standardeinstellungen eine Verbindung RFC.

Angenommen, die im Szenario Cross lokale der virtuellen Computern ausführen SAP-Systeme, die miteinander kommunizieren müssen in der gleichen Domäne sind. Daher unterscheidet sich die Einrichtung einer Verbindung RFC zwischen SAP-Systemen nicht aus dem Setupschritte und Eingaben im lokalen Szenarien.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Beim Zugriff auf 'lokale' Dateifreigaben von SAP-Instanzen in Azure oder umgekehrt

SAP-Instanzen in Azure ansässig Dateifreigaben zugreifen, die innerhalb der corporate lokale werden müssen. Darüber hinaus müssen lokale SAP-Instanzen Dateifreigaben zugreifen, die sich im Azure befinden. So aktivieren Sie die Dateifreigaben, müssen Sie die Berechtigungen und Freigabeoptionen auf dem lokalen System konfigurieren. Vergewissern Sie sich, um die Ports klicken Sie auf die Option VPN oder ExpressRoute Verbindung zwischen Azure und Datencenters zu öffnen.

## <a name="supportability"></a>Unterstützung
### <a name="a-name6f0a47f3-a289-4090-a053-2521618a28c3aazure-monitoring-solution-for-sap"></a><a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Überwachen der Lösung für SAP Azure
Um die Überwachung von Auftrag kritische SAP-Systemen auf Azure der Überwachungstools SAPOSCOL oder SAP-Host Agent Abrufen von Daten aus dem Azure-virtuellen Computerdienst Host über eine Azure-Erweiterung für die Überwachung für SAP SAP zu aktivieren. Da der Auslastung SAP spezieller auf Applikationen SAP wurden, möchte Microsoft keinen generisch Implementieren der erforderlichen Funktionen in Azure, es jedoch weiterhin für Kunden ihre virtuellen Computern in Azure notwendigen Überwachung Komponenten und Konfigurationen bereitstellen. Bereitstellung und Lifecycle Management Überwachung Komponenten wird jedoch hauptsächlich Azure automatisierten werden.

#### <a name="solution-design"></a>Lösungsentwurf

Die Lösung entwickelt, um die SAP-Überwachung aktivieren basiert auf der Architektur der Azure-virtuellen Computer-Agents und Erweiterung Framework. Die Idee von Azure-virtuellen Computer-Agents und eine Durchwahl Framework besteht darin, die Installation der Software Anwendung(en) verfügbar im Katalog Azure-virtuellen Computer-Erweiterung innerhalb eines virtuellen Computers zu ermöglichen. Die Prinzip Idee dieses Konzept besteht darin, die Bereitstellung von speziellen Funktionen in eines virtuellen Computers und die Konfiguration zum Zeitpunkt der Bereitstellung von Software (in Fällen wie Azure Überwachung Erweiterung für SAP) zu ermöglichen. 

Seit Februar 2014 ist die 'Azure virtueller Computer Agent', die das Behandeln von bestimmten Azure-virtuellen Computer-Erweiterungen innerhalb der virtuellen Computer ermöglicht in Windows-virtuellen Computern standardmäßig bei der Erstellung virtueller Computer, im Portal Azure eingefügt. Bei SUSE oder Red Hat Linux den virtuellen Computer ist Agent bereits Teil des Bilds Azure Marketplace. Für den Fall, dass eine eine Linux VM aus lokalen in Azure hochladen möchten weist des virtuellen Computer-Agents manuell installiert werden.


Die grundlegenden Bausteine der Überwachung-Lösung in Azure für SAP-sieht folgendermaßen aus:
 
![Microsoft Azure-Erweiterung Komponenten][planning-guide-figure-2400]

Wie in der obigen Blockdiagramm dargestellt, wird ein Teil der Überwachung Lösung für SAP gehostet wird in der Abbildung der Azure-virtuellen Computer und Azure Erweiterung Katalog also ein Repository global repliziert, die von Azure Vorgänge verwaltet wird. Es ist den Zuständigkeitsbereich die gemeinsame SAP/MS-Team über die Azure Durchführung des Arbeitens mit Azure Operationen neuere Versionen der Azure Überwachung Erweiterung für SAP-veröffentlichen SAP arbeiten. Diese Azure Überwachung Erweiterung für SAP wird die Erweiterung Microsoft Azure Diagnose (WAD) oder Linux Azure Diagnose (LAD) verwendet, können Sie um die erforderlichen Informationen zu gelangen. 

Wenn Sie einen neuen Windows virtuellen Computer bereitstellen, wird 'Azure virtueller Computer Agent' in den virtuellen Computer automatisch hinzugefügt. Die Funktion dieses Agents besteht darin, das Laden und Konfiguration der Azure Extensions für die Überwachung von SAP NetWeaver Systemen koordinieren. Für Linux virtuelle Computer ist der Azure-virtuellen Computer-Agent bereits Teil des Bilds Azure Marketplace OS.

Es gibt jedoch ein Schritt, der weiterhin vom Kunden ausgeführt werden soll. Dies ist die Aktivierung und Konfiguration der Leistungssammlung. Die im Zusammenhang mit der 'Konfiguration' wird durch einen PowerShell-Skript oder CLI-Befehl automatisch. PowerShell-Skript kann im Microsoft Azure Script Center in der [Bereitstellungshandbuch] [Bereitstellungshandbuch] beschriebenen heruntergeladen werden.

Die generelle die Überwachung Azure-Lösung für SAP-Architektur sieht wie folgt aus:
 
![Überwachen-Lösung für SAP NetWeaver Azure][planning-guide-figure-2500]

**Anweisungen Sie für die genauen Anleitungen und die detaillierten Schritte der Verwendung dieser PowerShell-Cmdlets oder CLI-Befehl während Bereitstellungen im [Bereitstellungshandbuch] [Bereitstellungshandbuch] angegeben.**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integration von Azure befindet SAP-Instanz in SAProuter

SAP-Instanzen in Azure ausgeführt müssen sowie vom SAProuter zugänglich sein.
 
![SAP-Router-Verbindung][planning-guide-figure-2600]

Eine SAProuter ermöglicht die TCP/IP Kommunikation zwischen teilnehmenden Systeme, wenn keine direkte IP-Verbindung besteht. Auf diese Weise den Vorteil, dass keine End-to-End-Verbindung zwischen den Partnern Kommunikation auf Netzwerkebene benötigt wird. Die SAProuter hört standardmäßig Port 3299.
SAP-Instanzen durch eine SAProuter zum Verbinden müssen Sie den SAProuter Zeichenfolge und Host Namen mit jeder Versuch Verbindung zuzuweisen.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java

Der Fokus des Dokuments bisher SAP NetWeaver im allgemeinen oder die SAP NetWeaver ABAP Stapeln. In diesem Abschnitt kleine werden bestimmte Aspekte für den SAP-Java-Stapel aufgeführt. Eines der wichtigsten SAP NetWeaver Java ausschließlich aufgrund Applikationen ist Enterprise-SAP-Portal. Andere SAP NetWeaver Applikationen basiert, wie SAP-PI und SAP-Lösung-Manager die SAP NetWeaver ABAP und die Java Stapel verwenden. Es natürlich also eine bestimmte Aspekte, die im Zusammenhang mit dem SAP NetWeaver Java Stapel sowie berücksichtigen müssen.

### <a name="sap-enterprise-portal"></a>SAP-Enterprise-Portal

Das Setup von einer SAP-Portal in einer Azure-virtuellen Computern unterscheidet aus einer auf lokale Installation sich nicht bei der Bereitstellung in Cross lokale Szenarien. Da die DNS-Einträge von lokal ausgeführt wird, können den Port zu der einzelnen Instanzen als konfigurierten lokalen vorgenommen werden. Die Empfehlungen und Einschränkungen, die bisher in diesem Dokument beschriebenen gelten im Allgemeinen für eine Anwendung wie SAP Enterprise Portal oder den Stapel SAP NetWeaver Java verfügbar. 

![Dargestellte SAP-Portal][planning-guide-figure-2700]

Ein Szenario spezielle Bereitstellung von einigen Kunden ist die direkte Offenlegung des Enterprise-SAP-Portals mit dem Internet, während der Host virtuellen Computern mit dem Firmennetzwerk über Standort-zu-Standort VPN-Tunnel oder ExpressRoute verbunden ist. Für den Fall sein müssen Sie sicherstellen, dass bestimmte Ports öffnen und nicht gesperrten durch Sicherheitsgruppe Firewall oder Netzwerk befinden. Die gleiche Funktionsweise müssten angewendet werden, wenn Sie zu einer SAP-Java-Instanz aus lokalen in einem Szenario Cloud nur herstellen möchten.

Das ursprüngliche Portal URI ist HTTP(s)://:`<Portalserver`>: 5XX00/Irj der Port ist, indem Sie 50000 Pluszeichen (Systemnumber x 100) formatiert. Ist das standardmäßige Portal URI von SAP-System 00 `<dns name`>. `<azure region`>.Cloudapp.azure.com:PublicPort/irj. Weitere Informationen hierzu Informationen über eine <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>. 
 
![Endpunktkonfiguration][planning-guide-figure-2800]

Wenn Sie die URL-Adresse und/oder Ports von Ihrer SAP-Enterprise-Portal anpassen möchten, überprüfen Sie diese Dokumentation:

* [Ändern des Portals URL](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL) 
* [Standard-Portnummern Portal Portnummern ändern](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers) 


## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Hohe Verfügbarkeit (HA) und Disaster Wiederherstellung (DR) für SAP NetWeaver auf Azure virtuellen Computern ausgeführt
### <a name="definition-of-terminologies"></a>Definition der Terminologie

Der Begriff **hohen Verfügbarkeit** im Allgemeinen bezieht sich auf eine Reihe von Technologien, die IT-Ausbluten minimiert durch die Bereitstellung von IT-Diensten über Geschäftskontinuität redundante, Fehlertoleranz oder Failover geschützte Komponenten innerhalb der **gleichen** Data Center. In diesem Fall innerhalb einer Azure Region.

**Wiederherstellung (DR)** ist auch minimieren Unterbrechung von IT-Dienste und deren Wiederherstellung verwendet, aber über **unterschiedliche** Daten befinden Centers, die in der Regel werden hundert Kilometer abwesend. In diesem Fall in der Regel zwischen verschiedenen Azure Regionen innerhalb derselben geopolitische Region oder als von Ihnen als Kunde definierte.

### <a name="overview-of-high-availability"></a>Übersicht der hohen Verfügbarkeit
Wir können die Diskussion zur hohen Verfügbarkeit von SAP in Azure in zwei Teile zu trennen:

* **Hohe Verfügbarkeit Azure Infrastruktur**, z. B. HA der berechnen (virtuellen Computern) Netzwerk, Speicher usw. und deren Vorteile zum Erhöhen der Verfügbarkeit von SAP-Anwendung.
* **Hohe Verfügbarkeit von SAP-Anwendung**, z. B. HA von SAP-Software-Komponenten:
    * SAP-Anwendungsserver
    * SAP-ASCS/SCS Instanz 
    * Datenbankserver

und wie sie mit Azure Infrastruktur HA kombiniert werden kann.

SAP-hohen Verfügbarkeit in Azure enthält einige Unterschiede im Vergleich zu SAP-hohe Verfügbarkeit in einer lokalen physische oder virtuelle Umgebung. Das folgende Papier von SAP werden standard hohen Verfügbarkeit von SAP-Konfigurationen in einer virtualisierten Umgebung unter Windows: <http://scn.sap.com/docs/DOC-44415>. Es ist keine Sapinst integriert SAP-AHA Konfiguration für Linux wie Windows vorhanden ist. Hinsichtlich SAP-HA lokal für Linux finden Sie hier weitere Informationen: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure Infrastruktur hohen Verfügbarkeit
Es steht keine Single-virtuellen Computer Vereinbarung zum SERVICELEVEL auf Azure virtuellen Computern sofort. Um eine Vorstellung zu erhalten, wie die Verfügbarkeit eines einzelnen virtuellen Computers möglicherweise aussehen, wie Sie das Produkt der verschiedenen verfügbaren Azure SLAs einfach erstellen können: <https://azure.microsoft.com/support/legal/sla/>.

Basis für die Berechnung ist 30 Tage pro Monat oder 43200 Minuten. 0,05 % Ausfallzeiten daher entspricht 21,6 Minuten. Wie gewohnt, wird die Verfügbarkeit von den anderen Diensten in der folgenden Weise multiplizieren:

Verfügbarkeit (Service #1/100) *(Verfügbarkeit Service #2/100)* (Verfügbarkeit Service #3/100) *...

Erste Möglichkeit:

(99,95/100) *(99,9/100)* (99,9/100) = 0.9975 oder eine allgemeine Verfügbarkeit von 99.75 %.

#### <a name="virtual-machine-vm-high-availability"></a>Hohe Verfügbarkeit von virtuellen Computern (virtueller Computer)

Es gibt zwei Arten von Ereignissen Azure-Plattform, die die Verfügbarkeit von Ihren virtuellen Computern beeinflussen können: geplante Wartung und nicht geplante Wartung.

* Geplante Wartung Ereignisse sind periodisch Aktualisierungen von Microsoft zur zugrunde liegenden Azure-Plattform zur Verbesserung der allgemeinen Zuverlässigkeit, Leistung und Sicherheit der Plattforminfrastruktur, die Ihren virtuellen Computern ausgeführt werden, klicken Sie auf.
* Nicht geplante Wartung Ereignisse auftreten, wenn die Hardware oder physische Infrastruktur zugrunde liegenden Ihres virtuellen Computers in irgendeiner Weise fehlerhaft ist. Dies kann lokales Netzwerkfehlern, bei der lokalen Festplatte oder andere den Shapes für Gestelle Ebene Fehler enthalten. Wenn solche ein Fehler erkannt wird, wird die Azure-Plattform des virtuellen Computers automatisch aus dem fehlerhaften physischen Server Hosten Ihrer virtuellen Computern mit einem fehlerfrei physischen Server migrieren. Solche Ereignisse sind selten, aber möglicherweise auch dazu führen, dass Ihre virtuellen Computern, neu zu starten.

Weitere Informationen hierzu finden Sie in dieser Dokumentation: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure Speicherredundanz

Die Daten in Ihrem Microsoft Azure-Speicher-Konto ist immer repliziert, um Zuverlässigkeit und hohe Verfügbarkeit, der Azure-Speicher Vereinbarung zum SERVICELEVEL auch unter vorübergehende Hardware-Fehlern Besprechung sicherzustellen

Da Azure-Speicher 3 Bilder Daten standardmäßig halten, sind RAID5 oder RAID1 auf mehrere Azure Datenträger nicht erforderlich.

Weitere Informationen hierzu finden Sie in diesem Artikel: <http://azure.microsoft.com/documentation/articles/storage-redundancy/> 

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Die Nutzung Azure Infrastruktur virtueller Computer neu starten, um "Höhere Verfügbarkeit" von SAP Anwendungen zu erzielen.

Wenn Sie nicht verwenden möchten Funktionen wie Windows Server Failover Cluster (WSFC) oder ein Linux entspricht (letztere, die eine noch nicht auf Azure in Kombination mit SAP-Software unterstützt wird), Azure virtueller Computer neu gestartet wird verwendet, um einen SAP-System vor geplanten und ungeplanten Ausfall eines der Azure physischen Server-Infrastruktur und generelle zugrunde liegenden Azure-Plattform zu schützen. 
 
> [AZURE.NOTE] Es ist wichtig, erwähnen, dass virtuellen Computern und nicht Applikationen Azure virtueller Computer neu gestartet hauptsächlich geschützt werden. Virtueller Computer neu gestartet bietet keine hohen Verfügbarkeit für SAP-Applikationen, aber es bietet ein gewisses Infrastruktur Verfügbarkeit und daher indirekt "höhere Verfügbarkeit" SAP-Systeme. Es gibt auch keine Vereinbarung zum SERVICELEVEL für die Zeit, die einen virtueller Computer neu starten, nachdem ein geplanten oder ungeplanten Host Ausfall dauert. Daher eignet sich diese Methode 'hoher Verfügbarkeit' nicht für wichtige Komponenten aus einem SAP-System wie (A) SCS oder DBMS.

Ein anderes wichtige Infrastrukturelement hohen Verfügbarkeit ist Speicher. Z. B. Azure-Speicher Vereinbarung zum SERVICELEVEL ist Verfügbarkeit von 99,9 %. Wenn eine bereitstellt alle virtueller Computer mit seine Laufwerke in einem einzelnen Azure Speicher-Benutzerkonto potenzieller Azure Storage Ausfall unterbindet Ausfall des alle virtuellen Computern, die in diesem Konto der Azure-Speicher abgelegt werden, und auch alle SAP-Komponenten innerhalb dieser virtuellen Computern ausführen.  

Statt mit allen virtuellen Computern in einer einzelnen Azure-Speicher-Konto, können Sie auch dedizierten Speicher Konten für jeden virtuellen Computer, und klicken Sie auf diese Weise vergrößern allgemeine Verfügbarkeit der virtuellen Computer und SAP-Anwendung mit mehreren unabhängigen Azure Speicher-Konten. 

Eine Stichprobe Architektur eines Systems, das Azure Infrastruktur verwendet SAP NetWeaver konnte HA wie folgt aussehen:
 
![Die Nutzung Azure Infrastruktur HA SAP-Anwendung "höheren" Verfügbarkeit erzielen.][planning-guide-figure-2900]

Für wichtige SAP-Komponenten erreicht wir die folgenden bisher:

* Hohe Verfügbarkeit der SAP-Anwendungsserver (AS)

Instanzen von SAP-Anwendung Server sind redundante Komponenten. Jede als Instanz auf einem eigenen virtuellen Computer bereitgestellt wird, die in einem anderen Azure Fehlerstrukturanalyse und Aktualisierung Domäne läuft SAP (finden Sie unter Kapitel [Fehlerstrukturanalyse Domänen] [planning-Leitfaden-3.2.1] und [Domains][planning-guide-3.2.2]) aktualisieren. Dies wird sichergestellt, indem Sie Azure Verfügbarkeit Symbolsätze (siehe Kapitel [Azure Verfügbarkeit Sets][planning-guide-3.2.3]). Mögliche geplanten oder ungeplanten Ausfall eines Azure Fehlerstrukturanalyse oder Aktualisierung Domäne bewirkt, dass nicht verfügbar sind, eine eingeschränkte Anzahl von virtuellen Computern mit ihrer SAP-AS Instanzen. Jede als Instanz wird in einem eigenen Konto Azure-Speicher platziert – potenzieller Ausfall des eine Azure-Speicherkonto bewirkt, dass Ausfall des virtuellen nur einen Computer mit seiner SAP AS SAP Instanz. Beachten Sie jedoch, dass maximal Azure-Speicherkonten innerhalb einer Azure-Abonnement vorhanden ist. Stellen Sie sicher Autostart Parameter (A) SCS Instanz Start-Profil in Kapitel [mithilfe von Autostart für SAP-Instanzen] beschriebenen festlegen, um automatischen Start der Buchstabe A genannten SCS Instanz nach dem Neustart virtueller Computer sicherzustellen, [planning-Leitfaden-11.5].
Sie können auch nachlesen Kapitel [hohen Verfügbarkeit für SAP-Anwendungsserver] [planning-Leitfaden-11.4.1] Weitere Details.

* _Höhere_ Verfügbarkeit von SAP-(A) SCS Instanz
 
Hier können wir nutzen, Azure virtueller Computer neu starten, um den virtuellen Computer mit installierten SAP (A) SCS Instanz schützen. Bei geplanten oder ungeplanten Ausfall eines Azure trennt, virtuellen Computern wird auf einem anderen verfügbaren Server neu gestartet werden. Wie zuvor schon erwähnt, Loss Azure virtueller Computer neu gestartet hauptsächlich virtuellen Computern und NOT Applications in diesem Fall (A) SCS Instanz. Durch die virtuellen Computer neu gestartet werden wir "höhere Verfügbarkeit" SAP-(A) SCS Instanz indirekt erreicht haben. Stellen Sie sicher Autostart Parameter (A) SCS Instanz Start-Profil in Kapitel [mithilfe von Autostart für SAP-Instanzen] beschriebenen festlegen, um den automatischen Start der Buchstabe A genannten SCS Instanz nach dem Neustart virtueller Computer stellen Sie sicher, [planning-Leitfaden-11.5]. Dies bedeutet der SCS (A) als einzelne Punkt Instanz der Fehler (SPOF) auf einem einzelnen virtuellen Computer ausgeführt werden der maßgeblichen Faktor für die Verfügbarkeit von die gesamte SAP-Umgebung. 

* _Höhere_ Verfügbarkeit von DBMS-Server

Hier, ähnlich wie der Groß-/Kleinschreibung von SAP-(A) SCS Instanz verwenden, wir nutzen Azure virtueller Computer neu starten, um den virtuellen Computer mit installierten DBMS-Software schützen, und wir erzielen "höhere Verfügbarkeit" des DBMS-Software durch virtueller Computer neu gestartet. DBMS ausgeführt wird, in einen einzelnen virtuellen Computer ist ebenfalls ein SPOF, und es wird der maßgeblichen Faktor für die Verfügbarkeit von der gesamten SAP-Querformat. 

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP-Anwendung hohen Verfügbarkeit auf Azure IaaS
Um eine vollständigen SAP-System hohen Verfügbarkeit zu erzielen, müssen wir, alle kritische SAP-Systemkomponenten, z. B. redundante SAP-Anwendungsserver, schützen und eindeutige Komponenten (z. B. einzelne Punkt der Fehler) wie SAP (A) SCS Instanz und DBMS. 

#### <a name="a-name5d9d36f9-9058-435d-8367-5ad05f00de77ahigh-availability-for-sap-application-servers"></a><a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Hohe Verfügbarkeit für SAP-Anwendungsserver
Für die SAP-Anwendung Servern/Dialogfeld Instanzen ist es nicht erforderlich, eine bestimmte hohen Verfügbarkeit Lösung anzustellen. Hohe Verfügbarkeit wird durch Redundanz und dadurch auch einfach erreicht genug haben in anderen virtuellen Computern Probleme. Beide sollten alle in den gleichen Azure Verfügbarkeit so einrichten, vermeiden platziert werden, dass die virtuellen Computern möglicherweise zur gleichen Zeit während der geplanten Wartung Ausfallzeiten aktualisiert werden. Die grundlegende Funktionen der verschiedenen Upgrade und Fehlerstrukturanalyse-Domänen innerhalb einer Azure Mengen Einheit erstellt wurde bereits in Kapitel [Domänen Upgrade] [planning-Leitfaden-3.2.2] eingeführt werden. Azure Verfügbarkeit Datensätze wurden in Kapitel [Azure Verfügbarkeit Sätze] [planning-Leitfaden-3.2.3] dieses Dokuments angezeigt. 

Es gibt keine unbegrenzte Anzahl von Fehlern und Upgrade-Domänen, die von einer Azure Verfügbarkeit festlegen innerhalb einer Azure Mengen Einheit verwendet werden kann. Dies bedeutet, dass mit einer Anzahl von virtuellen Computern in einer Verfügbarkeit festlegen früher oder später in der Fakultät, die mehrere virtueller Computer in derselben Domäne aktualisieren oder Fehlerstrukturanalyse, nach oben endet

Bereitstellen von wenigen SAP-Anwendung Serverinstanzen in deren dedizierten virtuellen Computern und unter der Voraussetzung, dass wir 5 Upgrade Domänen erhalten haben, tritt die folgende Abbildung am Ende aus. Die tatsächliche maximale Anzahl von Fehlerstrukturanalyse und Aktualisieren von Domänen innerhalb eines Satzes Verfügbarkeit ändert sich möglicherweise in der Zukunft:
 
![HA SAP-Anwendungsserver in Azure][planning-guide-figure-3000]

Weitere Informationen hierzu finden Sie in dieser Dokumentation: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>


#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>Hohe Verfügbarkeit für die Instanz SCS SAP (A) unter Windows

Windows Server Failover Cluster (WSFC) ist eine häufig verwendete Lösung für die Instanz SCS SAP (A) zu schützen. Es wird auch in Sapinst in Form von "HA Installation" integriert. Zu diesem Zeitpunkt kann die Azure-Infrastruktur nicht zu ermöglichen die erforderlichen Windows Server-Failovercluster genauso einrichten als es lokale erledigt wurde.

Ab Januar 2016 bietet die Betriebssystems Windows Azure-Cloud-Plattform nicht die Möglichkeit verwenden eines freigegebenen Clustervolume auf einem Datenträger zwischen zwei Azure-virtuellen Computern freigegeben.

Eine gültige Lösung ist jedoch die Verwendung von Drittanbietern 3rd Software mit einem gemeinsam genutzten Volume durch Replikation synchroner und transparente Festplatten mit in WSFC integriert werden kann. Dieser Ansatz setzt voraus, dass nur der aktiven Cluster-Knoten auf einer der Datenträger Kopien zu einem bestimmten Zeitpunkt zugreifen kann. Ab Januar 2016 dieser HA wird Konfiguration unterstützt, um die Instanz SCS SAP (A) auf Windows-Gast OS auf Azure-virtuellen Computern in Kombination mit der Software 3rd Hersteller SIOS DataKeeper schützen.

Die Lösung SIOS DataKeeper bietet die Ressource eines freigegebenen Datenträgers Cluster Windows Failoverclustern durch Probleme:

* Eine weitere Azure-virtuellen Festplatte zu den einzelnen virtuellen Computern (virtuelle Computer), die in einer Windows-Konfiguration sind angefügt
* SIOS DataKeeper Cluster Edition auf beiden Knoten virtueller Computer ausgeführt
* Probleme SIOS DataKeeper Cluster Edition so, dass es synchron den Inhalt der zusätzlichen virtuellen Festplatte spiegelt konfiguriert angefügt angefügten Lautstärke aus der Quelle virtuellen Computern auf Weitere virtuellen Festplatte Lautstärke des Ziels virtueller Computer.
* SIOS DataKeeper ist Abstrahierung der Quell- und Zielwebsites lokale Datenträger und Präsentieren von auf Windows-Failovercluster als einzelne freigegeben.
 
Alle Details finden Sie unter So installieren Sie ein Windows-Failovercluster mit SIOS Datakeeper und SAP im [Cluster SAP-ASCS Instanz mithilfe von Windows Server-Failovercluster auf Azure mit SIOS DataKeeper] [ ha-guide-classic] Whitepaper. 

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Hohe Verfügbarkeit für die SAP-(A) SCS-Instanz auf Linux
 
Zum Zeitpunkt Dez 2015 gibt es auch keine Entsprechung auf freigegebenen Datenträger WSFC für Linux virtuelle Computer auf Azure. Alternative Lösungen mit 3rd von Software wie SIOS für Windows werden noch nicht zum Ausführen von SAP auf Linux auf Azure überprüft.



#### <a name="high-availability-for-the-sap-database-instance"></a>Hohe Verfügbarkeit für die Instanz des SAP-Datenbank
Standard SAP-DBMS HA-Setups basiert auf zwei DBMS virtuellen Computern, wo DBMS hoher Verfügbarkeit-Funktionen verwendet wird, Daten von der aktiven DBMS Instanz auf den zweiten virtuellen Computer in eine passive DBMS Instanz repliziert.

Hohe Verfügbarkeit und Disaster Wiederherstellung Funktionalität für DBMS im Allgemeinen sowie spezielle werden DBMS im [DBMS Bereitstellungshandbuch] [Dbms-Leitfadens] beschrieben.


#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>End-to-End-hohe Verfügbarkeit für die vollständige SAP-System

Hier sind zwei Beispiele für eine vollständige SAP NetWeaver HA Architektur in Azure - eines für Windows und eines für Linux.
Die Konzepte wie unten beschrieben müssen möglicherweise etwas beeinträchtigt werden, wenn Sie viele SAP-Systeme bereitstellen und die Anzahl der virtuellen Computern bereitgestellt werden das maximale Limit der Speicherkonten pro Abonnement überschreiten. In diesem Fall müssen virtuelle Festplatten von virtuellen Computern in einem Speicherkonto kombiniert werden. Normalerweise würden Sie dies tun, durch Kombinieren der virtuellen Festplatten von SAP-Anwendung Layer virtuellen Computern von verschiedenen SAP-Systemen.  Wir kombiniert auch andere virtuelle Festplatten von anderen DBMS virtuellen Maschinen von verschiedenen SAP-Systemen in einem Azure-Speicher-Konto. Planmäßigen damit auch die IOPS Grenzwerte Azure-Speicher Kontotypen Punkte ( <https://azure.microsoft.com/documentation/articles/storage-scalability-targets> )

##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] HA unter Windows

![SAP NetWeaver Anwendung HA Architektur mit SQLServer in Azure IaaS][planning-guide-figure-3200]

Die folgenden Azure Konstrukte werden Auswirkung minimieren, indem Sie Infrastrukturprobleme und Hosten Patch für das System SAP NetWeaver verwendet:

* Der Bereitstellung des Systems abgeschlossen auf Azure (erforderlich - DBMS Layer, (A) SCS Instanz und vollständige Anwendung Layer an derselben Stelle ausführen müssen).
* Das vollständige System führt innerhalb einer Azure Abonnements (erforderlich).
* Das vollständige System führt in einem Netzwerk der Azure-virtuellen (erforderlich).
* Die Aufteilung der virtuellen Computern von einem SAP-System in drei Verfügbarkeit Sätze ist auch für alle, die mit dem gleichen virtuellen Netzwerk gehören virtuellen Computern möglich.
* Alle virtuellen Computern DBMS Instanzen von einem SAP-System ausgeführt werden in einem Verfügbarkeit festzulegen. Angenommen, dass es gibt mehrere virtuelle Computer ausgeführt DBMS Instanzen pro System seit systemeigenen DBMS hohen Verfügbarkeit, die Features wie SQL Server AlwaysOn oder Oracle Data Guard verwendet werden.
* Alle virtuellen Computern ausführen DBMS Instanzen verwenden eigene Speicherkonto. DBMS Daten und Log-Dateien werden von einem Speicherkonto an ein anderes Speicherkonto mithilfe von DBMS hohen Verfügbarkeit von Funktionen, die die Daten zu synchronisieren repliziert. Ausfall des SQL-Fenster ein Knoten, aber nicht die gesamte SQL Server-Dienst bewirkt, dass Ausfall des Speicher-Konto ein. 
* Alle virtuellen Computern (A) SCS-Instanz von einem SAP-System ausgeführt werden in einem Verfügbarkeit festzulegen. Innerhalb dieser virtuellen Computern ist Windows Server Failover Cluster (WSFC) (A) SCS Instanz schützen konfigurieren.
* Alle virtuellen Computern ausführen (A) SCS Instanzen verwenden eigene Speicherkonto. (A) SCS Instanz Dateien und globale SAP-Ordner werden von einem Speicherkonto an ein anderes Speicherkonto mithilfe von SIOS DataKeeper Replikation repliziert. Ausfall des einen Speicherkonto bewirkt, dass Ausfall des eine (A) SCS Windows Cluster-Knoten, aber nicht die gesamte (A) SCS-Dienst. 
* Alle Darstellung der SAP-Anwendung Server Folie virtuellen Computern sind in einem dritten Verfügbarkeit festgelegt.
* ALLE SAP-Anwendungsserver ausgeführt virtuellen Computern verwenden eigene Speicherkonto. Ausfall des einen Speicherkonto bewirkt, dass Ausfall des einen SAP-Anwendungsserver, wo andere SAP AS weiterhin ausgeführt.

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] HA unter Linux

Die Architektur für SAP-HA auf Linux auf Azure entspricht im Grunde der Windows wie zuvor beschrieben. Zum Zeitpunkt Jan 2016 gibt es zwei Einschränkungen:

* nur SAP-ASE 16 wird derzeit auf Linux auf Azure ohne ASE Replikationsfeatures unterstützt. 
* Es gibt keine SAP-(A) SCS HA Lösung unter Linux auf Azure noch nicht unterstützt

Als Folge ab Januar 2016 ein SAP-Linux-Azure-System kann nicht erzielen die gleiche Verfügbarkeit als erkläre SAP-System wegen fehlender HA für den SCS (A) Instanz und einer Instanz ASE SAP-Datenbank.

### <a name="a-name4e165b58-74ca-474f-a7f4-5e695a93204fausing-autostart-for-sap-instances"></a><a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Verwenden des Computers für SAP-Instanzen

SAP angeboten die Funktionalität zum Starten von SAP-Instanzen unmittelbar nach dem Start des Betriebssystems innerhalb der virtuellen Computer an. Die genauen Schritte in SAP-Knowledge Base-Artikel [1909114] behandelt wurden – SAP Starten von Instanzen automatisch Parameter Computers verwenden. SAP wird jedoch nicht empfohlen, verwenden Sie die Einstellung nicht mehr, da kein Steuerelement in der Reihenfolge der Instanz neu gestartet wurde ist, Voraussetzung virtueller mehr als einem Computer betroffen haben oder mehrerer Instanzen pro virtueller Computer ist. Eine typische Azure Szenario einer SAP-Anwendung Server Instanz eines virtuellen Computers und den Fall eines einzelnen virtuellen Computers erste Gelegenheit neu gestartet wird vorausgesetzt, die Autostart ist nicht wirklich kritischen und aktiviert werden kann, indem Sie für diesen Parameter hinzufügen:

    Autostart = 1

In der Start-Profil der SAP-ABAP und/oder Java-Instanz.

> [AZURE.NOTE] 
> Der Autostart-Parameter kann auch einige Fachkenntnissystemen aufweisen. Ausführlicher löst der Parameter bei der zugehörige Windows/Linux Dienst der Instanz gestartet wird Starten eines SAP-ABAP oder Java-Instanz aus. Dies ist der Fall sicher auf, wenn den Betriebssystemen startet nach oben. Jedoch beim Neustart von SAP-Diensten sind auch eine allgemeine Sache für SAP-Software Lifecycle Management Funktionen wie Summe oder andere aktualisiert oder aktualisiert. Diese Funktionen würden keine Instanz überhaupt automatisch neu gestartet werden. Daher sollte der Parameter Autostart deaktiviert werden, bevor Sie solche Aufgaben ausführen. Der Parameter Autostart sollte auch nicht für SAP-Instanzen verwendet werden, die gruppiert sind, wie ASCS/SCS/CI.

Finden Sie hier weitere Informationen zum Autostart für SAP Instanzen:

* [Start/Stopp SAP zusammen mit Ihrer Unix Server starten/beenden](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Starten und Beenden von SAP NetWeaver Management Agents](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Zum Aktivieren der automatischen Start von HANA Database](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)


### <a name="larger-3-tier-sap-systems"></a>Größere Ebene 3 SAP-Systeme
Aspekte bei hoher Verfügbarkeit der Ebene 3 SAP-Konfigurationen haben bereits in den vorherigen Abschnitten erläutert. Aber Wissenswertes zum Systeme, wo DBMS Server Vorschriften zu groß sein, damit diese in Azure, aber den SAP-Anwendung Layer ansässig sind, in Azure bereitgestellt werden?

#### <a name="location-of-3-tier-sap-configurations"></a>Speicherort der Ebene 3 SAP-Konfigurationen

Es wird nicht unterstützt, um die Anwendungsebene Teilen selbst oder die Anwendung und DBMS zwischen lokalen und Azure zu stufen. Ein SAP-System ist entweder vollständig bereitgestellten lokalen oder in Azure. Es wird ebenfalls nicht unterstützt, wenn Sie einige der Anwendungsserver lokal und einige andere in Azure ausgeführt haben. Dies ist der Ausgangspunkt der Diskussion. Auch werden wir nicht unterstützen, um die DBMS Komponenten eines SAP-System und den SAP-Anwendung Server Layer in zwei verschiedenen Azure Regionen bereitgestellt haben. Z. B. DBMS Westen USA und SAP-Anwendung Ebene in zentralen USA. Grund für die Unterstützung von nicht solche Konfigurationen ist die Wartezeit Empfindlichkeit der SAP NetWeaver-Architektur.

Jedoch im Laufe der letztes Jahr Data Center Partner entwickelt gemeinsame Speicherorte für Azure. Diese gemeinsame Orte, ist häufig sehr dicht an die physischen Azure-Daten innerhalb einer Region Azure Centers. Die kurze Abstand und die Verbindung eines Anlagegutes gemeinsame Speicherort durch ExpressRoute in Azure können eine Wartezeit führen, die weniger als 2 ms ist. In diesem Fall um den DBMS Layer (einschließlich Speicher SAN/NAS) solche eine gemeinsame Position und den SAP-suchen ist Anwendung Layer in Azure möglich. Zum Zeitpunkt Dez 2015 haben nicht wir möchten alle Bereitstellungen. Aber andere Kunden mit nicht-SAP-Anwendung Bereitstellungen solche Ansätze bereits verwenden. 

### <a name="offline-backup-of-sap-systems"></a>Offline Sichern von SAP-Betriebssysteme

Abhängig von der SAP-Konfiguration ausgewählt (Ebene 2 oder Ebene 3) Es könnte eine Sicherung müssen. Der Inhalt des den virtuellen Computer Pluszeichen selbst eine Sicherungskopie der Datenbank haben. Das DBMS im Zusammenhang mit Datenbank Methoden erfolgen Sicherungskopien erwartet werden. Eine detaillierte Beschreibung für die anderen Datenbanken finden Sie unter [DBMS Guide] [Dbms-Anleitung]. Andererseits, können SAP-Daten in einer offline gegenseitig (einschließlich sowie den Datenbankinhalt) in diesem Abschnitt beschriebenen oder online wie im nächsten Abschnitt beschrieben gesichert werden.

Offline-Sicherung würde im Grunde Beendigung den virtuellen Computer über das Portal Azure erfordern, und eine Kopie der Basis Datenträger virtueller Computer sowie alle virtuellen Festplatten an den virtuellen Computer angefügt. Diesen würde einen Punkt in Time-Image den virtuellen Computer und den zugehörigen Laufwerk zu erhalten. Es wird empfohlen, um Sicherungskopien' der' in ein anderes Konto der Azure-Speicher zu kopieren. Daher die beschrieben in Kapitel [Kopieren Datenträger zwischen Azure-Speicherkonten] [planning-Leitfaden-5.4.2] dieses Dokument anwenden möchten.
Zusätzlich zu den war(en) mithilfe der Azure-Portal eine kann auch erledigen über Powershell oder CLI wie hier beschrieben: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Eine Wiederherstellung dieses Staates würde bestehen aus den Basis virtuellen Computer als auch die ursprünglichen Datenträger, der den Basis virtuellen Computer löschen und virtuelle Festplatten, Kopieren der ursprünglichen Speicherkonto die gespeicherten virtuellen Festplatten und erneutes Klicken Sie dann auf das System nicht geladen.
In diesem Artikel zeigt ein Beispiel, wie Sie dieses Verfahren in Powershell Skripts: <http://www.westerndevs.com/azure-snapshots/>

Stellen Sie sicher, dass eine neue SAP-Lizenz zu installieren, da Restoing eine Sicherungskopie virtueller Computer wie oben beschrieben einen neuen Hardwareschlüssel erstellt wird.

### <a name="online-backup-of-an-sap-system"></a>Online ein SAP-System-Sicherung

Sichern des DBMS erfolgt mit bestimmten Methoden DBMS [DBMS Leitfaden] [Dbms-Leitfadens] beschrieben. 

Anderen virtuellen Computern in das SAP-System können mithilfe von Funktionen Azure-virtuellen Computern Sicherung gesichert werden. Azure-virtuellen Computern Sicherung früh im 2015 eingeführt haben und in der Zwischenzeit wird ein standardmäßiges Verfahren zum Sichern eines vollständigen virtuellen Computers in Azure. Azure Sicherung speichert die Sicherungskopien in Azure und ermöglicht eine Wiederherstellung eines virtuellen Computers. 

> [AZURE.NOTE] 
> Zum Zeitpunkt Dez 2015 verwenden virtueller Computer Sicherung ist nicht möglich, die einmalige Nr. virtueller Computer die für SAP verwendet wird Lizenzierung. Dies bedeutet, dass die Installation eines neuen SAP-Lizenz Schlüssels eine Wiederherstellung aus einer Sicherung virtueller Computer erforderlich ist, wie der wiederhergestellte virtuellen Computer betrachtet wird, eines neuen virtuellen Computers und kein Ersatz von derjenigen, die unter seinem früheren befunden werden. Zum Zeitpunkt Jan 2016 unterstützt Azure-virtuellen Computer Sicherung keine virtuellen Computern, die bereitgestellt werden mit Azure Resourc Manager noch.

> ![Windows][Logo_Windows] Windows
>
> Theoretisch virtuellen Computern, die ausführen Datenbanken konsistent ebenfalls gesichert werden können, wenn das DBMS-System unterstützt die Windows-VSS (Volume Dienstfehler <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx> ) wie z. B. SQL Server unterstützt.
> Bedenken Sie jedoch die Grundlage Azure-virtuellen Computer Sicherungskopien, Point-in-Time-Datenbanken wiederhergestellt wird, nicht möglich sind. Daher, empfiehlt es sich, Sicherungskopien der Datenbanken mit DBMS-Funktionen auf Azure-virtuellen Computer Sicherung angewiesen ausführen
>
> Um kennenzulernen Azure-virtuellen Computern Sicherung Bitte beginnen Sie hier: <https://azure.microsoft.com/documentation/articles/backup-azure-vms/>.
>
> Andere mögliche Werte sind eine Kombination von Microsoft Data Protection Manager installiert einen Azure-virtuellen Computer und Azure Sicherung zu Sicherung und Wiederherstellung Datenbanken verwenden. Weitere Informationen finden Sie hier: <https://azure.microsoft.com/documentation/articles/backup-azure-dpm-introduction/>.  


> ![Linux][Logo_Linux] Linux
> 
> Es gibt keine Entsprechung in Windows VSS in Linux. Daher sind nur Datei konsistent Sicherungskopien möglich, aber nicht Anwendung konsistent Sicherungskopien. Die SAP-DBMS Sicherung erfolgen sollte DBMS-Funktionen verwenden. Das Dateisystem, wozu auch die SAP Zusammenhang Daten können gespeichert werden, z. B. unter Verwendung Tar beschriebenen hier: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>


### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure als für die Herstellung SAP-Landschaften DR-Standort

Seit Teil 2014 aktivieren Erweiterungen zu verschiedenen Komponenten um Hyper-V, System Center und Azure die Verwendung von Azure an, wie DR Website virtuellen Computern ausgeführt lokal auf Hyper-V entsprechend. 

Ein Blog mit Informationen, wie diese Lösung bereitgestellt wird hier beschrieben: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>

## <a name="summary"></a>Zusammenfassung
Die wichtigsten Punkte der hohen Verfügbarkeit für SAP-Systeme in Azure sind:

* Zu diesem Zeitpunkt kann zum Zeitpunkt der SAP-einzelnen des Fehlers genauso gesichert werden, wie es in lokale Bereitstellungen ausgeführt werden kann. Der Grund ist die freigegebene Datenträger Cluster noch in Azure ohne Verwendung von 3rd Party Software erstellt werden können.
* Müssen Sie für den Layer DBMS DBMS-Funktionen verwendet, die nicht auf den freigegebenen Datenträger Cluster-Technologie basiert. Details werden [Dbms-Leitfadens] [DBMS Leitfaden] beschrieben.
* Um den Einfluss der Probleme innerhalb Fehlerstrukturanalyse Domänen in der Azure-Infrastruktur oder Hostnamen Wartung zu minimieren, sollten Sie Azure Verfügbarkeit Datensätze verwenden:
    * Es wird empfohlen, dass eine Verfügbarkeit für den SAP-Anwendung Layer festlegen.
    * Es wird empfohlen, dass eine separate Verfügbarkeit festlegen für den Layer SAP-DBMS.
    * Es wird nicht empfohlen, zum Anwenden der gleichen Verfügbarkeit für virtuelle Computer von verschiedenen SAP-Systemen festlegen.
* Aus Gründen der Sicherungskopie der SAP-DBMS Ebene überprüfen Sie [DBMS Leitfaden] [Dbms-Leitfadens] ein.
* Sichern von SAP-Dialogfeld Instanzen sinnvoll etwas da es in der Regel schneller wird erneut einfache Dialogfeld Instanzen bereitstellen.
* Sichern der virtuellen Computer die globale Verzeichnis das SAP-System und mit enthält alle Profile von den verschiedenen Instanzen ist sinnvoll und mit Windows-Sicherung durchgeführt werden soll oder z. B. tar unter Linux. Da Unterschiede zwischen Windows Server 2008 (R2 ab) und Windows Server 2012 (R2) vorhanden sind, die mit der neueren Windows Server sichern erleichtern frei, wir empfehlen, Windows Server 2012 (R2) als Gast-Betriebssystem Windows ausgeführt. 
