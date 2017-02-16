<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Thoughtworks statt | Microsoft Azure" 
    description="Erfahren Sie, wie Thoughtworks statt mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Lernprogramm: Azure-Active Directory-Integration in Thoughtworks statt.
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Thoughtworks statt anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Thoughtworks statt.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Integration für Thoughtworks statt der Anwendung
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Szenario")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Aktivieren die Integration für Thoughtworks statt der Anwendung
  
Ziel dieses Abschnitts ist es, wie die Telefonintegration für Thoughtworks statt der Anwendung gliedern.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Um die Integration für Thoughtworks statt der Anwendung zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Thoughtworks statt**.

    ![Katalog der Anwendung] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Statt Thoughtworks**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Statt ThoughtWorks] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Statt ThoughtWorks")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Thoughtworks statt mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie ein Zertifikat zum Thoughtworks statt hochladen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der **Thoughtworks statt **Integrationsseite Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei Sie Thoughtworks statt auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Konfigurieren einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Thoughtworks statt Mandanten URL** Geben Sie Ihre URL dem folgenden Muster "*http://company.mingle.thoughtworks.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am Thoughtworks statt** klicken Sie auf Download Metadaten, und klicken Sie dann auf Ihrem Computer gespeichert.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Konfigurieren einmaliges Anmelden")

5.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Thoughtworks statt** an.

6.  Klicken Sie auf **die Registerkarte** , und klicken Sie dann auf **SSO Config**.

    ![SSO Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO Config")

7.  Führen Sie im Abschnitt **Config SSO** die folgenden Schritte aus:

    ![SSO Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO Config")

    1.  Um die Metadatendatei hochladen möchten, klicken Sie auf **Datei auswählen**.
    2.  Klicken Sie auf **Änderungen speichern**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Konfigurieren einmaliges Anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Für AAD Benutzer anmelden können müssen sie zur Verwendung ihrer Azure Active Directory-Benutzernamen Thoughtworks statt Anwendung bereitgestellt werden.  
Im Falle von Thoughtworks statt ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite Thoughtworks statt an.

2.  Klicken Sie auf **Profil**.

    ![Des Projekts] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Des Projekts")

3.  Klicken Sie auf **die Registerkarte** , und klicken Sie dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Benutzer")

4.  Klicken Sie auf **Neuer Benutzer**.

    ![Neuer Benutzer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Neuer Benutzer")

5.  Klicken Sie auf der Seite **Neuer Benutzer** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Neuer Benutzer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Neuer Benutzer")

    1.  Geben Sie der **Anmeldename**, **Anzeigename**, **auswählen, Ihr Kennwort ein**, **bestätigen das Kennwort** eines gültigen AAD ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie als **Benutzertyp** **vollständige Benutzer**aus.
    3.  Klicken Sie auf **dieses Profil erstellen**.

>[AZURE.NOTE] Alle anderen Thoughtworks statt Benutzer Konto Creation Tools können oder APIs bereitgestellt, indem Sie statt Thoughtworks bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Zuweisen von Benutzern zu Thoughtworks statt führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der **Thoughtworks statt** Anwendung Integrationsseite auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
