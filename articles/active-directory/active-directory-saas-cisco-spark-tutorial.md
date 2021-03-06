<properties
    pageTitle="Lernprogramm: Azure-Active Directory-Integration in Cisco Spark | Microsoft Azure"
    description="Informationen Sie zum einmaligen Anmeldens zwischen Azure Active Directory und Cisco Spark konfigurieren."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Lernprogramm: Azure-Active Directory-Integration in Cisco Spark

In diesem Lernprogramm erfahren Sie, wie Cisco Spark mit Azure Active Directory (Azure AD) integriert werden soll.

Integrieren von Cisco Spark in Azure AD bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Cisco Spark zugreifen kann
- Sie können Ihre Benutzer automatisch auf Cisco Spark (einmaliges Anmelden) angemeldete Abrufen mit ihren Azure AD-Konten aktivieren.
- Sie können Ihre Konten an einem zentralen Ort – im klassischen Azure-Portal verwalten.

Wenn Sie weitere Details zu SaaS app-Integration in Azure AD-wissen möchten, finden Sie unter [Was ist Zugriff auf die Anwendung und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um Azure AD-Integration mit Cisco Spark konfigurieren zu können, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- **Cisco Spark** einmalige Anmeldung aktiviert Abonnements


> [AZURE.NOTE] Wenn Sie um die Schritte in diesem Lernprogramm zu testen, empfehlen wir nicht mit einer Umgebung für die Herstellung.


Führen Sie zum Testen der Schritte in diesem Lernprogramm Tips:

- Sie sollten Ihre Umgebung Herstellung nicht verwenden, es sei denn, dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung besitzen, können Sie eine einen Monat zum Testen [hier](https://azure.microsoft.com/pricing/free-trial/)erhalten.


## <a name="scenario-description"></a>Szenario Beschreibung
In diesem Lernprogramm testen Sie Azure AD-einmaliges Anmelden in einer testumgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei Hauptfenster Bausteine:

1. Hinzufügen von Cisco Spark aus dem Katalog
2. Konfigurieren und Testen Azure AD einmaliges Anmelden


## <a name="adding-cisco-spark-from-the-gallery"></a>Hinzufügen von Cisco Spark aus dem Katalog
Zum Konfigurieren der Integration von Cisco Spark in Azure AD müssen Sie Cisco Spark zu Ihrer Liste der verwalteten SaaS apps aus dem Katalog hinzuzufügen.

**Wenn Cisco Spark aus dem Katalog hinzufügen möchten, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der Verzeichnisansicht im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie auf **Hinzufügen** , am unteren Rand der Seite.

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** klicken Sie auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **Cisco Spark**aus.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. Wählen Sie im Ergebnisbereich **Cisco Spark aus**, und klicken Sie dann auf **abgeschlossen** , um die Anwendung hinzugefügt haben.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD-einmaliges Anmelden mit Cisco Spark basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden entwickelt muss Azure AD kennen, kann der Benutzer Gegenstück Cisco Spark einem Benutzer in Azure AD. Kurzum, muss eine Link Beziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Cisco Spark eingerichtet werden.
Dieser Link Beziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als der Wert für den **Benutzernamen** in Cisco Spark zuweisen. Zum Konfigurieren und Azure AD-einmaliges Anmelden mit Cisco Spark testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einmaligen Anmeldens](#configuring-azure-ad-single-single-sign-on)** - damit Ihre Benutzer dieses Feature verwenden können.
2. **[Erstellen einer Azure AD Benutzer testen](#creating-an-azure-ad-test-user)** : Azure AD-einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Cisco Spark Benutzer testen](#creating-a-cisco-spark-test-user)** : ein Gegenstück von Britta Simon in Cisco Spark haben, die in der Azure AD-Darstellung Ihrer verknüpft ist.
5. **[Testen Sie Benutzer zuweisen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon mit Azure AD-einmaliges Anmelden aktivieren.
5. **[Testen der einmaligen Anmeldens](#testing-single-sign-on)** - zur Überprüfung, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren von Azure AD-einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist Azure AD-einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Ihrer Anwendung Cisco Spark.

Cisco Spark Anwendung erwartet die SAML-Assertionen zu bestimmte Attributen enthalten. Konfigurieren Sie die folgenden Attribute für diese Anwendung. Sie können die Werte dieser Attribute der Registerkarte **"Atrributes"** der Anwendung verwalten. Das folgende Bildschirmabbild zeigt ein Beispiel dafür.

![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Führen Sie die folgenden Schritte aus, um Azure AD-einmaliges Anmelden mit Cisco Spark konfigurieren:**

1. Im klassischen Azure Portal **Cisco Spark** Anwendung Integration in die Seite, klicken Sie im Menü oben klicken Sie auf **Attribute**.
     
    ![Konfigurieren Sie einmaliges Anmelden][5]


2. Führen Sie die folgenden Schritte aus, klicken Sie im Dialogfeld **token SAML-Attribute** :

    ein. Klicken Sie auf **Benutzerattribut hinzufügen** , um das Dialogfeld **Benutzer Attribure hinzufügen** zu öffnen.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Geben Sie in das Textfeld **Name Attribut** **Uid**aus.
    
    c. Wählen Sie aus der Liste **Attributwert** **user.userprincipal**ein.
    
    d. Klicken Sie auf **abgeschlossen**. Klicken Sie dann **Änderungen übernehmen** am unteren Rand der Seite.

3. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Konfigurieren Sie einmaliges Anmelden][6]

4. Im Portal klassischen auf der Seite **Cisco Spark** Integration Anwendung klicken Sie auf **Konfigurieren einmaligen Anmeldens** zum Öffnen des Dialogfelds **Konfigurieren einmaliges Anmelden** .

    ![Konfigurieren Sie einmaliges Anmelden][7] 

5. Klicken Sie auf der Seite **Wie möchten Sie Benutzer bei der Cisco Spark auf** **Azure AD einmaliges Anmelden**wählen Sie aus, und klicken Sie dann auf **Weiter**.
    
    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Führen Sie auf der Seite Dialogfeld **Konfigurieren der App-Einstellungen** die folgenden Schritte aus:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    ein. Geben Sie in das Textfeld melden Sie sich auf URL eine URL, die mit dem folgenden Muster: `https://web.ciscospark.com/#/signin`.

    b. Klicken Sie auf **Weiter**.


7. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Cisco Spark** auf **Metadaten herunterladen**möchten, und speichern Sie die Datei auf Ihrem Computer.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Melden Sie sich bei [Cisco Cloud Zusammenarbeit Management](https://admin.ciscospark.com/) für Ihre vollständige Administratorberechtigungen.
9. Wählen Sie **Einstellungen** aus, und klicken Sie auf **Ändern**, klicken Sie im Abschnitt **Authentifizierung** .

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Wählen Sie **Integrieren einer 3rd Drittanbietern Identitätsanbieter aus. (Erweitert)** und navigieren Sie zu dem nächsten Bildschirm.
11. Klicken Sie auf **Metadaten-Datei herunterladen** , und speichern Sie die Datei auf Ihrem Computer.
12. Auf der Seite **Importieren Idp Metadaten** entweder ziehen Sie und legen Sie Azure AD-Metadaten-Datei auf das Zeichenblatt ab oder verwenden Sie die Datei Browser-Option zum Suchen und Azure AD-Metadaten-Datei hochladen. Klicken Sie dann **erfordern Zertifikat wurde von einer Zertifizierungsstelle in Metadaten (sicherer)** wählen Sie aus, und klicken Sie auf **Weiter**. 

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Wählen Sie **Die SSO-Verbindung testen**, und beim Öffnen einer neuen Browserregisterkarte, mit Azure AD authentifizieren Sie, indem Sie anmelden.
14. Zur Browserregisterkarte **Cisco Cloud Zusammenarbeit Management** zurückzukehren. Wenn der Test erfolgreich war, wählen Sie **aus dieser Test erfolgreich war. Aktivieren des einmaligen Anmeldens Option** , und klicken Sie auf **Weiter**.

7. Klicken Sie im Portal Azure AD-klassischen wählen Sie die Bestätigung Konfiguration für einzelne Zeichen, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung für einzelne anmelden** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Benutzers mit Azure AD-testen
In diesem Abschnitt erstellen Sie einen Testbenutzer im klassischen Portal Britta Simon bezeichnet.

![Erstellen von Azure AD-Benutzer][20]

**Führen Sie die folgenden Schritte aus, um einen Testbenutzer in Azure AD zu erstellen:**

1. Klicken Sie im **Azure klassischen Portal**auf der linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis für das Sie Verzeichnisintegration aktivieren möchten.

3. Wenn die Liste der Benutzer, klicken Sie im Menü oben anzeigen möchten, klicken Sie auf **Benutzer**.
    
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Klicken Sie im Dialogfeld **Benutzer hinzufügen** um in der Symbolleiste auf der Unterseite öffnen, auf **Benutzer hinzufügen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Seite **Teilen Sie uns zu diesem Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ des Benutzers neuen Benutzer in Ihrer Organisation ein.

    b. Geben Sie den Benutzernamen **Textfeld** **BrittaSimon**ein.

    c. Klicken Sie auf **Weiter**.

6.  Klicken Sie auf der Seite **Benutzerprofil** Dialogfeld führen Sie die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**aus.  

    b. In das letzte Textfeld **Name** , Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**aus.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erste temporäres Kennwort** auf **Erstellen**.

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Seite **erste temporäres Kennwort** die folgenden Schritte aus:

    ![Erstellen eines Benutzers mit Azure AD-testen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort ein**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-cisco-spark-test-user"></a>Erstellen eines Testbenutzers Cisco Spark

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Cisco Spark. In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Cisco Spark.

1. Wechseln Sie zu der [Cisco Cloud Zusammenarbeit Management](https://admin.ciscospark.com/) für Ihre vollständige Administratorberechtigungen.
2. Klicken Sie auf **Benutzer** und **Verwalten von Benutzern**.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. Wählen Sie im Fenster " **Benutzer verwalten** " **manuell hinzufügen oder Ändern von Benutzern** aus, und klicken Sie auf **Weiter**.
4. Wählen Sie **Namen und e-Mail-Adresse**. Klicken Sie dann füllen Sie das Textfeld wie folgen:

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**

    b. Geben Sie im Textfeld **Nachname** **Simon**

    c. Geben Sie in das Textfeld **e-Mail-Adresse** ein.**britta.simon@contoso.com**

5. Klicken Sie auf das Pluszeichen (+) Britta Simon hinzufügen. Klicken Sie dann auf **Weiter**.
6. Im Fenster **Dienste für Benutzer hinzufügen** klicken Sie auf **Speichern** , und klicken Sie dann auf **Fertig stellen**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Test-Benutzers

In diesem Abschnitt aktivieren Sie Britta Simon Azure einmaliges Anmelden verwenden, indem Sie keinen Zugriff auf Cisco Spark erteilen.

![Benutzer zuweisen][200] 

**Um Cisco Spark Britta Simon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im Portal klassischen zum Öffnen der Anwendungsansicht in der Verzeichnisansicht klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Applikationen **Cisco Spark**aus.

    ![Konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. Klicken Sie auf **Benutzer**, klicken Sie im Menü oben.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste alle Benutzer **Britta Simon**aus.

2. Klicken Sie unten auf der Symbolleiste auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen einmaliges Anmelden

Das Ziel der in diesem Abschnitt ist zum Azure AD-einzelne anmelden Überprüfen der Konfiguration mithilfe des Bedienfelds Access.

Wenn Sie die Kachel Cisco Spark im Bereich Access klicken, Sie sollten automatisch an Ihrer Anwendung Cisco Spark angemeldete abrufen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der zum Integrieren SaaS-Apps mit Azure-Active Directory-Lernprogramme](active-directory-saas-tutorial-list.md)
* [Was ist die Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
