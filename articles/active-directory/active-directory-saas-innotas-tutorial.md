<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Innotas | Microsoft Azure"
    description="Erfahren Sie, wie Innotas mit Azure Active Directory verwenden, aktivieren Sie einmaliges Anmelden, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-innotas"></a>Lernprogramm: Azure-Active Directory-Integration in Innotas
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Innotas anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten Innotas
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Innotas zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Innotas (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Innotas
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-innotas-tutorial/IC777331.png "Szenario")
##<a name="enabling-the-application-integration-for-innotas"></a>Aktivieren die Anwendungsintegration für Innotas
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Innotas aktiviert.

###<a name="to-enable-the-application-integration-for-innotas-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Innotas aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-innotas-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-innotas-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-innotas-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-innotas-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Innotas**.

    ![Katalog der Anwendung] (./media/active-directory-saas-innotas-tutorial/IC777332.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Innotas aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Innotas] (./media/active-directory-saas-innotas-tutorial/IC777333.png "Innotas")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Innotas mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Innotas** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-innotas-tutorial/IC777334.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Innotas auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-innotas-tutorial/IC777335.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Innotas melden Sie sich In die URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>. Innotas.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-innotas-tutorial/IC777336.png "Konfigurieren der app-URL")

4.  Klicken Sie auf das **Konfigurieren einmaliges Anmelden bei Innotas** Seite zum Herunterladen der Metadaten Ihrer, klicken Sie auf **Herunterladen von Metadaten**und dann die Daten Datei lokal als **c:\\InnotasMetaData.xml**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-innotas-tutorial/IC777337.png "Konfigurieren einmaliges Anmelden")

5.  Weiterleiten dieser Metadatendatei zu Innotas-Supportteam. Das Support-Team muss konfiguriert einmaliges Anmelden für Sie.

6.  Wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-innotas-tutorial/IC777338.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer zu Innotas provisioning konfigurieren.  
Wenn Sie ein zugeordneten Benutzer versucht, die bei Verwendung der Access-Systemsteuerung Innotas anzumelden, überprüft Innotas an, ob der Benutzer vorhanden ist.  
Wenn es noch kein Benutzerkonto verfügbar ist, wird es von Innotas automatisch erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-innotas-perform-the-following-steps"></a>Zuweisen von Benutzern zu Innotas führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Innotas **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-innotas-tutorial/IC777339.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-innotas-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).