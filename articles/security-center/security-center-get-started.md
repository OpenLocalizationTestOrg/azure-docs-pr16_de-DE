<properties
   pageTitle="Schnellstarthandbuch Sicherheitscenter Azure | Microsoft Azure"
   description="In diesem Artikel können Sie schnell mit Azure-Sicherheitscenter beginnen, indem er Sie durch die Sicherheit für die Überwachung und Richtlinie Verwaltungskomponenten begleitet und verknüpfen Sie mit nächsten Schritten fort."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Schnellstarthandbuch Azure-Sicherheitscenter

In diesem Artikel können Sie schnell mit Azure-Sicherheitscenter beginnen, indem er Sie durch die Sicherheit für die Überwachung und Richtlinie Verwaltungskomponenten der Sicherheitscenter begleitet.

> [AZURE.NOTE] In diesem Artikel wird den Dienst mithilfe einer Beispiel-bereitstellungs. In diesem Artikel ist keine schrittweise Anleitung.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um mit Sicherheitscenter beginnen, müssen Sie ein Microsoft Azure-Abonnement verfügen. Wenn Sie nicht über ein Abonnement verfügen, können Sie für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)anmelden.

Die kostenlose Ebene der Sicherheitscenter automatisch mit Ihrem Abonnement aktiviert ist, und bietet einen Einblick in den Sicherheitszustand Ihrer Azure Ressourcen. Darüber grundlegende Sicherheit Gruppenrichtlinien-Verwaltungskonsole, Mittelpunkt und Integration mit Sicherheitsprodukten und Diensten aus Azure-Partner zur Verfügung.

