<properties
    pageTitle="Einrichten von Benachrichtigungen für Ihre Microsoft Azure-Abonnements Abrechnung | Microsoft Azure"
    description="Beschreibt das Festlegen von Benachrichtigungen auf Ihrer Rechnung Azure, damit Sie Abrechnung Auflösung vermeiden können."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Einrichten von Abrechnung Benachrichtigungen für Ihre Microsoft Azure-Abonnements

Besteht das Risiko zu wie viel Sie jeden Monat für Ihr Abonnement Azure ausgeben? Wenn Sie für ein Abonnement Azure Konto-Administrator sind, können die Azure Abrechnung Alert Service zum Erstellen von angepassten Abrechnung Benachrichtigungen, mit denen Sie überwachen und Verwalten der Abrechnung Aktivität für Ihre Azure-Konten.

Dieser Dienst ist ein Dienst Vorschau Erstes müssen Sie nur melden Sie sich von dafür. Finden Sie auf [der Seite Vorschau-Features](https://account.windowsazure.com/PreviewFeatures) im Verwaltungsportal Azure-Konto dieses Feature aktivieren, gehen Sie wie folgt.

## <a name="set-the-alert-threshold-and-email-recipients"></a>Legen Sie die Benachrichtigung Schwellenwert und e-Mail-Empfänger

Nachdem Sie die e-Mail-Bestätigung, die den Dienst aktiviert ist, klicken Sie auf für Ihr Abonnement erhalten haben, finden Sie auf [der Seite Abonnements](https://account.windowsazure.com/Subscriptions) im Kontoportal. Klicken Sie auf das Abonnement, die, das Sie überwachen möchten, und klicken Sie dann auf **Benachrichtigungen**.

![][Image1]

Klicken Sie als Nächstes auf **Benachrichtigung hinzufügen** , um Ihr erstes Organigramm erstellen – Sie können insgesamt fünf Abrechnung Benachrichtigungen pro Abonnement, mit einem anderen Schwellenwert und bis zu zwei e-Mail-Empfänger für jede Warnung einrichten.

![][Image2]

Wenn Sie eine Benachrichtigung hinzufügen, Sie geben sie einen eindeutigen Namen, wählen Sie einen Ausgaben Schwellenwert aus, und wählen die e-Mail-Adressen, wenn Benachrichtigungen gesendet werden sollen. Beim Einrichten von des Schwellenwerts, können Sie aus der Liste **Benachrichtigungssounds für** **Abrechnung Total** oder eine **Gutschrift monetäre** auswählen. Für insgesamt Abrechnung wird eine Benachrichtigung gesendet, wenn Abonnement Ausgaben den Schwellenwert überschreitet. Für monetäre haben wird eine Benachrichtigung gesendet beim monetäre Gutschriften unter den Grenzwert ablegen. Monetäre Gutschriften gelten normalerweise für kostenlose Testversionen und Abonnements mit MSDN-Konten verbunden sind.

![][Image3]

Azure unterstützt alle e-Mail-Adresse, aber nicht überprüfen, ob der e-Mail-Adresse funktioniert, also Testinstanz, nach Tippfehlern.

## <a name="check-on-your-alerts"></a>Klicken Sie auf die Benachrichtigungen aktivieren

Nachdem Sie Benachrichtigungen eingerichtet haben, wird das Konto Center listet sie und zeigt, wie viele weitere Sie einrichten können. Für jede Warnung wird das Datum und Uhrzeit, die sie gesendet wurde, ob es sich um eine Benachrichtigung für insgesamt Abrechnung oder monetäre Kreditkarte ist, und der Grenzwert, den Sie einrichten. Das Datum und Uhrzeit-Format ist 24-Stunden-koordinieren (Weltzeit) und das Datum ist jjjj-mm-tt-Format. Klicken Sie auf das Pluszeichen (+) für eine Benachrichtigung in der Liste, um ihn zu bearbeiten, oder klicken Sie auf den Papierkorb-können, um ihn zu löschen.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
