<properties
   pageTitle="Vorgänge Management Suite Sicherheits- und Audit Lösung Daten | Microsoft Azure"
   description="Dieses Dokument wird erläutert, wie Daten verwaltet und Vorgänge Management Suite Sicherheit und Audit Lösung gewahrt wird."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Vorgänge Management Suite Sicherheit zu gewährleisten und Audit Lösung Daten Sicherheit

Damit Kunden zu verhindern, erkennen und Beantworten von Risiken zu verwenden, können Sie [Vorgänge Management Suite (OMS) Sicherheit und Audit-Lösung](operations-management-suite-overview.md) erfasst und verarbeitet Daten zu Ressourcen, wozu auch die:

- Sicherheitsereignisprotokoll
- Event Tracing for Windows (ETW) Ereignisse
- AppLocker ü Ereignisse
- Windows-Firewall-Protokoll
- Erweiterte Bedrohung Analytics Ereignisse
- Ergebnisse der geplanten Bewertung
- Ergebnisse der Modul Bewertung
- Ergebnisse der Update-Patch Bewertung
- Syslogs Streams, die auf dem Agent explizit aktiviert sind

Wir stellen signifikante Zusagen, um den Datenschutz und Sicherheit dieser Daten zu schützen. Microsoft befolgt strict Compliance- und Richtlinien – von der Codierung zum Ausführen eines Dienstes.
In diesem Artikel wird erläutert, wie Daten verwaltet und gewahrt in OMS Sicherheit und Audit-Lösung.

## <a name="data-sources"></a>Datenquellen

OMS Sicherheit und Audit Lösung Analysieren von Daten auf Ihrem virtuellen Computern und physischen Computern, die Stelle, an der der OMS-Agent installiert ist. OMS Sicherheit und Überwachungsrichtlinien Lösung können Konfigurationsinformationen zu Sicherheitsereignissen, wie Windows-Ereignisprotokoll, Überwachungsprotokolle, IIS-Protokolle und Syslog-Nachrichten sammeln. Beispiele für solche Daten sind: das Betriebssystem von Typ und Version, Prozesse, Computernamen, IP-Adressen ausgeführt Benutzer- und Mandanten-ID angemeldet  

## <a name="data-protection"></a>Datenschutz

**Trennung der Daten**: Daten werden auf jede Komponente in der gesamten Dienst logisch getrennt gespeichert. Alle Daten pro Organisation gekennzeichnet ist. Kategorisieren von diesem im gesamten Datenlebenszyklus weiterhin besteht, und jeder Ebene des Diensts wird erzwungen. 

**Zugreifen auf Daten**: um Vorschläge, wie Sicherheit und Sicherheitsrisiken untersuchen, Microsoft Personal Informationen gesammelt oder analysiert von Diensten zugreifen können. Halten wir uns auf die [Microsoft Online Services-Ausdrücke](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) und [Datenschutzbestimmungen](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), welche Zustand, dass Microsoft nicht Kundendaten verwenden oder Informationen daraus zu kommerziellen Zwecken Werbung oder ähnlichen abgeleitet wird. Wenn Sie Vorschläge, wie Sicherheit und ermitteln mögliche Sicherheitsrisiken, möglicherweise Microsoft Personal Informationen gesammelt oder analysiert von Diensten zugreifen. Wir verwenden nur Kundendaten nach Bedarf um zu beschleunigen Azure Dienste einschließlich Zwecke mit dieser Dienste bereitstellen kompatibel. Behalten Sie alle Rechte an Ihre eigenen Daten.

**Verwenden von Daten**: Microsoft verwendet zur Verbesserung unserer Funktionen Prevention und Erkennung; Muster und Bedrohungsanalyse über mehrere Mandanten angezeigt Wir tun Sie dies gemäß den Datenschutz Zusagen in unseren [Datenschutzbestimmungen](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)beschrieben.

> [AZURE.NOTE] Datenspeicherort ist Ebene der OMS Arbeitsbereich, während der Erstellung Arbeitsbereich Teil der erste OMS Sicherheit und Audit Konfigurationsprozess ist konfiguriert.

## <a name="see-also"></a>Siehe auch

In diesem Dokument haben gelernt wie Daten verwaltet und in OMS gewahrt werden. Weitere Informationen zum OMS Sicherheit und Audit Lösung finden Sie unter:

- [Vorgänge Management Suite (OMS) (Übersicht)](operations-management-suite-overview.md)
- [Für die Überwachung und Beantworten von Sicherheitshinweisen Vorgänge Management Suite Sicherheit und Audit-Lösung](oms-security-responding-alerts.md)
- [Überwachen von Ressourcen in Vorgänge Management Suite Sicherheit und Audit-Lösung](oms-security-monitoring-resources.md)

