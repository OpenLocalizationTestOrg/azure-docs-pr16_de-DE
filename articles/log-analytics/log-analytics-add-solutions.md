<properties
    pageTitle="Log Analytics Lösungen aus dem Lösungskatalog hinzufügen | Microsoft Azure"
    description="Log Analytics-Lösungen enthalten sind, dass eine Auflistung von Logik, Visualisierung und Daten Acquisition Regel, dass die Kennzahlen gedreht, um ein bestimmtes Problembereich bereitstellen."
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

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Hinzufügen von Lösungen Log Analytics aus dem Lösungskatalog

Log Analytics Lösungen sind eine Sammlung von **Logik**, **Visualisierung** und **Daten Acquisition Regeln** , die Metrik, um einen bestimmten Problembereich gedreht bereitstellen. Dieser Artikel Listen Lösungen Log Analytics unterstützt und erfahren, wie Sie das Hinzufügen und entfernen sie im Lösungskatalog verwenden.

Lösungen zulassen tiefere Einsichten zu:

- Untersuchen und Probleme lösen, die schneller Hilfe
- Sammeln Sie und zu koordinieren Sie verschiedene Arten von Computerdaten
- helfen Sie proaktiv mit Aktivitäten wie Kapazität planen, Patch Arbeitszeiten und Sicherheit Überwachung werden.


>[AZURE.NOTE] Log Analytics enthält Log-Suchfunktion, damit Sie nicht so installieren Sie eine Lösung, um es zu aktivieren. Allerdings können Sie datenvisualisierungen, empfohlene Suchbegriffe und Einblicken durch Hinzufügen von Lösungen aus den Lösungskatalog erhalten.

Nachdem Sie eine Lösung hinzugefügt haben, werden die Daten auf den Servern in Ihrer Infrastruktur erfassten und an den OMS-Dienst gesendet werden. Verarbeitung, indem Sie die OMS nimmt Dienst in der Regel ein paar Minuten in eine Stunde um. Nachdem der Dienst Daten verarbeitet, können Sie es in OMS anzeigen.

Sie können ganz einfach eine Lösung entfernen, wenn es nicht mehr benötigt wird. Wenn Sie eine Lösung entfernen, werden deren Daten nicht an OMS, gesendet verkürzen die Datenmenge verwendet, indem Sie Ihr Kontingent für den Tag, wenn dort noch besteht.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Unterstützt vom Microsoft Agent Überwachung Solutions

Zu diesem Zeitpunkt können Server, die mit dem Microsoft-Überwachung-Agent OMS verbunden sind die meisten der Lösungen, einschließlich:

- Active Directory-Bewertung
- Alert-Management (ohne SCOM Benachrichtigungen)
- Modul
- Nachverfolgen von Änderungen
- Sicherheit
- SQL-Bewertung
- System-Updates

Die folgenden Lösungen sind jedoch *nicht* unterstützt, mit dem Microsoft-Agent für die Überwachung und System Center Operations Manager (SCOM) Agent erforderlich.

- Alert-Management (einschließlich SCOM Benachrichtigungen)
- Kapazität Management
- Konfiguration Bewertung

Informationen zum Verbinden von SCOM-Agent mit Log Analytics finden Sie unter [Herstellen einer Verbindung Operations Manager Log Analytics](log-analytics-om-agents.md) .

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Hinzufügen eine Lösung im Lösungskatalog verwenden

1. Klicken Sie auf der Seite Übersicht in OMS auf die Kachel **Lösungskatalog** .    
    ![Lösungskatalog](./media/log-analytics-add-solutions/sol-gallery.png)
2. Erfahren Sie auf der Seite OMS Lösungskatalog jede Lösung zur Verfügung. Klicken Sie auf den Namen der Lösung, die Sie zu OMS hinzufügen möchten.
3. Klicken Sie auf der Seite für die Lösung, die Sie ausgewählt haben, wird die detaillierte Informationen zur Lösung angezeigt. Klicken Sie auf **Hinzufügen**.
4. Eine neue Kachel für die Lösung, die Sie hinzugefügt haben, klicken Sie auf der Übersicht über die angezeigten Seite OMS und Sie wieder verwenden kann, nachdem der OMS-Dienst Ihre Daten verarbeitet werden.

