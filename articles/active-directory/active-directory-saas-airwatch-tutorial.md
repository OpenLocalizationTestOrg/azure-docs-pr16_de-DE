<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in AirWatch | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von AirWatch mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Lernprogramm: Azure-Active Directory-Integration in AirWatch

Das Ziel dieses Lernprogramms ist die Integration von Azure und AirWatch anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine AirWatch einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie AirWatch zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite AirWatch (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für AirWatch
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>Aktivieren die Anwendungsintegration für AirWatch

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für AirWatch aktiviert.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Wenn die Anwendungsintegration für AirWatch aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **AirWatch**.

    ![Katalog der Anwendung] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **AirWatch aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu AirWatch mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **AirWatch** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der AirWatch auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie Ihre URL, die von den Benutzern verwendet, um an Ihrer Anwendung AirWatch melden Sie sich auf der Seite **Konfigurieren der App-URL** in das Textfeld **AirWatch melden Sie sich auf URL** (z. B.: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei AirWatch** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens AirWatch als Administrator.

6.  Klicken Sie im linken Navigationsbereich klicken Sie auf **Konten**, und klicken Sie dann auf **Administratoren**.

    ![Administratoren] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administratoren")

7.  Erweitern Sie im Menü **Einstellungen** , und klicken Sie dann auf **Directory Services**.

    ![Einstellungen] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Einstellungen")

8.  Klicken Sie auf der Registerkarte **Benutzer** in das **Basis-DN** , geben Sie Ihren Domänennamen ein, und klicken Sie dann auf **Speichern**.

    ![Benutzer] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Benutzer")

9.  Klicken Sie auf der Registerkarte **Server** .

    ![Server] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. Führen Sie die folgenden Schritte aus:

    ![Hochladen] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Hochladen")

    1.  Wählen Sie als **Verzeichnistyp** **keine**aus.
    2.  Wählen Sie **verwenden SAML für die Authentifizierung**ein.
    3.  Um die heruntergeladene Zertifikat hochladen möchten, klicken Sie auf **Hochladen**.

11. Führen Sie im Abschnitt **anfordern** die folgenden Schritte aus:

    ![Anfordern] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Anfordern")

    1.  Wählen Sie als **Bindungstyp anfordern** **Bereitstellen**.
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Airwatch** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und klicken Sie dann in das Textfeld **Identität Anbieter einzelne melden Sie sich auf URL** einzufügen.
    3.  Wählen Sie als **NameID Format** **E-Mail-Adresse**ein.
    4.  Klicken Sie auf **Speichern**.

12. Klicken Sie erneut auf die Registerkarte **Benutzer** .

    ![Benutzer] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Benutzer")

13. Führen Sie im Abschnitt **Attribut** die folgenden Schritte aus:

    ![Attribut] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Attribut")

    1.  Geben Sie in das Textfeld **Objekt-ID** **http://schemas.microsoft.com/identity/claims/objectidentifier**aus.
    2.  Geben Sie in das Textfeld für den **Benutzernamen** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**ein.
    3.  Geben Sie im Textfeld **Anzeigename** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**ein.
    4.  Geben Sie im Textfeld **Vorname** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**ein.
    5.  Geben Sie im Textfeld **Nachname** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**ein.
    6.  Geben Sie in das Textfeld **E-Mail** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**aus.
    7.  Klicken Sie auf **Speichern**.

14. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei AirWatch zu ermöglichen, müssen er in AirWatch bereitgestellt werden.  
Im Falle von AirWatch ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **AirWatch** an.

2.  Klicken Sie im Navigationsbereich auf der linken Seite klicken Sie auf **Konten**, und klicken Sie dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Benutzer")

3.  Klicken Sie im Menü **Benutzer** klicken Sie auf die **Listenansicht**, und klicken Sie dann auf **Hinzufügen \> Benutzer hinzufügen**.

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Fügen Sie Benutzer hinzu")

4.  Klicken Sie im Dialogfeld **hinzufügen / bearbeiten Benutzer** führen Sie die folgenden Schritte aus:

    ![Fügen Sie Benutzer hinzu] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Fügen Sie Benutzer hinzu")

    1.  Geben Sie den **Benutzernamen**, **Kennwort**, **Kennwort bestätigen**, **Vorname**, **Nachname**, **E-Mail-Adresse** eines gültigen Azure Active Directory-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen AirWatch Benutzer Konto Creation Tools können oder APIs bereitgestellt durch AirWatch bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Zuweisen von Benutzern zu AirWatch führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **AirWatch **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
