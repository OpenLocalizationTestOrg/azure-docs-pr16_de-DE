<properties
    pageTitle="Einrichten von Apache Tomcat einer Linux virtuellen Computers | Microsoft Azure"
    description="Informationen zum Einrichten von Apache Tomcat7 mit einer Azure-virtuellen Computern (virtueller Computer) mit Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>So richten Sie Tomcat7 auf einem Computer mit Microsoft Azure Linux

Apache Tomcat (oder einfach Tomcat, früher auch Jakarta Tomcat) ist ein open-Source-Webserver und Servletcontainer entwickelt von Apache Software Foundation (ASF). Tomcat implementiert das Java Servlet und die Angaben Java Server Pages (JSP) von Sonne Microsystems und stellt eine reine Java HTTP-Web Server-Umgebung in der Java-Code ausgeführt werden. Die einfachste Konfiguration ausgeführt wird Tomcat in einem einzigen Betriebssystem-Prozess ein. Dieses Verfahren wird ein Java-virtuellen Computern (JVM) ausgeführt. Jede HTTP-Anforderung über einen Browser zu Tomcat wird als separate Thread im Prozess Tomcat verarbeitet werden.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In diesem Handbuch tomcat7 auf ein Bild Linux installieren und es in Microsoft Azure bereitstellen.  

Lernen Sie Folgendes:  

-   Informationen zum Erstellen eines virtuellen Computers in Azure.
-   So bereiten Sie tomcat7 des virtuellen Computers.
-   So installieren Sie tomcat7.

