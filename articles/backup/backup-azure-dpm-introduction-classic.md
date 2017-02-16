<properties
    pageTitle="Einführung in Azure DPM Sicherung | Microsoft Azure"
    description="Einführung in die DPM-Servern mithilfe des Diensts für Azure Sicherung sichern"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="System Center Data Protection Manager, Daten Schutz-Manager und Dpm Sicherung"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Vorbereiten der Auslastung in Azure mit DPM sichern

> [AZURE.SELECTOR]
- [Azure Sicherung Server](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure Sicherung Server (klassisch)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klassisch)](backup-azure-dpm-introduction-classic.md)


Dieser Artikel enthält eine Einleitung zur Verwendung von Microsoft Azure Sicherung zum Schutz von System Center Data Protection Manager (DPM) Servers und Auslastung. Indem sie lesen, werden Sie verstehen:

- Funktionsweise von Azure DPM-Server-Sicherung
- Die erforderlichen Komponenten, um eine reibungslose Sicherung Erfahrung zu erzielen.
- Die normalen aufgetretenen Fehler und wie diese behandelt
- Unterstützte Szenarien

System Center DPM sichert Datei- und Anwendung Daten aus. Daten gesichert, um DPM können auf Band, auf einem Datenträger gespeichert oder in Azure mit Microsoft Azure Sicherung gesichert werden. DPM interagiert mit Azure Sicherung wie folgt ein:

- **Bereitstellung von DPM als eine physische Server- oder lokalen virtuellen Computern** – Wenn DPM bereitgestellt wird als eine physische Server oder eine lokale Hyper-V virtuellen Computern können Sie Daten in einer Sicherungskopie Azure Tresor zusätzlich Festplatte und Band sichern Sicherung.
- **DPM als eine Azure-virtuellen Computern bereitgestellt** – von System Center 2012 R2 mit Update 3, DPM als eine Azure-virtuellen Computern bereitgestellt werden kann. Wenn DPM als eine Azure-virtuellen Computern bereitgestellt wird, die Daten in Azure Datenträger sichern können, die die DPM Azure-virtuellen Computern angefügt, oder Sie können den Datenspeicher auslagern, indem sie auf eine Sicherung Azure Tresor sichern.

## <a name="why-backup-your-dpm-servers"></a>Warum sichern Server?

Die Business mit Azure Sicherung zum Sichern von DPM-Servern folgende Vorteile:

- Für lokal DPM-Bereitstellung können Sie als Alternative zum langfristiges Bereitstellung auf Band Azure Sicherung.
- Für DPM Bereitstellungen in Azure ermöglicht Azure Sicherung Auslagern Speicher vom Azure Datenträger, was Sie Skalierung durch ältere Daten in Azure sichern und neue Daten auf dem Datenträger speichern.

## <a name="how-does-dpm-server-backup-work"></a>Wie funktioniert die DPM-Server-Sicherung?
Um eine Sicherungskopie eines virtuellen Computers zu erstellen, ist zum ersten Mal eine Point-in-Time-Momentaufnahme der Daten benötigt. Die Sicherung Azure Service initiiert den Auftrag Sicherung zum geplanten Zeitpunkt und löst die Sicherung Erweiterung um eine Momentaufnahme erstellen. Die Sicherung Erweiterung koordiniert mit dem Dienst in Gast VSS, um Konsistenz zu erreichen, und wird die Blob Snapshot-API von der Azure-Speicherdienst aufgerufen, sobald Konsistenz erreicht ist. Dadurch wird eine einheitliche Momentaufnahme der Datenträger des virtuellen Computers, erhalten, ohne ihn zu beenden.

Nachdem der Momentaufnahme werden die Daten durch die Sicherung Azure Service zum Sicherung Tresor übertragen. Der Dienst sorgt identifizieren sowie zum Übertragen von nur die Blöcke, die von der letzten Sicherungskopie der Sicherungskopien Speicher und Netzwerk effizienter geändert wurden. Wenn die Datenübertragung abgeschlossen ist, wird der Snapshot entfernt und ein Wiederherstellungspunkts erstellt. In der klassischen Azure-Portal kann geleisteten Wiederherstellung angezeigt werden.

