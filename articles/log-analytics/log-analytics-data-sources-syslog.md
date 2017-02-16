<properties 
   pageTitle="Syslog-Nachrichten in Log Analytics | Microsoft Azure"
   description="Syslog ist ein Ereignis Protokollierung Protokoll das gemeinsame Linux.   In diesem Artikel beschreibt das Konfigurieren der Sammlung von Beiträgen Syslog Log Analytics und Details der Datensätze, die sie im Repository OMS erstellen."
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
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Log Analytics Syslog Datenquellen

Syslog ist ein Ereignis Protokollierung Protokoll das gemeinsame Linux.  Applikationen sendet Nachrichten, die auf dem lokalen Computer gespeichert oder an eine Syslog Collection übermittelt werden kann.  Wenn der OMS-Agent für Linux installiert ist, wird den lokalen Syslog Daemon zum Weiterleiten von Nachrichten an den Agent konfiguriert.  Der Agent sendet die Nachricht dann an Log Analytics, wo ein entsprechender Datensatz im OMS Repository erstellt wird.  

> [AZURE.NOTE]Log Analytics unterstützt Sammlung von Beiträgen, die von Rsyslog oder Syslog-ng gesendet. Der standardmäßige Syslog Daemon auf Red Hat Enterprise Linux, CentOS und Oracle Linux Version (Sysklog), Version 5 wird Syslog Ereignis Websitesammlung nicht unterstützt. Zum Sammeln von Daten Syslog von dieser Version von diese Verteilung sollte der [Rsyslog Daemon](http://rsyslog.com) installiert und konfiguriert Sysklog ersetzen.

![Syslog-Websitesammlung](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Konfigurieren von Syslog
Der OMS-Agent für Linux sammelt Ereignisse mit Fertigungsanlagen und Schweregrade, die in der Konfiguration angegeben werden.  Sie können über das OMS-Portal oder durch die Verwaltung von Dateien auf Ihrer Linux-Agents Konfiguration Syslog konfigurieren.


### <a name="configure-syslog-in-the-oms-portal"></a>Konfigurieren von Syslog OMS-Portal

Konfigurieren von Syslog aus dem [Menü ' Daten ' in Log Analytics-Einstellungen](log-analytics-data-sources.md#configuring-data-sources).  Diese Konfiguration wird in die Konfigurationsdatei auf jeden Linux-Agent übermittelt.

Sie können eine neue Einrichtung hinzufügen, indem Sie deren Namen eingeben und auf **+**.  Für jede Einrichtung werden nur Nachrichten mit dem ausgewählten Schweregrade erfasst.  Überprüfen Sie die Schweregrade für die bestimmte Einrichtung, die Sie erfassen möchten.  Sie können keine weiteren Kriterien zum Filtern von Nachrichten bereitstellen.

![Konfigurieren von Syslog](media/log-analytics-data-sources-syslog/configure.png)


Standardmäßig werden alle Änderungen der Konfiguration automatisch in alle Agents abgelegt.  Wenn Sie Syslog manuell auf jeden Linux-Agent konfigurieren möchten, deaktivieren Sie das Kontrollkästchen *unterhalb der Konfiguration, die auf meinem Computer Linux übernehmen*klicken.


### <a name="configure-syslog-on-linux-agent"></a>Konfigurieren der Syslog auf Linux-agent

Wenn [OMS Agent auf einen Linux-Client installiert ist](log-analytics-linux-agents.md), wird es installiert eine standardmäßige Syslog Konfigurationsdatei, die definiert werden, die Einrichtung und Schwere der Nachrichten, die erfasst wurden.  Sie können diese Datei zum Ändern der Konfiguration ändern.  Konfigurationsdatei unterscheidet sich je nach der Syslog Daemon, den der Client installiert wurde.

> [AZURE.NOTE] Wenn Sie die Bearbeitung der Syslog-Konfiguration, müssen Sie den Daemon Syslog, damit die Änderungen wirksam werden neu starten.

#### <a name="rsyslog"></a>rsyslog

Konfigurationsdatei für Rsyslog befindet sich unter **/etc/rsyslog.d/95-omsagent.conf**.  Den Standardinhalt sind nachstehend aufgeführt.  Dies sammelt Syslog Nachrichten aus dem lokalen Agent für alle Orten mit einer Ebene der Warnung oder höher.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Sie können eine Einrichtung entfernen, indem Sie dem Abschnitt der Konfigurationsdatei entfernen.  Sie können die Schweregrade einschränken, die für eine bestimmte Funktion erfasst werden, indem Sie der Einrichtung Eintrag ändern.  Zum Einschränken der Benutzer auf Nachrichten mit einem schwere eines Fehlers oder höher irgendeine ändern Sie beispielsweise die Zeile der Konfigurationsdatei wie folgt:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng

Konfigurationsdatei für Rsyslog ist die Position, an der **/etc/syslog-ng/syslog-ng.conf**.  Den Standardinhalt sind nachstehend aufgeführt.  Dies sammelt Syslog-Nachrichten aus dem lokalen Agent für alle Räume und alle Schweregrade gesendet.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Sie können eine Einrichtung entfernen, indem Sie dem Abschnitt der Konfigurationsdatei entfernen.  Sie können die Schweregrade einschränken, die für eine bestimmte Funktion erfasst werden, indem Sie sie aus der Liste entfernen.  Um die Einrichtung Benutzer nur benachrichtigen und wichtigen Nachrichten zu beschränken ändern Sie beispielsweise diesem Abschnitt der Konfigurationsdatei wie folgt:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Ändern den Port Syslog

Der OMS-Agent überwacht Syslog-Nachrichten auf dem lokalen Client am Port 25224.  Sie können diesen Anschluss ändern, indem Sie im folgenden Abschnitt **/etc/opt/microsoft/omsagent/conf/omsagent.conf**am OMS Agent-Konfigurationsdatei hinzufügen.  Ersetzen Sie im **Anschluss** Eintrag 25224 durch die gewünschte Zahl für den Port.  Beachten Sie, dass Sie auch die Konfigurationsdatei für den Syslog Daemon zum Senden von Nachrichten an diesen Port ändern müssen.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Datensammlung

Der OMS-Agent überwacht Syslog-Nachrichten auf dem lokalen Client am Port 25224. Konfigurationsdatei für den Syslog Daemon weiterleitet Syslog gesendete Nachrichten von Anwendung zu diesem Port, in dem sie nach Log Analytics erfasst wurden.


## <a name="syslog-record-properties"></a>Syslog Datensatzeigenschaften

Syslog Datensätze verfügen über einen Typ von **Syslog** und verfügen über die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer | Computer, auf dem das Ereignis von erfasst wurden. |
| Einrichtung | Definiert den Teil des Systems, die die Nachricht generiert. |
| HostIP | IP-Adresse des Systems, die die Nachricht gesendet wird.  |
| HostName | Name des Systems, die die Nachricht gesendet wird. |
| SeverityLevel | Schwere Ebene des Ereignisses. |
| SyslogMessage | Text der Nachricht. |
| Prozess-ID | ID des Prozesses, die die Nachricht generiert. |
| EventTime | Datum und Uhrzeit, die das Ereignis generiert wurde.



## <a name="log-queries-with-syslog-records"></a>Abfragen mit Datensätzen Syslog protokollieren

Die folgende Tabelle enthält verschiedene Beispiele für die Log-Abfragen, in denen Syslog Datensätze abgerufen werden.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = Syslog | Alle Syslogs. |
| Typ = Syslog SeverityLevel = zurück | Alle Syslog Datensätze mit Schwere der Fehler. |
| Typ = Syslog & #124; Messen count() vom Computer | Anzahl der Syslog Datensätze vom Computer. |
| Typ = Syslog & #124; Messen count() durch Einrichtung | Anzahl der Syslog von Datensätzen nach Einrichtung. |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Log Suchbegriffe](log-analytics-log-searches.md) , zum Analysieren der Daten von Datenquellen und Lösungen erfasst. 
- Verwenden Sie [Benutzerdefinierte Felder](log-analytics-custom-fields.md) zum Analysieren von Daten aus Datensätzen Syslog in einzelne Felder aus.
- [Konfigurieren von Linux-Agents](log-analytics-linux-agents.md) zu anderen Arten von Daten zu sammeln. 
