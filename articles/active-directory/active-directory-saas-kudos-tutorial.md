<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Kudos | Microsoft Azure" 
    description="Erfahren Sie, wie Kudos mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Lernprogramm: Azure-Active Directory-Integration in Kudos
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Kudos anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Kudos
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Kudos zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre Kudos Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Kudos
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Szenario")
##<a name="enabling-the-application-integration-for-kudos"></a>Aktivieren die Anwendungsintegration für Kudos
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Kudos aktiviert.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Kudos aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Kudos**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Katalog der Anwendung")

7.  Im Ergebnisbereich **Kudos**wählen Sie aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Kudos] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Kudos")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Kudos mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Kudos** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Kudos auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie Ihre URL dem folgenden Muster "*https://company.kudosnow.com*" auf der Seite **Konfigurieren der App-URL** in das Textfeld **Kudos melden Sie sich auf URL** , und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-kudos-tutorial/IC787804.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Kudos** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Kudos als Administrator.

6.  Klicken Sie auf **Einstellungen**, klicken Sie im Menü oben.

    ![Einstellungen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Einstellungen")

7.  Klicken Sie auf **Integration \> SSO**.

8.  Führen Sie im Abschnitt **SSO** die folgenden Schritte aus:

    ![SSO] (./media/active-directory-saas-kudos-tutorial/IC787807.png "SSO")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Kudos** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **URL anmelden** .
    2.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP]
        Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    3.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **x 509-Zertifikat**
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Kudos** kopieren Sie den Wert für die **Einzelnen Sign-Out Dienst-URL** , und fügen Sie ihn in das Textfeld **An URL Abmelden** .
    5.  Geben Sie im Textfeld **Ihre Kudos-URL** den Namen Ihres Unternehmens ein.
    6.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei Kudos aktivieren zu können, müssen er in Kudos bereitgestellt werden.  
Wenn Kudos ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Kudos** an.

2.  Klicken Sie auf **Einstellungen**, klicken Sie im Menü oben.

    ![Einstellungen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Einstellungen")

3.  Klicken Sie auf **Benutzeradministrator**.

4.  Klicken Sie auf der Registerkarte **Benutzer** , und klicken Sie dann auf **Benutzer hinzufügen**.

    ![Benutzeradministrator] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Benutzeradministrator")

5.  Führen Sie im Abschnitt **Hinzufügen eines Benutzers** die folgenden Schritte aus:

    ![Hinzufügen eines Benutzers] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Hinzufügen eines Benutzers")

    1.  Geben Sie den **Vornamen**, **Nachnamen**, **E-Mail** und andere Details eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer erstellen**.

>[AZURE.NOTE]Alle anderen Kudos Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Kudos bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Zuweisen von Benutzern zu Kudos führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Kudos **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).