<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Benefitsolver | Microsoft Azure"
    description="Informationen Sie zur Verwendung von Benefitsolver mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Lernprogramm: Azure-Active Directory-Integration in Benefitsolver

Das Ziel dieses Lernprogramms ist die Integration von Azure und Benefitsolver anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Benefitsolver einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Benefitsolver zugewiesen haben einzelnen melden Sie sich bei der Anwendung, die die [Einführung in die Access-Bereich](active-directory-saas-access-panel-introduction.md)verwenden können.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Benefitsolver
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Szenario")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Aktivieren die Anwendungsintegration für Benefitsolver

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Benefitsolver aktiviert.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Benefitsolver aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Benefitsolver**.

    ![Katalog der Anwendung] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Benefitsolver aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Benefitsolver mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Die Benefitsolver-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das Hinzufügen von benutzerdefinierten Attribut Zuordnungen zu der Konfiguration **token Saml-Attribute** erfordert.  
Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Attribute] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Benefitsolver** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Benefitsolver auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Konfigurieren Sie einmaliges Anmelden")

3.  Führen Sie auf der Seite **Einstellungen für die App konfigurieren** die folgenden Schritte aus:

    ![Konfigurieren der App-Einstellungen] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Konfigurieren der App-Einstellungen")

    1.  Geben Sie in das Textfeld **Melden Sie sich auf URL** **http://azure.benefitsolver.com**ein.
    2.  Geben Sie im Textfeld **URL Antworten** **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**ein.  


    3.  Klicken Sie auf **Weiter**.

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Benefitsolver** zum Herunterladen der Metadaten Ihrer klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Konfigurieren Sie einmaliges Anmelden")

5.  Senden Sie die heruntergeladene Metadatendatei an Ihr Supportteam Benefitsolver ein.

    >[AZURE.NOTE] Führen Sie die eigentliche SSO-Konfiguration muss das Supportteam Benefitsolver.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Konfigurieren Sie einmaliges Anmelden")

7.  Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attribute")

8.  Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    ![Attribute] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribute")

  	|Attributname|Attributwert|
  	|---|---|
  	|ClientID|Sie müssen diesen Wert von Ihrem Supportteam Benefitsolver zu gelangen.|
  	|ClientKey|Sie müssen diesen Wert von Ihrem Supportteam Benefitsolver zu gelangen.|
  	|LogoutURL|Sie müssen diesen Wert von Ihrem Supportteam Benefitsolver zu gelangen.|
  	|"EmployeeID"|Sie müssen diesen Wert von Ihrem Supportteam Benefitsolver zu gelangen.|

    1.  Klicken Sie für jede Datenzeile von in der Tabelle oben auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie in das Textfeld **Name Attribut** das Attribut Bankname für diese Zeile ein.
    3.  Wählen Sie in das Textfeld **Wert Attribut** für diese Zeile angezeigten Attributwert ein.
    4.  Klicken Sie auf **abgeschlossen**.

9.  Klicken Sie auf **Änderungen anzuwenden**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Benefitsolver zu ermöglichen, müssen er in Benefitsolver bereitgestellt werden.  
Bei Benefitsolver werden Mitarbeiterdaten in Ihrer Anwendung ausgefüllt, bis eine Zählung Datei von Ihrem HRIS-System (normalerweise jede Nacht) aus.  

>[AZURE.NOTE] Alle anderen Benefitsolver Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Benefitsolver bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Benutzer Azure AD-erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Zuweisen von Benutzern zu Benefitsolver führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Benefitsolver **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
