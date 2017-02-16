<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in drängeln | Microsoft Azure" 
    description="Erfahren Sie, wie drängeln mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Lernprogramm: Azure-Active Directory-Integration in drängeln
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und drängeln anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine drängeln einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie drängeln zugewiesen haben können einzelne melden Sie sich bei der Anwendung am drängeln der Website Ihres Unternehmens (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für drängeln
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="enabling-the-application-integration-for-huddle"></a>Aktivieren die Anwendungsintegration für drängeln
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für drängeln aktiviert.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für drängeln die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **drängeln**ein.

    ![Katalog der Anwendung] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Katalog der Anwendung")

7.  Im Ergebnisbereich **drängeln**wählen Sie aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Drängeln] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Drängeln")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu drängeln mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der **drängeln** Integrationsseite Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie, wie Benutzer bei Sie drängeln auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Drängeln melden Sie sich auf URL** Geben Sie die URL Ihrer drängeln Mandanten mit dem folgenden Muster "*http://company.huddle.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei drängeln** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Konfigurieren Sie einmaliges Anmelden")

    1.  Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.
    2.  Kopieren Sie der **URL des Herausgebers** -Wert, der **SAML SSO-URL** -Wert und die heruntergeladene Zertifikat, und klicken Sie dann an das Supportteam drängeln senden.

    >[AZURE.NOTE] Einmaliges Anmelden muss durch das Supportteam drängeln aktiviert sein.
Erhalten Sie eine Benachrichtigung, wenn die Konfiguration abgeschlossen wurde.

5.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei drängeln zu ermöglichen, müssen er in drängeln bereitgestellt werden.  
Im Falle von drängeln ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **drängeln** an.

2.  Klicken Sie auf **Arbeitsbereich**.

3.  Klicken Sie auf **Personen \> Personen einladen**.

    ![Personen] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Personen")

4.  Führen Sie im Abschnitt **Erstellen Sie eine neue Einladung** die folgenden Schritte aus:

    ![Neue Einladung] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Neue Einladung")

    1.  Wählen Sie in der Liste **Wählen Sie ein Team zum Einladen von Personen die Teilnahme** **Team**aus.
    2.  Geben Sie die **E-Mail-Adresse** eines gültigen AAD-Kontos ein, die Sie in das Textfeld Verwandte bereitstellen möchten.
    3.  Klicken Sie auf **einladen**.

    >[AZURE.NOTE] Der Inhaber des Azure AD-Konto erhalten eine e-Mail-Nachricht, einschließlich einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen drängeln Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Bereitstellen von AAD drängeln Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Zuweisen von Benutzern zu drängeln führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der **drängeln **Anwendung Integrationsseite auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).