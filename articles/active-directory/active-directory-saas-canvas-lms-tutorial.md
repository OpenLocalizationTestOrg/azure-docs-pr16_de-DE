<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Zeichenbereich LMS | Microsoft Azure" 
    description="Erfahren Sie, wie Zeichenbereich LMS mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Lernprogramm: Azure-Active Directory-Integration in Zeichenbereich LMS

Ziel dieses Lernprogramms ist die Integration von Azure und Zeichenbereich angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Zeichenbereich

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Zeichenbereich zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Zeichenbereich (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Zeichenbereich
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Szenario")
##<a name="enabling-the-application-integration-for-canvas"></a>Aktivieren die Anwendungsintegration für Zeichenbereich

In diesem Abschnitt Ziel ist es zu gliedernden So aktivieren Sie die Anwendungsintegration für Zeichenbereich.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Zeichenbereich die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Zeichenbereich**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Zeichenbereich aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Zeichenbereich] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Zeichenbereich")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Zeichnungsbereich mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Einmaliges Anmelden für den Zeichenbereich konfigurieren, müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Zeichenbereich** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Zeichenbereich auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Zeichenbereich melden Sie sich In die URL** den URL dem folgenden Muster `https://<tenant-name>.instructure.com`, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Konfigurieren der App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden bei Zeichenbereich** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Zeichenbereich als Administrator.

6.  Wechseln Sie zu **Kurse finden Sie unter \> verwaltete Konten \> Microsoft**.

    ![Zeichenbereich] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Zeichenbereich")

7.  Klicken Sie im Navigationsbereich auf der linken Seite Wählen Sie **Authentifizierung**aus, und klicken Sie dann auf **Neue SAML-Config hinzufügen**.

    ![Authentifizierung] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentifizierung")

8.  Klicken Sie auf der Seite aktuelle Integration führen Sie die folgenden Schritte aus:

    ![Aktuelle Integration] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Aktuelle Integration")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zeichenbereich** kopieren Sie den Wert der **Element-ID** , und fügen Sie ihn in das Textfeld **IdP Einheiten ID** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zeichenbereich** kopieren Sie des Werts **Remote Anmelde-URL** , und klicken Sie dann in das Textfeld **Log auf URL** einzufügen.
    3.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Zeichenbereich** kopieren Sie des Werts **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **, Protokoll-URL** .
    4.  Kopieren Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zeichenbereich** im Azure klassischen Portal den Wert für die **URL für das Kennwort ändern** , und fügen Sie ihn in das Textfeld **Link ' Kennwort ändern '** .
    5.  Kopieren Sie den **Fingerabdruck** Wert aus dem exportierte Zertifikat, und fügen Sie ihn in das Textfeld **Zertifikat Fingerabdruck** .  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [wie Sie ein Zertifikat Fingerabdruckwert abrufen](http://youtu.be/YKQF266SAxI)

    6.  Wählen Sie in der Liste **Login Attribut** **NameID**aus.
    7.  Wählen Sie aus der Liste **Format Bezeichner** **EmailAddress**aus.
    8.  Klicken Sie auf **Authentifizierungseinstellungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzer zur Anmeldung bei Zeichenbereich aktivieren zu können, müssen diese in den Zeichenbereich bereitgestellt werden.  
Wenn Zeichenbereich ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **Zeichenbereich** .

2.  Wechseln Sie zu **Kurse finden Sie unter \> verwaltete Konten \> Microsoft**.

    ![Zeichenbereich] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Zeichenbereich")

3.  Klicken Sie auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Benutzer")

4.  Klicken Sie auf **neuen Benutzer hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Benutzer")

5.  Führen Sie auf der Seite Dialogfeld neuen Benutzer hinzufügen die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie in das Textfeld **Vollständiger Name** den Namen des Benutzers ein.
    2.  Geben Sie in das Textfeld **-e-Mail** -e-Mail-Adresse des Benutzers ein.
    3.  Geben Sie in das Textfeld **Benutzername** Azure AD-e-Mail-Adresse des Benutzers ein.
    4.  Wählen Sie **E-Mail der Benutzer über das Erstellen von diesem Konto**.
    5.  Klicken Sie auf **Benutzer hinzufügen**.

>[AZURE.NOTE] Alle anderen Zeichenbereich Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Zeichenbereich bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Zuweisen von Benutzern zu Zeichenbereich führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration **Zeichenbereich **auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
