<properties 
    pageTitle="Lernprogramm: Azure-Active Directory-Integration-Integration in Druva | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Druva mit Azure Active Directory einmaliges Anmelden, automatisierte Bereitstellung und mehr aktivieren!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Lernprogramm: Azure-Active Directory-Integration-Integration in Druva

Das Ziel dieses Lernprogramms ist die Integration von Azure und Druva anzeigen.  
Das in diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass bereits die folgenden Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Druva einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms, werden die Azure AD-Benutzer, die Sie Druva zugewiesen haben können einzelne melden Sie sich bei der Anwendung bei Ihrer Firmenwebsite Druva (Service Provider initiiert signieren auf), oder verwenden die [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Druva
2.  Konfigurieren einmaliges Anmelden
3.  Konfigurieren der Benutzer bereitgestellt
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-druva-tutorial/IC795084.png "Szenario")
##<a name="enabling-the-application-integration-for-druva"></a>Aktivieren die Anwendungsintegration für Druva

Das Ziel der in diesem Abschnitt ist zu gliedernden wie die Anwendungsintegration für Druva aktiviert.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Wenn die Anwendungsintegration für Druva aktivieren möchten, führen Sie die folgenden Schritte aus:

1.  Klicken Sie im Azure klassischen-Portal auf der linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3.  Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-druva-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Anwendung hinzufügen] (./media/active-directory-saas-druva-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung von gallerry] (./media/active-directory-saas-druva-tutorial/IC749322.png "Hinzufügen einer Anwendung von gallerry")

6.  Geben Sie im **Suchfeld** **Druva**.

    ![Katalog der Anwendung] (./media/active-directory-saas-druva-tutorial/IC795085.png "Katalog der Anwendung")

7.  Wählen Sie im Ergebnisbereich **Druva aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Konfigurieren einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zu gliedernden wie Benutzer authentifizieren zu Druva mit ihrem Konto in Azure Active Directory Federation je nach verwendetem SAML-Protokoll verwenden aktiviert.  
Als Teil dieses Verfahrens müssen Sie eine Datei Base-64-codierte Zertifikat zu erstellen.  
Wenn Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

Die Druva-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnungen an der Konfiguration **token Saml-Attribute** hinzufügen erforderlich ist.  
Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Token SAML-Attribute] (./media/active-directory-saas-druva-tutorial/IC795087.png "Token SAML-Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Führen Sie die folgenden Schritte aus, einmaliges Anmelden um zu konfigurieren:

1.  Im Azure klassischen-Portal auf der Seite **Druva** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** um das Dialogfeld **Konfigurieren Single Sign On** öffnen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-druva-tutorial/IC795027.png "Konfigurieren Sie einmaliges Anmelden")

2.  Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Druva auf** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-druva-tutorial/IC795088.png "Konfigurieren Sie einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren der App-URL** in das Textfeld **Druva melden Sie sich auf URL** die URL, die von den Benutzern verwendet, um melden Sie sich an Ihrer Anwendung Druva auf (z. B.: "*https://cloud.druva.com/home/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren der App-URL] (./media/active-directory-saas-druva-tutorial/IC795089.png "Konfigurieren der App-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Druva** Wenn Ihr Zertifikat herunterladen möchten, klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-druva-tutorial/IC795090.png "Konfigurieren Sie einmaliges Anmelden")

5.  In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens Druva als Administrator.

6.  Wechseln Sie zu **verwalten \> Einstellungen**.

    ![Einstellungen] (./media/active-directory-saas-druva-tutorial/IC795091.png "Einstellungen")

