<properties
   pageTitle="Hinzufügen eine Web-Anwendung Firewall im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie die Sicherheitscenter Azure Empfehlungen **Hinzufügen eine Web-Anwendung Firewall** und **Finalize Anwendungsschutz**implementieren."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Hinzufügen einer Web-Anwendung Firewalls in Azure-Sicherheitscenter

Azure-Sicherheitscenter möglicherweise empfiehlt sich, dass Sie von einem Microsoft-Partner eine Web-Anwendung Firewall (WAF) hinzufügen, um Ihre Webanwendungen zu sichern. Dieses Dokument führt Sie durch ein Beispiel dazu.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Secure Webanwendung mithilfe der Web-Anwendung-Firewall**.
![Sichern von Web-Anwendung][1]

2. Wählen Sie das Blade **Sichern Ihrer Webanwendungen Web Anwendung Firewall mithilfe** einer Web-Anwendungs. Das **Hinzufügen einer Web-Anwendung Firewall** Blade wird geöffnet.
![Hinzufügen einer Web-Anwendung Firewalls][2]
3. Sie können auch eine vorhandene Firewall auf Web verwenden, falls vorhanden, oder Sie können einen neuen erstellen. In diesem Beispiel sind keine vorhandenen WAFs verfügbar, damit wir eine neue WAF erstellen können.

4. Wählen Sie zum Erstellen einer neuen WAF einer Lösung aus der Liste der integrierten Partner zur Verfügung. In diesem Beispiel werden wir **Barracuda Web Anwendung Firewall**auswählen.
5. Das Blade **Barracuda Web Anwendung Firewall** öffnet Sie Informationen über die Lösung Partner bereitgestellt. Wählen Sie in das Blade Informationen **Erstellen** aus.
![Firewall Informationen blade][3]

6. Das **Neue Web Anwendung Firewall** Blade geöffnet, wo Sie Konfigurationsschritte **Virtuellen Computer** ausführen und **WAF Informationen**bereitstellen können. Wählen Sie **Konfiguration virtueller Computer**.

7. In der **Konfiguration virtueller Computer** Blade-Geben Sie die Angaben zum Einrichten des virtuellen Computers gedreht werden soll, der die WAF ausgeführt werden soll.
![Virtueller Computer-Konfiguration][4]
8. Kehren Sie zu der **Neuen Web Anwendung Firewall** Blade zurück, und wählen Sie **WAF Informationen**. Konfigurieren Sie das Blade **WAF Informationen** der WAF selbst. Schritt 7 ermöglicht das Konfigurieren des virtuellen Computers, auf dem der WAF wird ausgeführt, und Schritt 8 ermöglicht es Ihnen, die WAF selbst bereitstellen.

## <a name="finalize-application-protection"></a>Fertigstellen Schutz der Anwendung

1. Kehren Sie zu der Blade **Empfehlungen** zurück. Ein neuer Eintrag wurde generiert, nachdem Sie die WAF, aufgerufen **Finalize Anwendungsschutz**erstellt. Dieser Eintrag können Sie wissen, dass Sie tatsächlich Kabel von der WAF innerhalb des Azure-virtuellen Netzwerks, damit sie die Anwendung schützen kann die Vorgehensweise zum ausführen müssen.
![Fertigstellen Schutz der Anwendung][5]

2. Wählen Sie **die Anwendungsschutz Finalize**. Ein neuer Blade wird geöffnet. Sie können sehen, dass es ist eine Anwendung, die den Datenverkehr umgeleitet sein muss.
3. Wählen Sie die Webanwendung. Eine Blade wird geöffnet, die Sie Schritte für das Web Anwendung Firewall-Setup abschließen können. Führen Sie die Schritte aus, und wählen Sie dann **Datenverkehr einschränken**. Sicherheitscenter führen Sie dann das Kabel auszurichten für Sie.
![Beschränken Sie den Datenverkehr][6]

> [AZURE.NOTE] Sie können mehrere Webanwendungen in Sicherheitscenter schützen, indem Sie diese Programme Bereitstellung Ihrer vorhandenen WAF hinzufügen. WAF Einheiten (erstellt mit dem Modell zur Bereitstellung von Ressourcenmanager) in einem separaten virtuellen Netzwerk bereitgestellt werden müssen. WAF Einheiten (erstellt mit dem Bereitstellungsmodell klassischen) sind bei der Verwendung einer Netzwerksicherheitsgruppe beschränkt. Diese Unterstützung wird in eine vollständig angepasste Bereitstellung von einer WAF Einheit (klassische) in der Zukunft erweitert werden. Weitere Informationen zu den [klassischen und Ressourcenmanager Bereitstellungsmodelle](../azure-classic-rm.md) Azure Ressourcen.

Die Protokolle aus dieser WAF sind jetzt vollständig integriert. Sicherheitscenter können beginnen, automatisch erfassen und analysieren die Protokolle aus, damit es für Sie wichtigen von Sicherheitshinweisen bereitstellen kann.

## <a name="see-also"></a>Siehe auch

Dieses Dokument wurde gezeigt, wie implementieren Sicherheitscenter empfohlen "Hinzufügen einer Webanwendung". Weitere Informationen zum Konfigurieren der Firewall eine Web-Anwendung, probieren Sie Folgendes ein:

- [Konfigurieren einer Web-Anwendung-Firewall (WAF) für App-Service-Umgebung](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge zu Azure Sicherheit und Kompatibilität.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
