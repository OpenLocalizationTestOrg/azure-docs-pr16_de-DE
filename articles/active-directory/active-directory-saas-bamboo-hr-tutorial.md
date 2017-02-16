<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Bambus HR | Microsoft Azure" 
    description="Erfahren Sie, wie einmaliges Anmelden, automatisierte Bereitstellung und mehr Aktivierung Bambus HR mit Azure Active Directory mithilfe!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Lernprogramm: Azure-Active Directory-Integration in Bambus HR

Das Ziel dieses Lernprogramms ist die Integration von Azure und BambooHR anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie die folgenden Elemente bereits:

-   Ein gültiges Azure-Abonnement
-   Eine BambooHR einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie BambooHR zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite BambooHR (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für BambooHR
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Szenario")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Aktivieren die Anwendungsintegration für BambooHR

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für BambooHR aktiviert.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>Wenn die Anwendungsintegration für BambooHR aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **BambooHR**.

    ![Katalog der Anwendung] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **BambooHR aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu BambooHR mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **BambooHR** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Szenario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Szenario")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der BambooHR auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Konfigurieren einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **BambooHR melden Sie sich auf die URL** Ihrer URL untersuchten Ihre Benutzer bei der Anwendung BambooHR, klicken Sie auf (z. B.: https://company.bamboohr.com), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der app-URL] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Konfigurieren der app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei BambooHR** klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Konfigurieren einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens BambooHR als Administrator.

6.  Führen Sie auf der Homepage der folgenden Schritte aus:

    ![Einmaliges Anmelden] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Einmaliges Anmelden")

    1.  Klicken Sie auf **Apps**.
    2.  Klicken Sie im Menü apps auf der linken Seite auf **Einmaliges Anmelden**.
    3.  Klicken Sie auf **SAML einmaliges Anmelden**.

7.  Führen Sie im Abschnitt **SAML einmaliges Anmelden** die folgenden Schritte aus:

    ![SAML einmaliges Anmelden] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML einmaliges Anmelden")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei BambooHR** kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **SSO Anmelde-URL** .
    2.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    3.  Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **X 509-Zertifikat**
    4.  Klicken Sie auf **Speichern**.

8.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren einmaliges Anmelden] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Konfigurieren einmaliges Anmelden")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei BambooHR zu ermöglichen, müssen er in BambooHR bereitgestellt werden.  
Im Falle von BambooHR ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um eine Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Website **BambooHR** an.

2.  Klicken Sie oben auf der Symbolleiste auf **Einstellungen**.

    ![Festlegen von] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Festlegen von")

3.  Klicken Sie auf **Übersicht**.

4.  Wechseln Sie im linken Navigationsbereich auf **Sicherheit \> Benutzer**.

5.  Geben Sie die Benutzernamen, Kennwort und die e-Mail-Adresse eines gültigen AAD-Kontos ein, die Sie in die verknüpfte Textfelder bereitstellen möchten.

6.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen BambooHR Benutzer Konto Creation Tools können oder APIs bereitgestellt durch BambooHR bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Zuweisen von Benutzern zu BambooHR führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **BambooHR **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
