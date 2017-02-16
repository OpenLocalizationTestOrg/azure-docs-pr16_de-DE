<properties 
    pageTitle="So verwenden Sie die Engagement API auf Windows Phone Silverlight" 
    description="So verwenden Sie die Engagement API auf Windows Phone Silverlight"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>So verwenden Sie die Engagement API auf Windows Phone Silverlight

Dieses Dokument ist ein Add-on für das Dokument [wie Mobile Engagement in Ihrer app für Windows Phone Silverlight integriert werden soll](mobile-engagement-windows-phone-integrate-engagement.md). Darüber Tiefe Details dazu, wie Sie die Engagement-API verwenden, um die Anwendung Statistik zu melden.

Wenn Sie nur Engagement an Ihrer Anwendungs Sitzungen, Aktivitäten, stürzt ab und technische Informationen zu Berichten möchten, dann ist die einfachste Möglichkeit, sämtliche Ihrer `PhoneApplicationPage` untergeordnete Klassen erben von der `EngagementPage` Class.

Wenn Sie möchten Sie tun Weitere, beispielsweise wenn Sie müssen bestimmte Anwendungsereignisse, Fehlern und Aufträge, Berichte oder müssen Sie Ihrer Anwendung Aktivitäten in eine andere Möglichkeit probiert Bericht als der Implementierung in der `EngagementPage` Klassen, müssen Sie die Recherchemöglichkeiten-API verwenden.

Die API Engagement erfolgt über die `EngagementAgent` Class. Sie können auf diese Methoden über zugreifen `EngagementAgent.Instance`.

Selbst wenn das Modul Agent nicht Initialisierung wurde, jeder Anruf an die API wurde zurückgestellt und erneut ausgeführt wird, wenn der Agent verfügbar ist.

##<a name="engagement-concepts"></a>Engagement Konzepte

Die folgenden Webparts verfeinern Sie die Mobile Engagement Konzepte für Windows Phone-Plattform.

### <a name="session-and-activity"></a>`Session`und`Activity`

Eine *Aktivität* in der Regel zugeordnet ist eine Seite der Anwendung, das heißt die *Aktivität* wird gestartet, wenn die Seite angezeigt wird, und wird beendet, wenn die Seite geschlossen wird: Dies ist der Fall, wenn Sie mithilfe des SDK Engagement integriert ist die `EngagementPage` Class.

Aber *Aktivitäten* können auch manuell mithilfe der API Engagement gesteuert werden. Auf diese Weise können Teilen eine bestimmte Seite in mehreren Sub Teilen, um weitere Details zur Verwendung von dieser Seite (beispielsweise auf bekannte wie oft und wie lange Dialogfelder innerhalb dieser Seite verwendet werden).

##<a name="reporting-activities"></a>Berichterstattung

### <a name="user-starts-a-new-activity"></a>Benutzer beginnt, eine neue Aktivität

#### <a name="reference"></a>Bezug

            void StartActivity(string name, Dictionary<object, object> extras = null)

Sie müssen Aufrufen `StartActivity()` jedes Mal, wenn der Benutzer Aktivität ändert. Der erste Anruf an diese Funktion startet eine neue Sitzung des Benutzers an.

> [AZURE.IMPORTANT] Das SDK automatisch rufen die Endaktivität Methode, wenn die Anwendung geschlossen wird. Auf diese Weise wird es dringend empfohlen, zum Aufrufen der StartActivity-Methode, wenn die Aktivität der Benutzer Änderung, und klicken Sie auf nie rufen Sie die Methode Endaktivität, da die aktuelle Sitzung beendet werden diese Methode aufzurufen erzwingt werden.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Benutzer endet Ihrer aktuellen Tätigkeit ein

#### <a name="reference"></a>Bezug

            void EndActivity()

