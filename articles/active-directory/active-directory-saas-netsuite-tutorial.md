<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in NetSuite | Microsoft Azure"
    description="Informationen Sie zur Verwendung von NetSuite mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!"
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

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Lernprogramm: Wie NetSuite mit Azure Active Directory integriert werden soll.

In diesem Lernprogramm erfahren Sie, wie Ihre Umgebung NetSuite Verbindung mit Ihrem Azure Active Directory (Azure AD). So konfigurieren Sie einmaliges Anmelden NetSuite, zum Aktivieren der automatischen Benutzer bereitgestellt und So weisen Sie Benutzern den Zugriff auf NetSuite lernen. 

##<a name="prerequisites"></a>Erforderliche Komponenten

1. Um Azure Active Directory über das [Azure klassischen Portal](https://manage.windowsazure.com)zugreifen zu können, müssen Sie zuerst ein gültiges Azure-Abonnement verfügen.

2. Sie müssen Administratorzugriff auf ein Abonnement [NetSuite](http://www.netsuite.com/portal/home.shtml) . Sie können eine kostenlose Testkonto für einen der Dienste verwenden.

##<a name="step-1-add-netsuite-to-your-directory"></a>Schritt 1: Hinzufügen von NetSuite zu Ihrem Verzeichnis

1. Klicken Sie im [Azure klassischen Portal](https://manage.windowsazure.com)auf der linken Navigationsbereich auf **Active Directory**.

    ![Wählen Sie im linken Navigationsbereich aus Active Directory.][0]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis, dem Sie zum NetSuite hinzufügen möchten.

3. Klicken Sie auf im oberen Menü on **Applications** .

    ![Klicken Sie auf on Applications.][1]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Klicken Sie auf Hinzufügen, um eine neue Anwendung hinzuzufügen.][2]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Klicken Sie auf eine Anwendung aus dem Katalog hinzufügen.][3]

6. Geben Sie im **Suchfeld** **NetSuite**. Klicken Sie dann wählen Sie aus den Ergebnissen der **NetSuite** aus, und klicken Sie auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Fügen Sie NetSuite hinzu.][4]

7. Sie sollten jetzt die Seite Schnellstart NetSuite finden Sie unter:

    ![Schnellstart-Seite in Azure AD-des NetSuite][5]

##<a name="step-2-enable-single-sign-on"></a>Schritt 2: Aktivieren Sie einmaliges Anmelden

1. Klicken Sie in Azure AD, klicken Sie auf der Seite Schnellstart für NetSuite auf die Schaltfläche **Konfigurieren einmaliges Anmelden** .

    ![Die einzelnen anmelden Schaltfläche Konfigurieren][6]

2. Es wird ein Dialogfeld geöffnet, und sehen Sie einen Bildschirm mit der Frage "Wie Benutzer bei NetSuite, klicken Sie auf aussehen sollen?" Wählen Sie **Azure AD einmaliges Anmelden**aus, und klicken Sie dann auf **Weiter**.

    ![Wählen Sie aus Azure AD einmaliges Anmelden][7]

    > [AZURE.NOTE] Weitere Informationen zu den verschiedenen einzelnen melden Sie sich auf Optionen, [Klicken Sie hier](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work) zu

3. Geben Sie auf der Seite **Konfigurieren der App-Einstellungen** für das Feld **Antwort-URL** in Ihrem NetSuite Mandanten URL mithilfe einer der folgenden Formate:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Geben Sie in Ihrem Mandanten-URL][8]

4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei NetSuite** klicken Sie auf **Herunterladen von Metadaten**, und klicken Sie dann speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.

    ![Laden Sie die Metadaten.][9]

5. Öffnen einer neuen Registerkarte in Ihrem Browser, und melden Sie sich bei Ihrem Netsuite Firmenwebsite als Administrator.

6. Klicken Sie in der Symbolleiste am oberen Rand der Seite klicken Sie auf **Setup**und dann auf **Setup-Manager**.

    ![Wechseln Sie zu Setup-Manager][10]

7. Wählen Sie in der Liste **Setupaufgaben** **Integration**ein.

    ![Wechseln Sie zur Integration][11]