>[AZURE.NOTE] Für Linux-virtuellen Computern ist nur Datei konsistente Sicherung möglich.

## <a name="prerequisites"></a>Erforderliche Komponenten
Bereiten Sie Azure Sicherung sichern Sie DPM Daten wie folgt vor:

1. **Erstellen einer Sicherungskopie Tresor** – Erstellen einer Tresor in der Sicherung von Azure-Verwaltungskonsole.
2. **Anmeldeinformationen für den Download Tresor** – In Azure Sicherung auf den Tresor hochladen das Projektmanagement-Zertifikat, das Sie erstellt haben.
3. **Installieren der Sicherungsdatei Azure-Agent und Registrieren des Servers** – aus Azure Sicherung, installieren Sie den Agent auf jedem DPM-Server und DpmPathMerge in die Sicherung Tresor registrieren.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Anforderungen (und Einschränkungen)

- DPM kann als physische Server oder einem Hyper-V virtuellen Computer installiert ist, klicken Sie auf System Center 2012 SP1 oder System Center 2012 R2 ausgeführt werden. Sie können auch als eine Azure-virtuellen Computern ausführen auf System Center 2012 R2 mit mindestens ausgeführt werden DPM 2012 R2 Update Rollup 3 oder in VMWare ausführen auf System Center 2012 R2 mit mindestens einem Windows-Computer Update Rollup 5.
- Wenn Sie DPM mit System Center 2012 SP1 ausführen, sollten Sie Update Rollup 2 für System Center Data Protection Manager SP1 installieren. Dies ist erforderlich, bevor Sie die Sicherung Azure-Agent installieren können.
- DpmPathMerge haben sollten, Windows PowerShell und .net Framework 4.5 installiert.
- DPM kann die meisten Auslastung auf Azure Sicherung sichern. Eine vollständige Liste der was finden Sie unter Unterstützte weist unterstützen die Sicherung Azure unten aufgeführten Elemente.
- In Azure Sicherung gespeicherte Daten können nicht mit der Kopieroption "auf Band" wiederhergestellt werden.
- Sie benötigen ein Azure-Konto mit der Sicherung Azure-Feature aktiviert. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen zum [Azure Sicherung Preise](https://azure.microsoft.com/pricing/details/backup/).
- Azure Sicherung verwenden, muss der Azure Sicherung Agent auf den Servern installiert werden, die Sie sichern möchten. Jeder Server müssen mindestens 10 % der Größe der Daten, die als lokale freier Speicherplatz verfügbar gesichert wird. Beispielsweise erfordert das Sichern von 100 GB an Daten mindestens 10 GB Speicherplatz Entwurfsbereich Speicherort aus. Während der Minimalwert 10 % ist, empfiehlt sich 15 % des kostenlosen lokalen Speicherplatz für den Cachespeicherort verwendet werden.
- Daten werden im Speicher Azure Tresor gespeichert werden. Es gibt keine Beschränkung für die Menge der Daten, die Sie nach einer Sicherung Azure Vaulting können jedoch die Größe einer Datenquelle (beispielsweise einer virtuellen Computern oder Datenbank) dürfen nicht 54,400 GB überschreiten.

Dieser Dateitypen werden für wieder nach Zeitphasen bis zum Azure unterstützt:

- Verschlüsselte (vollständige Sicherung)
- Komprimiert (inkrementelle Sicherungskopien unterstützt)
- Sparse (inkrementell Sicherungskopien unterstützt)
- Es komprimiert und gering (als Sparse behandelt)

Und diese werden nicht unterstützt:

- Servern Betriebssystemen Datei Groß-/Kleinschreibung beachtet werden nicht unterstützt.
- Feste Links (übersprungen)
- Analysepunkte (übersprungen)
- Verschlüsselte und komprimierte (übersprungen)
- Verschlüsselte und gering (übersprungen)
- Komprimierte stream
- Gering gefüllte stream

>[AZURE.NOTE] Aus können in System Center 2012 DPM mit SP1 oder höher, Sie Einrichten von DPM in Azure mit Microsoft Azure Sicherung geschützten Auslastung sichern.
