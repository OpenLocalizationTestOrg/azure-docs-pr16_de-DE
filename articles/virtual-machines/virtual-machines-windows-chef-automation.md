<properties
   pageTitle="Bereitstellung von Azure-virtuellen Computern mit Verwaltungsangestellte | Microsoft Azure"
   description="Erfahren Sie, wie mit Verwaltungsangestellte automatisierte virtuellen Computern Bereitstellung und Konfiguration auf Microsoft Azure führen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatisierung der Bereitstellung von Azure-virtuellen Computern mit Bäckermeister

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Verwaltungsangestellte ist ein großartiges Tool für die Bereitstellung von Automatisierung und des gewünschten Status Konfigurationen.

Mit unseren neueste Version Cloud-api bietet Verwaltungsangestellte nahtlose Integration in Azure, was Ihnen die Möglichkeit, Bereitstellung und Konfiguration Staaten über einen einzelnen Befehl bereitstellen.

In diesem Artikel werde ich zeigen, wie Ihre Bereitstellung von Azure-virtuellen Computern an, und zeigen Sie durch das Erstellen einer Richtlinie oder "Kochbuch", und klicken Sie dann auf eine Azure-virtuellen Computern bereitstellen dieses Kochbuch Verwaltungsangestellte-Umgebung einrichten.

Los geht's!

## <a name="chef-basics"></a>Bäckermeister-Grundlagen

Bevor Sie beginnen, vorschlagen ich an, dass Sie die grundlegenden Konzepte von Verwaltungsangestellte überprüfen. Es gibt gute Material <a href="http://www.chef.io/chef" target="_blank">hier</a> und ich empfehlen Sie eine schnelle gelesen haben, bevor Sie diese Schritte versuchen. Ich wird jedoch die Grundlagen Wiederholung, bevor wir beginnen.

Das folgende Diagramm enthält, die auf hoher Ebene Verwaltungsangestellte Architektur.

![][2]

Verwaltungsangestellte besteht aus drei Hauptkomponenten Architektur: Bäckermeister Server, Verwaltungsangestellte Client (Knoten) und Verwaltungsangestellte Arbeitsstationen.

Der Bäckermeister-Server ist unsere Management-Punkt, und es gibt zwei Optionen für den Server Verwaltungsangestellte: eine gehostete Lösung oder eine lokale-Lösung. Wir verwenden eine gehostete Lösung.

Der Bäckermeister-Client (Knoten) ist der Agent, der auf den Servern befindet, die Sie verwalten.

Die Verwaltungsangestellte Arbeitsstationen ist unsere Admin-Arbeitsstationen, wo wir unsere Richtlinien erstellen und Ausführen von unserem Befehle zur Verwaltung des. Wir führen Sie den Befehl **Details** aus der Verwaltungsangestellte Arbeitsstationen unsere Infrastruktur verwalten.

Es gibt auch des Konzepts der "Kochbücher" und "Rezepte". Dies sind effektiv die Richtlinien, die wir definieren und anwenden auf unseren Servern.

## <a name="preparing-the-workstation"></a>Vorbereiten der Arbeitsstationen

Zunächst können Sie die Arbeitsstationen vorbereiten. Verwende ich eine Windows Arbeitsstationen ein. Wir müssen ein Verzeichnis zum Speichern von unserem Config-Dateien und Kochbücher zu erstellen.

Erstellen Sie zuerst ein Verzeichnis namens C:\chef.

Erstellen Sie dann eine zweite Verzeichnis c:\chef\cookbooks bezeichnet.

Jetzt müssen wir unsere Azure-Einstellungsdatei herunterladen, damit Verwaltungsangestellte mit unseren Azure-Abonnement kommunizieren kann.

