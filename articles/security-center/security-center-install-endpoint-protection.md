<properties
   pageTitle="Installieren von Endpunkt Schutz im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie Azure Sicherheitscenter empfohlen **Installieren Endpunkt Schutz**implementieren."
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
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="install-endpoint-protection-in-azure-security-center"></a>Installieren von Endpunkt Schutz in Azure-Sicherheitscenter

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie ein Programm Modul zu Ihrer Azure-virtuellen Computern (virtuellen Computern) bereitstellen, wenn Modul noch nicht aktiviert ist. Diese Empfehlungen gilt nur für Windows-virtuellen Computern.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Endpunkt Schutz installieren**.
![Wählen Sie die Endpunkt Schutz installieren][1]

2. Das **Endpunkt Schutz installieren** Blade wird geöffnet, und eine Liste der virtuellen Computern ohne Modul aktiviert. Wählen Sie aus der Liste der virtuellen Computern, die Sie installieren Modul auf, und klicken Sie auf **auf virtuellen Computern installieren**möchten.
![Wählen Sie virtuellen Computern Modul auf installieren.][2]

3. Ermöglichen es Ihnen, die Modul-Lösung, die Sie verwenden möchten, wählen das Blade **Endpunkt Schutz auswählen** wird geöffnet. Wählen Sie in diesem Beispiel uns **Microsoft-Modul**.
![Wählen Sie Endpunkt Schutz][3]

4. Weitere Informationen zur Lösung Modul wird angezeigt. Wählen Sie auf **Erstellen**.
![Modul Lösung erstellen][4]

5. Geben Sie die erforderlichen Konfiguration Einstellungen auf das Blade **Erweiterung hinzufügen** , und wählen Sie dann auf **OK**. Weitere Informationen zu den Einstellungen Konfiguration finden Sie unter [Standard- und benutzerdefinierte Modul Konfiguration](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).

[Microsoft-Modul](../azure-security-antimalware.md) ist jetzt aktiv auf die ausgewählten virtuellen Computern.

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie implementiert Sicherheitscenter empfohlen "Installieren Endpunkt Schutz". Weitere Informationen zum Aktivieren von einem Programm Modul in Azure, probieren Sie Folgendes ein:

- [Microsoft-Modul für Cloud-Diensten und virtuellen Computern](../azure-security-antimalware.md) – erfahren Sie, wie Microsoft-Modul bereitstellen.

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge zu Azure Sicherheit und Kompatibilität.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
