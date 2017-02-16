<properties
   pageTitle="Wiederherstellung Services Vaulting häufig gestellte Fragen zu | Microsoft Azure"
   description="Diese Version von den häufig gestellten Fragen unterstützt Public Preview-Version des Diensts Azure sichern. Antworten auf häufig gestellte Fragen zu der Sicherungsdatei Agent, Sicherung und Aufbewahrung, Wiederherstellung, Sicherheit und andere häufig gestellte Fragen zur Azure Sicherung-Lösung."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="zusätzliche Lösung; zusätzliche service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Wiederherstellung Services Tresor – häufig gestellte Fragen


Dieser Artikel enthält Informationen, die speziell für Wiederherstellung Services Tresor, und es dient als Ergänzung der [Azure Sicherung häufig gestellte Fragen](backup-azure-backup-faq.md). Der Azure Sicherung häufig gestellte Fragen zu stellt den vollständigen Satz von Fragen und Antworten zum Dienst Azure Sicherung.  

Sie können Fragen zur Azure-Sicherung im Abschnitt Disqus in diesem Artikel oder eine verwandte Artikel. Sie können auch Fragen zu den Dienst Azure Sicherung im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)Posten.

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Wiederherstellung Services Depots sind Ressourcenmanager basiert. Werden Sicherung Depots (klassischen Modus) werden weiterhin unterstützt? <br/>
Ja, werden die Sicherung Depots weiterhin unterstützt. Erstellen Sie im [Portal klassischen](https://manage.windowsazure.com)Sicherung Depots. Erstellen Sie Wiederherstellung Services Depots im [Azure-Portal](https://portal.azure.com)an. Jedoch wird dringend empfohlen, Sie zum Erstellen der Wiederherstellung Services Tresor als alle zukünftigen Erweiterungen nur in Tresor Wiederherstellung Services sind verfügbar.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Kann ich eine Sicherung Tresor in einer Wiederherstellung Services Tresor migrieren? <br/>
Leider kann nicht Nein, zu diesem Zeitpunkt den Inhalt einer Sicherung Wölbung in einem Tresor Wiederherstellung Services zu migrieren. Wir arbeiten an diese Funktionalität hinzufügen, aber es ist nicht verfügbar als Teil des Public Preview-Version.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Unterstützen Wiederherstellung Services Depots klassische virtuellen Computern oder Grundlage Ressourcenmanager virtuellen Computern? <br/>
Wiederherstellung Services Depots unterstützen beide Modelle.  Sie können eine Sicherungskopie von eines virtuellen Computers in der klassischen-Portal (die klassischen Modus virtuellen Computern sind) erstellt oder ein virtuellen Computers erstellt im Azure-Portal (die Grundlage Ressourcenmanager sind) zu einem Tresor Wiederherstellung Services.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Ich haben mein klassischen virtuellen Computern in Sicherung Tresor gesichert. Ich möchte nun meine virtuellen Computern klassischen Modus in Ressourcenmanager Modus migrieren.  Wie kann ich diese in Wiederherstellung Services Tresor sichern?
Klassische virtuellen Computern in Sicherung Tresor Sicherungen wird nicht automatisch auf Wiederherstellung Services Tresor migrieren, beim Migrieren der virtuellen Computern von klassischen Modus Ressourcenmanager. Führen Sie diese Schritte für die Migration von virtuellen Computer Sicherungskopien:

1. Wechseln Sie zur Registerkarte **Geschützte Elemente** Sicherung Tresor und wählen Sie den virtuellen Computer aus. Klicken Sie auf [Beenden des Schutzes](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Lassen Sie *zugeordnete Sicherung Daten löschen* Option **nicht aktiviert**.
2. Migrieren des virtuellen Computers klassischen Ansicht auf Ressourcenmanager Modus. Stellen Sie sicher, dass Speicher und Netzwerk virtuellen Computern entspricht Ressourcenmanager Modus ebenfalls migriert werden.
3. Erstellen Sie einer Wiederherstellungsdatei Services Tresor und konfigurieren Sie die Sicherung auf dem migrierte virtuellen Computer mithilfe der **Sicherungsdatei** Aktion auf Tresor Dashboard. Weitere Informationen zum [Sichern in Wiederherstellung Services Tresor aktivieren](backup-azure-vms-first-look-arm.md)
