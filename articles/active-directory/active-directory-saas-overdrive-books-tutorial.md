<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Overdrive Bücher | Microsoft Azure" 
    description="Erfahren Sie, wie Overdrive Bücher mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Lernprogramm: Azure-Active Directory-Integration in Overdrive Bücher
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und OverDrive anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine OverDrive einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie OverDrive zugewiesen haben können einzelne melden Sie sich bei der Anwendung an Ihre OverDrive Firmenwebsite (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für OverDrive
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Szenario")
##<a name="enabling-the-application-integration-for-overdrive"></a>Aktivieren die Anwendungsintegration für OverDrive
  
So aktivieren Sie die Anwendungsintegration für OverDrive Gliedern ist das Ziel in diesem Abschnitt.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Wenn die Anwendungsintegration für OverDrive aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **OverDrive**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Katalog der Anwendung")

7.  Im Ergebnisbereich **OverDrive**wählen Sie aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![OverDrive] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu OverDrive mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **OverDrive** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Einmaliges Anmelden aktivieren")

2.  Klicken Sie auf der Seite **Wie möchten Sie, wie Benutzer bei OverDrive, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **OverDrive melden Sie sich In URL** ein Ihre URL dem folgenden Muster "*http://mslibrarytest.libraryreserve.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der **Konfigurieren einmaliges Anmelden bei OverDrive** Seite zum Herunterladen der Metadatendatei und senden Sie ihn an das Supportteam OverDrive.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Konfigurieren einmaliges Anmelden")

    >[AZURE.NOTE]Das Supportteam OverDrive einmaliges Anmelden für Sie konfiguriert und erhalten Sie eine Benachrichtigung, wenn die Konfiguration abgeschlossen ist.

5.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer bereitgestellt werden, um OverDrive konfigurieren.  
Wenn ein zugewiesene Benutzer zur Anmeldung bei OverDrive versucht, wird ein Konto OverDrive bei Bedarf automatisch erstellt.

>[AZURE.NOTE]Alle anderen OverDrive Benutzer Konto Creation Tools können oder APIs bereitgestellt durch OverDrive bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Zuweisen von Benutzern zu OverDrive führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **OverDrive **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).