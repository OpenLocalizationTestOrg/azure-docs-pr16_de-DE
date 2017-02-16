<properties
    pageTitle="Version 2.0 app Registrierung | Microsoft Azure"
    description="Wie Sie eine app mit Microsoft registrieren, für die Aktivierung der Anmeldung und den Zugriff auf Microsoft-Diensten, die mit den Endpunkt Version 2.0"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Wie Sie eine app mit den Endpunkt Version 2.0 registrieren

Um einer app zu erstellen, die sowohl MSA und Azure AD-akzeptiert anmelden, müssen Sie zuerst eine app bei Microsoft registrieren.  Zu diesem Zeitpunkt können Sie nicht mehr alle vorhandenen apps verwenden, möglicherweise mit Azure AD, oder MSA – Sie müssen einen ganz neuen erstellen.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Besuchen Sie das Microsoft-app Registrierung-portal
Wichtigste navigieren Sie zuerst - zu [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dies ist eine neue app Registrierung-Portal, in dem Sie Ihr Microsoft-apps verwalten können.

Melden Sie sich mit entweder eine persönliche, Arbeit oder Schule Microsoft-Konto.  Wenn Sie entweder besitzen, melden Sie sich für ein neues persönlichen Konto an. Fahren Sie fort, dauert nicht lange – wir hier warten.

Geschehen? Sie sollten jetzt Suchen in der Liste der Microsoft-apps, welche wahrscheinlich leer ist.  Lassen Sie uns ändern.

Klicken Sie auf **app hinzufügen**, und geben sie einen Namen ein.  Im Portal wird Ihre app eine global eindeutige Id für die Anwendung zuweisen, die Sie später in Ihrem Code verwenden möchten.  Wenn Ihre app eine serverseitige Komponente enthält benötigt, die Access-Token für Aufrufen von APIs (denken: Office, Azure oder eigene Web-API), wird auch hier eine **Anwendung geheim** erstellen möchten.
<!-- TODO: Link for app secrets -->

Fügen Sie die Plattformen, der Ihre app verwendet werden.

- Bieten Sie für Web-basierte apps eine **URI umgeleitet** , in dem Anmeldung Nachrichten gesendet werden kann.
- Kopieren Sie für mobile-apps ab dem standardmäßigen umleiten Uri automatisch für Sie erstellt.

Optional können Sie das Aussehen und Verhalten Ihrer Anmeldung Seite im Abschnitt Profile anpassen.  Vergewissern Sie sich auf **Speichern** klicken, bevor Sie fortfahren.

> [AZURE.NOTE] Wenn Sie eine Anwendung mit [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)erstellen, wird die Anwendung in den Mandanten Start des Kontos registriert, die Sie verwenden, melden Sie sich bei dem Portal.  Dies bedeutet, dass eine Anwendung nicht in Ihrem Azure AD-Mandanten mit einem persönlichen Microsoft-Konto registriert werden können.  Wenn Sie explizit eine Anwendung in einem bestimmten Mandanten registrieren möchten, melden Sie sich mit einem Konto in dieser Mandanten erstellt.

## <a name="build-a-quick-start-app"></a>Erstellen einer app Schnellstart
Jetzt, da Sie eine app für Microsoft verfügen, können Sie eine der unsere Version 2.0 Schnellstart-Lernprogramme fertig stellen.  Hier sind einige Vorschläge:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
