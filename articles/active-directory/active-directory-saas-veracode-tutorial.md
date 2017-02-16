<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Veracode | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Veracode mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Lernprogramm: Azure-Active Directory-Integration in Veracode
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Veracode anzeigen. Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Veracode einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Veracode zugewiesen haben einzelnen melden Sie sich bei der Anwendung, die die [Einführung in die Access-Bereich](active-directory-saas-access-panel-introduction.md)verwenden können.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Veracode
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Szenario")

##<a name="enabling-the-application-integration-for-veracode"></a>Aktivieren die Anwendungsintegration für Veracode
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Veracode aktiviert.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Veracode aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Veracode**.

    ![Katalog der Anwendung] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Veracode aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden
  
Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Veracode mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Die Veracode-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das Hinzufügen von benutzerdefinierten Attribut Zuordnungen zu Ihrer Konfiguration **token Saml-Attribute** erfordert.  
Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Attribute] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Veracode** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Veracode auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Konfigurieren Sie einmaliges Anmelden")

3.  Klicken Sie auf der Seite **Konfigurieren der App-Einstellungen** auf **Weiter**.

    ![Konfigurieren der App-Einstellungen] (./media/active-directory-saas-veracode-tutorial/IC802909.png "Konfigurieren der App-Einstellungen")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Veracode** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Veracode als Administrator.

6.  Klicken Sie im Menü oben klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Administrator**.

    ![Verwaltung] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Verwaltung")

7.  Klicken Sie auf der Registerkarte **SAML** .

8.  Führen Sie im Abschnitt **Organisation SAML-Einstellungen** die folgenden Schritte aus:

    ![Verwaltung] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Verwaltung")

    1.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Veracode** kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Herausgeber**
    2.  Wenn Ihr heruntergeladene Zertifikat hochladen möchten, klicken Sie auf **Datei auswählen**.
    3.  Wählen Sie die **Self-Registrierung aktivieren**.

9.  Im Abschnitt **Self Registrierung Einstellungen** führen Sie die folgenden Schritte aus, und klicken Sie dann auf **Speichern**:

    ![Verwaltung] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Verwaltung")

    1.  Wählen Sie als **Neuen Benutzer Aktivierung** **Keine Aktivierung erforderlich**.
    2.  Wählen Sie als **Benutzer Daten aktualisiert** **Voreinstellung Veracode Benutzerdaten**aus.
    3.  **Details der SAML-Attribut**wählen Sie Folgendes aus:
        -   **Benutzerrollen**
        -   **Richtlinien-Administrator**
        -   **Bearbeiter**
        -   **Sicherheit Lead**
        -   **Geschäftsleitung**
        -   **Absender**
        -   **Ersteller**
        -   **Alle Scannen Typen**
        -   **Team-Mitgliedschaften**
        -   **Standardteam**

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Konfigurieren Sie einmaliges Anmelden")

11. Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Attribute")

12. Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

    ![Attribute] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Attribute")

  	| Attributname | Attributwert |
  	|:---------------|:----------------|
  	| Vorname      | User.GivenName  |
  	| Nachname       | User.Surname    |
  	| E-Mail          | User.Mail       |

    1.  Klicken Sie für jede Datenzeile von in der Tabelle oben auf **Benutzerattribut hinzufügen**.
    
    2.  Geben Sie in das Textfeld **Name Attribut** das Attribut Bankname für diese Zeile ein.

    3.  Wählen Sie in das Textfeld **Wert Attribut** für diese Zeile angezeigten Attributwert ein.

    4.  Klicken Sie auf **abgeschlossen**.

13. Klicken Sie auf **Änderungen anzuwenden**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt
  
Um Azure AD-Benutzern zur Anmeldung bei Veracode zu ermöglichen, müssen er in Veracode bereitgestellt werden.  
Im Falle von Veracode ist die Bereitstellung einer automatisierten Aufgabe.  
Es ist keine Aktionselement für Sie..
  
Benutzer werden automatisch bei Bedarf beim ersten einzelnen anmelden erstellt.

>[AZURE.NOTE] Alle anderen Veracode Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Veracode bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Zuweisen von Benutzern zu Veracode führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Veracode **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Ja")
  
Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).