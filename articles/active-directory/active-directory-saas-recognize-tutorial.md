<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in erkennen | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der einmaligen Anmeldens zwischen Azure Active Directory und erkennen."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Lernprogramm: Azure-Active Directory-Integration in erkennen

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie erkennen mit Azure Active Directory (Azure AD) integrieren.

Integrieren von erkennen in Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf erkennen zugreifen kann
- Sie können den Benutzern automatisch in erkennen (einmaliges Anmelden) mit ihren Konten Azure AD-angemeldete Abrufen ermöglichen.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit erkennen, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Erkennen einmalige Anmeldung aktiviert Abonnements


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Erkennen aus dem Katalog hinzufügen
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-recognize-from-the-gallery"></a>Erkennen aus dem Katalog hinzufügen
Zum Konfigurieren der Integration von erkennen in Azure AD müssen Sie erkennen zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Wenn erkennen aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **erkennen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. Wählen Sie im Ergebnisfeld **erkennen**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzuzufügen.

    ![Markieren die app im Katalog](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit erkennen basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück erkennen an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in erkennen eingerichtet werden.

Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in erkennen zuweisen.

Zum Konfigurieren und Testen Azure AD-einmaliges Anmelden mit erkennen können, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer erkennen Benutzer testen](#creating-a-recognize-test-user)** : ein Gegenstück von Britta Simon in erkennen können, die in der Azure AD-Darstellung Ihrer verknüpft ist.
4. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD-einmaliges Anmelden

In diesem Abschnitt Azure AD-einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren in der Anwendung erkennen einmaliges Anmelden.

**So konfigurieren Sie Azure AD-einmaliges Anmelden mit erkennen die folgenden Schritte aus:**

1. Im Portal klassischen auf der Seite **erkennen** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .
     
    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer erkennen auf bei** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.
    
    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. Führen Sie auf der Seite **Einstellungen für die App konfigurieren** Dialogfeld die folgenden Schritte aus, und klicken Sie auf **Weiter**:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** eine URL, die mit dem folgenden Muster: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. Geben Sie in das Textfeld **Bezeichner** eine URL, die mit dem folgenden Muster: `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Wenn Sie über diese URLs nicht kennen, geben Sie Stichprobe URLs mit Beispiel Muster. Um diese Werte zu erhalten, können Sie weitere Details finden Sie in Schritt 9 oder wenden Sie sich an erkennen Supportteam über <mailto:support@recognizeapp.com>.

4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei erkennen** klicken Sie auf **Zertifikat herunterladen** , und speichern Sie die Datei auf Ihrem Computer:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. In einem anderen Webbrowserfenster melden Sie sich für den Zugriff auf Ihre erkennen Mandanten als Administrator.

6. Klicken Sie auf der oberen rechten Ecke klicken Sie **im Menü**aus. Wechseln Sie zu der **Unternehmensadministrator**.

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. Klicken Sie im linken Navigationsbereich auf **Einstellungen**.

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. Führen Sie die folgenden Schritte auf **SSO-Einstellungen** im Abschnitt ein.

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    ein. Wählen Sie als **SSO aktivieren** **auf**ein.

    b. In der **IDP Einheiten ID** setzen Textfeld den Wert des **Herausgebers URL** aus Azure AD-Anwendung Kontokonfigurations-Assistenten aus.

    c. In der **Ziel-Url Sso** setzen Textfeld den Wert der **Einzelnen anmelden Service URL** aus Azure AD-Anwendung Kontokonfigurations-Assistenten aus.

    d. In der **Ziel-Url Slo** setzen Textfeld den Wert der **Einzelnen Sign-Out Service URL** aus Azure AD-Anwendung Kontokonfigurations-Assistenten aus.

    e. Öffnen Sie der heruntergeladenen Zertifikatsdatei in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **Zertifikat** . 

    f. Klicken Sie auf die Schaltfläche **Einstellungen speichern** . 

9. Kopieren Sie neben im Abschnitt **SSO-Einstellungen** die URL unter **Service Provider-Metadaten-Url**ein.
    
    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Öffnen Sie die **Metadaten-URL-Link** unter einem leeren Browser zum Herunterladen des Dokuments Metadaten. Verwenden Sie dann den EntityDescriptor angegebene Wert erkennen für **Bezeichner** klicken Sie im Dialogfeld **Konfigurieren der App-Einstellungen** .

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. Im Portal klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

12. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers im klassischen Portal Britta Simon bezeichnet.

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. Geben Sie im Textfeld **Nachname** **Simon**aus.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-recognize-test-user"></a>Erstellen eines Testbenutzers erkennen

Um Azure AD-Benutzer zur Anmeldung bei erkennen können, müssen er in erkennen bereitgestellt werden. Wenn erkennen ist die Bereitstellung eine manuelle Aufgabe.

Diese app nicht unterstützt SCIM provisioning jedoch weist eine alternative Benutzer synchronisieren, die Vorschriften, die den Benutzer. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Um ein Benutzerkonto bereitstellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich als Administrator in Ihrer Firmenwebsite erkennen.

2.  Klicken Sie auf der oberen rechten Ecke klicken Sie **im Menü**aus. Wechseln Sie zu der **Unternehmensadministrator**.

3.  Klicken Sie im linken Navigationsbereich auf **Einstellungen**.

4.  Führen Sie die folgenden Schritte auf **Benutzer synchronisieren** Abschnitt ein.
    
    ![Neuer Benutzer] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Neuer Benutzer")

    ein. Wählen Sie als **Synchronisieren aktiviert**ist **auf**ein.

    b. Als **Synchronisieren-Anbieter auswählen**, wählen Sie aus **Microsoft / Office 365**.

    c. Klicken Sie auf **Benutzer synchronisieren ausführen**

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie erteilen keinen Zugriff zu erkennen.
    
![Benutzer zuweisen][200]

**Zuweisen von Britta Simon zu erkennen führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Liste Applikationen **erkennen**.
    
    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

5. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.
    
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.
 
Wenn Sie die Kachel erkennen Sie im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung erkennen angemeldete abrufen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
