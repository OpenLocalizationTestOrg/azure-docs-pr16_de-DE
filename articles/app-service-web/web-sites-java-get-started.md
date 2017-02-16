<properties
    pageTitle="Erstellen eine Java Web app im App-Verwaltungsdienst Azure | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie eine Java Web app auf App-Verwaltungsdienst Azure bereitgestellt."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Erstellen Sie eine Java-Web-app in Azure-App-Verwaltungsdienst

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In diesem Lernprogramm erfahren, wie Sie eine Java- [Web app im App-Verwaltungsdienst Azure] mithilfe der [Azure-Portal]zu erstellen. Das Azure-Portal ist eine Web-Benutzeroberfläche, die Sie zum Verwalten Ihrer Azure Ressourcen verwenden können.

> [AZURE.NOTE] Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Microsoft Azure-Konto an. Wenn Sie kein Konto haben, können Sie [die Vorteile Ihres Visual Studio Abonnenten aktivieren] , oder [Melden Sie sich für eine kostenlose Testversion]an.
>
> Wenn Sie mit Azure-App-Verwaltungsdienst anzufangen, bevor Sie für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen]. Vorhanden ist, können Sie sofort eine kurzlebige Starter Web app im App-Dienst erstellen; Es ist keine Kreditkarte erforderlich, und keine Zusagen.

## <a name="java-application-options"></a>Java-Anwendungsoptionen

Es gibt verschiedene Arten, um eine Java-Anwendung in einer App-Dienst Web app festgelegt werden können. 

