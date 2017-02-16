<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Adaptive Suite | Microsoft Azure"
    description="Erfahren Sie, wie mit Adaptive Suite mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktiviert!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Lernprogramm: Azure-Active Directory-Integration in Adaptive Suite

Das Ziel dieses Lernprogramms ist die Integration von Azure und Adaptive Suite anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Adaptive Suite Mandanten

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie der Adaptive Suite zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Adaptive Suite Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration Adaptive Suite
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Szenario")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Aktivieren die Anwendungsintegration Adaptive Suite

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration Adaptive Suite aktiviert.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Um die Anwendungsintegration Adaptive Suite zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Adaptive-Suite**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Adaptive Suite**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Adaptive Suite] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptive Suite")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren Adaptive der Suite mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden Adaptive Suite konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Adaptive Suite** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Adaptive Suite auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Einstellungen für die App konfigurieren** im Textfeld **URL Antworten** den URL dem folgenden Muster "*Https://login.adaptiveinsights.com:443/Samlsso/RlJFRVRSSUFMMTI3MTE =*", und klicken Sie dann auf **Weiter**.

    >[AZURE.NOTE] Sie können diesen Wert aus der Adaptive Suite **SAML** -Einstellungsseite SSO abrufen.

    ![Konfigurieren der App-Einstellungen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Konfigurieren der App-Einstellungen")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Adaptive Suite** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Adaptive Suite als Administrator.

6.  Wechseln Sie zu **Administrator**.

    ![Administrator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrator")

7.  Klicken Sie im Abschnitt **Benutzer und Rollen** **Verwalten SAML SSO-Einstellungen**aus.

    ![Verwalten von SAML SSO-Einstellungen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Verwalten von SAML SSO-Einstellungen")

8.  Klicken Sie auf der Einstellungsseite **SSO SAML** führen Sie die folgenden Schritte aus:

    ![SSO SAML-Einstellungen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SSO SAML-Einstellungen")

    1.  Geben Sie in das Textfeld **Name des Anbieters Identität** einen Namen für Ihre Konfiguration aus.
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Adaptive Suite** kopieren Sie den Wert der **Element-ID** , und fügen Sie ihn in das Textfeld **Identitätsanbieter Einheiten ID** .
    3.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Adaptive Suite** im Azure klassischen Portal des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **Identitätsanbieter SSO-URL** .
    4.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Adaptive Suite** im Azure klassischen Portal des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **benutzerdefinierte Abmeldung URL** .
    5.  Wenn Ihr heruntergeladene Zertifikat hochladen möchten, klicken Sie auf **Datei auswählen**.
    6.  Wählen Sie als **SAML-Benutzer-Id** **Adaptive Einsichten Benutzername des Benutzers**ein.
    7.  Wählen Sie als **Speicherort für die SAML-Id** **Benutzer-Id in der Betreffzeile NameID**aus.
    8.  Wählen Sie als **NameID SAML-Format** **e-Mail-Adresse**ein.
    9.  Wählen Sie als **SAML aktivieren** **SAML-SSO zulassen und direkte Adaptive Einsichten Login**.
    10. Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei Adaptive Suite aktivieren zu können, müssen er in Adaptive Suite bereitgestellt werden.  
Wenn Adaptive-Suite aus ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Adaptive Suite** an.

2.  Wechseln Sie zu **Administrator**.

    ![Administrator] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Administrator")

3.  Klicken Sie im Abschnitt **Benutzer und Rollen** auf **Benutzer hinzufügen**.

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Fügen Sie Benutzer hinzu")

4.  Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:

    ![Übermitteln] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Übermitteln")

    1.  Geben Sie **Name**, **Login**, **E-Mails**, **das Kennwort** eines gültigen Azure Active Directory-Benutzers, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie eine **Rolle**aus.
    3.  Klicken Sie auf **Absenden**.

>[AZURE.NOTE] Alle anderen Adaptive Suite Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Adaptive Suite bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Zuweisen von Benutzern zu Adaptive Suite führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration **Adaptive Suite **auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