Es wird vorausgesetzt, dass der Reader bereits ein Azure-Abonnement hat.  Andernfalls können Sie sich für eine kostenlose Testversion am [http://azure.microsoft.com](https://azure.microsoft.com/)registrieren. Wenn Sie ein MSDN-Abonnement verfügen, finden Sie unter [Microsoft Azure spezielle Preisgestaltung: MSDN, MPN und Bizspark Vorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Wenn Sie weitere Informationen zur Azure finden Sie unter [Neuigkeiten Azure?](https://azure.microsoft.com/overview/what-is-azure/).

In diesem Thema wird davon ausgegangen, dass Sie grundlegende Kenntnisse Tomcat und Linux verfügen.  

##<a name="phase-1-create-an-image"></a>Phase 1: Erstellen eines Bilds
In dieser Phase erstellen Sie einen virtuellen Computer mithilfe eines Bilds Linux in Azure.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Schritt 1: Generieren eines SSH Authentifizierung Schlüssels
SSH ist Werkzeug System-Administratoren. Konfigurieren der Access-Sicherheit basierend auf einem Kennwort Human bestimmt ist jedoch nicht empfohlen. Autorisierte Benutzer können in Ihrem System basierend auf einen Benutzernamen und ein Kennwort schwachen Teile aufzuteilen.

Die gute Nachricht ist, dass es eine Möglichkeit, lassen Sie remote Access geöffnet und nicht über die Kennwörter Gedanken machen müssen. Die Methode der Authentifizierung mit asymmetrische Verschlüsselung besteht aus. Privaten Schlüssel des Benutzers wird, die die Authentifizierung gewährt. Sie können auch das Benutzerkonto um Kennwortauthentifizierung vollständig verbieten sperren.

Ein weiterer Vorteil dieser Methode ist, dass Sie verschiedene Kennwörter für die Anmeldung bei anderen Servern nicht benötigen. Sie können die Authentifizierung mittels des persönlichen privaten Schlüssels auf allen Servern verhindert, dass mehrere Kennwörter merken müssen.

Es kann auch mit Anfordern eines Kennworts bei dieser Methode anmelden.

Wie folgt vor, um die SSH Authentifizierung Taste generieren.

1.  Herunterladen und Installieren von Puttygen von folgendem Speicherort: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Führen Sie PUTTYGEN. EXE-DATEI.
3.  Klicken Sie auf **generieren** , um die Tasten generieren. Im Prozess können Sie Zufälligkeit erhöhen, indem Sie die Maus über den leeren Bereich im Fenster bewegen.  
![][1]
4.  Nachdem der Prozess generieren wird Puttygen.exe Ihrer generierten Schlüssel angezeigt. Beispiel:  
![][2]
5.  Wählen Sie aus, und kopieren Sie den öffentlichen Schlüssel **Schlüssel** , und in einer Datei namens publicKey.pem gespeichert. Nicht klicken Sie auf **Speichern öffentlicher Schlüssel**, da die gespeicherten öffentlicher Schlüssel-Dateiformat von öffentlicher Schlüssel unterscheidet, die wir möchten.
6.  Klicken Sie auf **privaten Schlüssel speichern** , und speichern Sie sie in einer Datei namens privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Schritt 2: Erstellen des Bilds im Azure-Portal an.
Klicken Sie im [Portal Azure](https://portal.azure.com/)in der Taskleiste So erstellen ein Bild, das Linux Bild entsprechend Ihren Anforderungen auswählen auf **neu** . Im folgende Beispiel wird das Bild Ubuntu 14.04 verwendet.
![][3]

Für **Hostnamen** Geben Sie den Namen für die URL, die Sie und Internet-Clients verwenden, um dieses virtuellen Computers zugreifen. Den letzten Teil der DNS-Name, beispielsweise Tomcatdemo, definieren und Azure wird die URL als tomcatdemo.cloudapp.net erstellen.  

Kopieren Sie den Schlüsselwert **SSH Authentifizierung-Taste**aus der **publicKey.pem** -Datei, die den öffentlichen Schlüssel von Puttygen generierten enthält.  
![][4]

Konfigurieren Sie bei Bedarf weitere Einstellungen, und klicken Sie dann auf erstellen.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Phase 2: Vorbereiten des virtuellen Computers für Tomcat7
In dieser Phase werden Sie einen Endpunkt für Tomcat Datenverkehr konfigurieren und Verbinden mit dem neuen virtuellen Computer.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Schritt 1: Öffnen Sie den HTTP-Port um Webzugriff zulassen
Endpunkte in Azure bestehen ein Protokoll (TCP oder UDP), zusammen mit einem öffentlichen und privaten Port. Der private Port wird, die der Dienst des virtuellen Computers überwacht. Der öffentliche Port wird, die der Azure-Cloud-Dienst extern für eingehende, internetbasierten Datenverkehr überwacht.  

TCP-Anschluss 8080 ist der Anschluss Standardanzahl auf welche Tomcat überwacht. Öffnen diesen Anschluss mit Azure Endpunkt, den Sie und andere Clients Zugriff auf das Internet Seiten tomcat zulassen.  

1.  Klicken Sie auf **Durchsuchen**, im Portal Azure -> **virtuellen Computers**, und klicken Sie dann auf den virtuellen Computern, die Sie erstellt haben.  
![][5]
2.  Wenn Sie einen Endpunkt des virtuellen Computers hinzugefügt haben, klicken Sie auf das Feld **Endpunkte** .
![][6]
3.  Klicken Sie auf **Hinzufügen**.  
    1.  Für den **Endpunkt**geben einen Namen für den Endpunkt Endpunkt ein, und geben Sie dann in **Öffentlichen Port**80.  

        Wenn Sie auf 80 festlegen, brauchen Sie die Port-Nummer in der URL enthalten, die Sie Tomcat zugreifen können. Beispielsweise http://tomcatdemo.cloudapp.net.    

        Wenn Sie es in einen anderen Wert, z. B. 81, festlegen müssen Sie die URL für den Tomcat Zugriff auf die Nummer des Ports hinzuzufügen. Beispielsweise Http://tomcatdemo.cloudapp.net:81 /.
    2.  Geben Sie "Privat" Port 8080. Standardmäßig hört Tomcat TCP-8080. Wenn Sie geändert haben die Standardeinstellung Abhören Port des Tomcat, sollten Sie privat Port benutzerspezifisch, dass der Tomcat identisch Port Abhören aktualisieren.  
    ![][7]

4.  Klicken Sie auf **OK** , um den Endpunkt des virtuellen Computers hinzuzufügen.



###<a name="step-2-connect-to-the-image-you-created"></a>Schritt 2: Verbinden Sie mit dem Bild, das Sie erstellt haben.
Sie können eine beliebige SSH-Tool für die Verbindung zu Ihrem virtuellen Computern auswählen. In diesem Beispiel verwenden wir kitten.  

Rufen Sie zunächst, den DNS-Namen des virtuellen Computers vom Azure-Portal an. **Klicken Sie auf Durchsuchen** -> **virtuellen Computern** -> den Namen des virtuellen Computers -> **Eigenschaften**, und suchen Sie im Feld **Domain Name** der Kachel **Eigenschaften** .  

Rufen Sie die Nummer des Ports für SSH Verbindungen aus dem Feld **SSH** . Hier ist ein Beispiel für ein.  
![][8]

Laden Sie kitten von [hier](http://www.putty.org/) aus.  

Nach dem Herunterladen, klicken Sie auf die ausführbare Datei KITTEN. EXE-DATEI. Konfigurieren der Standardoptionen mit dem Hostnamen und port-Nummer, die von den Eigenschaften des virtuellen Computers abgerufen. Hier ist ein Beispiel:  
![][9]

Klicken Sie im linken Bereich auf **Verbindung** -> **SSH** -> **Authentifizierung** , und klicken Sie dann auf **Durchsuchen** , um den Speicherort der Datei **privateKey.ppk** angeben, die den privaten Schlüssel enthält generiert durch Puttygen in Phase 1: erstellen ein Bilds. Hier ist ein Beispiel:  
![][10]

Klicken Sie auf **Öffnen**. Sie können einem Meldungsfeld benachrichtigt werden. Wenn Sie den DNS-Namen konfiguriert haben und-Nummer richtig Port, klicken Sie auf **Ja**.
![][11]  


Sie sollten Folgendes angezeigt:  
![][12]

Geben Sie den Benutzernamen angegeben, bei der Erstellung des virtuellen Computers in Phase 1: erstellen ein Bilds. Sie sehen etwa so aus:  
![][13]





##<a name="phase-3-install-software"></a>Phase 3: Installieren der software
In dieser Phase installieren Sie die Java Runtime-Umgebung, Tomcat und andere Tomcat-Komponenten.  

###<a name="java-runtime-environment"></a>Java Runtime-Umgebung
Tomcat ist in Java geschrieben. Es gibt zwei Arten von Java Entwicklungskits (JDKs) (OpenJDK und Oracle JDK), und Sie können das gewünschte auswählen.  

>AZURE. Hinweis: Beide JDKs fast den gleichen Code für die Klassen haben, in der Java-API, aber der Code für den virtuellen Computer tatsächlich unterscheidet. Wenn es sich um Bibliotheken geht, eher OpenJDK geöffnete Bibliotheken verwenden, während Oracle eher geschlossene diejenigen verwenden. Oracle JDK mehr, besitzt aber Klassen und einige festen Fehlern und Oracle JDK Stabilität gegenüber OpenJDK ist.

Die folgenden Befehle Herunterladen der anderen JDKs.  

Öffnen-jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle-jdk  

-   JDK aus der Oracle-Website herunterladen:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   So erstellen Sie ein Verzeichnis für die JDK-Dateien enthalten:  

        sudo mkdir /usr/lib/jvm  

-   Die JDK-Dateien in das Verzeichnis/Usr/Bibliothek/Jvm/extrahiert werden sollen:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Oracle JDK als Standard JVM festlegen:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
Sie können einen Befehl wie den folgenden verwenden, um zu testen, ob die Java Runtime-Umgebung ordnungsgemäß installiert ist:  

    java -version  

Wenn Sie öffnen-Jdk installiert haben, sollten Sie eine Meldung wie die folgende angezeigt:![][14]

Wenn Sie Oracle-Jdk installiert haben, sollten Sie eine Meldung wie die folgende angezeigt:![][15]

###<a name="tomcat7"></a>Tomcat7
Verwenden den folgenden Befehl aus, um tomcat7 zu installieren:  

    sudo apt-get install tomcat7  

Wenn Sie tomcat7 nicht verwenden, verwenden Sie die entsprechende Variation dieser Befehl.  

####<a name="test"></a>Test:

Zum Überprüfen, ob tomcat7 erfolgreich installiert wurde, navigieren Sie zu Ihrer Tomcat Servernamen DNS-Einträge (http://tomcatexample.cloudapp.net/ ist die URL Beispiel in diesem Artikel). Wenn Sie eine Seite wie folgt angezeigt wird, haben Sie installiert tomcat7 richtig.
![][16]


###<a name="install-other-tomcat-components"></a>Andere Tomcat Komponenten installieren
Es gibt andere optional Tomcat-Komponenten, die Sie auch installieren können.  

Verwenden Sie den Befehl **Sudo apt-Cache-Suche tomcat7** , um alle verfügbaren Komponenten anzuzeigen. Die folgenden Befehle sind Beispiele für einige hilfreichen Teile zu installieren.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Phase 4: Konfigurieren von Tomcat
In dieser Phase verwalten Sie Tomcat.
###<a name="start-and-stop-tomcat7"></a>Starten und Beenden von tomcat7
Der Server tomcat7 wird automatisch gestartet, bei der Installation. Sie können auch sie sich mit dem folgenden Befehl starten:   

    sudo /etc/init.d/tomcat7 start

So beenden Sie die tomcat7:  

    sudo /etc/init.d/tomcat7 stop

So zeigen Sie den Status der tomcat7 an:  

    sudo /etc/init.d/tomcat7 status

Tomcat-Dienste neu zu starten:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Tomcat administration
Sie können die Datei Tomcat Benutzer Konfiguration für die Einrichtung von Administrator-Anmeldeberechtigungen mit den folgenden Befehl bearbeiten:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Hier ist ein Beispiel:  
![][17]  

>AZURE. Hinweis: Erstellen Sie ein sicheres Kennwort für den Administratorbenutzernamen.  

Nachdem Sie diese Datei zu bearbeiten, sollten Sie tomcat7 Services mit den folgenden Befehl aus, um sicherzustellen, dass die Änderungen wirksam werden neu starten:  

    sudo /etc/init.d/tomcat7 restart  

Öffnen Sie Ihren Browser, und geben Sie die URL **http://<your tomcat server DNS name>/Manager/html**. Damit das Beispiel in diesem Artikel ist die URL http://tomcatexample.cloudapp.net/manager/html.  

Nach dem Herstellen der Verbindung sollte ähnlich wie folgt angezeigt werden:  
![][18]

##<a name="common-issues"></a>Allgemeine Probleme

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Die virtuellen Computern mit Tomcat und Moodle nicht aus dem Internet zugreifen werden.

-   **Problem**  
Tomcat ausgeführt wird, aber nicht die Standardseite Tomcat in Ihrem Browser angezeigt.
-   **Mögliche Stamm Groß-/Kleinschreibung**   
    1.  Port zum Überwachen der Tomcat ist nicht identisch mit Ihrer virtuellen Computers Endpunkt für Tomcat Datenverkehr den Port "Privat".  

        Überprüfen Sie Ihre öffentliche und Private Ports Endpunkt Einstellungen, und stellen Sie sicher, dass der Port privat ist, dass entspricht der Tomcat Port abhören. Finden Sie unter Phase 1: Erstellen ein Bilds Anweisungen zum Konfigurieren von Endpunkten für den virtuellen Computer.  

        Öffnen Sie für die Ermittlung den Port zum Überwachen Tomcat /etc/httpd/conf/httpd.conf (veröffentlichte Version Red Hat) oder /etc/tomcat7/server.xml (Debian veröffentlichte Version) aus. Standardmäßig ist die Port zum Überwachen der Tomcat 8080. Hier ist ein Beispiel:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Wenn Sie einen virtuellen Computer verwenden wie Debian oder Ubuntu und Sie die standardmäßigen Port der Tomcat Abhören (beispielsweise 8081) ändern möchten, sollten Sie auch den Port für das Betriebssystem öffnen. Öffnen Sie zuerst das Profil ein:  

            sudo vi /etc/default/tomcat7  

        Klicken Sie dann heben Sie die letzte Zeile, und ändern Sie "Nein", "Ja".  

            AUTHBIND=yes

    2.  Die Firewall hat den Port zum Überwachen der Tomcat deaktiviert.

        Wenn Sie nur die Standardseite Tomcat vom lokalen Host sehen können, ist das Problem wahrscheinlich enthält der Anschluss mit dem durch Tomcat überwacht wird durch die Firewall blockiert wird. Das Tool w3m können die Webseite zu durchsuchen. Die folgenden Befehle w3m installieren, und navigieren Sie zu der Standardseite Tomcat:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Lösung**
    1. Wenn der Tomcat Abhören Anschluss ist nicht dieselbe als "Privat" Port für den Endpunkt für den Datenverkehr in des virtuellen Computers, müssen Sie den Port privat benutzerspezifisch, dass der Tomcat identisch Port Abhören ändern.   

    2.  Wenn das Problem durch die Firewall/Iptables verursacht wird, fügen Sie die folgenden Zeilen zu /etc/sysconfig/iptables aus:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Beachten Sie, dass die zweite Zeile nur für Https-Datenverkehr erforderlich ist.  

        Stellen Sie sicher, dass dies ist über alle Zeilen an, die Global Einschränken des Zugriffs, beispielsweise die folgende möchten:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Um die Iptables neu zu laden, führen Sie den folgenden Befehl aus:  

            service iptables restart  

        Dies wurde auf CentOS 6.3 getestet.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Zugriff verweigert, wenn Sie project-Dateien /var/lib/tomcat7/webapps Upload /  

-   **Problem**  
Wenn Sie alle SFTP-Client (z. B. FileZilla) verwenden, um eine Verbindung mit Ihrem virtuellen Computern und navigieren Sie zu /var/lib/tomcat7/webapps/zu Ihrer Website veröffentlichen, erhalten Sie eine Fehlermeldung wie die folgende:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Mögliche Stamm Groß-/Kleinschreibung** Sie haben keine Berechtigungen zum Zugriff auf den Ordner /var/lib/tomcat7/webapps.  
-   **Lösung**  
Sie müssen über die Berechtigung aus dem Stammordner Konto erhalten. Sie können den Besitz dieses Ordners Stammwebsitesammlung für den Benutzernamen ändern, die Sie beim Bereitstellen von des Computers verwendet. Hier ist ein Beispiel mit dem Kontonamen Azureuser ein:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    Verwenden Sie die Option -R, um die Berechtigungen für alle Dateien in ein Verzeichnis zu anzuwenden.  

    Beachten Sie, dass dieser Befehl auch für Verzeichnisse durchsuchen funktioniert. Die Option-R ändert sich die Berechtigungen für alle Dateien und Verzeichnisse innerhalb des Verzeichnisses. Hier ist ein Beispiel:  

        sudo chown -R username:group directory  

    Dieser Befehl ändert den Besitz (Benutzer und Gruppen) für alle Dateien und Verzeichnisse innerhalb Verzeichnisses und Verzeichnis selbst.  

    Mit dem folgende Befehl ändert sich die Berechtigungen des Verzeichnisses Ordner nur aber belässt die Dateien und Ordner, in dem Verzeichnis alleine.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
