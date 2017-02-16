<properties
    pageTitle="Azure kombinierte Authentifizierung häufig gestellte Fragen"
    description="Enthält eine Liste von häufig gestellte Fragen und Antworten, die im Zusammenhang mit Azure kombinierte Authentifizierung. Mehrstufige Authentifizierung ist eine Methode zum Überprüfen der Identität des Benutzers, die mehr als einen Benutzernamen und ein Kennwort erforderlich sind. Es stellt eine zusätzliche Sicherheitsebene Benutzer anmelden und Transaktionen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Azure kombinierte Authentifizierung häufig gestellte Fragen


Hier finden Sie Antworten auf häufige gestellte Fragen zu Azure kombinierte Authentifizierung und den kombinierte Authentifizierung-Dienst verwenden, einschließlich Fragen zur Abrechnung Modell und Nutzbarkeit.

## <a name="general"></a>Allgemeine

**F: wie behandelt Azure mehrstufige Authentifizierungsserver Benutzerdaten?**

Benutzerdaten werden mit mehrstufige Authentifizierungsserver nur auf den lokalen Servern gespeichert. Keine beständigen Benutzerdaten in der Cloud gespeichert. Wenn der Benutzer in zwei Schritten Überprüfung ausführt, sendet mehrstufige Authentifizierungsserver Daten in der Cloud-Service Azure kombinierte Authentifizierung für die Authentifizierung. Kommunikation zwischen mehrstufige Authentifizierungsserver und kombinierte Authentifizierung Cloud-Dienst verwendet Secure Sockets Layer (SSL) oder Transport Layer Security (TLS) über den Port 443 ausgehende an.

Berichte beim Authentifizierungsanfragen gesendet werden in der Cloud-Service Daten für die Authentifizierung und die Verwendung erfasst werden. Im Lieferumfang von zwei Überprüfung Protokolle Datenfelder sind wie folgt aus:

- **Einmalige Nr.** (entweder Benutzer Namen oder lokalen mehrstufige Authentifizierung Server-ID)
- **Vor-und Nachname** (optional)
- **E-Mail-Adresse** (optional)
- **Rufnummer** (Wenn eine VoIP-Anruf oder SMS-Authentifizierung verwendet)
- **Gerät Token** (bei Verwendung von mobile-app-Authentifizierung)
- **Authentifizierungsmodus**
- **Ergebnis der Authentifizierung**
- **Mehrstufige Authentifizierung-Servernamens**
- **Mehrstufige Authentifizierungsserver IP**
- **Client-IP-** (falls vorhanden)

Optionaler Felder können in mehrstufige Authentifizierungsserver konfiguriert sein.

Das Ergebnis Überprüfung (Erfolg oder Ablehnung) und den Grund, wenn es verweigert wurde, wird mit der Authentifizierungsdaten gespeichert und steht in Authentifizierung und die Verwendung von Berichten.


## <a name="billing"></a>Abrechnung

Die meisten Fragen können beantwortet werden, indem Sie auf der [Seite mehrstufige Authentifizierung Preise](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)verweisen.

**F: Dient Mein Unternehmen für Telefonanrufe oder Textnachrichten belastet zum meiner Benutzer authentifiziert?**

Organisationen unterliegen nicht für einzelne Telefonanrufe platziert oder Text über Azure kombinierte Authentifizierung für Benutzer gesendete Nachrichten. Telefon Besitzer können für die Telefonanrufe oder Textnachrichten, die sie, die entsprechend ihren persönlichen Telefondienst erhalten in Rechnung gestellt werden.

**F: kann die pro Benutzer Abrechnung Modell Gebühr basierend auf die Anzahl der Benutzer, die für die Verwendung der kombinierte Authentifizierung konfiguriert sind oder die Anzahl der Benutzer, die Durchführung werden?**

Abrechnung basiert auf die Anzahl der Benutzer, die so konfiguriert, dass die kombinierte Authentifizierung verwenden.

**F: wie funktioniert die kombinierte Authentifizierung Abrechnung?**

Bei Verwendung der "pro Benutzer" oder "pro Authentifizierung" ist das Modell, Azure MFA eine Ressource Verbrauch-basierten. Für die Organisation Azure-Abonnement ebenso wie virtuellen Computern, Websites, usw. werden alle Gebühren belastet.

Wenn Sie das Lizenzmodell verwenden, Azure kombinierte Authentifizierung Lizenzen gekauft und dann Benutzern zugewiesen, genau wie für Office 365 und andere Produkte-Abonnement.

**F: Gibt es eine kostenlose Version Azure kombinierte Authentifizierung für Administratoren?**

