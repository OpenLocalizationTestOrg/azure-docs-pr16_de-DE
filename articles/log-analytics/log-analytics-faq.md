<properties
    pageTitle="Melden Sie sich Analytics häufig gestellte Fragen zu | Microsoft Azure"
    description="Antworten auf häufig gestellte Fragen zu den Log Analytics-Dienst."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-analytics-faq"></a>Melden Sie sich Analytics häufig gestellte Fragen

Dieser Microsoft-FAQ wird eine Liste mit häufig gestellte Fragen zu Log Analytics in Microsoft Operations Management Suite (OMS). Wenn Sie weiteren Fragen zur Log Analytics haben, wechseln Sie zu der [Diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) bitte, und stellen Sie Ihre Fragen. Eine Person aus unserer Community hilft Ihnen Ihre Antworten zu erhalten. Wenn Sie eine Frage häufig gestellt wird, werden wir es in diesem Artikel hinzugefügt werden, damit sie schnell und einfach gefunden werden kann.

## <a name="general"></a>Allgemeine

**F: Welche gesucht werden durch die Anzeige und SQL-Bewertung Lösungen?**

A Die folgende Abfrage zeigt eine Beschreibung der alle Prüfungen, die aktuell ausgeführt:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Die Ergebnisse können dann zur späteren Überprüfung nach Excel exportiert werden.

* *F: Warum sehe ich etwas anderes als *OMS* in SCOM Administration? **

A: abhängig welche Update Rollup von SCOM Sie sich befinden, wird möglicherweise einen Knoten für *System Center Advisor*, *Betrieb Einblicken*oder *Log Analytics*angezeigt.

In einem Management Pack, die manuell importiert werden muss, ist die Zeichenfolge aktualisieren *OMS* Text enthalten. Folgen Sie den Anweisungen auf dem neuesten SCOM Update Rollup KB-Artikel, und aktualisieren Sie die OMS-Verwaltungskonsole, um die neuesten Updates im *OMS* Knoten finden Sie unter.

* *F: Gibt es eine *lokalen* Version OMS? **

A: Nein. Log Analytics verarbeitet und sehr große Datenmengen gespeichert werden. Als Cloud-Dienst kann Log Analytics vergrößern bei Bedarf und Auswirkung auf die Leistung zu Ihrer Umgebung zu vermeiden.

Darüber hinaus wird eine Cloud-Dienst bedeutet, Sie nicht die Protokoll Analytics Infrastruktur aufrechterhalten müssen, ausgeführt und häufig verwendeten Features Updates und Updates empfangen können.

## <a name="configuration"></a>Konfiguration
**F: ändern kann ich den Namen der Tabelle/Blob-Container zum Lesen von Azure-Diagnose (WAD)?**  

A  Nein, dies ist derzeit nicht möglich, aber für eine zukünftige Version geplant ist.

**F: Was IP-Adressen führen Sie das OMS-Diensten verwenden? Wie sicherstellen kann ich, dass meine Firewall nur den Datenverkehr in die OMS-Dienste lässt?**  

