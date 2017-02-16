<properties
   pageTitle="Sicherheitscenter Azure Data Security | Microsoft Azure"
   description="Dieses Dokument wird erläutert, wie Daten verwaltet und im Sicherheitscenter Azure gewahrt werden."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Sicherheit Sicherheitscenter Azure-Daten
Damit Kunden zu verhindern, erkennen und Beantworten von Risiken, Azure-Sicherheitscenter sammelt und verarbeitet Daten zu Ihren Azure Ressourcen, einschließlich Informationen zur Konfiguration, Metadaten, Ereignisprotokollen, stürzt ab, Sicherungsdateien und vieles mehr. Wir stellen signifikante Zusagen, um den Datenschutz und Sicherheit dieser Daten zu schützen. Microsoft befolgt strict Compliance- und Richtlinien – von der Codierung zum Ausführen eines Dienstes. 

In diesem Artikel wird erläutert, wie Daten verwaltet und im Sicherheitscenter Azure gewahrt werden.

## <a name="data-sources"></a>Datenquellen

Azure-Sicherheitscenter Analyse der Daten aus den folgenden Quellen:

- Azure Services: Liest die Informationen zur Konfiguration von Azure Services, dass Sie durch die Kommunikation mit dieser Ressource der Dienstanbieter bereitgestellt haben.
- Netzwerkverkehr: Liest Stichprobe Netzwerk-Datenverkehr Metadaten aus Microsoft Infrastruktur, wie z. B. Quelle/Ziel IP/Port, Paketgröße und Netzwerk-Protokoll.
- Partnerlösungen: Sammelt von Sicherheitshinweisen von integrierten partnerlösungen, wie z. B. Firewalls und Modul Lösungen. Diese Daten ist im Sicherheitscenter Azure-Speicher, aktuell befindet, in den Vereinigten Staaten gespeichert.
- Ihren virtuellen Computern: Azure Sicherheitscenter können sammeln Konfiguration und Weitere Informationen zu Sicherheitsereignissen, z. B. Windows-Ereignis und überwachenden Protokolle, IIS-Protokolle, Syslog-Nachrichten und Absturzabbilddateien aus Ihrer virtuellen Computern mit Daten Websitesammlungs-Agents. Finden Sie im Abschnitt "Verwalten von Datensammlung" unter Weitere Details aus.  

Darüber hinaus werden Informationen zu Sicherheitswarnungen, Empfehlungen und Sicherheit Integritätsstatus im Sicherheitscenter Azure-Speicher, die aktuell befindet, in den Vereinigten Staaten gespeichert. Diese Informationen gehören die verwandte Konfigurationsinformationen und Sicherheitsereignisse zusammengestellten aus Ihrer virtuellen Computern zum Beschleunigen der sicherheitswarnung, Empfehlungen oder Sicherheit Integritätsstatus nach Bedarf.

## <a name="data-protection"></a>Datenschutz

**Trennung der Daten**: Daten werden auf jede Komponente in der gesamten Dienst logisch getrennt gespeichert. Alle Daten pro Organisation gekennzeichnet ist. Kategorisieren von diesem im gesamten Datenlebenszyklus weiterhin besteht, und jeder Ebene des Diensts wird erzwungen. Darüber hinaus werden auf Ihre virtuellen Computer erfassten Daten in Ihr Konto Speicher gespeichert.

**Zugreifen auf Daten**: um Vorschläge, wie Sicherheit und Sicherheitsrisiken untersuchen, möglicherweise Microsoft Personal Informationen gesammelt oder analysiert von Azure-Diensten, einschließlich Sicherungsdateien Absturz zugreifen. Absturz Sicherungsdateien und Ereignisse beim Erstellen von Prozess möglicherweise unbeabsichtigt Kundendaten oder persönliche Daten aus Ihrem virtuellen Computern sind. Halten wir uns auf die [Microsoft Online Services-Ausdrücke](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) und [Datenschutzbestimmungen](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), welche Zustand, dass Microsoft nicht Kundendaten verwenden oder Informationen daraus zu kommerziellen Zwecken Werbung oder ähnlichen abgeleitet wird. Wir verwenden nur Kundendaten nach Bedarf um zu beschleunigen Azure Dienste einschließlich Zwecke mit dieser Dienste bereitstellen kompatibel. Behalten Sie alle Rechte auf Kundendaten aus.