In einigen Fällen Ja. Mehrstufige Authentifizierung für Administratoren Azure bietet eine Teilmenge der Azure MFA Features kostenlos. Dieses Angebot gilt für Mitglieder der Gruppe Azure globale Administratoren in Azure Active Directory-Instanzen, die mit einem Verbrauch-basierten Azure kombinierte Authentifizierung Anbieter verknüpft sind. Mithilfe eines Anbieters kombinierte Authentifizierung aktualisiert alle Administratoren und Benutzer im Verzeichnis, die für die kombinierte Authentifizierung, verwenden Sie die Vollversion von Azure kombinierte Authentifizierung konfiguriert sind.

**F: Gibt es eine kostenlose Version Azure kombinierte Authentifizierung für Benutzer von Office 365?**

In einigen Fällen Ja. Mehrstufige Authentifizierung für Office 365 bietet eine Teilmenge der Azure MFA Features kostenlos. Dieses Angebot gilt für Benutzer, die über eine zugewiesen werden, wenn ein Verbrauch-basierten Azure kombinierte Authentifizierung Identitätsanbieter nicht auf die entsprechende Azure Active Directory-Instanz verknüpft wurde Office 365-Lizenz verfügen. Mit dem Anbieter kombinierte Authentifizierung aktualisiert alle Administratoren und Benutzer im Verzeichnis, die für die kombinierte Authentifizierung, verwenden Sie die Vollversion von Azure kombinierte Authentifizierung konfiguriert sind.

**F: wechseln kann meine Organisation zwischen pro Benutzer und pro Authentifizierung Abrechnung Verbrauch-Modellen zu einem beliebigen Zeitpunkt?**

Ihrer Organisation wählt ein des Modells aus, wenn es sich um eine Ressource erstellt. Sie können ein des Modells nicht ändern, nachdem die Ressourcen bereitgestellt wird. Sie können jedoch, eine andere kombinierte Authentifizierung Ressource um der ursprünglichen Nachricht erstellen. Benutzereinstellungen und Konfigurationsoptionen können nicht in die neue Ressource übertragen werden.

**F: wechseln kann meine Organisation zu einem beliebigen Zeitpunkt zwischen dem Verbrauch Abrechnung und Lizenzmodell?**

Wenn Lizenzen ein Verzeichnis, die bereits einen pro Benutzer Azure kombinierte Authentifizierung Anbieter hinzugefügt werden, wird Verbrauch-basierten Abrechnung um die Anzahl der Lizenzen, die im Besitz verringert. Wenn alle Benutzer, die so konfiguriert ist, verwenden Sie die kombinierte Authentifizierung Lizenzen zugewiesen haben, kann der Administrator den Anbieter Azure kombinierte Authentifizierung löschen.

Sie können keine-Authentifizierung Verbrauch Abrechnung mit einem Lizenzmodell mischen. Wenn ein-Authentifizierung kombinierte Authentifizierung Identitätsanbieter in ein Verzeichnis verknüpft ist, wird die Organisation für alle kombinierte Authentifizierung Überprüfung Anfragen, unabhängig davon, noch im Besitz Lizenzen in Rechnung gestellt.

**F: verfügt Mein Unternehmen und Synchronisieren von Identitäten um Azure mehrstufige Authentifizierung verwenden?**

Wenn eine Organisation ein Verbrauch basierendes Abrechnung Modell verwendet wird, ist Azure Active Directory nicht erforderlich. Verknüpfen eines Anbieters kombinierte Authentifizierung in ein Verzeichnis ist optional. Wenn Ihre Organisation nicht um ein Verzeichnis verknüpft ist, können sie Azure mehrstufige Authentifizierungsserver oder die Azure mehrstufige Authentifizierung SDK lokal bereitstellen.

Azure-Active Directory ist für das Lizenzmodell erforderlich, da Lizenzen zum Verzeichnis hinzugefügt werden, wenn Sie erwerben und Benutzer im Verzeichnis zuweisen.


## <a name="usability"></a>Nutzbarkeit

**F: wie geht ein Benutzers vor, wenn sie eine Antwort auf ihrem Telefon erhalten nicht oder das Telefon nicht für den Benutzer verfügbar ist?**

Wenn der Benutzer eine Sicherung Telefon konfiguriert hat, sollten diese versuchen Sie es erneut und wählen die Telefon, wenn Sie auf der Anmeldeseite aufgefordert werden. Wenn der Benutzer eine andere Methode konfiguriert hat, kann Administrator der Organisation die Nummer des Benutzers primäre Rufnummer aktualisieren.


**F: wie funktioniert der Administrator? Wenn ein Benutzer den Administrator über ein Konto Kontakte, die der Benutzer nicht mehr zugreifen kann**

