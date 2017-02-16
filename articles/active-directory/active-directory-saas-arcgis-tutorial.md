<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in ArcGIS | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von ArcGIS mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Lernprogramm: Azure-Active Directory-Integration in ArcGIS

Das Ziel dieses Lernprogramms ist die Integration von Azure und ArcGIS anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine ArcGIS einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie ArcGIS zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite ArcGIS (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für ArcGIS
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Szenario")
##<a name="enabling-the-application-integration-for-arcgis"></a>Aktivieren die Anwendungsintegration für ArcGIS

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für ArcGIS aktiviert.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Wenn die Anwendungsintegration für ArcGIS aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **ArcGIS**.

    ![Applcation-Katalog] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Applcation-Katalog")

7.  Wählen Sie im Ergebnisbereich **ArcGIS aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu ArcGIS mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **ArcGIS** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der ArcGIS auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Konfigurieren Sie einmaliges Anmelden")

3.  Auf der Seite **Konfigurieren der App-URL** in das Textfeld **ArcGIS melden Sie sich In URL** ein Geben Sie die URL, die die Benutzer zum Melden Sie sich mit dem folgenden Muster "*https://company.maps.arcgis.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei ArcGIS** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens ArcGIS als Administrator.

6.  Klicken Sie auf **Einstellungen bearbeiten**.

    ![Bearbeiten der Einstellungen] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Bearbeiten der Einstellungen")

7.  Klicken Sie auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Sicherheit")

8.  Klicken Sie unter **Enterprise-Benutzernamen**klicken Sie auf **Identitätsanbieter festlegen**.

    ![Enterprise-Benutzernamen] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Enterprise-Benutzernamen")

9.  Führen Sie auf der Konfigurationsseite **Identitätsanbieter legen Sie** die folgenden Schritte aus:

    ![Festlegen der Identitätsanbieter] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Festlegen der Identitätsanbieter")

    1.  Geben Sie in das Textfeld Name den Namen Ihrer Organisation ein.
    2.  Wählen Sie für die **Metadaten für die Enterprise-Identitätsanbieter mit bereitgestellt werden** **Eine Datei**aus.
    3.  Um die heruntergeladene Metadatendatei hochladen möchten, klicken Sie auf **Datei auswählen**.
    4.  Klicken Sie auf **Identitätsanbieter festlegen**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei ArcGIS zu ermöglichen, müssen er in ArcGIS bereitgestellt werden.  
Im Falle von ArcGIS ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten **ArcGIS** .

2.  Klicken Sie auf **Mitglieder einladen**.

    ![Mitglieder einladen] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Mitglieder einladen")

3.  Wählen Sie **Mitglieder hinzufügen automatisch, ohne eine e-Mail zu senden**, und klicken Sie dann auf **Weiter**.

    ![Automatisches Hinzufügen von Mitgliedern] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Automatisches Hinzufügen von Mitgliedern")

4.  Klicken Sie auf der Seite **Mitglieder** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Hinzufügen und überprüfen] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Hinzufügen und überprüfen")

    1.  Geben Sie die **Vorname**, **Nachname** und **E-Mail** mit einer gültigen gewünschte AAD Konto bereitstellen.
    2.  Klicken Sie auf **Hinzufügen und überprüfen**.

5.  Überprüfen Sie die Daten, die Sie eingegeben haben, und klicken Sie dann auf **Mitglieder hinzufügen**.

    ![Mitglied hinzufügen] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Mitglied hinzufügen")

>[AZURE.NOTE] Alle anderen ArcGIS Benutzer Konto Creation Tools können oder APIs bereitgestellt durch ArcGIS bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Zuweisen von Benutzern zu ArcGIS führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **ArcGIS **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
