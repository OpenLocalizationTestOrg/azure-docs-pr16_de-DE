<properties
    pageTitle="Festlegen von Gerät-basierten bedingte Richtlinien für Applikationen Azure Active Directory verbunden | Microsoft Azure"
    description="Festlegen Sie bedingte Access Gerät basierende Richtlinien für Azure AD-verbunden Applications."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Festlegen von Gerät-basierten bedingte Richtlinien für Applikationen Azure Active Directory verbunden


Azure Active Directory (Azure AD) Gerät-basierten bedingten Zugriff ermöglicht Ihnen ein schützen Organisation Ressourcen aus:

- Access versucht aus unbekannten oder nicht verwalteten Geräte.
- Geräte, die nicht die Sicherheitsrichtlinien Ihrer Organisation entsprechen.

Übersicht über bedingte Access finden Sie unter [bedingte Azure Active Directory-Zugriff](active-directory-conditional-access.md).

Sie können bedingte Access Gerät basierende Richtlinien für den diesen Anwendungen schützen festlegen:

- Office 365 SharePoint Online in Ihrer Organisation Websites und Dokumente zu schützen
- Office 365 Exchange Online, zum Schützen Ihrer Organisation e-Mails
- Software als dienstanwendungen (SaaS), die mit Azure AD für Authentifizierung verbunden sind
- Lokale Anwendungen, die mithilfe von Azure AD-Anwendungsproxy-Dienste veröffentlicht werden

Wechseln Sie zum Festlegen einer Richtlinie bedingte Access-basierten Gerät in der Azure-Portal zu der jeweiligen Anwendung im Verzeichnis.


  ![Liste der Anwendungen im Portal Azure-Verzeichnis] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Applikationen")


Wählen Sie die Anwendung, und klicken Sie dann auf die Registerkarte **Konfigurieren** , um die bedingten Zugriffsrichtlinie festzulegen.  


  ![Konfigurieren der Anwendung] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Gerät basierenden Access-Regeln")




Wählen Sie zum Festlegen einer Richtlinie Gerät-basierten bedingte Zugriff im Abschnitt **Gerät basierenden Access Regeln** in **Access-Regeln aktivieren**, **Klicken Sie auf**.

Gerät-basierten bedingte Zugriffsrichtlinie besteht aus drei Komponenten:

- **Zum Anwenden**. Der Bereich der Benutzer aus, die, denen auf die Richtlinie angewendet wird.

- **Geräteregeln**. Die Bedingung muss ein Gerät genügen, bevor sie die Anwendung zugreifen kann.

- **Erzwingen der Anwendung**. Die Clientanwendungen (einheitlichen und im Browser) gilt die Richtlinie.

  ![Die drei Komponenten einer Access - basierten Gerät Richtlinie] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Gerät basierenden Access-Regeln")


## <a name="select-the-users-the-policy-applies-to"></a>Wählen Sie die Benutzer, die, denen auf die Richtlinie angewendet wird

Im Abschnitt **Anwenden** können Sie können den Umfang der Benutzer auswählen, die, denen dieser Richtlinie angewendet wird.

Beim Erstellen einer Access-Richtlinie Bereichs für Benutzer, stehen Ihnen zwei Optionen zur Verfügung:

- **Alle Benutzer**. Wenden Sie die Richtlinie für alle Benutzer mit Zugriff auf die Anwendung.

- **Gruppen**. Beschränken Sie die Richtlinie für Benutzer, die eine bestimmte Gruppe angehören.

![Übernehmen der Richtlinie für alle Benutzer oder einer Gruppe] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Anwenden auf")


 Wählen Sie das Kontrollkästchen **mit Ausnahme** zum Ausschließen eines Benutzers aus einer Richtlinie aus. Dies ist hilfreich, wenn Sie Berechtigungen erteilen, einen bestimmten Benutzer auf die Anwendung vorübergehend zugreifen müssen. Wählen Sie diese Option aus, z. B., wenn einige Benutzer Geräte verfügen, die nicht für bedingten Zugriff bereit sind. Die Geräte möglicherweise noch nicht registriert, oder sie sind, verstößt werden soll.


## <a name="select-the-conditions-that-devices-must-meet"></a>Wählen Sie die Konditionen, die Geräte erfüllt werden müssen

Verwenden von **Regeln Gerät** Bedingungen ein Gerät festlegen müssen Zugriff auf die Anwendung entsprechen.

Sie können bedingte Access-basierten Gerät für diese Dateitypen Gerät festlegen:

- Windows 10 Jahrestag Update, Windows 8.1 und Windows 7
- WindowsServer 2016, Windows Server 2012 R2, Windows Server 2008 R2 und WindowsServer 2012
- iOS-Geräte (iPad, iPhone)
- Android-Geräten

