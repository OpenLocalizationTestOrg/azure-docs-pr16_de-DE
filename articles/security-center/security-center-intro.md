<properties
   pageTitle="Einführung in das Sicherheitscenter Azure | Microsoft Azure"
   description="Informationen Sie zu Azure Sicherheitscenter, deren wichtigsten Funktionen und deren Funktionsweise."
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
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Einführung in Azure-Sicherheitscenter

Informationen Sie zu Azure Sicherheitscenter, deren wichtigsten Funktionen und deren Funktionsweise.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.

## <a name="what-is-azure-security-center"></a>Was ist das Sicherheitscenter Azure?
 Sicherheitscenter können Sie verhindern, dass erkennen und Beantworten von Risiken mit größerer Einblick in und Kontrolle über die Sicherheit Ihrer Azure Ressourcen. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Azure-Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

##  <a name="key-capabilities"></a>Key-Funktionen
 Sicherheitscenter bietet einfach zu verwendendes und effektiven Bedrohung Prevention, Erkennung und Antwort-Funktionen, die integriert sind in Azure. Key-Funktionen sind:

| | |
|----- |-----|
| Verhindern, dass | Überwacht den Sicherheitsstatus Ihrer Azure-Ressourcen |
| | Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen basierend auf Sicherheit Anforderungen Ihres Unternehmens, definiert die Arten von Applications, die Sie verwenden, und die Vertraulichkeit Ihrer Daten |
| | Richtlinie leistungsgesteuert verwendet Sicherheit Empfehlungen zum Dienstbesitzer durch das Verfahren implementieren Leitfaden erforderlichen Steuerelemente |
| | Schnell bereitstellt Sicherheitsservices und Einheiten von Microsoft und Partnern |
| Erkennen |Automatische Erfassung und Analyse der von Sicherheitsdaten aus Azure Ressourcen, die Netzwerk- und partnerlösungen wie Modul Programme und firewalls |
| | Globale Nutzung Verfahren zum Erstellen von Intelligence von Microsoft-Produkten und Diensten, Microsoft digitale Crimes Einheit (Datencacheeinheit), Microsoft Security Antwort Center (MSRC) und externe feeds |
| | Gültig für erweiterte Analytics, einschließlich Schulung maschinellen und Verhalten Analyse |
| Antworten | Priorität von Ereignissen/Sicherheitshinweisen enthält |
| | Bietet Einblicke in der Quelle der Angriffen und den betroffenen Ressourcen |
| | Enthält Vorschläge für Verfahren zum Beenden der aktuellen Angriffen und verhindern, dass zukünftige Angriffen |

