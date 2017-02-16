<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in kleinen Verbesserungen | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und kleine Verbesserungen konfigurieren."
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


# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a>Lernprogramm: Azure-Active Directory-Integration in kleinen Verbesserungen

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie kleine Verbesserungen in Azure Active Directory (Azure AD) zu integrieren.  
Integrieren von kleinen Verbesserungen in Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf kleine Verbesserungen zugreifen kann
- Sie können Ihre Benutzer automatisch auf kleine Verbesserungen (einmaliges Anmelden) angemeldete Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um Azure AD-Integration mit kleinen Verbesserungen konfigurieren zu können, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine kleine Verbesserungen einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Kleine Verbesserungen aus dem Katalog hinzufügen
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-small-improvements-from-the-gallery"></a>Kleine Verbesserungen aus dem Katalog hinzufügen
Zum Konfigurieren der Integration von kleinen Verbesserungen in Azure AD müssen Sie kleine Verbesserungen zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um kleine Verbesserungen aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **Wurde geringfügig verbessert**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_01.png)

7. Wählen Sie im Ergebnisbereich **Small Verbesserungen**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt wird veranschaulicht, wie konfigurieren und Testen Azure AD-einmaliges Anmelden mit kleinen Verbesserungen basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD Gegenstück Benutzer in kleinen Verbesserungen an einen Benutzer in Azure AD kennen. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in kleinen Verbesserungen eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in kleinen Verbesserungen zuweisen.

Zum Konfigurieren und Azure AD-einmaliges Anmelden mit kleinen Verbesserungen testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen eine kleine Verbesserungen Benutzer testen](#creating-a-small-improvements-test-user)** : ein Gegenstück von Britta Simon in kleinen Verbesserungen haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung kleine Verbesserungen an.



**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit kleinen Verbesserungen konfigurieren:**

1. Im Azure klassischen-Portal auf der Seite **Small verbesserte** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer kleine Verbesserungen auf bei** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_03.png) 

3. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_04.png) 

    > [AZURE.NOTE] Die kleinen Verbesserungen am Supportteam Kontakt [Support@small-improvements.com](mailto:support@small-improvements.com) einen Domänennamen für Ihr Konto konfigurieren. Dies ist für einmaliges Anmelden entwickelt erforderlich.


    ein. Geben Sie in das Textfeld **Anmelden-URL** die URL, die von den Benutzern die Anmeldung an Ihrer Anwendung kleine Verbesserungen verwendet.  
    b. Klicken Sie auf **Weiter**.


4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Small Verbesserungen** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


1. In einem anderen Browserfenster melden Sie sich auf der Website Ihres Unternehmens kleine Verbesserungen als Administrator.

1. Klicken Sie aus dem Hauptfenster Dashboard-Seite auf die Schaltfläche **Verwaltung** auf der linken Seite.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

1. Klicken Sie auf die Schaltfläche **SAML SSO-** **Integration** Abschnitts.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_07.png) 


1. Führen Sie auf der Seite SSO einrichten die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_08.png) 

    ein. Im Azure klassischen-Portal auf der Seite **Konfigurieren einmaliges Anmelden bei Small Verbesserungen** kopieren Sie des Werts **SAML SSO-URL** , und fügen Sie ihn in das Textfeld **HTTP-Endpunkt** .

    b. Öffnen das heruntergeladene Zertifikat in Editor, kopieren Sie den Inhalt aus, und fügen Sie ihn in den **X509 Zertifikat** Textfeld.

    c. Wenn Sie SSO und Login Formular Authentifizierungsoption für Benutzer verfügbar haben, und klicken Sie dann aktivieren Sie die Option **Zugriff über Benutzername und Kennwort auch aktivieren** möchten. 
  
    d. Geben Sie den entsprechenden Wert um SSO Anmeldeschaltfläche in der **SAML auffordern** Textfeld einen Namen ein. 

    e. Klicken Sie auf **Speichern**.


6. Im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.  

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_03.png) 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-small-improvements-test-user"></a>Erstellen eines kleinen Verbesserungen Testbenutzers

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in kleinen Verbesserungen bezeichnet.
Sie haben bereits in [Konfigurieren von Azure AD einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)aktiviert werden.


**Führen Sie die folgenden Schritte aus, um einen Benutzer namens Britta Simon in kleinen Verbesserungen zu erstellen:**

1. Melden Sie sich für den Zugriff auf Ihre kleine Verbesserungen Firmenwebsite als Administrator.

1. Öffnen Sie von der Homepage das Menü auf der linken Seite, klicken Sie auf **Verwaltung**.

1. Klicken Sie auf die Schaltfläche **Benutzer-Verzeichnis** von Abschnitt Benutzer verwalten. 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

1. Klicken Sie auf **Teammitglied hinzufügen**.
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

1. Geben Sie den **Vornamen**, **Nachnamen** und **E-Mail-Adresse** eines gültigen Benutzers, die Sie in die verknüpfte Textfelder bereitstellen möchten. 
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_12.png) 

1. Sie können auch auswählen, geben die persönliche Nachricht in das Feld **e-Mail-Benachrichtigung zu senden** . Wenn Sie nicht möchten, deaktivieren Sie dieses Kontrollkästchen, und Sie dann die Benachrichtigung senden.

1. Klicken Sie auf **Benutzer erstellen**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf kleine Verbesserungen erteilen.

    ![Assign User][200] 

**Um kleine Verbesserungen Britta Simon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applikationen **Kleine Verbesserungen**aus.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_50.png) 


1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel kleine Verbesserungen in der Systemsteuerung Access klicken, Sie sollten automatisch an Ihrer Anwendung kleine Verbesserungen angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_205.png