Laden Sie Ihre Einstellungen aus [hier](https://manage.windowsazure.com/publishsettings/)veröffentlichen.

Speichern Sie die Einstellungsdatei Veröffentlichen in C:\chef ein.

##<a name="creating-a-managed-chef-account"></a>Erstellen eines verwalteten Bäckermeister-Kontos

Melden Sie sich bei einem gehosteten Verwaltungsangestellte Konto [hier](https://manage.chef.io/signup).

Während des Prozesses beim Registrieren werden Sie aufgefordert, eine neue Organisation erstellen.

![][3]

Nachdem Sie Ihrer Organisation erstellt wurde, laden Sie das Starterkit aus.

![][4]

> [AZURE.NOTE] Wenn Sie gefragt werden Sie darauf hingewiesen, dass Ihre Keys zurückgesetzt werden, ist es ok, um den Vorgang fortzusetzen, da wir keine vorhandenen Infrastruktur als noch nicht konfiguriert haben.

Diese Starter Kit Zip-Datei enthält Ihrer Organisation Config-Dateien und Schlüssel.

##<a name="configuring-the-chef-workstation"></a>Konfigurieren der Verwaltungsangestellte Arbeitsstationen

Extrahieren des Inhalts der Bäckermeister-starter.zip zu C:\chef.

Kopieren Sie alle Dateien unter Bäckermeister-Starter\chef-Repo\.Verwaltungsangestellte in Ihrem Verzeichnis c:\chef.

Ihr Verzeichnis sollte jetzt ungefähr wie im folgenden Beispiel aussehen.

![][5]

Sie sollten jetzt vier Dateien, einschließlich der Azure publishing-Datei im Stammverzeichnis der c:\chef verfügen.

Die PEM-Dateien enthalten Ihrer Organisation und der Administrator private Schlüssel für die Kommunikation, während Sie die Datei knife.rb die Konfiguration Details enthält. Wir müssen zum Bearbeiten der Datei knife.rb.

Öffnen Sie die Datei in Editor Ihrer Wahl, und ändern Sie die "Cookbook_path" durch das Entfernen der /... / aus dem Pfad, damit er angezeigt wird, wie nachfolgend dargestellt.

    cookbook_path  ["#{current_dir}/cookbooks"]

Fügen Sie auch die folgende Zeile über die entsprechenden des Namens Ihrer Azure veröffentlichen Einstellungsdatei.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Die Datei knife.rb sollte nun ähnlich wie im folgenden Beispiel aussehen.

![][6]

Diese Zeilen werden sichergestellt, Details verweist auf die Kochbücher Verzeichnis unter c:\chef\cookbooks, und auch unsere Azure veröffentlichen Einstellungsdatei während Azure Vorgänge verwendet.

## <a name="installing-the-chef-development-kit"></a>Installieren der Verwaltungsangestellte Development Kit

Nächste [herunterladen und installieren](http://downloads.getchef.com/chef-dk/windows) der ChefDK (Verwaltungsangestellte Development Kit) zum Einrichten des Computers Verwaltungsangestellte.

![][7]

Installieren Sie den Standardspeicherort der c:\opscode ein. Diese Installation dauert ungefähr 10 Minuten.

Vergewissern Sie sich, dass Ihre Variable PATH-Einträge für C:\opscode\chefdk\bin enthält. C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Wenn sie nicht vorhanden sind, stellen Sie sicher, dass Sie diese Pfade hinzufügen!

*HINWEIS DIE REIHENFOLGE DER PFAD IST WICHTIG!* Wenn Ihre Opscode Pfade nicht in der richtigen Reihenfolge sind, haben Sie Probleme.

Starten Sie Ihre Arbeitsstationen neu, bevor Sie fortfahren.

Als Nächstes werden wir die Details Azure-Erweiterung installieren. Auf diese Weise Details mit den "Azure-Plug-Ins".

Führen Sie den folgenden Befehl ein.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Das Argument – Pre wird sichergestellt, dass Sie die neueste Version der Details Azure-Plug-Ins RC erhält der Zugriff auf den letzten Satz von APIs ermöglicht.

Ist zu rechnen, die eine Reihe von Abhängigkeiten auch zur gleichen Zeit installiert werden.

![][8]


Um sicherzustellen, dass alles richtig konfiguriert ist, führen Sie den folgenden Befehl ein.

    knife azure image list

Wenn alles richtig konfiguriert ist, wird eine Liste der verfügbaren Azure Bilder blättern Sie durch angezeigt.

Herzlichen Glückwunsch! Die Arbeitsstationen eingerichtet ist!

##<a name="creating-a-cookbook"></a>Erstellen einer Kochbuch

Ein Kochbuch wird von Verwaltungsangestellte verwendet, um eine Reihe von Befehlen zu definieren, die auf Ihre verwalteten Client ausgeführt werden soll. Erstellen einer Kochbuch recht einfach ist, und wir mithilfe des Befehls **Verwaltungsangestellte generieren Kochbuch** um unsere Kochbuch Vorlage zu erzeugen. Ich wird mein Webserver Kochbuch aufrufen werden, wie ich eine Richtlinie möchten, die automatisch IIS bereitgestellt wird.

Führen Sie unter Ihrem Verzeichnis C:\Chef den folgenden Befehl ein.

    chef generate cookbook webserver

Dadurch wird eine Reihe von Dateien im Verzeichnis C:\Chef\cookbooks\webserver generiert. Jetzt müssen wir definieren den Satz von Befehlen, dass wir unsere Verwaltungsangestellte Client für unsere verwalteten virtuellen Computern nicht ausführen möchten.

Die Befehle sind in der Datei default.rb gespeichert. In dieser Datei wird ich eine Reihe von Befehlen festzulegen, die IIS-Installationen, startet IIS und Vorlagendatei in den wwwroot-Ordner kopiert.

Ändern Sie die Datei C:\chef\cookbooks\webserver\recipes\default.rb, und fügen Sie die folgenden Zeilen hinzu.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Speichern Sie die Datei, sobald Sie fertig sind.

## <a name="creating-a-template"></a>Erstellen einer Vorlage

Wie bereits zuvor erwähnt, müssen wir eine Vorlagendatei generieren, die als unsere default.html Seite verwendet werden soll.

Führen Sie den folgenden Befehl aus, um die Vorlage zu generieren.

    chef generate template webserver Default.htm

Jetzt navigieren Sie zu der Datei C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Bearbeiten Sie die Datei, indem Sie einige einfachen "Hallo Welt" HTML-Code hinzufügen, und klicken Sie dann speichern Sie die Datei.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Hochladen der Kochbuch auf dem Server Bäckermeister

In diesem Schritt werden wir aufzeichnen eine Kopie der Kochbuch, die wir auf Ihrem lokalen Computer erstellt haben und es auf die Verwaltungsangestellte Host Server hochgeladen wird. Sobald hochgeladen wird, wird die Kochbuch unter der Registerkarte **Richtlinie** angezeigt.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Bereitstellen eines virtuellen Computers mit Details Azure

Wir nun eine Azure-virtuellen Computern bereitstellen und Anwenden der Kochbuch "Webserver" wodurch unsere IIS-Web-Dienst und Standardwert die Webseite installiert werden.

Um dies zu tun, verwenden Sie den Befehl **Details Azure-Server erstellen** .

Bin, dass die nächsten Beispiel für den Befehl angezeigt wird.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Die Parameter sind sofort verständlich. Ersetzen von bestimmten Variablen und ausführen.

> [AZURE.NOTE] Durch das die Befehlszeile ich bin auch Automatisieren von meinem Regeln Endpunkt Netzwerk Filtern mit dem Parameter – Tcp-Endpunkte. Kann ich haben von Ports 80 und 3389 bieten Zugriff auf meiner Webseite und RDP-Sitzung geöffnet.

Nachdem Sie den Befehl ausführen, wechseln Sie zur Azure-Portal, und sehen Sie Ihrem Computer bereitstellen zu beginnen.

![][13]

Im Eingabeaufforderungsfenster wird weiter.

![][10]

Nach Abschluss die Bereitstellung, werden wir eine Verbindung mit dem Webdienst über den Port 80 herstellen, wie wir den Port geöffnet hatten, wenn wir nach der Bereitstellung des virtuellen Computers mit dem Befehl Details Azure. Ungeändert dieses virtuellen Computers der nur virtuellen Computern in meinem Clouddienst, werde ich mit der Cloud-Service-Url Verbindung herstellen.

![][11]

Wie Sie sehen können, habe ich mit meinem HTML-Code kreative.

Vergessen Sie nicht, dass wir auch über eine RDP-Sitzung vom Azure klassischen Portal über Port 3389 herstellen können.

Dies wurde hilfreich hoffentlich! Wechseln Sie, und starten Sie Ihre Infrastruktur als Code Reise mit Azure heute!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
