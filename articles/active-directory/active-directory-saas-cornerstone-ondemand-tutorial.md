<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Grundsteine auf-Anforderung | Microsoft Azure" 
    description="Erfahren Sie, wie mit Grundsteine auf-Anforderung mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktiviert!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Lernprogramm: Azure-Active Directory-Integration in Grundsteine auf-Anforderung

Ziel dieses Lernprogramms ist die Integration von Azure und auf Grundsteine-Anforderung angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Grundsteine auf-Anforderung

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie auf Grundsteine-Anforderung zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Grundsteine auf-Anforderung (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Grundsteine auf-Anforderung
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Szenario")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>Aktivieren die Anwendungsintegration für Grundsteine auf-Anforderung

Das Ziel der in diesem Abschnitt ist zu gliedernden So aktivieren Sie die Anwendungsintegration für Grundsteine auf-Anforderung.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Grundsteine auf-Anforderung die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **auf Grundsteine-Anforderung**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Katalog der Anwendung")

7.  Im Ergebnisbereich auswählen **Auf Grundsteine-Anforderung**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Auf Grundsteine-Anforderung] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Auf Grundsteine-Anforderung")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer auf Grundsteine-Anforderung mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden Authentifizierung aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Auf Grundsteine-Anforderung** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Einmaliges Anmelden aktivieren")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Grundsteine auf-Anforderung auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Microsoft Azure AD einmaliges Anmelden] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Grundsteine auf-Anforderung melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*http://company.csod.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Grundsteine auf-Anforderung** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Konfigurieren Sie einmaliges Anmelden")

5.  Send die folgenden Elemente in der Grundsteine auf-Anforderung Team unterstützen:

    1.  Die heruntergeladene Zertifikat
    2.  Der Wert **Remote Anmelde-URL**
    3.  Der **Remote Abmeldung URL** -Wert.

    >[AZURE.NOTE] Einmaliges Anmelden muss von der Grundsteine auf-Anforderung-Supportteam konfiguriert sein.
Sie erhalten eine Benachrichtigung vom Supportteam nach Beendigung die Konfiguration.

6.  Wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Grundsteine auf-Anforderung zu ermöglichen, müssen er in Grundsteine auf-Anforderung bereitgestellt werden.  
Wenn Grundsteine auf-Anforderung ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Senden Sie die Informationen (z. B.: Name, Emial) über den Azure AD-Benutzer bereitstellen, um auf die Grundsteine-Anforderung gewünschte Supportteam.

>[AZURE.NOTE] Alle anderen Grundsteine auf-Anforderung Benutzer Konto Creation Tools können oder APIs, die auf Grundsteine-Anforderung bereitstellen AAD Benutzerkonten bereitstellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Zuweisen von Benutzern zu Grundsteine auf-Anforderung führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Auf Grundsteine-Anforderung **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Zuweisen von Benutzern")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
