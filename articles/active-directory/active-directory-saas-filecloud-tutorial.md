<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in FileCloud | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und FileCloud konfigurieren."
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
    ms.date="09/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a>Lernprogramm: Azure-Active Directory-Integration in FileCloud

Ziel dieses Lernprogramms ist es zu zeigen, wie Sie FileCloud mit Azure Active Directory (Azure AD) integrieren.

Integration von FileCloud mit Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, die auf FileCloud zugreifen
- Sie können Ihre Benutzer automatisch auf FileCloud (einmaliges Anmelden) mit ihren Konten Azure AD-angemeldete abrufen aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit FileCloud, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Eine FileCloud einmaligen Anmeldung aktiviert Abonnement


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
Ziel dieses Lernprogramms ist, sodass Sie in einer Umgebung für Azure AD-einmaligen Anmeldens testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von FileCloud aus dem Katalog
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-filecloud-from-the-gallery"></a>Hinzufügen von FileCloud aus dem Katalog
Um die Integration der FileCloud in Azure AD zu konfigurieren, müssen Sie FileCloud zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Um FileCloud aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **FileCloud**ein.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_01.png)

7. Im Ergebnisfeld **FileCloud**wählen Sie aus, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Markieren die app im Katalog](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
Das Ziel der in diesem Abschnitt ist erläutert, wie Sie konfigurieren und Testen der Azure AD-einmaliges Anmelden mit FileCloud basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD wissen, was der Benutzer Gegenstück FileCloud an einen Benutzer in Azure AD ist. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in FileCloud eingerichtet werden.

Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in FileCloud zuweisen.

Zum Konfigurieren und Azure AD-einmaliges Anmelden mit FileCloud testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer FileCloud Benutzer testen](#creating-a-filecloud-test-user)** : ein Gegenstück von Britta Simon in FileCloud haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
4. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD-einmaliges Anmelden

In diesem Abschnitt Azure AD-einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren in Ihrer Anwendung FileCloud einmaliges Anmelden.

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit FileCloud konfigurieren:**

1. Im Portal klassischen auf der Seite **FileCloud** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .
     
    ![Konfigurieren Sie einmaliges Anmelden][6] 

2. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der FileCloud auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.
    
    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_03.png)

3. Führen Sie auf der Seite **Einstellungen für die App konfigurieren** Dialogfeld die folgenden Schritte aus, und klicken Sie auf **Weiter**:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_04.png)

    ein. Geben Sie in das Textfeld **Melden Sie sich auf URL** eine URL, die mit dem folgenden Muster: `https://<subdomain>.filecloudhosted.com`.

    b. Geben Sie in das Textfeld **Bezeichner** : `https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`.

    c. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Bitte beachten Sie, dass Sie diese Werte mit der tatsächlichen melden Sie sich auf URL und der Bezeichner aktualisiert haben. Um diese Werte zu erhalten, wenden Sie sich an FileCloud Supportteam über <mailto:support@codelathe.com>.

4. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei FileCloud** führen Sie die folgenden Schritte aus, und klicken Sie auf **Weiter**:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_05.png)

    ein. Klicken Sie auf **Herunterladen von Metadaten**aus, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

5. In einem anderen Webbrowserfenster melden Sie sich für den Zugriff auf Ihre FileCloud Mandanten als Administrator.

6. Klicken Sie im linken Navigationsbereich auf **Einstellungen**. 

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

7. Klicken Sie auf die Registerkarte Einstellungen im Abschnitt **SSO** . 

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

8. Wählen Sie **SAML** Dateityp **SSO Standard** Bedienfeld der **Einstellungen für einzelne anmelden (SSO)** .

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

9. In der **IdP Endpunkt URL** setzen Textfeld den Wert der **Element-ID** aus Azure AD-Anwendung Kontokonfigurations-Assistenten aus, klicken Sie auf Systemsteuerung **SAML-Einstellungen** .

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

10. Öffnen der heruntergeladenen Metadatendatei in Editor, kopieren Sie den Inhalt der es in der Zwischenablage, und fügen Sie ihn in das Textfeld **IdP Metadaten** auf Systemsteuerung **SAML-Einstellungen** .

    ![Konfigurieren der einzelnen anmelden auf App-Seite](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

11. Klicken Sie auf die Schaltfläche **Speichern** .

12. Im Portal klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

13. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
Das Ziel der in diesem Abschnitt besteht im Erstellen eines Testbenutzers im klassischen Portal Britta Simon bezeichnet.

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. Geben Sie im Textfeld **Nachname** **Simon**aus.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_07.png)

8. Klicken Sie auf der Seite **erste temporäres Kennwort** führen Sie die folgenden Schritte aus:
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-filecloud-test-user"></a>Erstellen eines Testbenutzers FileCloud

Das Ziel der in diesem Abschnitt ist zum Erstellen eines Benutzers Britta Simon in FileCloud bezeichnet. FileCloud unterstützt in-Time-Bereitstellung, welche ist standardmäßig aktiviert.

Keine für Sie in diesem Abschnitt Aktionselement ist vorhanden. Bei dem Versuch, FileCloud zugreifen, wenn er noch nicht vorhanden ist, wird ein neuer Benutzer erstellt werden. 

> [AZURE.NOTE] Wenn Sie einen Benutzer manuell erstellen müssen, müssen Sie wenden Sie sich an das Supportteam FileCloud.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

Das Ziel der in diesem Abschnitt ist für die Aktivierung der Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf FileCloud erteilen.
    
![Benutzer zuweisen][200]

**Um Britta Simon FileCloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Liste Applications **FileCloud**.
    
    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_50.png)

3. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**aus.

5. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.
    
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.
 
Wenn Sie die Kachel FileCloud im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung FileCloud angemeldete abrufen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_205.png
