<properties
    pageTitle="Problembehandlung bei Azure Active Directory-Zugriffsprobleme | Microsoft Azure"
    description="Erfahren Sie die Schritte, die Sie zur Lösung von Problemen von Access mit Ihrer Organisation Onlineressourcen ergreifen können."
    services="active-directory"
    keywords="Aktivieren des bedingten auf Gerät-basierten, Gerät Registrierung, Gerät Registrierung, Gerät Registrierung und MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Problembehandlung bei Azure Active Directory-Zugriffsprobleme

Sie versuchen, SharePoint Online Intranet Ihrer Organisation zugreifen, und Sie erhalten eine Fehlermeldung "Zugriff verweigert". Was machst du?

Dieser Artikel behandelt das Schritte zur Wiederherstellung, mit denen Sie Zugriffsprobleme mit Ihrer Organisation Onlineressourcen beheben können.

Um Hilfe zur Lösung Azure Active Directory (Azure AD) Zugriff auf Probleme, wechseln Sie zum Abschnitt im Artikel, die Ihre Geräteplattform behandelt:

-   Windows-Gerät
-   iOS-Gerät (Überprüfen Sie bald wieder Unterstützung mit iPhones und iPads.)
-   Android-Gerät (versuchen Sie es bald Unterstützung für Android-Smartphones und tablets.)

## <a name="access-from-a-windows-device"></a>Zugriff auf einem Windows-Gerät

Wenn Ihr Gerät eine der folgenden Plattformen ausgeführt wird, suchen Sie in den nächsten Abschnitten die Fehlermeldung, die angezeigt wird, wenn Sie versuchen, eine Anwendung oder einen bestimmten Dienst zugreifen:

- Windows-10
- Windows 8.1
- Windows 8
- Windows 7
- WindowsServer 2016
- Windows Server 2012 R2
- WindowsServer 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Gerät ist nicht registriert.

Wenn Ihr Gerät nicht mit Azure AD registriert ist und die Anwendung mit einer Richtlinie Gerät-basierten geschützt ist, wird möglicherweise eine Seite angezeigt, die eine der folgenden Fehlermeldungen angezeigt wird:

!["Es hier erreicht werden kann" für nicht eingetragenen Geräte Nachrichten] (./media/active-directory-conditional-access-device-remediation/01.png "Szenario")

Wenn Ihr Gerät Domäne in Active Directory in Ihrer Organisation ist, versuchen Sie Folgendes:

1.  Stellen Sie sicher, dass Sie bei Windows anmelden mit Konto bei der Arbeit (Ihrer Active Directory-Konto).
2.  Verbinden Sie mit dem Unternehmensnetzwerk über ein virtuelles privates Netzwerk (VPN) oder DirectAccess.
3.  Nachdem Sie verbunden sind, drücken Sie die Windows-Logo-Taste + die L-TASTE, um die Windows-Sitzung zu sperren.
4.  Geben Sie Ihre Arbeit Kontoanmeldeinformationen ein, um die Windows-Sitzung entsperren.
5.  Warten Sie eine Minute um, und versuchen Sie es erneut Zugriff auf die Anwendung oder den Dienst.
6.  Wenn Sie auf derselben Seite sehen, klicken Sie auf den Link **Weitere Details** , und wenden Sie sich an den Administrator, die Details.

Wenn Ihr Gerät nicht Domänenverbund und Windows 10 führt, haben Sie zwei Optionen:

- Ausführen von Azure AD-Verknüpfung
- Fügen Sie Ihres Kontos geschäftlichen oder schulnotizbücher zur Windows hinzu

Informationen darüber, wie diese Optionen unterscheiden finden Sie unter [Verwenden der Windows-10-Geräte in Ihrem Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).

Azure AD teilnehmen abläuft, führen Sie die folgenden Schritte aus, für die Plattform Ihrem Gerät bei ausgeführt wird. (Azure AD-Verknüpfung ist nicht verfügbar in Windows Phones.)

**Aktualisieren von Windows 10 Jahrestag**

