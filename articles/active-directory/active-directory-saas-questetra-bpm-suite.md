<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Questetra BPM-Suite | Microsoft Aure"
    description="Informationen Sie zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und Questetra BPM-Suite."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Lernprogramm: Azure-Active Directory-Integration in Questetra BPM-Suite

Ziel dieses Lernprogramms ist zu veranschaulichen Questetra BPM-Suite mit Azure Active Directory (Azure AD) integriert werden soll.  
Integrieren von Azure AD Questetra BPM-Suite bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD steuern, wer auf Questetra BPM-Suite zugreifen kann 
- Sie können Ihre Benutzer automatisch auf Questetra BPM-Suite (einmaliges Anmelden) angemeldete Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit Questetra BPM-Suite, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine [Questetra BPM-Suite](https://senbon-imadegawa-988.questetra.net/) einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wichtigsten Bausteine:

1. Hinzufügen von Questetra BPM-Suite aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Hinzufügen von Questetra BPM-Suite aus dem Katalog
Zum Konfigurieren der Integration von Questetra BPM-Suite in Azure AD müssen Sie Ihre Liste von verwalteten SaaS apps Questetra BPM-Suite aus dem Katalog hinzufügen.

**Um Questetra BPM-Suite aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Questetra BPM-Suite**aus.

    ![Applikationen][5]

7. Im Bereich Ergebnisse wählen Sie **Questetra BPM-Suite aus**und dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit Questetra BPM-Suite basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück Questetra BPM-Suite an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Questetra BPM-Suite eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in Questetra BPM-Suite zuweisen.
 
Zum Konfigurieren und Azure AD-einmaliges Anmelden mit Questetra BPM-Suite testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Questetra BPM-Suite Benutzer testen](#creating-a-questetra-bpm-suite-test-user)** : ein Gegenstück von Britta Simon in Questetra BPM-Suite haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung Questetra BPM-Suite auf.

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit Questetra BPM-Suite konfigurieren:**

1. Im Azure klassischen-Portal auf der Seite **Questetra BPM-Suite** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][8]

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Questetra BPM-Suite auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][9]


3. In einem anderen Webbrowserfenster melden Sie sich bei der Website Ihres Unternehmens **Questetra BPM-Suite** als Administrator.

4. Klicken Sie im Menü oben auf **Systemeinstellungen**. 

    ![Azure AD einmaliges Anmelden][10]

5. Klicken Sie auf **SSO (SAML)**, um die Seite **SingleSignOnSAML** zu öffnen. 

    ![Azure AD einmaliges Anmelden][11]


6. Führen Sie im Azure klassischen-Portal auf der Seite **Einstellungen für die App konfigurieren** Dialogfeld die folgenden Schritte aus: 

    ![Konfigurieren der App-Einstellungen][13]
 
    ein. Aktivieren Sie **Questetra BPM-Suite** Firmenwebsite, im Abschnitt SP Informationen kopieren Sie die **ACS-URL**, und fügen Sie ihn in das Textfeld **Melden Sie sich auf URL** .

    b. Aktivieren Sie **Questetra BPM-Suite** Firmenwebsite, im Abschnitt SP Informationen kopieren Sie die **Element-ID**, und fügen Sie ihn in das Textfeld **URL des Herausgebers** .

    c. Aktivieren Sie **Questetra BPM-Suite** Firmenwebsite, im Abschnitt SP Informationen kopieren Sie die **ACS-URL**, und fügen Sie ihn in das Textfeld **URL Antworten** .

    d. Klicken Sie auf **Weiter**.

 
7. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Questetra BPM-Suite** klicken Sie auf **Zertifikat herunterladen**, und klicken Sie dann speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden][14]


8. Führen Sie auf Sie **Questetra BPM-Suite** Firmenwebsite die folgenden Schritte aus: 

    ![Konfigurieren Sie einmaliges Anmelden][15]

    ein. Kontrollkästchen Sie **einmaliges Anmelden aktivieren**das.
     
    b. Klicken Sie im Portal Azure klassischen kopieren Sie den Wert des **Herausgebers URL** , und fügen Sie ihn in das Textfeld **Einheiten ID** .

    c. Klicken Sie im Portal Azure klassischen kopieren Sie den Wert für die **Einzelnen anmelden Dienst-URL** , und fügen Sie ihn in das Textfeld **Anmeldung Seiten-URL** .

    d. Klicken Sie im Portal Azure klassischen kopieren Sie den Wert für die **Einzelnen Sign-Out Dienst-URL** , und fügen Sie ihn in das Textfeld **Abmeldung Seiten-URL** .

    e. Geben Sie im Textfeld **Formatieren NameID** **Urn: Oasis: Namen: Tc: SAML:1.1:nameid-Format: EmailAddress**.


    f. Erstellen Sie eine codierte Base-64-Datei aus der heruntergeladenen Zertifikat ein. 

    >[AZURE.TIP] Weitere Informationen hierzu finden Sie unter [Konvertieren ein binäres Zertifikat in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

    g. Öffnen Sie Ihre Base-64-codierte Zertifikat in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Überprüfung Zertifikat** . 

    h. Klicken Sie auf **Speichern**.


9. Klicken Sie im Portal Azure klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**. 

    ![Was ist Azure AD-Verbindung herstellen][17]


10. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Was ist Azure AD-Verbindung herstellen][18]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen Sie Testbenutzer Azure AD-][100] 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen Sie Testbenutzer Azure AD-][101] 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Erstellen Sie Testbenutzer Azure AD-][102] 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen Sie Testbenutzer Azure AD-][103]
 
    ein. Wählen Sie als **Typ des Benutzers** **neuen Benutzer in Ihrer Organisation**ein.
  
    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf Weiter.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Erstellen Sie Testbenutzer Azure AD-][104] 
  
    ein. Geben Sie im Textfeld **Vorname** **Britta**aus. 
 
    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen Sie Testbenutzer Azure AD-][105]  

8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:

    ![Erstellen Sie Testbenutzer Azure AD-][106]   
  
    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.
  
    b. Klicken Sie auf **abgeschlossen**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Erstellen eines Testbenutzers Questetra BPM-Suite

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in Questetra BPM-Suite bezeichnet.

**Führen Sie die folgenden Schritte aus, um einen Benutzer namens Britta Simon in Questetra BPM-Suite zu erstellen:**

1.  Melden Sie sich für den Zugriff auf Ihre Questetra BPM-Suite Firmenwebsite als Administrator.
2.  Wechseln Sie zu **Systemeinstellungen > Benutzerliste > neuer Benutzer**. 
3.  Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld neuen Benutzer: 

    ![Erstellen Sie Testbenutzer][300] 

    ein. Geben Sie in das Textfeld **Name** Brittas Benutzername in Azure AD ein.

    b. Geben Sie in das Textfeld **E-Mail** Brittas Benutzernamens in Azure AD ein.

    c. Geben Sie im Textfeld **Kennwort** ein Kennwort ein.

4.  Klicken Sie auf **neuen Benutzer hinzufügen**.



### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf Questetra BPM Suite erteilen.

![Was ist Azure AD-Verbindung herstellen][200]

**Um Britta Simon Questetra BPM-Suite zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Was ist Azure AD-Verbindung herstellen][201]

2. Wählen Sie in der Liste Applications **Questetra BPM-Suite**aus.

    ![Was ist Azure AD-Verbindung herstellen][205]

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Was ist Azure AD-Verbindung herstellen][202]

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

    ![Was ist Azure AD-Verbindung herstellen][203]

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Was ist Azure AD-Verbindung herstellen][204]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel Questetra BPM-Suite im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung Questetra BPM-Suite angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 