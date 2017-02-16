<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Panopto | Microsoft Azure" 
    description="Erfahren Sie, wie Panopto mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Lernprogramm: Azure-Active Directory-Integration in Panopto
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Panopto anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Panopto
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Panopto zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Panopto (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Panopto
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Szenario")
##<a name="enabling-the-application-integration-for-panopto"></a>Aktivieren die Anwendungsintegration für Panopto
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Panopto aktiviert.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Panopto aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Panopto**.

    ![Appkication-Katalog] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication-Katalog")

7.  Wählen Sie im Ergebnisbereich **Panopto aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Panopto mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Panopto** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Panopto auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Panopto melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Panopto.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Panopto** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Panopto als Administrator.

6.  Klicken Sie in der Symbolleiste auf der linken Seite klicken Sie auf **System**, und klicken Sie dann auf **Identitätsanbieter**.

    ![System] (./media/active-directory-saas-panopto-tutorial/IC777670.png "System")

7.  Klicken Sie auf **Anbieter hinzufügen**.

    ![Identitätsanbieter] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Identitätsanbieter")

8.  Führen Sie im Abschnitt SAML-Anbieter der folgenden Schritte aus:

    ![SaaS-Konfiguration] (./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS-Konfiguration")

    1.  Wählen Sie aus der Liste **Typ Anbieter** **SAML20**
    2.  Geben Sie in das Textfeld **Instanznamen** einen Namen für die Instanz aus.
    3.  Geben Sie im Textfeld **Beschreibung** eine Beschreibung ein.
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Panopto** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber** .
    5.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Panopto** im Azure klassischen Portal des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **Url der Seite springen** .
    6.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    7.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Öffentlicher Schlüssel**
    8.  Klicken Sie auf **Speichern**.
        ![Speichern] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Speichern")

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer zu Panopto provisioning konfigurieren.  
Wenn Sie ein zugeordneten Benutzer versucht, die bei Verwendung der Access-Systemsteuerung Panopto anzumelden, überprüft Panopto an, ob der Benutzer vorhanden ist.  
Wenn es noch kein Benutzerkonto verfügbar ist, wird es von Panopto automatisch erstellt.

>[AZURE.NOTE]Alle anderen Panopto Benutzer Konto Creation Tools können oder APIs von Panopto zur Bereitstellung von Azure AD-Benutzerkonten bereitgestellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Zuweisen von Benutzern zu Panopto führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Panopto **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).