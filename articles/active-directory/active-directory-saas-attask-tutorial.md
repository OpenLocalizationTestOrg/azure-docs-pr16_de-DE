<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in @Task| Microsoft Azure"
    description="Informationen zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und @Task."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Lernprogramm: Azure-Active Directory-Integration in@Task

Das Ziel dieses Lernprogramms besteht darin, die zeigen, wie Sie integrieren @Task mit Azure Active Directory (Azure AD).  
Integration von @Task mit Azure AD bietet Ihnen die folgenden Vorteile: 

- Sie können in Azure AD, der Zugriff auf weist steuern@Task
- Sie können die Benutzer automatisch angemeldet für den Zugriff auf abrufen aktivieren @Task (einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

So konfigurieren Sie Azure AD-Integration in @Task, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine @Task einmalige Anmelden aktiviert Abonnements


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten. 

 
## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wichtigsten Bausteine:

1. Hinzufügen von @Task aus dem Katalog 
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-task-from-the-gallery"></a>Hinzufügen von @Task aus dem Katalog
So konfigurieren Sie die Integration von @Task in Azure AD müssen Sie hinzufügen @Task aus dem Katalog zu Ihrer Liste der verwalteten SaaS apps.

**Hinzufügen @Task aus dem Katalog, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1] 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2] 

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3] 

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4] 

6. Geben Sie im Suchfeld **@Task**.

    ![Applikationen][5] 

7. Wählen Sie im Bereich Ergebnisse **@Task**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Applikationen][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden

Das Ziel der in diesem Abschnitt besteht darin, die erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit @Task basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD für einmaliges Anmelden entwickelt, muss wissen, welche Benutzer die Gegenstück in @Task an einen Benutzer in Azure AD ist. Kurzum, eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in @Task hergestellt werden muss.   
Diese Verknüpfung Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in zuweisen @Task.
 
Konfigurieren und Testen Azure AD-einmaliges Anmelden mit @Task, Sie die folgenden Bausteine ausführen müssen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer @Tasktest Benutzer](#creating-a-halogen-software-test-user) ** – ein Gegenstück von Britta Simon in haben @Taskthat in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD einmaliges Anmelden

In diesem Abschnitt Ziel ist es Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer @Task Anwendung.

**So konfigurieren Sie Azure AD-einmaliges Anmelden mit @Task, führen Sie die folgenden Schritte aus:**

1. Im Azure klassischen-Portal auf die **@Task** Anwendungsintegration Seite, klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf die **Wie möchten Sie Benutzer anmelden @Task ** Seite, wählen Sie **Azure AD einmaliges Anmelden**aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][7] 

3. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren der App-Einstellungen][8] 
 
     ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** die URL verwendet, die für Ihre Benutzer melden Sie sich für den Zugriff auf Ihre @Task Anwendung (z. B.:*https://<Tenant name>.attask-ondemand.com*).

     b. Klicken Sie auf **Weiter**.

4. Klicken Sie auf die **Konfigurieren einmaliges Anmelden bei @Task ** Seite auf **Metadaten herunterladen**, speichern Sie die Metadatendatei lokal auf Ihrem Computer, und klicken Sie dann auf **Weiter**.

    ![Was ist Azure AD-Verbindung herstellen][9] 



1. Melden Sie sich für den Zugriff auf Ihre @Task Firmenwebsite als Administrator.

2. Wechseln Sie zu **einmaliges Anmelden Konfiguration**.


1. Gehen Sie folgendermaßen vor, klicken Sie im Dialogfeld **Einmaliges Anmelden**

    ![Konfigurieren Sie einmaliges Anmelden][23]

    ein. Wählen Sie als **Typ** **SAML 2.0**ein.

    b. Wählen Sie **Service Provider-ID**an.

    c. Klicken Sie im Portal Azure klassischen kopieren Sie der **Remote-Anmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-Portal-URL** ein.

    d. Klicken Sie im Portal Azure klassischen kopieren Sie die **Einzelnen Sign-Out Dienst-URL**, und klicken Sie dann in das Textfeld **Sign-Out URL** einzufügen.

    e. Klicken Sie im Portal Azure klassischen kopieren Sie der **URL für das Kennwort ändern**, und fügen Sie ihn in das Textfeld **URL für das Kennwort ändern** .

    f. Klicken Sie auf **Speichern**.

6. Klicken Sie im Portal Azure klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**. 

    ![Was ist Azure AD-Verbindung herstellen][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  

    ![Was ist Azure AD-Verbindung herstellen][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers aufgerufen Britta Simon im klassischen Azure-Portal.  

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**. 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus: 

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-an-task-test-user"></a>Erstellen einer @Task Testbenutzers

Das Ziel der in diesem Abschnitt besteht darin, erstellen einen Benutzer namens Britta Simon in @Task.


**So erstellen einen Benutzer namens Britta Simon in @Task, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich auf Ihre @Task Firmenwebsite als Administrator.

2. Klicken Sie im Menü oben auf **Personen**.

3. Klicken Sie auf **neue hinzufügen**. 

4. Klicken Sie im Dialogfeld neue Person führen Sie die folgenden Schritte aus:

    ![Erstellen einer @Task Testbenutzers][21] 

    ein. Geben Sie im Textfeld **Vorname** "Britta" ein.

    b. Geben Sie im Textfeld **Nachname** "Simon" ein.

    c. Geben Sie in das Textfeld **E-Mail-Adresse** Britta Simons e-Mail-Adresse in Azure Active Directory.

    d. Klicken Sie auf die **Person hinzufügen**.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

In diesem Abschnitt Ziel ist es für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf @Task.

![Benutzer zuweisen][200] 

**Zuweisen von Britta Simon zu @Task, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal Azure klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applications **@Task**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.  
Beim Klicken auf die @Task Kachel im Bereich Access sollte werden automatisch angemeldet für den Zugriff auf Ihre @Task Anwendung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






