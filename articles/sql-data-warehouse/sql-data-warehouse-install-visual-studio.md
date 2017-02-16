<properties
   pageTitle="Installieren Sie Visual Studio und SSDT für SQL Datawarehouse | Microsoft Azure"
   description="Installieren Sie Visual Studio und SQL Server-Entwicklung-Tools (SSDT) für SQL Azure Datawarehouse"
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
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Installieren Sie Visual Studio 2015 und SSDT für SQL Datawarehouse

Es wird empfohlen, zum Entwickeln von Applications für SQL Data Warehouse mit Visual Studio 2015 mit der neuesten Version von SQL Server Data Tools (SSDT).  Visual Studio 2013 Update 5 mit SSDT wird ebenfalls für Abwärtskompatibilität unterstützt.  

Verwenden Visual Studio mit SSDT können Sie an, mit dem SQL Server-Objekt-Explorer optisch untersuchen von Tabellen, Sichten, gespeicherten Prozeduren und viele weitere Objekte in Ihr SQL Data Warehouse sowie Ausführen von Abfragen.

> [AZURE.NOTE] SQL Data Warehouse unterstützt noch keine Visual Studio Database Projects.  Dieses Feature wird in zukünftigen Versionen hinzugefügt werden.

## <a name="step-1-install-visual-studio-2015"></a>Schritt 1: Installieren Sie Visual Studio 2015

Folgen Sie diesen Links zum Herunterladen und Installieren von Visual Studio 2015 aus. Wenn Sie bereits Visual Studio 2013 oder 2015 installiert haben, können Sie fahren Sie mit Schritt2 fort, SSDT zu installieren.

1. [Visual Studio 2015 herunterladen][].
2. Führen Sie die [Installation von Visual Studio][] -Leitfaden auf MSDN, und wählen Sie den standardmäßigen Konfigurationen aus.

## <a name="step-2-install-ssdt"></a>Schritt 2: Installieren Sie SSDT

Zum Installieren von SSDT für Visual Studio einfach überprüfen, ob ein Update SSDT von Visual Studio mit folgenden Schritten.

1. Klicken Sie auf **Extras**, in Visual Studio / **Extensions und Updates...**  /  **Updates**
2. Wählen Sie **Produktupdates** aus, und suchen Sie nach **Microsoft SQL Server-Update für Datenbank-Tools**

Wenn ein Update nicht gefunden wird, sollten Sie die neueste Version installiert verfügen.  Um zu bestätigen, SSDT installiert ist, klicken Sie auf **Hilfe** / **Zu Microsoft Visual Studio** und suchen Sie nach SQL Server Data Tools in der Liste.  Die neueste Version von SSDT ist 14.0.60525.0.  Wenn die Option zum Installieren nicht von Visual Studio verfügbar ist, können Sie können auch Sie auf der Hilfeseite [SSDT herunterladen][] zum Herunterladen und installieren SSDT manuell.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, dass Sie die neueste Version von SSDT haben, können Sie [für die Verbindung][] zu Ihrer SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[Verbinden]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Herunterladen von Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Installieren von Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT herunterladen]: https://msdn.microsoft.com/library/mt204009.aspx