1. Erstellen einer app aus, und klicken Sie dann konfigurieren **ApplicationSettings**.

    App-Dienst bietet mehrere Tomcat und Pier Versionen mit Standard-Konfiguration. Wenn die Anwendung, die Sie gehostet werden, werden mit einer der integrierten Versionen funktionieren, dieses Verfahren zum Einrichten eines Containers Web ist die einfachste und perfekten ist, wenn alle gewünschten führen Sie eine War-Datei zu einem Webcontainer hochladen. Für diese Methode erstellen eine app im Portal Azure, und wechseln Sie dann zu dem Blade **Anwendungseinstellungen** für Ihre app zu Ihrer Version von Java zusammen mit den gewünschten Java Webcontainer auswählen. Diese Methode werden sowohl Java als auch der Webcontainer aus Programmdateien ausgeführt. Die anderen Methoden setzen Sie den Webcontainer und potenziell die JVM in den verfügbaren Speicherplatz. Wenn Sie dieses Modell verwenden, haben Sie keinen Zugriff zum Bearbeiten von Dateien in diesem Teil des Dateisystems. Das heißt, die nicht mögliche Aktionen, beispielsweise konfigurieren Sie die Datei *server.xml* oder platzieren Bibliotheksdateien im Ordner " */ lib* ". Weitere Informationen finden Sie unter der [Erstellen und konfigurieren eine Java Web app](#appsettings) weiter unten in diesem Lernprogramm.
    
2. Verwenden einer Vorlage aus dem Azure Marketplace.

    Die Azure Marketplace enthält Vorlagen, die automatisch erstellen und Konfigurieren von Java Web apps mit Tomcat oder Pier Web Container. Die Web-Container, die die Vorlagen erstellen werden konfiguriert. Weitere Informationen finden Sie unter Abschnitt [mithilfe einer Vorlage Java aus dem Azure Marketplace](#marketplace) dieses Lernprogramms.
  
3. Erstellen einer app manuell kopieren und Bearbeiten von Konfigurationsdateien 

    Möglicherweise möchten eine benutzerdefinierte Java-Anwendung zu hosten, die bereitgestellt werden nicht in einem der Web Container von App-Dienst bereitgestellt wird. Beispiel:
    
    * Ihre Java-Anwendung erfordert eine Version von Tomcat oder Pier, die nicht direkt von der App-Dienst unterstützt oder im Katalog bereitgestellt.
    * Ihre Java-Anwendung akzeptiert HTTP-Anfragen und wird nicht als eine WAR in einem bereits vorhandenen Webcontainer bereitgestellt.
    * Möchten Sie den Webcontainer von Grund auf selbst konfigurieren. 
    * Sie möchten, verwenden Sie eine Version von Java, die im App-Dienst nicht unterstützt wird und es selbst hochladen möchten.

    Für diesen Fällen können Sie Erstellen einer app im Portal Azure verwenden, und geben Sie dann die entsprechende Runtime-Dateien manuell. In diesem Fall werden die Dateien anhand Ihrer Leerzeichen Speicherkontingente für Ihre App-Serviceplan gezählt. Weitere Informationen finden Sie unter [Hochladen eine benutzerdefinierte Java-Web-app in Azure].

## <a name="a-nameportala-create-and-configure-a-java-web-app"></a><a name="portal"></a>Erstellen und Konfigurieren einer Java Web app

In diesem Abschnitt veranschaulicht, wie eine Web app erstellen und konfigurieren Sie ihn für Java **Anwendungseinstellungen** Falz im Portal verwenden.

1. Melden Sie sich bei der [Azure-Portal].

2. Klicken Sie auf **Neu > Web + Mobile > Web App**.

    ![Neue Web App][newwebapp]

4. Geben Sie einen Namen für das Web app im **Web app** -Feld ein.

    Dieser Name muss in der Domäne azurewebsites.net eindeutig sein, da die URL des Web app {Name} ist. azurewebsites.net. Wenn der eingegebene Name nicht eindeutig ist, wird Sie in das Textfeld ein rotes Ausrufezeichen angezeigt.

5. Wählen Sie eine **Ressourcengruppe** oder erstellen Sie einen neuen.

    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden der Azure-Portal zum Verwalten Ihrer Azure Ressourcen].

6. Wählen Sie eine **App-Dienst Plan/Speicherort** aus, oder Erstellen eines neuen Kontos.

    Weitere Informationen zur App-Service-Pläne finden Sie unter [Übersicht über die App-Verwaltungsdienst Azure-Pläne].

7. Klicken Sie auf **Erstellen**.

    ![Erstellen von Web App][newwebapp2]
 
8. Wenn das Web app erstellt wurde, klicken Sie auf **Web Apps > {Web app}**.
 
    ![Wählen Sie Web App][selectwebapp]

9. Klicken Sie in das Blade **Web app** auf **Einstellungen**.

10. Klicken Sie auf **ApplicationSettings**.

11. Wählen Sie die gewünschte **Java-Version**aus. 

12. Wählen Sie die gewünschte **Nebenversion Java**aus. Wenn Sie **Absteigend**auswählen, wird Ihre app die neueste Nebenversion verwendet, die im App-Dienst für diesen Java Hauptversion verfügbar ist. Das Element **Absteigend** gilt nur für die **Anwendung Settings**Dokumentvorlagen Java-apps. Wenn Sie Ihre Java-app aus dem Katalog erstellen, müssen Sie Ihre eigenen Container und JVM Änderungen verwaltet. 

12. Wählen Sie den gewünschten **Web-Container**aus. Wenn Sie einen Containernamen, der mit **Absteigend**beginnt auswählen, wird Ihre app in der neuesten Version von diesem Web Container-Hauptversion beibehalten, die im App-Dienst zur Verfügung. 

    ![Web Container Versionen][versions]

13. Klicken Sie auf **Speichern**.

    In einen Moment werden Ihre Web app erst Java-basierte und konfiguriert den Webcontainer verwenden, den Sie ausgewählt haben.

14. Klicken Sie auf **Web apps > {neue Web app}**.

15. Klicken Sie auf die **URL** , um die neue Website zu suchen.

    Der Webseite bestätigt, dass Sie eine Java-basierte Web app erstellt haben.

## <a name="a-namemarketplacea-use-a-java-template-from-the-azure-marketplace"></a><a name="marketplace"></a>Verwenden einer Java-Vorlage aus dem Azure Marketplace

In diesem Abschnitt wird gezeigt, wie die Azure Marketplace zum Erstellen einer Java Web app verwendet wird. Der gleiche allgemeine Fluss kann auch zum Erstellen eines Java-basierte Mobile oder API-app verwendet werden. 

1. Melden Sie sich bei der [Azure-Portal]

2. Klicken Sie auf **Neu > Marketplace**.

    ![Neue Marketplace][newmarketplace]

3. Klicken Sie auf **Web + Mobile**.

    Möglicherweise müssen links einen Bildlauf durchführen, um das Blade **Marketplace** anzuzeigen, in dem Sie **Web + Mobile**auswählen zu können.

4. Klicken Sie im Textfeld suchen Geben Sie den Namen der Java-Anwendungsserver, wie z. B. **Apache Tomcat** oder **Pier**, und drücken Sie dann die EINGABETASTE.

5. Klicken Sie in den Suchergebnissen auf die Java-Anwendungsserver.

    ![Mobile Pier Web][webmobilejetty]

6. Klicken Sie in der ersten **Apache Tomcat** oder **Pier** Blade auf **Erstellen**.

    ![Jetty Portal Blade][jettyblade]

7. Geben Sie einen Namen für das Web app im **Web app** -Feld in das nächste **Apache Tomcat** oder **Pier** Blade.

    Dieser Name muss in der Domäne azurewebsites.net eindeutig sein, da die URL des Web app {Name} ist. azurewebsites.net. Wenn der eingegebene Name nicht eindeutig ist, wird Sie in das Textfeld ein rotes Ausrufezeichen angezeigt.

8. Wählen Sie eine **Ressourcengruppe** oder erstellen Sie einen neuen.

    Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden der Azure-Portal zum Verwalten Ihrer Azure Ressourcen].

