<properties 
    pageTitle="So installieren Sie Apache Qpid Proton-C auf einer Linux VM | Microsoft Azure"
    description="So erstellen Sie ein CentOS Linux virtueller Computer mit Azure-virtuellen Computern und zum Erstellen und installieren die Apache Qpid Proton-C-Bibliothek."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Installieren Sie ein Azure Linux virtuellen Computers Apache Qpid Proton-C

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Dieser Abschnitt listet die zum Erstellen eines CentOS Linux virtuellen Computers mit Azure-virtuellen Computern und zum Herunterladen, erstellen und installieren Sie die Apache Qpid Proton-C Library zusammen mit der Sprache Bindungen Python und PHP. Nachdem Sie diese Schritte ausgeführt haben, werden Sie die in diesem Handbuch enthaltenen Python und PHP-Beispiele ausführen zu können.

Dieser erste Schritt erfolgt über das [Azure klassischen Portal][]. Der folgende Screenshot zeigt an, wie ein CentOS virtueller Computer mit dem Namen "Stefan-Centos" erstellt wurde:

![Proton einer Azure Linux virtuellen Computers][0]

Nachdem bereitgestellt, haben zeigt das Portal Folgendes Ergebnis an:

![Proton einer Azure Linux virtuellen Computers][1]

Müssen auf dem Computer anzumelden, müssen Sie den Endpunktport für SSH kennen. Sie können diesen Wert aus dem [Azure klassischen Portal][] , indem Sie den neu erstellten virtuellen Computer und durch Klicken auf die Registerkarte **Endpunkte** abrufen. Die folgende Abbildung zeigt, dass der öffentliche SSH Port für diesen Computer 57146 ist.

![Proton einer Azure Linux virtuellen Computers][2]

Sie können jetzt auf den Computer mithilfe von SSH verbinden. In diesem Beispiel wird das PuTTY Tool, wie im folgenden Screenshot:

![Proton einer Azure Linux virtuellen Computers][3]

Für die apps Python und PHP verwendet dieses Beispiel die Proton Client-Bibliotheken von Apache. Diese Bibliotheken sind auf [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)zum Download verfügbar. Die Infodatei in das Verteilungspaket wird erläutert, die erforderlichen Schritte zum Installieren der Abhängigkeiten und Proton erstellen. Hier ist eine Zusammenfassung der Schritte aus:

1.  Bearbeiten die Konfigurationsdatei Yum (/ etc/yum.conf) und Kommentieren der Ausschluss nach Updates suchen auf Kernel Header (\# ausschließen Kernel =\*). Dies ist erforderlich, vor den Compiler Gcc installieren.

2.  Installieren Sie die erforderlichen Pakete an:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Herunterladen der Bibliothek Proton an:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Extrahieren Sie den Code Proton aus dem Archiv Verteilung an:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Erstellen Sie und installieren Sie den Code, indem die folgenden Schritte aus, die Infodatei entnommen:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Nachdem Sie diese Schritte ausgeführt haben, wird Proton auf dem Computer installiert sein und zur Verwendung bereit.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP (Übersicht)][]

[Service Bus AMQP (Übersicht)]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure klassischen-portal]: http://manage.windowsazure.com