8. Klicken Sie im Abschnitt **Authentifizierung verwalten** auf **SAML einmaliges Anmelden**.

    ![Wechseln Sie zu SAML einmaliges Anmelden][12]

9. Klicken Sie auf der Seite **SAML-Setup** führen Sie die folgenden Schritte aus:

    - Kopieren Sie in Azure Active Directory den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Feld **Identität Anbieter-Anmeldeseite** in NetSuite.

        ![Der SAML-Einrichtungsseite.][13]

    - Wählen Sie in der NetSuite **Primäre Authentifizierungsmethode**aus.

    - Wählen Sie für das Feld mit der Bezeichnung **SAMLV2 Identität Anbietermetadaten** **IDP-Metadaten-Datei hochladen**aus. Klicken Sie dann auf **Durchsuchen** , um die Metadatendatei hochladen, die Sie in Schritt 4 # heruntergeladen haben.

        ![Hochladen der Metadaten][16]

    - Klicken Sie auf **Absenden**.

9. Wählen Sie in Azure AD Konfiguration für einzelne Zeichen Bestätigung aktivieren, um das Zertifikat zu aktivieren, das Sie in den NetSuite hochgeladen. Klicken Sie dann auf **Weiter**.

    ![Aktivieren Sie das Kontrollkästchen zur Bestätigung][14]

10. Geben Sie auf der letzten Seite des Dialogfelds in einer e-Mail-Adresse, wenn Sie e-Mail-Benachrichtigungen für Fehlern und Warnungen im Zusammenhang mit der Wartung dieser Konfiguration für einzelne Zeichen erhalten möchten. 

    ![Geben Sie Ihre e-Mail-Adresse ein.][15]

11. Klicken Sie auf **abgeschlossen** , um das Dialogfeld zu schließen. Klicken Sie als Nächstes auf **Attribute** am oberen Rand der Seite.

    ![Klicken Sie auf Attribute.][17]

12. Klicken Sie auf **Benutzerattribut hinzufügen**.

    ![Klicken Sie auf Hinzufügen, Benutzerattribut.][18]

13. Geben Sie für das **Attribut** Namensfeld in `account`. Geben Sie für das **Attribut** Wertfeld in Ihrer NetSuite-ID-Konto an. So suchen Sie Ihre Konto-ID Anweisungen sind unter enthalten:

    ![Fügen Sie Ihrer NetSuite-ID-Konto an hinzu.][19]

    - Klicken Sie in NetSuite im Menü der oberen Navigationsleiste auf **Einrichten** .
    - Klicken Sie dann im Abschnitt **Setupaufgaben** des im linken Navigationsmenü auf, wählen Sie im Abschnitt **Integration** , und klicken Sie auf **Web Services-Einstellungen**.
    - Kopieren Sie Ihre NetSuite Konto-ID, und fügen Sie ihn in das Feld **Wert Attribut** in Azure Active Directory.

        ![Abrufen von Ihrem Konto-ID][20]

14. Klicken Sie in Azure AD auf **abgeschlossen** , um das SAML-Attribut hinzugefügt haben. Klicken Sie dann auf **Änderungen übernehmen** unten im Menü auf.

    ![Die Änderungen zu speichern.][21]

