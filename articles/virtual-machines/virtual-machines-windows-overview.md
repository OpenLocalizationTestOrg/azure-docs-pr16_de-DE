<properties
    pageTitle="Übersicht über die Windows-virtuellen Computern | Microsoft Azure"
    description="Informationen Sie zu erstellen und Verwalten von Windows-virtuellen Computern in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Übersicht über die Windows-virtuellen Computern in Azure

Azure-virtuellen Computern (virtueller Computer) ist der verschiedene Arten von [bei Bedarf, skalierbare computing-Ressourcen](../app-service-web/choose-web-site-cloud-service-vm.md) , die Azure bietet. In der Regel, wählen Sie ein virtuellen Computers aus, wenn Sie mehr Kontrolle über die Umgebung als die anderen Optionen bieten benötigen. In diesem Artikel erhalten Sie Informationen, was Sie berücksichtigen sollten vor dem Erstellen eines virtuellen Computers, wie Sie sie erstellt haben und wie Sie verwalten.

Ein Azure-virtuellen Computer bietet Ihnen die Flexibilität von Virtualisierung ohne zu kaufen und verwalten die physische Hardware, die es ausgeführt wird. Allerdings müssen Sie immer noch den virtuellen Computer verwalten, indem Sie die Durchführung von Aufgaben, wie etwa konfigurieren, Patch und Installation der Software, die darauf ausgeführt wird.

Azure-virtuellen Computern kann auf verschiedene Weise verwendet werden. Einige Beispiele für sind:

- **Entwicklung und Test** – Azure-virtuellen Computern bieten eine schnelle und einfache Weise zum Erstellen von eines Computers mit bestimmten Konfigurationen erforderlich, um den Code, und Testen der Anwendung.
- **Applications in der Cloud** – da Demand für eine Anwendung schwanken kann, ist es möglicherweise sinnvoll economic für die Ausführung eines virtuellen Computers in Azure. Sie Zahlen für zusätzliche virtuellen Computern, wenn Sie benötigen, und sie fahren, wenn Sie nicht.
- **Erweiterte Datacenter** – virtuellen Computern in einem Azure-virtuellen Netzwerk kann leicht mit Netzwerk Ihrer Organisation verbunden sein.

Die Anzahl der virtuellen Computern, die eine Anwendung verwendet kann nach oben und heraus skalieren, an welchen je nach Bedarf erforderlich ist.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Was muss ich anzustellen vor dem Erstellen eines virtuellen Computers?

Es gibt eine Vielzahl von [gibt](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) immer wenn Sie sich eine Anwendungsinfrastruktur in Azure erstellen. Diese Aspekte eines virtuellen Computers werden überlegen müssen, bevor Sie beginnen:

- Die Namen der Anwendungsressourcen
- Die Position, wo die Ressourcen gespeichert sind
- Die Größe der virtuellen Computer
- Die maximale Anzahl von virtuellen Computern, die erstellt werden können
- Das Betriebssystem, das der virtuellen Computer ausgeführt wird.
- Die Konfiguration von den virtuellen Computer nach dem Start
- Die zugehörigen Ressourcen, die der virtuellen Computer muss

### <a name="naming"></a>Benennen

Ein virtuellen Computers weist einen [Namen](virtual-machines-windows-infrastructure-naming-guidelines.md) zugewiesen werden, und es hat einen Computernamen als Teil des Betriebssystems konfiguriert. Der Name eines virtuellen Computers kann bis zu 15 Zeichen sein.

Wenn Sie Azure verwenden, um das Betriebssystem Laufwerk erstellen, entsprechen den Computernamen und den Namen des virtuellen Computers. Wenn Sie [Eigene Bild hochladen und verwenden](virtual-machines-windows-upload-image.md) , die einem zuvor konfiguriert Betriebssystem enthält und ihre Verwendung zum Erstellen eines virtuellen Computers, die Namen unterschiedlich sein können. Es empfiehlt sich, wenn Sie Ihre eigenen Bilddatei hochladen, Sie den Namen des Computers in das Betriebssystem vornehmen, und nennen Sie die virtuellen Computern gleich.

### <a name="locations"></a>Speicherorte

