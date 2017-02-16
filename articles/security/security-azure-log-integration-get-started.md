<properties
   pageTitle="Erste Schritte mit Azure Log Integration | Microsoft Azure"
   description="Informationen Sie zum Installieren des Azure anmelden Integration-Diensts und Protokolle Azure-Speicher, Azure Überwachungsprotokolle und Azure-Sicherheitscenter Benachrichtigungen zu integrieren."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Erste Schritte mit Azure Log Integration (Preview)

Integration von Azure-Protokoll ermöglicht es Ihnen unformatierte Protokollen aus Azure Ressourcen in Ihrem lokalen Informationen zu Sicherheit und Ereignis Management (SIEM) Systeme integrieren. Diese Integration bietet ein einheitliches Dashboard für alle Anlagen, lokal oder in der Cloud, damit Sie aggregieren, koordinieren, analysieren und für Sicherheitsereignisse im Zusammenhang mit der Anwendung benachrichtigen.

In diesem Lernprogramm führt Sie durch so Azure Log Integration installieren und Protokolle Azure-Speicher, Azure Überwachungsprotokolle und Azure-Sicherheitscenter Benachrichtigungen zu integrieren. Geschätzte Zeit bis zum Abschluss des Lernprogramms ist eine Stunde.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramms abgeschlossen haben, müssen Sie Folgendes:

