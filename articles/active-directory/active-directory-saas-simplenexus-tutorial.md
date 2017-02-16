<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in SimpleNexus | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von SimpleNexus mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Lernprogramm: Azure-Active Directory-Integration in SimpleNexus
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und SimpleNexus anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine SimpleNexus einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie SimpleNexus zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite SimpleNexus (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für SimpleNexus
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Szenario")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Aktivieren die Anwendungsintegration für SimpleNexus
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für SimpleNexus aktiviert.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Wenn die Anwendungsintegration für SimpleNexus aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **einfache Nexus**aus.

    ![Katalog der Anwendung] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **SimpleNexus aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Einfache Nexus] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Einfache Nexus")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu SimpleNexus mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **SimpleNexus** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der SimpleNexus auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **SimpleNexus melden Sie sich In die URL** den URL dem folgenden Muster "*https://simplenexus.com/CompanyName\_Login*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei SimpleNexus** klicken Sie auf **Herunterladen von Metadaten**und leiten Sie dann an das Supportteam SimpleNexus Metadaten-Datei.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Konfigurieren Sie einmaliges Anmelden")

    >[AZURE.NOTE] Einmaliges Anmelden muss durch das Supportteam SimpleNexus aktiviert sein.

5.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei SimpleNexus zu ermöglichen, müssen er in SimpleNexus bereitgestellt werden.  
Im Falle von SimpleNexus ist die Bereitstellung eines manuellen Vorgang ausgeführte Arbeit von der mandantenadministrator.

>[AZURE.NOTE] Alle anderen SimpleNexus Benutzer Konto Creation Tools können oder APIs bereitgestellt durch SimpleNexus bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Zuweisen von Benutzern zu SimpleNexus führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **SimpleNexus **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).