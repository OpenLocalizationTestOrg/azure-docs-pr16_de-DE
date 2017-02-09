<properties
   pageTitle="Azure Datenkatalog unterstützter Datenquellen | Microsoft Azure"
   description="Spezifikation der derzeit unterstützten Datenquellen."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Azure Datenkatalog unterstützte Datenquellen

Benutzer des Katalogs Azure Daten können mithilfe einer öffentlichen API, klicken Sie auf eine Metadaten veröffentlichen-einmal Registration tool oder web-Portal durch die manuelle Eingabe von Informationen direkt auf dem Datenkatalog. Im folgende Raster werden alle Quellen durch den Katalog heute sowie die Funktionen für die Veröffentlichung für jede unterstützt zusammengefasst.  Auch aufgelistet sind die externe Daten-Tools, die jeder Quelle aus unserer Erfahrung Portal "Öffnen in" starten kann. Das zweite Raster im Artikel verfügt über weitere technische Spezifikation der einzelnen Datenquellen Verbindungseigenschaften.


## <a name="list-of-supported-data-sources"></a>Liste der unterstützten Datenquellen

<table>

    <tr>
       <td><b>Quellobjekt Daten</b></td>
       <td><b>API</b></td>
       <td><b>Manuelle Eingabe</b></td>
       <td><b>Registration-Tool</b></td>
       <td><b>Öffnen In Tools</b></td>
       <td><b>Notizen</b></td>
    </tr>

    <tr>
      <td>Azure Lake Datenspeicher-Verzeichnis</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Lake Datenspeicher-Datei</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Blob Azure-Speicher</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure-Speicher-Verzeichnis</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabelle Azure-Speicher</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS-Verzeichnis</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS-Datei</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Strukturtabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Struktur anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle-Datenbank-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Anzeigen der Oracle-Datenbank</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Andere (generische Anlage)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Datawarehouse-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Datawarehouse-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Dimension</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Measure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQLServer Reporting Services-Bericht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Browser</font></td>
      <td><font size=2>Einheitlichen Modus nur Server. SharePoint-Modus wird nicht unterstützt.</font></td>
    </tr>

    <tr>
      <td>SQL Server-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP-Hana anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Berechnung und analytischen Ansichten nur; Attribut Ansichten werden nicht unterstützt.</font></td>
    </tr>

    <tr>
      <td>DB2-Tabelle</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>DB2-Ansicht</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-Verzeichnis</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Bericht</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Endpunkt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-Entität festlegen</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-Funktion</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL-Tabelle</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL-Ansicht</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP-Hana anzeigen</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Vertrieb-Objekt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SharePoint-Liste </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Wenn Sie Unterstützung für zusätzliche Quellen benötigen, fordern Sie ein Feature mit dem [Datenkatalog Azure-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Bezug der Datenquellenangabe
> [AZURE.NOTE] "DSL Struktur" Spalte in der folgenden Tabelle nur Listen Verbindungseigenschaften für die Eigenschaftensammlung "Adresse", die von Azure Datenkatalog verwendet werden (d. h. kann "Adresse" Eigenschaftensammlung andere Verbindungseigenschaften der Datenquelle dem Datenkatalog Azure weiterhin besteht, Sie aber nicht enthalten.)
<table>
    <tr>
       <td><b>Quellentyp</b></td>
       <td><b>Objekt-Datentyp</b></td>
       <td><b>Objekttypen</b></td>
       <td><b>DSL Struktur<b></td>
    </tr>
    <tr>
      <td>Azure Lake Datenspeicher</td>
      <td>Container</td>
      <td>Daten Lake</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {grundlegende, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Lake Datenspeicher</td>
      <td>Tabelle</td>
      <td>Datei-Verzeichnis</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {grundlegende, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protokoll: Azure-Blobs <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Container</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Tabelle</td>
      <td>BLOB, Verzeichnis</td>
      <td>
        <font size=2>Protokoll: Azure-Blobs <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Container <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Namen</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protokoll: Azure-Tabellen <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2>Protokoll: Azure-Tabellen <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Namen</font>
      </td>
    </tr>
    <tr>
      <td>COSMOS</td>
      <td>Container</td>
      <td>Virtuelle Cluster</td>
      <td>
        <font size=2>Protokoll: Cosmos <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>COSMOS</td>
      <td>Tabelle</td>
      <td>Stream, Stream festlegen, Ansicht</td>
      <td>
        <font size=2>Protokoll: Cosmos <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {keine, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Bericht</td>
      <td>Bericht Dashboard</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {keine, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: db2 <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: db2 <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema</font>
      </td>
    </tr>
    <tr>
      <td>Dateisystem</td>
      <td>Tabelle</td>
      <td>Datei</td>
      <td>
        <font size=2>Protokoll: Datei <br>Authentifizierung: {keine, einfache, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Pfad</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabelle</td>
      <td>Datei-Verzeichnis</td>
      <td>
        <font size=2>Protokoll: ftp <br>Authentifizierung: {keine, einfache, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {grundlegende, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tabelle</td>
      <td>Datei-Verzeichnis</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {grundlegende, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Struktur</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Struktur <br>Authentifizierung: {Hdinsight, grundlegende, Benutzernamen, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>ConnectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ServerProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Struktur</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: Struktur <br>Authentifizierung: {Hdinsight, grundlegende, Benutzernamen, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>ConnectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ServerProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {keine, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Bericht</td>
      <td>Bericht Dashboard</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {keine, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Tabelle</td>
      <td>Endpunkt, Datei</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {keine, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Mysql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: Mysql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entitätencontainer</td>
      <td>
        <font size=2>Protokoll: Odata <br>Authentifizierung: {keine, einfache, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabelle</td>
      <td>Festlegen der Entität, (Funktion)</td>
      <td>
        <font size=2>Protokoll: Odata <br>Authentifizierung: {keine, einfache, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ressource</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Oracle <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: Oracle <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Postgresql <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: Postgresql <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Website</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {keine, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Bericht</td>
      <td>Bericht Dashboard</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {keine, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tabelle</td>
      <td>Daten Mashup</td>
      <td>
        <font size=2>Protokoll: Power-Abfrage <br>Authentifizierung: {Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Vertrieb</td>
      <td>Tabelle</td>
      <td>Objekt</td>
      <td>
        <font size=2>Protokoll: com-Vertrieb <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Klasse <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Elementname</font>
      </td>
    </tr>
    <tr>
      <td>SAP-Hana</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protokoll: Sap-Hana-Sql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </td>
    </tr>
    <tr>
      <td>SAP-Hana</td>
      <td>Tabelle</td>
      <td>Ansicht</td>
      <td>
        <font size=2>Protokoll: Sap-Hana-Sql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tabelle</td>
      <td>Liste</td>
      <td>
        <font size=2>Protokoll: Sharepoint-Liste <br>Authentifizierung: {grundlegende, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>Befehl</td>
      <td>Gespeicherte Prozedur</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SqlServer</td>
      <td>Befehl</td>
      <td>Gespeicherte Prozedur</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SqlServer</td>
      <td>TableValuedFunction</td>
      <td>Tabellenwertfunktion</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SqlServer</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>SqlServer</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-multidimensionale</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-multidimensionale</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-multidimensionale</td>
      <td>Measure</td>
      <td>Measure</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-multidimensionale</td>
      <td>Tabelle</td>
      <td>Dimension</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Dimension}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarischen</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarischen</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarischen</td>
      <td>Measure</td>
      <td>Measure</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarischen</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Basic, anonym, keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Table}</font>
      </td>
    </tr>
    <tr>
      <td>SQLServer Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protokoll: reporting Services <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQLServer Reporting Services</td>
      <td>Bericht</td>
      <td>Bericht</td>
      <td>
        <font size=2>Protokoll: reporting Services <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Pfad <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Teradata <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabelle</td>
      <td>Tabelle anzeigen</td>
      <td>
        <font size=2>Protokoll: Teradata <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server-Master Data Services</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size="2">Protokoll: Mssql-Mds <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server-Master Data Services</td>
      <td>Tabelle</td>
      <td>Entität</td>
      <td>
        <font size="2">Protokoll: Mssql-Mds <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Entität</font>
      </td>
    </tr>
    <tr>
      <td>Andere (nicht eins der oben)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protokoll: generisch-Objekt <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Posten-ID</font>
      </td>
    </tr>
</table>