A Der Protokoll Analytics-Dienst basiert auf Azure und die Endpunkte empfangen IP-Adressen, die in der [Microsoft Azure Datacenter IP-Bereiche](http://www.microsoft.com/download/details.aspx?id=41653)sind.

Während der Bereitstellung von Diensten vorgenommen werden, ändern die eigentlichen IP-Adressen der Dienste OMS aus. Die DNS-Namen, die durch Ihre Firewall zulassen sind zu [Konfigurieren Proxy und Firewall-Einstellungen in Log Analytics](log-analytics-proxy-firewall.md)dokumentieren.

**F: Ich verwende ExpressRoute für das Herstellen einer Verbindung mit Azure. Werden meine Log Analytics Datenverkehr Meine ExpressRoute Verbindung verwenden?**  

A Die verschiedenen Typen von ExpressRoute Datenverkehr werden in der [Dokumentation ExpressRoute](./expressroute/expressroute-faqs.md#supported-services)beschrieben.

Den Datenverkehr in Log Analytics verwendet die öffentlichen peering ExpressRoute Verbindung.

**F: Gibt es eine einfache und einfache Möglichkeit, um einen vorhandenen Log Analytics-Arbeitsbereich zu einem anderen Protokoll Analytics Arbeitsbereich/Azure-Abonnement wechseln?**  Wir haben mehrere Kunden OMS Arbeitsbereiche, die wir testen wurden und teste in unseren Azure-Abonnement, und die Bewegung zu Herstellung, damit wir sie eigene Azure/OMS Abonnement verschieben möchten.  

A Die `Move-AzureRmResource` Cmdlet können Sie ein Protokoll Analytics-Arbeitsbereich, und ein Konto Automatisierung aus einem Azure-Abonnement zu einem anderen zu verschieben. Weitere Informationen finden Sie unter [Verschieben-AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Diese Änderung kann auch im Portal Azure vorgenommen werden.

Sie können keine Verschieben von Daten aus einem Protokoll Analytics Arbeitsbereich zu einem anderen oder ändern den Bereich mit der Log Analytics-Daten in gespeichert ist.

**F: wie kann ich SCOM OMS hinzufügen?**

A: aktualisieren, um das neueste Updaterollup und Importieren von Management Packs ermöglicht Ihnen Log Analytics SCOM Verbindung.

Beachten Sie, dass die SCOM-Verbindung zu Log Analytics nur verfügbar für SCOM 2012 SP1 oder höher ist.

**F: wie kann ich bestätigen Sie, dass ein Agent kommunizieren mit Log Analytics ist?**

A: um sicherzustellen, dass der Agent mit OMS kommunizieren kann, wechseln Sie zu: Control Panel, Sicherheit und Einstellungen **Für die Überwachung Microsoft-Agent**.

Suchen Sie nach einem grünen Häkchen, klicken Sie auf der Registerkarte **Azure Log Analytics (OMS)** . Einem grünen Häkchen bestätigt, dass der Agent mit OMS-Dienst kommunizieren.

Ein gelbes Warnungssymbol bedeutet, dass der Agent Probleme Kommunikation mit OMS ein Problem auftritt. Ein häufiger Grund ist der Überwachung Microsoft-Agent-Dienst wurde beendet und neu gestartet werden muss.

**F: Wie beende ich einen Agent aus der Kommunikation mit Log Analytics die?**

A: In SCOM, entfernen Sie den Computer aus der Liste der verwalteten OMS. Dadurch wird die Kommunikation über SCOM für diesen Agent beendet. Für Agents OMS sofort verbunden, Sie können verhindern, dass sie zum Kommunizieren mit OMS bis: Control Panel, Sicherheit und Einstellungen **Für die Überwachung Microsoft-Agent**.
Entfernen Sie unter **Azure Log Analytics (OMS)**alle Arbeitsbereiche aufgeführt.

## <a name="agent-data"></a>Agentdaten

**F: senden wie viele Daten kann ich durch den Agent an Log Analytics? Gibt es eine maximale Datenmenge pro Kunde?**  
A Der kostenlose Plan legt eine tägliche Linienende von 500 MB pro Arbeitsbereich an. Standard- und Premium-Pläne haben keine Beschränkung der Datenmenge, die hochgeladen wird. Als Cloud-Dienst, Log Analytics in OMS entwickelt, um automatisch Skalierung nach Zeitphasen bis zum Ziehpunkt die Lautstärke von einem Kunden – in Kürze, auch wenn es TB pro Tag handelt.

Der Log Analytics-Agent wurde entwickelt, um sicherzustellen, dass sie eine kleine ist und einige einfache Daten Komprimierung unterstützt. Einer unserer Kunden hat einen Blog tatsächlich geschrieben, klicken Sie auf die Tests, die sie für unsere Agent und wie beeindruckt Waren ausgeführt. Die Lautstärke der Daten variiert basierend auf den Kunden ermöglicht Lösungen. Sie können finden ausführliche Informationen auf dem Datenträger Daten und finden Sie unter der Aufsplittung Bezug auf Öffnung von Lösung unter **Verwendung** der Kachel in der Prozessübersicht OMS.

Weitere Informationen können Sie einen [Kunden Blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) über die niedrig Platzbedarf des OMS-Agents lesen.

**F: wie viel Bandbreite beim Senden von Daten zu Log Analytics von Microsoft Management Agent (MMA) verwendet wird?**

A Bandbreite ist eine Funktion auf die Menge der Daten, die gesendet wird. Daten werden komprimiert, wie sie über das Netzwerk gesendet wird.

**F: wie viele Daten pro Agent gesendet werden?**

A Dies hängt weitgehend:

- die Lösungen aktiviert sind
- die Anzahl der Protokolle und Leistungsindikatoren erfasst wird
- Daten in die Protokolle der Lautstärke

Die kostenlose Preisgestaltung Ebene ist eine gute Möglichkeit, integrierte verschiedene Servers und der Monitor die Lautstärke typische Daten. Insgesamt wird Verwendung auf der Seite **Verwendung** angezeigt.
Auf Computern, die den Agent WireData ausführen können, können Sie sehen, wie viele Daten mithilfe der folgenden Abfrage gesendet werden:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Protokoll Analytics und Abrufen und Ausführung in Minuten [Erste Schritte mit Log Analytics](log-analytics-get-started.md) .
