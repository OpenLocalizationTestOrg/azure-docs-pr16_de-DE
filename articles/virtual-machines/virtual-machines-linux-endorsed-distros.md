<properties
    pageTitle="Unterstützt die Versionen von Linux | Microsoft Azure"
    description="Lernen Sie auf Verteilung Azure unterstützt, einschließlich der Richtlinien für Ubuntu, OpenLogic, Oracle und SUSE Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux auf Verteilung Azure unterstützt

> [AZURE.NOTE] Wenn Sie eine kurze haben, helfen Sie uns, in der Dokumentation Azure Linux virtueller Computer zu verbessern, indem Sie die Teilnahme an dieser [Umfrage schnelle](https://aka.ms/linuxdocsurvey) Ihrer Person aufführen. Jeder Antwort hilft uns helfen Ihnen, die Sie Ihre Arbeit vermitteln.

Linux Bilder in der Azure-Katalog oder Marketplace anhand einer Anzahl von Partner bereitgestellt werden, und wir arbeiten mit verschiedenen Linux Communitys noch mehr Arten zur unterstützt Verteilerliste hinzufügen. In der Zwischenzeit können Sie für die Verteilung nicht verfügbar aus dem Katalog immer schalten-Your-Besitzer-Linux anhand der Richtlinien auf [dieser Seite](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Unterstützte Verteilung und Versionen ##

Die folgende Tabelle listet die Linux-Versionen und die Versionen, die auf Azure unterstützt werden. Lizenzinformationen auch finden Sie ausführliche Informationen [für Linux Bilder in Microsoft Azure unterstützen](https://support.microsoft.com/en-us/kb/2941892) .

Die Linux Integration Services (LIS) Treiber für Hyper-V und Azure sind Kernelmodule, die Microsoft direkt an den übergeordneten Linux Kernel beiträgt.  Die LIS-Treiber sind entweder in die Verteilung der Kernel standardmäßig integriert oder Verteilung stehen als separate Download für ältere RHEL/CentOS-basierten [hier](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Finden Sie weitere Informationen zu den Treibern LIS [in diesem Artikel](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) .

Der Azure Linux-Agent ist bereits auf die Bilder Azure-Katalog vorinstalliert und die Verteilung des Pakets Repository in der Regel verfügbar sind.  Quellcode finden Sie auf [GitHub](https://github.com/azure/walinuxagent).

Verteilung|Version|Treiber|Agent
---|---|---|---
CentOS durch OpenLogic | CentOS 6.3 + 7.0 + | CentOS 6.3: [LIS herunterladen](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: Im Kernel | Paket: In [OpenLogic Repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) unter "WALinuxAgent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | Im Kernel | Quellcode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7.9 +, 8.2 + | Im Kernel | Paket: In Repo unter "Waagent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle-Linux | 6.4 +, 7.0 + | Im Kernel | Paket: In Repo unter "WALinuxAgent" <br/>Quellcode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Red Hat Enterprise Linux | RHEL 6,7 + 7.1 + | Im Kernel|Paket: In Repo unter "WALinuxAgent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 SP1 oder höher und <p> SLES für SAP-11 SP3 oder höher | Im Kernel | Paket: In der [Cloud: Tools](https://build.opensuse.org/project/show/Cloud:Tools) Repo unter "Python-Azure-Agent" <br/>Quellcode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | OpenSUSE 13.2 + | Im Kernel | Paket: In der [Cloud: Tools](https://build.opensuse.org/project/show/Cloud:Tools) Repo unter "Python-Azure-Agent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16.10 | Im Kernel | Paket: In Repo unter "Walinuxagent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partner

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic ist eine führende Anbieter von Enterprise open-Source-Lösungen für die Cloud und das Data Center. OpenLogic hilft hundert führenden Enterprise über eine Vielzahl von Branchen sicher erfassen, unterstützen und open Source-Software steuern. OpenLogic bietet den technischen Support für kommerzielle-Noten und Entschädigung für 600 open-Source-Paketen von der OpenLogic Experten Community, einschließlich der Ebene Enterprise-Unterstützung für CentOS wird als der Schnellstartleiste Partner für die Bereitstellung von CentOS-basierten Bilder auf Azure gesicherten.

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/Running-coreos/Cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Von der CoreOS-Website:

*CoreOS dient zur Sicherheit, Zuverlässigkeit und Konsistenz. Statt der Installation Pakete per Yum oder apt verwendet CoreOS Linux Container zum Verwalten Ihrer Dienste auf einer höheren Ebene abstrakte an. Ein einzelner Dienst Code und alle Abhängigkeiten sind innerhalb eines Containers verpackt, die auf einen oder mehrere CoreOS Computern ausgeführt werden kann.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-Blog/Debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ ist eine unabhängige bietet Beratungsdienste und Dienstleistungsunternehmen, sich bei der Entwicklung und Implementierung von professionell Lösungen mithilfe von kostenlosen Software auf. Als führende Open Source-Spezialisten hat Credative internationale Anerkennung mit vielen IT-Abteilung, mit deren Unterstützung. In Verbindung mit Microsoft besteht Credativ aktuell entsprechende Debian Bilder Debian 8 (Jessie) und Debian vor 7 (Wheezy), vorbereitet, die speziell für Azure ausgeführt und kann über die Plattform einfach verwaltet werden. Credativ unterstützen auch die Wartung langfristig und der Debian Bilder für Azure über deren öffnen Quelle Support Centers aktualisieren.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/Cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle Strategie besteht darin, anbieten ein umfangreichen Portfolio an Lösungen für öffentliche und private Wolken, während Sie mit dem Kunden Auswahl und Flexibilität bei, wie er im Oracle Wolken sowie andere Wolken die Oracle-Clientsoftware bereitstellen.  Oracle Zusammenarbeit mit Microsoft kann Kunden in Microsoft öffentlichen und privaten Wolken ohne das Sicherheitsrisiko der Zertifizierung die Oracle-Clientsoftware bereitstellen und unterstützen von Oracle.  Oracle Engagement und Ihren Investitionen in Oracle öffentliche und private Cloudlösungen unverändert.

### <a name="red-hat"></a>Red Hat
[http://www.redhat.com/en/Partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Der weltweit führende Anbieter der open-Source-Lösungen, Red Hat kann mehr als 90 % der Fortune 500-Unternehmen Lösen von Problemen konfrontiert, richten Sie ihre IT und Business-Strategien und Vorbereiten für die Zukunft der Technologie. Red Hat wird secure Solutions über ein Modell öffnen Business und ein Abonnementmodell kostengünstiger, vorhersehbar bereitgestellt.

### <a name="suse"></a>SUSE
[http://www.SuSE.com/SuSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server auf Azure ist eine bewährte Plattform, die übergeordnete Zuverlässigkeit und Sicherheit für das cloud-computing SFA. Vielseitige des SUSE Linux-Plattform nahtlose Integration mit Azure Cloud Services vorführen eine leicht zu verwaltenden Cloud-Umgebung. Und mit mehr als 9,200 zertifizierten Applications aus mehr als 1.800 unabhängigen Software-Anbietern für SUSE Linux Enterprise Server, SUSE gewährleistet Auslastung ausgeführt in Data Center unterstützten zuverlässig auf Azure bereitgestellt werden können.

### <a name="canonical"></a>Kanonische
[http://www.Ubuntu.com/Cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanonische technisch und offene Community Governance Laufwerk Ubuntu Erfolg im Client, Server und Cloud computing, einschließlich persönlicher Cloud Services für Verbraucher. Der kanonisch Vision einer kostenlosen einheitlichen Plattform in Ubuntu, über das Telefon in die cloud mit einer Familie von zusammenhängendes Schnittstellen für Telefon, Tablet, TV und Desktop, macht Ubuntu zur erste Wahl für unterschiedliche Institutionen aus öffentlichen Cloud-Anbieter den Herstellern von Unterhaltungselektronik und als Favorit zwischen den einzelnen Technikexperten.

Mit Entwickler und engineering Centers auf der ganzen Welt kanonisch ist eindeutig positioniert, um mit Hardware Entscheidungsträger, Inhaltsanbietern und Softwareentwickler Ubuntu Lösungen bringen vermarkten, partner aus PCs Servern und mobilen Geräten.

