<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Vertrieb | Microsoft Azure"
    description="Erfahren Sie, wie Vertrieb mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Lernprogramm: Wie Vertrieb mit Azure Active Directory integriert werden soll.

In diesem Lernprogramm erfahren Sie, wie Verbindung Ihrer Umgebung Vertrieb zu Ihrer Azure-Active Directory. So konfigurieren Sie einmaliges Anmelden Vertrieb, zum Aktivieren der automatischen Benutzer bereitgestellt und So weisen Sie Benutzern den Zugriff auf Vertrieb lernen.

##<a name="prerequisites"></a>Erforderliche Komponenten

1. Um Azure Active Directory über das [Azure klassischen Portal](https://manage.windowsazure.com)zugreifen zu können, müssen Sie zuerst ein gültiges Azure-Abonnement verfügen.

2. Sie müssen einen gültigen Mandanten in [Salesforce.com](https://www.salesforce.com/).

> [AZURE.IMPORTANT] Wenn Sie **ein Testkonto Salesforce.com** verwenden, werden dann Sie nicht konfiguriert Automatisches Benutzer bereitgestellt werden. Testkonten verfügen nicht den erforderlichen API Zugriff aktiviert, bis sie erworben wurden.
> 
> Sie können diese Einschränkung umgehen, mithilfe einer [kostenlosen Entwicklertools Konto](https://developer.salesforce.com/signup) zum Bearbeiten dieses Lernprogramms.

Wenn Sie eine Vertrieb Sandkasten-Umgebung verwenden, finden Sie unter den [Vertrieb Sandkasten-Integration Lernprogramm](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Video-Lernprogramme

Führen Sie in diesem Lernprogramm verwenden die folgenden Videos.

**Video-Lernprogramm Teil 1: Aktivieren der einmaliges Anmelden**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Video-Lernprogramm Teil 2: Wie automatisieren Benutzer bereitgestellt**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Schritt 1: Hinzufügen von Vertrieb zum Verzeichnis

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)auf der linken Navigationsbereich auf **Active Directory**.

    ![Wählen Sie im linken Navigationsbereich aus Active Directory.][0]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis, dem Sie hinzufügen, auf Vertrieb möchten.

3. Klicken Sie auf im oberen Menü on **Applications** .

    ![Klicken Sie auf on Applications.][1]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Klicken Sie auf Hinzufügen, um eine neue Anwendung hinzuzufügen.][2]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Klicken Sie auf eine Anwendung aus dem Katalog hinzufügen.][3]

6. Geben Sie im **Suchfeld** **Vertrieb**. Klicken Sie dann wählen Sie aus den Ergebnissen der **Vertrieb** aus, und klicken Sie auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Fügen Sie Vertrieb hinzu.][4]

7. Sie sollten jetzt die Seite Schnellstart Vertrieb finden Sie unter:

    ![Schnellstart-Seite in Azure AD-des Vertrieb][5]

##<a name="step-2-enable-single-sign-on"></a>Schritt 2: Aktivieren Sie einmaliges Anmelden

1. Bevor Sie einmaliges Anmelden konfigurieren können, müssen Sie einrichten und Bereitstellen ein benutzerdefiniertes Domänennamens für Ihre Umgebung Vertrieb. Anweisungen zum erledigen finden Sie unter [Festlegen von einem Domänennamen](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Klicken Sie auf Vertriebs Schnellstart Seite in Azure AD-klicken Sie auf die Schaltfläche **Konfigurieren einmaliges Anmelden** .

    ![Die einzelnen anmelden Schaltfläche Konfigurieren][6]

3. Es wird ein Dialogfeld geöffnet, und sehen Sie einen Bildschirm mit der Frage "Wie Benutzer bei Vertrieb, klicken Sie auf aussehen sollen?" Wählen Sie **Azure AD einmaliges Anmelden**aus, und klicken Sie dann auf **Weiter**.

    ![Wählen Sie aus Azure AD einmaliges Anmelden][7]

    > [AZURE.NOTE] Weitere Informationen zu den verschiedenen einzelnen melden Sie sich auf Optionen, [Klicken Sie hier](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work) zu

4. Klicken Sie auf der Seite **Einstellungen für die App konfigurieren** füllen Sie die **Melden Sie sich auf URL** mit der Eingabe in der Vertrieb Domänen-URL in folgendem Format ein aus:
 - Enterprise-Konto:`https://<domain>.my.salesforce.com`
 - Entwicklertools Konto:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Geben Sie Ihren Anmeldenamen auf URL][8]

5. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am Vertrieb** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Zertifikat herunterladen][9]

6. Öffnen einer neuen Registerkarte in Ihrem Browser und melden Sie sich bei Ihrem Administratorkonto Vertrieb.

7. Klicken Sie im Navigationsbereich **Administrator** **Sicherheit Steuerelemente** , um die zugehörigen Abschnitt zu erweitern. Klicken Sie dann auf **Einstellungen für einzelne anmelden**.

    ![Klicken Sie auf die einzelnen anmelden Einstellungen unter Sicherheit Steuerelemente][10]

