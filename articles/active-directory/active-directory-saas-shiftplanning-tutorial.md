<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in ShiftPlanning | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von ShiftPlanning mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-shiftplanning"></a>Lernprogramm: Azure-Active Directory-Integration in ShiftPlanning
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und ShiftPlanning anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine ShiftPlanning einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie ShiftPlanning zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite ShiftPlanning (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für ShiftPlanning
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-shiftplanning-tutorial/IC786612.png "Szenario")
##<a name="enabling-the-application-integration-for-shiftplanning"></a>Aktivieren die Anwendungsintegration für ShiftPlanning
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für ShiftPlanning aktiviert.

###<a name="to-enable-the-application-integration-for-shiftplanning-perform-the-following-steps"></a>Wenn die Anwendungsintegration für ShiftPlanning aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-shiftplanning-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-shiftplanning-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-shiftplanning-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-shiftplanning-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **ShiftPlanning**.

    ![Katalog der Anwendung] (./media/active-directory-saas-shiftplanning-tutorial/IC786613.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **ShiftPlanning aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![ShiftPlanning] (./media/active-directory-saas-shiftplanning-tutorial/IC786614.png "ShiftPlanning")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu ShiftPlanning mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **ShiftPlanning** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-shiftplanning-tutorial/IC786615.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der ShiftPlanning auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-shiftplanning-tutorial/IC786616.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **ShiftPlanning melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*https://company.shiftplanning.com/includes/saml/*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-shiftplanning-tutorial/IC786617.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei ShiftPlanning** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-shiftplanning-tutorial/IC786618.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens **ShiftPlanning** als Administrator.

6.  Klicken Sie oben im Menü auf **Administrator**.

    ![Administrator] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Administrator")

7.  Klicken Sie unter **Integration**klicken Sie auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden] (./media/active-directory-saas-shiftplanning-tutorial/IC786620.png "Einmaliges Anmelden")

8.  Führen Sie im Abschnitt **Einmaliges Anmelden** die folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-shiftplanning-tutorial/IC786905.png "Einmaliges Anmelden")

    1.  Wählen Sie **SAML aktiviert**.
    2.  Wählen Sie **Kennwort Anmeldung erlauben**
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei ShiftPlanning** kopieren Sie des Werts **Remote Anmelde-URL** , und klicken Sie dann in das Textfeld **SAML Herausgeber URL** einzufügen.
    4.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei ShiftPlanning** kopieren Sie des Werts **Remote Abmeldung-URL** , und fügen Sie ihn in das Textfeld **Remote Abmeldung-URL** .
    5.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    6.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **X 509-Zertifikat**
    7.  Klicken Sie auf **Einstellungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-shiftplanning-tutorial/IC786621.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei ShiftPlanning zu ermöglichen, müssen er in ShiftPlanning bereitgestellt werden.  
Im Falle von ShiftPlanning ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **ShiftPlanning** an.

2.  Klicken Sie auf **Administrator**.

    ![Administrator] (./media/active-directory-saas-shiftplanning-tutorial/IC786619.png "Administrator")

3.  Klicken Sie auf **Personal**.

    ![Personal] (./media/active-directory-saas-shiftplanning-tutorial/IC786623.png "Personal")

4.  Klicken Sie unter **Aktionen**auf **Mitarbeiter hinzufügen**.

    ![Hinzufügen von Mitarbeitern] (./media/active-directory-saas-shiftplanning-tutorial/IC786624.png "Hinzufügen von Mitarbeitern")

5.  Führen Sie im Abschnitt **Hinzufügen von Mitarbeitern** die folgenden Schritte aus:

    ![Speichern der Mitarbeiter] (./media/active-directory-saas-shiftplanning-tutorial/IC786625.png "Speichern der Mitarbeiter")

    1.  Geben Sie den **Vornamen**, **Nachnamen** und **E-Mail** mit einem gültigen AAD-Konto, die, das Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Mitarbeiter speichern**.

>[AZURE.NOTE]Alle anderen ShiftPlanning Benutzer Konto Creation Tools können oder APIs bereitgestellt durch ShiftPlanning bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-shiftplanning-perform-the-following-steps"></a>Zuweisen von Benutzern zu ShiftPlanning führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **ShiftPlanning **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-shiftplanning-tutorial/IC786626.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-shiftplanning-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).