<properties
   pageTitle="Datensammlung in Azure-Sicherheitscenter aktivieren | Microsoft Azure"
   description=" Erfahren Sie, wie Datensammlung in Azure-Sicherheitscenter zu aktivieren. "
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

# <a name="enable-data-collection-in-azure-security-center"></a>Datensammlung in Azure-Sicherheitscenter aktivieren

Um Hilfe Kunden zu verhindern, erkennen und Beantworten von Risiken, Azure-Sicherheitscenter sammelt und verarbeitet Daten zu Ihrer Azure-virtuellen Computern, einschließlich Informationen zur Konfiguration, Metadaten, Ereignisprotokollen und vieles mehr. Wenn Sie erstmals Sicherheitscenter zugreifen, ist die Datensammlung auf allen virtuellen Computern in Ihr Abonnement aktiviert. Datensammlung wird empfohlen, aber Sie können melden Sie sich durch Datensammlung in die Richtlinie Sicherheitscenter deaktivieren (siehe [Datensammlung deaktivieren](#disabling-data-collection)). Wenn Sie Datensammlung deaktivieren, wird das Sicherheitscenter empfiehlt sich, Datensammlung in der Sicherheitsrichtlinie für die Abonnement zu aktivieren.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt. Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie die Kachel **Empfehlungen** auf das **Sicherheitscenter** Blade aus.  Dadurch wird das Blade **Empfehlungen** geöffnet.
![Sicherheitscenter blade][1]

2. Wählen Sie in der **Empfehlungen** Blade **Datensammlung für Abonnements aktivieren**aus.  Dadurch wird das **Aktivieren von Datensammlung** Blade geöffnet.
![Empfehlungen blade][2]

3. Wählen Sie Ihr Abonnement, auf das Blade **Datensammlung aktivieren** . Das Blade **Sicherheitsrichtlinie** für diesen Abonnement wird geöffnet.

4. Wählen Sie auf der Blade **Sicherheitsrichtlinie** **auf** unter **Datensammlung** Protokolle automatisch sammeln. Datensammlung einschalten werden auch die Überwachung Erweiterung auf alle alten und neuen unterstützten virtuellen Computern im Abonnement bereitstellen.
![Die Richtlinie Blade Sicherheit][3]

5.  Wählen Sie **Speichern**aus.

6.  **Auswählen eines Kontos Speicherplatz pro Region**auswählen Für die einzelnen Regionen, in denen Sie virtuellen Computern ausgeführt haben, wählen Sie das Speicherkonto auf diese virtuellen Computer erfassten Daten gespeichert ist. Wenn Sie ein Speicherkonto für die einzelnen Regionen nicht auswählen, wird es automatisch für Sie erstellt. In diesem Beispiel werden wir **Newstoracct**auswählen. Sie können das Speicherkonto später ändern, indem zurückgeben, um die Sicherheitsrichtlinie für Ihr Abonnement und einen anderen Speicherplatz Konto auswählen.
![Wählen Sie ein Speicherkonto][4]

7.  Wählen Sie **OK**aus.

> [AZURE.NOTE] Es empfiehlt sich, dass Sie Datensammlung aktivieren, und wählen Sie zuerst ein Speicherkonto Ebene der Abonnement aus. Sicherheitsrichtlinien können bei der Azure-Abonnement und Ressourcen Gruppenebene festgelegt werden, aber Konfiguration von Daten Sammlung und Speicher Konto nur Ebene Abonnement auftritt.

## <a name="after-data-collection-is-enabled"></a>Nachdem Datensammlung aktiviert ist

Datensammlung wird über die Überwachung Azure-Agent und die Erweiterung Azure Sicherheit Überwachung aktiviert. Die Erweiterung Azure Sicherheit Überwachung für verschiedene Sicherheit relevanten Konfiguration scannt und sendet es in [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) auf. Darüber hinaus wird das Betriebssystem, das Einträge im Ereignisprotokoll erstellt. Die Überwachung Azure-Agent liest Einträge im Ereignisprotokoll und ETW verfolgt und kopiert sie in Ihr Speicherkonto für die Analyse. Die Überwachung Agent kopiert Absturzabbilddateien auch bei Ihrem Speicherkonto. Dies ist das Speicherkonto, das die in der Sicherheitsrichtlinie konfiguriert wurde.

## <a name="disabling-data-collection"></a>Deaktivieren von Datensammlung

Sie können jederzeit Datensammlung deaktivieren, die alle Überwachung vorherigen von installierten Agents Sicherheitscenter entfernt werden.  Wählen Sie ein Abonnement für die Sammlung von Daten zu deaktivieren.

> [AZURE.NOTE] Sicherheitsrichtlinien können bei der Azure-Abonnement und Ressourcen Gruppenebene festgelegt werden, aber Sie müssen ein Abonnement Datensammlung deaktivieren aktivieren.

1.  Das **Sicherheitscenter** Blade wieder ein, und wählen Sie die **Richtlinie** Kachel. Dadurch wird das **Sicherheit Richtlinie-definieren Richtlinie pro Abonnement oder Ressourcengruppe** Blade geöffnet.
![Wählen Sie die Richtlinie-Kachel][5]

2.  Wählen Sie das Abonnement, das Sie Datensammlung deaktivieren möchten, klicken Sie auf das Blade **Sicherheit Richtlinie-definieren Richtlinie pro Abonnement oder Ressourcengruppe** .
![Select-Abonnement Datensammlung deaktivieren][6]

3.  Das Blade **Sicherheitsrichtlinie** für diesen Abonnement wird geöffnet.  Wählen Sie unter Datensammlung **Deaktivieren** aus.

4.  Wählen Sie im Menüband oben **Speichern** .

5.  Wählen Sie im oberen Menüband So entfernen Sie Agents aus vorhandenen virtuellen Computern **Agents löschen** .

## <a name="see-also"></a>Siehe auch

In diesem Artikel wurde gezeigt, wie das Sicherheitscenter Empfehlungen implementieren "Aktivieren Datensammlung". Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwaltung von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md)--erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md)– Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) --erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md)– suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)– neueste Sicherheit von Azure Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/security-center-blade.png
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
