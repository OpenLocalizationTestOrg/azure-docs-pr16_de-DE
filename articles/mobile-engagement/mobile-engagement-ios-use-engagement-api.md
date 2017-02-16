<properties
    pageTitle="So verwenden Sie die Recherchemöglichkeiten-API unter iOS"
    description="Neueste iOS SDK - Verwendung der Engagement-API unter iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>So verwenden Sie die Recherchemöglichkeiten-API unter iOS

Dieses Dokument ist eine Ergänzung zu den Dokument so integrieren Engagement unter iOS: Es bietet Tiefe Details dazu, wie Sie die Engagement-API verwenden, um die Anwendung Statistik zu melden.

Denken Sie daran, dass Sie nur Engagement, Ihrer Anwendungs Sitzungen, Aktivitäten, stürzt ab und technische Informationen zu melden möchten, klicken Sie dann die einfachste Methode ist, nehmen Sie alle Ihre benutzerdefinierten `UIViewController` Objekte erben von der entsprechenden `EngagementViewController` Class.

Wenn Sie möchten Sie tun Weitere, beispielsweise wenn Sie müssen bestimmte Anwendungsereignisse, Fehlern und Aufträge, Berichte oder müssen Sie Ihrer Anwendung Aktivitäten in eine andere Möglichkeit probiert Bericht als der Implementierung in der `EngagementViewController` Klassen, müssen Sie die Recherchemöglichkeiten-API verwenden.

Die API Engagement erfolgt über die `EngagementAgent` Class. Eine Instanz dieser Klasse kann abgerufen werden durch Aufrufen der `[EngagementAgent shared]` statische Methode (Beachten Sie, dass die `EngagementAgent` zurückgegebene Objekt ist einem einzelnen).

