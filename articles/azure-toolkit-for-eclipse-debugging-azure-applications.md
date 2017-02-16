<properties
    pageTitle="Für das Debuggen Azure Applications in "Ellipse""
    description="Informationen Sie zu den für das Debuggen Azure Applications mit dem Azure-Toolkit für "Ellipse"."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Für das Debuggen Azure Applications in "Ellipse" #

Verwenden der Azure-Toolkit für "Ellipse", können Sie Ihre Programme debuggen, ob sie in Azure oder in der Serveremulator ausgeführt werden, wenn Sie als Betriebssystem Windows verwenden. Die folgende Abbildung zeigt die Dialogfeld dazu verwendet, aktivieren Sie das Debuggen Remoteprozeduraufruf **Debuggen** Eigenschaften:

![][ic719504]

In diesem Lernprogramm wird davon ausgegangen, dass Sie bereits über eine Anwendung verfügen, die wurde erfolgreich erstellt, und der Serveremulator vertraut und Bereitstellen von in Azure sind.

Wir verwenden die Anwendung von des Lernprogramms [mithilfe der Azure Service Runtime-Bibliothek in JSP][] als Ausgangspunkt für dieses Thema. Erstellen Sie bevor Sie fortfahren die Anwendung, wenn Sie dies noch nicht getan haben.

## <a name="to-debug-your-application-while-running-in-azure"></a>Die Anwendung während der Ausführung in Azure Debuggen ##

>[AZURE.WARNING] Das Toolkit des aktuellen Unterstützung für das Debuggen von remote Java hauptsächlich in Azure-Serveremulator ausgeführt Bereitstellungen gelten. Da die Debuggen Verbindung nicht sicher ist, sollten Sie die remote Debuggen in Herstellung Bereitstellungen nicht aktivieren. Wenn Sie in der Cloud Azure ausgeführte Anwendung debuggen müssen, verwenden Sie eine staging Bereitstellung, aber bewusst, dass nicht autorisierter Zugriff auf Ihre Sitzung Debuggen möglich, wenn jemand die IP-Adresse der Cloudbereitstellung, weiß, ist selbst wenn sie eine Bereitstellung von staging ist.

1. Erstellen Sie Ihr Projekt im Emulator Testzwecken: In "Ellipse" des Projekt-Explorer mit der rechten Maustaste **MyAzureProject**, klicken Sie auf **Eigenschaften**, klicken Sie auf **Azure**und mit **in die cloud-Bereitstellung**festlegen **für erstellen** .
1. Erstellen Sie Ihr Projekt neu: Wählen Sie im Menü "Ellipse" klicken Sie auf **Projekt**, und klicken Sie auf **Alle erstellen**.
1. Bereitstellen Sie die Anwendung auf *das staging* in Azure.
    >[AZURE.IMPORTANT] Wie zuvor erwähnt, wird empfohlen, dass Sie in der Serveremulator in den meisten Fällen debuggen und dann in das staging-Umgebung debuggen, nur, wenn Sie weitere für das Debuggen erforderlich ist. Es wird empfohlen, nicht in dieser Umgebung debuggen.
1. Wenn Ihre Bereitstellung in Azure bereit ist, erhalten Sie den DNS-Namen für die Bereitstellung aus dem [Azure-Verwaltungsportal][]. Eine Bereitstellung von staging hat einen DNS-Namen in Form von http://*&lt;Guid&gt;*. cloudapp.net, wo * &lt;Guid&gt; * ein GUID-Wert von Azure zugewiesen ist.
1. In den "Ellipse" Projekt-Explorer mit der rechten Maustaste **WorkerRole1**, **Azure**klicken Sie auf, und klicken Sie dann auf **Debuggen**.
1. Im Dialogfeld **Eigenschaften für das Debuggen von WorkerRole1** :
    1. Aktivieren Sie **remote-Debuggen für diese Rolle aktiviert.**
    1. Verwenden Sie für **Eingabesprache Endpunkt verwendet** **Debuggen (öffentlich: 8090, "Privat": 8090)**ein.
    1. Sicherstellen Sie, dass **Starten JVM im angehaltenen Modus, warten auf einer Debuggerverbindung** nicht aktiviert ist.
        >[AZURE.IMPORTANT] Die Option **Starten JVM im angehaltenen Modus, warten auf einer Debuggerverbindung** ist für erweiterte Szenarien in der Serveremulator nur Debuggen vorgesehen (nicht für Cloud-Bereitstellungen). Wenn die Option **Starten JVM im angehaltenen Modus, warten auf einer Debuggerverbindung** verwendet wird, wird es beim Start-Prozess des Servers anzuhalten, bis der Ellipse Debugger mit der JVM verbunden ist. Während Sie diese Option für eine Debuggen Sitzung mithilfe der Serveremulator verwenden können, verwenden sie nicht für eine Debuggen Sitzung in einen Cloudbereitstellung. Des Servers Initialisierung wird bei einem Vorgang Azure Start, und Azure-Cloud keine öffentlichen Endpunkte verfügbar bis der Start-Vorgang abgeschlossen ist. Daher wird ein Startprozess nicht erfolgreich abgeschlossen, wenn diese Option in einen Cloudbereitstellung aktiviert ist, da es keine Verbindung aus einem externen Ellipse Client empfangen werden kann.
