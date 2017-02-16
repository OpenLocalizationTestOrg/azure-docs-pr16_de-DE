<properties
   pageTitle="Visualisieren von SQL Data Warehouse Daten mit Power BI Microsoft Azure"
   description="Visualisieren von SQL Data Warehouse Daten mit Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Visualisieren von Daten mit Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Learning Azure-Computern](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Power BI eine Verbindung mit SQL Data Warehouse und einige einfache Visualisierungen erstellen.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen Sie folgende Aktionen ausführen:

- Ein SQL Data Warehouse vorab mit der Datenbank AdventureWorksDW geladen. Um dies bereitstellen, finden Sie unter [Erstellen einer SQL-Data Warehouse][] , und wählen Sie die Beispieldaten zu laden. Wenn Sie bereits ein Datawarehouse haben, aber keinen Beispieldaten, können Sie die [Beispieldaten manuell laden][].


## <a name="1-connect-to-your-database"></a>1. Herstellen einer Verbindung mit Ihrer Datenbank

So öffnen Sie Power BI und Verbinden mit Ihrer Datenbank AdventureWorksDW:

1. Melden Sie sich bei der [Azure-Portal][]werden soll.
2. Klicken Sie auf **SQL-Datenbanken** , und wählen Sie die Datenbank AdventureWorks SQL Data Warehouse aus.

    ![Suchen Sie Ihre Datenbank][1]

3. Klicken Sie auf die Schaltfläche 'In Power BI öffnen'.

    ![Power BI-Schaltfläche][2]

4. Die Seite SQL Data Warehouse Verbindung anzeigen die Webadresse Ihrer Datenbank sollte jetzt angezeigt werden. Klicken Sie auf Weiter.

    ![Power BI-Verbindung][3]

6. Geben Sie Ihren Benutzernamen ein SQL Azure-Server, und das Kennwort ein, und Sie werden vollständig zu Ihrer Datenbank SQL Data Warehouse verbunden sein.

    ![Power BI anmelden][4]

7. Nachdem Sie Power BI angemeldet haben, klicken Sie auf das Dataset AdventureWorksDW auf der linken Blade. Dadurch wird die Datenbank geöffnet.

    ![Power BI öffnen AdventureWorksDW][5]



## <a name="2-create-a-report"></a>2. Erstellen eines Berichts

Sie können nun Power BI verwenden, um Ihre AdventureWorksDW Beispieldaten zu analysieren. Um die Analyse ausführen zu können, hat AdventureWorksDW eine Ansicht mit der Bezeichnung AggregateSales. Diese Ansicht enthält einige der wichtigen Kriterien für die Analyse die Umsätze des Unternehmens.

1. Um eine Karte Umsatzbetrag nach Postleitzahl, klicken Sie im rechten Felder zu erstellen, klicken Sie auf die Ansicht AggregateSales, um ihn zu erweitern. Klicken Sie auf die Spalten Postleitzahl und "Umsatzvolumen", um diese auszuwählen.

    ![Wählen Sie Power BI aus AggregateSales][6]

    Power BI erkennt automatisch Dies ist die geografischen Daten, und setzen es in ein Schema für Sie.

    ![Power BI-Karte][7]

2. Dieser Schritt erstellt ein Balkendiagramm, den Umfang der Umsätze pro Kunden Einkommen angezeigt wird. Wechseln Sie zu der erweiterten AggregateSales Ansicht diesem zu erstellen. Klicken Sie auf das Feld SalesAmount. Ziehen Sie das Feld Kunden Einkommen nach links, und legen Sie es an der Achse.

    ![Power BI Achse][8]

    Wir haben im Balkendiagramm über die links verschoben.

    ![Power BI-Leiste][9]

3. Dieser Schritt erstellt ein Liniendiagramm mit Umsatzbetrag pro Bestelldatum. Wechseln Sie zu der erweiterten AggregateSales Ansicht diesem zu erstellen. Klicken Sie auf SalesAmount und OrderDate. In Visualisierungen Spalte klicken Sie auf das Symbol Liniendiagramm. Dies ist das erste Symbol in der zweiten Zeile unter Visualisierungen.

    ![Power BI select-Liniendiagramm][10]

    Sie verfügen nun über einen Bericht, der drei andere Visualisierungen Daten zeigt.

    ![Power BI-Linie][11]

Sie können den Fortschritt zu einem beliebigen Zeitpunkt speichern, indem Sie auf **Datei** und **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da wir einige Zeit zum Aufwärmen mit den Beispieldaten angegeben haben, finden Sie unter So [entwickeln][], [Laden][]oder [Migrieren][]. Oder schauen Sie sich die [Power BI-Website][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[Migrieren von]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Beim Laden]: sql-data-warehouse-overview-load.md
[Laden Sie die Beispieldaten manuell]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Erstellen einer SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure-portal]: https://portal.azure.com/
[Power BI-website]: http://www.powerbi.com/
