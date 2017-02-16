<properties
   pageTitle="Verwenden von Power BI mit SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Power BI mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Verwenden von Power BI mit SQL Datawarehouse
Als ermöglicht mit SQL Azure-Datenbank, SQL Data Warehouse direkte verbinden Benutzer nutzen Sie leistungsfähige logische Pushdown entlang der Analysefunktionen von Power BI.  Mit direkten verbinden werden Abfragen wieder in SQL Azure Data Warehouse in Echtzeit gesendet, wie Sie die Daten zu analysieren.  Dadurch wird in Kombination mit der Skalierung des SQL Data Warehouse ermöglicht Benutzern das dynamische Berichte in Minuten gegen TB Daten erstellen.  Darüber hinaus kann die Einführung von den in Power BI-Schaltfläche Öffnen Benutzer Verbindung direkt Power BI zu ihrer SQL Data Warehouse ohne Sammeln von Informationen aus anderen Teile Azure.

Bei Verwendung von Notiz Bitte direkte verbinden:

+ Geben Sie den vollständigen Servernamen beim Herstellen einer Verbindung (Details finden Sie unter Weitere)
+ Sicherstellen Sie, dass die Firewall-Regeln für die Datenbank zur "Zulassen des Zugriffs auf Azure Services" konfiguriert sind.
+ Jeder Aktion aus, wie Sie eine Spalte auswählen oder Hinzufügen eines Filters wird das Datawarehouse direkt Abfragen.
+ Kacheln werden aktualisiert etwa 15 Minuten (Aktualisierung muss nicht geplant werden)
+ F & A ist nicht verfügbar für Datasets direkte verbinden
+ Schemänderungen werden nicht automatisch übernommen
+ Alle direkten Verbinden Abfragen werden Reaktion nach 2 Minuten

Diese Einschränkungen und Notizen können geändert werden, wie wir weiter Benutzeroberflächen zu verbessern. Die Schritte zum Verbinden werden nachfolgend detailliert beschrieben.  

## <a name="using-the-open-in-power-bi-button"></a>Verwenden der Schaltfläche 'In Power BI öffnen'
Die einfachste Möglichkeit zum Navigieren zwischen Ihrem SQL Data Warehouse und Power BI ist die im Power BI-Schaltfläche geöffnet. Diese Schaltfläche können Sie nahtlos mit dem Erstellen von neuen Dashboards in Power BI beginnen.  

1.  Um anzufangen navigieren Sie zu Ihrem SQL Data Warehouse Instanz im klassischen Azure-Portal.
2.  Klicken Sie auf die Schaltfläche 'In Power BI öffnen'.
3.  Wenn wir nicht direkt anmelden können, oder wenn Sie nicht über eine Power BI-Konto verfügen, müssen Sie sich anmelden.  
4.  Sie werden auf der Seite SQL Data Warehouse Verbindung mit den Informationen aus Ihrem SQL Data Warehouse vorab weitergeleitet werden.
5.  Nach der Eingabe Ihrer Anmeldeinformationen werden Sie vollständig in Ihre SQL Data Warehouse verbunden sein.

## <a name="connecting-through-the-power-bi-portal"></a>Herstellen einer Verbindung über das Power BI-portal
Zusätzlich zu verwenden, die im Power BI-Schaltfläche geöffnet ist, können Benutzer auch auf ihren SQL Data Warehouse über das Power BI-Portal verbinden.

1.  Klicken auf 'Daten abrufen' am unteren Rand des Navigationsbereichs.
2.  Wählen Sie 'Datenbanken' aus.
3.  Wählen Sie auf der Seite Datenbanken einmal 'Azure SQL-Data Warehouse', und klicken Sie dann auf 'Verbinden'.
4.  Geben Sie die erforderlichen Verbindungsinformationen.  Im Portal Azure können Ihren Servernamen und den Datenbanknamen gefunden werden.
5.  Sie wieder zu der Hauptseite von Power BI geleitet und nach dem herstellen die Verbindung einen neuen Eintrag unter 'Datasets' werden durch den Namen Ihrer Instanz angezeigt.  
6.   Klicken Sie auf das neue Dataset zum Durchsuchen aller Tabellen und Sichten in der Datenbank. Auswählen einer Spalteninhalts wird eine Abfrage zurück an die Quelle dynamisch erstellen Ihrer Visual senden. Diese visuellen Elemente können in einer neuen Bericht gespeichert und wieder zum Dashboard angehefteten werden.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
