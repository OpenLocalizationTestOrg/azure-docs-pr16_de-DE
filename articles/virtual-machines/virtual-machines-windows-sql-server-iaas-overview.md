<properties
    pageTitle="Übersicht über SQLServer auf Azure-virtuellen Computern | Microsoft Azure"
    description="Informationen Sie zum vollständige SQL Server-Editionen auf Azure-virtuellen Computern ausgeführt werden. Abrufen von direkten Links auf alle SQL Server virtueller Computer Bilder und verknüpften Inhalt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Übersicht über SQLServer auf Azure-virtuellen Computern

In diesem Thema werden Ihre Optionen für die Ausführung von SQL Server auf Azure-virtuellen Computern (virtuelle Computer), zusammen mit [Links zu Portal-Bilder](#option-1-create-a-sql-vm-with-per-minute-licensing) und einen Überblick über die [allgemeinen Aufgaben](#manage-your-sql-vm)an.

>[AZURE.NOTE] Wenn Sie bereits mit SQL Server vertraut sind und nur Gewusst wie: Bereitstellen eines SQL Server virtuellen Computers anzeigen möchten, finden Sie unter [Bereitstellen einer SQL Server-virtuellen Computern Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>(Übersicht)
Wenn Sie einen Datenbankadministrator oder ein Entwickler sind, Möglichkeit Azure-virtuellen Computern, Ihren lokalen SQL Server-Auslastung und Anwendungen in der Cloud zu verschieben. Das folgende Video bietet eine technische Übersicht über SQL Server Azure-virtuellen Computern an.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Das Video umfasst die folgenden Bereiche an:

|Zeit|Bereich|
|---|---|
| 00:21 | Was sind Azure-virtuellen Computern? |
| 01:45 | Sicherheit |
| 02:50 | Konnektivität |
| 03:30 | Speicher Zuverlässigkeit und Leistung |
| 05:20 | Virtueller Computer Größen |
| 05:54 | Hohe Verfügbarkeit und Vereinbarung zum SERVICELEVEL |
| 07:30 | Unterstützung für Konfiguration |
| 08:00 | Für die Überwachung |
| 08:32 | Demo: Erstellen eines SQL Server-2016 virtuellen Computers |

>[AZURE.NOTE] Das Video liegt der Schwerpunkt auf SQL Server 2016, sondern Azure bietet virtueller Computer Bilder für viele Versionen von SQL Server 2008, 2012, 2014 und 2016 einschließlich. 

## <a name="scenarios"></a>Szenarien
Es gibt viele Gründe, die Ihre Daten in Azure gehostet werden sollen. Wenn eine Anwendung in Azure verschoben wird, verbessert es die Leistung um die Daten zu verschieben. Es gibt jedoch einige weitere Vorteile. Sie können automatisch auf mehrere Daten Centers für eine globale Anwesenheits- und Disaster Wiederherstellung zugreifen. Die Daten sind auch hochgradig abgesichert und permanente.

SQL Server auf Azure-virtuellen Computern ausgeführt wird, eine Option zum Speichern von relationalen Daten in Azure. Es ist eine gute Wahl für verschiedene Szenarien. Beispielsweise möchten Sie möglicherweise den Azure-virtuellen Computer ebenso wie möglich an den Computer mit einer lokalen SQL Server zu konfigurieren. Oder möchten Sie möglicherweise zusätzliche Anwendungen und Dienste auf dem gleichen Datenbankserver ausführen. Es gibt zwei Hauptfenster Ressourcen, die Sie über noch mehr Szenarien und Aspekte vorstellen helfen können:

 - [SQL Server auf Azure-virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/) bietet einen Überblick über die beste Szenarios für die Verwendung von SQL Server auf Azure-virtuellen Computern an. 
 - [Wählen Sie eine Cloud SQL Server-Option aus: Azure SQL (PaaS)-Datenbank oder SQL Server auf Azure-virtuellen Computern (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) ermöglicht einen detaillierten Vergleich zwischen SQL-Datenbank und SQL Server auf einem virtuellen Computer ausgeführt.

## <a name="create-a-new-sql-vm"></a>Erstellen einer neuen SQL VM
Die folgenden Abschnitte enthalten Links direkt Azure-Portal für die SQL Server-virtuellen Computern Katalog Bilder. Je nach dem Bild, das Sie auswählen, können Sie entweder Bezahlung für SQL Server-Lizenzierung Kosten auf Basis pro Minute oder können Sie Ihre eigenen Lizenz (BYOL) übertragen.

Das [Bereitstellen einer SQL Server-virtuellen Computern im Portal Azure](virtual-machines-windows-portal-sql-server-provision.md)-Lernprogramm finden Sie schrittweise Anleitung für dieses Verfahren. Überprüfen Sie [Leistung bewährte Methoden für SQL Server-virtuellen Computern](virtual-machines-windows-sql-performance.md), die erklärt, so markieren Sie die entsprechenden Computer Größe und andere Features, die während der Bereitstellung zur Verfügung.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Option 1: Erstellen einer SQL VM mit Lizenzierung pro minute
Die folgende Tabelle enthält eine Matrix von verfügbaren SQL Server-Bilder im Katalog virtuellen Computern an. Klicken Sie auf eine beliebige Verknüpfung zum Erstellen einer neuen SQL VM mit der angegebenen Version, Edition und Betriebssystem beginnen.

|Version|Betriebssystem|Edition|
|---|---|---|
|**SQL-2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Entwickler](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL-2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|WindowsServer 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|WindowsServer 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Option 2: Erstellen einer SQL VM mit einer bestehenden Lizenz
Sie können auch eigene Lizenz (BYOL) bringen. In diesem Szenario wird nur für den virtuellen Computer ohne eventuelle zusätzlichen Gebühren für SQL Server-Lizenzierung bezahlen. Um eigene Lizenz zu verwenden, können verwenden Sie die Matrix von SQL Server-Versionen, Editionen und Betriebssysteme unten. **{BYOL}**ist diese Bildnamen im Portal vorangestellt.

|Version|Betriebssystem|Edition|
|---|---|---|
|**SQLServer 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Um Bilder BYOL VM verwenden zu können, müssen Sie und Enterprise Agreement mit [Lizenz Mobilität über Software Assurance-Azure](https://azure.microsoft.com/pricing/license-mobility/). Sie benötigen ferner eine gültige Lizenz für die Version/Edition von SQL Server, die Sie verwenden möchten. Sie müssen [die notwendigen BYOL Informationen an Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) innerhalb von **10** Tagen nach Ihrer virtuellen Computer bereitgestellt.

## <a name="manage-your-sql-vm"></a>Verwalten Sie Ihrer SQL virtueller Computer
Nach der Bereitstellung Ihrer SQL Server virtueller Computer, gibt es mehrere optional Verwaltungsaufgaben. In vielen Aspekten konfigurieren und Verwalten von SQL Server genau, wie Sie eine lokalen SQL Server-Instanz verwalten möchten. Es gibt jedoch einige Aufgaben Azure-spezifischen. Markieren Sie in den folgenden Abschnitten einige dieser Bereiche mit Links zu weiteren Informationen.

### <a name="connect-to-the-vm"></a>Verbinden mit dem virtuellen Computer
Einer der grundlegendsten Management Schritte ist für die Verbindung zu Ihrer SQL Server virtueller Computer mithilfe von Tools, wie etwa SQL Server Management Studio (SSMS). Herstellen einer Verbindung mit Ihrem neuen SQL Server virtueller Computer Anweisungen finden Sie unter [Verbinden zu einer SQL Server virtuellen Computern auf Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrieren von Daten
Wenn Sie eine vorhandene Datenbank haben, erhalten Sie, die für die neu eingerichtete SQL VM verschieben möchten. Eine Liste der Migrationsoptionen sowie Anleitungen finden Sie unter [Migrieren einer Datenbank mit SQL Server ein Azure-virtuellen Computers](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Konfigurieren der hohen Verfügbarkeit
Wenn Sie eine hohen Verfügbarkeit benötigen, erwägen Sie das Konfigurieren von SQL Server Verfügbarkeit Gruppen. Dies umfasst mehrere Azure virtuelle Computer in einem virtuellen Netzwerk. Azure-Portal enthält eine Vorlage für die von dieser Konfiguration für Sie fest. Weitere Informationen finden Sie unter [Konfigurieren einer AlwaysOn Verfügbarkeit Gruppe virtuellen-Ressourcenmanager Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Wenn Sie Ihre Verfügbarkeit Gruppe und die zugehörigen Zuhörer manuell konfigurieren möchten, finden Sie unter [Konfigurieren von AlwaysOn Verfügbarkeit Gruppen Azure-virtuellen Computer](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Weitere Aspekte der hohen Verfügbarkeit finden Sie unter [hohe Verfügbarkeit und Wiederherstellung für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Sichern Ihrer Daten
Azure-virtuellen Computern können [Automatische Sicherung](virtual-machines-windows-sql-automated-backup.md)nutzen, die regelmäßig Sicherungskopien der Datenbank zu Blob-Speicher erstellt. Sie können auch manuell dieses Verfahren zu verwenden. Weitere Informationen finden Sie unter [Verwenden von Azure-Speicher für SQL Server sichern und Wiederherstellen](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Eine Übersicht über alle Optionen zum Sichern und Wiederherstellen finden Sie unter [Sichern und Wiederherstellen für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatisieren von updates
Azure-virtuellen Computern können [Automatisierte Patch](virtual-machines-windows-sql-automated-patching.md) ein Wartungszeitfensters für die Installation von wichtigen Windows planen und SQL Server automatisch aktualisiert.

### <a name="customer-experience-improvement-program-ceip"></a>Customer Experience Improvement-Programm (CEIP)
Customer Experience Improvement Programm (CEIP) ist standardmäßig aktiviert. Sendet regelmäßig Berichte an Microsoft zur Verbesserung von SQL Server. Es ist keine Management-Aufgabe, die mit CEIP erforderlich, es sei denn, Sie deaktivieren möchten, um ihn nach der Bereitstellung. Sie können anpassen oder zur Teilnahme am CEIP deaktivieren, indem Sie die Verbindung mit dem virtuellen Computer mit dem Remotedesktop. Führen Sie dann das Programm **SQL Server-Fehler und Verwendungsberichte** aus. Folgen Sie den Anweisungen zum reporting deaktivieren. 

Weitere Informationen finden Sie unter Abschnitt CEIP des Themas [Lizenzvertrag anzunehmen](https://msdn.microsoft.com/library/ms143343.aspx) . 

## <a name="next-steps"></a>Nächste Schritte
[Durchsuchen der Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf Azure-virtuellen Computern.

Weitere Frage? Zunächst finden Sie unter dem [SQL Server auf Azure-virtuellen Computern häufig gestellte Fragen](virtual-machines-windows-sql-server-iaas-faq.md). Aber auch Ihre Fragen oder Kommentare an das Ende einer beliebigen SQL VM Themen Interaktion mit Microsoft und der Community hinzufügen.
