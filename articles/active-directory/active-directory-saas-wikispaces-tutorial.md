<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Wikispaces | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Wikispaces mit Azure Active Directory einmaliges Anmelden, automatisierte bereitgestellt und mehr aktivieren!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Lernprogramm: Azure-Active Directory-Integration in Wikispaces
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Wikispaces anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Wikispaces einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Wikispaces zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Wikispaces (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Wikispaces
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Sceanrio] (./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Sceanrio")

##<a name="enabling-the-application-integration-for-wikispaces"></a>Aktivieren die Anwendungsintegration für Wikispaces
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Wikispaces aktiviert.

###<a name="to-enable-the-application-integration-for-wikispaces-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Wikispaces aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Wikispaces**.

    ![Katalog der Anwendung] (./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Wikispaces aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Wikispaces] (./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Wikispaces mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Wikispaces** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Wikispaces auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Wikispaces melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*http://company.wikispaces.net*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-wikispaces-tutorial/IC787190.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Wikispaces** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Konfigurieren Sie einmaliges Anmelden")

5.  Senden Sie die Metadatafile an das Supportteam Wikispaces ein.

    >[AZURE.NOTE] Die einzelne anmelden Konfiguration weist das Supportteam Wikispaces leisten müssen. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Wikispaces zu ermöglichen, müssen er in Wikispaces bereitgestellt werden.  
Im Falle von Wikispaces ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Wikispaces** an.

2.  Wechseln Sie zu der **Mitglieder**.

    ![Mitglieder] (./media/active-directory-saas-wikispaces-tutorial/IC787193.png "Mitglieder")

3.  Klicken Sie auf die **Personen einladen**.

    ![Einladen von Personen] (./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Einladen von Personen")

4.  Führen Sie im Abschnitt **Personen einladen** die folgenden Schritte aus:

    ![Einladen von Personen] (./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Einladen von Personen")

    1.  Geben Sie den **Benutzernamen oder e-Mail-Adresse** eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Senden**.  

        >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhält eine e-Mail-Nachricht, einschließlich einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Wikispaces Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Wikispaces bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-wikispaces-perform-the-following-steps"></a>Zuweisen von Benutzern zu Wikispaces führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Wikispaces **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).