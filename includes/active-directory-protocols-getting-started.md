<properties
    pageTitle="Azure AD-.NET Protokoll Übersicht | Microsoft Azure"
    description="Informationen zum HTTP-Nachrichten zu verwenden, um die Autorisierung des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten Azure AD-."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/21/2016"
    ms.author="priyamo"/>

<!--TODO: Introduction -->

## <a name="register-your-application-with-your-ad-tenant"></a>Registrieren Sie die Anwendung mit Ihrem AD-Mandanten

Zuerst müssen Sie Ihre Anwendung mit Ihrem Active Directory-Mandanten registrieren. Dies wird bieten Ihnen eine Client-ID für eine Anwendung, als auch Aktivieren der Arbeitsmappe Token zu erhalten.

- Melden Sie sich bei der Azure-Verwaltungsportal.

- Klicken Sie in der linken Navigationsleiste auf **Active Directory**.

- Wählen Sie einen Mandanten, in dem die Anwendung registrieren aus.

- Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Einzug auf **Hinzufügen** .

- Erstellen einer neuen Anwendung, und folgen Sie den Anweisungen. Es unerheblich, ob sie eine Webanwendung oder eine systemeigene Anwendung in diesem Lernprogramm ist nicht, aber Belieben Sie bestimmte Beispiele für Webanwendungen oder systemeigenen Applications, sehen Sie sich unsere Schnellstart [hier](../articles/active-directory/active-directory-developers-guide.md).

- Für Webanwendungen, bieten **Anmelden URL** die Basis der app-URL ist, in dem Benutzer melden können in z. B. `http://localhost:12345`. Der **App-ID-URI** ist ein eindeutiger Bezeichner für eine Anwendung. Ist die Verwendung der Messe `https://<tenant-domain>/<app-name>`, z. B.`https://contoso.onmicrosoft.com/my-first-aad-app`

- Systemeigene Anwendungsmöglichkeiten Bereitstellen eines **URI umleiten**, womit Azure AD token Antworten zurückgeben. Geben Sie einen bestimmten Wert an Ihrer Anwendung,. z.B.`http://MyFirstAADApp`

- Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihrer Anwendung eine eindeutige Client-ID. Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie auf der Registerkarte **Konfigurieren** Ihrer Anwendung.