9. Wählen Sie eine **App-Dienst Plan/Speicherort** aus, oder Erstellen eines neuen Kontos.

    Weitere Informationen zur App-Service-Pläne finden Sie unter [Übersicht über die App-Verwaltungsdienst Azure-Pläne].

10. Klicken Sie auf **Erstellen**.

    ![Erstellen von jetty-Portal][jettyportalcreate2]

    In kurzer Zeit, in der Regel weniger als einer Minute endet Azure das neuen Web app erstellen.

11. Klicken Sie auf **Web apps > {neue Web app}**.

12. Klicken Sie auf die **URL** , um die neue Website zu suchen.

    ![Jetty URL][jettyurl]

    Tomcat im Lieferumfang von eines Standardsatz von Seiten; Wenn Sie Tomcat ausgewählt haben, möchten Sie eine Seite ähnlich wie im folgenden Beispiel sehen.

    ![Verwenden von Apache Tomcat Web-app][tomcat]

    Wenn Sie Pier, möchten Sie eine Seite ähnlich wie im folgenden Beispiel wird angezeigt. Pier muss keinen Standardsatz Seite, damit die gleiche JSP, die für eine leere Java-Website verwendet wird hier wiederverwendet wird.

    ![Verwenden von Pier Web-app][jetty]

Jetzt, da Sie die Web-app mit einem Container app erstellt haben, finden Sie im Abschnitt [Weitere Schritte](#next-steps) Informationen dazu, wie Sie Ihrer Anwendung zu Web app hochladen aus.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt müssen Sie einen Java-Anwendungsserver in die Web-app im App-Verwaltungsdienst Azure ausgeführt. Bereitstellen von eigenem Code für das Web app finden Sie unter [Hinzufügen einer Anwendung oder Webseite ein, um Ihre Java Web app].

Weitere Informationen zum Entwickeln von Java-Anwendungen in Azure finden Sie im [Java Developer Center].

<!-- URL List -->

[Hinzufügen einer Anwendung oder eine Webseite zu Ihrer Java-Web-app]: ./web-sites-java-add-app.md
[Azure-App-Service-Pläne (Übersicht)]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure-Portal]: https://portal.azure.com/
[Aktivieren Sie die Vorteile Ihres Visual Studio-Abonnent]: http://go.microsoft.com/fwlink/?LinkId=623901
[Registrieren Sie sich für eine kostenlose Testversion]: http://go.microsoft.com/fwlink/?LinkId=623901
[Wiederholen Sie den App-Dienst]: http://go.microsoft.com/fwlink/?LinkId=523751
[Web app im App-Verwaltungsdienst Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java-Entwicklercenter]: /develop/java/
[Verwenden zum Verwalten Ihrer Azure Ressourcen Azure-Portal]: ../azure-portal/resource-group-portal.md
[Hochladen einer benutzerdefinierten Java Web app zu Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
