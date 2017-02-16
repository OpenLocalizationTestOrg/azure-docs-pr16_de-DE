<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in e-Generators | Microsoft Azure" 
    description="Erfahren Sie, wie zur Verwendung von e-Generators mit Azure Active Directory aktivieren einmaliges Anmelden, automatisierte bereitgestellt und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Lernprogramm: Azure-Active Directory-Integration in e-Generator
  
Ziel dieses Lernprogramms ist die Integration von Azure und e-Generators angezeigt.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einem e-Generator-Mandanten
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie-e-Generator zugewiesen haben können einzelne melden Sie sich bei der Anwendung an der Website Ihres Unternehmens-e-Generator (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für e-Generator
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Szenario")
##<a name="enabling-the-application-integration-for-e-builder"></a>Aktivieren die Anwendungsintegration für e-Generator
  
In diesem Abschnitt Ziel ist es zu gliedernden So aktivieren Sie die Anwendungsintegration für e-Generators.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Um die Anwendungsintegration für e-Generators zu aktivieren, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **E-Generators**.

    ![Katalog der Anwendung] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **E-Generator aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![E-Generator] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "E-Generator")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren e-Generators mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **E-Generator-** Anwendung Integration klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Konfigurieren einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der e-Generators auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **e-Generator anmelden URL** den URL dem folgenden Muster "*https://\<Mandanten-Name\>.e-Builder.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Konfigurieren der app-URL")

4.  Klicken Sie auf das **Konfigurieren einmaliges Anmelden bei e-Generator-** Seite zum Herunterladen der Metadaten Ihrer, klicken Sie auf **Herunterladen von Metadaten**und dann die Daten Datei lokal als **c:\\e-BuilderMetaData.xml**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Konfigurieren einmaliges Anmelden")

5.  Weiterleiten dieser Metadatendatei-e-Generator-Supportteam. Das Support-Team muss konfiguriert einmaliges Anmelden für Sie.

6.  Wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Es gibt keine Aktionselement für Ihre Benutzer-e-Generator provisioning konfigurieren.  
Wenn ein zugewiesene Benutzer zur Anmeldung bei e-Generators mithilfe des Bedienfelds Access versucht, überprüft-e-Generator an, ob der Benutzer vorhanden ist.  
Wenn es noch kein Benutzerkonto verfügbar ist, wird es von e-Generator automatisch erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Zuweisen von Benutzern zu e-Generators führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **E-Generator- **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