## <a name="to-configure-solutions"></a>Konfigurieren von Lösungen
1. Sie müssen einige Lösungen zu konfigurieren. Sie müssen beispielsweise Automatisierung, Azure Website Wiederherstellung und Sicherung konfigurieren, bevor Sie sie verwenden können.
2. Damit diese Lösungen klicken Sie auf die Kachel auf der Seite Übersicht.  
    ![Konfigurieren der Lösung](./media/log-analytics-add-solutions/configure-additional.png)
3. Klicken Sie dann konfigurieren Sie die Lösung mit den erforderlichen Informationen ein, und klicken Sie dann auf **Speichern**.  
    ![Konfigurieren der Lösung](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>So entfernen Sie eine Lösung im Lösungskatalog verwenden

1. Klicken Sie auf der Seite Übersicht in OMS auf die Kachel " **Einstellungen** ".
2. Klicken Sie auf der Einstellungsseite unter der Registerkarte Lösungen auf **Entfernen** für die Lösung, die Sie entfernen möchten.
3. Klicken Sie im Bestätigungsdialogfeld auf **Ja,** um die Lösung zu entfernen.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Einzelheiten zur Datensammlung für OMS-Features und Lösungen

Die folgende Tabelle zeigt Datensammlungsmethoden und andere Details, wie Daten für OMS-Features und Lösungen erfasst werden. Direkte-Agents und SCOM Agents sind im Wesentlichen identisch, jedoch der direkte Agent zusätzliche Funktionen zum Zulassen darauf umfasst, um eine Verbindung mit dem Arbeitsbereich OMS und über einen Proxy weiterleiten. Wenn Sie einen Agent SCOM verwenden, muss es als Agent OMS zur Kommunikation mit OMS verwendet werden. SCOM Agents in dieser Tabelle sind die OMS-Agents, die mit SCOM verbunden sind. Informationen zum Verbinden Ihrer vorhandenen SCOM Umgebung zu OMS finden Sie unter [Log Analytics Operations Manager verbinden](log-analytics-om-agents.md) .

>[AZURE.NOTE] Der Typ des Agents, mit denen Sie bestimmt, wie Daten an OMS, mit den folgenden Situationen gesendet werden:

- Sie verwenden entweder die direkte Agent oder Agents OMS SCOM angefügt.
- Wenn SCOM erforderlich ist, werden SCOM Agentdaten für die Lösung immer OMS gesendet mithilfe der Management Group unter SCOM. Außerdem, wenn SCOM erforderlich ist, wird der SCOM-Agent von der Lösung verwendet.
- Wenn SCOM nicht erforderlich ist, und die Tabelle zeigt, dass SCOM Agentdaten mit dem Management Group OMS gesendet werden, werden immer SCOM Agentdaten an OMS gesendet Entwurfsphase Management. Direkte Agents das Management Group unter umgehen und senden ihre Daten direkt an OMS.
- Wenn SCOM Agentdaten nicht mit einer Management Group unter gesendet werden, klicken Sie dann die Daten direkt an OMS gesendet – das Management Group unter umgehen.


|Datentyp| Plattform | Direkte Agent | SCOM agent | Azure-Speicher | SCOM erforderlich? | SCOM Agentdaten per Management Group unter gesendeten | Häufigkeit Collection |
|---|---|---|---|---|---|---|---|
|AD-Bewertung|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 Tage|
|AD Replikationsstatus|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Tage|
|Benachrichtigungen (Nagios)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Beim Eintreffen|
|Benachrichtigungen (Zabbix)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 minute|
|Benachrichtigungen (Operations Manager)|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 Minuten|
|Modul|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| stündlich|
|Kapazität Management|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| stündlich|
|Nachverfolgen von Änderungen|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| stündlich|
|Nachverfolgen von Änderungen|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|stündlich|
|Konfiguration Bewertung (legacy Advisor)|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| zweimal pro Tag|
|ETW|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|IIS-Protokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|Key Depots|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 Minuten|
|Netzwerk-Anwendungsgateways|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 Minuten|
|Netzwerk-Sicherheitsgruppen|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 Minuten|
|Office 365|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Benachrichtigung|
|-Datenquellen|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|als geplant, mindestens 10 Sekunden|
|-Datenquellen|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|als geplant, mindestens 10 Sekunden|
|Dienst Fabric|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|SQL-Bewertung|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 Tage|
|SurfaceHub|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Beim Eintreffen|
|Syslog|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|aus Azure-Speicher: 10 Minuten; Agents: beim Eintreffen|
|System-Updates|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| mindestens 2 Mal pro Tag und 15 Minuten nach der Installation eines Updates|
|Windows-Sicherheits-Ereignisprotokollen|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| für Azure-Speicher: 10 min; für den Agent: beim Eintreffen|
|Windows-Firewall von Protokollen|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| Beim Eintreffen|
|Windows-Ereignisprotokollen|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| für Azure-Speicher: 1 min; für den Agent: beim Eintreffen|
|Über das Netzwerk Daten|Windows (2012 R2 / 8.1 oder höher)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| Jede minute|

## <a name="log-analytics-preview-solutions-and-features"></a>Melden Sie sich Analytics Vorschau Lösungen und Funktionen

Durch einen Dienst ausgeführt und Devops Methoden folgen können wir mit Kunden zum Entwickeln von Features und Lösungen für partner.

Während der privaten Vorschau teilen wir eine kleine Gruppe von Kunden Access eine frühe Implementierung von Features oder Lösung für Ihr Feedback zu erhalten und zu optimieren. Diese frühe Implementierung hat minimale Features und Funktionen von funktionsfähig.

Unser Ziel ist, Dinge schnell zu testen, damit wir erreichen können, was funktioniert und was nicht funktioniert. Wir durchlaufen Sie diesen Vorgang, bis das Feedback von Kunden als "Privat" Vorschau uns darüber informiert, dass wir für eine öffentliche Vorschau bereit sind.

Bei der öffentlichen Vorschau Verfügbarmachen wir das Feature oder die Lösung für alle Benutzer erhalten Sie weitere Feedback und überprüfen unsere Skalierung und Effizienz. In dieser Phase:

- Vorschau-Features werden in der Registerkarte Einstellungen angezeigt und können von jedem Benutzer aktiviert werden
- Vorschau Lösungen hinzugefügt werden können, bis der Katalog oder mithilfe eines veröffentlichten Skripts

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Was muss ich über die Funktionen für Vorschau und Lösungen wissen?

Wir freuen zu neuen Features und Lösungen und wir gerne arbeiten mit Ihnen diese entwickeln.

Vorschau Features und Lösungen nicht für jeden rechts durch, also Gruppenmitglieder für die Teilnahme an, bevor Sie eine Vorschau private oder eine öffentliche Vorschau aktivieren sicherstellen, dass Sie OK mit etwas arbeiten, die in der Entwicklung ist.

Wenn Sie ein Feature Vorschau über das Portal aktivieren durchgeführt haben, Sie finden Sie unter eine Warnung, die Sie daran erinnert, dass das Feature in der Vorschau ist.

#### <a name="for-both-private-and-public-preview"></a>Für *private* und *Öffentliche* preview

Öffentliche und private Vorschauen gilt Folgendes:

- Punkte funktioniert immer nicht richtig.
  - Probleme im Bereich von wird eine kleinere Belästigung bis auf bestimmte gar nicht funktioniert
- Es ist für die Vorschau, um einen negativen Einfluss auf Ihre Systeme haben mögliche / Umgebung
  - Wir versuchen, vermeiden Sie negative Punkte weiterhin auf die Systeme mit OMS verwenden, doch manchmal unerwarteten Dinge auftreten
- Datenverlust / Beschädigung kann auftreten.
- Möglicherweise bitten wir Sie zum Erfassen von Diagnoseprotokollen oder andere Daten zu Problemen
- Das Feature oder die Lösung möglicherweise (vorübergehend oder dauerhaft) entfernt werden
  - Basierend auf unsere Befunde bei der Vorschau, die doch wir möglicherweise nicht das Feature oder die Lösung lassen
- Vorschauen funktioniert möglicherweise nicht oder möglicherweise nicht wurde getestet mit allen Konfigurationen, und wir möglicherweise beschränken:
  - Die Betriebssysteme, die verwendet werden können (z. B. ein Feature möglicherweise gelten nur für Linux in der Vorschau)
  - Den Typ des Agents (MMA, SCOM), die verwendet werden kann (z. B. ein Feature funktioniert möglicherweise nicht mit SCOM in der Vorschau)  
- Vorschau Lösungen und Features fallen nicht unter den Ebene-Servicevertrag
- Verwendung der Preview-Features werden Verwendung anfallen
- Features oder -Funktionen, die Sie für das Feature benötigen / Lösung sinnvoll genutzt werden möglicherweise fehlt oder unvollständig
- Features / Lösungen möglicherweise nicht verfügbar in allen Regionen
- Features / Lösungen möglicherweise nicht lokalisiert
- Features / Lösungen möglicherweise einen Grenzwert für die Anzahl der Kunden oder Geräte, die sie verwenden können
- Möglicherweise müssen Sie die Skripts Konfiguration ausführen, und aktivieren die Lösung-Funktion verwenden.
- Die Benutzeroberfläche (UI) wird unvollständig und Änderung von Tag
- Öffentliche Vorschauen möglicherweise nicht für Ihre Herstellung entsprechenden / kritische Systeme

#### <a name="for-private-preview"></a>Für *private* preview

Zusätzlich zu den obigen sind folgende speziell für private Vorschauen:

- Wir erwarten, dass Sie uns Feedback senden, klicken Sie auf der Benutzeroberfläche bereitstellen, damit wir die Feature oder eine Lösung verbessern können
- Wir können Sie für die Verwendung von Umfragen, Telefonanrufe oder e-Mail-Feedback wenden.
- Punkte funktionieren immer ordnungsgemäß nicht
- Wir eine nicht-Offenlegung Vertrag (NDA) für die Teilnahme erfordern möglicherweise oder enthalten möglicherweise vertrauliche Inhalte
  - Überprüfen Sie mit dem Programm-Manager die Vorschau verantwortlich, Einschränkungen Offenlegung zu verstehen, vor dem Bloggen, TWEETS oder andernfalls Kommunikation mit Drittanbietern
- Nicht ausgeführt Herstellung / kritische Systeme


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Wie erhalte ich Zugriff auf private Preview-Features und Lösungen?

Wir laden Sie Ihre Kunden auf private Vorschauen durch verschiedene Arten je nach der Vorschau.

- Die monatliche Kundenumfrage beantwortet und erteilen uns Rechte für bei Ihnen melden verbessert Ihre Chancen, um eine Vorschau der privaten eingeladen werden.
- Ihr Team der Microsoft-Konto können Sie Diskussionsbeiträge.
- Anhand von Details veröffentlicht Twitter [Msopsmgmt](https://twitter.com/msopsmgmt) anmelden können
- Basierend auf Details freigegebenen Community Ereignisse – anmelden können besprechen für uns prüfen verstärken, Konferenzen und online-Communities.


## <a name="next-steps"></a>Nächste Schritte

- [Suchen von Protokollen](log-analytics-log-searches.md) , detaillierte von Lösungen gesammelten Informationen anzuzeigen.