8. Klicken Sie auf der Seite **Einstellungen für einzelne melden Sie sich** auf die Schaltfläche **Bearbeiten** .

    ![Klicken Sie auf die Schaltfläche ' Bearbeiten '][11]

    > [AZURE.NOTE] Wenn Sie nicht Einstellungen für einmaliges Anmelden für Ihr Konto Vertrieb aktivieren können, müssen Sie Vertriebs-Support wenden, um die für Sie aktiviert haben.

9. Wählen Sie **SAML aktiviert**, und klicken Sie dann auf **Speichern**.

    ![Wählen Sie SAML aktiviert][12]

10. Zum Konfigurieren der SAML einzelnen anmelden Einstellungen, klicken Sie auf **neu**.

    ![Wählen Sie SAML aktiviert][13]

11. Nehmen Sie die folgenden Konfigurationen vor, auf der Seite **SAML einzelnen anmelden Einstellung zu bearbeiten** :

    ![Screenshot der Konfigurationen, die Sie erstellen sollten][14]

 - Geben Sie für das Feld **Name** einen Namen für diese Konfiguration. Einen Wert bereitstellen für **Name** füllen Sie das Textfeld **API Name** automatisch auf.

 - Kopieren Sie in Azure AD den Wert für die **URL des Herausgebers** , und fügen Sie ihn in das Feld **Herausgeber** in Vertrieb.

 - Geben Sie in das **Textfeld Einheiten Id**Ihren Vertrieb Domänennamen mit dem folgenden Muster:
     - Enterprise-Konto:`https://<domain>.my.salesforce.com`
     - Entwicklertools Konto:`https://<domain>-dev-ed.my.salesforce.com`

 - Klicken Sie auf **Durchsuchen** oder **Datei auswählen** zum Öffnen des Dialogfelds **Datei zum Hochladen auswählen** , wählen Sie Ihr Zertifikat Vertrieb, und klicken Sie dann auf **Öffnen** , wenn Sie das Zertifikat hochladen.

 - Wählen Sie für den **Typ der SAML-Identität** **Assertion enthält salesforce.com-Benutzername des Benutzers**ein.

 - Wählen Sie für **SAML Identität Speicherort** **Identität befindet sich im NameIdentifier Element der Betreff-Anweisung**

 - Kopieren Sie in Azure AD den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Feld **Identität Anbieter Anmelde-URL** in Vertrieb.

 - Wählen Sie für **Service Provider initiiert Bindung anfordern** **HTTP-Umleitung**ein.

 - Klicken Sie abschließend auf **Speichern** , um Ihre SAML einzelnen anmelden Einstellungen anzuwenden.

12. Klicken Sie auf der linken Navigationsbereich in Vertrieb klicken Sie auf **Domain Management** , um die zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **Meine Domäne**.

    ![Klicken Sie auf meiner Domäne][15]

13. Führen Sie einen Bildlauf nach unten bis zum Abschnitt **Konfiguration der Authentifizierung** , und klicken Sie auf die Schaltfläche **Bearbeiten** .

    ![Klicken Sie auf die Schaltfläche ' Bearbeiten '][16]

14. Klicken Sie im Abschnitt **Authentifizierungsdienst** wählen Sie den Anzeigenamen Ihrer SSO SAML-Konfiguration, und klicken Sie dann auf **Speichern**.

    ![Wählen Sie aus der SSO-Konfiguration][17]

    > [AZURE.NOTE] Wenn mehr als ein Authentifizierungsdienst ausgewählt ist, werden klicken Sie dann, wenn Benutzer versuchen, einmaliges Anmelden für Ihre Umgebung Vertrieb initiieren sie aufgefordert, welcher Authentifizierungsdienst wählen Sie diese in melden möchten. Wenn Sie nicht, dass dies auftritt möchten, sollte dann Sie **Alle anderen Authentifizierungsdienste deaktiviert lassen**.

15. Wählen Sie in Azure AD Konfiguration für einzelne Zeichen Bestätigung aktivieren, um das Zertifikat zu aktivieren, das Sie auf Vertrieb hochgeladen. Klicken Sie dann auf **Weiter**.

    ![Aktivieren Sie das Kontrollkästchen zur Bestätigung][18]

16. Geben Sie auf der letzten Seite des Dialogfelds in einer e-Mail-Adresse, wenn Sie e-Mail-Benachrichtigungen für Fehlern und Warnungen im Zusammenhang mit der Wartung dieser Konfiguration für einzelne Zeichen erhalten möchten. 

    ![Geben Sie Ihre e-Mail-Adresse ein.][19]

