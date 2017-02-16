<properties
    pageTitle="Konfigurieren der automatischen Gerät Registrierung für Windows 8.1-Domäne beitreten Geräte | Microsoft Azure"
    description=" Schritte zum Konfigurieren von Gruppenrichtlinien für Windows 8.1 Domänenverbund Geräte mit Azure AD automatisch registrieren. "
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Konfigurieren der automatischen Gerät Registrierung für Windows 8.1-Domäne beitreten Geräte

Eine Active Directory-Gruppenrichtlinien können so konfigurieren Sie die Geräte Ihrer Windows 8.1-Domäne hinzugefügt, automatisch mit Azure AD zu registrieren. Um die Gruppenrichtlinien konfigurieren zu können, müssen Sie mindestens eine Domäne verknüpfte Windows Server 2012 R2 oder Windows 8.1-Computer mit dem Gruppenrichtlinien-Feature installiert haben. Nachdem Sie diese Gruppenrichtlinien für Ihre Domäne aktiviert ist, wird jedes Benutzers der Domäne, die an den Computer anmeldet automatisch und im Hintergrund mit einem Geräteobjekt in Azure AD registriert sein. Kann ein Geräteobjekt in Azure AD für jeden registrierten Benutzer das physische Gerät. Achten Sie darauf, dass durchlesen, und führen Sie die erforderlichen Komponenten von der automatischen Gerät Registrierung mit Azure Active Directory for Windows Domain-Joined Geräten.

>[AZURE.NOTE]
 Neueste Anweisungen zum Einrichten von automatischen Gerät Registrierung finden Sie unter, [zum Einrichten der automatischen Registrierung von Windows-Domäne beigetreten Geräte mit Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Konfigurieren der Gruppenrichtlinien für Ihre Windows 8.1-Domäne beitreten-Geräte

1. Server-Manager zu öffnen, und navigieren Sie zu **Extras** > **Gruppenrichtlinien-Verwaltungskonsole**.
2. Navigieren Sie zu den Domänenknoten, der die Domäne entspricht, in dem Sie **Automatische Arbeitsplatz teilnehmen**aktivieren möchten, aus der Gruppenrichtlinien-Verwaltungskonsole.
3. Mit der rechten Maustaste in **Die Richtlinienobjekte gruppieren** , und wählen Sie **neu**aus. Benennen Sie Ihre Gruppenrichtlinienobjekt, z. B. **Automatische Arbeitsplatz teilnehmen**. Klicken Sie auf **OK**.
4. Mit der rechten Maustaste auf Ihr neues Gruppenrichtlinienobjekt, und wählen Sie dann auf **Bearbeiten**.
5. Navigieren Sie zur **Konfiguration des Computers** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Arbeitsplatz teilnehmen an**.
6. Mit der rechten Maustaste automatisch in Arbeitsplatz Verknüpfung Clientcomputern, und wählen Sie dann auf **Bearbeiten**.
7. Aktivieren Sie das Optionsfeld aktiviert, und klicken Sie dann auf Übernehmen. Klicken Sie auf **OK**.
8. Sie können jetzt das Gruppenrichtlinienobjekt an einem Speicherort Ihrer Wahl verknüpfen. Um diese Richtlinie für alle Windows 8.1-Domäne beitreten Geräte in Ihrer Organisation zu aktivieren, verknüpfen Sie die Gruppenrichtlinien mit der Domäne aus.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Aufheben der Registrierung von Ihrer Domäne Windows 8.1 beigetreten Geräte

Sie können sich entscheiden aufgehoben werden Ihre Windows 8.1-Geräte-Domäne hinzugefügt, indem Sie wie folgt vorgehen: Ändern von Einstellungen für den Arbeitsplatz teilnehmen Gruppenrichtlinien im vorherigen Abschnitt erstellt haben. Festlegen der automatisch Arbeitsplatz Verknüpfung Computern Clientrichtlinie deaktiviert. Dadurch wird verhindert, dass neue Geräte automatisch Teilnahme am Arbeitsplatz.

Aufheben der Registrierung von der vorhandenen Domäne hinzugefügt Windows 8.1-Computer mithilfe eines der beiden folgenden Optionen:

* Option 1: **Registrierung aufheben einer Windows 8.1-Domäne beigetreten Geräten mit PC-Einstellungen**
  1. Navigieren Sie zu **PC-Einstellungen**auf dem Gerät Windows 8.1 > **Netzwerk** > **Arbeitsplatz**
  2. Wählen Sie **verlassen**aus.
Dieses Verfahren muss für jede Domänenbenutzer wiederholt werden, die in den Computer angemeldet hat und wurde automatisch Arbeitsplatz verknüpft.

* Option 2: Aufheben der Registrierung von einem Windows 8.1 Domäne verknüpfte Gerät mithilfe eines Skripts
    1. Öffnen Sie ein Eingabeaufforderungsfenster auf dem Computer Windows 8.1 und führen Sie den folgenden Befehl aus:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Dieser Befehl muss im Kontext jeder Domänenbenutzer, die signiert hat in dem Computer ausgeführt werden.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Ereignisanzeige und Fehler für Windows 8.1-Domäne beitreten-Geräte

Windows-Ereignisprotokoll auf einem Computer Windows 8.1 zeigt Nachrichten, die im Zusammenhang mit dem Gerät Registrierung. Für erfolgreiche und nicht erfolgreiche Ereignisse finden Sie Nachrichten. 

Im Ereignisprotokoll finden Sie im Viewer unter Anwendungen und Dienste **Protokolle** > **Microsoft** > **Windows > Arbeitsplatz teilnehmen**.

##<a name="additional-details"></a>Weitere details

Die Gruppenrichtlinien ermöglichen einen Vorgang geplant System, die in dem Kontext des Benutzers ausgeführt wird, und klicken Sie auf Benutzer anmelden ausgelöst. Die Aufgabe wird im Hintergrund der Benutzer und Geräte mit Azure AD nach der Anmeldung Abschluss registrieren. Der Vorgang geplant befindet sich auf Windows 8.1-Geräte in der Aufgabenplanungsbibliothek unter **Microsoft** > **Windows** > **Arbeitsplatz teilnehmen**. Die Aufgabe wird ausgeführt und alle Active Directory-Benutzer zu registrieren, melden Sie sich in. 

## <a name="additional-topics"></a>Weitere Themen
- [Azure-Active Directory-Gerät Registrierung (Übersicht)](active-directory-conditional-access-device-registration-overview.md)
- [Automatische Gerät Registrierung mit Azure Active Directory für Windows 10 Domänenverbund Geräte](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurieren der automatischen Gerät Registrierung für Windows 7-Domäne beitreten-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)

