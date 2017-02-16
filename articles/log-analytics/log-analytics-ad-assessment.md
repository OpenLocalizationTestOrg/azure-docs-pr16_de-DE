<properties
    pageTitle="Optimieren Sie Ihre Umgebung mit der Bewertung von Active Directory-Lösung in Log Analytics | Microsoft Azure"
    description="Die Bewertung der Active Directory-Lösung können Sie um die Risiken und Integritätsstatus Ihrer Server-Umgebungen in regelmäßigen Abständen zu ermitteln."
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

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimieren Sie Ihre Umgebung mit der Log Analytics-Lösung Active Directory-Bewertung

Die Bewertung der Active Directory-Lösung können Sie um die Risiken und Integritätsstatus Ihrer Server-Umgebungen in regelmäßigen Abständen zu ermitteln. In diesem Artikel helfen Ihnen, installieren und verwenden die Lösung aus, damit Sie auf mögliche Probleme Maßnahmen ergreifen können.

Diese Lösung bietet eine Liste mit Prioritätsstufe empfohlenen speziell für Ihre Serverinfrastruktur bereitgestellten. Die Empfehlungen über vier eingestuft werden Schwerpunkte dessen Hilfe Sie schnell das Risiko verstehen und agieren.

Die Empfehlungen basieren auf das Wissen und die Erfahrung von Microsoft-Experten aus Tausende von Kundenbesuchen. Jede Empfehlungen bietet eine Anleitung zum warum möglicherweise ein Problem für Sie unerheblich, und wie Sie die vorgeschlagenen Änderungen implementieren.

Sie können den Fokus Bereiche auswählen, die für Ihre Organisation wichtigsten und Nachverfolgen Ihrer Fortschritt im Hinblick auf Computern Risiko frei / fehlerfrei eingerichtet.

Nachdem Sie die Lösung hinzugefügt haben, und eine Bewertung abgeschlossen, Zusammenfassung ist werden Informationen für Bereiche des Fokus auf dem Dashboard **AD-Bewertung** für die Infrastruktur in Ihrer Umgebung angezeigt. In den folgenden Abschnitten wird beschrieben, wie die Informationen auf dem Dashboard **AD Bewertung** zu verwenden, wo Sie anzeigen und dann die empfohlene Aktionen für Ihre Active Directory-Server-Infrastruktur ausführen können.

![Abbildung der Kachel SQL-Bewertung](./media/log-analytics-ad-assessment/ad-tile.png)

