<properties 
    pageTitle="Erste Schritte mit flexible Datenbanktools" 
    description="Grundlegende Erläuterung der Datenbank flexible Toolsfeature Azure SQL-Datenbank, einschließlich einfach, zum Beispiel-app ausführen." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Erste Schritte mit flexible Datenbanktools

Dieses Dokument werden die Entwicklertools Oberfläche durch Ausführen der Stichprobe app vorgestellt. Das Beispiel erstellt eine einfache sharded Anwendung und wichtige Funktionen von flexible Datenbanktools untersucht. Das Beispiel veranschaulicht die Funktionen der [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md)

Wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), um die Bibliothek zu installieren. Beachten Sie, dass die Bibliothek mit der nachfolgend beschriebenen Beispielapp installiert ist.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Mit c# Visual Studio 2012 oder höher ist erforderlich. Herunterladen einer kostenlosen Version in [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)an.
2. NuGet 2.7 oder höher. Um die neueste Version zu erhalten, finden Sie unter [Installieren von NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Herunterladen und Ausführen der Beispiel-app

Die **flexible mit SQL Azure-Datenbank – erste Schritte** Beispiel-Anwendung veranschaulicht die wichtigsten Aspekte der Entwicklung Erfahrung für sharded Applikationen flexible Datenbanktools SQL Azure verwenden. Es Schwerpunkt Key verwenden Fällen für [Shard zuordnen Management](sql-database-elastic-scale-shard-map-management.md), [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md) und [Abfragen mit mehreren Shard](sql-database-elastic-scale-multishard-querying.md)ein. Zum Herunterladen, und führen Sie das Beispiel, gehen Sie folgendermaßen vor: 

1. Öffnen Sie Visual Studio, und wählen Sie **Datei -> neu-Projekt >**.
2. Klicken Sie im Dialogfeld auf **Online**.

    ![Neues Projekt > Online][2]
3. Klicken Sie dann auf **Visual c#** unter **Beispiele**.

    ![Klicken Sie auf Visual C#][3]
4. Geben Sie in das Suchfeld **flexible Db** So suchen Sie nach der Stichprobe. Der Titel wird **Flexible DB Tools für Azure SQL - erste Schritte** .

    ![Suchfeld][1]
 
5. Wählen Sie aus der Stichprobe, wählen Sie einen Namen und einen Speicherort für das neue Projekt ein, und drücken Sie **OK** , um das Projekt zu erstellen.
6. Öffnen Sie **die App** in die Lösung für die Stichprobe Projekt, und folgen Sie die Anweisungen in der Datei, um den Servernamen für SQL Azure-Datenbank und Ihre Anmeldeinformationen (Benutzername und Kennwort) hinzufügen.
7. Erstellen Sie, und führen Sie die Anwendung. Wenn Sie aufgefordert werden, warten Sie Visual Studio NuGet-Pakete der Lösung wiederherstellen. Dadurch wird die neueste Version der Clientbibliothek flexible Datenbank aus NuGet herunterladen.
8. Mit den anderen Optionen erfahren Sie mehr über die Funktionen der Client-Bibliothek wiedergeben. Beachten Sie die Schritte, die in der Verwaltungskonsole die Anwendung annimmt, ausgeben und gerne untersuchen den Code im Hintergrund.

    ![Fortschritt][4]

Herzlichen Glückwunsch – Sie erfolgreich erstellt haben, und führen Sie die erste sharded Anwendung flexible Datenbanktools Azure SQL-Datenbank mit. Machen Sie einen Blick auf die mehrere Shards hinweg, die in die Stichprobe durch Herstellen einer Verbindung mit Visual Studio oder SQL Server Management Studio zu Ihrer Azure-Datenbankserver erstellt. Beachten Sie neue Stichprobe Shard Datenbanken und eine Shard Karte Manager-Datenbank, die die Stichprobe erstellt hat.

> [AZURE.IMPORTANT] Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Key Textstellen im Beispiel

1. **Verwalten von mehrere Shards hinweg und Shard Karten**: der Code veranschaulicht, wie mehrere Shards hinweg, Bereiche, konzipiert und Zuordnungen in Datei **ShardMapManagerSample.cs**. Weitere Informationen zu diesem Thema hier finden Sie: [Shard Karte Management](http://go.microsoft.com/?linkid=9862595).  
2. **Daten abhängige Routing**: Routing der Transaktionen, die die richtigen Shard im **DataDependentRoutingSample.cs**angezeigt wird. Weitere Informationen hierzu finden Sie unter [Daten abhängige Routing](http://go.microsoft.com/?linkid=9862596). 
3. **Erstellen von Abfragen über mehrere mehrere Shards hinweg**: Erstellen von Abfragen über mehrere Shards hinweg ist in der Datei **MultiShardQuerySample.cs**dargestellt. Weitere Informationen hierzu finden Sie unter [Abfragen mit mehreren Shard](http://go.microsoft.com/?linkid=9862597).
4. **Hinzufügen von leeren mehrere Shards hinweg**: die iterative Hinzufügen von neuen, leeren mehrere Shards hinweg durch den Code in der Datei **AddNewShardsSample.cs**ausgeführt wird. Hier werden die Details dieses Themas behandelt: [Shard Karte Management](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Andere Vorgänge flexible skalieren

1. **Teilen einer vorhandenen Shard**: die Möglichkeit, mehrere Shards hinweg aufteilen erfolgt über das **Tool Teilen und Zusammenführen**. Weitere Informationen zu diesem Tool Hier finden Sie: [Teilen und Zusammenführen Tool – Übersicht](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Zusammenführen von vorhandenen mehrere Shards hinweg**: Shard zusammengeführt werden auch durchgeführt mit dem **Tool Teilen und Zusammenführen**. Weitere Informationen finden Sie in: [Teilen und Zusammenführen Tool – Übersicht](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Kosten

Flexible Datenbanktools sind kostenlos. Flexible Datenbanktools keine zusätzliche Gebühren über die Kosten für Ihre Verwendung Azure vorgangseinschränkung. 

Beispielsweise erstellt die Stichprobe Anwendung neue Datenbanken. Die Kosten hängen davon ab, die Azure SQL-DB Database Edition ausgewählt haben und die Azure Verwendung Ihrer Anwendung.

Preisinformationen finden Sie unter [SQL-Datenbank Preise Details](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den flexible Datenbanktools finden Sie unter:

* [Wegweiser für die Dokumentation flexible Datenbank-tools](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Codebeispielen: 
    -    [Flexible DB mit Azure SQL - erste Schritte](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Flexible DB mit Azure SQL - Integration mit Entität Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Shard Elastizität Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [Ankündigung flexible skalieren](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Channel 9: [flexible skalieren Übersicht Video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Diskussionsforum: [Azure SQL-Datenbank-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Messen der Leistung: [Leistungsindikatoren Shard Karte-Manager](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
