<properties
    pageTitle="SQLServer auf Azure-virtuellen Computern häufig gestellte Fragen zu | Microsoft Azure"
    description="In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zur Azure-virtuellen Computern SQL Server ausgeführt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQLServer auf Azure-virtuellen Computern häufig gestellte Fragen

Dieses Thema bietet Antworten auf einige der am häufigsten gestellten Fragen zu [SQL Server auf Azure virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/)ausgeführt werden.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

1. **Wie erstelle ich eine Azure-virtuellen Computern mit SQL Server?**

    Es gibt zwei Möglichkeiten zur Verfügung. Die einfachste Lösung besteht im Erstellen einer virtuellen Computern, die SQL Server umfasst. Ein Lernprogramm für Azure anmelden und eine SQL VM aus dem Portal erstellen finden Sie unter [Bereitstellen einer SQL Server-virtuellen Computern im Portal Azure](virtual-machines-windows-portal-sql-server-provision.md). Sie haben auch die Möglichkeit, manuell der Installation von SQL Server auf einem virtuellen Computer und Wiederverwenden von einer lokalen Lizenz mit [Lizenz Mobilität über Software Assurance-Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Was ist der Unterschied zwischen SQL-virtuellen Computern und der SQL-Datenbank-Dienst?**

    Im Prinzip läuft SQL Server auf eine Azure-virtuellen Computern nicht, die mit SQL Server in einem remote Datencenter abweicht. Im Gegensatz dazu bietet [SQL-Datenbank](../sql-database/sql-database-technical-overview.md) Datenbank als Dienst an. Mit SQL-Datenbank haben Sie nicht Zugriff auf den Computern, die Ihre Datenbanken hosten. Eine vollständige Vergleich, finden Sie unter [Wählen Sie eine Cloud SQL Server-Option aus: Azure SQL (PaaS)-Datenbank oder SQL Server auf Azure-virtuellen Computern (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Wie kann ich meinen lokalen SQL Server-Datenbank in der Cloud migrieren?**

    Erstellen Sie zuerst eine Azure-virtuellen Computern mit einer SQL Server-Instanz ein. Migrieren von Ihrem lokalen Datenbanken klicken Sie dann auf diese Instanz. Strategien zum Migrieren von Daten finden Sie unter [Migrieren einer SQL Server-Datenbank mit SQL Server auf eine Azure-virtuellen Computer](virtual-machines-windows-migrate-sql.md).

2. **Kann ich ändern die installierten Features oder installieren eine zweite Instanz von SQL Server auf dem gleichen virtuellen Computer?**

    Ja. Die SQL Server-Installationsmedien befindet sich in einem Ordner auf Laufwerk **C** . Führen Sie **Setup.exe** von dort neue Instanzen von SQL Server hinzufügen oder Ändern der installierten Funktionen von SQL Server auf dem Computer an.

3. **Wie kann ich auf eine neue Version/Version von SQL Server auf eine Azure-virtuellen Computer aktualisieren?**

    Es gibt derzeit keine in-situ-Upgrades für SQL Server auf eine Azure-virtuellen Computer ausgeführt. Erstellen Sie einer neuen Azure-virtuellen Computern mit der gewünschten SQL Server-Version/Edition, und klicken Sie dann migrieren Sie Ihre Datenbanken auf dem neuen Server mit den [Daten Migration Techniken](virtual-machines-windows-migrate-sql.md).

4. **Wie kann ich meine lizenzierte Version von SQL Server auf eine Azure-virtuellen Computer installieren?**

    Kopieren der SQL Server-Installationsmedien auf den Windows Server virtuellen Computer, und klicken Sie dann auf dem virtuellen Computer installieren von SQL Server. Für die Lizenzierung Gründe, müssen Sie die [Lizenz Mobilität über Software Assurance-Azure](https://azure.microsoft.com/pricing/license-mobility/)verfügen.

5. **Haben Sie die SQL-Kosten eines virtuellen Computers Interesse, wenn sie nur für Standby/Failover verwendet wird?**

    Wenn Sie die SQL VM durch den Katalog erstellen, müssen Sie eine separate Lizenz für die standby SQL VM, und die Preise identisch ist. Wenn Sie SQL Server auf einem Computer mit [Lizenz Mobilität](https://azure.microsoft.com/pricing/license-mobility/)manuell installieren, müssen Sie die Möglichkeit, eine kostenlose passive SQL-Instanz für Failover haben. Überprüfen Sie die Einschränkungen und Anforderungen an.

6. **Wie werden Updates und Servicepacks auf einen SQL Server-virtuellen angewendet?**

    Virtuellen Computern gewähren Sie die Kontrolle über die Host-Computer, einschließlich wann und wie Sie Updates anwenden. Für das Betriebssystem können Sie Windows-Updates manuell anwenden, oder Sie können einen Planung Dienst [Automatisierte Patch](virtual-machines-windows-classic-sql-automated-patching.md)aufgerufen aktivieren. Automatische Patchen Installationen von Updates, die wichtig, einschließlich SQL Server-Updates in dieser Kategorie markiert sind. Andere optionale Updates mit SQL Server müssen manuell installiert werden.

7. **Ist es möglich, zum Einrichten von Konfigurationen im Katalog virtuellen Computern (zum Beispiel Windows 2008 R2 + SQL Server 2012) nicht angezeigt?**

    Nein. Für virtuellen Computern Gallery-Bilder, die SQL Server enthalten, müssen Sie eines der Bilder bereitgestellten auswählen.

9. **Wie installiere ich meine Azure-virtuellen Computers SQL Datentools?**

    Herunterladen Sie und installieren Sie die SQL-Datentools von [Microsoft SQL Server Data Tools – Business Intelligence für Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Ressourcen

Sehen Sie einen Überblick über die SQL Server auf Azure virtuellen Computern der [Azure-virtuellen Computer ist die beste Plattform für SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)video ein. Sie können auch eine gute Einführung im Thema [SQL Server auf Azure-virtuellen Computern Übersicht](virtual-machines-windows-sql-server-iaas-overview.md)erhalten.

Weitere Ressourcen gehören:

- [Bereitstellen einer SQL Server-virtuellen Computern Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [Migrieren einer Datenbank mit SQLServer ein Azure-virtuellen Computers](virtual-machines-windows-migrate-sql.md)
- [Hohe Verfügbarkeit und Wiederherstellung für SQLServer in Azure-virtuellen Computern](virtual-machines-windows-sql-high-availability-dr.md)
- [Leistung bewährte Methoden für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-performance.md)
- [Anwendung Mustern und Entwicklungsstrategien für SQLServer in Azure-virtuellen Computern](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