1. Klicken Sie auf **Debuggen Konfigurationen erstellen**.
1. Im Dialogfeld **Azure Debuggen Konfiguration** :
    1. Wählen Sie für **Java-Projekts Debuggen** **MyHelloWorld** Projekt aus.
    1. Aktivieren Sie für das **Konfigurieren für das Debuggen für** **Azure Cloud (Bereitstellung)**ein.
    1. Stellen Sie sicher, dass **Azure berechnen Emulator** nicht aktiviert ist.
    1. Geben Sie für **Host**den DNS-Namen, der Ihre gestaffelter Bereitstellung, aber ohne die vorherige **http://**ein. Beispielsweise (verwenden die GUID anstelle der hier gezeigten GUID): **4e616d65-6f6e-6 d 65-6973-526f62657274.cloudapp.net**
1. Klicken Sie auf **OK** , um das Dialogfeld **Konfiguration von Azure Debuggen** zu schließen.
1. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften für das Debuggen WorkerRole1** zu schließen.
1. Wenn Sie eine fortzuschreiten bereits in index.jsp eingerichtet haben, legen Sie es:
    1. Innerhalb des "Ellipse" Projekt-Explorer erweitern Sie **MyHelloWorld** **Inhaltsordner**, und doppelklicken Sie auf **index.jsp**.
    1. In index.jsp mit der rechten Maustaste in der blauen Leiste links neben Ihren Java-Code, und klicken Sie auf **Den Schalter Haltepunkte**, wie im folgenden dargestellt: ![][ic551537]
1. Innerhalb des Ellipse Menüs klicken Sie auf **Ausführen** , und klicken Sie dann auf **Debuggen Konfigurationen**.
1. Klicken Sie im Dialogfeld **Debuggen Konfigurationen** erweitern Sie im linken Bereich **Remote Java-Anwendung** , wählen Sie **Azure Cloud (WorkerRole1)**, und klicken Sie auf **Debuggen**.
1. Führen Sie in Ihrem Browser Anwendung nach die Staging, **http://***&lt;Guid&gt;***.cloudapp.net/MyHelloWorld**, ersetzen die GUID aus Ihren DNS-Namen für * &lt;Guid&gt;*. Wenn Sie dazu aufgefordert werden, von einem Dialogfeld **Bestätigen Perspektive wechseln** , klicken Sie auf **Ja**. Ihre Sitzung Debuggen soll jetzt in die Zeile des Codes ausgeführt werden, in dem die fortzuschreiten festgelegt wurde.

>[AZURE.NOTE] Wenn Sie versuchen, einen Remote gestartet wird für das Debuggen Verbindung mit einer Bereitstellung mit mehreren Rolleninstanzen ausgeführt, können nicht Sie welche Instanz Steuerelement derzeit Debugger Anfangs, verbunden wie Azure Lastenausgleich eine Instanz zufällig auswählen wird. Nachdem Sie mit dieser Instanz verbunden sind, werden Sie jedoch das Debuggen der gleichen Instanz fortsetzen. Beachten Sie auch, ist eine Zeitspanne von mehr als 4 Minuten (beispielsweise, wenn Sie an einer fortzuschreiten zu lange angehalten sind), Azure möglicherweise die Verbindung schließen.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Eine Instanz der jeweiligen Rolle in einer Bereitstellung mit mehreren Instanzen Debuggen ##

Wenn Ihre Bereitstellung in der Cloud ausgeführt wird, werden Sie wahrscheinlich er ausgeführt werden in mehreren berechnen, oder die Rolle, die Instanzen. Dadurch können Sie Ihrer Anwendung zu skalieren und Azure 99,95 % Verfügbarkeit Garantie nutzen.

