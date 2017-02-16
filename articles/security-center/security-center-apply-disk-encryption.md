<properties
   pageTitle="Anwenden der Verschlüsselung in Azure-Sicherheitscenter Festplatten | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie Azure-Sicherheitscenter empfohlen **Übernehmen Datenträger Verschlüsselung**implementieren."
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

# <a name="apply-disk-encryption-in-azure-security-center"></a>Anwenden der Verschlüsselung in Azure-Sicherheitscenter Festplatten

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie Verschlüsselung Festplatten anwenden, wenn Sie Windows oder Linux VM Laufwerke, die nicht mit Azure Datenträger Verschlüsselung verschlüsselt werden. Verschlüsselung Festplatten können Sie Ihre Windows und Linux Neuerung Datenträger verschlüsseln.  Verschlüsselung wird für das Betriebssystem und die Daten Datenmengen Ihrer virtuellen Computers empfohlen.


Verschlüsselung Festplatten nutzt die Branche [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Standardfunktion von Windows und die Funktion [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux bereitstellen OS und Daten Verschlüsselung zum Schützen und schützen Sie Ihre Daten und Ihre Organisation Sicherheit und Kompatibilität Zusagen entsprechen. Verschlüsselung Festplatten ist integriert [Azure Schlüssel Tresor](https://azure.microsoft.com/documentation/services/key-vault/) , mit deren Hilfe Sie steuern und Verwalten der Schlüssel zur Verschlüsselung und vertrauliche Informationen in Ihrem Abonnement Schlüssel Tresor und dabei sicherstellen, dass alle Daten im virtuellen Computer Laufwerke zum Rest in Ihrem [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)verschlüsselt werden.

> [AZURE.NOTE] Klicken Sie auf der folgenden Windows-Serverbetriebssysteme - Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 wird Azure Datenträger Verschlüsselung unterstützt. Klicken Sie auf den folgenden Linux Serverbetriebssysteme - Ubuntu, CentOS, SUSE und SUSE Linux Enterprise Server (SLES) wird Datenträger Verschlüsselung unterstützt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Übernehmen Datenträger Verschlüsselung**.
2. In das Blade **Übernehmen Datenträger Verschlüsselung** wird eine Liste der virtuellen Computern angezeigt werden, für die Verschlüsselung Festplatten wird empfohlen.
3. Folgen Sie den Anweisungen zum Verschlüsselung auf diese virtuellen Computern anzuwenden.

![][1]

Azure-virtuellen Computern Verschlüsselung, die vom Sicherheitscenter identifiziert wurden als Verschlüsselung benötigen, empfehlen wir die folgenden Schritte aus:

- Installieren und Konfigurieren von Azure PowerShell. Dadurch können Sie zum Ausführen der PowerShell-Befehle erforderlich, um die erforderliche Azure-virtuellen Computern verschlüsseln einzurichten.
- Beziehen Sie, und führen Sie das Skript Azure Datenträger Verschlüsselung erforderliche Komponenten Azure PowerShell.
- Verschlüsseln Sie Ihre virtuellen Computer an.

[Verschlüsseln einer Azure-virtuellen Computern](security-center-disk-encryption.md) werden Sie diese Schritte durchzuführen.  In diesem Thema wird davon ausgegangen, dass Sie Windows 10 als Clientcomputer verwenden werden, aus der Sie die Verschlüsselung Festplatten konfigurieren.

Es gibt viele Methoden, mit die die erforderlichen Komponenten einrichten und konfigurieren Sie die Verschlüsselung für Azure virtuellen Computern verwendet werden können. Wenn Sie bereits über Kenntnisse in Azure PowerShell oder Azure CLI sind, müssen Sie vielleicht alternative Ansätze verwenden. Weitere Informationen zu dieser anderen Vorgehensweisen finden Sie unter [Verschlüsselung Azure Festplatten](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Siehe auch

Dieses Dokument wurde gezeigt, wie implementieren Sicherheitscenter empfohlen "Übernehmen Datenträger Verschlüsselung". Um weitere Informationen zur Verschlüsselung Festplatten, probieren Sie Folgendes ein:

- [Verschlüsselung und Key-Verwaltung mit Azure Schlüssel Tresor](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video 36 min 39 sec) – erfahren Sie, wie Datenträger Verschlüsselung Verwaltung für IaaS virtuellen Computern und Azure-Taste Tresor besser schützen und Schützen von Daten verwenden.
- [Verschlüsseln einer Azure-virtuellen Computern](security-center-disk-encryption.md) (Dokument) – erfahren, wie Sie Azure-virtuellen Computern verschlüsseln.
- [Azure Datenträger-Verschlüsselung](../security/azure-security-disk-encryption.md) (Dokument) – erfahren Sie, wie Datenträger Verschlüsselung für Windows und Linux virtuellen Computern aktivieren.

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes an.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge zu Azure Sicherheit und Kompatibilität.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
