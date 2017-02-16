<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Projectplace | Microsoft Azure" 
    description="Erfahren Sie, wie Projectplace mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Lernprogramm: Azure-Active Directory-Integration in Projectplace
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Projectplace anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Projectplace einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Projectplace zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Projectplace (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Projectplace
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Szenario")
##<a name="enabling-the-application-integration-for-projectplace"></a>Aktivieren die Anwendungsintegration für Projectplace
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Projectplace aktiviert.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Projectplace aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Projectplace**.

    ![Katalog der Anwendung] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Projectplace aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Projectplace mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Projectplace** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Konfigurieren von einzigen Anmeldung")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Projectplace auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Konfigurieren von einzigen Anmeldung")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Projectplace melden Sie sich auf URL** die URL Ihrer ProjectPlace Mandanten (z. B.: "*http://company.projectplace.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Projectplace** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Konfigurieren von einzigen Anmeldung")

5.  Senden Sie die Metadatafile an das Supportteam Projectplace ein.

    >[AZURE.NOTE] Die einzelne anmelden Konfiguration weist das Supportteam Projectplace leisten müssen. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Konfigurieren von einzigen Anmeldung")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Projectplace zu ermöglichen, müssen er in Projectplace bereitgestellt werden.  
Im Falle von Projectplace ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Projectplace** an.

2.  Wechseln Sie zu der **Personen**ein, und klicken Sie dann auf **Mitglieder**.

    ![Personen] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Personen")

3.  Klicken Sie auf **Element hinzufügen**.

    ![Hinzufügen von Mitgliedern] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Hinzufügen von Mitgliedern")

4.  Führen Sie im Abschnitt **Mitglied hinzufügen** die folgenden Schritte aus:

    ![Neue Mitglieder] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Neue Mitglieder")

    1.  Geben Sie im Textfeld **Neuer Mitglieder** die e-Mail-Adresse eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Senden**

        >[AZURE.NOTE] Eine e-Mail-Nachricht einen Link, um das Konto zu bestätigen, bevor diese aktiv wird einschließlich wird an den Inhaber des Azure Active Directory-Konto gesendet.
    
>[AZURE.NOTE]Alle anderen Projectplace Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Projectplace bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Zuweisen von Benutzern zu Projectplace führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Projectplace **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).