Sie auf Sicherheitscenter aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. Wenn Sie weitere Informationen zur Azure-Portal finden Sie unter der [Portalseite Dokumentation](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Datensammlung

Sicherheitscenter sammelt Daten aus Ihrem virtuellen Computern (virtuellen Computern) bewerten ihren Sicherheitsstatus, Vorschläge, wie Sicherheit und informieren Sie Risiken. Wenn Sie erstmals Sicherheitscenter zugreifen, ist die Datensammlung auf alle virtuellen Computern in Ihr Abonnement aktiviert. Datensammlung wird empfohlen, aber durch Deaktivieren der Sammlung von Daten in die Richtlinie Sicherheitscenter kündigen können.

Die folgenden Schritte beschreiben zum Zugreifen auf und Verwenden der Komponenten von Sicherheitscenter. In den folgenden Schritten lernen Sie, wie Sie Datensammlung deaktivieren, wenn Sie festlegen, dass das Abonnement kündigen.

## <a name="access-security-center"></a>Access-Sicherheitscenter

Im Portal folgendermaßen Sie vor, um das Sicherheitscenter zuzugreifen:

1. Wählen Sie im Menü **Microsoft Azure** **Sicherheitscenter**.
![Azure-Menü][1]

2. Wenn Sie auf Sicherheitscenter zum ersten Mal zugreifen, wird das **Willkommen** Blade geöffnet. Wählen Sie Ja aus **! Ich möchte Launch Azure-Sicherheitscenter** öffnen Sie das **Sicherheitscenter** Blade und Datensammlung aktivieren.
![Willkommenseite][10]

3. Nachdem Sie Sicherheitscenter aus dem Willkommen Blade starten, oder wählen Sie im Menü der Microsoft Azure Sicherheitscenter, wird das **Sicherheitscenter** Blade geöffnet. Für den einfachen Zugriff auf das **Sicherheitscenter** Blade in der Zukunft die Option **Pin Blade zum Dashboard** (oben rechts).
![PIN-Blade zu Dashboard-option][2]

## <a name="use-security-center"></a>Verwenden Sie das Sicherheitscenter

Sie können die Sicherheitsrichtlinien für Ihre Azure-Abonnements und Ressourcengruppen konfigurieren. Lassen Sie uns eine Sicherheitsrichtlinie für Ihr Abonnement zu konfigurieren:

1. Wählen Sie die **Richtlinie** -Kachel auf das **Sicherheitscenter** Blade aus.
![Sicherheitsrichtlinie][3]

2. Wählen Sie in der Blade **Sicherheitsrichtlinie - Richtlinie pro Abonnement oder die Ressourcengruppe definieren** ein Abonnement aus.
3. Klicken Sie auf das Blade **Sicherheitsrichtlinie** ist **Datensammlung** zum Erfassen von Protokolle automatisch aktiviert. Die Überwachung Erweiterung wird auf alle virtuellen Computern aktuelle und die neue im Abonnement bereitgestellt. (Sie können Datensammlung ablehnen, durch Festlegen der **Sammlung von Daten** zu **Deaktivieren**, aber Sicherheitscenter Dies verhindert, dass Sie von Sicherheitshinweisen und Empfehlungen zugewiesen.)
4. Wählen Sie in der **Sicherheitsrichtlinie** Blade **Auswählen eines Kontos Speicherplatz pro Region**ein. Für jeden Bereich, in dem Sie virtuelle Computer ausgeführt haben, wählen Sie das Speicherkonto auf diese virtuelle Computer erfassten Daten gespeichert ist. Wenn Sie ein Speicherkonto für die einzelnen Regionen nicht auswählen, wird es für Sie erstellt. Die gesammelten Daten ist logisch von Daten mit anderen Kunden aus Sicherheitsgründen isoliert.

     > [AZURE.NOTE] Es empfiehlt sich, dass Sie Datensammlung aktivieren, und wählen Sie zuerst ein Speicherkonto Ebene der Abonnement aus. Sicherheitsrichtlinien können bei der Azure-Abonnement und Ressourcen Gruppenebene festgelegt werden, aber die Konfiguration von Datensammlung und Speicher-Konto auf der Abonnementebene tritt nur.

5. Wählen Sie auf der Blade **Sicherheitsrichtlinie** **Prevention Richtlinie**ein. Dadurch wird das **Richtlinie Prevention** Blade geöffnet.
![Prevention-Richtlinie][4]

6. Aktivieren Sie das Blade **Prevention Richtlinie** über die empfohlene, die als Teil der Sicherheitsrichtlinie angezeigt werden sollen. Beispiele:

 - **System-Updates** **auf** Einstellung scannt alle unterstützten virtuellen Computer auf fehlende Updates für OS.
 - **OS Schwachstellen** **auf** Einstellung scannt alle unterstützten virtuellen Computern um OS Varianten zu identifizieren, die des virtuellen Computers weniger vor Angriffen geschützt werden kann.

### <a name="view-recommendations"></a>Ansicht Empfehlungen

1. Das **Sicherheitscenter** Blade wieder ein, und wählen Sie die Kachel **Empfehlungen** . Sicherheitscenter analysiert regelmäßig den Sicherheitsstatus Ihrer Azure Ressourcen. Wenn Sicherheitscenter potenzieller Sicherheitslücken bezeichnet, werden Empfehlungen auf das Blade **Empfehlungen** angezeigt.
![Empfehlungen im Sicherheitscenter Azure][5]

2.  Wählen Sie Empfehlungen auf das Blade **Empfehlungen** um weitere Informationen anzuzeigen und/oder auszuführende Aktion aus, um das Problem zu beheben.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Anzeigen des Status Gesundheit und Sicherheit von Ressourcen

1.  Zurück zum das **Sicherheitscenter** Blade. Die Kachel **Ressourcen Sicherheit Gesundheit** enthält Indikatoren für den Sicherheitszustand für virtuelle Computer, Netzwerke, Daten und Applikationen.
2.  Wählen Sie **virtuellen Computern** , um weitere Informationen anzuzeigen. Das Blade **virtuellen Computern** wird geöffnet und zeigt eine Zusammenfassung Status Modul Programme, System-Updates, neu gestartet wurde und OS Schwachstellen Ihrer virtuellen Computer an.
![Die Ressourcen Gesundheit Kachel in Azure-Sicherheitscenter][6]

3.  Wählen Sie eine Empfehlungen unter **Virtuellen Computern EMPFEHLUNGEN** zum Anzeigen weiterer Informationen und/oder agieren so konfigurieren Sie die erforderlichen Steuerelemente aus.
4.  Wählen Sie einen virtuellen Computer unter **virtuellen Computern** , um zusätzliche Details anzuzeigen.

### <a name="view-security-alerts"></a>Anzeigen von Sicherheitshinweisen

1.  Das **Sicherheitscenter** Blade wieder ein, und wählen Sie die Kachel **von Sicherheitshinweisen** . Das **von Sicherheitshinweisen** Blade wird geöffnet und zeigt eine Liste von Benachrichtigungen. Die Analyse Sicherheitscenter Sicherheitsprotokolle und Netzwerkaktivität generiert diese Benachrichtigungen. Benachrichtigungen von integrierten partnerlösungen sind enthalten.
![Von Sicherheitshinweisen in Azure-Sicherheitscenter][7]

    > [AZURE.NOTE] Sicherheitswarnungen sind nur verfügbar, wenn die standardmäßige Ebene der Sicherheitscenter aktiviert ist. Eine kostenlose Testversion von 90 Tage der Standard-Stufe steht. Informationen zum Abrufen der Standardansicht Ebene finden Sie unter [Nächste Schritte](#next-steps) .

2.  Wählen Sie eine Warnung, um weitere Informationen anzuzeigen. In diesem Beispiel markieren wir nun **geändert System binäre erkannt**. Daraufhin wird ein Blades, die weitere Details zur jeweiligen Warnung bereitstellen.
![Sicherheit benachrichtigen Details in Azure-Sicherheitscenter][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Anzeigen des Integritätsstatus Ihrer partnerlösungen

1. Zurück zum das **Sicherheitscenter** Blade. Die Kachel **partnerlösungen** können Sie, auf einen Blick den Status des Ihrer partnerlösungen integriert mit Ihrem Azure-Abonnement zu überwachen.
2. Wählen Sie die Kachel **partnerlösungen** aus. Eine Blade wird geöffnet und zeigt eine Liste von Ihrem partnerlösungen mit Sicherheitscenter verbunden.
![Partnerlösungen][9]

3. Wählen Sie eine Lösung Partner aus. Wählen Sie in diesem Beispiel die Lösung **F5-WAF** uns aus.  Eine Blade wird geöffnet und zeigt Sie den Status der Lösung Partner und die Lösung den zugeordneten Ressourcen. Wählen Sie **Lösung Console** um den Partner Verwaltungsoption für diese Lösung zu öffnen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel, die Sie auf die Sicherheit für die Überwachung und Richtlinie Verwaltungskomponenten der Sicherheitscenter eingeführt. Nachdem Sie nun mit dem Sicherheitscenter vertraut sind, versuchen Sie die folgenden Schritte aus:

- Konfigurieren einer Sicherheitsrichtlinie für Ihr Abonnement Azure. Weitere Informationen finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Sicherheitscenter](security-center-policies.md).
- Mithilfe der Empfehlungen im Sicherheitscenter können Sie Ihre Azure Ressourcen zu schützen. Weitere Informationen finden Sie unter [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md).
- Überprüfen Sie und verwalten Sie Ihrer aktuellen von Sicherheitshinweisen. Weitere Informationen finden Sie unter [Verwaltung und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md).
- Weitere Informationen über die [erweiterten Features zur Erkennung von Bedrohung](security-center-detection-capabilities.md) , die im Zusammenhang mit der [standardmäßigen Ebene](security-center-pricing.md) der Sicherheitscenter. Eine kostenlose Testversion von 90 Tage der Standard-Stufe steht.
- Wenn Sie Fragen zur Nutzung von Sicherheitscenter haben, finden Sie unter den [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
