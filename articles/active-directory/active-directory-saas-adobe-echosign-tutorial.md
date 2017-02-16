<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Adobe EchoSign | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Adobe EchoSign mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Lernprogramm: Azure-Active Directory-Integration in Adobe EchoSign

Das Ziel dieses Lernprogramms ist die Integration von Azure und Adobe EchoSign anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Melden Sie eine Adobe EchoSign einzelne Abonnements aktiviert

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Adobe EchoSign zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Adobe EchoSign Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Adobe EchoSign
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Szenario")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Aktivieren die Anwendungsintegration für Adobe EchoSign

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Adobe EchoSign aktiviert.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration Adobe EchoSign die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Adobe EchoSign**ein.

    ![Katalog der Anwendung] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Adobe EchoSign aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Adobe EchoSign mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren Single Sign On** , im Azure klassischen-Portal auf der Seite **Adobe EchoSign** Anwendung Integration.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Adobe EchoSign auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Adobe EchoSign melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*https://company.echosign.com/*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Adobe EchoSign als Administrator.

6.  Klicken Sie im Menü oben klicken Sie auf **Konto**, und klicken Sie dann im Navigationsbereich auf die Die Links unter **Kontoeinstellungen** **SAML-Einstellungen** auf.

    ![Konto] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Konto")

7.  Führen Sie im Abschnitt SAML-Einstellungen die folgenden Schritte aus:

    ![SAML-Einstellungen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML-Einstellungen")

    1.  Wählen Sie als **SAML-Modus** **SAML obligatorisch**.
    2.  Wählen Sie **EchoSign Konto-Administratoren zum Melden Sie sich mit ihren Anmeldeinformationen EchoSign zulassen**.
    3.  Wählen Sie als **Die Erstellung des Benutzers** **automatisch hinzufügen Benutzer über SAML authentifiziert**.

8.  Verschieben Sie, die folgenden Schritte ausführen:

    ![SAML-Einstellungen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML-Einstellungen")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** kopieren Sie den Wert der **Element-ID** , und fügen Sie ihn in das Textfeld **IdP Einheiten ID** .
    2.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **IdP Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** .
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **IdP Abmeldung URL** einzufügen.
    4.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie ein binäres Zertifikat in eine Textdatei konvertiert](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **IdP Zertifikat** 6.  Klicken Sie auf **Änderungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei Adobe EchoSign aktivieren zu können, müssen diese in Adobe EchoSign bereitgestellt werden.  
Im Falle von Adobe EchoSign ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Adobe EchoSign** an.

2.  Klicken Sie im Menü oben klicken Sie auf **Konto**, und klicken Sie dann im Navigationsbereich auf die Links Die auf **Benutzer und Gruppen**, und klicken Sie dann auf **Erstellen eines neuen Benutzers**.

    ![Konto] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Konto")

3.  Führen Sie im Abschnitt **Erstellen neuer Benutzer** die folgenden Schritte aus:

    ![Erstellen Sie Benutzer] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Erstellen Sie Benutzer")

    1.  Geben Sie die **E-Mail-Adresse**, **vor-** und **Nachnamen** eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer erstellen**.

        >[AZURE.NOTE] Der Inhaber des Azure Active Directory-Konto erhalten eine e-Mail-Nachricht, die enthält einen Link, um das Konto zu bestätigen, bevor sie aktiviert wurde.

>[AZURE.NOTE] Alle anderen Adobe EchoSign Benutzer Konto Creation Tools können oder APIs bereitgestellt von Adobe EchoSign bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Zuweisen von Benutzern zu Adobe EchoSign führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Adobe EchoSign **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
