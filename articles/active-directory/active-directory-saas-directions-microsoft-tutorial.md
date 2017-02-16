<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration mit Anweisungen zum Microsoft | Microsoft Azure" 
    description="Erfahren Sie, wie einmaliges Anmelden, automatisierte Bereitstellung und mehr Aktivierung erfahren Sie, wie auf Microsoft Azure Active Directory mithilfe!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Lernprogramm: Azure-Active Directory-Integration mit Anweisungen zum Microsoft

Das Ziel dieses Lernprogramms ist die Integration von Azure Active Directory und erfahren Sie, wie Microsoft anzuzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine erfahren Sie, wie Microsoft-Abonnements

Wenn Sie eine partnerverbundkontakte erfahren Sie, wie auf noch Microsoft-Abonnement besitzen, per e-Mail eine Anforderung an "*service@DirectionsOnMicrosoft.com*".

Am Ende dieses Lernprogramms, werden die Azure Active Directory-Benutzer, die Sie Anweisungen zum Microsoft zugewiesen haben können einzelne melden Sie sich bei der Anwendung mit einmaliges Anmelden.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Integration der Anwendung um Anweisungen zum Microsoft
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Szenario")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Aktivieren die Integration der Anwendung um Anweisungen zum Microsoft

Das Ziel der in diesem Abschnitt ist so aktivieren Sie die Integration der Anwendung um Anweisungen zum Microsoft gliedern.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration um Anweisungen zum Microsoft die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **erfahren Sie, wie Sie auf Microsoft**.

    ![Katalog der Anwendung] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich die **Anweisungen auf Microsoft**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Szenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Szenario")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu erfahren Sie, wie Sie auf Microsoft mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **erfahren Sie, wie Sie auf Microsoft** Anwendung Integration klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Einmaliges Anmelden aktivieren")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Anweisungen zum Microsoft auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Microsoft Azure AD Singel einmaliges Anmelden] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld melden Sie sich auf URL Geben Sie **https://www.directionsonmicrosoft.com/user/login**, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei erfahren Sie, wie Sie auf Microsoft** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Konfigurieren Sie einmaliges Anmelden")

5.  Die Metadatendatei an den Anweisungen auf der Microsoft-Support-Team senden (*service@DirectionsOnMicrosoft.com*). Klicken Sie zum Aktivieren der Anweisungen zum Microsoft-Support-Team Ihre Websitegruppenmitgliedschaft partnerverbundkontakte suchen wird einbeziehen Sie der Firmeninformationen in Ihre e-Mail-Adresse.

    >[AZURE.NOTE] Einmaliges Anmelden für eine Anleitung Microsoft muss aktiviert sein, indem Sie die Anweisungen auf der Microsoft-Supportteam.
Sie erhalten eine Benachrichtigung, wenn einmaliges Anmelden aktiviert wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Es gibt keine Aktionselement für Sie so konfigurieren Sie Benutzer zu erfahren Sie, wie Sie auf Microsoft bereitgestellt.  
Wenn ein zugewiesene Benutzer zur Anmeldung bei Anweisungen zum Microsoft mithilfe des Bedienfelds Access versucht, ein Anweisungen zum Microsoft-überprüft, ob der Benutzer vorhanden ist. Wenn es noch kein Benutzerkonto verfügbar ist, wird es von Anweisungen zum Microsoft automatisch erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Zuweisen von Benutzern zu erfahren Sie, wie Sie auf Microsoft führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **erfahren Sie, wie Sie auf Microsoft **Anwendung Integration auf **Zuweisen von Benutzern**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Ja")
