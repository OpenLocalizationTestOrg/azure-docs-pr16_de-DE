<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Coupa | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Coupa mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Lernprogramm: Azure-Active Directory-Integration in Coupa

Das Ziel dieses Lernprogramms ist die Integration von Azure und Coupa anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Coupa einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Coupa zugewiesen haben einzelnen melden Sie sich bei der Anwendung, die die [Einführung in die Access-Bereich](active-directory-saas-access-panel-introduction.md)verwenden können.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Coupa
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Szenario")
##<a name="enabling-the-application-integration-for-coupa"></a>Aktivieren die Anwendungsintegration für Coupa

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Coupa aktiviert.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Coupa aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Coupa**.

    ![Katalog der Anwendung] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Coupa aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Coupa mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Coupa konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Melden Sie sich als Administrator klicken Sie auf der Website Ihres Unternehmens Coupa an.

2.  Wechseln Sie zu **Setup \> Sicherheit Steuerelement**.

    ![Sicherheit-Steuerelemente] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Sicherheit-Steuerelemente")

3.  Um die Coupa-Metadaten-Datei auf Ihren Computer herunterzuladen, klicken Sie auf **herunterladen und SP-Metadaten importieren**.

    ![Coupa SP-Metadaten] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP-Metadaten")

4.  Melden Sie sich in einem anderen Browserfenster klicken Sie auf die Azure klassischen-Portal an.

5.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , auf der Seite **Coupa** Anwendung Integration.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Konfigurieren Sie einmaliges Anmelden")

6.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Coupa auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Konfigurieren Sie einmaliges Anmelden")

7.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Konfigurieren der App-URL")

    1.  Geben Sie im Textfeld **Melden Sie sich auf URL** ein URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung Coupa auf (z. B.: "*http://company.Coupa.com*").
    2.  Öffnen der heruntergeladenen Coupa-Metadaten-Datei, und kopieren Sie dann die **AssertionConsumerService Index/URL**.
    3.  Fügen Sie in das Textfeld **Coupa Antwort-URL** den Wert **AssertionConsumerService Index-URL** ein.
    4.  Klicken Sie auf **Weiter**.

8.  Auf der Seite **Konfigurieren einmaliges Anmelden bei Coupa** um die Metadatendatei herunterladen möchten, klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Datei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Konfigurieren Sie einmaliges Anmelden")

9.  Wechseln Sie auf der Website Coupa Unternehmen zu **Setup \> Sicherheit Steuerelement**.

    ![Sicherheit-Steuerelemente] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Sicherheit-Steuerelemente")

10. Führen Sie im Abschnitt **Melden Sie sich mit Coupa Anmeldeinformationen** die folgenden Schritte aus:

    ![Melden Sie sich mit Coupa Anmeldeinformationen] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Melden Sie sich mit Coupa Anmeldeinformationen")

    1.  Wählen Sie aus, **Melden Sie sich mithilfe von SAML**.
    2.  Klicken Sie auf **Durchsuchen** , um die heruntergeladene Azure-Active Metadaten-Datei hochladen.
    3.  Klicken Sie auf **Speichern**.

11. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Coupa zu ermöglichen, müssen er in Coupa bereitgestellt werden.  
Im Falle von Coupa ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Coupa** an.

2.  Klicken Sie auf **Setup**, und klicken Sie dann auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Benutzer")

3.  Klicken Sie auf **Erstellen**.

    ![Erstellen von Benutzern] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Erstellen von Benutzern")

4.  Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:

    ![Benutzerdetails] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Benutzerdetails")

    1.  Geben Sie die **Login**, **Vorname**, **Nachname**, **Einzelne ID anmelden**, **E-Mail** -Attribute eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Erstellen**.

    >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhalten eine e-Mail mit einem Link zu das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Coupa Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Coupa bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Zuweisen von Benutzern zu Coupa führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Coupa **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