In solchen Szenarien müssen Sie Ihre Java-Anwendung in einer bestimmten Rolleninstanz Remote Debuggen. Jedoch, wenn Sie nur einen normalen Eingabewerte Endpunkt für das Debuggen aktivieren, werden Azure Lastenausgleich praktisch unmöglich, eine Verbindung mit einer bestimmten Rolleninstanz den Debugger erleichtern. Stattdessen werden sie mit einer Instanz der Rolle verbunden, die Größe zufällig ausgewählt.

Dies ist der Typ von Szenario, in dem nutzen Instanz von Endpunkten für eine bestimmte Rolleninstanz Debuggen erleichtern wird.

Angenommen, Sie bis zu 5 Rolleninstanzen der Bereitstellung ausführen möchten. Verwenden die **Endpunkte** Eigenschaftenseite im Dialogfeld Eigenschaften Rolle aus, erstellen Sie einen Instanz von Endpunkt und weisen sie einen Zellbereich öffentliche Ports, statt einer einzelnen Ports Zahl. Geben Sie beispielsweise im Eingabefeld **öffentlichen Port** **81-85**ein.

Nach der Bereitstellung Ihrer Anwendungs mit diesem Instanzendpunkt werden Azure eindeutige Port-Nummer aus diesem Bereich jede der Rolleninstanzen zuweisen. Klicken Sie dann, um herauszufinden, welche Instanz, welche Port-Nummer zugewiesen haben, können, verwenden Sie die *InstanceEndpointName***_PUBLICPORT** Umgebungsvariable (wobei *InstanceEndpointName* den Namen, die Ihnen zugewiesen werden, wenn Sie den Instanzendpunkt erstellt haben) vom Toolkit in der Bereitstellung (z. B., indem Sie dessen Wert in der Fußzeile auf einer Webseite zurückgegeben wird, damit Sie gelesen werden konnten, wenn Sie damit durchsuchen) automatisch konfiguriert.

Wenn Sie wissen, welche öffentlichen Port-Nummer die betreffende Instanz zugewiesen wurde, auftritt, indem sie auf den Hostnamen des Diensts in der Konfiguration Debuggen in Ellipse, verweisen. Dadurch wird die Ellipse Remotedebuggers die jeweilige Instanz und nicht auf eine der anderen Instanzen aktiviert.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Nur Windows: die Anwendung während der Ausführung in der Serveremulator Debuggen ##

>[AZURE.NOTE] Der Azure Emulator ist nur unter Windows verfügbar. Überspringen Sie diesen Abschnitt, wenn Sie ein Betriebssystem als Windows verwenden. 

1. Erstellen Sie Ihr Projekt im Emulator Testzwecken: In "Ellipse" des Projekt-Explorer mit der rechten Maustaste **MyAzureProject**, klicken Sie auf **Eigenschaften**, klicken Sie auf **Azure**und zum **Testen im Emulator**festlegen **für erstellen** .
1. Erstellen Sie Ihr Projekt neu: Wählen Sie im Menü "Ellipse" klicken Sie auf **Projekt**, und klicken Sie auf **Alle erstellen**.
1. In den "Ellipse" Projekt-Explorer mit der rechten Maustaste **WorkerRole1**, **Azure**klicken Sie auf, und klicken Sie dann auf **Debuggen**.
1. Im Dialogfeld **Eigenschaften für das Debuggen von WorkerRole1** :
    1. Aktivieren Sie **remote-Debuggen für diese Rolle aktiviert.**
    1. Verwenden Sie für **Eingabe Endpunkt verwendet**den Standardendpunkt automatisch generiert vom Toolkit, als **Debuggen (öffentlich: 8090, "Privat": 8090)**aufgelistet.
    1. Stellen Sie sicher, dass die Option **Starten JVM im angehaltenen Modus, warten auf einer Debuggerverbindung** deaktiviert ist.
        >[AZURE.IMPORTANT] Die Option **Starten JVM im angehaltenen Modus, warten auf einer Debuggerverbindung** ist für erweiterte Szenarien in der Serveremulator nur Debuggen vorgesehen (nicht für Cloud-Bereitstellungen). Wenn die Option **Starten JVM im angehaltenen Modus, warten auf einer Debuggerverbindung** verwendet wird, wird es beim Start-Prozess des Servers anzuhalten, bis der Ellipse Debugger mit der JVM verbunden ist. Während Sie diese Option für eine Debuggen Sitzung mithilfe der Serveremulator verwenden können, verwenden sie nicht für eine Debuggen Sitzung in einen Cloudbereitstellung. Des Servers Initialisierung wird bei einem Vorgang Azure Start, und Azure-Cloud keine öffentlichen Endpunkte verfügbar bis der Start-Vorgang abgeschlossen ist. Daher wird ein Startprozess nicht erfolgreich abgeschlossen, wenn diese Option in einen Cloudbereitstellung aktiviert ist, da es keine Verbindung aus einem externen Ellipse Client empfangen werden kann.