Vor API Aufrufen die `EngagementAgent` Objekt durch Aufrufen der Methode Initialisierung werden muss`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Engagement Konzepte

Die folgenden Webparts verfeinern die allgemeine [Mobile Engagement Konzepte](mobile-engagement-concepts.md) für die iOS-Plattform.

### <a name="session-and-activity"></a>`Session`und`Activity`

Eine *Aktivität* wird in der Regel zugeordnet einen Bildschirm der Anwendung, d. h. die *Aktivität* wird gestartet, wenn der Bildschirm angezeigt wird, und wird beendet, wenn der Bildschirm geschlossen ist: Dies ist der Fall, wenn Sie mithilfe des SDK Engagement integriert ist die `EngagementViewController` Klassen.

Aber *Aktivitäten* können auch manuell mithilfe der API Engagement gesteuert werden. Auf diese Weise können Teilen einen angegebenen Bildschirm in mehrere Sub-Webparts können Sie weitere Details zur Verwendung von dieser Bildschirm (beispielsweise auf bekannte wie oft und wie lange Dialogfelder innerhalb dieser Bildschirm verwendet werden) zu gelangen.

##<a name="reporting-activities"></a>Berichterstattung

### <a name="user-starts-a-new-activity"></a>Benutzer beginnt, eine neue Aktivität

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Sie müssen Aufrufen `startActivity()` jedes Mal, wenn der Benutzer Aktivität ändert. Der erste Anruf an diese Funktion startet eine neue Sitzung des Benutzers an.

### <a name="user-ends-his-current-activity"></a>Benutzer endet Ihrer aktuellen Tätigkeit ein

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Sie sollten **nie** rufen Sie diese Funktion alleine, außer wenn Sie eine Verwendung Ihrer Anwendung in mehreren Sitzungen teilen möchten: ein Anruf an diese Funktion die aktuelle Sitzung sofort, Ja, beenden möchten nachfolgende Anruf an `startActivity()` würde eine neue Sitzung gestartet. Diese Funktion wird automatisch vom SDK aufgerufen, wenn eine Anwendung geschlossen wird.

##<a name="reporting-events"></a>Reporting Ereignisse

### <a name="session-events"></a>Sitzungsereignisse

Sitzungsereignisse sind in der Regel verwendet, um die Aktionen, die von einem Benutzer ausgeführt werden, während seine Sitzung zu melden.

**Beispiel ohne zusätzliche Daten:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Beispiel mit zusätzlichen Daten:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Eigenständige Ereignisse

Im Gegensatz zu Sitzungsereignisse können eigenständigen Ereignisse außerhalb des Kontexts einer Sitzung verwendet werden.

**Beispiel:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Melden von Fehlern

### <a name="session-errors"></a>Fehler bei der Sitzung

Sitzungsfehler werden in der Regel zum Melden der Fehler beeinträchtigen des Benutzers seinen Sitzung verwendet.

**Beispiel:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Fehler bei der eigenständigen

Im Gegensatz zu Fehlern bei der Sitzung können Fehler bei der eigenständigen außerhalb des Kontexts einer Sitzung verwendet werden.

**Beispiel:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Reporting Aufträge

**Beispiel:**

Nehmen Sie an, dass Sie die Dauer der Anmeldevorgang melden möchten:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Berichtfehler während eines Auftrags

Fehler können mit einem laufenden Projekt statt wird im Zusammenhang mit der aktuellen Benutzer Sitzung verknüpft werden.

**Beispiel:**

Nehmen Sie an, dass Sie einen Fehler während der Anmeldevorgang melden möchten:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Ereignisse während eines Auftrags

Ereignisse können mit einem laufenden Projekt statt wird im Zusammenhang mit der aktuellen Benutzer Sitzung verknüpft werden.

**Beispiel:**

Angenommen Sie, wir haben ein soziales Netzwerk, und wir verwenden eines Auftrags Bericht auf die Gesamtzeit, während, die der Benutzer auf dem Server verbunden ist. Der Benutzer kann über seine Freunde Nachrichten empfangen, dies ist ein Ereignis Position.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Zusätzliche Parameter

Ereignisse, Fehler, Aktivitäten und Aufträge können beliebige Daten zugeordnet werden.

Diese Daten strukturiert werden können, es verwendet den iOS NSDictionary Klasse.

Beachten Sie, dass Extras enthalten können `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` oder eine andere `NSDictionary` Instanzen.

> [AZURE.NOTE] Der zusätzliche Parameter wird in JSON serialisiert. Wenn Sie andere Objekte als die oben beschriebenen übergeben möchten, müssen Sie die folgende Methode in der Klasse implementieren:
>
             -(NSString*)JSONRepresentation;
>
> Die Methode sollte eine JSON-Darstellung des Objekts zurück.

### <a name="example"></a>Beispiel

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Grenzwerte

#### <a name="keys"></a>Tasten

Jeder Schlüssel in der `NSDictionary` muss den folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dies bedeutet, dass die Tasten mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche begonnen werden müssen (\_).

#### <a name="size"></a>Größe

Extras sind maximal **1024** Zeichen pro Anruf (einmal in JSON vom Agent Engagement codierte).

Im vorherigen Beispiel ist die an den Server gesendete JSON 58 Zeichen lang sein:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Reporting Anwendungsinformationen

Sie können manuell melden, Nachverfolgen von Informationen (oder eine beliebige andere anwendungsspezifische Informationen) mit den `sendAppInfo:` (Funktion).

Beachten Sie die folgenden Informationen inkrementell gesendet werden kann: nur der letzten Wert für einen angegebenen Schlüssel für ein angegebenes Gerät bleiben.

Ereignis Extras, wie die `NSDictionary` Klasse wird verwendet, um die abstrakte Anwendungsinformationen, beachten Sie, dass Arrays oder untergeordnete Wörterbücher als flachen Zeichenfolgen (mit JSON-Serialisierung) behandelt werden.

**Beispiel:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Grenzwerte

#### <a name="keys"></a>Tasten

Jeder Schlüssel in der `NSDictionary` muss den folgenden regulären Ausdruck übereinstimmen:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dies bedeutet, dass die Tasten mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche begonnen werden müssen (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen sind maximal **1024** Zeichen pro Anruf (einmal in JSON vom Agent Engagement codierte).

Im vorherigen Beispiel ist die an den Server gesendete JSON 44 Zeichen lang sein:

    {"birthdate":"1983-12-07","gender":"female"}
