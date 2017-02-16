<properties
   pageTitle="Log Analytics benachrichtigen Webhook Beispiel"
   description="Eine der Aktionen, die Sie als Antwort auf eine Warnung Log Analytics ausführen können ist eine *Webhook*, dem Sie einen externen Prozess über eine einzelne HTTP-Anforderung aufrufen kann. In diesem Artikel führt durch ein Beispiel für eine Aktion Webhook in einer Log Analytics mit Pufferzeit erstellen."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks Log Analytics-Benachrichtigungen

Eine der Aktionen, die Sie als Antwort auf eine [Benachrichtigung Log Analytics](log-analytics-alerts.md) ausführen können ist eine *Webhook*, dem Sie einen externen Prozess über eine einzelne HTTP-Anforderung aufrufen kann.  Sie können folgende Informationen Details Benachrichtigungen und Webhooks Benachrichtigungen [in Log Analytics](log-analytics-alerts.md)

In diesem Artikel werden wir durchgehen ein Beispiel für das Erstellen einer Aktion Webhook in einer Log Analytics Pufferzeit also ein messaging-Dienst verwenden.

>[AZURE.NOTE] Sie müssen ein Pufferzeit Konto in diesem Beispiel ausführen.  Sie können für ein kostenloses Konto bei [slack.com](http://slack.com)registrieren.

## <a name="step-1---enable-webhooks-in-slack"></a>Schritt 1: Aktivieren der Webhooks in Pufferzeit
2.  Melden Sie sich bei [slack.com](http://slack.com)Pufferzeit Ende.
3.  Wählen Sie einen Kanal im Abschnitt **Kanäle** im linken Bereich ein.  Dies ist der Kanal, dem die Nachricht gesendet werden.  Sie können einen der standardmäßigen Kanäle wie **Allgemeine** oder **Zufallszahl**auswählen.  In einem Szenario Herstellung möchten Sie wahrscheinlich einen speziellen Kanal wie **Criticalservicealerts**erstellen. <br>

    ![Pufferzeit Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Klicken Sie auf **app hinzufügen oder benutzerdefinierte Integration** um Verzeichnis App zu öffnen.
3.  Geben Sie *Webhooks* in das Suchfeld ein, und wählen Sie dann **Eingehende WebHooks**. <br>

    ![Pufferzeit Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Klicken Sie neben dem Namen der Teamwebsite auf **Installieren** .
5.  Klicken Sie auf **Konfiguration hinzufügen**.
6.  Wählen Sie aus der Kanal, den Sie verwenden Sie für dieses Beispiel, und klicken Sie dann auf **Integration eingehende WebHooks hinzufügen**.  
6. Kopieren Sie die **Webhook-URL**ein.  Sie können dies in die Konfiguration der Warnung eingefügt werden. <br>

    ![Pufferzeit Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Schritt 2 – benachrichtigen Regel in Log Analytics erstellen
1.  [Erstellen eine Regel](log-analytics-alerts.md) die folgenden Einstellungen.
    - Abfrage:```    Type=Event EventLevelName=error ```
    - Aktivieren Sie für diese Warnung jeder: 5 Minuten
    - Ist die Anzahl der Ergebnisse: größer als 10
    - Über diese Zeitfenster: 60 Minuten
    - Wählen Sie **Ja** für **Webhook** und **nicht** für andere Aktionen.
7. Fügen Sie die Pufferzeit-URL in das Feld **Webhook-URL** ein.
8. Wählen Sie die Möglichkeit, **eine benutzerdefinierte JSON-Nutzlast einzubeziehen**.
9. Pufferzeit erwartet eine Nutzlast JSON mit einem *Text*-Parameter formatiert ist.  Dies ist der Text, den in der Nachricht angezeigt werden, die sie erstellt.  Können eine oder mehrere der Benachrichtigung Parameter mithilfe der *#* Symbol-beispielsweise wie im folgenden Beispiel gezeigt.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![Beispiel für JSON-Nutzlast](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Klicken Sie auf **Speichern** , um die Regel zu speichern.

10. Warten Sie genügend Zeit für eine Benachrichtigung erstellt werden, und aktivieren Sie dann die Pufferzeit für eine Nachricht die werden ähnlich wie der folgende an.

    ![Beispiel für Webhook in Pufferzeit](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Erweiterte Webhook Nutzlast für Pufferzeit

Sie können eingehende Nachrichten mit Pufferzeit umfassend anpassen. Weitere Informationen finden Sie unter [Eingehende Webhooks](https://api.slack.com/incoming-webhooks) auf der Website Pufferzeit. Es folgt eine komplexere Nutzlast eine umfangreiche Nachricht mit Formatierung zu erstellen:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Dies erzeugt eine Nachricht in Pufferzeit wie folgt.

![Beispiel für die Nachricht in Pufferzeit](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Zusammenfassung

Mit dieser Regel benachrichtigen direkte müssten Sie eine Nachricht an Pufferzeit jedes Mal, wenn die Kriterien erfüllt ist.  

Dies ist nur ein Beispiel für eine Aktion, die Sie als Antwort auf eine Benachrichtigung erstellen können.  Sie können eine Webhook Aktion, die einer anderen externen Dienst ruft, eine Aktion Runbooks, eine Runbooks in Azure Automatisierung starten oder eine e-Mail-Aktion zum Senden einer e-Mails an sich selbst oder andere Empfänger erstellen.   

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen Sie zu [Warnungen im Log Analytics](log-analytics-alerts.md) einschließlich anderer Aktionen.
- [Erstellen von Runbooks in Azure Automatisierung](../automation/automation-webhooks.md) , die von einem Webhook aufgerufen werden können.