Alle Ressourcen, die in Azure erstellt werden auf mehreren [Ländern / Regionen](https://azure.microsoft.com/regions/) auf der ganzen Welt verteilt. In der Regel heißt die Region **Speicherort** , beim Erstellen eines virtuellen Computers. Für einen virtuellen Computer gibt den Speicherort der virtuellen Festplatten gespeichert sind.

Diese Tabelle zeigt einige der Methoden, mit denen, die Sie eine Liste der verfügbaren Speicherorte aus zugreifen können.

| Methode | Beschreibung |
| ------ | ----------- |
| Azure-portal | Wählen Sie einen Speicherort aus der Liste aus, wenn Sie einen virtuellen Computer erstellen. |
| Azure PowerShell | Verwenden des Befehls [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| REST-API | Verwenden des [Liste Speicherorte](https://msdn.microsoft.com/library/dn790540.aspx) Vorgangs an. |

### <a name="vm-size"></a>Virtueller Speicher

Die [Größe](virtual-machines-windows-sizes.md) des den virtuellen Computer, die Sie verwenden, wird durch die Arbeitsbelastung bestimmt, die Sie ausführen möchten. Die Größe, die Sie wählen bestimmt Faktoren wie Verarbeitung Power, Arbeitsspeicher und Speicherkapazität. Azure bietet eine Vielzahl von Größen zur Unterstützung von viele Arten von verwendet.

Azure Gebühren einem [Preis pro Stunde](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) basierend auf Größe und Betriebssystem des virtuellen Computers. Teilweise Stunden Azure Gebühren nur für die Anzahl der Minuten verwendet. Speicher ist Preis und separat in Rechnung gestellt.

### <a name="vm-limits"></a>Grenzwerte für virtuellen Computer

Ihr Abonnement ist Standard- [Kontingent Grenzwerte](../azure-subscription-service-limits.md) in Ort, an dem die Bereitstellung von vieler virtueller Computer für ein Projekt beeinträchtigen kann. Die aktuelle Begrenzung auf Basis pro Abonnement ist 20 virtuellen Computern pro Region. Grenzwerte können durch eine Support-Ticket Anfordern einer erhöhen Ablage ausgelöst werden.

### <a name="operating-system-disks-and-images"></a>Betriebssystem-Datenträger und Bilder

Virtuellen Computern verwenden [virtuelle Festplatten (virtuelle Festplatten)](virtual-machines-windows-about-disks-vhds.md) , um ihre Betriebssystem (BS) und die Daten zu speichern. Virtuelle Festplatten werden auch für die Bilder verwendet, die, denen Sie auswählen können, um ein Betriebssystem zu installieren. 

Azure bietet viele [Marketplace Bilder](https://azure.microsoft.com/marketplace/virtual-machines/) zur Verwendung mit verschiedenen Versionen und Typen von Windows-Serverbetriebssysteme. Marketplace-Bilder identifizierten Bild Publisher, Angebot, Sku und Version (normalerweise ist Version als Spätester angegeben haben). 

Diese Tabelle enthält einige Methoden, um die Informationen für ein Bild zu finden.

| Methode | Beschreibung |
| ------ | ----------- |
| Azure-portal | Die Werte werden automatisch für Sie angegeben, wenn Sie ein Bild verwenden auswählen. |
| Azure PowerShell | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -Speicherort "Speicherort"<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -Speicherort "Speicherort"-Publisher "PublisherName"<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -Speicherort "Speicherort"-Publisher "PublisherName"-anbieten "OfferName" |
| REST-APIs | [Liste Bild Herausgeber](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Bietet ' Liste '](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Liste Bild skus](https://msdn.microsoft.com/library/mt743701.aspx) |

Sie können entscheiden, [Hochladen, und verwenden Sie ein eigenes Bild](virtual-machines-windows-upload-image.md) und in diesem Fall der Name des Herausgebers, Angebot und Sku verwendet nicht zur Verfügung.

### <a name="extensions"></a>Extensions

Virtueller Computer- [Erweiterungen](virtual-machines-windows-extensions-features.md) Geben Sie Ihre virtuellen Computer Zusatzfunktionen bis Beitrag Bereitstellungskonfiguration und automatisierte Aufgaben.

Diese allgemeinen Aufgaben können mithilfe von Erweiterungen ausgeführt werden:

- **Ausführen benutzerdefinierter Skripts** – die [Erweiterung für benutzerdefinierte Skripts](virtual-machines-windows-extensions-customscript.md) , hilft Ihnen die Konfiguration der Auslastung des virtuellen Computers durch Ausführen der Skripts aus, wenn Sie der virtuellen Computer bereitgestellt wird.
- **Bereitstellen und Verwalten von Konfigurationen** – die [Erweiterung PowerShell gewünscht Zustand Konfiguration (DSC)](virtual-machines-windows-extensions-dsc-overview.md) hilft Ihnen beim Einrichten eines virtuellen Computers DSC Konfigurationen und Umgebungen verwalten.
- **Diagnosedaten sammeln** – die [Azure Diagnose Erweiterung](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) hilft Ihnen die Konfiguration des virtuellen Computer, um Diagnosedaten zu erfassen, die verwendet werden können, um die Integrität Ihrer Anwendung zu überwachen.

### <a name="related-resources"></a>Zugehörige Ressourcen

Die Ressourcen in dieser Tabelle werden von den virtuellen Computer verwendet und müssen vorhanden oder erstellt werden, wenn Sie der virtuellen Computer erstellt wird.

| Ressource | Erforderlich | Beschreibung |
| -------- | -------- | ----------- |
| [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) | Ja | Der virtuellen Computer muss in einer Ressourcengruppe enthalten sein. |
| [Speicher-Konto](../storage/storage-create-storage-account.md) | Ja | Der virtuellen Computer benötigt das Speicherkonto zum Speichern von deren virtuellen Festplatten. |
| [Virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) | Ja | Der virtuellen Computer muss ein Mitglied eines virtuellen Netzwerks. |
| [Öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Nein | Der virtuellen Computer kann eine öffentliche IP-Adresse für den sie Remotezugriff auf zugewiesen haben. |
| [Netzwerk-Benutzeroberfläche](../virtual-network/virtual-network-network-interface-overview.md) | Ja | Der virtuellen Computer benötigt die Schnittstelle im Netzwerk kommunizieren. |
| [Daten Datenträger](virtual-machines-windows-attach-disk-portal.md) | Nein | Der virtuellen Computer kann Daten Datenträger um Speicherfunktionen erweitern einbeziehen. |

## <a name="how-do-i-create-my-first-vm"></a>Wie kann ich meine erste virtueller Computer erstellen?

Sie haben eine Reihe von Optionen zum Erstellen der virtuellen Computer. Die Option, die Sie vornehmen, hängt von der Umgebung Ihnen ab. 

Diese Tabelle enthält Informationen für den Sie Schritte Erstellen Ihrer virtuellen Computer.

| Methode | Artikel |
| ------ | ------- |
| Azure-portal | [Erstellen Sie einen virtuellen Computer mit Windows mit dem portal](virtual-machines-windows-hero-tutorial.md) |
| Vorlagen | [Erstellen von einem Windows-Computer mit einer Vorlage Ressourcenmanager](virtual-machines-windows-ps-template.md) |
| Azure PowerShell | [Erstellen eines Windows virtuellen Computers mithilfe der PowerShell](virtual-machines-windows-ps-create.md) |
| Client-SDKs | [Bereitstellen von mit c# Azure-Ressourcen](virtual-machines-windows-csharp.md) |
| REST-APIs | [Erstellen oder Aktualisieren eines virtuellen Computers](https://msdn.microsoft.com/library/mt163591.aspx) |

Sie hoffen, dass es nie geschieht, aber gelegentlich etwas falschen geht. Wenn Sie diese Situation geschieht, prüfen Sie die Informationen in [Ressourcenmanager Behandeln von Problemen bei der Bereitstellung mit einem Windows-Computer in Azure erstellen](virtual-machines-windows-troubleshoot-deployment-new-vm.md)aus.

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Wie verwalte ich den virtuellen Computer an, die ich erstellt?

Virtuellen Computern können mithilfe einer browserbasiertes Portal Befehlszeile Tools mit Unterstützung für Scripting sicher sind, oder direkt über APIs verwaltet werden. Einige typische Verwaltungsaufgaben, die Sie möglicherweise ausführen abrufen-Befehl ein virtueller Computer, Sicherungskopien erstellen und Verwalten der Verfügbarkeit von Informationen eines virtuellen Computers.

### <a name="get-information-about-a-vm"></a>Erhalten von Informationen zu eines virtuellen Computers

In dieser Tabelle werden einige der Methoden, die Sie die Informationen eines virtuellen Computers zugreifen können.

| Methode | Beschreibung |
| ------ | ----------- |
| Azure-portal | Klicken Sie im Menü Hub klicken Sie auf **virtuellen Computern** , und wählen Sie dann den virtuellen Computer in der Liste aus. Klicken Sie auf das Blade für den virtuellen Computer haben Sie Zugriff auf einen Überblick erhalten, Kennzahlen für die Überwachung und Festlegen von Werten. |
| Azure PowerShell | Informationen zur Verwendung von PowerShell zum Verwalten von virtuellen Computern finden Sie unter [Verwalten von Azure virtuellen Computern Ressourcenmanager und PowerShell zu verwenden](virtual-machines-windows-ps-manage.md). |
| REST-API | Verwenden Sie die [erste virtueller Computer Informationen](https://msdn.microsoft.com/library/mt163682.aspx) -Operation um Informationen eines virtuellen Computers zu erhalten. |
| Client-SDKs | Informationen zur Verwendung von C#-virtuellen Computern verwalten finden Sie unter [Verwalten von Azure virtuellen Computern verwenden Azure Ressourcenmanager und c#](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Melden Sie sich an den virtuellen Computer

Sie verwenden die Schaltfläche verbinden im Azure-Portal zum [Starten einer Sitzung RDP (Remote Desktop)](virtual-machines-windows-connect-logon.md). Etwas können manchmal schief geht, bei dem Versuch, eine Verbindung zu verwenden. Wenn Sie diese Situation geschieht, schauen Sie sich die Hilfeinformationen in [Remotedesktop Problembehandlung bei Verbindungen mit einer Azure-virtuellen Computern unter Windows](virtual-machines-windows-troubleshoot-rdp-connection.md).

### <a name="manage-availability"></a>Verwalten der Verfügbarkeit

Es ist wichtig für Sie zu verstehen, wie Sie [Sicherstellen einer hohen Verfügbarkeit](virtual-machines-windows-manage-availability.md) für eine Anwendung. Diese Konfiguration umfasst das Erstellen von mehreren virtuellen Computern, um sicherzustellen, dass Sie mindestens eines ausgeführt wird.

In der Reihenfolge für die Bereitstellung für unsere 99.95 virtueller Computer Ebene Servicevertrag qualifizieren müssen Sie zwei oder mehr virtuellen Computern ausführen Ihrer Arbeitsbelastung innerhalb einer [Verfügbarkeit festlegen](virtual-machines-windows-infrastructure-availability-sets-guidelines.md)bereitstellen. Diese Konfiguration stellt sicher, Ihre virtuellen Computern sind über mehrere Fehlerstrukturanalyse Domänen verteilt und auf Hosts mit anderen Wartung Windows bereitgestellt werden. Die vollständige [Azure Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wird erläutert, die garantierte Verfügbarkeit von Azure als Ganzes.
 
### <a name="back-up-the-vm"></a>Sichern der virtuellen Computer
 
Ein [Wiederherstellung Services Tresor](../backup/backup-introduction-to-azure-backup.md) dient zum Schützen von Daten und Ressourcen sowohl Azure Sicherung und Wiederherstellung der Azure-Website-Dienste. Sie können eine Wiederherstellung Services Tresor zum [Bereitstellen und Verwalten von Sicherungskopien für Ressourcenmanager bereitgestellt virtuelle Computer mithilfe der PowerShell](../backup/backup-azure-vms-automation.md)verwenden. 

## <a name="next-steps"></a>Nächste Schritte

- Ist für die Arbeit mit Linux virtuellen Computern Ihrer Absicht prüfen Sie [Azure und Linux](virtual-machines-linux-azure-overview.md).
- Weitere Informationen zu den Richtlinien, um Ihre Infrastruktur in das [Beispiel Azure Infrastruktur Exemplarische Vorgehensweise](virtual-machines-windows-infrastructure-example.md)einrichten.
- Stellen Sie sicher, dass Sie die [Bewährte Methoden für die Ausführung eines Windows virtuellen Computers auf Azure](virtual-machines-windows-guidance-compute-single-vm.md)folgen.


