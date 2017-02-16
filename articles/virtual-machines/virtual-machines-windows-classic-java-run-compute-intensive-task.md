<properties
    pageTitle="Java-Anwendung eines virtuellen Computers rechenintensiven | Microsoft Azure"
    description="Erfahren Sie, wie eine Azure-virtuellen Computern erstellen, die eine rechenintensiven Java-Anwendung ausgeführt wird, die von einer anderen Java-Anwendung überwacht werden kann."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Wie Sie einen Vorgang berechnen ankommt Java auf einem virtuellen Computer ausgeführt werden

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Mit Azure können Sie einer virtuellen Computern rechenintensiven Aufgaben zu behandeln. Beispielsweise können ein virtuellen Computers behandeln Aufgaben und Vorführen Ergebnisse auf Clientcomputern oder Windows-Dienste. Nach dem Lesen dieses Artikels, müssen Sie einen Überblick über die zum Erstellen eines virtuellen Computers, das eine rechenintensiven Java-Anwendung ausgeführt wird, die von einer anderen Java-Anwendung überwacht werden kann.

In diesem Lernprogramm wird davon ausgegangen, Sie wissen, wie Sie Java-Konsole Applications erstellen, können Bibliotheken an Ihrer Anwendung Java importieren und können eine Java-Archiv (JAR) generieren. Es wird keine Kenntnis von Microsoft Azure angenommen.

Lernen Sie Folgendes:

* So erstellen Sie einen virtuellen Computer mit einer Java Development Kit (JDK) bereits installiert.
* Wie sich bei Ihrem virtuellen Computern remote anmelden.
* So erstellen Sie einen Service Bus Namespace.
* Informationen zum Erstellen von Java-Anwendung, die einen rechenintensiven Vorgang ausführt.
* Informationen zum Erstellen von Java-Anwendung, die den Fortschritt des Vorgangs rechenintensiven überwacht.
* So führen Sie die Java-Programme.
* So beenden Sie die Java-Programme.

In diesem Lernprogramm wird Handlungsreisendenproblems für den Vorgang berechnen ankommt verwendet werden. So sieht ein Beispiel für die Ausführung der Aufgabe berechnen ankommt Java-Anwendung.

![Handlungsreisendenproblems solver][solver_output]

So sieht ein Beispiel für die Überwachung des rechenintensiven Vorgangs Java-Anwendung.

