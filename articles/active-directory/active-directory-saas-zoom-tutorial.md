<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Zoom | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Zoom mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Lernprogramm: Azure-Active Directory-Integration in Zoom
  
Ziel dieses Lernprogramms ist die Integration von Azure und Zoom angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Zoom-Mandanten
  
Nach Abschluss dieses Lernprogramms müssen werden die Azure AD-Benutzer, die Sie Zoom zugewiesen haben einzelnen melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Zoom (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md) können
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Zoom
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Szenario")

##<a name="enabling-the-application-integration-for-zoom"></a>Aktivieren die Anwendungsintegration für Zoom
  
In diesem Abschnitt Ziel ist es zu gliedernden So aktivieren Sie die Anwendungsintegration für Zoom.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Zoom aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Vergrößern**.

    ![Katalog der Anwendung] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Vergrößern**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Vergrößern] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Vergrößern")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren auf Zoom mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf die Anwendung Integration Zeichenblatt **Zoomen** klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie, wie Benutzer bei Zoom, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Vergrößern melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*http://company.zoom.us*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zoom** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Zoom als Administrator.

6.  Klicken Sie auf der Registerkarte **Einmaliges Anmelden** .

    ![Einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Einmaliges Anmelden")

7.  Klicken Sie auf die Registerkarte **Sicherheit** , und wechseln Sie dann zu der Einstellungen für **Einmaliges Anmelden** .

8.  Führen Sie im Abschnitt einmaliges Anmelden die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Einmaliges Anmelden")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zoom** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **Anmeldung Seiten-URL** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zoom** kopieren Sie den Wert für die **Einzelnen Sign-Out Dienst-URL** , und fügen Sie ihn in das Textfeld **Abmeldung Seiten-URL** .
    3.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    4.  Öffnen des Base-64-codierten Zertifikats in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Identität Anbieter Zertifikat**
    5.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zoom** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber** .
    6.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzer zur Anmeldung bei Zoom aktivieren zu können, müssen er in Zoom bereitgestellt werden.  
Wenn Zoom ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Vergrößern** an.

2.  Klicken Sie auf der Registerkarte **Konto-Verwaltung** , und klicken Sie dann auf **Benutzermanagement**.

3.  Klicken Sie im Abschnitt Benutzermanagement auf **Benutzer hinzufügen**.

    ![Verwaltung der Benutzer] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Verwaltung der Benutzer")

4.  Führen Sie auf der Seite **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Hinzufügen von Benutzern")

    1.  Wählen Sie als **Benutzertyp** **grundlegende**Schritte aus.
    2.  Geben Sie in das Textfeld **E-Mails** die e-Mail-Adresse eines gültigen AAD-Kontos ein, die Sie bereitstellen möchten.
    3.  Klicken Sie auf **Hinzufügen**.

>[AZURE.NOTE] Alle anderen Zoom Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Zoom bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Zuweisen von Benutzern zu Zoom führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf die **Zoom **-Anwendung Integrationsseite klicken Sie auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).