15. Bevor Benutzer in NetSuite einmaliges Anmelden ausführen können, müssen sie zuerst die geeigneten Berechtigungen in NetSuite zugeordnet werden. Gehen Sie diese Berechtigungen zuweisen.

    - Klicken Sie im Menü oberen Navigationsleiste klicken Sie auf **Setup**und dann auf **Setup-Manager**.

        ![Wechseln Sie zu Setup-Manager][10]

    - Klicken Sie im linken Navigationsbereich auf ausgewählte **Benutzer/Rollen**und dann auf **Rollen verwalten**.

        ![Wechseln zum Verwalten von Rollen][22]

    - Klicken Sie auf **neue Rolle**.

    - Geben Sie einen **Namen** für die neue Rolle aus, und aktivieren Sie das Kontrollkästchen **Einzelne Anmelden nur** .

        ![Benennen Sie die neue Rolle ein.][23]

    - Klicken Sie auf **Speichern**.

    - Klicken Sie im Menü oben klicken Sie auf **Berechtigungen**. Klicken Sie dann auf **Setup**.

        ![Wechseln Sie zu Berechtigungen][24]

    - **Festlegen von SAM einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Hinzufügen**.

    - Klicken Sie auf **Speichern**.

    - Klicken Sie im Menü oberen Navigationsleiste klicken Sie auf **Setup**und dann auf **Setup-Manager**.

        ![Wechseln Sie zu Setup-Manager][10]

    - Klicken Sie im linken Navigationsbereich auf ausgewählte **Benutzer/Rollen**und dann auf **Benutzer verwalten**.

        ![Wechseln zum Verwalten von Benutzern][25]

    - Wählen Sie einen Testbenutzer aus. Klicken Sie dann auf **Bearbeiten**.

        ![Wechseln zum Verwalten von Benutzern][26]

    - Klicken Sie im Dialogfeld Rollen wählen Sie die Rolle aus, die Sie erstellt haben, und klicken Sie auf **Hinzufügen**.

        ![Wechseln zum Verwalten von Benutzern][27]

    - Klicken Sie auf **Speichern**.

19. Klicken Sie zum Testen der Konfigurations finden Sie weiter unten im Abschnitt [Benutzer NetSuite zuweisen](#step-4-assign-users-to-netsuite)aus.

##<a name="step-3-enable-automated-user-provisioning"></a>Schritt 3: Aktivieren Sie Automatisches Benutzer bereitgestellt

> [AZURE.NOTE] Standardmäßig werden die bereitgestellte Benutzer die Niederlassung Stammordner Ihrer Umgebung NetSuite hinzugefügt werden.

1. Klicken Sie in Azure Active Directory auf der Seite Schnellstart für NetSuite, klicken Sie auf **Konfigurieren Benutzer bereitgestellt**.

    ![Konfigurieren der Benutzer bereitgestellt][28]

2. Klicken Sie im Dialogfeld, das geöffnet wird, geben Sie im Administrator-Anmeldeberechtigungen für NetSuite und dann auf **Weiter**.

    ![Geben Sie im NetSuite Administrator-Anmeldeberechtigungen.][29]

3. Geben Sie auf der Bestätigungsseite in Ihre e-Mail-Adresse der Bereitstellung von Fehlern-Benachrichtigungen erhalten.

    ![Bestätigen.][30]

4. Klicken Sie auf **abgeschlossen** , um das Dialogfeld zu schließen.

##<a name="step-4-assign-users-to-netsuite"></a>Schritt 4: Zuweisen von Benutzern zu NetSuite

1. Klicken Sie zum Testen der Konfigurations beginnen Sie, erstellen ein neues Testkonto im Verzeichnis.

2. Klicken Sie auf der Seite NetSuite Schnellstart klicken Sie auf die Schaltfläche **Benutzer zuweisen** .

    ![Klicken Sie auf Benutzer zuweisen][31]

3. Wählen Sie Ihre Testbenutzer aus, und klicken Sie auf die Schaltfläche **zuweisen** am unteren Rand des Bildschirms:

 - Wenn Sie noch nicht aktivieren Sie Automatisches Benutzer bereitgestellt, und klicken Sie dann die folgende Aufforderung zur Bestätigung angezeigt werden:

        ![Confirm the assignment.][32]

 - Wenn Sie Automatisches provisioning Benutzer aktiviert haben, dann sehen Sie eine Aufforderung zum definieren, in welcher der Rolle des Benutzers in NetSuite sein soll. Neu eingerichtete Benutzer sollten in Ihrer Umgebung NetSuite nach ein paar Minuten angezeigt werden.

4. Testen der Einstellungen für einzelnen Zeichen, öffnen Sie die Access-Systemsteuerung am [https://myapps.microsoft.com](https://myapps.microsoft.com/), melden Sie sich bei der Testkonto, und klicken auf **NetSuite**.

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel Index für Anwendungsverwaltung in Azure-Active Directory](active-directory-apps-index.md)
- [Liste der Lernprogramme erfahren Sie, wie Apps SaaS integriert werden soll.](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