- Einen Computer (lokal oder in der Cloud), den Azure anmelden Integrationsdienst zu installieren. Dieser Computer muss mit .net 4.5.1 installiert eine 64-Bit-Windows-Betriebssystem ausgeführt werden. Dieser Computer heißt die **Azlog Integrator**.
- Azure-Abonnement. Wenn Sie eine nicht verfügen, können Sie für ein [kostenloses Konto](https://azure.microsoft.com/free/)anmelden.
- Azure Diagnose für Ihre Azure-virtuellen Computern (virtuellen Computern) aktiviert. Zum Aktivieren der Diagnose für Cloud Services finden Sie unter [Azure-Diagnose in Azure Cloud Services aktivieren](../cloud-services/cloud-services-dotnet-diagnostics.md). Zum Aktivieren der Diagnose für eine Azure virtueller Computer unter Windows finden Sie unter [Verwenden von PowerShell in einer virtuellen Computern unter Windows Azure-Diagnose aktivieren](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Konnektivität aufgrund der Azlog zu Azure-Speicher und zum Authentifizieren und autorisieren Azure-Abonnement.
- Für Azure-virtuellen Computer anmeldet muss der SIEM-Agent (z. B. Splunk Universal Weiterleitung, HP ArcSight Windows Ereignis Collection Agent oder IBM QRadar WinCollect) auf den Azlog Integrator installiert sein.

## <a name="deployment-considerations"></a>Aspekte beim Bereitstellen

Wenn Ereignis Lautstärke hoch ist, können Sie mehrere Instanzen von der Azlog Integrator ausführen. Lastenausgleich Azure-Diagnose Speicher-Konten für Windows *(WAD)* und die Anzahl von Abonnements für die Instanzen zur Verfügung stellen sollte auf Ihre Kapazität basieren.

Auf einem Computer mit 8 Prozessoren (Core) kann eine Instanz des Azlog Integrator ungefähr 24 Millionen Ereignisse pro Tag (~1M/hour) verarbeiten.

Klicken Sie auf einem Computer mit 4-Prozessor (Core) kann eine Instanz des Azlog Integrator ungefähr 1,5 Millionen Ereignisse pro Tag (~62.5K/hour) verarbeiten.

## <a name="install-azure-log-integration"></a>Installieren von Azure Log-integration

Herunterladen von [Azure melden Integration](https://www.microsoft.com/download/details.aspx?id=53324).

Der Azure Log-Integration-Dienst erfasst werden Daten aus dem Computer, auf dem es installiert ist.  Werden Daten erfasst werden:

- Protokollieren Integration von Ausnahmen, die während der Ausführung von Azure auftreten
- Kennzahlen über die Anzahl der Abfragen und verarbeiteten Ereignisse
- Über welche Azlog.exe Befehlszeilenoptionen verwendeten Statistiken

> [AZURE.NOTE] Sie können die Sammlung von Daten werden deaktivieren diese Option deaktivieren.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integrieren von Azure-virtuellen Computer Protokolle aus Ihren Azure-Diagnose Speicher-Konten

1. Überprüfen Sie die erforderlichen Komponenten, um sicherzustellen, dass Ihr WAD Speicherkonto Protokolle sammeln ist, bevor Sie fortfahren Ihre Azure Log-Integration aufgeführten an. Führen Sie die folgenden Schritte nicht aus, wenn Ihr WAD Speicherkonto nicht Protokolle sammeln ist.

2. Öffnen Sie das Eingabeaufforderungsfenster und die **cd** in **c:\Programme\Gemeinsame Dateien\Microsoft Azure Log Integration**an.

3. Führen Sie den Befehl

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Ersetzen Sie StorageAccountName durch den Namen des Kontos Azure-Speicher so konfiguriert, dass die um Diagnose Ereignisse von Ihrem virtuellen Computer zu erhalten.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Wenn Sie die Abonnement-Id im XML angezeigt erhalten möchten, fügen Sie die Abonnement-ID in den Anzeigenamen an:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Warten Sie 30-60 Minuten (Es konnte eine Stunde dauern), und zeigen Sie dann die Ereignisse aus, die aus dem Speicherkonto abgerufen werden. Öffnen Sie zum Anzeigen **Ereignisanzeige > Windows-Protokolle > weitergeleitete Ereignisse** auf dem Azlog Integrator.

5. Stellen Sie sicher, dass der standardmäßige SIEM Verbinder, die auf dem Computer installiert und konfiguriert ist wählen Sie die Ereignisse aus dem Ordner **Weitergeleitete Ereignisse** Ihre SIEM Instanz zu leiten. Überprüfen Sie die Konfiguration der SIEM zum Konfigurieren und die Protokolle der Integration.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Was geschieht, wenn Daten nicht in den Ordner weitergeleitete Ereignisse angezeigt werden?

Wenn Sie nach einer Stunde Daten nicht in den Ordner **Weitergeleitete Ereignisse** dann angezeigt werden:

1. Überprüfen Sie den Computer, und bestätigen Sie, dass es Azure zugreifen kann. Versuchen Sie zum Testen der Konnektivität, der [Azure-Portal](http://portal.azure.com) im Browser zu öffnen.

2. Stellen Sie sicher, dass der Benutzer Konto **Azlog** Schreibzugriff auf den Ordner **Users\azlog**verfügt.

3. Vergewissern Sie sich das Speicherkonto hinzugefügt haben, in den Befehl **Azlog Quelle hinzufügen** aufgeführt ist, wenn Sie den Befehl **Azlog Quellliste**ausführen.
4. Wechseln Sie zu **Ereignisanzeige > Windows-Protokolle > Anwendung** gemeldet aus Azure Log Integration angezeigt, wenn ein Fehler auftritt.

Wenn Sie immer noch nicht finden Sie unter den Ereignissen, dann:

1. Herunterladen von [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/).

2. Verbinden Sie mit dem Speicherkonto hinzugefügt haben, in den Befehl **Azlog Quelle hinzuzufügen**.

3. Microsoft Azure-Speicher-Explorer navigieren Sie zu der Tabelle **WADWindowsEventLogsTable** , um festzustellen, ob irgendwelche Daten. Wenn dies nicht der Fall ist, klicken Sie dann auf dem virtuellen Computer Diagnose nicht richtig konfiguriert ist.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrieren von Azure Überwachungsprotokolle und Sicherheitscenter Benachrichtigungen

1. Öffnen Sie das Eingabeaufforderungsfenster und die **cd** in **c:\Programme\Gemeinsame Dateien\Microsoft Azure Log Integration**an.

2. Führen Sie den Befehl

        azlog createazureid

      Dieser Befehl werden Sie aufgefordert, Ihren Benutzernamen Azure. Der Befehl erstellt ein [Azure Active Directory-Dienst Tilgungsanteile](../active-directory/active-directory-application-objects.md) klicken Sie dann in der Azure AD-Mandanten, die der Azure-Abonnements hosten, in denen der Benutzer ein Administrator, eine gemeinsame Administrator oder einen Besitzer ist. Der Befehl schlägt fehl, wenn der Benutzer nur ein Gastbenutzer in den Azure AD-Mandanten ist. Authentifizierung in Azure erfolgt über Azure Active Directory (AD).  Erstellen einer Tilgungsanteile Service zur Integration Azlog erstellt die Identität Azure AD-, die Zugriff zum Lesen aus Azure-Abonnements gewährt wird.

3. Führen Sie den Befehl

        azlog authorize <SubscriptionID>

      Dies weist Reader Zugriff auf das Abonnement für den Dienst Hauptbenutzer in Schritt 2 erstellt haben. Wenn Sie eine SubscriptionID nicht angeben, versucht es Hauptbenutzer Dienst Rolle zuweisen auf alle, die Abonnements, dem Sie eine beliebige Zugriff haben.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Warnungen möglicherweise angezeigt werden, wenn Sie den Befehl **Autorisieren** unmittelbar nach dem **Createazureid** Befehl ausführen. Es gibt einige Wartezeit zwischen der Erstellung des Kontos Azure AD- und wenn das Konto zur Verfügung steht. Wenn Sie nach dem Ausführen des Befehls **Createazureid** zum Ausführen des Befehls **Autorisieren** ungefähr 10 Sekunden warten, sollten Sie diese Warnungen nicht angezeigt.

4. Überprüfen Sie die folgenden Ordner, um zu bestätigen, dass der Überwachungsprotokolle JSON vorhanden sind:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Überprüfen Sie die folgenden Ordner, um zu bestätigen, dass Benachrichtigungen Sicherheitscenter in diese vorhanden sind:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Zeigen Sie den standardmäßigen SIEM Datei Weiterleitung Verbinder auf den entsprechenden Ordner, um die Daten der Instanz SIEM leiten. Möglicherweise wird die Zuordnung einiger Feld basierend auf der SIEM-Produkt, das Sie verwenden.

Wenn Sie Fragen zur Azure Log Integration haben, senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie wie Azure Log Integration installieren und Integrieren von Protokollen aus Azure-Speicher. Weitere Informationen finden Sie hier:

- [Microsoft Azure Log Integration für Azure Protokolle (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center für Details, systemvoraussetzungen und Anweisungen Azure Log Integration installieren.
- [Einführung in Azure Log Integration](security-azure-log-integration-overview.md) – dieses Dokument führt Sie in Azure Log Integration, deren wichtigsten Funktionen und deren Funktionsweise.
- [Partner Konfigurationsschritte](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – in diesem Blogbeitrag wird gezeigt, wie Azure Log-Integration mit partnerlösungen Splunk, HP ArcSight und IBM QRadar entwickelt konfigurieren.
- [Azure Log Integration häufig gestellte Fragen (FAQS)](security-azure-log-integration-faq.md) – diese häufig gestellte Fragen finden Sie Antworten auf Fragen zur Azure Log Integration.
- [Integration von Sicherheitscenter Benachrichtigungen mit Azure melden Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – dieses Dokument wird gezeigt, wie-Sicherheitscenter Benachrichtigungen, zusammen mit virtuellen Computern Sicherheitsereignisse erfasst von Azure-Diagnose und Azure Überwachungsprotokolle, mit dem Log Analytics oder SIEM-Lösung synchronisieren.
- [Neue Features für Azure Diagnose und Azure Überwachungsprotokolle](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – in diesem Blogbeitrag lernen Sie Überwachungsprotokolle Azure und andere Features, die Ihnen helfen gewinnen Sie Einsichten in die Vorgänge Azure Ressourcen.
