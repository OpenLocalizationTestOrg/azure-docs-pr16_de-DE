<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Mindflash | Microsoft Azure" 
    description="Erfahren Sie, wie Mindflash mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Lernprogramm: Azure-Active Directory-Integration in Mindflash
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Mindflash anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine Mindflash einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Mindflash zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Mindflash (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Mindflash
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Szenario")
##<a name="enabling-the-application-integration-for-mindflash"></a>Aktivieren die Anwendungsintegration für Mindflash
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Mindflash aktiviert.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Mindflash aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Mindflash**.

    ![Katalog der Anwendung] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Mindflash aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Mindflash mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Mindflash** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Mindflash auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie Ihre URL dem folgenden Muster "*http://company.mindflash.com*" auf der Seite **Konfigurieren der App-URL** in das Textfeld **Melden Sie sich auf URL** , und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Mindflash** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Konfigurieren Sie einmaliges Anmelden")

5.  Senden Sie die Metadatafile an das Supportteam Mindflash ein.

    >[AZURE.NOTE] Die einzelne anmelden Konfiguration weist das Supportteam Mindflash leisten müssen. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Mindflash zu ermöglichen, müssen er in Mindflash bereitgestellt werden.  
Im Falle von Mindflash ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Mindflash** an.

2.  Wechseln Sie zum **Verwalten von Benutzern**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Verwalten von Benutzern")

3.  Klicken Sie auf **Benutzer hinzufügen**, und klicken Sie dann auf **neu**.

4.  Führen Sie im Abschnitt **Hinzufügen neuer Benutzer** die folgenden Schritte aus:

    ![Zum Hinzufügen neuer Benutzer] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Zum Hinzufügen neuer Benutzer")

    1.  Geben Sie den **Vornamen**, **Nachnamen** und **E-Mail** mit einem gültigen AAD-Konto, die, das Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Hinzufügen**.

>[AZURE.NOTE]Alle anderen Mindflash Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Mindflash bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Zuweisen von Benutzern zu Mindflash führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Mindflash **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).