![Handlungsreisendenproblems client][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Zum Erstellen eines virtuellen Computers

1. Melden Sie sich im [Azure klassischen Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **neu**, klicken Sie auf **zu berechnen**, klicken Sie auf **virtuellen Computern**, und klicken Sie dann auf **Aus Galerie**.
3. Wählen Sie im Dialogfeld **Wählen Sie Bild virtuellen Computern** **JDK 7, Windows Server 2012**.
Beachten Sie, dass **JDK 6 Windows Server 2012** verfügbar ist, falls Sie ältere Applikationen, die noch nicht bereit sind verfügen, führen Sie unter JDK 7 sind.
4. Klicken Sie auf **Weiter**.
4. Klicken Sie im Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Geben Sie einen Namen für den virtuellen Computer an.
    2. Geben Sie die Größe des virtuellen Computers verwenden.
    3. Geben Sie einen Namen für den Administrator im Feld **Benutzername** ein. Denken Sie daran, dieser Name und das Kennwort ein, das Sie als Nächstes eingeben möchten, verwenden Sie diese beim des virtuellen Computers Remotezugriff auf.
    4. Geben Sie ein Kennwort in das Feld **Neues Kennwort ein** , und geben Sie es erneut in das Feld **bestätigen** ein. Dies ist das Kontokennwort Administrator.
    5. Klicken Sie auf **Weiter**.
5. Im nächsten **Konfiguration des virtuellen Computers** Dialogfeld folgendermaßen vor:
    1. **Cloud-Dienst**verwenden Sie die Standardeinstellung **Erstellen einer neuen Cloud-Dienst**aus.
    2. Der Wert für **Cloud-Dienst DNS-Name** muss über cloudapp.net eindeutig sein. Falls erforderlich, ändern Sie diesen Wert, damit die Azure zeigt an, dass er eindeutig ist.
    2. Geben Sie eine Region, Zugehörigkeit Gruppe oder virtuelles Netzwerk. Geben Sie einen Bereich, z. B. **Westen US**Zwecken dieses Lernprogramms an.
    2. Wählen Sie für **Speicher-Konto** **Verwenden Sie ein automatisch generiertes Speicherkonto**ein.
    3. Wählen Sie zum **Festlegen der Verfügbarkeit** **(keine)**aus.
    4. Klicken Sie auf **Weiter**.
5. In der endgültigen klicken Sie im Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Akzeptieren Sie die standardmäßigen Endpunkt Einträge ein.
    2. Klicken Sie auf **abgeschlossen**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Remote an Ihre virtuellen Computern anmelden

1. Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **virtuellen Computern**.
3. Klicken Sie auf den Namen des virtuellen Computers, die Sie sich anmelden möchten.
4. Klicken Sie auf **Verbinden**.
5. Reagieren Sie auf den Anweisungen, je nach Bedarf Verbindung zum des virtuellen Computers. Wenn Sie für den Administratornamen und das Kennwort aufgefordert werden, verwenden Sie die Werte, die Ihnen bei der Erstellung des virtuellen Computers erteilt.

Beachten Sie, dass der Azure-Dienstbus erfordern des Zertifikats Baltimore CyberTrust Root als Teil Ihrer JREs **Cacerts** Store installiert werden. Dieses Zertifikat wird automatisch in die Java Runtime Umgebung (JRE) in diesem Lernprogramm verwendeten aufgenommen. Wenn Sie dieses Zertifikat nicht in Ihrer JRE **Cacerts** Store haben, finden Sie unter [Hinzufügen eines Zertifikats in Java CA Zertifikat gespeichert] [ add_ca_cert] Informationen zum Hinzufügen von es (sowie Informationen zum Anzeigen der Zertifikate in Ihrem Cacerts Store).

## <a name="how-to-create-a-service-bus-namespace"></a>So erstellen Sie einen Service Bus namespace

Um mit Servicebuswarteschlangen in Azure zu beginnen, müssen Sie zuerst einen Dienstnamespace erstellen. Ein Dienstnamespace stellt einen Bereiche Container zum Adressieren Dienstbus Ressourcen innerhalb Ihrer Anwendung bereit.

So erstellen Sie einen Namespace service

1.  Melden Sie sich bei der [Azure klassischen Portal](https://manage.windowsazure.com).
2.  Klicken Sie in der unteren linken Navigationsbereich des klassischen Azure Portals auf **Dienstbus, Access Control und Zwischenspeichern**.
3.  Klicken Sie auf den Knoten **Dienstbus** , und klicken Sie dann auf die Schaltfläche ' **neu** ', in die obere linke Bereich des Portals Azure klassischen.  
    ![Screenshot der Dienst Bus Knoten][svc_bus_node]
4.  Klicken Sie im Dialogfeld **Erstellen einer neuen Dienst Namespace** Geben Sie einen **Namespace**, und klicken Sie dann um sicherzustellen, dass er eindeutig ist, klicken Sie auf die Schaltfläche **Verfügbarkeit prüfen** .  
    ![Erstellen eines neuen Namespace Screenshots][create_namespace]
5.  Nachdem Sie den Namespace sichergestellt Name verfügbar ist, und wählen Sie das Land oder Region, in dem Ihre Namespace gehostet werden soll, und klicken Sie dann auf die Schaltfläche **Namespace erstellen** .  

    Den Namespace erstellten wird dann in der klassischen Azure-Portal angezeigt und dauert einen Moment zu aktivieren. Warten Sie, bis der Status **aktiv** ist, bevor Sie fortfahren mit dem nächsten Schritt fort.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Stellen Sie die Standard Management Anmeldeinformationen für den namespace

Um z. B. das Erstellen einer Warteschlange, klicken Sie auf der neuen Namespace-Management-Vorgänge ausführen müssen Sie die Verwaltung Anmeldeinformationen für den Namespace zu erhalten.

1.  Klicken Sie im linken Navigationsbereich auf den **Dienstbus** -Knoten, um die Liste der verfügbaren Namespaces anzuzeigen.
    ![Screenshot der verfügbaren Namespaces][avail_namespaces]
2.  Wählen Sie den soeben erstellten in der angezeigten Liste Namespace.
    ![Screenshot des Namespace-Liste][namespace_list]
3.  Im rechten Bereich der **Eigenschaften** Listet die Eigenschaften für den neuen Namespace.
    ![Screenshot der Eigenschaften-Bereich][properties_pane]
4.  Die **Taste Standard** wird ausgeblendet. Klicken Sie auf die Schaltfläche **Ansicht** , um die Sicherheitsanmeldeinformationen anzuzeigen.
    ![Screenshot der Standard-Taste][default_key]
5.  Notieren Sie **Standard Herausgeber** und die **Taste Standard** , wie Sie diese Informationen unten zum Ausführen von Vorgängen mit dem Namespace verwendet werden.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>So erstellen Sie eine Java-Anwendung, die eine Aufgabe berechnen ankommt ausführt

1. Auf Ihrem Entwicklungscomputer (die keinen enthält des virtuellen Computers sein, die Sie erstellt haben), laden Sie das [Azure SDK für Java](https://azure.microsoft.com/develop/java/).
2. Erstellen Sie eine Java mit den folgenden Code am Ende dieses Abschnitts. In diesem Lernprogramm wird als Dateinamen Java **TSPSolver.java** verwendet. Ändern der **Ihre\_Service\_Bus\_Namespace**, **Ihrer\_Service\_Bus\_Besitzer**, und **Ihrer\_Service\_Bus\_Schlüssel** Platzhalter mit Ihrem Dienst bus **Namespace**, **Standard Herausgeber** und **Standard** Schlüsselwerte.
3. Nach dem Schreiben von Code, die Anwendung in eine ausführbare Java-Archiv (JAR) exportieren, und die erforderlichen Bibliotheken in der generierten JAR packen. In diesem Lernprogramm werden wir **TSPSolver.jar** als generierte JAR-Namen verwenden.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>So erstellen Sie eine Java-Anwendung, die den Fortschritt des Vorgangs rechenintensiven überwacht

1. Erstellen Sie auf Ihrem Entwicklungscomputer einer Java Console-Anwendungs, die mit den folgenden Code am Ende dieses Abschnitts aus. In diesem Lernprogramm wird als Dateinamen Java **TSPClient.java** verwendet. Wie zuvor gezeigt wird, Ändern der **Ihre\_Service\_Bus\_Namespace**, **Ihrer\_Service\_Bus\_Besitzer**, und **Ihrer\_Service\_Bus\_Schlüssel** Platzhalter mit Ihrem Dienst bus **Namespace**, **Standard Herausgeber** und **Standard** Schlüsselwerte.
2. Exportieren der Anwendungs zu einem ausführbare Glas, und die erforderlichen Bibliotheken in der generierten JAR packen. In diesem Lernprogramm werden wir **TSPClient.jar** als generierte JAR-Namen verwenden.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>So führen Sie die Java-Anwendungen
Führen Sie die Anwendung berechnen ankommt zuerst in der Warteschlange zu erstellen, um das Problem unterwegs Saleseman lösen, die der aktuellen optimale weiterleiten an die Service Bus Warteschlange hinzufügen. Während die Anwendung berechnen ankommt läuft (oder später), wird den Client zum Anzeigen der Ergebnisse aus der Dienst Bus Warteschlange ausführen.

### <a name="to-run-the-compute-intensive-application"></a>Zum Ausführen der Anwendung berechnen ankommt

1. Melden Sie sich bei Ihrem virtuellen Computern an.
2. Erstellen Sie einen Ordner, in dem Sie die Anwendung ausführen möchten. Beispielsweise **c:\TSP**.
3. Kopieren Sie **TSPSolver.jar** in **c:\TSP**,
4. Erstellen Sie eine Datei namens **c:\TSP\cities.txt** mit dem folgenden Inhalt.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. Über die Befehlszeile wechseln Sie zum c:\TSP.
6. Stellen Sie sicher, dass JREs Papierkorb Ordner in der Variable PATH-Umgebung befindet.
7. Sie müssen die Service Bus Warteschlange vor dem Ausführen der TSP Solver Permutationen erstellen. Führen Sie zum Erstellen der Service Bus Warteschlange den folgenden Befehl ein.

        java -jar TSPSolver.jar createqueue

8. Nachdem Sie nun die Warteschlange erstellt wurde, können Sie die TSP Solver Permutationen ausführen. Führen Sie beispielsweise den folgenden Befehl zum Ausführen des Solver für 8 Orte aus.

        java -jar TSPSolver.jar 8

 Wenn Sie eine Zahl nicht angeben, wird es für 10 Orte ausgeführt. Wie das Solver aktuellen geschätzte leitet findet, werden sie der Warteschlange hinzugefügt.

> [AZURE.NOTE]
> Je größer die Zahl, die Sie, die mehr angeben das Solver wird ausgeführt. Beispielsweise ausgeführt für 14 Orte können einige Minuten dauern, und die Ausführung für 15 Orte mehrere Stunden dauern. Zunehmender zu 16 oder mehr Orten ergeben in Tagen Laufzeit (Gelegenheit Wochen, Monaten und Jahren). Dies wird durch die Anzahl der Permutationen von Solver ausgewertet wird, als die Anzahl der Orte erhöht die schnelle erhöhen.

### <a name="how-to-run-the-monitoring-client-application"></a>So führen Sie die Überwachung Clientanwendung
1. Melden Sie sich auf Ihrem Computer, auf dem Sie die Clientanwendung ausgeführt werden. Dies muss nicht auf dem gleichen Computer ausführen der Anwendung **TSPSolver** befinden, aber sie können.
2. Erstellen Sie einen Ordner, in dem Sie die Anwendung ausführen möchten. Beispielsweise **c:\TSP**.
3. Kopieren Sie **TSPClient.jar** in **c:\TSP**,
4. Stellen Sie sicher, dass JREs Papierkorb Ordner in der Variable PATH-Umgebung befindet.
5. Über die Befehlszeile wechseln Sie zum c:\TSP.
6. Führen Sie den folgenden Befehl ein.

        java -jar TSPClient.jar

    Geben Sie optional die Anzahl der Minuten zwischen die Warteschlange durch die Übergabe ein Argument Befehlszeile Auschecken deaktiviert. Die Dauer der standardmäßigen Ruhezeit zum Überprüfen der Warteschlange ist 3 Minuten, der verwendet wird, wenn kein Argument Befehlszeile an **TSPClient**übergeben wird. Wenn Sie einen anderen Wert für das Ruheintervall verwenden möchten, beispielsweise führen Sie eine Minute, den folgenden Befehl aus.

        java -jar TSPClient.jar 1

    Der Client wird ausgeführt, bis es erkennt, dass eine Nachricht in Warteschlange Einfügen von "Abgeschlossen". Beachten Sie, wenn Sie mehrere Vorkommen des Solver ausführen, ohne den Client ausführen, Sie möglicherweise dem Client mehrmals in der Warteschlange vollständig leeren ausführen müssen. Alternativ können Sie die Warteschlange löschen und erstellen Sie es dann erneut. Um die Warteschlange zu löschen, führen Sie den folgenden Befehl aus **TSPSolver** (nicht **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    Das Solver wird ausgeführt, bis alle Arbeitspläne untersuchen abgeschlossen ist.

## <a name="how-to-stop-the-java-applications"></a>So beenden Sie die Java-Clientanwendungen
Für die Solver und Clientanwendungen drücken Sie **STRG + C** , um beenden, wenn Sie vor dem normalen Abschluss beenden möchten.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
