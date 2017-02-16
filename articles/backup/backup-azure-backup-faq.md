<properties
   pageTitle="Zusätzliche häufig gestellte Fragen zur Azure | Microsoft Azure"
   description="Antworten auf häufig gestellte Fragen zur Sicherung Dienst, Sicherung Agent, Sichern und Aufbewahrungsrichtlinien, Wiederherstellung, Sicherheit und andere häufig gestellte Fragen zu sichern und Disaster Wiederherstellung."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="Sichern und Disaster Wiederherstellung; zusätzliche service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; giridham; arunak; markgal; jimpark;"/>

# <a name="azure-backup-service--faq"></a>Azure Sicherung Service – häufig gestellte Fragen zu


In diesem Artikel wird eine Liste mit häufig gestellte Fragen (und die entsprechenden Antworten) zum Sichern von Azure Service. Unsere Community Antworten schnell, und wenn eine Frage häufig gestellt wird, wir dieses Artikels. Die Antworten auf Fragen in der Regel Verweis bereitstellen oder Informationen zu unterstützen. Sie können Fragen zu Azure Sicherung im Abschnitt Disqus in diesem Artikel oder eine verwandte Artikel stellen. Sie können auch Fragen zu den Dienst Azure Sicherung im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)Posten.


## <a name="what-is-the-list-of-supported-operating-systems-from-which-i-can-back-up-to-azure-using-azure-backup-br"></a>Was ist die Liste der unterstützten Betriebssysteme, aus denen ich in Azure mit Azure Sicherung sichern können? <br/>
Azure Sicherung unterstützt die folgende Liste der Betriebssysteme für Dateiordner Sicherung Anwendung sichern Sicherung Azure-Server und SCDPM verwenden.

| Betriebssystem        | Plattform           | SKU  |
| :------------- |-------------| :-----|
| Windows 8 und neuesten SPs      | 64-bit | Enterprise, Pro |
| Windows 7 und neuesten SPs      | 64-bit | Ultimate, Enterprise, Professional, Home Premium-Home Basic Starter |
| Windows 8.1 und neuesten SPs | 64-bit      |    Enterprise, Pro |
| Windows-10      | 64-bit | Enterprise, Pro, Start |
|Windows Server 2012 R2 und neuesten SPs| 64-bit| Standard, Datacenter, Foundation|
|Windows Server 2012 und neuesten SPs|    64-bit| Datacenter, Foundation, Standard|
|Windows Storage Server 2012 R2 und neuesten SPs  |64-bit|    Standard, Arbeitsgruppe|
|Windows-Speicher Server 2012 und neuesten SPs |64-bit |Standard, Arbeitsgruppe
|Windows Server 2012 R2 und neuesten SPs  |64-bit|    Grundlegende|
|Windows Server 2008 R2 SP1 |64-bit|    Standard, Enterprise, Datacenter, Foundation|
|Windows Server 2008 SP2    |64-bit|    Standard, Enterprise, Datacenter, Foundation|

Für Azure-virtuellen Computer Sicherung

- **Linux**: Azure Sicherung unterstützt, [eine Liste der Verteilung, die von Azure unterstützt werden](../virtual-machines/virtual-machines-linux-endorsed-distros.md) , mit Ausnahme von Core OS Linux.  Andere schalten-Your-Besitzer-Linux-Versionen auch möglicherweise arbeiten, solange des virtuellen Computer-Agents auf dem virtuellen Computer zur Verfügung und Unterstützung für Python vorhanden ist.
- **Windows Server**: Versionen, die älter als Windows Server 2008 R2 werden nicht unterstützt.

