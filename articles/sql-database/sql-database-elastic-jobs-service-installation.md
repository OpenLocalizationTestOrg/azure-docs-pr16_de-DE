<properties 
    pageTitle="Installieren von flexible Datenbankaufträge | Microsoft Azure" 
    description="Gehen Sie durch die Installation des Features flexible Position." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="installing-elastic-database-jobs-overview"></a>Installieren von flexible Datenbank Aufträge (Übersicht)

[**Flexible Datenbank Aufträge**](sql-database-elastic-jobs-overview.md) über PowerShell installiert werden kann oder über die klassischen Portal.You Azure zugreifen kann zum Erstellen und Verwalten von Aufträgen mithilfe der PowerShell-API, nur, wenn Sie das Paket PowerShell installieren. Darüber hinaus der PowerShell APIs bieten erheblich größeren Funktionsumfang als im Portal zu diesem Zeitpunkt.

Wenn Sie von einem vorhandenen **Ressourcenpool flexible Datenbank**bereits **flexible Datenbank Aufträge** über das Portal installiert haben, enthält die neueste Powershell-Vorschau Skripts aus, um die vorhandene Installation aktualisieren. Es wird dringend empfohlen, die Installation auf die neuesten **flexible Datenbank Aufträge** Komponenten aktualisieren, um neue Funktionen verfügbar gemacht über die PowerShell-APIs nutzen.

