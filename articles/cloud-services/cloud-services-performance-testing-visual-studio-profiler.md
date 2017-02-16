<properties 
    pageTitle="Profil Erstellen eines Cloud-Diensts lokal in der Serveremulator | Microsoft Azure" 
    services="cloud-services"
    description="Ermitteln Sie mit dem Visual Studio Profiler Leistungsprobleme Cloud-Dienste" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Testen die Leistung von einem Cloud-Dienst lokal in der Azure-Serveremulator mit dem Visual Studio Profiler

Eine Vielzahl von Tools und Techniken sind zum Testen der Leistung von Cloud-Dienste zur Verfügung.
Wenn Sie einen Clouddienst Azure veröffentlichen, können Sie Visual Studio Profilerstellungsdaten sammeln und dann als beschrieben im [Profil einer Azure-Anwendung]lokal, analysieren haben[1].
Sie können auch Diagnose zum Nachverfolgen von einer Vielzahl von Leistungsindikatoren verwenden, in [verwenden Leistungsindikatoren in Azure]beschriebenen[2].
Sie möchten möglicherweise auch die Anwendung lokal in der Serveremulator vor der Bereitstellung in der Cloud analysiert.

Dieser Artikel behandelt die CPU werden-Methode der Profil erstellen, die lokal im Emulator ausgeführt werden kann. CPU-Stichproben ist, dass eine Methode zum Profil erstellen, die nicht sehr Einfluss ist. In einem Intervall vorgesehenen werden wird der Profiler eine Momentaufnahme des Stapels Anruf. Die Daten über einen Zeitraum erfasst, und in einem Bericht angezeigt. Dieses Verfahren zum Profil erstellen in der Regel um anzugeben, wo in eine rechenintensive Anwendung die meisten CPU-Aufgaben, vorgenommen wird.  Dies bietet Ihnen die Möglichkeit, auf der "langsamste Pfad" konzentrieren, wo eine Anwendung die meiste Zeit Ausgaben.



## <a name="1-configure-visual-studio-for-profiling"></a>1: Konfigurieren von Visual Studio für ein Profil erstellen

Es gibt zunächst einige Visual Studio-Konfigurations-Optionen, die möglicherweise hilfreich, wenn Sie ein Profil erstellen. Wenn Sie einen Einblick in die Profilerstellungsberichte erhalten möchten, benötigen Sie für die Anwendung und auch die Symbole für Systembibliotheken Symbole (PDB-Dateien). Sie möchten sicherstellen, dass Sie die verfügbaren Symbolserver verweisen. Klicken Sie dazu im Menü **Extras** in Visual Studio wählen Sie **Optionen**aus, und wählen Sie dann **Debuggen**, dann **Symbole**. Stellen Sie sicher, dass Microsoft Symbol Servers, klicken Sie unter **Symbol Dateispeicherorte (.pdb aufgeführt ist)**.  Sie können auch http://referencesource.microsoft.com/symbols, verweisen, die gegebenenfalls zusätzliche Zeichen Dateien haben.

![Symboloptionen][4]

Falls gewünscht, können Sie die Berichte vereinfachen, die der Profiler generiert wird, indem Sie nur mein Code. Mit nur mein Code aktiviert ist werden Funktion Anruf Stapel vereinfacht, damit Anrufe zu Bibliotheken und .NET Framework vollständig internen aus den Berichten ausgeblendet werden. Wählen Sie im Menü **Extras** **Optionen**aus. Klicken Sie dann erweitern Sie den Knoten **Performance Tools** , und wählen Sie **Allgemein**aus. Aktivieren Sie das Kontrollkästchen für **Nur meinen Code aktivieren für Profilerberichten**aus.

![Nur mein Code-Optionen][17]

Sie können die folgenden Schritte mit einem vorhandenen Projekt oder ein neues Projekt verwenden.  Wenn Sie zum Testen der unten beschriebenen Verfahren ein neues Projekt erstellen, wählen Sie ein Projekt c# **Azure-Cloud-Dienst** , und wählen Sie eine **Web-Rolle** und eine **Worker-Rolle**aus.

![Azure-Cloud-Dienst Projektrollen][5]

