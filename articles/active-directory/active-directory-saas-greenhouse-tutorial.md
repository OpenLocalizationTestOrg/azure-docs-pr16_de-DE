<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Treibhausgasen | Microsoft Azure" 
    description="Erfahren Sie, wie zur Verwendung von Treibhausgasen mit Azure Active Directory aktivieren einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Lernprogramm: Azure-Active Directory-Integration in Treibhausgasen
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Treibhausgasen anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Treibhausgasen einzelnes anmelden Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Treibhausgasen zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Treibhausgasen (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Treibhausgasen
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Szenario")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Aktivieren die Anwendungsintegration für Treibhausgasen
  
So aktivieren Sie die Anwendungsintegration für Treibhausgasen Gliedern ist das Ziel in diesem Abschnitt.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Treibhausgasen der folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Treibhausgasen**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Treibhausgasen aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Treibhausgasen] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Treibhausgasen")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Treibhausgasen mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Treibhausgasen** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer Treibhausgasen bei, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie Ihre URL dem folgenden Muster "*https://company.greenhouse.io*" auf der Seite **Konfigurieren der App-URL** in das Textfeld **Melden Sie sich auf URL** , und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Treibhausgasen** auf **Metadaten herunterladen**und dann speichern Sie Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Konfigurieren einmaliges Anmelden")

5.  Weiterleiten dieser Datei Metadaten an Treibhausgasen Supportteam.

    >[AZURE.NOTE] Einmaliges Anmelden muss durch das Supportteam Treibhausgasen aktiviert werden.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei Treibhausgasen aktivieren zu können, müssen er in Treibhausgasen bereitgestellt werden.  
Im Falle von Treibhausgasen ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Treibhausgasen** an.

2.  Klicken Sie auf **Konfigurieren**, und klicken Sie dann auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Benutzer")

3.  Klicken Sie auf **Neuer Benutzer**.

    ![Neuer Benutzer] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Neuer Benutzer")

4.  Führen Sie im Abschnitt **Hinzufügen neuer Benutzer** die folgenden Schritte aus:

    ![Hinzufügen eines neuen Benutzers] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Hinzufügen eines neuen Benutzers")

    1.  Geben Sie in das Textfeld **EINGABETASTE Benutzer e-Mails** die e-Mail-Adresse eines gültigen Azure Active Directory-Kontos ein, die Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.
        
        >[AZURE.NOTE] Halter der Azure-Active Directory-Konto erhalten eine e-Mail-Nachricht, einschließlich einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Treibhausgasen Benutzer Konto Creation Tools können oder APIs bereitgestellt von Treibhausgasen bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Zuweisen von Benutzern zu Treibhausgasen führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Treibhausgasen **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).