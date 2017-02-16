<properties
    pageTitle="Testen Sie die Anwendung mithilfe von Visual Studio Team Services laden | Microsoft Azure"
    description="Erfahren Sie, wie Test hervor Ihrer Azure Service Fabric Applikationen mithilfe von Visual Studio Team Services."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Testen Sie die Anwendung mithilfe von Visual Studio Team Services laden

In diesem Artikel veranschaulicht, wie Microsoft Visual Studio laden Testen der Funktionen um zu betonen testen eine Anwendung. Ein Azure Service Fabric dynamische Dienst Back-End und ein Web-front-End statusfreie Dienst verwendet. Die hier verwendete Beispiel-Anwendung ist ein Flugzeug Speicherort Simulator. Sie bieten eine Flugzeug-ID, Flüge und Ziel ein. Back-End der Anwendung verarbeitet die Anfragen und front-End-zeigt auf einer Karte das Flugzeug, das die Kriterien entspricht.

Das folgende Diagramm veranschaulicht die Fabric Service-Anwendung, die Sie testen werden können.

![Diagramm der Anwendung Beispiel Flugzeug Speicherort][0]

## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie beginnen, müssen Sie die folgenden Aktionen ausführen:

- Erhalten eines Visual Studio Team Services-Kontos an. Sie können in [Visual Studio Team Services](https://www.visualstudio.com)kostenlos erhalten.
- Abrufen Sie und installieren Sie Visual Studio 2013 oder Visual Studio 2015. In diesem Artikel wird Visual Studio 2015 Enterprise Edition verwendet, aber Visual Studio 2013 und anderen Editionen funktionieren sollte ähnlich wie.
- Bereitstellen Sie die Anwendung in eine staging-Umgebung. Informationen hierzu finden Sie unter [Gewusst wie: Bereitstellen von Applications zu einem remote Cluster mit Visual Studio](service-fabric-publish-app-remote-cluster.md) .
- Grundlegendes zu Ihrer Anwendung Verwendungsmuster aus. Diese Informationen werden verwendet, um das Laden Muster zu reproduzieren.
- Verstehen Sie das Ziel für das Laden testen. Auf diese Weise können Sie bei der Interpretation und Analysieren der Testergebnisse laden.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Erstellen Sie und führen Sie der Project Web Leistung und Auslastung testen aus

### <a name="create-a-web-performance-and-load-test-project"></a>Erstellen eines Projekts Web Leistung und Auslastung testen

1. Öffnen Sie Visual Studio 2015. Wählen Sie die **Datei** > **neu** > **Projekt** in der Menüleiste im Dialogfeld **Neues Projekt** zu öffnen.

2. Erweitern Sie den **Visual C#-** Knoten, und wählen Sie **Test** > **Web Leistung und Auslastungstest Project**. Geben Sie dem Projekt einen Namen, und wählen Sie dann auf die Schaltfläche **OK** .

    ![Screenshot der im Dialogfeld Neues Projekt][1]

    Es sollte ein neues Web Leistung und Auslastungstest Projekt im Solution Explorer angezeigt.

    ![Screenshot der Lösung Explorer mit dem neuen Projekt][2]

### <a name="record-a-web-performance-test"></a>Aufzeichnen eines Web Performance Tests

1. Öffnen Sie das Projekt .webtest.

2. Wählen Sie das Symbol **Aufzeichnung hinzufügen** , um eine Aufzeichnung Sitzung in Ihrem Browser zu starten.

    ![Screenshot des Symbols Aufzeichnung hinzufügen im browser][3]

    ![Screenshot der Schaltfläche Datensatz in einem browser][4]

3. Navigieren Sie zu der Fabric Service-Anwendung. Klicken Sie im Bereich Aufzeichnung sollte die Webanfragen anzeigen.

    ![Screenshot der Webanfragen im Bereich "Aufzeichnen"][5]

4. Führen Sie eine Abfolge von Aktionen, die Sie erwarten, die Benutzer dass die durchführen. Diese Aktionen werden als ein Muster verwendet, die Auslastung generieren.

5. Wenn Sie fertig sind, wählen Sie die Schaltfläche **Beenden** , um die Aufzeichnung zu beenden.

    ![Screenshot der Schaltfläche "Beenden"][6]

    Das Projekt .webtest in Visual Studio sollte eine Reihe von Besprechungsanfragen erfasst haben. Dynamische Parameter werden automatisch ersetzt. An diesem Punkt können Sie keine zusätzliche, wiederholten Abhängigkeit Anfragen löschen, die nicht Bestandteil von Ihrem Testszenario sind.

6. Speichern Sie das Projekt, und wählen Sie dann auf den Befehl **Ausführen testen** den Web Performance Test lokal ausführen, und stellen Sie sicher, dass alles ordnungsgemäß funktioniert.

    ![Screenshot des Befehls Test ausführen][7]

### <a name="parameterize-the-web-performance-test"></a>Parametrisieren des Web Performance Tests

Sie können den Web Performance Test parametrisieren, indem Sie ihn in einen codierten Webleistungstest konvertieren, und klicken Sie dann Bearbeiten des Codes. Als Alternative können Sie eine Datenliste im Web Performance Test binden, damit die Daten der Test durchläuft. Ausführliche Informationen zum Konvertieren des Web Performance Tests zu einem Test der codierten finden Sie unter [generieren, und führen Sie einen codierten Web Performance Test](https://msdn.microsoft.com/library/ms182552.aspx) . Informationen zum Binden von Daten an einen Web Performance Test finden Sie unter [Hinzufügen einer Datenquelle zu einer Web Performance testen](https://msdn.microsoft.com/library/ms243142.aspx) .

In diesem Beispiel werden wir einen codierten Test den Web Performance Test konvertieren, sodass die ID Flugzeug mit einer generierten GUID ersetzen und mehr Anfragen Flüge an unterschiedlichen Standorten senden hinzugefügt werden können.

### <a name="create-a-load-test-project"></a>Erstellen Sie ein Projekt laden

Ein oder mehrere Szenarien, die von der Web Performance Test und Einheit testen, zusammen mit zusätzlichen angegebenen laden Test Einstellungen beschriebenen besteht ein Projekt laden. Die folgenden Schritte zeigen, wie ein Laden Test-Projekt zu erstellen:

1. Wählen Sie im Kontextmenü des Projekts Web Leistung und Auslastungstest **Hinzufügen** > **Laden testen**. Wählen Sie im Assistenten **Laden testen** die Schaltfläche **Weiter** so konfigurieren Sie die Test-Einstellungen aus.

2. Wählen Sie im Abschnitt **Muster laden** , ob eine Konstante Benutzerlast oder eine schrittweise Auslastung, beginnt mit wenigen Benutzern und der Benutzer über einen Zeitraum erhöht werden soll.

    Wenn Sie eine gute Schätzung der Höhe der Benutzer laden haben und wie das aktuelle System führt anzeigen möchten, wählen Sie die **Konstante zu laden**. Wenn Ihr Ziel ist es, wenn Sie wissen, ob das System durchgängig bei verschiedenen Auslastung durchführt, wählen Sie **Schrittweise Auslastung**aus.

3. Klicken Sie im Abschnitt **Mischung testen** wählen Sie die Schaltfläche **Hinzufügen** , und wählen Sie dann auf Tests, der in der Test enthalten sein sollen. Sie können die Spalte **Verteilung** verwenden, um den Prozentsatz der Tests insgesamt für jeden Test ausgeführt anzugeben.

4. Geben Sie im Abschnitt **Einstellungen ausführen** die Dauer des Tests an.

    >[AZURE.NOTE] Die Option **Testiterationen** steht nur bei der Ausführung eines laden Tests lokal mit Visual Studio.

5. Geben Sie den Speicherort, an dem Laden Test Anfragen generiert werden, im Abschnitt **Speicherort** **Ausführen**Einstellungen an. Der Assistent fordert Sie eventuell auf Melden Sie sich bei Ihrem Team Services-Konto. Melden Sie sich an, und wählen Sie dann eine geographischen Standort. Wenn Sie fertig sind, wählen Sie die Schaltfläche **Fertig stellen** .

6. Nachdem der Test erstellt wurde, öffnen Sie das Projekt .loadtest, und wählen Sie die aktuelle Einstellung, z. B. **Run Settings**ausführen > **Settings1 ausführen [aktiv]**. Dadurch wird die ausführen Einstellungen im Fenster **Eigenschaften** geöffnet.

7. Im Abschnitt **Ergebnisse** des Fensters Eigenschaften **Einstellungen ausführen** sollte die Einstellung **Speicher für Details der Anzeigedauer** **keinen** als den Standardwert ist. Ändern Sie diesen Wert auf **Alle einzelnen Details** , um weitere Informationen über das Laden Testergebnisse abgerufen. Weitere Informationen zum Verbinden mit Visual Studio Team Services, und führen Sie einen Test Laden finden Sie unter [Testen zu laden](https://www.visualstudio.com/load-testing.aspx) .

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Ausführen des Tests mit Visual Studio Team Services

Auswählen des Befehls **Laden Test ausführen** , um den Testlauf zu starten

![Screenshot des Befehls laden Test ausführen][8]

## <a name="view-and-analyze-the-load-test-results"></a>Zeigen Sie an und analysieren Sie der Testergebnisse laden

Testen als die laden fortschreitet, die Leistungsinformationen im Diagramm angezeigt wird. Das folgende Diagramm ungefähr sollte angezeigt werden.

![Screenshot der Leistung Graph für Testergebnisse laden][9]

1. Wählen Sie den Link **herunterladen Bericht** am oberen Rand der Seite aus. Nachdem der Bericht heruntergeladen wurde, wählen Sie die Schaltfläche **Bericht** aus.

    Klicken Sie auf der Registerkarte **Grafik** können Sie Diagramme für verschiedene Performance-Zähler anzeigen. Klicken Sie auf der Registerkarte **Zusammenfassung** angezeigt werden die Gesamtergebnisse testen. Die Registerkarte " **Tabellen** " zeigt die Gesamtzahl der übergebene und Fehler beim Laden überprüft.

2. Wählen Sie die Zahl Links der **Test** > **fehlgeschlagen** und der **Fehler** > **Anzahl** Spalten, um Fehlerdetails anzuzeigen.

    Auf die Registerkarte **Details** zeigt virtuelle Benutzer und Test Szenario für fehlgeschlagene Anfragen. Diese Daten ist nützlich, wenn der Test Szenarien mit mehreren enthält.

Weitere Informationen zum Anzeigen der Ergebnisse eines finden Sie unter [Analysieren laden Testergebnisse in der Diagrammansicht der Auslastungstestanalyse](https://www.visualstudio.com/load-testing.aspx) .

## <a name="automate-your-load-test"></a>Automatisieren von den test

Visual Studio Team Services laden Test stellt APIs zum Laden Tests verwalten und analysieren die Ergebnisse in ein Team Services-Konto an. Weitere Informationen finden Sie in der [Cloud laden testen Rest-APIs](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) .

## <a name="next-steps"></a>Nächste Schritte
- [Überwachung und Diagnose von Diensten in einer lokalen Computer Entwicklung einrichten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
