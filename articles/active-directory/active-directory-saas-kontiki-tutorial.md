<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Kontiki | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Kontiki mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Lernprogramm: Azure-Active Directory-Integration in Kontiki
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Kontiki anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Kontiki einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Kontiki zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Kontiki (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Kontiki
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Szenario")
##<a name="enabling-the-application-integration-for-kontiki"></a>Aktivieren die Anwendungsintegration für Kontiki
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Kontiki aktiviert.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Kontiki aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Kontiki**.

    ![Katalog der Anwendung] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Kontiki aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Kontiki mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Kontiki** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Konfigurieren von einzigen Anmeldung")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Kontiki auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Konfigurieren von einzigen Anmeldung")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Kontiki melden Sie sich auf URL** die URL, die Ihre Benutzer bei der Kontiki auf untersuchten (z. B.: "*https://company.mc.eval.kontiki.com/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Kontiki** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Konfigurieren von einzigen Anmeldung")

5.  Senden Sie die Metadatafile an das Supportteam Kontiki ein.

    >[AZURE.NOTE] Die einzelne anmelden Konfiguration weist das Supportteam Kontiki leisten müssen. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren von einzigen Anmeldung] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Konfigurieren von einzigen Anmeldung")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer zu Kontiki provisioning konfigurieren.  
Wenn Sie ein zugeordneten Benutzer versucht, die bei Verwendung der Access-Systemsteuerung Kontiki anzumelden, überprüft Kontiki an, ob der Benutzer vorhanden ist.  
Wenn es noch kein Benutzerkonto verfügbar ist, wird es von Kontiki automatisch erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Zuweisen von Benutzern zu Kontiki führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Kontiki **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).