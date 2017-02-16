<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in EmpCenter | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von EmpCenter mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Lernprogramm: Azure-Active Directory-Integration in EmpCenter
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und EmpCenter anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine EmpCenter einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie EmpCenter zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite EmpCenter (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für EmpCenter
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Szenario")
##<a name="enabling-the-application-integration-for-empcenter"></a>Aktivieren die Anwendungsintegration für EmpCenter
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für EmpCenter aktiviert.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Wenn die Anwendungsintegration für EmpCenter aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **EmpCenter**.

    ![Katalog der Anwendung] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **EmpCenter aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu EmpCenter mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **EmpCenter** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der EmpCenter auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Einstellungen für die App konfigurieren** die folgenden Schritte aus:

    ![Konfigurieren der App-Einstellungen] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Konfigurieren der App-Einstellungen")

    1.  Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL, die von den Benutzern die Anmeldung an Ihrer Anwendung EmpCenter verwendet (z. B.: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Klicken Sie auf **Weiter**

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei EmpCenter** zum Herunterladen der Metadaten Ihrer auf **Metadaten herunterladen**und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Konfigurieren Sie einmaliges Anmelden")

5.  Senden Sie die heruntergeladene Metadatendatei an Ihr Supportteam EmpCenter ein.

    >[AZURE.NOTE] Führen Sie die eigentliche SSO-Konfiguration muss das Supportteam EmpCenter.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei EmpCenter zu ermöglichen, müssen er in EmpCenter bereitgestellt werden.  
Bei EmpCenter müssen die Benutzerkonten von Ihrem Supportteam EmpCenter erstellt werden.

>[AZURE.NOTE] Alle anderen EmpCenter Benutzer Konto Creation Tools können oder APIs von bereitgestellten EmpCenter Bereitstellen von Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Zuweisen von Benutzern zu EmpCenter führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **EmpCenter **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).