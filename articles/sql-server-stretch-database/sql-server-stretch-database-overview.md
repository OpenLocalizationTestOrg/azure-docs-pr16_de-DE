<properties
    pageTitle="Dehnen Datenbank Übersicht | Microsoft Azure"
    description="Erfahren Sie, wie gedehnt Datenbank kalten Daten transparent und sicher in der Cloud Microsoft Azure vom migriert werden."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Dehnen Datenbank (Übersicht)

Dehnen Datenbank migriert die kalten Daten transparent und sicher in der Cloud Microsoft Azure an.

Wenn Sie sofort mit gedehnt Datenbank beginnen möchten, finden Sie unter [Erste Schritte, indem Sie die Datenbank aktivieren gedehnt-Assistenten ausführen](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Was sind die Vorteile von gedehnt Datenbank?
Dehnen Datenbank bietet folgende Vorteile:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Stellt Kosten\-effektiven Verfügbarkeit für kalt Daten
Dehnen Sie warme und kalte Transaktionen Daten dynamisch aus SQL Server in Microsoft Azure mit SQL Server gedehnt-Datenbank. Im Gegensatz zu normalen kalt Datenspeicher sind Ihre Daten immer online und Abfragen zur Verfügung. Sie können mehr Daten Aufbewahrung Zeitachsen bereitstellen, ohne die Welt bei großen Tabellen wie Kunden Reihenfolge Verlauf. Nutzbringend mit niedrigen Kosten von Azure statt Skalierung teure, klicken Sie auf\-Speicher von Gebäuden. Wählen Sie die Preisgestaltung Ebene aus, und Konfigurieren von Einstellungen im Portal Azure Kontrolle über Kosten. Je nach Bedarf nach oben oder unten skaliert werden soll. Besuchen Sie die [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) Seite Details.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Änderungen an Abfragen oder Anwendung nicht erforderlich ist.
Zugriff auf Ihre SQL Server-Daten nahtlos unabhängig davon, ob es auf\-von Gebäuden oder in der Cloud gestreckt.  Sie legen Sie die Richtlinie, die festlegt, in dem Daten werden gespeichert, und SQL Server behandelt das Verschieben von Daten in den Hintergrund. Die gesamte Tabelle ist immer online und abfragbar. Und, gedehnt Datenbank setzt voraus Änderungen an vorhandenen Abfragen oder Anwendung – der Speicherort der Daten für die Anwendung vollständig transparent ist.

### <a name="streamlines-on-premises-data-maintenance"></a>Optimierung auf\-Daten Wartung von Gebäuden
Klicken Sie auf Verkleinern\-von Gebäuden Wartung und Speicherplatz für Ihre Daten. Sicherungskopien für Ihre auf\-lokale Daten schneller ausgeführt und innerhalb des Fensters Wartung Fertig stellen. Sicherungskopien für die Cloud Teil Ihrer Daten automatisch ausgeführt. Ihre auf\-lokale Speicher Anforderungen wurden erheblich verringert. Azure-Speicher kann 80 % kostengünstiger als hinzufügen auf\-SSD von Gebäuden.

### <a name="keeps-your-data-secure-even-during-migration"></a>Ihre Daten behält auch während der Migration secure
Verlässliche Sicherheit an, wie Sie Ihre wichtigsten Applikationen sicher in der Cloud Strecken. SQL Server immer verschlüsselt bietet Verschlüsselung für Ihre Daten in Bewegung. Zeile Ebene Sicherheit Datensatzebene und andere erweiterte SQL Server-Sicherheits-Features funktionieren auch mit gedehnt Datenbank zum Schutz Ihrer Daten.

## <a name="what-does-stretch-database-do"></a>Welche Funktion hat die Datenbank gedehnt?
Nachdem Sie für eine SQL Server-Instanz, einer Datenbank und mindestens eine Tabelle gedehnt Datenbank aktivieren, beginnt gedehnt Datenbank im Hintergrund Ihrer kalten Daten in Azure migrieren.

-   Wenn Sie kalte Daten in einer separaten Tabelle speichern möchten, können Sie die gesamte Tabelle migrieren.

-   Wenn die Tabelle wichtiges und kalte Daten enthält, können Sie eine Filterfunktion, markieren Sie die Zeilen migrieren angeben.

**Sie müssen nicht vorhandenen Abfragen und Client-apps zu ändern.** Weiterhin nahtlose Zugriff auf lokale und remote-Daten, auch während der Migration von Daten auftreten. Es ist ein wenig Wartezeit für remote Abfragen, aber nur diese Wartezeit beim Auftreten kalt Datenabfrage.

Wenn ein Fehler, während der Migration auftritt zu **Datenbank gedehnt wird sichergestellt, dass keine Daten verloren** . Es kann darüber hinaus "Wiederholen" Behandeln von Verbindungsproblemen, die während der Migration auftreten können. Eine dynamische Ansicht enthält den Status der Migration.

**Sie können Datenmigration anhalten** der Problembehandlung auf dem lokalen Server oder die verfügbaren Bandbreite maximieren.

![Dehnen Datenbank (Übersicht)][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Ist gedehnt Datenbank für Sie?
Wenn Sie eine die folgenden Aussagen vornehmen können, möglicherweise gedehnt Datenbank Ihren Anforderungen entsprechen, und lösen Sie die Probleme hilfreich.

|Wenn Sie ein Entscheidungsträger sind|Wenn Sie einen Datenbankadministrator sind|
|------------------------------|-------------------|
|Ich habe Transaktionen Daten längere Zeit beibehalten.|Die Größe des mein Tabellen ist außerhalb des Steuerelements abrufen.|
|Manchmal habe ich die kalt Datenabfrage ausgeführt.|Meine Benutzer angenommen, dass sie Zugriff auf Daten kalt möchten, aber sie nur selten verwenden.|
|Ich habe apps, einschließlich älterer apps, die ich nicht aktualisieren möchten.|Ich habe zum Kauf und Hinzufügen von mehr Speicher beibehalten.|
|Ich möchte eine Möglichkeit zum Speichern von Geld Speichermenge zu suchen.|Ich kann nicht sichern oder Wiederherstellen von solche großen Tabellen innerhalb der Vereinbarung zum SERVICELEVEL.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Welche Arten von Datenbanken und Tabellen sind Kandidaten für eine Datenbank gedehnt?
Dehnen Sie Datenbank Ziele Transaktionen Datenbanken mit großen Mengen von kalt Daten in einer kleinen Anzahl von Tabellen in der Regel gespeichert. In diesen Tabellen können mehr als eine Milliarden Zeilen enthalten.

Wenn Sie das Tabellenfeature zeitliche von SQL Server 2016 verwenden, verwenden Sie gedehnt Datenbank migrieren vollständig oder teilweise der zugehörigen Verlaufstabelle Kosten\-effektiven Speicher in Azure. Weitere Informationen finden Sie unter [Verwalten von Aufbewahrungsrichtlinien von zurückliegenden Daten in System Versionsnummern zeitliche Tabellen](https://msdn.microsoft.com/library/mt637341.aspx).

Verwenden Sie gedehnt Datenbank Advisor, ein Feature von SQL Server 2016 Upgrade Advisor, um Datenbanken und Tabellen für gedehnt Datenbank zu identifizieren. Weitere Informationen finden Sie unter [identifizieren Datenbanken und Tabellen für die Datenbank gedehnt](sql-server-stretch-database-identify-databases.md). Weitere Informationen zu blockierenden Probleme finden Sie unter [Einschränkungen für gedehnt Datenbank](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Testen Sie Laufwerk gedehnt Datenbank
**Testen Sie Laufwerk gedehnt-Datenbank mit der Datenbank AdventureWorks.** Zum Abrufen der Datenbank AdventureWorks herunterladen Sie mindestens die Datenbankdatei und die Datei Beispiele und Skripts aus [hier](https://www.microsoft.com/download/details.aspx?id=49502). Nachdem Sie die Beispieldatenbank in eine Instanz von SQL Server 2016 wiederherstellen, entpacken Sie die Datei Beispiele, und öffnen Sie die Beispiele für gedehnt DB-Datei aus dem Ordner gedehnt DB. Führen Sie die Skripts in dieser Datei, um den Abstand verwendet, indem Sie Ihre Daten vor und nach der Aktivierung gedehnt Datenbank, Verfolgen des Fortschritts der Datenmigration und zu bestätigen, dass Sie können vorhandene Daten Abfragen, und fügen neue Daten, während sowohl weiterhin nach der Datenmigration zu prüfen.

## <a name="next-step"></a>Als Nächstes
**Identifizieren von Datenbanken und Tabellen, die für die Datenbank gedehnt geeignet sind.** Herunterladen von SQL Server 2016 Upgrade Advisor, und führen Sie die gedehnt Datenbank Advisor zum Identifizieren von Datenbanken und Tabellen, die für die Datenbank gedehnt geeignet sind. Dehnen Datenbank Advisor identifiziert auch blockierenden Probleme. Weitere Informationen finden Sie unter [identifizieren Datenbanken und Tabellen für die Datenbank gedehnt](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
