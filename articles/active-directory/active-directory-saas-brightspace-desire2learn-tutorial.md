<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Brightspace durch Desire2Learn | Microsoft Azure" 
    description="Erfahren Sie, wie Sie mithilfe von Desire2Learn Brightspace mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktiviert!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Lernprogramm: Azure-Active Directory-Integration in Brightspace von Desire2Learn

Ziel dieses Lernprogramms ist die Integration von Azure und Brightspace durch Desire2Learn angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Brightspace durch Desire2Learn einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie durch Desire2Learn Brightspace zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Brightspace durch Desire2Learn Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Brightspace durch Desire2Learn
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Szenario")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Aktivieren die Anwendungsintegration für Brightspace durch Desire2Learn

In diesem Abschnitt Ziel ist es zu gliedernden wie Sie die Anwendungsintegration für Brightspace durch Desire2Learn aktivieren.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Brightspace durch Desire2Learn die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Brightspace durch Desire2Learn**aus.

    ![Apllication-Katalog] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Apllication-Katalog")

7.  Im Bereich Ergebnisse wählen Sie **Brightspace durch Desire2Learn aus**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Brightspace von Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace von Desire2Learn")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Brightspace durch Desire2Learn mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Brightspace durch Desire2Learn** Anwendung Integration klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Brightspace nach Desire2Learn auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der App-URL] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Konfigurieren der App-URL")

    1.  Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL, die von den Benutzern verwendet werden, melden Sie sich bei Ihrem **Brightspace durch Desire2Learn** (z. B.: **).
    2.  Klicken Sie auf **Weiter**

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Brightspace durch Desire2Learn** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Herunterladen von Metadaten**, und klicken Sie dann speichern Sie die Metadaten auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Konfigurieren Sie einmaliges Anmelden")

5.  Senden Sie die heruntergeladene Metadatendatei an Ihre Brightspace vom Supportteam Desire2Learn.

    >[AZURE.NOTE] Führen Sie die eigentliche SSO-Konfiguration muss Ihrer Brightspace vom Supportteam Desire2Learn.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei Brightspace durch Desire2Learn aktivieren möchten, müssen sie durch Desire2Learn in Brightspace bereitgestellt werden.  
Im Falle von Brightspace durch Desire2Learn müssen die Benutzerkonten durch Ihre Brightspace vom Supportteam Desire2Learn erstellt werden.

>[AZURE.NOTE] Können Sie alle anderen Brightspace Desire2Learn Benutzer Konto Creation Tools verwenden oder APIs von bereitgestellten Brightspace durch Desire2Learn Bereitstellen von Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Zuweisen von Benutzern zu Brightspace durch Desire2Learn führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Brightspace durch Desire2Learn **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
