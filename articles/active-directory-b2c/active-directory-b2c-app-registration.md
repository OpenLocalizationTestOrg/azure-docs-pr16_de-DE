<properties
    pageTitle="Azure Active Directory B2C: Registrierung Anwendung | Microsoft Azure"
    description="Wie Sie Ihre Anwendung mit Azure Active Directory B2C registrieren"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrieren Sie Ihrer Anwendung

## <a name="prerequisite"></a>Voraussetzung

Zum Erstellen einer Anwendung, die Consumer Anmeldung und-Anmeldung akzeptiert, müssen Sie zuerst die Anwendung mit einem Mandanten Azure Active Directory B2C registrieren. Rufen Sie Ihrer eigenen Mandanten mithilfe der Schritte in [Erstellen einer B2C von Azure AD-Mandanten ab](active-directory-b2c-get-started.md). Nachdem Sie alle Schritte in diesem Artikel folgen, müssen Sie das B2C Features Blade angehefteten zu Ihrer Startboard.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Navigieren Sie zu dem B2C Features blade

Wenn Sie das B2C Features Blade angehefteten zu Ihrer Startboard haben, sehen Sie das Blade, sobald Sie [Azure-Portal](https://portal.azure.com/) als globaler Administrator für den Mandanten B2C anmelden.

Sie können auch das Blade zugreifen, indem Sie auf **Durchsuchen** , **Azure AD B2C** im linken Navigationsbereich auf das [Portal Azure](https://portal.azure.com/).

> [AZURE.IMPORTANT] Sie müssen den Mandanten B2C das B2C Features Blade zugreifen können ein globaler Administrator sein. Von einem beliebigen anderen Mandanten ein globaler Administrator oder ein Benutzer von einem beliebigen Mandanten kann nicht darauf zugreifen.  Sie können mithilfe der Mandanten wechseln in der oberen rechten Ecke des Portals Azure zu Ihrem Mandanten B2C wechseln.

## <a name="register-an-application"></a>Registrieren einer Anwendung

1. Klicken Sie auf das B2C Features Blade Azure-Portal klicken Sie auf **Anwendungen**.
2. Klicken Sie auf **+ Add** am oberen Rand der Blade.
3. Geben Sie einen **Namen** für die Anwendung, in denen die Anwendung Nutzer beschrieben wird. Sie können beispielsweise "Contoso B2C app" eingeben.
4. Wenn Sie eine webbasierte Anwendung schreiben, bringen Sie den Schalter " **Web app einschließen / web-API** " auf **Ja**. Die **Antwort URLs** sind die Endpunkte Azure AD B2C gibt, in dem alle Token zurück, die die Anwendung anfordert. Geben Sie zum Beispiel `https://localhost:44321/`. Wenn Sie die Webanwendung auch einige Web-API durch Azure AD B2C gesicherte zurückgerufen wird, sollten Sie eine **Anwendung geheim** auch erstellen, indem Sie auf die Schaltfläche **Generieren Key** .

    > [AZURE.NOTE] Eine **Anwendung geheim** ist eine wichtige Sicherheitsanmeldeinformationen und ordnungsgemäß gesichert werden sollte.

5. Wenn Sie eine mobile Anwendung schreiben, bringen Sie den **einschließen native Client** -Schalter auf **Ja**. Kopieren Sie nach unten der Standard- **URI umgeleitet** , die automatisch für Sie erstellt wird.
6. Klicken Sie auf **Erstellen** , um eine Anwendung zu registrieren.
7. Klicken Sie auf die Anwendung, die Sie soeben erstellt haben, und kopieren Sie die global eindeutige **Anwendung Client-ID** , die Sie später in Ihrem Code verwenden möchten.

> [AZURE.IMPORTANT] Erstellt in das B2C Features Blade Applikationen müssen an derselben Stelle verwaltet. Wenn Sie B2C bearbeiten Applikationen mithilfe der PowerShell oder ein anderes Portal diese werden nicht unterstützt und funktioniert wahrscheinlich nicht mit Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Erstellen Sie eine Anwendung Schnellstart

Jetzt, da Sie eine Anwendung mit Azure AD B2C registriert haben, können Sie eine der unsere Schnellstart-Lernprogramme zu den Einstieg und Fertig stellen. Hier sind einige Vorschläge:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
