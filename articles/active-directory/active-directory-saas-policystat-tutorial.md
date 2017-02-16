<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in PolicyStat | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von PolicyStat mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Lernprogramm: Azure-Active Directory-Integration in PolicyStat
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und PolicyStat anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Einen Mandanten PolicyStat
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie PolicyStat zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite PolicyStat (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für PolicyStat
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Szenario")
##<a name="enabling-the-application-integration-for-policystat"></a>Aktivieren die Anwendungsintegration für PolicyStat
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für PolicyStat aktiviert.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Wenn die Anwendungsintegration für PolicyStat aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **PolicyStat**.

    ![Katalog der Anwendung] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **PolicyStat aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu PolicyStat mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Die PolicyStat-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnungen an der Konfiguration **token Saml-Attribute** hinzufügen erforderlich ist.  
Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Attribute] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **PolicyStat** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der PolicyStat auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Einstellungen für die App konfigurieren** im Textfeld **Melden Sie sich auf URL** die URL, die von den Benutzern die Anmeldung an Ihrer Anwendung URL PolicyStat verwendet (z. B.: *"https://demo-azure.policystat.com"*), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-Einstellungen] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Konfigurieren der App-Einstellungen")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei PolicyStat** klicken Sie auf **Herunterladen von Metadaten**, und speichern Sie die Metadatendatei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens PolicyStat als Administrator.

6.  Klicken Sie auf **die Registerkarte** , und klicken Sie dann im linken Navigationsbereich auf **Einzelne Konfiguration anmelden** .

    ![Menü "Administrator"] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Menü "Administrator"")

7.  Wählen Sie im Abschnitt **Einrichten** **Anmelden Integration einzelnen aktivieren**aus.

    ![Konfiguration für einzelne Zeichen] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Konfiguration für einzelne Zeichen")

8.  Klicken Sie auf **Attribute konfigurieren**, und klicken Sie dann im Abschnitt **Attribute konfigurieren** , gehen Sie folgendermaßen vor:

    ![Konfiguration für einzelne Zeichen] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguration für einzelne Zeichen")

    1.  Geben Sie in das Textfeld **Benutzername Attribut** **Uid**aus.
    2.  Geben Sie im Textfeld **Vorname Attribut** **Firstname**aus.
    3.  Geben Sie in das Textfeld **Last Name-Attribut** **Lastname**aus.
    4.  Geben Sie in das Textfeld **E-Mail-Attribut** **Emailaddress**aus.
    5.  Klicken Sie auf **Änderungen speichern**.

9.  Klicken Sie auf **Ihr IDP Metadaten**aus, und klicken Sie dann im Abschnitt **Your IDP Metadaten** gehen Sie folgendermaßen vor:

    ![Konfiguration für einzelne Zeichen] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguration für einzelne Zeichen")

    1.  Öffnen Sie die heruntergeladene Metadatendatei, kopieren Sie den Inhalt und fügen Sie ihn in das Textfeld **Ihre Identität Anbietermetadaten**
    2.  Klicken Sie auf **Änderungen speichern**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Konfigurieren Sie einmaliges Anmelden")

11. 12. Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Attribute")

13. Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    ![Attribute] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Attribute")

    1.  Klicken Sie auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie in das Textfeld **Name Attribut** **Uid**aus.
    3.  Wählen Sie in das Textfeld **Attributwert** **ExtractMailPrefix()**ein.
    4.  Wählen Sie in der Liste **E-Mail** **User.mail**aus.
    5.  Klicken Sie auf **abgeschlossen**.
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei PolicyStat zu ermöglichen, müssen er in PolicyStat bereitgestellt werden.  
PolicyStat unterstützt nur in der Zeit Benutzer bereitgestellt. Dies bedeutet, Sie brauchen die Benutzer PolicyStat manuell hinzufügen.  
Die Benutzer werden automatisch bei ihrer ersten Anmeldung durch einmaliges Klicken Sie auf hinzugefügt abrufen.

>[AZURE.NOTE]Alle anderen PolicyStat Benutzer Konto Creation Tools können oder APIs bereitgestellt durch PolicyStat bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Zuweisen von Benutzern zu PolicyStat führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **PolicyStat **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).