## <a name="introductory-walkthrough"></a>Einführung Exemplarische Vorgehensweise
 Sie auf Sicherheitscenter aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. [Melden Sie sich mit dem Portal](https://portal.azure.com), wählen Sie **Durchsuchen**, und dann einen Bildlauf zur Option **Sicherheitscenter** oder wählen Sie die Kachel **Sicherheitscenter** , die Sie zuvor auf dem Portal Dashboard angeheftet aus.

![Sicherheit Kachel Azure-Portal][1]

Sicherheitscenter können Sie festlegen von Sicherheitsrichtlinien, Sicherheitskonfigurationen überwachen und Anzeigen von Sicherheitshinweisen.

### <a name="security-policies"></a>Sicherheitsrichtlinien

Sie können die Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen gemäß Ihres Unternehmens Sicherheit Anforderungen definieren. Sie können auch irrelevante für die Typen von Applications, die Sie verwenden, oder Sie können die Empfindlichkeit der Daten in jedem Abonnement. Beispielsweise möglicherweise für die Entwicklung oder die Prüfung verwendete Ressourcen andere Sicherheitsstandards gelten als die für die Herstellung Applications. Ebenso erfordern Applikationen mit geregelten Daten wie PII möglicherweise eine höhere Sicherheit.

> [AZURE.NOTE] Wenn eine Sicherheitsrichtlinie auf der Abonnementebene oder die Ressource Gruppe ändern möchten, müssen Sie der Besitzer des Abonnements oder einen Mitwirkenden zu sein.

Wählen Sie auf das **Sicherheitscenter** Blade die Kachel **Richtlinie** für eine Liste Ihrer Abonnements und Ressourcengruppen ein.   

![Sicherheitscenter blade][2]

Wählen Sie auf der Blade **Sicherheitsrichtlinie** ein Abonnement für die Richtliniendetails anzeigen aus.

![Sicherheit Richtlinie Blade-Abonnements][3]

**Datensammlung** (siehe oben) ermöglicht die Datensammlung für eine Sicherheitsrichtlinie. Aktivieren der stehen:

- Tägliche Überprüfung aller unterstützt virtuelle Computer für die Sicherheit überwachen und Empfehlungen.
- Auflistung von Sicherheitsereignissen, für die Analyse und Bedrohung Erkennung.

**Auswählen eines Kontos Speicherplatz pro region** (siehe oben) ermöglicht es Ihnen, die für jeden Bereich in der virtuellen Computern ausgeführt werden, stehen das Speicherkonto auszuwählen, aus diesen virtuellen Computern gesammelte Daten gespeichert ist. Wenn Sie ein Speicherkonto für die einzelnen Regionen nicht auswählen, wird es für Sie erstellt. Die gesammelten Daten ist logisch von Daten mit anderen Kunden aus Sicherheitsgründen isoliert.

> [AZURE.NOTE] Datensammlung und Auswählen von einem Konto Speicherplatz pro Region ist Ebene Abonnement konfiguriert.

Wählen Sie **Prevention Richtlinie** (siehe oben) um das Blade **Prevention Richtlinie** zu öffnen. **Empfehlungen für anzeigen** , können Sie auswählen, dass die Sicherheit-Steuerelemente, die Sie verwenden möchten, überwachen und empfiehlt sich auf den Anforderungen Sicherheit den Ressourcen innerhalb des Abonnements basierend.

Wählen Sie dann eine Ressourcengruppe Richtliniendetails anzeigen.

![Sicherheit Richtlinie Blade Ressourcengruppe][4]

**Vererbung** (siehe oben) können Sie die Ressourcengruppe als definieren:

- Geerbt (Standard), alle Sicherheitsrichtlinien für diese Ressourcengruppe d. h., werden von der Abonnementebene geerbt.
- Eindeutige was bedeutet, dass die Ressourcengruppe eine benutzerdefinierte Sicherheitsrichtlinie haben. Sie benötigen, klicken Sie unter **Anzeigen von Empfehlungen für**Änderungen vornehmen.

> [AZURE.NOTE] Ist ein Konflikt zwischen Abonnement Ebene und Ressourcen Gruppe Ebene Richtlinie, hat die Ressource abgleichen Gruppenrichtlinien Vorrang vor.

### <a name="security-recommendations"></a>Sicherheit Empfehlungen

 Sicherheitscenter analysiert den Sicherheitszustand Azure Ressourcen potenzieller Sicherheitslücken identifizieren müssen. Eine Liste der Empfehlungen führt Sie durch das Verfahren zum Konfigurieren von Steuerelementen erforderlich. Beispiele für:

- Bereitstellung von Modul besser erkennen und Entfernen von Schadsoftware
- Konfigurieren von Sicherheitsgruppen Netzwerk und Regeln für die Kontrolle des Datenverkehrs auf virtuellen Computern
- Bereitstellung von Web-Anwendung Firewalls helfen Angriffen dieser Ziel eingesetzt Ihrer Webanwendungen
- Bereitstellen von fehlenden Systemupdates
- Adressieren OS Konfigurationen, die nicht die empfohlenen Basislinien entsprechen

Klicken Sie auf die Kachel **Empfehlungen** für eine Liste der Empfehlungen. Klicken Sie auf jedes Empfehlungen, um zusätzliche Informationen anzuzeigen oder Schritte durchführen, um das Problem zu beheben.

![Sicherheit Empfehlungen im Sicherheitscenter Azure][5]

### <a name="resource-health"></a>Ressourcen-Dienststatus

Die Kachel **Ressource Sicherheit Gesundheit** zeigt den Sicherheitsstatus der Umgebung nach der Ressourcenart, einschließlich virtuellen Computern, Webanwendungen und anderen Ressourcen.   

Wählen Sie einen Ressourcentyp auf die Kachel **Ressource Sicherheit Gesundheit** zum Anzeigen weiterer Informationen, einschließlich einer Liste der potenziellen Sicherheitslücken, die identifiziert wurden. (**Virtuelle Computer** ist im folgenden Beispiel aktiviert.)

![Ressourcen-Dienststatus-Kachel][6]

### <a name="security-alerts"></a>Von Sicherheitshinweisen

 Sicherheitscenter automatisch erfasst, analysiert und Integration von Log-Daten aus Azure Ressourcen, die Netzwerk- und partnerlösungen wie Modul Programme und Firewalls. Wenn Viren gefunden werden, wird eine Warnung erstellt. Beispiele für Erkennung von:

- Betroffenen virtuellen Computern zum Kommunizieren mit bekannten bösartiger IP-Adressen
- Erweiterte Schadsoftware mithilfe der Windows-Fehlerberichterstattung erkannt
- Bruteforce-Angriffen auf virtuellen Computern
- Von Sicherheitshinweisen integrierte Modul Programme und firewalls

Durch Klicken auf die Kachel **von Sicherheitshinweisen** zeigt eine Liste der Priorität Benachrichtigungen.

![Von Sicherheitshinweisen][7]

Auswählen einer Benachrichtigung zeigt Weitere Informationen zu den Angriffen und Vorschläge für die Vorgehensweise, um ihn zu beheben.

![Sicherheit benachrichtigen details][8]

### <a name="partner-solutions"></a>Partnerlösungen

Die Kachel **partnerlösungen** kann Monitor auf einen Blick der Status des Ihrer partnerlösungen mit Ihrem Abonnement Azure integriert. Sicherheitscenter zeigt Benachrichtigungen aus den Lösungen stammen.

Wählen Sie die Kachel **partnerlösungen** aus. Eine Blade wird geöffnet, und eine Liste aller verbundenen partnerlösungen.

![Partnerlösungen][9]

## <a name="get-started"></a>Erste Schritte
Um mit dem Sicherheitscenter anzufangen, benötigen Sie ein Abonnement von Microsoft Azure. Sicherheitscenter, die mit Ihrem Azure-Abonnement aktiviert ist. Wenn Sie nicht über ein Abonnement verfügen, können Sie für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.

 Sie auf Sicherheitscenter aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. Finden Sie weitere im [Portal Dokumentation](https://azure.microsoft.com/documentation/services/azure-portal/) .

[Erste Schritte mit Azure Sicherheitscenter](security-center-get-started.md) begleitet schnell Sie über die Sicherheit Überwachung und Richtlinie-Verwaltungskomponenten der Sicherheitscenter.

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden Sicherheitscenter, deren wichtigsten Funktionen und Schritte vorgestellt. Weitere Informationen finden Sie hier:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten der Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachen von partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
