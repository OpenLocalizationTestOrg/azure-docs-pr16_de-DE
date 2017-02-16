<properties
   pageTitle="Transparent Data Verschlüsselung in Azure-Sicherheitscenter aktivieren | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie implementieren Azure-Sicherheitscenter empfohlen **Transparent Daten Verschlüsselung aktivieren**."
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

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Transparent Data Verschlüsselung in Azure Sicherheitscenter aktivieren

Azure-Sicherheitscenter wird empfehlen mit transparenten Daten Verschlüsselung (TDE) auf SQL-Datenbanken aktivieren, wenn TDE noch nicht aktiviert ist. TDE sind Ihre Daten geschützt und hilft Ihnen die Einhaltung von Vorschriften zu erfüllen durch die Verschlüsselung der Datenbank, zugeordneten Sicherungskopien und Transaktionsprotokolldateien ruhen – ohne Änderungen an Ihrer Anwendung. Informationen finden Sie unter Weitere [Transparente Verschlüsselung von Daten mit Azure SQL-Datenbank](https://msdn.microsoft.com/library/dn948096).

Diese Empfehlungen gilt für den SQL Azure-Dienst. SQL, die auf Ihre virtuellen Computern nicht eingeschlossen werden.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Transparent Daten Verschlüsselung aktivieren**.
![Aktivieren Sie die Daten als Transparent-Verschlüsselung][1]

2. Dadurch wird das Blade **Transparent Daten-Verschlüsselung aktivieren auf SQL-Datenbanken** geöffnet. Wählen Sie eine SQL-Datenbank TDE auf aktivieren.
![Wählen Sie SQL-DB TDE auf aktivieren.][2]
3. Klicken Sie auf das Blade **Transparent Data Verschlüsselung** wählen Sie unter verschlüsselte Daten **auf** aus, und wählen Sie **Speichern** , im oberen Menüband des Blades.
![TDE aktivieren][3]

  Nachdem der ausgewählten SQL-Datenbank TDE aktiviert ist, ändert sich der **Status der Verschlüsselung** in **verschlüsselte**.    

  ![Status der Verschlüsselung][4]

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie das Sicherheitscenter Empfehlungen implementiert "Transparent Daten-Verschlüsselung aktivieren" aus. Weitere Informationen zum SQL TDE, probieren Sie Folgendes ein:

- [Transparent Data-Verschlüsselung mit SQL Azure-Datenbank](https://msdn.microsoft.com/library/dn948096)
- [Erste Schritte mit transparenten Daten Verschlüsselung (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes an.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
