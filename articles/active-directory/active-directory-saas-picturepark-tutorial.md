<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Picturepark | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Picturepark mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Lernprogramm: Azure-Active Directory-Integration in Picturepark
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Picturepark anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Picturepark
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Picturepark zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Picturepark (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Picturepark
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Szenario")

##<a name="enabling-the-application-integration-for-picturepark"></a>Aktivieren die Anwendungsintegration für Picturepark
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Picturepark aktiviert.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Picturepark aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Picturepark**.

    ![Katalog der Anwendung] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Picturepark aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Picturepark mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für Picturepark konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie ein Zertifikat Fingerabdruckwert abgerufen](http://youtu.be/YKQF266SAxI)...

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Picturepark** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Picturepark auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Picturepark melden Sie sich auf URL** Geben Sie Ihre URL dem folgenden Muster "*http://company.picturepark.com*" ein, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Picturepark** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Picturepark als Administrator.

6.  Klicken Sie in der Symbolleiste oben auf **Verwaltung**, und klicken Sie dann auf **Verwaltungskonsole**.

    ![-Verwaltungskonsole] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Verwaltungskonsole")

7.  Klicken Sie auf **Authentifizierung**, und klicken Sie dann auf **Identitätsanbieter**.

    ![Authentifizierung] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Authentifizierung")

8.  Führen Sie im Abschnitt **Konfiguration des Anbieters Identität** die folgenden Schritte aus:

    ![Die Konfiguration des Anbieters Identität] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Die Konfiguration des Anbieters Identität")

    1.  Klicken Sie auf **Hinzufügen**.
    2.  Geben Sie einen Namen für die Konfiguration ein.
    3.  Wählen Sie **als Standard festlegen**.
    4.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Picturepark** im Azure klassischen Portal des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **Herausgeber URI** .
    5.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Vertrauenswürdige Herausgeber Ziehpunkt drucken** .  

        >[AZURE.TIP]Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    6.  Klicken Sie auf **JoinDefaultUsersGroup**.
    7.  Geben Sie zum Festlegen des **Emailaddress** Attributs in das Textfeld **anfordern** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**ein.
        ![Konfiguration] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Konfiguration")
    8.  Klicken Sie auf **Speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Konfigurieren Sie einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Picturepark zu ermöglichen, müssen er in Picturepark bereitgestellt werden.  
Im Falle von Picturepark ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Picturepark** .

2.  Klicken Sie oben auf der Symbolleiste klicken Sie auf **Verwaltung**, und klicken Sie dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Benutzer")

3.  Klicken Sie auf der Registerkarte **Übersicht über die Benutzer** auf **neu**.

    ![Verwaltung der Benutzer] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Verwaltung der Benutzer")

4.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **Benutzer erstellen** :

    ![Erstellen Sie Benutzer] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Erstellen Sie Benutzer")

    1.  Typ der: **e-Mail-Adresse**, **Ihr Kennwort ein**, **bestätigen ein Kennwort**, **Vorname**, **Nachname**, **Firma**, **Land**, **ZIP**, **Ort** der gültigen Azure Active Directory-Benutzers ot Bereitstellen in den verknüpften Textfeldern werden soll.
    2.  Wählen Sie eine **Sprache**aus.
    3.  Klicken Sie auf **Erstellen**.

>[AZURE.NOTE]Alle anderen Picturepark Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Picturepark bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Zuweisen von Benutzern zu Picturepark führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Picturepark **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).