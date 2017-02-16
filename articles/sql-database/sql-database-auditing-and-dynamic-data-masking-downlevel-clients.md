<properties
    pageTitle="SQL-Datenbank kompatible Clients unterstützen und -IP-Endpunkt ändert Überwachung | Microsoft Azure"
    description="Erfahren Sie mehr über die SQL-Datenbank zur Unterstützung für kompatible Clients und IP-Endpunkt Änderungen für die Überwachung."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>SQL-Datenbank - Clients älterer Versionen unterstützen und -IP-Endpunkt Änderungen für die Überwachung


[Überwachung](sql-database-auditing-get-started.md) funktioniert automatisch mit SQL-Clients, die TDS Umleitung unterstützen.


##<a name="a-idsubheading-1adownlevel-clients-support"></a><a id="subheading-1"></a>Unterstützung von Clients älterer Versionen

Jeder Client die TDS 7.4 implementiert, sollten auch Umleitung unterstützen. Ausnahmen hierzu gehören JDBC 4.0, in dem das Feature für die Umleitung nicht vollständig unterstützt und lästige für Node.JS in die Umleitung wurde nicht implementiert.

Für "Kompatible Clients" sollten d. h. welche Support TDS Version 7.3 und unter - Server FQDN in der Verbindung Zeichenfolge geändert werden:

Ursprüngliche Server FQDN in der Verbindungszeichenfolge: <*Servername*>. database.windows.net

Geänderte Server FQDN in der Verbindungszeichenfolge: <*Servername*> .database. **sichere**. Windows

Eine Teilliste der "Clients älterer Versionen" umfasst:

- .NET 4.0 und früher,
- ODBC-10.0 und unten.
- JDBC (zwar JDBC TDS 7.4 unterstützt, das TDS Umleitung Feature wird nicht vollständig unterstützt)
- Langwierig (für Node.JS)

**Hinweis:** Der oben aufgeführten Server FDQN Änderung sein auch für die Anwendung einer Richtlinie Überwachung von SQL Server auf ohne erneute für einen Konfigurationsschritt in jeder Datenbank (temporäre Reduzierung) hilfreich.

##<a name="a-idsubheading-2aip-endpoint-changes-when-enabling-auditing"></a><a id="subheading-2"></a>IP-Endpunkt Änderungen bei der Aktivierung der Überwachung

Bitte beachten Sie ändert, wenn Sie die Überwachung aktivieren, der IP-Endpunkt der Datenbank. Wenn Sie nur Firewalleinstellungen haben, aktualisieren Sie die Firewalleinstellungen entsprechend.

Der neue Datenbank-IP-Endpunkt wird der Bereich in der Datenbank abhängig sind:

| Bereich in der Datenbank | Mögliche IP-Endpunkte |
|----------|---------------|
| Nord-China  | 139.217.29.176, 139.217.28.254 |
| China OST  | 42.159.245.65, 42.159.246.245 |
| Australien OST  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australien oder | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brasilien Süd  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| USA – zentral  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Ostasien   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Ostasiatische USA 2 | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Ostasiatische US   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Zentrale Indien  | 104.211.98.219, 104.211.103.71 |
| Süd Indien   | 104.211.227.102, 104.211.225.157 |
| Westen Indien  | 104.211.161.152, 104.211.162.21 |
| Japan OST   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japan "Westen"    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Nord-zentralen US  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| North Europa  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Süd zentralen US  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Oder Asien  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Westen Europa  | 104.40.230.120 13.80.23.64 137.117.171.161 13.80.8.37 104.47.167.215 40.118.56.193 104.40.176.73, 40.118.56.20 |
| Westen US  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Kanada Central  | 13.88.248.106 |
| Kanada OST  |  40.86.227.82 |
