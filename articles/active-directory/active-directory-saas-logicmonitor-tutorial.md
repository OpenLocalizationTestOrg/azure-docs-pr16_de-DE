<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in LogicMonitor | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von LogicMonitor mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Lernprogramm: Azure-Active Directory-Integration in LogicMonitor
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und LogicMonitor anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten LogicMonitor
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für LogicMonitor
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Szenario")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Aktivieren die Anwendungsintegration für LogicMonitor
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für LogicMonitor aktiviert.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Wenn die Anwendungsintegration für LogicMonitor aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Logicmonitor**.

    ![Katalog der Anwendung] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **LogicMonitor aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu LogicMonitor mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **LogicMonitor **Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der LogicMonitor auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Melden Sie sich auf URL** den URL untersuchten Ihre Benutzer bei der LogicMonitor auf \(e, g,: "*http://company.logicmonitor.com*"\), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei LogicMonitor** klicken Sie auf **Herunterladen von Metadaten**aus, und klicken Sie dann auf Ihrem Computer speichern.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Konfigurieren Sie einmaliges Anmelden")

5.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **LogicMonitor** an.

6.  Klicken Sie auf **Einstellungen**, klicken Sie im Menü oben.

    ![Einstellungen] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Einstellungen")

7.  Klicken Sie im Navigationsbereich auf der linken Seite gleich auf **Single Sign On**

    ![Einmaliges Anmelden] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Einmaliges Anmelden")

8.  Führen Sie im Abschnitt **Einstellungen für einmaliges Anmelden (SSO)** die folgenden Schritte aus:

    ![Einstellungen für einzelne Zeichen] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Einstellungen für einzelne Zeichen")

    1.  Kontrollkästchen Sie **einmaliges Anmelden aktivieren**das.
    2.  Wählen Sie als **Standard Rollenzuweisung** **schreibgeschützt**.
    3.  Öffnen Sie die heruntergeladene Metadatendatei in Editor, und fügen Sie in das Textfeld **Identität Anbietermetadaten** Inhalt der Datei.
    4.  Klicken Sie auf **Änderungen speichern**.

9.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Konfigurieren Sie einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Für AAD Benutzer anmelden können müssen diese mit der Verwendung ihrer Azure Active Directory-Benutzernamen LogicMonitor-Anwendung bereitgestellt werden.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite LogicMonitor an.

2.  Klicken Sie im Menü oben klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Rollen und Benutzer**.

    ![Rollen und Benutzer] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Rollen und Benutzer")

3.  Klicken Sie auf **Hinzufügen**.

4.  Führen Sie im Abschnitt **Hinzufügen eines Kontos** die folgenden Schritte aus:

    ![Hinzufügen eines Kontos] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Hinzufügen eines Kontos")

    1.  Geben Sie den **Benutzernamen**, **E-Mail**, **Kennwort** und **Kennwort erneut ein** Werte des Benutzers Azure Active Directory, den Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie die **Rollen**, **Anzeigeberechtigungen** und den **Status**aus.
    3.  Klicken Sie auf **Absenden**.

>[AZURE.NOTE]Alle anderen LogicMonitor Benutzer Konto Creation Tools können oder APIs von bereitgestellten LogicMonitor Bereitstellen von Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Zuweisen von Benutzern zu LogicMonitor führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **LogicMonitor** Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).