Sie müssen Aufrufen `EndActivity()` mindestens einmal an der Benutzer endet Ihrer letzten Tätigkeit ein. Dieser informiert das Engagement SDK, dass der Benutzer aktuell im Leerlauf ist, und ab, dass die Benutzer Sitzung einmal das Sitzungstimeout geschlossen werden müssen läuft (Wenn Sie aufrufen `StartActivity()` vor Ablauf das Sitzungstimeout ist einfach die Sitzung Fortsetzung).

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Reporting Aufträge

### <a name="start-a-job"></a>Starten eines Projekts

#### <a name="reference"></a>Bezug

            void StartJob(string name, Dictionary<object, object> extras = null)

Den Auftrag können Sie um über einen Zeitraum vorübegehend Aufgaben zu verfolgen.

#### <a name="example"></a>Beispiel

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Beenden eines Auftrags

#### <a name="reference"></a>Bezug

            void EndJob(string name)

Sobald eine Aufgabe nachverfolgt, indem Sie ein Projekt beendet wurde, sollten Sie die EndJob Methode für dieses Projekt, aufrufen, indem Sie den Namen der Position.

#### <a name="example"></a>Beispiel

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Reporting Ereignisse

Es gibt drei Arten von Ereignissen ein:

-   Eigenständige Ereignisse
-   Sitzungsereignisse
-   Job-Ereignisse

### <a name="standalone-events"></a>Eigenständige Ereignisse

#### <a name="reference"></a>Bezug

            void SendEvent(string name, Dictionary<object, object> extras = null)

Eigenständige Ereignisse außerhalb des Kontexts einer Sitzung ausgeführt werden sollen.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sitzungsereignisse

#### <a name="reference"></a>Bezug

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sitzungsereignisse sind in der Regel verwendet, um die Aktionen, die von einem Benutzer ausgeführt werden, während seine Sitzung zu melden.

#### <a name="example"></a>Beispiel

**Ohne Daten:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Mit Daten:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Job-Ereignisse

#### <a name="reference"></a>Bezug

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Job-Ereignisse sind in der Regel verwendet, um die Aktionen, die von einem Benutzer ausgeführt werden, während eines Auftrags melden.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Melden von Fehlern

Es gibt drei Arten von Fehlern:

-   Fehler bei der eigenständigen
-   Fehler bei der Sitzung
-   Fehler bei der Position

### <a name="standalone-errors"></a>Fehler bei der eigenständigen

#### <a name="reference"></a>Bezug

            void SendError(string name, Dictionary<object, object> extras = null)

Im Gegensatz zu Sitzungsfehler können eigenständigen Fehler außerhalb des Kontexts einer Sitzung auftreten.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Fehler bei der Sitzung

#### <a name="reference"></a>Bezug

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sitzungsfehler werden in der Regel zum Melden der Fehler beeinträchtigen des Benutzers seinen Sitzung verwendet.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Fehler bei der Position

#### <a name="reference"></a>Bezug

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Fehler können mit einem laufenden Projekt statt wird im Zusammenhang mit der aktuellen Benutzer Sitzung verknüpft werden.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Reporting abstürzen

Der Agent bietet zwei Methoden für den Umgang mit stürzt ab.

### <a name="send-an-exception"></a>Senden Sie eine Ausnahme

#### <a name="reference"></a>Bezug

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Beispiel

Sie können eine Ausnahme zu einem beliebigen Zeitpunkt durch Einwahl senden:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Einen optionalen Parameter können Sie auch die Sitzung Engagement zur gleichen Zeit als Senden des Absturzes beendet werden. Rufen Sie hierzu:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Wenn Sie dies tun, werden die Sitzung und Aufträge nur nach dem Senden des Absturzes geschlossen werden.

### <a name="send-an-unhandled-exception"></a>Ausnahmefehler senden

#### <a name="reference"></a>Bezug

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Angebot umfasst auch eine Methode zum Ausnahmefehler zu senden. Dies ist besonders hilfreich, wenn im Silverlight UnhandledException-Ereignishandler verwendet.

Diese Methode wird **immer** beenden die Sitzung Engagement und Aufträge nach aufgerufen wird.

#### <a name="example"></a>Beispiel

