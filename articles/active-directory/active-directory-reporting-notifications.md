<properties
    pageTitle="Reporting Azure-Active Directory-Benachrichtigungen"
    description="So verwenden Sie Benachrichtigungen für verdächtigen Vorzeichen reporting Azure Active Directory ins."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Reporting Azure-Active Directory-Benachrichtigungen

## <a name="what-reports-generate-email-notifications"></a>Welche Berichte e-Mail-Benachrichtigungen erstellen

Zu diesem Zeitpunkt e-Mails nur die unregelmäßiges anmelden Aktivität Bericht Trigger Benachrichtigungen.

## <a name="what-is-an-irregular-sign-in"></a>Was ist ein "unregelmäßigen anmelden"?

Unregelmäßiges Sign-ins sind diejenigen, die von unserem Computer learning Algorithmen, die auf der Grundlage einer "unmöglich Reisen" Bedingung in Kombination mit einer abweichenden Speicherort und Gerät identifiziert wurden. Dies kann hinweisen, dass ein Hacker versucht hat, melden Sie sich mit diesem Konto.

## <a name="who-receives-the-email-notifications"></a>Wer erhält die e-Mail-Benachrichtigungen?

Für alle globalen Administratoren, die eine Active Directory-Premium-Lizenz zugewiesen wurden, wird die e-Mail gesendet. Um sicherzustellen, dass sie bereitgestellt wird, senden wir es an die Administratoren alternative e-Mail-Adresse als auch ein. Administratoren sollte enthalten aad-alerts-noreply@mail.windowsazure.com in ihrer Liste der sicheren Absender, damit sie die e-Mail nicht verpasst werden.

## <a name="how-often-are-these-emails-sent"></a>Wie oft werden diese e-Mails gesendet werden?

Die e-Mail-Nachricht wird gesendet, wenn 10 neue unregelmäßige Anmeldung Aktivitäten in den letzten 30 Tagen auftreten, oder seit die letzte e-Mail gesendet wurde, je nachdem, was kleiner.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Wie greife ich auf den Bericht in der e-Mail angegeben ist?

Wenn Sie auf den Link klicken, werden Sie für die Berichtseite in der klassischen Azure-Portal weitergeleitet. Um den Bericht zugreifen zu können, müssen Sie beide sein:

- Ein oder co-Administrator Ihres Azure-Abonnements

- Ein globaler Administrator im Verzeichnis, und eine Active Directory-Premium-Lizenz zugewiesen. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Kann ich diese e-Mails deaktivieren?

Ja, um Benachrichtigungen im Zusammenhang mit abweichenden Sign-ins im klassischen Azure-Portal zu deaktivieren, klicken Sie auf **Konfigurieren**, und wählen Sie dann unter dem Abschnitt **Benachrichtigungen** **deaktiviert** .

## <a name="whats-next"></a>Nächste Schritte
- Wissen möchten, welche Sicherheit, Audit und Aktivität Berichte zur Verfügung stehen? Schauen Sie sich [Azure AD-Sicherheit, Audit, und Berichte](active-directory-view-access-usage-reports.md)
- [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Fügen Sie Unternehmen branding anmelden und Access-Bereich Seiten hinzu](active-directory-add-company-branding.md)