1. Klicken Sie auf **Debuggen Konfigurationen erstellen**.
1. Im Dialogfeld **Azure Debuggen Konfiguration** :
    1. Wählen Sie für **Java-Projekts Debuggen** **MyHelloWorld** Projekt aus.
    1. Aktivieren Sie für das **Konfigurieren für das Debuggen für** **Azure Emulator zu berechnen**.
1. Klicken Sie auf **OK** , um das Dialogfeld **Konfiguration von Azure Debuggen** zu schließen.
1. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften für das Debuggen WorkerRole1** zu schließen.
1. Festlegen einer fortzuschreiten in index.jsp:
    1. Innerhalb des "Ellipse" Projekt-Explorer erweitern Sie **MyHelloWorld** **Inhaltsordner**, und doppelklicken Sie auf **index.jsp**.
    1. In index.jsp mit der rechten Maustaste in der blauen Leiste links neben Ihren Java-Code, und klicken Sie auf **Den Schalter Haltepunkte**, wie im folgenden dargestellt: ![][ic551537]

       Eine fortzuschreiten wird festgelegt, wenn Sie ein Symbol fortzuschreiten innerhalb der blauen Leiste links neben Ihren Java-Code angezeigt.
1. Starten Sie die Anwendung in der Serveremulator an, indem Sie auf die Schaltfläche **in Azure Emulator ausführen** , klicken Sie auf der Symbolleiste Azure.
1. Innerhalb des Ellipse Menüs klicken Sie auf **Ausführen** , und klicken Sie dann auf **Debuggen Konfigurationen**.
1. Klicken Sie im Dialogfeld **Debuggen Konfigurationen** erweitern Sie im linken Bereich **Remote Java-Anwendung** , wählen Sie **Azure Emulator (WorkerRole1 aus)**, und klicken Sie auf **Debuggen**.
1. Nachdem der Serveremulator gibt an, dass die Anwendung in Ihrem Browser ausführen **Http://localhost: 8080/MyHelloWorld**ausgeführt wird.
    Wenn Sie dazu aufgefordert werden, von einem Dialogfeld **Bestätigen Perspektive wechseln** , klicken Sie auf **Ja**.
    Ihre Sitzung Debuggen soll jetzt in die Zeile des Codes ausgeführt werden, in dem die fortzuschreiten festgelegt wurde.

Dies wurde gezeigt, wie in der Serveremulator Debuggen; Im nächste Abschnitt wird gezeigt, wie Debuggen eine Anwendung in Azure bereitgestellt werden kann.

## <a name="debugging-notes"></a>Für das Debuggen von Notizen ##

* Nach dem Debuggen, können Sie die Perspektive aus **Debuggen** **Java** wechseln, über die auf die Ellipse-Menü, klicken **im Fenster** **Öffnen Perspektive**, und wählen die Perspektive, die Sie verwenden möchten.
* Remote Debuggen in GlassFish aktivieren, verwenden Sie nicht das remote Debuggen Konfiguration Feature des Azure-Toolkits für "Ellipse". Stattdessen konfigurieren Sie GlassFish manuell. Aufgrund der Art und Weise, die glassfish Java-Optionen in der Umgebungsvariablen vordefinierter behandelt, funktioniert die-Toolkits remote Debuggen Konfigurations-Funktion nicht ordnungsgemäß mit GlassFish. Wenn das Toolkit des remote Debuggen Konfigurations-Feature aktiviert ist, kann nicht bei jeder GlassFish ordnungsgemäß.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für "Ellipse"][]

[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"][]

[Installieren des Azure-Toolkits für "Ellipse"][] 

Weitere Informationen zur Verwendung von Azure mit Java finden Sie im [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java-Entwicklercenter]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Verwaltungsportal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure-Toolkit für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen einer Hallo Welt Anwendungs Azure in "Ellipse"]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installieren des Azure-Toolkits für "Ellipse"]: http://go.microsoft.com/fwlink/?LinkId=699546
[Verwenden der Azure Service Runtime-Bibliothek in JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
