<properties
    pageTitle="Nicht Azure-Abonnement anmelden | Microsoft Azure"
    description="Beschreibt, wie Sie einige allgemeine Azure-Abonnement Anmeldung Probleme beheben."
    services=""
    documentationCenter=""
    authors="genlin"
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
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Ich kann nicht anmelden bei meinem Azure-Abonnement zu verwalten

In diesem Artikel führt Sie durch einige der am häufigsten verwendeten Methoden zur Lösung von Problemen Login.

## <a name="page-hangs-in-the-loading-status"></a>Seite hängt in den Status laden

Wenn Ihre Internet Browserseite hängt, versuchen Sie, die folgenden Schritte aus, bis Sie das [Azure-Portal](https://portal.azure.com)zugreifen können.

-   Aktualisieren Sie die Seite.
-   Verwenden Sie einen anderen Browser.
-   Wenn Sie Microsoft Internet Explorer verwenden, wechseln Sie mithilfe des InPrivate-Browsermodus Modus Azure-Portal an. 

    A  Klicken Sie auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicherheit** > **InPrivate-Browsermodus**.

    B.  Navigieren Sie zu der [Azure-Portal](https://portal.azure.com), und klicken Sie dann auf das Portal anmelden.

## <a name="error-message-no-subscriptions-found"></a>Fehlermeldung "Keine Abonnements gefunden"

Wenn Ihr Konto keine ausreichende Berechtigungen aufweisen, wird möglicherweise eine Fehlermeldung **kein Abonnement gefunden** angezeigt. Kann nur von einem Kontoadministrator [Account Center](https://account.windowsazure.com/), erhalten nicht die Dienstadministratoren (SA) oder Co-Administratoren (CA).

**Szenario 1: Fehlermeldung wird im [Portal Azure](https://portal.azure.com) empfangen.**

Zur Behebung des Problems, für das Konto [die gemeinsame Administrator oder Besitzer Rolle hinzufügen](billing-add-change-azure-subscription-administrator.md) .

**Szenario 2: Fehlermeldung wird in der [Mitte der Azure-Konto](https://account.windowsazure.com/Subscriptions) empfangen.**

Überprüfen Sie, ob das Konto, das Ihnen verwendete Konto-Administrator. Gehen Sie folgendermaßen vor, um zu überprüfen, die der Kontoadministrator ist:

1.  Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.
2.  Wählen Sie im Menü Hub **Abonnement**aus.
3.  Wählen Sie das Abonnement, das Sie überprüfen möchten, und wählen Sie dann auf **Einstellungen**.
4.  Wählen Sie **Eigenschaften**aus. Konto-Administrator des Abonnements wird im Feld **Konto Administrator** angezeigt.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Sie werden automatisch als ein anderer Benutzer angemeldet

Dieses Problem kann auftreten, wenn Sie mehr als ein Benutzerkonto in einem Internetbrowser verwenden.

Um das Problem zu beheben, versuchen Sie eine der folgenden Methoden aus:

-   Löschen Sie den Cache und löschen Sie Internetcookies. Klicken Sie in Internet Explorer auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internetoptionen** > **Löschen**. Stellen Sie sicher, dass die Kontrollkästchen für temporäre Dateien, Cookies, Kennwort und Verlauf ausgewählt sind, und klicken Sie dann auf Löschen.

-   Zurücksetzen der Internet Explorer-Einstellungen, um alle persönlichen Einstellungen zurückzukehren, die Sie vorgenommen haben. Klicken Sie auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internetoptionen** > **Erweitert** > Aktivieren Sie das **Löschen von persönlicher Einstellungen** > **Zurücksetzen**.

-   Navigieren Sie zu der Azure-Portal im Modus InPrivate-Browsermodus. Klicken Sie auf **Extras** ![Schaltfläche Extras](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicherheit** > **InPrivate-Browsermodus**.

## <a name="need-help-contact-support"></a>Benötigen Sie Hilfe? Wenden Sie sich an Support. 

Wenn Sie Hilfe benötigen, [kontaktieren Sie den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , um Ihr Problem gelöst wurde schnell noch benötigen. 