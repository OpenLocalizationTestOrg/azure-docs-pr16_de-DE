<properties
   pageTitle="Problembehandlung von Microsoft Power BI eingebettete Vorschau"
   description="Problembehandlung von Microsoft Power BI eingebettete Vorschau"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Problembehandlung von Microsoft Power BI eingebettete Vorschau
Dieser Artikel bietet Antworten für **Power BI eingebettete**Behandeln von Problemen mit.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Festlegen von Zeichenfolgen für SQL Server-Verbindung
Wenn eine SQL Server herstellen einer Verbindung Zeichenfolge festlegen möchten, müssen Sie folgen ein bestimmtes Format. Es folgt eine Beispiel Verbindungszeichenfolge für SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Weitere Informationen zum SQL Server-Verbindungszeichenfolgen finden Sie unter den folgenden Artikeln:

-   [SQL Server-Verbindungszeichenfolgen](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Festlegen von Anmeldeinformationen
In den Fall, in dem Sie Anmeldeinformationen für eine Entwicklung oder staging-Umgebung, wie z. B. Benutzername und Kennwort verfügen, müssen Sie Anmeldeinformationen aktualisieren, die eine Lösung für die Herstellung entsprechen.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit der Stichprobe](power-bi-embedded-get-started-sample.md)
- [Was ist Power BI eingebettete](power-bi-embedded-what-is-power-bi-embedded.md)
