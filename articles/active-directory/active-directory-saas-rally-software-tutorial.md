<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Zusammenkunft Software | Microsoft Azure" 
    description="Erfahren Sie, wie zur Verwendung von Rally Software mit Azure Active Directory aktivieren einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Lernprogramm: Azure-Active Directory-Integration in Zusammenkunft Software
  
Ziel dieses Lernprogramms ist die Integration von Azure und Rally Software angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Rally Software
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Software Rally
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Szenario")
##<a name="enabling-the-application-integration-for-rally-software"></a>Aktivieren die Anwendungsintegration für Software Rally
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Software Rally aktiviert.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Software Rally der folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Zusammenkunft**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Software Rally aus**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Rally Software] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally Software")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren Software-Rally mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie ein Zertifikat Software-Rally hochladen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite Anwendung Integration **Rally Software **klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie, wie Benutzer bei Zusammenkunft, klicken Sie auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Microsoft Azure AD einmaliges Anmelden] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Rally Software Mandanten URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. rally.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Zusammenkunft** klicken Sie auf Download Metadaten, und klicken Sie dann auf Ihrem Computer gespeichert.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Konfigurieren einmaliges Anmelden")

5.  Melden Sie sich bei Ihrem Mandanten **Rally Software** .

6.  Klicken Sie in der Symbolleiste oben auf **Setup**, und wählen Sie dann auf **Abonnements**.

    ![Abonnement] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Abonnement")

7.  Klicken Sie auf die **interaktive** Schaltfläche auf der Symbolleiste oben auf der rechten Seite, und wählen Sie dann die **Anmeldung bearbeiten**.

8.  Führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Speichern und schließen**, auf der Seite **Abonnements** Dialogfeld:

    ![Authentifizierung] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Authentifizierung")

    1.  Wählen Sie aus der Dropdownliste Authentifizierung **Zusammenkunft oder SSO-Authentifizierung**
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Rally Software** kopieren Sie des Werts **Identität Anbieter-ID** , und fügen Sie ihn in das Textfeld **Identität Provider-URL**
    3.  Kopieren Sie im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Rally Software** des **Remote Abmeldung URL** -Werts.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Für AAD Benutzer anmelden können müssen er zur Verwendung ihrer Azure Active Directory-Benutzernamen Rally Software Anwendung bereitgestellt werden.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich bei Ihrem Mandanten Rally Software.

2.  Wechseln Sie zu **Setup \> Benutzer**, und klicken Sie dann auf **+ Add New**.

    ![Benutzer] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Benutzer")

3.  Geben Sie den Namen in das Textfeld neuen Benutzer ein, und klicken Sie dann auf **mit Details hinzufügen**.

4.  Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:

    ![Erstellen Sie Benutzer] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Erstellen Sie Benutzer")

    1.  Geben Sie in das Textfeld **Benutzername** den Namen des Benutzers Azure AD-, die Sie bereitstellen möchten.
    2.  Geben Sie in das Textfeld **E-Mail-Adresse** die e-Mail-Adresse des Benutzers Azure AD-, die Sie bereitstellen möchten.
    3.  Klicken Sie auf **Speichern und schließen**.

>[AZURE.NOTE]Alle anderen Rally Software Benutzer Konto Creation Tools können oder APIs zur Verfügung gestellt von Rally Software bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Zuweisen von Benutzern zu Rally Software führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite Anwendung Integration **Rally Software** auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).




