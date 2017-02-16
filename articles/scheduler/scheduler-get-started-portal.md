<properties
 pageTitle="Erste Schritte mit Azure Scheduler Azure-Portal | Microsoft Azure"
 description="Erste Schritte mit Azure Scheduler Azure-Portal"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Erste Schritte mit Azure Scheduler Azure-Portal

Es ist denkbar einfach geplante Aufträge in Azure Scheduler. In diesem Lernprogramm erfahren Sie, wie Sie ein Projekt erstellen. Lernen Sie auch die Überwachung und Verwaltung der Scheduler-Funktionen.

## <a name="create-a-job"></a>Erstellen Sie ein Projekt

1.  Melden Sie sich beim [Portal Azure](https://portal.azure.com/)an.  

2.  Klicken Sie auf **+ neu** > Geben Sie in das Suchfeld ein _Scheduler_ > **Scheduler** in Suchergebnissen auswählen > Klicken Sie auf **Erstellen**.

     ![][marketplace-create]

3.  Lassen Sie uns Erstellen eines Auftrags, das einfach http://www.microsoft.com/ für eine GET-Anforderung erreicht. Geben Sie im Bildschirm **Job Scheduler** die folgenden Informationen ein:

    1.  **Name:**`getmicrosoft`  

    2.  **Abonnement:** Ihr Abonnement Azure   

    3.  **Auflistung der beruflichen Position:** Wählen Sie eine vorhandene Position Sammlung aus, oder klicken Sie auf **Neu erstellen** > Geben Sie einen Namen ein.

4.  Als Nächstes in den **Einstellungen von Objektaktion**definieren Sie die folgenden Werte ein:

    1.  **Aktionstyp:**` HTTP`  

    2.  **Methode:**`GET`  

    3.  **URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Schließlich einen Zeitplan uns definieren. Der Auftrag als einmaligen Auftrag definiert werden, aber wir wählen Sie einen Zeitplan Serie aus:

    1. **Serie**:`Recurring`

    2. **Starten**: des heutigen Datums

    3. **Wiederholenden jeder**:`12 Hours`

    4. **Endet am**: zwei Tage ab dem heutigen Datum das Datum  

      ![][recurrence-schedule]

6.  Klicken Sie auf **Erstellen**

## <a name="manage-and-monitor-jobs"></a>Verwalten und Überwachen von Projekten

Nachdem ein Projekt erstellt wurde, wird es im Hauptfenster Azure Dashboard angezeigt. Klicken Sie auf den Auftrag und ein neues Fenster öffnet, mit den folgenden Registerkarten:

1.  Eigenschaften  

2.  Aktionseinstellungen  

3.  Zeitplan  

4.  Verlauf

5.  Benutzer

    ![][job-overview]

### <a name="properties"></a>Eigenschaften

Diese schreibgeschützten Eigenschaften beschreiben die Metadaten für den Job Scheduler.

   ![][job-properties]


### <a name="action-settings"></a>Aktionseinstellungen

Für ein Projekt im Bildschirm **Aufträge** klicken, können Sie die Löschung zu konfigurieren. Dies können Sie erweiterte Einstellungen konfigurieren, wenn Sie diese in konfiguriert haben der Assistent zum schnellen erstellen.

Für alle Aktionstypen können Sie die Richtlinie "Wiederholen" und die Fehleraktion geändert werden.

Für HTTP und HTTPS Auftrag Aktionstypen können die Methode zu einem beliebigen zulässigen HTTP-Verb geändert werden. Sie können auch hinzufügen, löschen oder Ändern von Kopf- und Standardauthentifizierung Informationen.

Für Speicher Warteschlange Aktionstypen können Sie die Speicher-Konto, Name der Warteschlange, SAS Token und Textkörper ändern.

Für Aktion Bustypen Dienst können Sie den Namespace Thema/Warteschlange Pfad, Authentifizierungseinstellungen,, Eigenschaften der Nachricht und Transporttyp Nachrichtentext ändern.

   ![][job-action-settings]

### <a name="schedule"></a>Zeitplan

Auf diese Weise können Sie den Zeitplan, neu zu konfigurieren, wenn Sie möchten, ändern Sie den Zeitplan Sie erstellt, in haben der Assistent zum schnellen erstellen.

Dies ist eine Verkaufschance [komplexe Zeitpläne und erweiterte Serie in Ihrem Auftrag](scheduler-advanced-complexity.md) erstellen

Sie können den Anfangstermin geändert und Zeit, Serie planen und Ende Datum und Uhrzeit (Wenn Sie der Auftrag wiederkehrende ist.)

   ![][job-schedule]


### <a name="history"></a>Verlauf

Die Registerkarte **Verlauf** zeigt die ausgewählte Kriterien für die Ausführung jeder Auftrags im System für die ausgewählte Stelle. Diese Kennzahlen bieten in Echtzeit Werte zu den Integritätsstatus Ihrer Scheduler an:

1.  Status  

2.  Details  

3.  Wiederholungsversuche

4.  Vorkommen: 1st, 2nd, 3rd, usw..

5.  Startzeit des Ausführung  

6.  Endzeit der Ausführung

   ![][job-history]

Klicken Sie auf eine ausführen, um die folgenden **Details Verlauf**die gesamte Antwort für jede Ausführung anzuzeigen. In diesem Dialogfeld können auch die Antwort in die Zwischenablage zu kopieren.

   ![][job-history-details]

### <a name="users"></a>Benutzer

Azure rollenbasierte Access Steuerelement (RBAC) ermöglicht die Verwaltung von abgestimmte Zugriff für Azure Scheduler. Informationen zum Verwenden der Registerkarte Benutzer, verweisen auf [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Siehe auch

 [Was ist der Scheduler?](scheduler-intro.md)

 [Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [So erstellen Sie komplexe Zeitpläne und erweiterte Serie mit Azure Scheduler](scheduler-advanced-complexity.md)

 [Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Grenzwerte für Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Ausgehende Scheduler-Authentifizierung](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
