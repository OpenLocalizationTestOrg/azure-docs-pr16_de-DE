<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in KnowBe4 | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und KnowBe4 konfigurieren."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-knowbe4"></a>Lernprogramm: Azure-Active Directory-Integration in KnowBe4

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie KnowBe4 mit Azure Active Directory (Azure AD) integrieren.  
Integrieren von KnowBe4 in Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, die auf KnowBe4 zugreifen
- Sie können Ihre Benutzer automatisch auf KnowBe4 angemeldete abrufen aktivieren (einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten an einem zentralen Ort – das Azure Active Directory-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit KnowBe4, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine KnowBe4 einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von KnowBe4 aus dem Katalog
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-knowbe4-from-the-gallery"></a>Hinzufügen von KnowBe4 aus dem Katalog
Um die Integration der KnowBe4 in Azure AD zu konfigurieren, müssen Sie KnowBe4 zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um KnowBe4 aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **KnowBe4**ein.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_01.png)

7. Wählen Sie im Ergebnisbereich **KnowBe4 aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit KnowBe4 basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück KnowBe4 an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in KnowBe4 eingerichtet werden.  
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in KnowBe4 zuweisen.

Zum Konfigurieren und Azure AD-einmaliges Anmelden mit KnowBe4 testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer KnowBe4 Benutzer testen](#creating-a-KnowBe4-test-user)** : ein Gegenstück von Britta Simon in KnowBe4 haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung KnowBe4.



**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit KnowBe4 konfigurieren:**

1. Im Azure klassischen-Portal auf der Seite **KnowBe4** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der KnowBe4 auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_03.png) 

3. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_04.png) 


    ein. Geben Sie in das Textfeld melden Sie sich auf URL die URL Ihrer Benutzer melden Sie sich für den Zugriff auf Ihre KnowBe4-Anwendung unter Verwendung des folgenden Musters untersuchten: **"https://\<Firmennamen\>.knowbe4.com/auth/saml/aad168.ccsctp.net)"**.


4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei KnowBe4** führen Sie die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Um für die Anwendung konfigurierten SSO zu erhalten, wenden Sie sich an Ihr [Supportteam für KnowBe4](mailto:support@knowbe4.com ). Fügen Sie die heruntergeladene Zertifikatdatei zu Ihrer e-Mails, und teilen Sie die Urls Metadaten (Entität-ID, SSO anmelden URL, und melden Sie sich ab URL) mit KnowBe4 Team SSO auf ihrer Seite einrichten.


6. Im Portal Azure klassischen wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.  

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_03.png) 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-knowbe4-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-knowbe4-test-user"></a>Erstellen eines Testbenutzers KnowBe4

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in KnowBe4 bezeichnet. KnowBe4 unterstützt in-Time-Bereitstellung, welche ist standardmäßig aktiviert.

Keine für Sie in diesem Abschnitt Aktionselement ist vorhanden. Bei dem Versuch, KnowBe4 zugreifen, wenn er noch nicht vorhanden ist, wird ein neuer Benutzer erstellt werden. 

> [AZURE.NOTE] Wenn Sie einen Benutzer manuell erstellen müssen, müssen Sie wenden Sie sich an das Supportteam KnowBe4.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf KnowBe4 erteilen.

    ![Assign User][200] 

**Um Britta Simon KnowBe4 zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **KnowBe4**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-knowbe4-tutorial/tutorial_knowbe4_50.png) 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Wenn Sie die Kachel KnowBe4 im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung KnowBe4 angemeldete abrufen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-knowbe4-tutorial/tutorial_general_205.png
