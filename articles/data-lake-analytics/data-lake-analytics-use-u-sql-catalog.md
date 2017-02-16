<properties
   pageTitle="Katalog Azure Daten dem Analytics U-SQL vorstellen | Azure"
   description="Katalog Azure Daten dem Analytics U-SQL vorstellen"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Verwenden von U SQL Katalog

U-SQL-Katalogs dient zum Strukturieren von Daten und Code, damit nach U-SQL-Skripts gemeinsam genutzt werden können. Der Katalog ermöglicht die höchste Performance mit Daten in Azure Daten Lake möglich.

Jede Azure Daten dem Analytics-Konto verfügt genau ein U SQL Katalog zugeordnet. U-SQL-Katalogs kann nicht gelöscht werden. Zurzeit können U SQL Kataloge zwischen Konten Lake Datenspeicher freigegeben werden.

Jeder U-SQL-Katalog enthält eine Datenbank namens- **Master-Shape**. Der Master-Datenbank kann nicht gelöscht werden.  Jeder U-SQL-Katalog können weitere zusätzlichen Datenbanken enthalten.

U-SQL-Datenbank enthält:

- Assemblys – freigeben .NET Code zwischen U-SQL-Skripts.
- Tabelle-Werte Funktionen – freigeben U-SQL-Code zwischen U-SQL-Skripts.
- Tabellen – Freigeben von Daten zwischen U-SQL-Skripts.
- Freigeben von Schemas - Tabellenschemas zwischen U-SQL-Skripts.

## <a name="manage-catalogs"></a>Kataloge verwalten
Jedes Azure Daten dem Analytics-Konto verfügt über eine Azure Lake Datenspeicher-Standardkonto zugeordnet. Dieses Konto Lake Datenspeicher wird als Standardkonto Lake Datenspeicher bezeichnet. U SQL Katalog wird in der Lake Datenspeicher Standardkonto unter dem Ordner/Catalog gespeichert. Löschen Sie alle Dateien im Ordner/Catalog nicht.

### <a name="use-azure-portal"></a>Verwenden von Azure-portal

Finden Sie unter [Verwalten von Daten dem Analytics verwenden portal](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Verwenden Sie für Visual Studio Lake Datentools.

Daten dem Tools für Visual Studio können Sie um den Katalog zu verwalten.  Weitere Informationen zu den Tools finden Sie unter [Verwenden von Daten dem Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Verwalten des Katalogs**

1. Öffnen Sie Visual Studio, und Verbinden mit Azure. Die Anweisungen finden Sie unter [Verbinden mit Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Öffnen Sie **Server-Explorer** , indem Sie **STRG + ALT + S**drücken.
2. Aus dem **Server-Explorer**erweitern Sie **Azure**, erweitern Sie die **Daten dem Analytics**, erweitern Sie Ihre Daten dem Analytics-Konto, erweitern Sie **Datenbanken**, und dann **master**.



    - Wenn Sie eine neue Datenbank hinzufügen möchten, mit der rechten Maustaste **Datenbank**, und klicken Sie dann auf **Datenbank erstellen**.
    - Wenn Sie eine neue Assembly hinzufügen möchten, mit der rechten Maustaste **Assemblys**, und klicken Sie dann auf **Assembly registrieren**.
    - Um ein neues Schema hinzuzufügen, mit der rechten Maustaste **Schemas**aus, und klicken Sie dann auf "erstellen Schema **.
    - Wenn Sie eine neue Tabelle hinzufügen möchten, mit der rechten Maustaste **Tabellen**, und klicken Sie dann auf "" erstellen Tabelle **.
    - Zum Hinzufügen einer neuen Tabellenwertfunktion finden Sie unter [Operatoren für Daten dem Analytics Aufträge für benutzerdefinierte U-SQL entwickeln](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Visual Studio U SQL Kataloge durchsuchen](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Siehe auch

- Erste Schritte
    - [Erste Schritte mit Daten dem Analytics mit Azure-portal](data-lake-analytics-get-started-portal.md)
    - [Erste Schritte mit Daten dem Analytics mithilfe der PowerShell Azure](data-lake-analytics-get-started-powershell.md)
    - [Erste Schritte mit Daten dem Analytics mit Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [Entwickeln Sie U-SQL-Skripts mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)

- SQL-U & Entwicklung
    - [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)
    - [Verwenden Sie U-SQL-Funktionen für Azure Daten dem Analytics Aufträge](data-lake-analytics-use-window-functions.md)
    - [Entwickeln Sie U-SQL benutzerdefinierte Operatoren für Daten dem Analytics Aufträge](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Projektmanagement
    - [Verwalten von Azure Daten dem Analytics mit Azure-portal](data-lake-analytics-manage-use-portal.md)
    - [Verwalten von Azure Daten dem Analytics mithilfe der PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
    - [Überwachen Sie und Behandeln von Problemen mit Azure Daten dem Analytics-Aufträge mithilfe von Azure-portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- End-to-End-Lernprogramm
    - [Verwenden der interaktiven Azure Daten dem Analytics-Lernprogramme](data-lake-analytics-use-interactive-tutorials.md)
    - [Analysieren von Website-Protokolle mit Azure Daten dem Analytics](data-lake-analytics-analyze-weblogs.md)
