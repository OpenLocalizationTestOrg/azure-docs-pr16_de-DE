<properties
   pageTitle="Service Stabilität Anleitungen | Microsoft Azure"
   description="Links zu Wiederherstellung und proaktive Stabilität und Verfügbarkeit Anleitungen für Microsoft Azure-Dienste."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Hinweise zu Microsoft Azure Diensten Stabilität
Microsoft Azure ist bietet Ihnen mit den Ressourcen, die Sie benötigen, wenn Sie diese benötigen. Als Bestandteil der ein guter Entwurf und Betrieb Methoden sollten Sie wissen, sowohl zum Planen der Verwendung von Azure Services eine hohen Verfügbarkeit als auch was zu tun ist, wenn eine Anwendung nach einer dienststörung beeinträchtigt wird. Um Sie dabei unterstützen, enthält dieses Dokument Links zu Disaster Wiederherstellung Anleitungen ebenso wie Entwurf Anleitungen für verschiedene Azure-Dienste.

##<a name="disaster-recovery-guidance"></a>Disaster Wiederherstellung Anleitungen
Die Disaster Wiederherstellung Anleitung unten Links sind kann Ihnen mit den Informationen, die Sie benötigen, mit deren Hilfe Sie Ihrer Anwendung wieder online schnell zu erhalten, wenn Sie von einer Azure dienststörung betroffen sind. Diese Links wurden entwickelt, um Sie bei der Frage, "Ich bin wird durch beeinträchtigt ein Azure dienststörung, was kann ich tun?"

##<a name="design-guidance"></a>Entwurf Anleitungen
Die Entwurf Anleitungen Links unten sind Entwurf und Architektur Ratschläge, die erstellt wurden, um Ihnen helfen zu verstehen wie am besten mit jeder Azure Service auf eine Weise, die Verfügbarkeit der Anwendung maximiert. Diese Links wurden erstellt, mit denen Sie die Antwort auf die Frage "Wie ich sicher, dass die allgemeine Verfügbarkeit meiner Anwendung wird nicht, einen Fehler, Hardware-Fehlers, dienststörung oder andere Fehler beeinflussen machen?" Ist keine spezifische Leitfäden für den Dienst, die, den zurzeit von Ihnen gesuchte, möglicherweise der [hohen Verfügbarkeit bei Microsoft Azure](./resiliency-high-availability-azure-applications.md) -Artikel zusätzliche Informationen, die Sie in Ihrem Entwurf helfen können. 

##<a name="service-guidance"></a>Hinweise zu Diensten
| Dienst  | Disaster Wiederherstellung Anleitungen | Entwurf Anleitungen |
|:---------|:--------------------------:|:------------------:|
| [Cloud-Dienste] (https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services")       | [Link] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure Cloud Services Disaster Wiederherstellung Anleitungen")   | Nicht verfügbar |
| [Key Tresor] (https://azure.microsoft.com/services/key-vault/ "Azure Key Tresor")                      | [Link] (../key-vault/key-vault-disaster-recovery-guidance.md "Azure Schlüssel Tresor Disaster Wiederherstellung Anleitungen")        | Nicht verfügbar |
| [Speicher] (https://azure.microsoft.com/services/storage/ "Azure-Speicher")                            | [Link] (../storage/storage-disaster-recovery-guidance.md "Azure-Speicher Disaster Wiederherstellung Anleitungen")          | Nicht verfügbar |
| [SQL-Datenbanken] (https://azure.microsoft.com/services/sql-database/ "SQL Azure-Datenbanken")           | [Link] (../sql-database/sql-database-disaster-recovery.md  "Azure SQL-Datenbank Disaster Wiederherstellung Anleitungen")    | [Link] (../sql-database/sql-database-business-continuity.md "Übersicht über Geschäftskontinuität mit Azure SQL-Datenbank") |
| [Virtuellen Computern] (https://azure.microsoft.com/services/virtual-machines/ "Azure-virtuellen Computern") | [Link] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azure-virtuellen Computern Disaster Wiederherstellung Anleitungen") | Nicht verfügbar |
| [Virtuelles Netzwerk] (https://azure.microsoft.com/services/virtual-network/ "Azure virtuelles Netzwerk")    | [Link] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure-virtuellen Netzwerk Disaster Wiederherstellung Anleitungen")  | Nicht verfügbar |

##<a name="next-steps"></a>Nächste Schritte
Wenn Sie Anleitungen, die breiterer auf Systeme und Lösungen suchen Schwerpunkt, lesen Sie [Wiederherstellung und hohe Verfügbarkeit bei Microsoft Azure](https://aka.ms/drtechguide).