Der Administrator kann des Benutzers zum Zurücksetzen des Kontos Sie sich erneut über die Registrierung zu wechseln. Weitere Informationen zum [Verwalten von Benutzer- und Einstellungen für Audiogeräte mit Azure kombinierte Authentifizierung in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: wie funktioniert ein Administrator? bei Verlust oder Diebstahl eines Benutzers Telefon, die app Kennwörter verwendet wird**

Der Administrator kann des Benutzers app Kennwörter, um unbefugten Zugriff zu verhindern löschen. Nachdem der Benutzer ein Ersatz Gerät verfügt, kann der Benutzer die Kennwörter neu erstellen. Weitere Informationen zum [Verwalten von Benutzer- und Einstellungen für Audiogeräte mit Azure kombinierte Authentifizierung in der Cloud](multi-factor-authentication-manage-users-and-devices.md).

**F: Was passiert, wenn der Benutzer nicht Browser-apps nicht bei anmelden kann?**

Ein Benutzer, die so konfiguriert ist, verwenden Sie kombinierte Authentifizierung erfordert eine app Kennworts anmelden bei einigen nicht-Browser-apps. Ein Benutzer muss deaktivieren (löschen) Anmeldung Informationen und starten Sie die app, melden Sie sich mit ihrer Benutzer Namen und app Kennwort.

Erhalten Sie weitere Informationen zum Erstellen von app-Kennwörter und andere [mit Kennwörtern app Hilfe](multi-factor-authentication-end-user-app-passwords.md)


>[AZURE.NOTE] Modernes Authentifizierung für Office 2013-clients
>
> Office 2013-Clients (einschließlich Outlook) unterstützt neue Protokolle für die Authentifizierung. Sie können Office 2013 zur Unterstützung von kombinierte Authentifizierung konfigurieren. Nachdem Sie Office 2013 konfiguriert haben, sind app Kennwörter nicht für Office 2013-Clients erforderlich. Weitere Informationen finden Sie in der [Office 2013 moderne Authentifizierung public Preview-Version von Abitur](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**F: wie geht ein Benutzers vor, wenn der Benutzer eine Textnachricht nicht, oder wenn der Benutzer Antworten auf eine bidirektionale Textnachricht, aber die Überprüfung Timeout erreicht?**

Vorführen der Textnachrichten und Bestätigung der Antworten in bidirektionale SMS ist nicht gewährleistet, da kann Faktoren, die die Zuverlässigkeit des Diensts beeinflussen möglicherweise vorhanden sind. Zu diesen Faktoren zählen angegebene Land, die auf dem Mobiltelefon Carrier und das Signal schwach.

Benutzer, die Probleme beim Empfangen von Textnachrichten zuverlässig auftreten sollten die mobile-app oder Anruf Methode stattdessen auswählen. Die mobile-app kann Benachrichtigungen sowohl über Mobiltelefon und WLAN-Verbindungen erhalten. Darüber hinaus kann die mobile app Überprüfung Codes generieren, auch wenn das Gerät überhaupt kein Signal hat. Die app Microsoft Authenticator steht für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Wenn Sie Textnachrichten verwenden müssen, empfehlen wir unidirektionale SMS statt bidirektionale SMS, falls möglich. Unidirektionale SMS ist mehr zuverlässig und verhindert, dass Benutzer dabei globale SMS Gebühren Antworten an eine Textnachricht, die von einem anderen Land gesendet wurde.


**F: verwenden kann ich Hardwaretoken mit Azure mehrstufige Authentifizierungsserver?**

Wenn Sie Azure mehrstufige Authentifizierungsserver verwenden, können Sie von Drittanbietern öffnen Authentifizierung (EID) zeitbasierter, einmalige Kennwort (TOTP) Token importieren, und verwenden Sie diese dann zur Bestätigung in zwei Schritten.

Sie können ActiveIdentity Token verwenden, die Angehörigen TOTP Token sind, wenn Sie die geheime Key-Datei in eine CSV-Datei setzen und auf Azure mehrstufige Authentifizierungsserver importieren. Sie können Angehörigen Token mit Active Directory Federation Services (ADFS), Remote-Authentifizierung einwählen User Service (RADIUS), wenn das Clientsystem Access Herausforderung Antworten verarbeitet werden kann, und Internet Information Server (IIS) formularbasierte Authentifizierung verwenden.

Sie können Drittanbieter-Angehörigen TOTP Token mit den folgenden Formaten importieren:  
- Portable Symmetric Key Container (PSKC)  
- CSV-Datei eine fortlaufende Zahl, einen geheimen Schlüssel im Format Base 32 und ein Zeitintervall enthält  

**F: verwenden kann ich Azure mehrstufige Authentifizierungsserver Terminal Services gesichert?**

Ja, aber wenn Sie mithilfe von Windows Server 2012 R2 oder höher nur von Remote Desktop Gateway (RD Gateway).

Sicherheit Änderungen in Windows Server 2012 R2 haben die Möglichkeit geändert, die Azure mehrstufige Authentifizierungsserver und der lokalen Zertifizierungsstelle (LSA) Sicherheitspaket in Windows Server 2012 und früheren Versionen besteht. Für Versionen von Terminal Services in Windows Server 2012 oder einer früheren Version können Sie die [Anwendung mit Windows-Authentifizierung zu sichern](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Wenn Sie Windows Server 2012 R2 arbeiten, benötigen Sie RD-Gateway.

**F: Warum würde ein Benutzer einen Anruf kombinierte Authentifizierung von anonymen Anrufer erhalten, nach dem Einrichten der Anrufer-ID?**

Mehrstufige Authentifizierung Anrufe über das öffentliche Telefonnetz platziert werden, werden manchmal sie über eine Carrier weitergeleitet, die Anrufer-ID nicht unterstützt Anrufer-ID wird aus diesem Grund nicht unbedingt, obwohl das System kombinierte Authentifizierung immer sendet.


## <a name="errors"></a>Fehler

**F: Was sollten Benutzer tun, wenn sie eine Fehlermeldung "Authentifizierung Anforderung ist nicht für ein Konto aktivierten" angezeigt, wenn mobile-app-Benachrichtigungen verwendet?**

Fordern Sie ihn auf führen Sie diese Schritte aus, um ihr Konto aus der mobilen app zu entfernen, und dann erneut hinzufügen:

1. Wechseln Sie zu [Ihrem Azure Portals Profil](https://account.activedirectory.windowsazure.com/profile/) , und melden Sie sich mit dem organisationskonto an.
2. Wählen Sie **zusätzliche Sicherheit Überprüfung**aus.
4. Entfernen Sie die vorhandenen Kontos aus der mobilen app aus.
5. Klicken Sie auf **Konfigurieren**, und folgen Sie dann die Anweisungen, um die mobile-app neu zu konfigurieren.


**F: Was sollten Benutzer tun, wenn eine 0x800434D4L Fehlermeldung beim Anmelden bei einer nicht-Browser-Anwendungs angezeigt?**

Derzeit kann ein Benutzer zusätzliche Sicherheit Überprüfung verwenden, nur für Anwendungen und Dienste, die der Benutzer über einen Browser zugreifen kann. Mit Konten, die benötigen zusätzliche Sicherheit Überprüfung, funktioniert nicht ohne Browser Applications (auch als *Rich-Clientanwendungen*bezeichnet), die auf einem lokalen Computer, wie etwa Windows PowerShell installiert werden. In diesem Fall möglicherweise der Benutzer die Anwendung, einen Fehler 0x800434D4L generieren angezeigt.

Problemumgehung für Dies ist, dass Benutzer separate Konten für Admin-bezogene und Administratorberechtigung Vorgänge. Später können Sie Postfächer zwischen Ihrem Administrator und Administratorberechtigung Konto verknüpfen, sodass Sie in Outlook mit Ihrem Konto Administratorberechtigung anmelden können. Für weitere Details, erfahren Sie, wie Sie [gewähren einem Administrator die Möglichkeit zum Öffnen und Anzeigen des Inhalts eines Benutzerpostfachs](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie hier Ihre Frage beantwortet wird, lassen Sie sie in den Kommentaren am unteren Rand der Seite. Oder hier sind einige zusätzliche Optionen zum Aufrufen der Hilfe:


**F: Wie erhalte ich Hilfe mit Azure kombinierte Authentifizierung?**

- Suchen Sie in der [Microsoft Support Knowledge Base](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) für Lösungen für häufige technische Probleme.

- Suchen nach und technischen Fragen und Antworten aus der Community durchsuchen oder eigene Frage in den [Foren Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Wenn Sie ein älteres PhoneFactor-Kunde sind, und Sie Fragen haben oder benötigen Sie Hilfe beim Zurücksetzen eines Kennworts, verwenden Sie den Link [Kennwort zurücksetzen](mailto:phonefactorsupport@microsoft.com) , um eine Supportanfrage zu öffnen.

- Wenden Sie sich an einem Supportmitarbeiter durch die [Unterstützung von Azure mehrstufige Authentifizierungsserver (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Wenn Sie uns wenden, ist es hilfreich, wenn Sie so viele Informationen über das Problem möglichst aufnehmen können. Informationen, die Sie angeben können, enthält die Seite, die Sie gesehen, in dem haben der Fehler, den spezifischen Fehlercode, die ID für eine bestimmte Sitzung und die ID des Benutzers, dem den Fehler gesehen haben.
