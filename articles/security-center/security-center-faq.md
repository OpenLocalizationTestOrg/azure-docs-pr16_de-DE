<properties
   pageTitle="Häufig gestellte Fragen (FAQ) Azure-Sicherheitscenter | Microsoft Azure"
   description="Hier finden Sie Antworten auf Fragen zur Azure-Sicherheitscenter."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure-Sicherheitscenter häufig gestellte Fragen (FAQ)

Hier finden Sie Antworten auf Fragen zur Azure-Sicherheitscenter, ein Dienst, der hilft Ihnen zu verhindern, erkennen und Beantworten von Risiken mit größerer Einblick in und Kontrolle über die Sicherheit Ihrer Microsoft Azure-Ressourcen.

## <a name="general-questions"></a>Allgemeine Fragen

### <a name="what-is-azure-security-center"></a>Was ist das Sicherheitscenter Azure?
Azure-Sicherheitscenter hilft Ihnen der verhindern, erkennen und Beantworten von Risiken mit größerer Einblick in und Kontrolle über die Sicherheit Ihrer Azure Ressourcen. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

### <a name="how-do-i-get-azure-security-center"></a>Wie erhalte ich Azure-Sicherheitscenter?
Azure-Sicherheitscenter mit Ihrem Microsoft Azure-Abonnement aktiviert und aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. ([Melden Sie sich mit dem Portal](https://portal.azure.com), wählen Sie **Durchsuchen**, und führen Sie einen Bildlauf zum **Sicherheitscenter**).  

## <a name="billing"></a>Abrechnung

### <a name="how-does-billing-work-for-azure-security-center"></a>Wie funktioniert der Abrechnung für das Sicherheitscenter Azure?
Sicherheitscenter wird in zwei Stufen angeboten: frei / Standard.

Die kostenlose Ebene können Sie zum Festlegen von Sicherheitsrichtlinien und Empfangen von Sicherheitshinweisen, Fälle und Vorschläge, die Sie schrittweise durch die Verfahren zum Konfigurieren von Steuerelementen erforderlich. Mit der kostenlosen Ebene können Sie auch den Sicherheitsstatus Ihrer Azure Ressourcen und mit Ihrem Abonnement Azure integriert partnerlösungen überwachen.

Die Standard-Ebene bietet die kostenlosen Ebene Pluszeichen erweiterte Features entdeckt wurden: Verfahren zum Erstellen von Intelligence, Verhalten Analyse, Absturzanalyse und Normalbetriebswerte. Kostenlose Testversion 90 Tage der Standard-Stufe steht. Upgrade, wählen Sie in der [Sicherheitsrichtlinie](security-center-policies.md#setting-security-policies-for-subscriptions)Preise in aus. Weitere Informationen finden Sie in der [Preise Sicherheitscenter](security-center-pricing.md) .

## <a name="data-collection"></a>Datensammlung

Sicherheitscenter sammelt Daten aus Ihrem virtuellen Computern und zu bewerten, deren Sicherheitsstatus, Vorschläge, wie Sicherheit, informieren Sie über Risiken. Wenn Sie erstmals Sicherheitscenter zugreifen, ist die Datensammlung auf allen virtuellen Computern in Ihr Abonnement aktiviert. Datensammlung wird empfohlen, aber Sie können melden Sie sich durch die in der Richtlinie Sicherheitscenter [Datensammlung deaktivieren](#how-do-i-disable-data-collection) .

### <a name="how-do-i-disable-data-collection"></a>Wie deaktiviere ich Datensammlung?

Sie können die **Sammlung von Daten** für ein Abonnement in der Sicherheitsrichtlinie zu einem beliebigen Zeitpunkt deaktivieren. ([Azure-Portal anmelden](https://portal.azure.com), wählen Sie **Durchsuchen**, wählen Sie **Das Sicherheitscenter**, und wählen **Richtlinie**.)  Wenn Sie ein Abonnement auswählen, ein neuer Blade geöffnet, und bietet Ihnen die Möglichkeit, eine **Sammlung von Daten** zu deaktivieren. Wählen Sie die Option **Agents löschen** , in der oberen Multifunktionsleiste Agents aus vorhandenen virtuellen Computern entfernen.

> [AZURE.NOTE] Sicherheitsrichtlinien können bei der Azure-Abonnement und Ressourcen Gruppenebene festgelegt werden, aber Sie müssen ein Abonnement Datensammlung deaktivieren aktivieren.

### <a name="how-do-i-enable-data-collection"></a>Wie aktiviere ich Datensammlung?
Sie können für Ihre Azure Abonnements in der Sicherheitsrichtlinie Datensammlung aktivieren. Klicken Sie zum Aktivieren der Sammlung von Daten, [Azure-Portal anmelden](https://portal.azure.com), wählen Sie **Durchsuchen**, wählen Sie **Das Sicherheitscenter**, und wählen Sie **Richtlinie**. **Datensammlung** auf **auf** festgelegt, und konfigurieren Sie die Stelle, an der gewünschten Daten, die zu erfassenden Speicherkonten (finden Sie unter Frage "[ist meine Daten gespeichert?](#where-is-my-data-stored)"). Wenn **Datensammlung** aktiviert ist, werden automatisch Sicherheit Konfiguration und Ereignis Informationen aus allen unterstützten virtuellen Computern im Abonnement erfasst.

> [AZURE.NOTE] Sicherheitsrichtlinien können bei der Azure-Abonnement und Ressourcen Gruppenebene festgelegt werden, aber Konfiguration von Datensammlung nur auf Abonnementebene auftritt.

### <a name="what-happens-when-data-collection-is-enabled"></a>Was geschieht beim Datensammlung aktiviert ist?
Datensammlung wird über die Überwachung Azure-Agent und die Erweiterung Azure Sicherheit Überwachung aktiviert. Die Erweiterung Azure Sicherheit Überwachung für verschiedene Sicherheit relevanten Konfiguration scannt und sendet es in [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) auf. Darüber hinaus wird das Betriebssystem, das Einträge im Ereignisprotokoll erstellt.  Die Überwachung Azure-Agent liest Einträge im Ereignisprotokoll und ETW verfolgt und kopiert sie in Ihr Speicherkonto für die Analyse.  Dies ist das Speicherkonto, das die in der Sicherheitsrichtlinie konfiguriert wurde. Weitere Informationen über das Speicherkonto finden Sie unter Frage "[ist meine Daten gespeichert?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Wirkt sich die Erweiterung Überwachung Agent oder Sicherheit für die Überwachung die Leistung von meinem Server(s)?
Der Agent und die Erweiterung einer nominal viele Systemressourcen verbraucht und sollte wenig Einfluss auf die Leistung haben.

### <a name="where-is-my-data-stored"></a>Wo sind meine Daten gespeichert?
Für die einzelnen Regionen, in denen Sie virtuellen Computern ausgeführt haben, wählen Sie das Speicherkonto auf diese virtuellen Computer erfassten Daten gespeichert ist. Dies erleichtert das für Sie Daten in der gleichen geografischen Bereich für Datenschutz und Daten Hoheit Zwecke beibehalten werden soll. Wählen Sie das Konto Speicherplatz für ein Abonnement in der Sicherheitsrichtlinie. ([Azure-Portal anmelden](https://portal.azure.com), wählen Sie **Durchsuchen**, wählen Sie **Das Sicherheitscenter**, und wählen **Richtlinie**.) Beim Klicken auf ein Abonnement, wird kein neuer Blade geöffnet. Wählen Sie **Speicher-Konten auswählen** , um einen Bereich auszuwählen.

> [AZURE.NOTE] Sicherheitsrichtlinien können bei der Azure-Abonnement und Ressourcen Gruppenebene festgelegt werden, aber gleichzeitige Auswahl eines Bereichs für Ihr Speicherkonto erfolgt am das Abonnement-Level.

Wenn Sie weitere Informationen zur Azure-Speicher und Speicherkonten finden Sie unter [Speicher Dokumentation](https://azure.microsoft.com/documentation/services/storage/) und [zur Azure-Speicher-Konten](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Verwenden von Azure-Sicherheitscenter

### <a name="what-is-a-security-policy"></a>Was ist eine Sicherheitsrichtlinie?
Eine Sicherheitsrichtlinie definiert die Steuerelemente, die für Ressourcen innerhalb des angegebenen Abonnement oder Ressourcengruppe vorgeschlagen werden. Azure-Sicherheitscenter definieren Sie Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen gemäß Ihres Unternehmens Sicherheit Anforderungen und Anwendungstyp oder Vertraulichkeit der Daten in jedes Abonnement.

Beispielsweise möglicherweise für die Entwicklung oder Test verwendete Ressourcen andere Sicherheitsstandards gelten als die für die Herstellung Applications. Ebenso erfordern möglicherweise Applikationen mit geregelten Daten wie personenbezogene Informationen (Personally Identifiable Information) eine höhere Sicherheit. Die Sicherheitsrichtlinien im Sicherheitscenter Azure aktiviert werden Mittelpunkt und Überwachung versorgen. Um weitere Informationen zur Sicherheitsrichtlinien finden Sie unter [Sicherheit Gesundheit Azure Sicherheitscenter zu überwachen](security-center-monitoring.md).

> [AZURE.NOTE] Bei einen Konflikt zwischen Abonnement Ebene und Ressourcen Gruppe Ebene Richtlinie hat die Ressource abgleichen Gruppenrichtlinien Vorrang vor.

### <a name="who-can-modify-a-security-policy"></a>Wer kann eine Sicherheitsrichtlinie ändern?
Sicherheitsrichtlinien werden für jedes Abonnement oder Ressourcengruppe konfiguriert. Wenn eine Sicherheitsrichtlinie auf die Gruppe Abonnementebene oder Ressourcen ändern möchten, müssen Sie einen Besitzer oder Mitwirkender dieser Abonnementtyp sein.

So konfigurieren Sie eine Sicherheitsrichtlinie finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Sicherheitscenter](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Was ist eine Empfehlungen Sicherheit?
Azure-Sicherheitscenter analysiert den Sicherheitszustand Azure Ressourcen. Wenn ein potenzieller Sicherheitslücken identifiziert werden, werden Empfehlungen erstellt. Die Empfehlungen führen Sie durch die Vorgehensweise zum Konfigurieren des Steuerelements erforderlich. Beispiele für sind:

- Bereitstellung von Modul besser erkennen und Entfernen von Schadsoftware
- Konfigurieren von [Sicherheitsgruppen Netzwerk](../virtual-network/virtual-networks-nsg.md) und Regeln für die Kontrolle des Datenverkehrs auf virtuellen Computern
- Bereitstellung von Web Anwendung Firewall zum Schutz vor Angriffen, die auf Ihre Webanwendungen-Hilfe
- Bereitstellen von fehlenden Systemupdates
- Adressieren OS Konfigurationen, die nicht die empfohlenen Basislinien entsprechen

Hier werden nur die Empfehlungen, die in den Richtlinien für Sicherheit aktiviert werden angezeigt.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Wie kann ich den aktuellen Sicherheitsstatus meiner Azure Ressourcen sehen?
Eine Kachel **Ressourcen Dienststatus** auf das **Sicherheitscenter** Blade zeigt den Sicherheitsstatus Ihrer Umgebung aufgeschlüsselt nach virtuellen Computern, Webanwendungen und anderen Ressourcen. Jeder Ressource hat eine Indikator mit an, wenn alle potenziellen Sicherheitslücken eingestuft wurden. Durch Klicken auf die Kachel der Ressourcen Gesundheit zeigt Ihre Ressourcen und identifiziert, bei denen Aufmerksamkeit erforderlich oder möglicherweise Probleme bestehen.

### <a name="what-triggers-a-security-alert"></a>Was eine Warnung auslöst?
Azure-Sicherheitscenter automatisch erfasst, analysiert und dem Fixieren Log-Daten aus Azure Ressourcen, die Netzwerk- und partnerlösungen wie Modul und Firewalls. Wenn Viren gefunden werden, wird eine Warnung erstellt. Beispiele für Erkennung von:

- Betroffenen virtuellen Computern zum Kommunizieren mit bekannten bösartiger IP-Adressen
- Erweiterte Schadsoftware erkannt mithilfe der Windows-Fehlerberichterstattung
- Bruteforce-Angriffen auf virtuellen Computern
- Von Sicherheitshinweisen aus integrierte Sicherheit partnerlösungen wie Anti-Malware oder Firewalls der Web-Anwendung

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Was ist der Unterschied zwischen Risiken erkannt und auf vom Microsoft Security Response Center im Vergleich zu Azure-Sicherheitscenter benachrichtigt werden?
Microsoft Security Antwort Center (MSRC) führt select Sicherheit Überwachung der Azure Netzwerk- und Infrastruktur und empfängt Threat Intelligence und Missbrauch Beschwerden von Drittanbietern. Wenn MSRC bewusst, dass Kundendaten durch einen dritten ungesetzlichen oder nicht autorisierte zugegriffen wurde oder der vom Kunden mit Azure nicht bei den Konditionen für die zulässigen verwenden, entspricht ein Vorfall-Manager benachrichtigt den Kunden wird. Benachrichtigung wird in der Regel durch Senden einer e-Mails an die Sicherheit Kontakt(e) angegebenen Azure-Sicherheitscenter oder den Abonnementbesitzer Azure-ein Kontakts Sicherheit nicht angegeben ist.

Sicherheitscenter handelt es sich um eine Azure-Dienst, der kontinuierlich überwacht Azure-Umgebung des Kunden und gilt für Analytics, um eine Vielzahl von Aktivitäten schreibgeschützter automatisch erkennen. Diese Erkennung dargestellt werden als von Sicherheitshinweisen im Sicherheitscenter Dashboard.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Wie werden Berechtigungen im Sicherheitscenter Azure behandelt?
Azure-Sicherheitscenter unterstützt rollenbasierte Zugriff. Weitere Informationen zu Access rollenbasierte Steuerelement (RBAC) in Azure finden Sie unter [Azure Active Directory rollenbasierte Access Control](../active-directory/role-based-access-control-configure.md).

Wenn ein Benutzer öffnet das Sicherheitscenter, nur Empfehlungen und für Benachrichtigungen, die mit Ressourcen zusammenhängen, die, denen der Benutzer auf zugreifen kann, sichtbar. Dies bedeutet, dass Benutzer nur Elemente im Zusammenhang mit der Stelle, an der der Benutzer zugeordnet ist die Rolle des Besitzer, Mitwirkender oder Reader zum Abonnement oder Ressourcengruppe, zu der eine Ressource gehört Ressourcen angezeigt werden.

Bei Bedarf:

- **Bearbeiten eine Sicherheitsrichtlinie**, müssen Sie einen Besitzer oder Mitwirkender des Abonnements sein.
- **Anwenden eines Empfehlungen**, müssen Sie einen Besitzer oder Mitwirkender des Abonnements sein.
- **Einblick in den Sicherheitszustand über alle Abonnements haben**, müssen Sie ein Besitzer, Mitwirkender oder Reader (IT-Administrator, Security Team) für jedes Abonnement sein.
- **Einblick in den Sicherheitszustand Ihrer Ressourcen haben**, müssen Sie eine Ressourcengruppe Besitzer Mitwirkender oder Reader (DevOps) sein.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welche Azure Ressourcen von Azure-Sicherheitscenter überwacht werden?
Azure-Sicherheitscenter überwacht die Azure folgenden Ressourcen:

- Virtuellen Computern (virtuellen Computern) (einschließlich [Cloud Services](../cloud-services/cloud-services-choose-me.md))
- Azure virtuelle Netzwerke
- Azure SQL-Dienst
- Mit Ihrem Abonnement Azure, z. B. ein Web-Anwendung Firewall auf virtuellen Computern und auf [App-Service-Umgebung](../app-service/app-service-app-service-environments-readme.md) integriert partnerlösungen

## <a name="virtual-machines"></a>Virtuellen Computern

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Welche Arten von virtuellen Computern werden unterstützt?
Überwachung der Sicherheit Gesundheit und Empfehlungen stehen für virtuellen Computern (virtuelle Computer) mit der [klassischen und Ressourcenmanager Bereitstellungsmodelle](../azure-classic-rm.md)erstellt.

Unterstützte Windows-virtuellen Computern:

- Windows Server 2008 R2
- WindowsServer 2012
- Windows Server 2012 R2

Unterstützte Linux virtuellen Computern:

- Ubuntu Versionen 12.04, 14.04, 16.04
- Debian Versionen 7, 8
- CentOS Versionen 6. \*, 7.*
- Red Hat Enterprise Linux (RHEL) Versionen 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) Versionen 11. \*, 12.*
- Oracle Linux Versionen 6. \*, 7.*

Virtuelle Computer in einen Cloud-Dienst ausgeführt werden ebenfalls unterstützt. Nur Cloud services Web und Worker Rollen in Herstellung, Steckplätze überwacht werden, ausgeführt. Weitere Informationen zum Cloud-Dienst finden Sie unter [Übersicht über die Cloud Services](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Warum erkennt Azure-Sicherheitscenter nicht die Modul-Lösung, die auf meinem Azure-virtuellen Computer ausgeführt?

Azure-Sicherheitscenter hat nur Einblick in Modul über Azure Extensions installiert. Beispielsweise kein Sicherheitscenter Modul erkennen, die auf ein Bild, die Sie zur Verfügung gestellt oder wenn Sie Modul auf Ihre virtuellen Computern mithilfe Ihrer eigenen Prozesse (z. B. Konfiguration Management Systeme) installiert vorinstalliert wurde.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Warum erhalte die Meldung "Scannen Daten fehlen" ich für meine virtueller Computer?

Es kann einige Zeit dauern (in der Regel weniger als eine Stunde) für Scan Daten gefüllt wird, nachdem Datensammlung in Azure-Sicherheitscenter aktiviert ist. Scannt werden nicht für virtuelle Computer beendet zu füllen.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Warum erhalte ich, die Nachricht "Virtuellen Computer-Agent wird nicht angezeigt?"

Des virtuellen Computer-Agents muss auf virtuellen Computern installiert sein, um die Sammlung von Daten zu aktivieren. Der virtuellen Computer Agent ist standardmäßig für virtuelle Computer installiert, die aus dem Azure Marketplace bereitgestellt werden. Informationen zum Installieren des virtuellen Computer-Agents auf anderen virtuellen Computern, finden im Blogbeitrag [virtueller Computer-Agents und Erweiterungen](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
