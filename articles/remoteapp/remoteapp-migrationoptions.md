
<properties 
    pageTitle="Optionen für die Migration von Azure RemoteApp | Microsoft Azure" 
    description="Lernen Sie die Optionen für die Migration von Azure RemoteApp aus." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Optionen für die Migration von Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Wenn Sie nicht mehr verwenden Azure RemoteApp aufgrund der [Rente Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) , oder weil Sie Ihre Bewertung abgeschlossen haben, müssen Sie sich von Azure RemoteApp zu einer anderen app-Verwaltungsdienst migrieren. Es gibt zwei verschiedene Ansätze für die Migration: eine Lektion verwaltete (oft Infrastruktur als Dienst [IaaS] bezeichnet) Bereitstellung oder eine vollständig verwaltete (häufig bezeichnet Plattform als Dienst) oder Software als Service [PaaS/SaaS] Geschenk. 

Self-service IaaS ist einer http://www.Office.com/redir/xt103221361 Umgebung, die verwaltet, betrieben und Besitzer, Sie direkt auf virtuellen Computern (virtuellen Computern) oder physischen Systemen bereitgestellt wird. Am Ende anderen eine vollständig verwaltete PaaS/SaaS-Lösung ist mehr wie RemoteApp Azure - ein Partners bietet eine Dienstebene über eine Remotinglösung, die Betrieb behandelt und Wartung, während Sie als den Kunden, führen Sie einige Bild- und app Management.

Lesen Sie auf Weitere Informationen, einschließlich Beispiele für die verschiedenen Hostinganbieter Optionen aus.    

## <a name="self-managed-iaas-solutions"></a>Self-managed (IaaS) Lösungen

### <a name="rds-on-iaas"></a>**Klicken Sie auf IaaS RDS** 
Sie können eine systemeigene Sitzung-basierten Remote Desktop Services (in Windows Server)-Bereitstellung mit entweder RemoteApp oder Desktops lokalen bereitstellen oder in einer gehosteten Umgebung (wie auf Azure-virtuellen Computern). RDS auf IaaS Bereitstellungen am besten für Kunden mit bereits vertraut sind und die vorhandenes Fachwissen mit RDS Bereitstellungen aufweisen. 

> [AZURE.NOTE]
> Sie benötigen Volume Licensing mit Software Assurance (SA) RDS Access Lizenzen für diese Bereitstellungsoption verwenden.

