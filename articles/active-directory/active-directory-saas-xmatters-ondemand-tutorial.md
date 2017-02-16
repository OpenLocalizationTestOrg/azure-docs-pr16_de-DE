<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit xMatters auf-Anforderung | Microsoft Azure"
    description="Erfahren Sie, wie xMatters auf-Anforderung mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Lernprogramm: Azure Active Directory-Integration mit xMatters auf-Anforderung
  
Ziel dieses Lernprogramms ist die Integration von Azure und xMatters auf-Anforderung angezeigt. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen xMatters auf-Anforderung-Mandanten
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie xMatters auf-Anforderung zugewiesen haben können einzelne melden Sie sich bei der Anwendung am xMatters auf-Anforderung der Website Ihres Unternehmens (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für xMatters auf-Anforderung
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Szenario")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Aktivieren die Anwendungsintegration für xMatters auf-Anforderung
  
Das Ziel der in diesem Abschnitt ist zu gliedernden So aktivieren Sie die Anwendungsintegration für xMatters auf-Anforderung.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für xMatters auf-Anforderung die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **xMatters auf-Anforderung**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Katalog der Anwendung")

7.  Im Ergebnisbereich auswählen **Auf XMatters-Anforderung**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![xMatters auf-Anforderung] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters auf-Anforderung")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer zu XMatters OnDemand authentifizieren mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Auf XMatters-Anforderung** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der XMatters auf-Anforderung auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Konfigurieren einmaliges Anmelden")

3.  Führen Sie auf der Seite **Konfigurieren der App-URL** die folgenden Schritte aus:

    ![Konfigurieren der app-URL] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Konfigurieren der app-URL")

    ein. Geben Sie in das Textfeld **XMatters auf-Anforderung melden Sie sich In URL** den URL dem folgenden Muster:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Klicken Sie auf **Weiter**.


4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei XMatters auf-Anforderung** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Zertifikatsdatei lokal als **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Sie müssen das Zertifikat an das Supportteam xMatters weiterleiten. Das Zertifikat muss durch das Supportteam xMatters hochgeladen werden, bevor Sie die einzelne anmelden Konfiguration abschließen können.

    ![Einzelne konfigurieren melden Sie sich auf] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Einzelne konfigurieren melden Sie sich auf")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens XMatters OnDemand als Administrator.

6.  Klicken Sie in der Symbolleiste oben auf **Administrator**, und klicken Sie dann in der Navigationsleiste auf der linken Seite auf **Ihres Unternehmens** .

    ![Administrator] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Administrator")

7.  Führen Sie auf der Seite **SAML-Konfiguration** die folgenden Schritte aus:

    ![SAML-Konfiguration] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-Konfiguration")

    1.  Wählen Sie **SAML aktivieren**.
    2.  Kopieren Sie des Werts **Identität Anbieter-ID** , und fügen Sie ihn in das Textfeld **Identität Anbieter-ID** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei XMatters auf-Anforderung** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei XMatters auf-Anforderung** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **Einzelne melden Sie sich auf URL** .
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei XMatters auf-Anforderung** kopieren Sie den Wert für die **Einzelnen Sign-Out Dienst-URL** , und fügen Sie ihn in das Textfeld **URL der einzelnen Abmelden** .
    5.  Klicken Sie auf der Seite Details der Firma klicken Sie oben auf **Save Changes**.
        ![Details für Unternehmen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Details für Unternehmen")

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Einzelne konfigurieren melden Sie sich auf] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Einzelne konfigurieren melden Sie sich auf")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei XMatters OnDemand aktivieren zu können, müssen er in XMatters OnDemand bereitgestellt werden.  
Wenn XMatters OnDemand ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **XMatters auf-Anforderung** .

2.  Klicken Sie auf die Registerkarte **Benutzer** .

3.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Benutzer")

4.  Wählen Sie **aktiv**.

5.  Führen Sie im Abschnitt **Hinzufügen eines Benutzers** die folgenden Schritte aus:

    ![Hinzufügen eines Benutzers] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Hinzufügen eines Benutzers")

    1.  Geben Sie die **Benutzer-ID**, **Vorname**, **Nachname**, **Website** eines gültigen AAD-Kontos ein, die Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen XMatters OnDemand Benutzer Konto Creation Tools können oder APIs bereitgestellt durch XMatters OnDemand bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Zuweisen von Benutzern zu XMatters OnDemand führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Auf XMatters-Anforderung **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).