<properties
   pageTitle="Treiber für SQL Datawarehouse | Microsoft Azure"
   description="Verbindungszeichenfolgen und Treiber für SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="sonyama;barbkess"/>


# <a name="drivers-for-azure-sql-data-warehouse"></a>Treiber für SQL Azure Datawarehouse

Sie können mit mehreren anderen Anwendungsprotokolle wie z. B. [ADO.NET][], [ODBC-][], [PHP][] und [JDBC][]mit SQL Data Warehouse herstellen. Nachstehend sind einige Beispiele für Verbindungszeichenfolgen für jedes Protokoll ein.  Sie können auch das Azure-Portal verwenden, zum Erstellen der Verbindungszeichenfolge.  Navigieren Sie zum Erstellen der Verbindungszeichenfolge mithilfe des Azure-Portals zu Ihrer Datenbank Blade, klicken Sie unter *Essentials* auf *Datenbank Verbindungszeichenfolgen anzeigen*.

## <a name="sample-adonet-connection-string"></a>Beispiel für ADO.NET-Verbindungszeichenfolge

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a>Beispiel für ODBC-Verbindungszeichenfolge

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a>Beispiel von PHP Verbindungszeichenfolge

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a>Beispiel für JDBC Verbindungszeichenfolge

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [AZURE.NOTE] Erwägen Sie die Einstellung Connection Timeout 300 Sekunden akzeptieren, um die Verbindung zu überstehen kurze Zeiträume nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Um Ihr Datawarehouse mit Visual Studio und anderen Clientanwendungen Abfragen zu starten, finden Sie unter [Abfrage mit Visual Studio][].

<!--Image references-->

<!--Azure.com references-->
 [Abfrage mit Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
 
<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->
