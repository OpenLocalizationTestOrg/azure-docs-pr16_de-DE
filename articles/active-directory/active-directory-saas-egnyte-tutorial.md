<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Egnyte | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Egnyte mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Lernprogramm: Azure-Active Directory-Integration in Egnyte
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Egnyte anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Egnyte einmaliges Anmelden aktiviert Abonnement
  
Nach Abschluss dieses Lernprogramms müssen werden die Azure AD-Benutzer, die Sie Egnyte zugewiesen haben einzelnen melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Egnyte (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md) können
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Egnyte
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-egnyte-tutorial/IC787812.png "Szenario")
##<a name="enabling-the-application-integration-for-egnyte"></a>Aktivieren die Anwendungsintegration für Egnyte
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Egnyte aktiviert.

###<a name="to-enable-the-application-integration-for-egnyte-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Egnyte aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-egnyte-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-egnyte-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-egnyte-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Egnyte**.

    ![Katalog der Anwendung] (./media/active-directory-saas-egnyte-tutorial/IC787813.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Egnyte aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Egnyte] (./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Egnyte mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Egnyte** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-egnyte-tutorial/IC787815.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Egnyte auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-egnyte-tutorial/IC787816.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Egnyte melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*https://company.egnyte.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-egnyte-tutorial/IC787817.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Egnyte** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-egnyte-tutorial/IC787818.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Egnyte als Administrator.

6.  Klicken Sie auf **Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-egnyte-tutorial/IC787819.png "Einstellungen")

7.  Klicken Sie im Menü **Einstellungen**auf.

    ![Einstellungen] (./media/active-directory-saas-egnyte-tutorial/IC787820.png "Einstellungen")

8.  Klicken Sie auf die Registerkarte **Konfiguration** , und klicken Sie dann auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-egnyte-tutorial/IC787821.png "Sicherheit")

9.  Führen Sie im Abschnitt **Authentifizierung für einzelne Zeichen** die folgenden Schritte aus:

    ![Einmaliges Anmelden Authentifizierung] (./media/active-directory-saas-egnyte-tutorial/IC787822.png "Einmaliges Anmelden Authentifizierung")

    1.  Wählen Sie als **einzelne anmelden Authentifizierung** **SAML 2.0**.
    2.  Wählen Sie als **Identitätsanbieter** **AzureAD**ein.
    3.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Egnyte** .
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Egnyte** kopieren Sie den Wert der **Element-ID** , und fügen Sie ihn in das Textfeld **Identität Anbieter Einheiten ID** .
    5.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    6.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Identität Anbieter Zertifikat** .
    7.  Wählen Sie als **Standard-Zuordnung des Benutzers** **e-Mail-Adresse**ein.
    8.  Wählen Sie als **verwenden spezifische Herausgeber Wert** **deaktiviert**.
    9.  Klicken Sie auf **Speichern**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-egnyte-tutorial/IC787823.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Egnyte zu ermöglichen, müssen er in Egnyte bereitgestellt werden.  
Im Falle von Egnyte ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer **Egnyte** Egnyte Firmenwebsite an.

2.  Wechseln Sie zu **Einstellungen \> Benutzer und Gruppen**.

3.  Klicken Sie auf **Neuen Benutzer hinzufügen**, und wählen Sie dann auf den Typ des Benutzers, die Sie hinzufügen möchten.

    ![Benutzer] (./media/active-directory-saas-egnyte-tutorial/IC787824.png "Benutzer")

4.  Führen Sie im Abschnitt **Neuer Standardbenutzer** die folgenden Schritte aus:

    ![Neue Standard-Benutzer] (./media/active-directory-saas-egnyte-tutorial/IC787825.png "Neue Standard-Benutzer")

    1.  Geben Sie die **E-Mail**, **Benutzername** und andere Details eines gültigen Azure Active Directory-Kontos ein, die Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto, eine e-Mail-Benachrichtigung erhalten.

>[AZURE.NOTE] Alle anderen Egnyte Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Egnyte bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-egnyte-perform-the-following-steps"></a>Zuweisen von Benutzern zu Egnyte führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Egnyte **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-egnyte-tutorial/IC787826.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-egnyte-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).