1.  Öffnen Sie die **Einstellungen** -app.
2.  Klicken Sie auf **Konten** > **Access Arbeit oder Schule**.
3.  Klicken Sie auf **Verbinden**.
4.  Klicken Sie auf die **Teilnahme an diesem Gerät zu Azure AD**.
5.  In Ihrer Organisation authentifizieren Sie, bereitzustellen Sie mehrstufige Authentifizierung, wenn Sie dazu aufgefordert werden, und klicken Sie dann die Schritte, die angezeigt werden.
6.  Melden Sie sich ab, und melden Sie sich mit Ihrem Konto bei Arbeit.
7.  Versuchen Sie erneut, auf die Anwendung zugreifen.


**Aktualisieren von Windows 10 November 2015**

1.  Öffnen Sie die **Einstellungen** -app.
2.  Klicken Sie auf **System** > **zu**.
3.  Klicken Sie auf die **Teilnahme an Azure AD**.
4.  In Ihrer Organisation authentifizieren Sie, bereitzustellen Sie mehrstufige Authentifizierung, wenn Sie dazu aufgefordert werden, und klicken Sie dann die Schritte, die angezeigt werden.
5.  Melden Sie sich ab, und melden Sie sich mit Ihrem Konto Arbeit (Ihre Azure AD-Konto).
6.  Versuchen Sie erneut, auf die Anwendung zugreifen.

Wenn Ihr Konto geschäftlichen oder schulnotizbücher hinzufügen möchten, führen Sie die folgenden Schritte aus:

**Aktualisieren von Windows 10 Jahrestag**

1.  Öffnen Sie die **Einstellungen** -app.
2.  Klicken Sie auf **Konten** > **Access Arbeit oder Schule**.
3.  Klicken Sie auf **Verbinden**.
4.  In Ihrer Organisation authentifizieren Sie, bereitzustellen Sie mehrstufige Authentifizierung, wenn Sie dazu aufgefordert werden, und klicken Sie dann die Schritte, die angezeigt werden.
5.  Versuchen Sie erneut, auf die Anwendung zugreifen.


**Aktualisieren von Windows 10 November 2015**

1.  Öffnen Sie die **Einstellungen** -app.
2.  Klicken Sie auf **Konten** > **Ihre Konten**.
3.  Klicken Sie auf **Hinzufügen Arbeit oder Schule Konto**.
4.  In Ihrer Organisation authentifizieren Sie, bereitzustellen Sie mehrstufige Authentifizierung, wenn Sie dazu aufgefordert werden, und klicken Sie dann die Schritte, die angezeigt werden.
5.  Versuchen Sie erneut, auf die Anwendung zugreifen.

Wenn Ihr Gerät nicht Domänenverbund und Windows 8.1 führt, führen Sie eine Arbeitsplatz Join-und registrieren in Microsoft Intune, führen Sie die folgenden Schritte aus:

1.  Öffnen Sie die **PC-Einstellungen**.
2.  Klicken Sie auf **Netzwerk** > **Arbeitsplatz**.
3.  Klicken Sie auf die **Teilnahme an**.
4.  In Ihrer Organisation authentifizieren Sie, bereitzustellen Sie mehrstufige Authentifizierung, wenn Sie dazu aufgefordert werden, und klicken Sie dann die Schritte, die angezeigt werden.
5.  Klicken Sie auf **Aktivieren**.
6.  Versuchen Sie erneut, auf die Anwendung zugreifen.


### <a name="browser-is-not-supported"></a>Browser wird nicht unterstützt.

Wenn Sie versuchen, eine Anwendung oder einen bestimmten Dienst zugreifen, indem Sie eine der folgenden Browser, möglicherweise Zugriff verweigert werden:

- Chrome, Firefox oder einen anderen Browser als Microsoft Edge oder Microsoft Internet Explorer 10 für Windows oder Windows Server 2016
- Firefox in Windows 8.1, Windows 7, Windows Server 2012 R2, WindowsServer 2012 oder Windows Server 2008 R2

Sie sehen eine Fehlerseite, die sieht wie folgt aus:

!["Sie können keine es hier abrufen" Nachricht für nicht unterstützte Browser] (./media/active-directory-conditional-access-device-remediation/02.png "Szenario")

Die einzige Behebung besteht darin, einen Browser verwenden, den die Anwendung für Ihre Geräteplattform unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Bedingte Azure Active Directory-Zugriff](active-directory-conditional-access.md)