## <a name="prerequisites"></a>Erforderliche Komponenten
* Ein Azure-Abonnement. Kostenlose Testversion finden Sie unter [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. Installieren der neuesten Version über das [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376)an. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).
* [NuGet Befehlszeilenprogramms](https://nuget.org/nuget.exe) wird verwendet, um das flexible Datenbank Aufträge Paket zu installieren. Weitere Informationen finden Sie unter http://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a>Herunterladen Sie und importieren Sie das Datenbank flexible Aufträge PowerShell-Paket
1. Starten Sie Microsoft Azure PowerShell-Befehl-Fenster, und navigieren Sie zu dem Verzeichnis, in dem Sie NuGet Befehlszeilenprogramms (nuget.exe) heruntergeladen haben.

2. Herunterladen und Importieren von **flexible Datenbank Aufträge** Paket in das aktuelle Verzeichnis mit den folgenden Befehl aus:

        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease

    **Flexible Datenbank Aufträge** Dateien werden im lokalen Verzeichnis in einem Ordner namens **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** , wo *x.x.xxxx.x* die Versionsnummer widerspiegelt, platziert. Der PowerShell-Cmdlets (einschließlich erforderliche Client-DLL) befinden sich im Verzeichnis **Tools\ElasticDatabaseJobs** untergeordnete und der PowerShell-Skripts zu installieren, aktualisieren und deinstallieren Sie auch befinden sich im Verzeichnis untergeordnete **Tools** .

3. Navigieren Sie in das untergeordnete Verzeichnis von Tools unter dem Ordner Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x durch Eingeben der cd-Tools, beispielsweise:

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4.  Führen Sie das Skript.\InstallElasticDatabaseJobsCmdlets.ps1 zum Verzeichnis ElasticDatabaseJobs in $Home\Documents\WindowsPowerShell\Modules zu kopieren. Dies wird auch automatisch das Modul für die Verwendung, beispielsweise importieren:

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a>Installieren Sie die flexible Aufträge Datenbankkomponenten mithilfe der PowerShell
1.  Starten ein Microsoft Azure PowerShell-Befehl-Fensters, und navigieren Sie zu \tools untergeordnete Verzeichnis unter dem Ordner Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x: Geben Sie die cd \tools

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2.  Führen Sie der.\InstallElasticDatabaseJobs.ps1 PowerShell-Skript aus, und geben Sie Werte für die angeforderten Variablen. Dieses Skript erstellt die Komponenten beschrieben [flexible Aufträge Datenbankkomponenten und Preise](sql-database-elastic-jobs-overview.md#components-and-pricing) sowie Konfigurieren der Azure-Cloud-Dienst um abhängigen Komponenten ordnungsgemäß zu verwenden.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

Wenn Sie diesen Befehl ausführen, wird ein Fenster mit Bitte um einen **Benutzernamen** und **ein Kennwort**geöffnet. Dies ist keine Azure Ihre Anmeldeinformationen ein, geben Sie den Benutzernamen und Kennwort ein, die Administratorberechtigungen können Sie für den neuen Server erstellen möchten.

Die in diesem Beispiel aufrufen bereitgestellten Parametern können für die gewünschten Einstellungen geändert werden. Im folgenden finden Sie weitere Informationen auf das Verhalten der einzelnen Parameter:

<table style="width:100%">
  <tr>
    <th>Parameter</th>
    <th>Beschreibung</th>
  </tr>

<tr>
    <td>ResourceGroupName</td>
    <td>Stellt den Azure Ressource Gruppennamen erstellt, um den neu erstellten Azure-Komponenten enthalten. Für diesen Parameter standardmäßig "__ElasticDatabaseJob". Es wird nicht empfohlen, diesen Wert zu ändern.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Stellt den Azure Speicherort für den neu erstellten Azure-Komponenten verwendet werden soll. Für diesen Parameter standardmäßig an den zentralen US-Speicherort.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Enthält die Anzahl der Dienst Kollegen zu installieren. Für diesen Parameter Standardwert 1. Eine höhere Anzahl von Kollegen kann den Dienst skalieren und zur Bereitstellung hoher Verfügbarkeit verwendet werden. Es wird empfohlen, "2" für die Bereitstellung verwenden, hohen Verfügbarkeit des Diensts erforderlich sind.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>Stellt die Größe des virtuellen Computer für Verwendung in der Cloud-Dienst an. Für diesen Parameter standardmäßig A0. Parameterwerte A0/A1/A2/A3 werden akzeptiert die Worker-Rolle Inhaltsverzeichnisses mit einer Größe ExtraSmall und kleinen/Mittel/großen verursachen. Fo Worker Rolle an Papiergrößen, Weitere Informationen finden Sie unter [flexible Aufträge Datenbankkomponenten und Preise](sql-database-elastic-jobs-overview/#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Eine Standard Edition bereit Dienst Ebene Ziel. Für diesen Parameter standardmäßig S0. Parameterwerte von S0/S1/S2/S3 werden akzeptiert die Azure SQL-Datenbank mit dem entsprechenden SLO verursachen. Weitere Informationen zu SQL-Datenbank SLOs finden Sie unter [flexible Aufträge Datenbankkomponenten und Preise](sql-database-elastic-jobs-overview/#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Stellt den Benutzernamen Administrator für den neu erstellten Azure SQL-Datenbankserver an. Wenn nicht angegeben, wird ein PowerShell Anmeldeinformationen-Fenster geöffnet, um die Anmeldeinformationen abgefragt.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Stellt die Administratorkennworts für den neu erstellten Azure SQL-Datenbankserver an. Wenn nicht angegeben, wird ein PowerShell Anmeldeinformationen-Fenster geöffnet, um die Anmeldeinformationen abgefragt.</td>
</tr>
</table>

Für Systeme, die als Ziel Probleme großen Anzahl von Aufträgen parallel bei einer großen Anzahl von Datenbanken, die ausgeführt werden, ist es wird empfohlen, Parameter angeben, z. B.: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>Aktualisieren einer vorhandenen Datenbank flexible Aufträge Komponenten Installations mithilfe der PowerShell

**Flexible Datenbank Einzelvorgänge** können in einer vorhandenen Installation skalieren und hoher Verfügbarkeit aktualisiert werden. Dieser Vorgang ermöglicht für zukünftige Aktualisierungen der dienstleistungscode ohne zu löschen und die Datenbank Steuerelement neu zu erstellen. Dieses Verfahren kann auch in dieselbe Version so ändern Sie die Größe der Dienst virtuellen Computer oder die Anzahl der Server-Worker verwendet werden.

Um die Größe virtueller Computer, der eine Installation zu aktualisieren, führen Sie das folgende Skript mit Parametern aktualisiert, um die Werte Ihrer Wahl.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parameter</th>
  <th>Beschreibung</th>
</tr>

  <tr>
    <td>ResourceGroupName</td>
    <td>Gibt den Azure Ressource Gruppennamen verwendet, wenn die flexible Auftrag Datenbankkomponenten ursprünglich installiert wurden. Für diesen Parameter standardmäßig "__ElasticDatabaseJob". Da es nicht empfohlen wird dieser Wert zu ändern, müssen Sie dürfen nicht für diesen Parameter angeben.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Enthält die Anzahl der Dienst Kollegen zu installieren.  Für diesen Parameter Standardwert 1.  Eine höhere Anzahl von Kollegen kann den Dienst skalieren und zur Bereitstellung hoher Verfügbarkeit verwendet werden.  Es wird empfohlen, "2" für die Bereitstellung verwenden, hohen Verfügbarkeit des Diensts erforderlich sind.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>Stellt die Größe des virtuellen Computer für die Verwendung in der Cloud-Dienst an. Für diesen Parameter standardmäßig A0. Parameterwerte A0/A1/A2/A3 werden akzeptiert die Worker-Rolle Hilfethemas mit einer Größe ExtraSmall und kleinen/Mittel/großen verursachen. Fo Worker Rolle an Papiergrößen, Weitere Informationen finden Sie unter [flexible Aufträge Datenbankkomponenten und Preise](sql-database-elastic-jobs-overview/#components-and-pricing).</td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a>Installieren Sie die flexible Aufträge Datenbankkomponenten mit dem Portal

Nachdem Sie [eine Datenbank flexible Ressourcenpool erstellt](sql-database-elastic-pool-create-portal.md)haben, können Sie **Einzelvorgänge flexible Datenbank** -Komponenten, um die Ausführung von administrativen Aufgaben für jede Datenbank in der Datenbank flexible Ressourcenpool aktivieren installieren. Im Gegensatz zu wird bei Verwendung der **Datenbank flexible Aufträge** PowerShell-APIs Portal-Oberfläche gerade auf nur mit einem vorhandenen Pool ausführen.


**Geschätzte Dauer:** 10 Minuten.

1. Klicken Sie in der Dashboardansicht im Ressourcenpool flexible Datenbank über das [Azure-Portal](https://portal.azure.com/#) auf **Auftrag erstellen**.
2. Wenn Sie zum ersten Mal ein Projekt erstellen, müssen Sie **flexible Datenbank Aufträge** installieren, indem Sie auf **Vorschau AUSDRÜCKE**.
3. Übernehmen Sie die Konditionen, indem Sie auf das Kontrollkästchen.
4. Klicken Sie in der Ansicht "Services installieren" auf **Auftrag Anmeldeinformationen**.

    ![Installieren der Dienste][1]

5. Geben Sie einen Benutzernamen und das Kennwort für eine Datenbank-Administrator. Als Teil der Installation wird ein neuer Azure SQL-Datenbankserver erstellt. In dieser neuen Server wird eine neue Datenbank, die Datenbank Steuerelement genannt erstellt und verwendet, um die Metadaten für flexible Datenbank Aufträge enthalten. Benutzername und Kennwort hier erstellten dienen zur Anmeldung bei der Datenbank steuern. Für die Skript-Ausführung für die Datenbanken im Pool wird eine separate Anmeldeinformationen verwendet.

    ![Benutzername und Kennwort erstellen][2]

6. Klicken Sie auf die Schaltfläche OK. Die Komponenten werden in wenigen Minuten in einer neuen [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md)für Sie erstellt. Die neue Ressourcengruppe wird an den Anfang Karte angeheftet, wie unten dargestellt. Nach der Erstellung sind flexible Datenbankaufträge (Cloud-Dienst, SQL-Datenbank, Dienstbus und Speicher), werden alle in der Gruppe erstellt.

    ![Ressourcengruppe im Start-Karte][3]

7. Sehen Sie die folgende Meldung angezeigt, wenn Sie versuchen, erstellen oder Verwalten eines Projekts, während flexible Datenbankaufträge installieren, beim Bereitstellen von **Anmeldeinformationen** .

    ![Bereitstellung noch in Bearbeitung][4]

Wenn die Deinstallation erforderlich ist, löschen Sie die Ressourcengruppe aus. Erfahren Sie, [wie die flexible Auftrag Datenbankkomponenten deinstallieren können](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie sicher, dass jede Datenbank in der Gruppe ein Eintrag mit den entsprechenden Berechtigungen für die Ausführung von Skript erstellt wurde, finden Sie weitere Informationen [zum Schützen Ihrer SQL-Datenbank](sql-database-security.md).
Finden Sie unter [Erstellen und Verwalten einer Datenbank flexible Aufträge](sql-database-elastic-jobs-create-and-manage.md) um anzufangen.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