**Verwenden von Daten**: Microsoft verwendet zur Verbesserung unserer Funktionen Prevention und Erkennung; Muster und Bedrohungsanalyse über mehrere Mandanten angezeigt Wir tun Sie dies gemäß den Datenschutz Zusagen in unseren [Datenschutzbestimmungen](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)beschrieben.

**Speicherort der Daten**: Speicher-Konto wird angegeben, für die einzelnen Regionen angezeigt, in dem virtuelle Computer ausgeführt werden. Dadurch können Sie zum Speichern von Daten in der gleichen Region als des virtuellen Computers, aus der die Daten erfasst werden. Diese Daten, einschließlich Absturzabbilddateien, werden dauerhaft in Ihr Speicherkonto gespeichert werden. Der Dienst speichert auch Informationen von Sicherheitshinweisen, einschließlich Benachrichtigungen von integrierten partnerlösungen, Empfehlungen und Sicherheit Integritätsstatus im Sicherheitscenter Azure-Speicher, aktuell befindet, in den USA.

## <a name="managing-data-collection-from-virtual-machines"></a>Verwalten von Datensammlung aus virtuellen Computern

Wenn Sie auf Sicherheitscenter Azure aktivieren auswählen, ist Datensammlung für jede Ihrer Abonnements eingeschaltet. Sie können Ihre Azure Security Center Dashboard im Abschnitt "Sicherheitsrichtlinie" Datensammlung deaktivieren. Wenn Datensammlung aktiviert ist, unterstützt Azure-Sicherheitscenter Vorschriften Überwachung-Agent auf alle vorhandenen Azure-virtuellen Computern und keine neuen Datensätze, die erstellt werden. Die Erweiterung Azure Sicherheit Überwachung scannt für Sicherheit auf verschiedenen Konfigurationen und Ereignisse verfolgt es bei [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Darüber hinaus löst das Betriebssystem Ereignisprotokollereignisse im Verlauf des Computers ausgeführt. Beispiele für solche Daten sind: das Betriebssystem von Typ und Version, betrieben Systemprotokolle (Windows-Ereignisprotokollen), Prozesse, Computernamen, IP-Adressen ausgeführt Benutzer- und Mandanten-ID angemeldet Die Überwachung Azure-Agent liest Einträge im Ereignisprotokoll und ETW verfolgt und kopiert sie in Ihr Speicherkonto für die Analyse. 

Für die einzelnen Regionen virtuellen Computern ausgeführt werden, wobei Daten gesammelt von virtuellen Computern in die gleiche Region gespeichert ist Ihnen ein Speicherkonto angegeben. Dies erleichtert das für Sie Daten in der gleichen geografischen Bereich für Datenschutz und Daten Hoheit Zwecke beibehalten werden soll. Sie können Ihre Azure Security Center Dashboard im Abschnitt "Sicherheitsrichtlinie" Speicherkonten für die einzelnen Regionen konfigurieren.

Die Überwachung Azure-Agent kopiert Absturzabbilddateien auch bei Ihrem Speicherkonto.  Azure-Sicherheitscenter temporäre Kopien Ihrer Absturzabbilddateien sammelt und analysiert, nach Hinweisen auf Versuche Angriffen und erfolgreichen Angriffen.  Azure-Sicherheitscenter führt diese Analyse innerhalb derselben geografische Region als Speicherkonto aus, und löscht die temporären Kopien, nach Abschluss der Analyse.

Sie können Datensammlung aus virtuellen Computern jederzeit deaktivieren, die alle von Azure-Sicherheitscenter zuvor installierten Überwachung-Agents entfernt werden.


## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie wie Daten verwaltet und im Sicherheitscenter Azure gewahrt werden. Wenn Sie weitere Informationen zur Azure-Sicherheitscenter finden Sie unter:

- [Planen der Azure Sicherheit Center und Operations Guide](security-center-planning-and-operations-guide.md) – Informationen zum Planen und grundlegende Informationen zu den Entwurfsaspekte Azure-Sicherheitscenter Sichtspalten übernehmen.
- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen
- [Überwachen von partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Suchen nach Blogbeiträge zu Azure Sicherheit und Kompatibilität