7.  Klicken Sie im Dialogfeld Einstellungen für einzelne Zeichen führen Sie die folgenden Schritte aus:

    ![Einstellungen für einzelne Druckseite anmelden] (./media/active-directory-saas-druva-tutorial/IC795092.png "Einstellungen für einzelne Druckseite anmelden")

    1.  Kopieren Sie den Wert **Remote Anmelde-URL** , und fügen Sie ihn in das Textfeld **-ID-Anbieter Anmelde-URL** , im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Druva** .
    2.  Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Druva** kopieren Sie des Werts **Remote Abmeldung-URL** , und klicken Sie dann in das Textfeld **-ID-Anbieter Abmelden URL** einzufügen.
    3.  Erstellen Sie eine **Base-64-codierte** Datei aus Ihrem heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [So konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

    4.  Öffnen des Base-64-codierten Zertifikats in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **-ID-Anbieter Zertifikat**
    5.  Klicken Sie zum Öffnen **der Seite** auf **Speichern**.

8.  Klicken Sie auf **der Einstellungsseite** auf **Token SSO generieren**.

    ![Einstellungen] (./media/active-directory-saas-druva-tutorial/IC795093.png "Einstellungen")

9.  Klicken Sie im Dialogfeld **Einzelnen anmelden Authentication Token** führen Sie die folgenden Schritte aus:

    ![Token für einmaliges Anmelden] (./media/active-directory-saas-druva-tutorial/IC795094.png "Token für einmaliges Anmelden")

    1.  Klicken Sie auf **Kopieren**.
    2.  Klicken Sie auf **Schließen**.

10. Klicken Sie im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **abgeschlossen** , um das Dialogfeld **Konfigurieren Single Sign On** schließen.

    ![Konfigurieren Sie einmaliges Anmelden] (./media/active-directory-saas-druva-tutorial/IC795095.png "Konfigurieren Sie einmaliges Anmelden")

11. Klicken Sie im Menü oben auf **Attributen** , um das Dialogfeld **Token SAML-Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-druva-tutorial/IC795096.png "Attribute")

12. Um die Zuordnungen erforderliches Attribut hinzufügen möchten, führen Sie die folgenden Schritte aus:

  	|Attributname|Attributwert|
  	|---|---|
  	|Datenträgerkonfiguration\_autorisierende\_token|<*Wert der Zwischenablage*>|

    1.  Klicken Sie für jede Datenzeile von in der Tabelle oben auf **Benutzerattribut hinzufügen**.
    2.  Geben Sie in das Textfeld **Name Attribut** das Attribut Bankname für diese Zeile ein.
    3.  Geben Sie in das Textfeld **Wert Attribut** für diese Zeile angezeigten Attributwert ein.
    4.  Klicken Sie auf **abgeschlossen**.

13. Klicken Sie auf **Änderungen anzuwenden**.
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitgestellt

Um Azure AD-Benutzern zur Anmeldung bei Druva zu ermöglichen, müssen er in Druva bereitgestellt werden.  
Im Falle von Druva ist die Bereitstellung eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Führen Sie zum Konfigurieren der Benutzer bereitgestellt, die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite **Druva** an.

2.  Wechseln Sie zu **verwalten \> Benutzer**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-druva-tutorial/IC795097.png "Verwalten von Benutzern")

3.  Klicken Sie auf **neu erstellen**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-druva-tutorial/IC795098.png "Verwalten von Benutzern")

4.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld neuen Benutzer erstellen:

    ![Erstellen neuer Benutzer] (./media/active-directory-saas-druva-tutorial/IC795099.png "Erstellen neuer Benutzer")

    1.  Geben Sie die e-Mail-Adresse und den Namen des ein gültiges Azure Active Directory-Benutzerkonto, die, das Sie in die verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer erstellen**.

>[AZURE.NOTE] Alle anderen Druva Benutzer Konto Creation Tools können oder APIs bereitgestellt durch Druva bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Klicken Sie zum Testen der Konfigurations müssen Sie die Azure AD-Benutzer erteilen, dass Sie mit der Anwendung darauf zugreifen sollen, durch das Zuordnen zulassen möchten.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Zuweisen von Benutzern zu Druva führen Sie die folgenden Schritte aus:

1.  Erstellen Sie ein Testkonto im Portal Azure klassischen.

2.  Klicken Sie auf der Seite **Druva **Anwendung Integration auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-druva-tutorial/IC795100.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer aus, klicken Sie auf **zuweisen**, und klicken Sie dann auf **Ja,** um Ihre Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-druva-tutorial/IC767830.png "Ja")

Wenn Sie Ihre Einstellungen für einzelnen Zeichen testen möchten, öffnen Sie die Access-Systemsteuerung. Weitere Details über den Bereich Access finden Sie unter [Einführung in die Access-Systemsteuerung](active-directory-saas-access-panel-introduction.md).