Beispielsweise einfügen Zwecke, einen Code zu einem Projekt, das viel Zeit veranschaulicht einige offensichtlich Leistungsproblem. Fügen Sie beispielsweise den folgenden Code zu einem Worker Rolle Projekt hinzu:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Rufen Sie diesen Code aus der RunAsync-Methode in der Worker-Rolle RoleEntryPoint abgeleitete Klasse. (Ignorieren Sie die Warnung über die Methode synchron ausgeführt wird.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Erstellen Sie und führen Sie der Cloud-Dienst lokal ohne Debuggen (STRG + F5) aus, mit der Lösung-Konfiguration auf **Release**festgelegt. Dadurch wird sichergestellt, dass alle Dateien und Ordner für die Ausführung der Anwendung lokal erstellt werden, und damit ist sichergestellt, dass alle Emulatoren gestartet werden. Starten der berechnen Emulator Benutzeroberfläche über die Taskleiste zu überprüfen, ob Ihre Worker-Rolle ausgeführt wird.

## <a name="2-attach-to-a-process"></a>2: Anfügen an einen Prozess

Statt Profil der Anwendung, indem Sie ihn aus der Visual Studio 2010 IDE zu starten, müssen Sie den Profiler an einen laufenden Prozess anfügen. 

Wählen Sie zum Anfügen eines Prozesses, im Menü **Analyse** des Profilers **Profiler** und **Anfügen/Trennen**.

![Fügen Sie die Profiloption][6]

Suchen Sie den WaWorkerHost.exe Prozess, für eine Worker-Rolle.

![WaWorkerHost Prozess][7]

Wenn Ihr Projektordner auf einem Netzlaufwerk befindet, wird der Profiler in Geben Sie einen anderen Speicherort zum Speichern der Profilerstellungsberichte aufgefordert werden.

 Sie können auch durch Anfügen an WaIISHost.exe zu einer Webrolle anfügen.
Wenn in Ihrer Anwendung mehrere Arbeitsprozesse Rolle vorhanden sind, müssen Sie Prozess-ID verwenden, um sie zu unterscheiden. Sie können die Prozess-ID programmgesteuert Zugriff auf das Objekt Prozess Abfragen. Wenn Sie diesen Code der Run-Methode der RoleEntryPoint abgeleiteten Klasse in einer Rolle hinzufügen, können Sie beispielsweise im Benutzeroberfläche Emulator zu berechnen, wissen, welche Prozess zum Herstellen einer Verbindung mit den Benutzer suchen.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Um das Protokoll anzuzeigen, beginnen Sie der Emulator-Benutzeroberfläche zu berechnen.

![Starten der Serveremulator UI][8]

Öffnen Sie Console-Fenster des Worker Rolle Log in der Emulator-Benutzeroberfläche zu berechnen, indem Sie auf die Titelleiste der Console-Fensters. Sie können die Prozess-ID in der Log anzeigen.

![Ansicht Prozess-ID][9]

Eine, die Sie angehängt haben, führen Sie die Schritte in der Benutzeroberfläche Ihrer Anwendung (falls erforderlich) Wenn Sie um das Szenario zu reproduzieren.

Wenn Sie ein Profil erstellen beenden möchten, wählen Sie den Link zum **Profil beenden** .

![Beenden Sie die Option ein Profil erstellen][10]

## <a name="3-view-performance-reports"></a>3: Anzeigen von Berichten zur workflowleistung

Der Bericht "projektleistung" für die Anwendung wird angezeigt.

Der Profiler an dieser Stelle die Ausführung beendet, speichert Daten in einer VSP-Datei und zeigt einen Bericht, der eine Analyse dieser Daten zeigt.

![Profilerbericht][11]


Wenn Sie in der langsamste Pfad String.wstrcpy angezeigt wird, klicken Sie auf nur mein Code so ändern Sie die Ansicht aus, um nur Benutzercode anzeigen.  Wenn Sie String.Concat angezeigt wird, versuchen Sie, drücken die Schaltfläche Alle Code anzeigen.

Die Methode verketten und ein großer Teil der Ausführung Zeit aufzeichnen String.Concat auftreten.

![Analyse des Berichts][12]

Wenn Sie den Zeichenfolge Verkettung Code in diesem Artikel hinzugefügt haben, sollte eine Warnung in der Aufgabenliste für diesen angezeigt werden. Sie können auch eine Warnung sehen, dass eine übermäßige Menge Garbagecollection, die aufgrund der Anzahl von Zeichenfolgen ist, die erstellt und freigegeben werden.

![Warnungen Leistung][14]

## <a name="4-make-changes-and-compare-performance"></a>4: vorzunehmen und vergleichen Sie die Leistung

Sie können auch die Leistung vor und nach der Änderung einer Code vergleichen.  Beenden Sie den laufenden Prozess, und bearbeiten Sie den Code, um die Zeichenfolge Verkettungsvorgang mit der Verwendung von StringBuilder zu ersetzen:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Führen Sie eine andere Leistung ausführen, und anschließend Vergleichen Sie das Leistungsverhalten. In der Leistung-Explorer Wenn der ausgeführt wird in der gleichen Sitzung, sind können Sie nur wählen Sie beide Berichte aus, das Kontextmenü zu öffnen, und wählen **Leistungsberichte vergleichen**. Wenn Sie mit einer ausführen in einer anderen Leistung Sitzung vergleichen möchten, öffnen Sie das Menü **Analysieren** , und wählen Sie **Leistungsberichte vergleichen**. Geben Sie im Dialogfeld beide Dateien ein.

![Vergleichen Sie die Option für Performance-Berichte][15]

Die Berichte hervorheben Unterschiede zwischen den beiden ausgeführt.

![Vergleichsbericht][16]

Herzlichen Glückwunsch! Sie haben mit dem Profiler gestartet abgerufen.

## <a name="troubleshooting"></a>Behandlung von Problemen

- Stellen Sie sicher, Sie sind ein Profil erstellen einer Version erstellen und ohne Debuggen starten.

- Wenn die Option Anfügen/Trennen im Menü Profiler nicht aktiviert ist, führen Sie die Leistung-Assistenten aus.

- Verwenden Sie der Emulator-Benutzeroberfläche zu berechnen, um den Status Ihrer Anwendung anzuzeigen. 

- Wenn Probleme beim Starten von Applications im Emulator, oder Anfügen des Profilers fahren, nach der Serveremulator unten, und starten Sie ihn erneut. Wenn das Problem auf diese Weise nicht gelöst werden kann, starten Sie neu aus. Dieses Problem kann auftreten, wenn Sie der Serveremulator mithilfe unterbrechen und laufende Bereitstellungen entfernen.

- Wenn Sie eine der Profilerstellungstools Befehle über die Befehlszeile, insbesondere der globalen Einstellungen verwendet haben, stellen Sie sicher, dass VSPerfClrEnv/GlobalOff aufgerufen wurde und VsPerfMon.exe beendet wurde.

- Wenn bei Stichproben, die Nachricht angezeigt "PRF0025: Es wurden keine Daten aufgeführt," prüfen, ob der Vorgang, an der Sie, CPU-Aktivität verfügt. Anwendungen, die keine berechnete Arbeit ausführen, sind möglicherweise keine werden Daten zurück.  Es ist es möglich, dass der Prozess beendet, bevor alle Stichproben durchgeführt wurde. Stellen Sie sicher, dass die Run-Methode für eine Rolle aus, die Sie ein Profil erstellen nicht beendet wird.

## <a name="next-steps"></a>Nächste Schritte

Instrumentation Azure Binärdateien im Emulator wird im Visual Studio Profiler nicht unterstützt, aber wenn Sie arbeitsspeicherzuteilung testen möchten, können Sie diese Option auswählen, wenn Sie ein Profil erstellen. Sie können auch Parallelität ein Profil erstellen, derer Sie feststellen, ob Threads befinden sich weiteren Zeitaufwand Anspruch auf Sperren oder Ebene Interaktion ein Profil erstellen, das Sie bei Leistungsproblemen aufzufinden bei der Interaktion zwischen Ebenen einer Anwendung, am häufigsten zwischen der Datenebene und eine Worker-Rolle unterstützt.  Sie können Datenbankabfragen, die Ihre app generiert anzeigen und verwenden die Profilerstellungsdaten zur Verbesserung Ihrer Verwendung der Datenbank. Informationen zu Ebene Interaktion Profil erstellen, finden Sie im Blogbeitrag [Exemplarische Vorgehensweise: verwenden die Ebene Interaktion Profiler in Visual Studio Team System 2010][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 