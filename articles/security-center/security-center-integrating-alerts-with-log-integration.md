<properties
   pageTitle="Integrieren von Azure-Sicherheitscenter Benachrichtigungen in Azure Log Integration (Preview) | Microsoft Azure"
   description="In diesem Artikel können Sie die Integration von Sicherheitscenter Benachrichtigungen in Azure Log Integration behilflich."
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
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Integrieren von Sicherheitscenter Benachrichtigungen in Azure Log Integration (Preview)

Viele Sicherheitsvorgänge und Reaktionsteams aufsetzen an einer Lösung Informationen zu Sicherheit und Ereignis Management (SIEM) als Ausgangspunkt für Selektierung und Untersuchen von Sicherheitswarnungen. Mit Azure Log-Integration können Kunden Sicherheitscenter Azure-Benachrichtigungen, zusammen mit virtuellen Computern Sicherheitsereignisse erfasst, Azure-Diagnose und Azure Überwachungsprotokolle, mit dem Log Analytics oder SIEM-Lösung in Echtzeit synchronisieren.

Azure Log Integration funktioniert mit HP ArcSight, Splunk, IBM QRadar und andere.

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle können werden integriert?

Azure erzeugt umfassende Protokollierung für jeden Dienst. Diese Protokolle werden als kategorisiert:

- **Control Management/Protokolle** die Steuerung Einblick in die Vorgänge Azure Ressourcenmanager erstellen, aktualisieren und löschen.
- **Data Plane Protokolle** die Steuerung Einblick in die Ereignisse ausgelöst, wenn eine Ressource Azure verwenden. Ein Beispiel ist die Windows-Ereignisprotokoll - Sicherheit und Anwendung in einer virtuellen Computern protokolliert.

Integration von Azure-Protokoll unterstützt derzeit die Integration von:

- Azure-virtuellen Computer von Protokollen
- Azure Überwachungsprotokolle
- Azure-Sicherheitscenter Benachrichtigungen

## <a name="install-azure-log-integration"></a>Installieren von Azure Log-integration

Herunterladen von [Azure melden Integration](https://www.microsoft.com/download/details.aspx?id=53324).

Der Azure Log-Integration-Dienst erfasst werden Daten aus dem Computer, auf dem es installiert ist.  Werden Daten erfasst werden:

- Protokollieren Integration von Ausnahmen, die während der Ausführung von Azure auftreten
- Kennzahlen über die Anzahl der Abfragen und verarbeiteten Ereignisse
- Über welche Azlog.exe Befehlszeilenoptionen verwendeten Statistiken

> [AZURE.NOTE] Sie können die Sammlung von Daten werden deaktivieren diese Option deaktivieren.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrieren von Azure Überwachungsprotokolle und im Sicherheitscenter Benachrichtigungen

1. Öffnen Sie das Eingabeaufforderungsfenster und die **cd** in **c:\Programme\Gemeinsame Dateien\Microsoft Azure Log Integration**an.

2. Ausführen des Befehls **Azlog Createazureid** ein [Azure Active Directory-Dienst Tilgungsanteile](../active-directory/active-directory-application-objects.md) in den Mandanten Azure Active Directory (AD) zu erstellen, die host der Azure-Abonnements

    Sie werden aufgefordert, Ihren Benutzernamen Azure.

    > [AZURE.NOTE] Sie müssen der Besitzer des Abonnements oder eine gemeinsame Administrator des Abonnements.

    Authentifizierung in Azure erfolgt über Azure AD.  Erstellen einer Tilgungsanteile Service zur Integration von Azure Log erstellt die Identität Azure AD-, die Zugriff zum Lesen aus Azure-Abonnements gewährt wird.

3. Führen Sie die **Azlog autorisieren <SubscriptionID> ** Befehl aus, um die in Schritt 2 erstellten Dienst der Tilgungsanteile Reader Zugriff auf das Abonnement zuweisen. Wenn Sie eine **SubscriptionID**nicht angeben, wird dann die Tilgungsanteile Dienst die Rolle Leser alle Abonnements zugewiesen werden, dem Sie Zugriff haben.

    > [AZURE.NOTE] Warnungen möglicherweise angezeigt werden, wenn Sie den Befehl **Autorisieren** unmittelbar nach dem Befehl **Createazureid** ausgeführt werden, da es gibt einige Wartezeit zwischen der Erstellung des Kontos Azure AD- und wenn das Konto zur Verfügung steht. Wenn Sie nach dem Ausführen des Befehls **Createazureid** zum Ausführen des Befehls **Autorisieren** ungefähr 10 Sekunden warten, sollten Sie diese Warnungen nicht angezeigt.

4. Überprüfen Sie die folgenden Ordner, um zu bestätigen, dass der Überwachungsprotokolle JSON vorhanden sind:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Überprüfen Sie die folgenden Ordner, um zu bestätigen, dass Benachrichtigungen Sicherheitscenter in diese vorhanden sind:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Zeigen Sie den standardmäßigen SIEM Datei Weiterleitung Verbinder auf den entsprechenden Ordner, um die Daten der Instanz SIEM leiten. Näheres enthält [SIEM Konfigurationen](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) auf Ihrer SIEM-Konfiguration.

Wenn Sie Fragen zur Azure Log Integration haben, senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen zur Azure Überwachungsprotokolle und Eigenschaftsdefinitionen finden Sie unter:

- [Überwachen von Vorgängen mit Ressourcenmanager](../resource-group-audit.md)
- [Listet die Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) - Audit Log Ereignisse abzurufen.

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.