17. Klicken Sie auf **abgeschlossen** , um das Dialogfeld zu schließen. Klicken Sie zum Testen der Konfigurations finden Sie weiter unten im Abschnitt [Benutzer Vertrieb zuweisen](#step-4-assign-users-to-salesforce)aus.

##<a name="step-3-enable-automated-user-provisioning"></a>Schritt 3: Aktivieren Sie Automatisches Benutzer bereitgestellt

1. Klicken Sie für den Vertrieb auf der Seite Azure AD-Schnellstart auf die Schaltfläche **Konfigurieren Benutzer bereitgestellt** .

    ![Klicken Sie auf die Schaltfläche Konfigurieren Benutzer bereitgestellt werden.][20]

2. Geben Sie im Dialogfeld **konfigurieren die Benutzer bereitgestellt** in Ihren Vertrieb Administrator-Benutzernamen und Ihr Kennwort ein.

    ![Geben Sie in Ihrem Administrator-Benutzernamen oder Kennwort][21]

    > [AZURE.NOTE] Wenn Sie eine Herstellung Umgebung konfigurieren, ist die beste Methode so erstellen Sie ein neues Administratorkonto in Vertrieb speziell für diesen Schritt. Dieses Konto muss im Vertrieb zugewiesenen **System Administrator** -Profil verfügen.

3. Um Ihre Sicherheit Vertrieb token erhalten möchten, öffnen Sie eine neue Registerkarte und melden Sie sich in der gleichen Vertrieb Admin-Konto ein. Klicken Sie auf der oberen rechten Ecke der Seite klicken Sie auf Ihren Namen, und klicken Sie dann auf **Meine Einstellungen**.

    ![Klicken Sie auf Ihren Namen, und klicken Sie auf Meine Einstellungen][22]

4. Klicken Sie im linken Navigationsbereich auf klicken Sie auf **Persönliche** in den zugehörigen Abschnitt zu erweitern, und klicken Sie dann auf **Zurücksetzen Meine Sicherheitstoken**.

    ![Klicken Sie auf Ihren Namen, und klicken Sie auf Meine Einstellungen][23]

5. Klicken Sie auf der Seite **Zurücksetzen Meine Sicherheitstoken** klicken Sie auf die Schaltfläche **Sicherheitstoken zurücksetzen** .

    ![Lesen Sie die Warnungen.][24]

6. Aktivieren Sie den e-Mail-Posteingang mit diesem Administratorkonto verknüpft ist. Suchen Sie eine e-Mail-Nachricht aus Salesforce.com, die das neue Sicherheitstoken enthält.

7. Kopieren Sie das Token, wechseln Sie zu Ihrer Azure AD-Fenster, und fügen Sie ihn in das Feld **Benutzer Sicherheitstoken** . Klicken Sie dann auf **Weiter**.

    ![Fügen Sie das Sicherheitstoken][25]

8. Klicken Sie auf der Bestätigungsseite können Sie auswählen, e-Mail-Benachrichtigungen zu erhalten, wenn provisioning Fehler auftreten. Klicken Sie auf **abgeschlossen** , um das Dialogfeld zu schließen.

    ![Geben Sie Ihre e-Mail-Adresse Benachrichtigungen erhalten][26]

##<a name="step-4-assign-users-to-salesforce"></a>Schritt 4: Zuweisen von Benutzern zu Vertrieb

1. Starten Sie zum Testen der Konfigurations durch Erstellen einer neuen Testkonto im Verzeichnis.

2. Klicken Sie auf der Seite Vertrieb Schnellstart klicken Sie auf die Schaltfläche **Benutzer zuweisen** .

    ![Klicken Sie auf Benutzer zuweisen][27]

3. Wählen Sie Ihre Testbenutzer aus, und klicken Sie auf die Schaltfläche **zuweisen** am unteren Rand des Bildschirms:

 - Wenn Sie noch nicht aktivieren Sie Automatisches Benutzer bereitgestellt, und klicken Sie dann die folgende Aufforderung zur Bestätigung angezeigt werden:

        ![Confirm the assignment.][28]

 - Wenn Sie Automatisches provisioning Benutzer aktiviert haben, dann sehen Sie eine Aufforderung zum definieren, welche Art von Vertrieb Profil der Benutzer erhalten soll. Neu eingerichtete Benutzer sollten in Ihrer Umgebung Vertrieb nach ein paar Minuten angezeigt werden.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Wenn Sie bei einer Vertrieb **Entwicklertools** -Umgebung bereitgestellt werden, müssen Sie eine sehr eingeschränkte Anzahl von Lizenzen für jedes Profil zur Verfügung. Daher, empfiehlt es sich, Bereitstellen von Benutzern in das **Geschwätz kostenlosen** Benutzerprofil, das 4,999 Lizenzen verfügbar sind.

4. Klicken Sie zum Testen der Einstellungen für einzelnen Zeichen, öffnen Sie die Access-Systemsteuerung am [https://myapps.microsoft.com](https://myapps.microsoft.com/), und klicken Sie dann melden Sie sich bei der Testkonto, und klicken Sie auf **Vertrieb**.

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
