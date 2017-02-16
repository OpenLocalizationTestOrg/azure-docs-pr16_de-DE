<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Sprinklr | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Sprinklr mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Lernprogramm: Azure-Active Directory-Integration in Sprinklr
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Sprinklr anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Sprinklr
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Sprinklr zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Sprinklr (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Sprinklr
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Szenario")

##<a name="enabling-the-application-integration-for-sprinklr"></a>Aktivieren die Anwendungsintegration für Sprinklr
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Sprinklr aktiviert.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Sprinklr aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Sprinklr**.

    ![Katalog der Anwendung] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Sprinklr aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Sprinklr mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Sprinklr** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Sprinklr auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Sprinklr melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. sprinklr.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Sprinklr** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Sprinklr als Administrator.

6.  Wechseln Sie zu **Administration \> Einstellungen**.

    ![Verwaltung] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Verwaltung")

7.  Wechseln Sie zu **Partner verwalten \> Single Sign** auf aus dem linken Bereich.

    ![Verwalten von Partner] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Verwalten von Partner")

8.  Klicken Sie auf **+ Add-Ons für einmaliges Anmelden**.

    ![Einzelnes Zeichen-Ons] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Einzelnes Zeichen-Ons")

9.  Führen Sie die folgenden Schritte aus, auf der Seite **Für einmaliges Anmelden** :

    ![Einzelnes Zeichen-Ons] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Einzelnes Zeichen-Ons")

    1.  Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration (z. B.: *WAADSSOTest*).
    2.  Wählen Sie **aktiviert**aus.
    3.  Wählen Sie **Neue SSO-Zertifikat verwenden**.
    4.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    5.  Öffnen Sie Ihrer Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Identität Anbieter Zertifikat** ,
    6.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Sprinklr** im Azure klassischen Portal des Werts **Identität Anbieter-ID** , und fügen Sie ihn in das Textfeld **Einheiten Id** .
    7.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Sprinklr** .
    8.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Sprinklr** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **Identität Anbieter Abmelden URL** einzufügen.
    9.  Wählen Sie als **SAML-Benutzer-ID-Typ** **Assertion enthält Benutzer "s sprinklr.com Benutzername**.
    10. Wählen Sie als **SAML-Benutzer-ID-Speicherort** **Benutzer-ID wird im Namen Identifier-Element der Betreff-Anweisung**ein.
    11. Schließen Sie **Speichern**.

        ![SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Für AAD Benutzer anmelden können müssen sie für den Zugriff innerhalb der Sprinklr Anwendung bereitgestellt werden.  
In diesem Abschnitt beschrieben, wie Sie Benutzerkonten innerhalb Sprinklr AAD erstellt wird.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Um ein Benutzerkonto in Sprinklr bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite Sprinklr.

2.  Wechseln Sie zu **Administration \> Einstellungen**.

    ![Verwaltung] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Verwaltung")

3.  Wechseln Sie zu **Clientcomputer verwalten \> Benutzer** aus dem linken Bereich.

    ![Einstellungen] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Einstellungen")

4.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Einstellungen] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Einstellungen")

5.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **Benutzer bearbeiten** :

    ![Benutzer bearbeiten] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Benutzer bearbeiten")

    1.  Geben Sie in der **E-Mail**, **vor-** und **Nachnamen** Textfelder möchten die Informationen eines Benutzerkontos Azure AD-bereitstellen.
    2.  Wählen Sie **Kennwort deaktiviert**.
    3.  Wählen Sie eine **Sprache**aus.
    4.  Wählen Sie einen **Benutzertyp**aus.
    5.  Klicken Sie auf **Aktualisieren**.

    >[AZURE.IMPORTANT] **Kennwort deaktiviert** muss aktiviert sein, einen Benutzer sich über einem Identitätsanbieter anmelden.

6.  Wechseln Sie zu der **Rolle**aus, und gehen Sie folgendermaßen vor:

    ![Rollen für Partner] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Rollen für Partner")

    1.  Wählen Sie in der Liste **Global** **alle\_Berechtigungen**.
    2.  Klicken Sie auf **Aktualisieren**.

>[AZURE.NOTE] Alle anderen Sprinklr Benutzer Konto Creation Tools können oder APIs von Sprinklr zur Bereitstellung von Azure AD-Benutzerkonten bereitgestellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Zuweisen von Benutzern zu Sprinklr führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Sprinklr **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).