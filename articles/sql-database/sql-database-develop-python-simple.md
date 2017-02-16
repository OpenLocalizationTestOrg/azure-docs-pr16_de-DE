<properties
    pageTitle="Verbinden mit SQL-Datenbank mithilfe von Python | Microsoft Azure"
    description="Zeigt ein Beispiel der Python-Code, die Sie verwenden können, um zu Azure SQL-Datenbank herstellen."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Verbinden Sie mit SQL-Datenbank mithilfe von Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


In diesem Thema wird gezeigt, wie eine Verbindung herstellen und Abfragen einer Azure SQL-Datenbank mithilfe von Python wird. Sie können in diesem Beispiel aus Windows, Mac oder Ubuntu Linux Plattformen ausführen.


## <a name="step-1-create-a-sql-database"></a>Schritt 1: Erstellen einer SQL-Datenbank

Finden Sie auf der [Seite Erste Schritte](sql-database-get-started.md) zum Erstellen einer Beispieldatenbank finden.  Es ist wichtig, dass Sie den Leitfaden zum Erstellen einer **AdventureWorks-Datenbankvorlage**folgen. Die nachfolgend aufgeführten Beispiele funktionieren nur mit dem **Schema für AdventureWorks**. Nachdem Sie Ihre Datenbank stellen Sie sicher, dass erstellen aktivieren Sie den Zugriff auf Ihre IP-Adresse durch die Firewallregeln aktivieren, in dem Fenster [' Erste Schritte '](sql-database-get-started.md) beschriebenen

## <a name="step-2-configure-development-environment"></a>Schritt 2: Konfigurieren von Entwicklungsumgebung

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Installieren Sie die erforderlichen Module
Öffnen Sie das Terminal und installieren

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Öffnen Sie das Terminal, und navigieren Sie zu ein Verzeichnis, auf Ihrer Python Skript erstellt werden soll. Geben Sie die folgenden Befehle, **FreeTDS** und **Pymssql**zu installieren. Pymssql verwendet FreeTDS, um die Verbindung mit SQL-Datenbanken.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Installieren Sie Pymssql von [**hier**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql)aus. 

Stellen Sie sicher, dass Sie die richtige Whl Datei auswählen. Beispiel: Wählen Sie aus, wenn Sie Python 2.7 auf einem 64-Bit-Computer verwenden: Pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Nachdem Sie herunterladen die Datei .whl platzieren Sie es in den Ordner C:/Python27.

Jetzt installieren des Pymssql-Treibers Pip über die Befehlszeile verwenden. CD in C:/Python27 und Ausführen der folgenden
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Anweisungen zum Aktivieren der Verwendung Pip finden Sie [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Schritt 3: Ausführen Stichprobe code

Erstellen Sie eine Datei mit der Bezeichnung **sql_sample.py** , und fügen Sie den folgenden Code darin enthaltenen. Sie können dies über die Befehlszeile mit ausführen:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Verbinden Sie mit der SQL-Datenbank

Die Funktion [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) wird verwendet, die Verbindung zu einer SQL-Datenbank herstellen.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Führen Sie eine SQL-SELECT-Anweisung

Die Funktion [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) kann verwendet werden, zum Abrufen von ein Resultset aus einer Abfrage in SQL-Datenbank. Diese Funktion akzeptiert im Wesentlichen eine Abfrage, und gibt, die ein Resultset können mit der Verwendung von [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone)durchlaufen.


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Einfügen einer Zeile, Parameter übergeben und Abrufen des generierten Primärschlüssels

In SQL-Datenbank können die Eigenschaft [Identität](https://msdn.microsoft.com/library/ms186775.aspx) und das Objekt [SEQUENZ](https://msdn.microsoft.com/library/ff878058.aspx) verwendet werden, automatisch [Primärschlüssel](https://msdn.microsoft.com/library/ms179610.aspx) Werte generieren. 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transaktionen.


In diesem Codebeispiel veranschaulicht die Verwendung der Transaktionen, in dem Sie:

* Starten einer Transaktions
* Einfügen einer Zeile mit Daten
* Zurücksetzen Ihrer Transaktion einfügen rückgängig machen 

Fügen Sie den folgenden Code in sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der [SQL-Datenbank Entwicklung (Übersicht)](sql-database-develop-overview.md)
* Weitere Informationen zu den [Microsoft Python-Treiber für SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Besuchen Sie die [Python Developer Center](/develop/python/).

## <a name="additional-resources"></a>Zusätzliche Ressourcen 

* [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Untersuchen Sie die [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)