Bereitstellen von RDS auf Azure-virtuellen Computern ist einfacher denn je, bei Verwendung von Bereitstellung und Patch Vorlagen (Lesen eine [Übersicht](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) und dann [zu gelangen sie erhalten](https://aka.ms/rdautomation)). Sie können die gleichen flexible Anpassungsbereich für Funktionen mit Azure klassische Modell Bereitstellungsressourcen (nicht Azure Ressourcenmodell Ressourcen) in Azure RemoteApp erhalten, mithilfe der [automatischen Skalierung Skript](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), zwar weitere Anpassungen und Konfigurationen bestehen. Wenn Sie RDS auf Azure-virtuellen Computern bereitstellen, wird durch [Azure unterstützen](https://azure.microsoft.com/support/plans/), die gleichen Supportmitarbeiter unterstützt, die Sie mit Azure RemoteApp unterstützt. Je nach vorhandenen Verwendung durch Kontaktieren des [Supports Azure](https://azure.microsoft.com/support/plans/)schätzt Kosten abrufen können, oder Sie können Berechnungen durchführen, selbst mit einer demnächst Kosten Taschenrechner.  Mit der N-Serie virtuellen Computern (aktuell in privaten Preview) Sie können auch hinzufügen vGPU - Weitere Informationen zum Hinzufügen von vGPU und Informationen zu [RDS Verbesserungen in Windows Server 2016 nutzen](https://myignite.microsoft.com/videos/2794) in unseren Ignite Sitzung zu hören.   

Wir haben Schritt-für-Schritt-Bereitstellung Führungslinien für [Windows Server 2012 R2](http://aka.ms/rdsonazure) und [Windows Server 2016](http://aka.ms/rdsonazure2016) zur Unterstützung bei der Bereitstellung. Schauen Sie sich den [Blog Remotedesktop](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) die neuesten Informationen.
 
### <a name="citrix-on-iaas"></a>**Klicken Sie auf IaaS Citrix** 
Einer systemeigenen Citrix Bereitstellung von Sitzung-basierten XenApp oder XenDesktop kann werden bereitgestellt lokalen oder in einer gehosteten Umgebung (z. B. auf Azure-virtuellen Computern). 

Schauen Sie sich die schrittweise Bereitstellungshandbuch, [Citrix XA 7.6 auf Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), Weitere Informationen. Weitere Informationen zu [Citrix auf Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), einschließlich einen Taschenrechner Preis. Sie können auch eine [Citrix wenden Sie sich an](http://citrix.com/English/contact/index.asp) Ihre Optionen zum Diskutieren suchen.

## <a name="fully-managed-paassaas-offerings"></a>Vollständig verwaltete (PaaS/SaaS) Angebote

### <a name="citrix-cloud"></a>**Citrix Cloud** 
[Citrix vorhandenen Cloud-Lösung](https://www.citrix.com/products/citrix-cloud/), Architektur mit Citrix XenApp Express identisch. Citrix bietet eine [50 % Rabatt Werbeaktion](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) für vorhandene Azure RemoteApp Kunden an. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (in Tech Preview)**
[Registrieren Sie sich für ihre Tech Preview](http://now.citrix.com/remoteapp), und schauen Sie sich ihre [Ignite Sitzung](https://myignite.microsoft.com/videos/2792) (beginnend minute 20:30). XenApp Express entspricht Architektur Citrix Cloud außer darin enthaltenen vereinfachtes Management Benutzeroberfläche und andere Features und Funktionen, die ähnliche Azure RemoteApp sind. 

Weitere Informationen zu [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Citrix Service Provider Program** 
Die Citrix Service Provider Program erleichtert Dienstanbieter zum Vorführen einer einfach zu verwendenden virtuelle Cloud computing auf kleine und mittlere Unternehmen, sie in einem Modell einfach, je nach Bedarf berechnet gewünschten Dienste anbieten. Citrix-Dienstanbieter ihre Microsoft SPLA Unternehmen wächst und erweitern Sie ihre Investitionen in RDS Plattformen mit einem beliebigen Gerät, ORTSUNABHÄNGIGER Zugriff, die größte Unterstützung der Anwendung, eine umfangreiche Funktionen, mehr Sicherheit und höhere Skalierbarkeit. Wiederum Citrix Dienstanbieter gewinnen mehr Teilnehmern, Akzeptanz seitens der Kunden und deren Betrieb zu senken nutzen. [Weitere Informationen finden Sie](http://www.citrix.com/products/service-providers.html) oder [ein Partners zu finden](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft gehostet Dienstanbieter** 
Hostinganbieter Partner bieten in der Regel eine vollständig verwaltete Windows-Desktop gehostet und Anwendungsdienst, z. B. Verwalten der Azure Ressourcen, Betriebssysteme,-Anwendungen und Helpdesk des Partners mithilfe der Lizenzierung Agreements mit Microsoft und anderen Softwareanbietern zusammen mit einer Service Provider-Lizenzvertrag dürfen vertreibt von Abonnenten Access License (SAL) wird. Die folgende Informationen enthält Details und Kontaktinformationen für einen Teil der Hoster, die auf die Unterstützung von Kunden bei der Migration Azure RemoteApp spezialisiert sind. Schauen Sie sich [die aktuelle Liste der Dienstanbieter gehostet](http://aka.ms/rdsonazurecertified) , die die RDS auf IaaS learning Pfad und Bewertung abgeschlossen haben.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) hat sich in der Cloud Übergang ISVs und ISV auf ' suchen, um deren aktuellen Cloud-Setups zu optimieren. ASPEX bietet eine Vielzahl von verwalteten Services, Devops und Beratungsdienste.  

Primärer Speicherort: Antwerpen, Belgien

Vorgang Region: westliche Europa

Partner-Status: [Silber](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoft-Cloud-Service Provider: Ja

Anbieten von Sitzung-basierten RemoteApp und Desktop-Lösungen: Ja, beide

Azure RemoteApp Migration Lösungen: Ja, [informieren Sie sich](https://www.aspex.be/en/azure-remote-apps)

**Kontakt:**

- Telefon: +3232202198
- E-Mail:[info@aspex.be](mailto:info@aspex.be)
- Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (Platform Name: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) ist eine Automatisierungsplattform für IT-Unternehmen zu vereinfachen und Optimieren der Migration und Bereitstellung von remote Desktops, remote-Anwendungen und in der Microsoft Azure-Cloud-Infrastruktur skalieren. 

Die MyCloudIT-Plattform Verkürzung der Bereitstellung von 95 %, 30 % und Kosten Azure und verschiebt gesamten IT-Infrastruktur des Kunden in der Cloud in wenigen ein paar wichtige Striche. Partner können nun Kunden aus einem globalen Dashboard verwalten, Dienst Endbenutzer auf der ganzen Welt wie nie zuvor und wächst Umsätze ohne zusätzlichen Aufwand oder umfangreiche Azure Schulung.  

Primärer Speicherort: Dallas, TX, USA

Vorgang Region: weltweit

Partner-Status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoft-Cloud-Service Provider: Ja

Anbieten von Sitzung-basierten RemoteApp und Desktop-Lösungen: Ja, beide

Azure RemoteApp Migration Lösungen: Ja, [informieren Sie sich](https://mycloudit.com/remote-app-microsoft/)

**Kontakt:**
- Robert Garoutte, VP für Geschäftsentwicklung

   Telefon: 972-218-0741

   E-Mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) hat sich auf Lösungen gehosteten desktop, ausliefern von vollständigen Windows-Desktop und ISV Applikationen Erfahrung zu einem globalen Basis Client aus Azure und ihre Datencenter auf der Microsoft-Technologie erstellt wurden.

Primärer Speicherort: London, Großbritannien; Singapur; Houston, TX

Vorgang Region: weltweit

Partner-Status: Gold

Microsoft-Cloud-Service Provider: Ja

Anbieten von Sitzung-basierten RemoteApp und Desktop-Lösungen: Ja, beide

Azure RemoteApp Migration Lösungen: Ja, [informieren Sie sich](http://www.acuutech.com/ara-migration/)

**Kontakt:**

- Vereinigtes Königreich:

  5/6 York Haus Langston Straßen,

  Loughton, erwähnt IG10 3TQ
  
  Telefon: + 44 (0) 20 8502 2155
 
- Singapur:

  100 Cecil Straße, #09-02, 
  
  Der Globus, Singapur 069532
  
  Telefon: + 65 6709 4933
 
- Nordamerika: 

  3601 S. Sandman Mo.
  
  Suite 200, Houston, TX 77098
  
  Telefon: +1 713 691 0800

## <a name="need-more-help"></a>Benötigen Sie weitere Hilfe?
Immer noch benötigen Sie Hilfe auswählen oder weiteren haben Fragen? Verwenden Sie eine der folgenden Methoden an, um Hilfe zu erhalten. 

1.  Uns eine e-Mail an [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Wenden Sie sich an [Azure unterstützen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Öffnen einer [Azure unterstützen Fall](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)Sie zunächst.
3.  Rufen Sie uns an. [Suchen nach einer lokalen Auftragsnummer](https://azure.microsoft.com/overview/sales-number/).