Sie können eigene UnhandledException Ereignishandler implementiert wird (besonders, wenn Sie den automatischen Absturz des Projekts Berichterstellungsfunktion deaktiviert haben). Angenommen, in der `Application_UnhandledException` der Methode der `App.xaml.cs` Datei:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##<a name="onactivated"></a>OnActivated

### <a name="reference"></a>Bezug

            void OnActivated(ActivatedEventArgs e)

Wenn der Benutzer weiterleiten, nicht an eine Anwendung navigiert, nachdem das Deactivated Ereignis ausgelöst wurde, versucht das Betriebssystem, das die Anwendung in einem inaktiv Zustand versetzen. Klicken Sie dann ist die Anwendung veraltet. In diesem Prozess eine Anwendung beendet ist jedoch einige Daten über den Status der Anwendung und die einzelnen Seiten innerhalb der Anwendung werden zwar beibehalten.

Sie müssen einfügen `EngagementAgent.Instance.OnActivated(e)` in der `Application_Activated` Methode aus der Datei App.xaml.cs Engagement-Agent zurücksetzen, wenn die Anwendung als veraltet markiert wurde.

### <a name="example"></a>Beispiel

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##<a name="device-id"></a>Geräte-Id

            String GetDeviceId()

Sie können die Engagement Geräte-Id, indem Sie diese Methode erhalten.

##<a name="extras-parameters"></a>Extras Parameter

Ein Ereignis, einen Fehler, eine Aktivität oder eines Auftrags können beliebige Daten zugeordnet werden. Diese Daten können unter Verwendung eines Wörterbuchs strukturiert werden. Tasten und Werte können beliebigen Typs sein.

Extras Daten serialisiert werden, wenn Sie Ihren eigenen Typ in Extras einfügen möchten, Sie einen Datenvertrag für dieses Typs hinzufügen müssen.

### <a name="example"></a>Beispiel

Wir erstellen eine neue Klasse "Person".

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Wir werden dann, Hinzufügen einer `Person` Instanz mit einer Extra.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Wenn Sie andere Arten von Objekten setzen, stellen Sie sicher, dass ihre ToString()-Methode implementiert wird, um eine lesbare Zeichenfolge zurück.

### <a name="limits"></a>Grenzwerte

#### <a name="keys"></a>Tasten

Jeder Schlüssel in das Objekt, muss den folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Dies bedeutet, dass die Tasten mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche begonnen werden müssen (\_).

#### <a name="size"></a>Größe

Extras sind auf **1024** Zeichen pro Anruf begrenzt.

##<a name="reporting-application-information"></a>Reporting Anwendungsinformationen

### <a name="reference"></a>Bezug

            void SendAppInfo(Dictionary<object, object> appInfos)

Sie können manuell Nachverfolgen von Informationen (oder eine beliebige andere anwendungsspezifische Informationen) mithilfe der SendAppInfo() (Funktion) melden.

Beachten Sie die folgenden Informationen inkrementell gesendet werden kann: nur der letzten Wert für einen angegebenen Schlüssel für ein angegebenes Gerät bleiben. Wie Ereignis Extras, verwenden Sie ein Wörterbuch\<Objekt, Objekt\> Informationen anfügen.

### <a name="example"></a>Beispiel

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Grenzwerte

#### <a name="keys"></a>Tasten

Jeder Schlüssel in das Objekt, muss den folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Dies bedeutet, dass die Tasten mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche begonnen werden müssen (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen sind auf **1024** Zeichen pro Anruf beschränkt.

Im vorherigen Beispiel ist die an den Server gesendete JSON 44 Zeichen lang sein:

            {"subscription":"2013-12-07","premium":"true"}
 
##<a name="logging"></a>Protokollierung
###<a name="enable-logging"></a>Aktivieren der Protokollierung

Das SDK kann zum von Testprotokollen in der Verwaltungskonsole IDE Naturprodukte konfiguriert werden.
Diese Protokolle sind standardmäßig nicht aktiviert. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der Werte aus der `EngagementTestLogLevel` Enumeration, z. B.:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