Unterstützung für Mac wird in Kürze zur Verfügung.

  ![Übernehmen Richtlinie Geräte] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Applikationen")

 >[AZURE.NOTE] Informationen zu den Unterschieden zwischen Domänenverbund und Azure AD-beigetreten Geräten finden Sie unter [Verwenden der Windows-10-Geräte in Ihrem Arbeitsplatz](active-directory-azureadjoin-windows10-devices.md).

Sie haben zwei Optionen für das Geräteregeln:

- **Alle Geräte kompatibel sein müssen**. Alle Plattformen, die Zugriff auf die Anwendung müssen kompatibel sein. Zugriff verweigert Geräte, die auf Plattformen ausgeführt werden, die bedingte Access-basierten Gerät nicht unterstützen.

- **Nur die ausgewählte Geräten kompatibel sein müssen**. Nur bestimmte Geräteplattformen müssen kompatibel sein. Zugang für andere Plattformen oder andere Plattformen, die die Anwendung zugreifen können.

  ![Legen Sie den Bereich für Geräteregeln] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Applikationen")

Azure AD-beigetreten Geräte sind kompatibel, wenn sie als **kompatible** im Verzeichnis markiert sind, indem Sie Intune oder ein Mobilgerät Drittanbieters Management-System, die mit Azure AD integriert.

Ein Gerät Domänenverbund ist kompatibel ist:

- Es ist in Azure Active Directory registriert. Viele Organisationen behandeln Domänenverbund Geräte als vertrauenswürdig Geräte.
- Es wird als **kompatibel** in Azure AD durch System Center-Konfigurations-Manager 2016 gekennzeichnet.

 ![Domänenverbund Geräte, die kompatibel sind.] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Geräteregeln")

Persönliche Windows-Geräten sind kompatibel, wenn sie als **kompatible** im Verzeichnis markiert sind, indem Sie Intune oder ein Mobilgerät Drittanbieters Management-System, die mit Azure AD integriert.

Geräten ohne Windows sind kompatibel, wenn sie als **kompatible** im Verzeichnis von Intune markiert sind.

 >[AZURE.NOTE] Weitere Informationen zum Einrichten von Azure AD für Gerät Compliance in verschiedenen Management-Systeme finden Sie unter [bedingte Azure Active Directory-Zugriff](active-directory-conditional-access.md).


Sie können eine oder mehrere Plattformen für ein Gerät-basierten Zugriffsrichtlinie auswählen. Dies umfasst, Android, iOS, (Windows 8.1 Telefonen und Tablets) unter Windows Mobile und Windows (alle anderen Windows-Geräten, einschließlich aller Windows-10-Geräte).
Auswertung der Richtlinie erfolgt nur auf den ausgewählten Plattformen. Wenn ein Gerät, das Zugriff auf eine der ausgewählten Plattformen nicht ausgeführt wird, kann das Gerät die Anwendung zugreifen, wenn der Benutzer zugreifen kann. Keine Geräterichtlinie ausgewertet wird.

![Wählen Sie Plattformen für Geräteregeln] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Geräteregeln")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Festlegen der Auswertung von Richtlinien für einen Typ von Anwendung

Legen Sie im Abschnitt **Anwendung Durchsetzung** Anwendungstyp, die die Richtlinie für Benutzer oder Gerät Access ausgewertet wird.

Sie haben zwei Optionen für den Typ der Anwendung aufnehmen möchten:

- Im Browser und in Formaten
- Nur systemeigene Applikationen

![Browser auswählen oder Formaten] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Applikationen")

Um für Applikationen-Richtlinien zu erzwingen, wählen Sie **für die im Browser und in einer systemeigenen Applications**. Dann können Sie einbeziehen:

- Andere Browser (z. B. Microsoft Edge in Windows 10) oder in iOS Safari.
- Programme, die die Bibliothek Authentifizierung von Active Directory (ADAL) auf einer beliebigen Plattform verwenden, oder verwenden Sie, die die WebAccountManager (WAM) API in Windows 10.

>[AZURE.NOTE] Weitere Informationen zu Browserunterstützung oder andere Aspekte für einen Benutzer, die Zugriff auf ein Gerät-basierten ist Zertifikat Zertifizierungsstelle geschützte Anwendung, finden Sie unter [bedingte Azure Active Directory-Zugriff](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>Schützen von e-Mail-Zugriff von Exchange ActiveSync-basierten Anwendungen

In Office 365 Exchange Online-Clientanwendungen können Sie Exchange ActiveSync-e-Mail-Zugriff auf Exchange ActiveSync-basierten e-Mail-Programme blockieren.

![Exchange ActiveSync Compliance-Optionen] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Applikationen")

![Benötigen Sie ein kompatibles Gerät zu-e-Mails] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Applikationen")


## <a name="next-steps"></a>Nächste Schritte

- [Bedingte Azure Active Directory-Zugriff](active-directory-conditional-access.md)