![Abbildung der SQL-Bewertung dashboard](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Installieren und konfigurieren die Lösung
Verwenden Sie die folgende Informationen zum Installieren und Konfigurieren der Lösungen.

- Agents müssen auf Domänencontroller installiert werden, die Mitglieder der Domäne ausgewertet werden.
- Die Bewertung der Active Directory-Lösung erfordert .NET Framework 4 auf jedem Computer, die ein OMS-Agent ist installiert.
- Fügen Sie die Bewertung von Active Directory-Lösung in Ihren OMS Arbeitsbereich mithilfe des Prozesses [Hinzufügen Log Analytics Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschrieben.  Es ist keine weitere Konfiguration erforderlich.

    >[AZURE.NOTE] Nachdem Sie die Lösung hinzugefügt haben, wird die Datei AdvisorAssessment.exe auf Servern mit Agents hinzugefügt. Konfigurationsdaten gelesen und dann an die OMS-Dienst in der Cloud für die Verarbeitung gesendet werden. Logik wird angewendet, um die empfangenen Daten und der Cloud-Dienst Einträge die Daten.

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory-Bewertung Einzelheiten zur Datensammlung

Active Directory-Bewertung sammelt WMI-Daten, Registrierungsdaten und von Leistungsdaten mithilfe von Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt Datensammlungsmethoden für Agents, ob Operations Manager (SCOM) erforderlich ist, und wie oft Daten von einem Agent erfasst.

| Plattform | Direkte Agent | SCOM agent | Azure-Speicher | SCOM erforderlich? | SCOM Agentdaten per Management Group unter gesendeten | Häufigkeit Collection |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Nein](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![Nein](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 Tage|


## <a name="understanding-how-recommendations-are-prioritized"></a>Grundlegendes zu wie Empfehlungen Priorität zugewiesen sind

Jeder Empfehlungen vorgenommen wird gewichteten Werten angegeben, die die relative Wichtigkeit von empfohlen identifiziert. Es werden nur die zehn wichtigsten Empfehlungen angezeigt.

### <a name="how-weights-are-calculated"></a>Wie werden-Stärken berechnet.

Wobei werden zusammengefasste Werte basierend auf drei wichtige Faktoren:

- Die *Wahrscheinlichkeit* , dass ein Problem identifiziert Probleme verursachen kann. Eine höhere Wahrscheinlichkeit entspricht eine größere allgemeine Bewertung für empfohlen.

- Die *Auswirkungen* des Problems auf Ihrer Organisation, wenn es ein Problem verursacht. Eine höhere Auswirkung entspricht eine größere allgemeine Bewertung für empfohlen.

- Der *Aufwand* erforderlich, um die Empfehlungen implementieren. Eine höhere Initiative entspricht eine kleinere allgemeine Bewertung für empfohlen.

Die Gewichtung für jede Empfehlungen ist ausgedrückt als Prozentsatz der das Gesamtergebnis für jeden Fokusbereich zur Verfügung. Beispielsweise weist eine Empfehlungen im Fokusbereich Sicherheit und Einhaltung von Vorschriften eine Bewertung von 5 %, wird dieser Empfehlungen implementieren Ihre allgemeine Sicherheit und Kompatibilität Punktzahl von 5 % vergrößern

### <a name="focus-areas"></a>Fokus Bereiche

**Sicherheit und Einhaltung von Vorschriften** – in diesem Fokusbereich zeigt Empfehlungen für potenzielle Sicherheitsrisiken und Verletzung, Unternehmensrichtlinien und technische, rechtliche Hinweise und gesetzlichen Vorschriften.

**Verfügbarkeit und Geschäftskontinuität** – in diesem Fokusbereich zeigt Empfehlungen für die Verfügbarkeit von Diensten, Stabilität Ihrer Infrastruktur und Schutz des Unternehmens.

**Leistung und Skalierbarkeit** – in diesem Fokusbereich zeigt Empfehlungen Ihrer Organisation helfen IT-Infrastruktur wächst, stellen Sie sicher, dass Ihre IT-Umgebung aktuelle Leistung Anforderungen erfüllt, und zum Ändern von Infrastruktur Anforderungen reagieren.

**Upgrade-Migration und-Bereitstellung** – in diesem Fokusbereich zeigt Empfehlungen, mit deren Hilfe Sie das upgrade, migrieren und Bereitstellen von Active Directory in eine vorhandene Infrastruktur.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Sie sollten (100 %) in jedem Fokusbereich erhalten sollen?

Nicht unbedingt. Die Empfehlungen basieren auf die Kenntnisse und Erfahrung gewonnen von Microsoft-Experten über Tausende von Kundenbesuchen. Jedoch keine zwei Server-Infrastruktur sind gleich, und Empfehlungen im Zusammenhang mit bestimmten möglicherweise mehr oder weniger für Sie interessanten. Einige Mittelpunkt möglicherweise beispielsweise weniger relevant, wenn Ihre virtuellen Computer nicht mit dem Internet verbunden sind. Einige Verfügbarkeit Empfehlungen für Dienste weniger relevante möglicherweise, die niedriger Priorität ad-hoc-Datensammlung und Berichte bereitstellen. Probleme, die zu einem Reifen Unternehmen wichtig sind möglicherweise weniger wichtig ist, für den ein Start. Möglicherweise möchten identifizieren, welche Bereiche Fokus Ihren Prioritäten sind und prüfen, wie Ihre Ergebnisse im Zeitverlauf ändern.

Jeder Empfehlungen enthält Anleitung, warum es wichtig ist. Verwenden Sie diese Anleitung für ausgewertet werden soll, ob implementieren empfohlen für Sie geeigneten die Art Ihrer IT-Dienste und der geschäftliche Anforderungen Ihrer Organisation angegeben ist.

## <a name="use-assessment-focus-area-recommendations"></a>Verwenden Sie die Bewertung Fokus Bereich Empfehlungen

Bevor Sie eine Bewertung-Lösung in OMS verwenden können, müssen Sie die Lösung installiert haben. Weitere Informationen zum Installieren von Lösungen, finden Sie unter [Hinzufügen von Log Analytics Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md). Nachdem es installiert ist, können Sie die Zusammenfassung der Empfehlungen mithilfe der Kachel Bewertung auf der Seite Übersicht in OMS anzeigen.

Anzeigen der zusammengefasste Compliance-Bewertung für Ihre Infrastruktur und klicken Sie dann auf Drillinto Empfehlungen an.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Empfehlungen für einen Fokusbereich anzeigen und Ausführen von Maßnahmen

1. Klicken Sie auf der Seite **Übersicht** auf der Kachel **Bewertung** Ihrer Server-Infrastruktur.
2. Klicken Sie auf der Seite **Bewertung** überprüfen Sie die Zusammenfassungsinformationen in einem Bereich den Fokus Blade, und klicken Sie dann auf Termin, um Empfehlungen für den betreffenden Fokusbereich anzeigen.
3. Klicken Sie auf eine der Fokus im Bereichsseiten können Sie die Priorität Empfehlungen für Ihre Umgebung vorgenommen anzeigen. Klicken Sie unter **Betroffene Objekte** Anzeigen von Details zu den Empfehlungen Warum erfolgt auf empfohlen.  
    ![Abbildung der Bewertung Empfehlungen](./media/log-analytics-ad-assessment/ad-focus.png)
4. Sie können die erforderlichen Maßnahmen, die im **Vorgeschlagenen Aktionen**vorgeschlagen. Wenn das Element gerichtet worden ist, werden später Bewertung aufzeichnen, die ergriffen wurden, und Ihr Ergebnis Compliance nimmt empfohlen. Korrigierte Elemente werden als **Objekte übergeben**.

## <a name="ignore-recommendations"></a>Empfehlungen ignorieren

Wenn Sie Empfehlungen, die Sie ignorieren möchten haben, können Sie eine Textdatei erstellen, die OMS verwendet werden, um zu verhindern, dass Empfehlungen in Ihre Bewertungsergebnisse angezeigt werden.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Um Empfehlungen zu identifizieren, die Sie ignorieren wird

1.  Melden Sie sich bei dem Arbeitsbereich, und öffnen Sie Log suchen. Verwenden Sie die folgende Abfrage in der Liste Empfehlungen, die fehlgeschlagen ist für Computer in Ihrer Umgebung an.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Hier ist ein Screenshot mit der Log Suchabfrage: ![Fehler bei Empfehlungen](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Wählen Sie Empfehlungen, die Sie ignorieren möchten. Sie können die Werte für RecommendationId im nächsten Verfahren verwenden.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Erstellen und Verwenden einer Textdatei IgnoreRecommendations.txt

1.  Erstellen Sie eine Datei namens IgnoreRecommendations.txt.
2.  Fügen Sie ein, oder geben Sie jede RecommendationId für jede Empfehlungen gewünschte Log Analytics zu ignorieren in einer separaten Zeile und klicken Sie dann auf Speichern und schließen Sie die Datei.
3.  Setzen Sie die Datei im folgenden Ordner auf jedem Computer, der Sie OMS Empfehlungen ignorieren möchten.
    - Auf Computern mit dem Microsoft Überwachung-Agent (direkt oder durch Operations Manager verbunden sind) – *Systemlaufwerk*: \Programme\Microsoft Überwachung Agent\Agent
    - Klicken Sie auf die Operations Manager Management Server - *Systemlaufwerk*: \Programme\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Um sicherzustellen, dass Empfehlungen ignoriert werden

Nach dem nächsten Bewertung ausgeführt, standardmäßig alle 7 Tage geplant, die angegebenen Empfehlungen werden *ignoriert* gekennzeichnet, und Sie werden nicht auf dem Dashboard Bewertung angezeigt werden.

1. Die folgenden Log Suchabfragen können Sie alle ignorierten empfohlenen Liste.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Wenn Sie später entscheiden, dass ignorierte Empfehlungen angezeigt werden sollen, entfernen Sie alle Dateien IgnoreRecommendations.txt, oder Sie können RecommendationIDs daraus entfernen.

## <a name="ad-assessment-solutions-faq"></a>AD Bewertung Lösungen häufig gestellte Fragen

*Wie oft kann eine Bewertung werden ausgeführt?*
- Die Bewertung werden alle 7 Tage ausgeführt wird.

*Gibt es eine Möglichkeit zum Konfigurieren, wie oft die Bewertung ausgeführt wird?*
- Nicht zu diesem Zeitpunkt.

*Wenn Sie einen anderen Server für erkannt wird, nachdem ich eine Lösung für die Bewertung hinzugefügt haben, wird es werden überprüft?*
- Ja, sobald sich herausstellt, dass es aus, dann alle 7 Tage bewertet wird.

*Wenn eine erfüllt ist, wird Wenn sie über die Bewertung werden entfernt?*
- Wenn ein Server Daten nicht für 3 Wochen sendet, wird es entfernt.

*Was ist der Name des Prozesses, der die Sammlung von Daten unterstützt?*
- AdvisorAssessment.exe

*Wie lange dauert es, für die zu erfassenden Daten?*
- Die Sammlung von ist-Daten auf dem Server dauert ungefähr 1 Stunde. Es kann auf Servern dauern, die eine große Anzahl von Active Directory-Servern aufweisen.

*Welche Art von Daten gesammelt?*
- Die folgenden Arten von Daten erfasst werden:
    - WMI
    - Registrierung
    - -Datenquellen

*Gibt es eine Möglichkeit zum Konfigurieren, wenn die Daten erfasst werden?*
- Nicht zu diesem Zeitpunkt.

*Warum werden angezeigt nur die Top 10 Empfehlungen?*
- Anstatt Sie eine vollständige überwältigende Liste von Vorgängen, empfehlen wir, dass Sie sich auf die Priorität Empfehlungen zuerst adressieren konzentrieren. Nachdem Sie sich damit befassen, werden zusätzliche Informationen verfügbar. Wenn Sie ausführliche Liste anzeigen möchten, können Sie alle empfohlenen mithilfe der Suchfunktion Protokoll anzeigen.

*Gibt es eine Möglichkeit, ein Empfehlungen ignorieren?*
- Ja, finden Sie [Empfehlungen ignorieren](#ignore-recommendations) Abschnitt oben.


## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie zum Anzeigen der Detaildaten einer AD-Bewertung und Empfehlungen [Log durchsucht Log Analytics](log-analytics-log-searches.md) ein.
