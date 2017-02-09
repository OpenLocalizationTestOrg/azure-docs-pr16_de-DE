<properties
    pageTitle="Stapel Azure Service App Technical Preview 1, bevor Sie loslegen | Microsoft Azure"
    description="Schritte zum Abschließen vor der Bereitstellung von Web Apps auf Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Bevor Sie mit Azure Stapel Web Apps beginnen

> [AZURE.NOTE] Die folgende Informationen gilt nur für Azure Stapel TP1 Bereitstellungen.

Sie benötigen einige Elemente, Azure Stapel Web Apps zu installieren:

- Eine abgeschlossene Bereitstellung von [Azure Stapel Technical Preview 1](azure-stack-run-powershell-script.md)
- Genügend Speicherplatz in Ihrem System Azure Stapel für eine kleine Bereitstellung von Azure Stapel Web Apps.  Der erforderliche Speicherplatz ist ungefähr 20GB an Speicherplatz.
- [Einem Server, auf dem SQL Server ausgeführt wird](#SQL-Server).

>[AZURE.NOTE] Die folgenden Schritte aus, die alle des Client virtuellen Computers stattfinden.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Bevor Sie Azure Stapel Web Apps bereitstellen

Um eine Ressourcenanbieter bereitstellen zu können, müssen Sie die PowerShell Integrated Scripting Environment(ISE) als Administrator ausführen. Daher müssen Sie Cookies und JavaScript im Internet Explorer-Profil zu ermöglichen, die Sie für die Anmeldung bei Azure Active Directory verwenden.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Deaktivieren Sie Internet Explorer eine verbesserte Sicherheit

1.  Melden Sie sich an den Computer Azure Stapel Prüfung des Konzepts (Prüfung des Konzepts ist), als **AzureStack/Administrator**aus, und öffnen Sie **Server-Manager**.
2.  Deaktivieren Sie **Erweiterte Sicherheitskonfiguration von Internet Explorer** für Administratoren und Benutzer aus.
3.  Melden Sie sich als Administrator am virtuellen Computer ClientVM.AzureStack.local, und öffnen Sie **Server-Manager**.
4.  Deaktivieren Sie **Erweiterte Sicherheitskonfiguration von Internet Explorer** für Administratoren und Benutzer aus.

## <a name="enable-cookies"></a>Aktivieren von cookies

1.  Wählen Sie **Starten**, > **Alle apps**, > **Windows-Zubehör**. Mit der rechten Maustaste in **Internet Explorer** > **Weitere** > **als Administrator ausführen**.
2.  Wenn Sie aufgefordert werden, wählen Sie die **Verwendung empfohlen Sicherheit**, und wählen Sie dann auf **OK**.
3.  Wählen Sie in Internet Explorer **Extras** (Zahnradsymbol) > **Internetoptionen** > **private** > **Erweitert**.
4.  Klicken Sie auf **Erweitert**. Stellen Sie sicher, dass beide Kontrollkästchen **akzeptieren** ausgewählt sind, und Sie dann **OK** und **OK** erneut wählen.
5.  Schließen Sie Internet Explorer, und starten Sie PowerShell ISE als Administrator.

## <a name="install-the-latest-version-of-azure-powershell"></a>Installieren der neuesten Version von Azure PowerShell

1.  Melden Sie sich an den Computer Azure Stapel Prüfung des Konzepts ist als **AzureStack/Administrator**.
2.  Verwenden Sie die Remotedesktop-Verbindung ClientVM.AzureStack.local virtuellen Computer als Administrator anmelden.
3.  Öffnen Sie die **Systemsteuerung** , und wählen Sie **Programm deinstallieren**. 
4.  Mit der rechten Maustaste auf **Microsoft Azure PowerShell - November 2015** , und wählen Sie **Deinstallieren**aus.
5.  Nach der Deinstallation abgeschlossen ist, starten Sie ClientVM.AzureStack.local virtuellen Computers neu.
6.  Herunterladen Sie und installieren Sie der neuesten [Azure PowerShell](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SqlServer

Standardmäßig wird das Installationsprogramm Azure Stapel Web Apps festgelegt, die SQL Server verwenden, die zusammen mit der Azure Stapel-Anbieter für SQL Ressourcen installiert ist. Bei der Installation von SQL Server Ressource-Anbieter (SQL RP) stellen Sie sicher, dass Sie die Datenbank-Administratorbenutzernamen und-Kennwort beachten. Sie benötigen beide bei der Installation von Azure Stapel Web Apps.
Hinweis: Sie haben die Möglichkeit zum Bereitstellen und Verwenden von einem anderen Server zum Ausführen von SQL Server. Sie können auswählen, SQL Server-Instanz verwenden, wenn Sie die Optionen im Installationsprogramm Azure Stapel Web Apps abschließen.
