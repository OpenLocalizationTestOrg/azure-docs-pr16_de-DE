<properties
   pageTitle="IIS-Protokolle in die Protokolldateien Analytics | Microsoft Azure"
   description="Internet Information Services (IIS) speichert Benutzeraktivitäten in Protokolldateien, die vom Protokoll Analytics erfasst werden können.  In diesem Artikel beschreibt das Konfigurieren der Sammlung von IIS-Protokolle und Details der Datensätze, die sie im Repository OMS erstellen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="iis-logs-in-log-analytics"></a>IIS-Protokolle in die Protokolldateien Analytics
Internet Information Services (IIS) speichert Benutzeraktivitäten in Protokolldateien, die vom Protokoll Analytics erfasst werden können.  

![IIS-Protokolle](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurieren von IIS ' Protokolle '
Log Analytics erfasst Einträge aus Protokolldateien IIS, erstellt, sodass Sie [IIS für die Protokollierung konfigurieren müssen](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics nur unterstützt IIS-Protokolldateien im W3C-Format gespeichert und benutzerdefinierte Felder oder IIS erweiterte Protokollierung nicht unterstützt.  
Log Analytics sammelt keine Protokolle im systemeigenen NCSA oder IIS-Format.

Konfigurieren von IIS-Protokollen in Log Analytics aus dem [Menü ' Daten ' in Log Analytics-Einstellungen](log-analytics-data-sources.md#configuring-data-sources).  Es ist keine Konfiguration erforderlich nur **Sammeln W3C Format IIS-Protokolldateien**auswählen.

Es empfiehlt sich, wenn Sie IIS Log Websitesammlung aktivieren, sollten Sie die IIS Log Rollover Einstellung auf jedem Server konfigurieren.


## <a name="data-collection"></a>Datensammlung

Log Analytics sammelt IIS-Protokolleinträge aus jeder Quelle verbundenen etwa 15 Minuten.  Der Agent Einträge seine Position in jedes Ereignisprotokoll, das sie von erfasst.  Wenn der Agent in den Offlinemodus wechselt, sammelt dann Log Analytics Ereignisse aus, in dem letzten Unterbrechung, auch wenn diese Ereignisse erstellt wurden, während der Agent offline war.


## <a name="iis-log-record-properties"></a>IIS-Protokoll Datensatzeigenschaften

IIS-Protokolldatensätze haben einen Typ von **W3CIISLog** und verfügen über die Eigenschaften in der folgenden Tabelle:

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer | Name des Computers, der das Ereignis von erfasst wurden. |
| Überweisung | IP-Adresse des Clients. |
| csMethod | Methode der Anforderung beispielsweise GET oder POST. |
| csReferer | Website, dass der Benutzer eine Verknüpfung von der aktuellen Website folgen. |
| csUserAgent | Geben Sie ein Browser des Clients. |
| csUserName | Name des den authentifizierten Benutzer, den Zugriff auf den Server. Anonyme Benutzer werden durch ein Bindestrich angezeigt. |
| csUriStem | Ziel der Anfrage wie eine Webseite anzuzeigen. |
| csUriQuery | Abfragen Sie, denen Sie, dass der Client auszuführen versucht wurde. |
| "Verwaltungsgruppenname" | Name der Management Group für Operations Manager-Agents.  Für andere Agents, dies ist AOI -\<Arbeitsbereich-ID\> |
| RemoteIPCountry | Land der IP-Adresse des Clients. |
| RemoteIPLatitude | Breite der Client-IP-Adresse. |
| RemoteIPLongitude | Länge der Client-IP-Adresse. |
| scStatus | HTTP-Statuscode. |
| scSubStatus | Untergeordneter Fehlercode. |
| scWin32Status | Status-Code für Windows. |
| sIP | IP-Adresse des Web-Servers. |
| SourceSystem  | OpsMgr |
| Sportart | Port auf dem Server zu verbundene Client. |
| sSiteName | Name der IIS-Website. |
| TimeGenerated | Datum und Uhrzeit, die der Eintrag protokolliert wurde. |
| TimeTaken | Zeitdauer zum Verarbeiten der Anforderung in Millisekunden. |

## <a name="log-searches-with-iis-logs"></a>Log-Suchvorgänge mit IIS-Protokolle

Die folgende Tabelle enthält verschiedene Beispiele für die Log-Abfragen, die Protokolldatensätze IIS abrufen.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = IISLog | Alle IIS-Protokolleinträge. |
| Typ = IISLog EventLevelName = zurück | Alle Ereignisse im Windows mit Schwere der Fehler. |
| Typ = W3CIISLog & #124; Überweisung count() messen | Zählen von IIS Protokolleinträge nach IP-Adresse an. |
| Typ = W3CIISLog CsHost = "www.contoso.com" & #124; Messen count() csUriStem | Zählen von IIS Protokolleinträge anhand der URL für den Host www.contoso.com. |
| Typ = W3CIISLog & #124; Messen Sie Sum(csBytes) vom Computer & #124; Top 500000| Gesamtzahl der Bytes von jeder IIS-Computer empfangen. |

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren von Log Analytics anderen [Datenquellen](log-analytics-data-sources.md) für die Analyse sammeln.
- Erfahren Sie mehr über [Log Suchbegriffe](log-analytics-log-searches.md) , zum Analysieren der Daten von Datenquellen und Lösungen erfasst.
- Konfigurieren von Benachrichtigungen in Log Analytics vorausschauende benachrichtigt Sie wichtige Bedingungen in IIS-Protokolle gefunden.