## <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Wo kann ich den neuesten Sicherung Azure-Agent herunterladen? <br/>
Sie können den neuesten Agent zum Sichern von Windows Server, System Center DPM oder Windows-Client, von [hier](http://aka.ms/azurebackup_agent)herunterladen. Wenn Sie einen virtuellen Computer sichern möchten, verwenden Sie den virtuellen Computer-Agent (der die richtige Erweiterung automatisch installiert wird). Virtueller Computer-Agent ist bereits auf virtuellen Computern aus dem Katalog Azure erstellt.

## <a name="which-version-of-scdpm-server-is-supported-br"></a>Welche Version von SCDPM Server unterstützt wird? <br/>
Es empfiehlt sich, dass die Installation der [neuesten](http://aka.ms/azurebackup_agent) Sicherung Azure-Agents auf das aktuelle Rollup der SCDPM (UR11 ab August 2016)

## <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>Beim Konfigurieren des Sicherung von Azure-Agents werde ich aufgefordert, die Anmeldeinformationen Tresor einzugeben. Ablaufen gehen Sie wie folgt Tresor Anmeldeinformationen?
Ja, Ablauf die Anmeldeinformationen Tresor nach 48 Stunden. Wenn die Datei läuft ab, melden Sie sich bei der Azure-Portal und Laden Sie die Tresor Anmeldeinformationen Dateien aus Ihrem Tresor.

## <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Bestehen Einschränkungen hinsichtlich der Anzahl der Depots, die in jedem Azure Abonnement erstellt werden können? <br/>
Ja. Ab September 2016 können Sie 25 Sicherung Depots pro Abonnement erstellen. Sie können bis zu 25 Wiederherstellung Services Depots pro jeden unterstützten Bereich des Azure Sicherung pro Abonnement erstellen. Wenn Sie mehr Depots benötigen, erstellen Sie ein neues Abonnement.

## <a name="are-there-any-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Gibt es Grenzwerte für die Anzahl der Servern/Maschinen, die für jeden Tresor registriert werden können? <br/>
Ja, können Sie bis zu 50 Maschinen pro Tresor registrieren. Bei IaaS Azure-virtuellen Computern beträgt Beschränkung 200 virtuellen Computern pro Tresor. Wenn Sie mehrere Maschinen registrieren müssen, erstellen Sie eine neue Tresor.

## <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Wie registriere ich meine Server zu einem anderen Datacenter?<br/>
Zusätzliche Daten werden in das Rechenzentrum von der Tresor gesendet, dem sie registriert ist. Die einfachste Möglichkeit zum Ändern der Datacenter besteht darin zu deinstallieren Sie den Agent und den Agent und registrieren, um eine neue Tresor, zu der gewünschten Datacenter gehört.

## <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Was passiert, wenn ich einen WindowsServer umbenennen, der Daten in Azure sichern ist?<br/>
Wenn Sie auf einen Server umbenennen, werden alle derzeit konfigurierte Sicherungskopien beendet.
Sie müssen den neuen Namen des Servers mit dem Tresor Sicherung zu registrieren. Beim Erstellen einer neuen Registrierung ist der erste Sicherung Vorgang eine vollständige Sicherung und keine inkrementell sichern. Wenn Sie zum Wiederherstellen von Daten, die zuvor zum Tresor mit den alten Servernamen gesichert wurde benötigen, können Sie die Daten, die Option für [**einen anderen Server**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) verwenden, klicken Sie im Assistenten **Wiederherstellen** wiederherstellen.

## <a name="what-types-of-drives-can-i-backup-files-and-folders-from-br"></a>Welche Arten von Festplatten kann ich Dateien und Ordner aus sichern? <br/>
Die folgende Gruppe von Laufwerke/Datenmengen zugreifen nicht Sicherung:

- Wechselmedien: Das Laufwerk muss eine feste werden zusätzliche Elementquelle verwendete melden.
- Schreibgeschützte Datenmengen: die Lautstärke für den Schatten-Dienst (VSS) für Funktion nicht schreibgeschützt sein.
- Offline Datenmengen: Die Lautstärke muss für VSS Funktion online sein.
- Netzwerkfreigabe: die Lautstärke muss lokal auf dem Server mit online-Sicherung gesichert werden.
- BitLocker geschützten Datenmengen: die Lautstärke muss nicht gesperrte sein, bevor die Sicherung durchgeführt werden kann.
- Datei System Kennung: NTFS ist nur für diese Version von der Sicherungsdatei Onlinedienst unterstützt Dateisystem.

## <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Welche Arten von Dateien und Ordner kann ich von meinem Server sichern?<br/>
Die folgenden Datentypen werden unterstützt:

- Verschlüsselt
- Komprimiert
- Gering
- Komprimierte + gering
- Feste Links: Nicht unterstützt, übersprungen
- Erneute Analyse Punkt: Nicht unterstützt, übersprungen
- Verschlüsselte + komprimiert: Nicht unterstützt, wird übersprungen
- Verschlüsselte + gering gefüllte: Nicht unterstützt, wird übersprungen
- Komprimierte Stream: Nicht unterstützt, übersprungen
- Gering gefüllte Stream: Nicht unterstützt, übersprungen

## <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Was ist die Anforderung Mindestgröße für den Cacheordner? <br/>
Die Größe des Cacheordners bestimmt die Menge der Daten, die Sie möchten sichern. Cacheordners sollte 5 % des Speicherplatzes für Datenspeicher erforderlich sein.

## <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Wenn meine Organisation ein Tresor sind, können wie ich eine Server-Daten aus einem anderen Server isolieren, wenn Daten wiederherstellen?<br/>
Alle Server, die zum gleichen Tresor registriert sind, können durch andere Server *verwenden, die Sie identische Kennwörter*gesicherten Daten wiederherstellen. Wenn Sie über Server, deren zusätzliche Daten, die Sie von anderen Servern in Ihrer Organisation isolieren möchten verfügen, verwenden Sie ein vorgesehenen Kennwort für diese. Beispielsweise könnten Personalwesen Servern eine Verschlüsselung Kennwort, accounting Servers ein anderes und Speicher-Servern eine dritte verwenden.

## <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Kann ich "Meine zusätzliche Daten oder Tresor zwischen Abonnements migrieren"? <br/>
Nein. Der Tresor Ebene eines Abonnements erstellt und kann nicht zu einem anderen Abonnement zugewiesen werden, nachdem sie erstellt wurde.

## <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Funktioniert die Sicherung Azure-Agents auf einem Server, die Windows Server 2012 Deduplication verwendet? <br/>
Ja. Der Agent-Dienst konvertiert die deduplizierte Daten in normalen Daten an, wenn sie den Vorgang Sicherung vorbereitet. Klicken Sie dann die Daten für die Sicherung optimiert, verschlüsselt die Daten und sendet die verschlüsselten Daten an die Sicherung Onlinedienst.

## <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Wenn ich eine Sicherung abbrechen, nachdem er gestartet wurde, werden die Sicherung übertragenen Daten gelöscht? <br/>
Nein. Der Sicherung Tresor speichert die gesicherten Daten, die auf den Punkt, der die Kündigung weitergeleitet wurde. Azure Sicherung wird ein Verfahren Wissensstand gelegentlich Kontrollpunkten der Sicherung von Daten hinzufügen, während die Sicherung verwendet. Da Kontrollpunkten in den gesicherten Daten vorhanden sind, kann in der nächste Sicherung Prozess die Integrität der Dateien überprüfen. Die nächste Sicherung ausgelöst wäre inkrementell über die Daten, die zuvor gesichert wurden hatte. Eine Sicherung inkrementelle bietet eine bessere Nutzung der Bandbreite, damit Sie nicht benötigen, wiederholt dieselben Daten zu übertragen.

Bei Azure-virtuellen Computer sichern Nachdem Sie der Auftrag abgebrochen wird, übertragene Daten ignoriert und neue Sicherungskopie inkrementelle Daten aus zuvor erfolgreichen Sicherung übertragen.

## <a name="why-am-i-seeing-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-had-scheduled-regular-backups-previously-br"></a>Warum werden die Warnung "Azure Sicherungskopien nicht für diesen Server konfiguriert wurden" angezeigt, obwohl regelmäßigen Sicherungen zuvor geplant hatten? <br/>
Diese Warnung tritt auf, wenn die Sicherungsdatei Terminplan Einstellungen auf dem lokalen Server gespeicherten nicht die Einstellungen im Sicherung Tresor gespeichert sind. Wenn der Server oder die Einstellungen zu einem sicher funktionierenden Zustand wiederhergestellt wurde, können die Sicherung Zeitpläne nicht mehr synchronisieren. Wenn Sie diese Warnung, [die Sicherung Richtlinie umkonfigurieren](backup-azure-manage-windows-server.md) und dann **Führen Sie jetzt sichern** um wieder zu den lokalen Server mit Azure synchronisieren erhalten.

## <a name="what-firewall-rules-should-be-configured-for-azure-backup-br"></a>Welche Firewall Regeln für die Sicherung Azure konfiguriert werden sollte? <br/>
Nahtlose Schutz auf lokale-zum Azure und Arbeitsbelastung-Azure-Daten empfiehlt es sich, dass Sie Ihre Firewall zur Kommunikation mit den folgenden URLs zulassen:

- www.msftncsi.com
- \*. Microsoft.com
- \*. WindowsAzure.com
- \*. microsoftonline.com
- \*. Windows

##<a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Kann ich den Sicherung Azure-Agent eine Azure virtuellen Computers bereits gesicherten vom Dienst Azure Sicherung mit der Erweiterung virtueller Computer installieren? <br/>
Absolut. Azure Sicherung bietet virtueller Computer Ebene Sicherung für Azure-virtuellen Computern Erweiterung virtuellen Computer verwenden. Sie können den Sicherung Azure-Agent auf einen Windows-Betriebssystem Gast zum Schützen von Dateien und Ordnern auf diesen Gast OS installieren.

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Kann ich den Sicherung Azure-Agent eine Azure-virtuellen Computers Sichern von Dateien und Ordnern präsentieren auf temporäre Speicherung von den Azure-virtuellen Computer bereitgestellten installieren? <br/>
Sie können den Sicherung Azure-Agent auf das Windows-Betriebssystem Gast installieren und Sichern von Dateien und Ordner in den temporären Speicher. Beachten Sie jedoch, dass Sicherungskopien fehl, sobald temporäre Speicherung Daten endgültig gelöscht werden. Auch, wenn die temporäre Speicherung Daten gelöscht wurden, können Sie nur zu permanenten Speicher wiederherstellen.

## <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-scdpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Ich habe Azure Sicherung Agent zum Schützen von Meine Dateien und Ordner installiert. Kann ich für die Arbeit mit Azure Sicherung Agent zum Schutz der lokalen Anwendung/virtueller Computer Auslastung in Azure SCDPM jetzt installieren? <br/>
Um Azure Sicherung mit SCDPM verwenden zu können, wird empfohlen SCDPM zuerst und erst dann installieren, um Azure Sicherung Agent installieren. Nahtlose Integration von der Sicherung von Azure-Agent mit SCDPM: Damit ist sichergestellt, und Schützen von Dateien/Ordnern, -Auslastung und virtuellen Computern in Azure, direkt von der Verwaltungskonsole der SCDPM ermöglicht. Installieren nach der Installation von Azure Sicherung SCDPM ist Agent Zwecken weiter oben erwähnten nicht empfohlen oder unterstützt.

## <a name="what-is-the-length-of-file-path-that-can-be-specified-as-part-of-azure-backup-policy-using-azure-backup-agent-br"></a>Was ist die Länge der Dateipfad, kann als Teil der Sicherung Azure-Richtlinie unter Verwendung von Azure Sicherung Agent angegeben werden? <br/>  
Azure Sicherung Agent basiert auf NTFS. Der [Dateipfad Länge Spezifikation von Windows-API zugänglich ist](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Bei Sichern von Dateien mit Datei Pfadlänge größer als die von Windows-API angegeben haben, können Kunden den übergeordneten Ordner oder dem Festplattenlaufwerk des Sicherungsdateien sichern.  

## <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Welche Zeichen dürfen im Dateipfad für die Sicherung Azure Richtlinie Azure Sicherung Agent verwenden? <br>  
 Azure Sicherung Agent basiert auf NTFS. Sie können als Teil der Dateispezifikation [NTFS unterstützt Zeichen](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) .  

## <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Kann ich zusätzliche Azure-Server zum Erstellen einer Sicherungskopie absolut erforderlichen Metall Wiederherstellung mithilfe für einen physischen Server verwenden? <br/>
Ja.

## <a name="can-i-configure-the-backup-service-to-send-mail-if-a-backup-job-fails-br"></a>Kann ich die Sicherung Service zum Senden von Nachrichten, wenn ein Sicherung Auftrag fehlschlägt konfigurieren? <br/>
Ja, hat der Dienst Sicherung mehrere Ereignis-basierte Alarme, die mit einem Powershellskript verwendet werden können. Eine vollständige Beschreibung finden Sie unter [-Benachrichtigung](backup-azure-manage-vms.md#alert-notifications)

## <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Gibt es eine Beschränkung auf die Größe der einzelnen Datenquellen zu sichernden? <br/>
Während der Ebene Tresor ist nicht begrenzt auf die Menge der Daten, die Sie sichern können, Azure Sicherung vorgangseinschränkung eine Einschränkung (für alle praktischen Zwecke diese Grenzwerte sind sehr hoch) auf die maximale Größe der Datenquelle. Seit August 2015 die ist der maximale Größe Datenquelle für die unterstützten Betriebssysteme aus:

|S.No | Betriebssystem |  Maximale Größe der Datenquelle |
| :-------------: |:-------------| :-----|
|1| WindowsServer 2012 oder höher| 54400 GB|
|2| Windows 8 oder höher| 54400 GB|
|3| WindowsServer 2008, Windows Server 2008 R2 | 1700 GB|
|4| Windows 7 | 1700 GB|

In der folgenden Tabelle wird erläutert, wie die Größe jeder Datenquelle bestimmt wird.

|   Datenquelle  |   Details |
| :-------------: |:-------------|
|Lautstärke |Die Datenmenge von einzelnen Volume eines Server- oder Client Computers gesichert|
|Hyper-V-Computer | Die Summe der Daten für alle virtuellen Festplatten des virtuellen Computers zu sichernden|
|Microsoft SQL Server-Datenbank | Größe eines einzelnen SQL-Datenbank zu sichernden |
|Microsoft SharePoint |Summe der Inhalts- und Datenbanken innerhalb einer SharePoint-Farm zu sichernden|
|Microsoft Exchange |Die Summe aller Exchange-Datenbanken in einen Exchange-Server zu sichernden|
|BMR/Systemstatus |Jede einzelne Kopie BMR oder im externen System Status des Computers zu sichernden|

## <a name="are-there-limits-on-the-number-of-times-a-backup-job-can-be-scheduled-per-daybr"></a>Gibt es Grenzwerte für die Anzahl der oft, die eine Sicherung pro Tag geplant werden kann?<br/>
Ja, können Sie zusätzliche Aufträge auf Ausführen Windows Server oder Windows-Client bis zu dreimal pro Tag. Sie können zusätzliche Aufträge auf System Center DPM nach Zeitphasen bis zum zweimal täglich ausführen. Sie können eine Sicherung für IaaS virtuellen Computern täglich ausführen.

## <a name="is-there-a-difference-between-the-scheduling-policy-for-dpm-and-windows-server-ie-on-windows-server-without-dpm-br"></a>Gibt es einen Unterschied zwischen der Planung Richtlinie für DPM und Windows Server (d. h. auf Windows Server ohne DPM)? <br/>
Ja. DPM können Sie täglich, wöchentlich, Monats- und Jahreskalender mit Zeitpläne angeben. Windows Server (ohne DPM) können Sie nur tägliche oder wöchentliche Zeitpläne angeben.

## <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-ie-on-windows-server-without-dpmbr"></a>Gibt es einen Unterschied zwischen die Aufbewahrungsrichtlinie für DPM und Windows Server-Client (d. h. auf Windows Server ohne DPM)?<br/>
Nein, beide DPM und Windows Server/Client haben täglich, wöchentlich, monatlich und jährlich Aufbewahrungsrichtlinien.

## <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Kann ich meine Aufbewahrungsrichtlinien, die Richtlinien Selektives – d. h. wöchentlich konfigurieren und täglich, aber nicht jährlich und monatlich konfigurieren?<br/>
Ja, können mit die Struktur Azure Sicherung Aufbewahrung Sie vollständige Flexibilität beim Definieren der Aufbewahrungsrichtlinie gemäß Ihren Anforderungen haben.

## <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Kann ich "Planen einer Sicherung" 6 Uhr und angeben "Aufbewahrungsrichtlinien" zu einem anderen Zeitpunkt?<br/>
Nein. Aufbewahrungsrichtlinien können nur auf zusätzliche Punkte angewendet werden. In der folgenden Abbildung wird die Aufbewahrungsrichtlinie für Sicherungskopien bei 00 Uhr und 18 Uhr erfasste angegeben. <br/>

![Sicherung planen und Aufbewahrungsrichtlinien](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled-br"></a>Wird eine Kopie inkrementelle für die geplante Aufbewahrungsrichtlinien übertragen? <br/>
Die inkrementelle Kopie wird Nein, klicken Sie auf die Uhrzeit angegeben ist, in der Seite Zeitplan Sicherung basierend gesendet. Die Punkte, die beibehalten werden können werden basierend auf die Aufbewahrungsrichtlinie bestimmt.

## <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Wenn eine Sicherung bei einem langen erhalten bleibt, dauert es mehr Zeit zum Wiederherstellen eines älteren Datenpunkts? <br/>
 Nein, ist die Zeit für die älteste wiederherstellen oder den neuesten Punkt identisch. Jede Wiederherstellungspunkt verhält sich wie eine vollständige Punkt.

## <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Ist jede Wiederherstellungspunkt wie eine vollständige Punkt, wirkt sich diese auf den Gesamtspeicher berechenbaren Sicherung aus?<br/>
Typische langfristiges Aufbewahrung Point-Produkte speichern zusätzliche Daten als vollständigen Punkte. Die vollständige Punkte sind Speicher *nicht effizient* jedoch schneller und wiederherstellen. Inkrementelle Kopien sind Speicher *effizient* , doch ist es erforderlich, eine Kette von Daten wiederherstellen, die Wiederherstellungszeit wirkt sich auf. Azure Sicherung Speicherarchitektur bietet Ihnen die Vorteile beider durch optimal Speichern von Daten für schnelle Wiederherstellung und niedrig Speicherkosten anfallen. Dadurch Speicher Daten stellen Sie sicher, dass Ihre eingehende und Ausgang Bandbreite effizient verwendet wird. Sowohl die Daten Speicherplatz und die Zeit zum Wiederherstellen der Daten ist auf ein Minimum beschränkt. Erfahren Sie, dass weitere Informationen zum wie [inkrementell Sicherungskopien](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) speichern effizient sind.

## <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Gibt es eine Beschränkung auf die Anzahl der Wiederherstellungspunkte, die erstellt werden können?<br/>
Nein. Wir haben Grenzwerte für die Wiederherstellungsdatei Punkte ausgeschlossen. Sie können beliebig viele Wiederherstellungspunkte, wie Sie möchten erstellen.

## <a name="why-is-the-amount-of-data-transferred-in-backup-not-equal-to-the-amount-of-data-i-backed-upbr"></a>Warum wird die Menge der Daten in der Datenmenge ungleich Sicherung übertragen, die ich gesichert?<br/>
 Alle Daten, die aus Azure Sicherung Agent oder SCDPM oder Azure Sicherungsserver, gesichert werden, komprimiert und verschlüsselt werden, bevor er übertragen. Sobald die Komprimierung und Verschlüsselung angewendet wird, ist die Daten in der Sicherungsdatei Tresor 30-40 % kleiner.

## <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Gibt es eine Möglichkeit, die vom Dienst Sicherung verwendete Bandbreite anpassen?<br/>
 Ja, verwenden Sie die Option **Eigenschaften ändern** im Agent Sicherung Bandbreite anpassen. Passen Sie die Bandbreite und Uhrzeiten, wenn Sie die Bandbreite verwenden. Weitere Informationen finden Sie unter [Netzwerk Begrenzungsebene](../backup-configure-vault.md#enable-network-throttling).

## <a name="my-internet-bandwidth-is-limited-for-the-amount-of-data-i-need-to-back-up-is-there-a-way-i-can-move-data-to-a-certain-location-with-a-large-network-pipe-and-push-that-data-into-azure-br"></a>Meine Internetbandbreite ist für die Anzahl der zu sichernden benötigten Daten beschränkt. Gibt es eine Möglichkeit, die ich Daten zu einer bestimmten Stelle mit einem großen Netzwerk leiten, und drücken Sie die Daten in Azure wechseln können? <br/>
Sie können Daten sichern in Azure über das Standardverfahren online Sicherung, oder Sie können den Azure Import/Export-Dienst verwenden, um BLOB-Speicher in Azure Datenübertragung. Es gibt keine weiteren Methoden für die erste Sicherung Datum Azure-Speicher. Informationen zum Verwenden des Azure Import/Export-Diensts mit Azure Sicherung finden im Artikel [Offline Sicherung Workflow](backup-azure-backup-import-export.md) .

## <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Wie viele Wiederherstellung kann ich auf der Registerkarte Daten durchführen, die in Azure gesichert wird?<br/>
Es gibt keine Beschränkung für die Anzahl der Wiederherstellung aus Azure Sicherung aus.

## <a name="do-i-have-to-pay-for-the-egress-traffic-from-azure-data-center-during-recoveriesbr"></a>Muss ich mich während der Wiederherstellung für den Datenverkehr Ausgang Azure Data Center bezahlen?<br/>
 Nein. Der Wiederherstellung sind kostenlos, und Sie sind nicht für den Datenverkehr Ausgang belastet.

## <a name="is-the-data-sent-to-azure-encrypted-br"></a>Werden die Daten in Azure verschlüsselt werden gesendet? <br/>
Ja. Daten werden auf dem lokalen Server/Client/SCDPM Computer mit AES256 verschlüsselt und die Daten über einen sicheren HTTPS-Link gesendet werden.

## <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Befindet sich die gesicherten Daten auf Azure ebenfalls verschlüsselt?<br/>
 Ja. An Azure gesendeten Daten bleibt verschlüsselt (bei Rest). Microsoft die Sicherung Daten zu einem beliebigen Zeitpunkt nicht entschlüsseln. Azure-virtuellen Computer Sicherung Azure Sicherung beruht auf Verschlüsselung des virtuellen Computers d. h., wenn Ihre virtuellen Computer mithilfe von Azure Datenträger Verschlüsselung oder eine andere Verschlüsselung Technologie verschlüsselt ist, Azure Sicherungskopie, dass die Verschlüsselung verwendet, um Ihre Daten zu schützen.

## <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Was ist die minimale Länge des Verschlüsselungsschlüssels zur Sicherung Verschlüsselung verwendet? <br/>
 Der Schlüssel sollten mindestens 16 Zeichen.

## <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Was passiert, wenn ich die Verschlüsselungsschlüssels vergessen? Können die Daten wiederherstellen (oder) kann Microsoft die Daten wiederherstellen? <br/>
Der Schlüssel zum Verschlüsseln der Sicherungsdatei Daten ist nur lokal Kunden vorhanden. Microsoft behält keine Kopie in Azure und hat keinen Zugriff auf die-Taste. Wenn Sie der Kunden die Taste misplaces, kann nicht Microsoft die gesicherten Daten wiederherstellen.

## <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Wie ändere ich den Cachespeicherort für die Sicherung Azure-Agent angegeben?<br/>
 Wechseln Sie nacheinander durch die Liste mit Bildaufzählungszeichen unten, um den Cachespeicherort ändern möchten.
- Beenden Sie die Sicherung-Engine, indem Sie in ein erweitertes Eingabeaufforderungsfenster den folgenden Befehl ausführen:

  ```PS C:\> Net stop obengine```

- Verschieben Sie die Dateien nicht. Kopieren Sie stattdessen den Cache Speicherplatz Ordner auf einem anderen Laufwerk mit genügend Speicherplatz. Der ursprünglichen Cache Abstand kann nach Bestätigung, dass die Sicherungskopien arbeiten mit den neuen Cache Abstand entfernt werden.

- Aktualisieren Sie die folgenden Registrierungseinträge durch den Pfad zu den neuen Cache Speicherplatz Ordner ein.<br/>

|Registrierungspfad | Registrierungsschlüssel | Wert |
| ------ | ------- | ------|
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` | ScratchLocation | *Neuen Speicherort des Ordners cache* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` | ScratchLocation | *Neuen Speicherort des Ordners cache* |

- Starten Sie die Sicherung-Engine neu, indem Sie in ein erweitertes Eingabeaufforderungsfenster den folgenden Befehl ausführen:

  ```PS C:\> Net start obengine```

  Sobald die Sicherung Erstellung am neuen Speicherort des Caches erfolgreich abgeschlossen ist, können Sie den ursprünglichen Cacheordner entfernen.

## <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Wo kann ich den Cache-Ordner für den Azure Sicherung Agent erwartungsgemäß abgelegt habe?<br/>
Die folgenden Speicherorten für den Cache-Ordner werden nicht empfohlen:

- Freigeben oder Wechselmedien Netzwerk: der Cache-Ordner auf dem Server, die mit der online-Sicherung sichern lokal sein. Netzwerkspeicherorte oder Wechselmedien wie USB-Laufwerke werden nicht unterstützt.
- Offline Datenmengen: Der Cache-Ordner muss für erwarteten Sicherung mithilfe des Sicherung Azure-Agents online sein.

## <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Gibt es Attribute des Cache-Ordners, die nicht unterstützt werden?<br/>
 Die folgenden Attribute oder deren Kombinationen werden für den Cache-Ordner nicht unterstützt:

- Verschlüsselt
- Heben dupliziert
- Komprimiert
- Gering
- Erneute Analyse Punkt

Es wird empfohlen, dass keine der Cache Ordner oder die Metadaten virtuelle Festplatte die Attribute über für die Sicherung Azure-Agent erwarteten funktionieren.
