<properties
    pageTitle="Offline-Daten synchronisieren in Azure Mobile-Apps | Microsoft Azure"
    description="Konzeptionelle Bezug und Übersicht über das Feature zur Synchronisierung offline-Daten für Azure Mobile-Apps"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Offline-Daten synchronisieren in Azure Mobile-Apps

## <a name="what-is-offline-data-sync"></a>Was ist offline-Daten synchronisieren?

Offline-Daten synchronisieren ist ein Client und Server SDK Feature von Azure Mobile-Apps, die Entwicklern das Erstellen von apps, die Verbindung funktionieren erleichtert.

Wenn Ihre app im Offlinemodus befindet, können Benutzer weiterhin erstellen und Ändern von Daten, der auf einem lokalen Speicher gespeichert werden soll. Wenn die app wieder online ist, können sie lokale Änderungen mit Ihrer Azure Mobile-App Back-End-synchronisieren. Das Feature unterstützt auch zum Erkennen von Konflikten, wenn auf dem Client und die Back-End-denselben Datensatz geändert wird. Konflikte können dann entweder auf dem Server oder dem Client verarbeitet werden.

Offlinesynchronisierung weist eine Reihe von Vorteilen:

* Verbesserung der Reaktionszeiten der app durch Server-Daten auf dem Gerät lokal zwischenspeichern
* Erstellen Sie robuste apps, die hilfreiche bleiben Vorliegens Netzwerkproblemen
* Erstellen und Ändern von Daten, selbst wenn es ist keine Unterstützung von Szenarien mit wenig oder gar Connectivity-Netzwerkzugriff vorschreibt Endbenutzer zulassen
* Synchronisieren von Daten über mehrere Geräte und Konflikte erkennen, wenn Sie demselben Datensatz von zwei Geräte geändert wird
* Einschränken der Verwendung im Netzwerk in Wartezeiten oder getakteten Netzwerken

Die folgenden Lernprogramme anzeigen, wie Ihre mobilen Clients Azure Mobile-Apps mit offlinesynchronisierung hinzugefügt:

* [Android: Aktivieren der offlinesynchronisierung]
* [iOS: Aktivieren der offlinesynchronisierung]
* [IOS Xamarin: Aktivieren der offlinesynchronisierung]
* [Xamarin Android: Aktivieren der offlinesynchronisierung]
* [Xamarin.Forms: Aktivieren offlinesynchronisierung](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Universal Windows-Plattform: Aktivieren der offlinesynchronisierung]

## <a name="what-is-a-sync-table"></a>Was ist eine Synchronisierungstabelle?

Für den Zugriff auf den Endpunkt "/ Tabellen" bieten Azure-Mobilclient SDKs Schnittstellen wie `IMobileServiceTable` (.NET Client SDK) oder `MSTable` (iOS-Client). Diese APIs direkt auf die Mobile-App Azure Back-End-verbinden und schlägt fehl, wenn das Client-Gerät nicht über eine Netzwerkverbindung verfügt.

Um die Offlineverwendung zu unterstützen, Ihre app sollte stattdessen anhand der *Synchronisierungstabelle* APIs, wie `IMobileServiceSyncTable` (.NET Client SDK) oder `MSSyncTable` (iOS-Client). Arbeiten dieselben Vorgänge (erstellen, lesen, aktualisieren, löschen) gegen Synchronisierungstabelle APIs, außer jetzt diese lesen oder Schreiben von einem *lokalen Speicher*. Damit alle synchronisieren Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher Initialisierung.

## <a name="what-is-a-local-store"></a>Was ist eine lokale Store?

Ein lokaler Speicher ist der Beibehaltung Datenebene auf das Client-Gerät an. Die Mobile-Apps Azure Client SDKs bieten eine standardmäßige lokale Store Implementierung. Klicken Sie auf Windows, Xamarin und Android basiert er auf SQLite; unter iOS ist es Core anhand von Daten.

Um die SQLite-basierte Implementierung unter Windows Phone oder Windows Store 8.1 verwenden zu können, müssen Sie eine Erweiterung SQLite zu installieren. Weitere Informationen hierzu finden Sie unter [Universal Windows-Plattform: Aktivieren der offlinesynchronisierung]. Android Lieferumfang iOS und eine Version von SQLite in das Betriebssystem des Geräts selbst, daher es nicht erforderlich ist, auf Ihrer eigenen Version von SQLite verweisen.

Entwickler können auch ihre eigenen lokalen Speicher implementieren. Wenn Sie Daten in einem verschlüsselten Format auf dem mobilen Client speichern möchten, können Sie beispielsweise einen lokalen Speicher definieren, der SQLCipher für die Verschlüsselung verwendet.

## <a name="what-is-a-sync-context"></a>Was ist ein Kontext synchronisieren?

Ein *Synchronisieren Kontext* einem mobilen Client-Objekt zugeordnet ist (z. B. `IMobileServiceClient` oder `MSClient`) und verfolgt Änderungen, die mit Tabellen synchronisieren vorgenommen werden. Im Kontext synchronisieren verwaltet, dass ein *Vorgangswarteschlange* die einer sortierten Liste von CRUD-Vorgänge (erstellen, aktualisieren, löschen), die behält später auf dem Server gesendet wird.

Kontext synchronisieren mit einer Initialisierungsmethode ist ein lokaler Speicher zugeordnet `IMobileServicesSyncContext.InitializeAsync(localstore)` im [.NET Client SDK].

## <a name="a-namehow-sync-worksahow-offline-synchronization-works"></a><a name="how-sync-works"></a>Wie offline Synchronisierung funktioniert

Beim Synchronisieren von Tabellen steuert Ihre Client-Code beim lokale Änderungen mit einer Azure Mobile-App Back-End-synchronisiert werden soll. Nichts wird an die Back-End-gesendet, bis ein Anrufs *Pushbenachrichtigungen* lokalen Änderungen vorhanden ist. Auf ähnliche Weise wird der lokale Speicher mit neuen Daten aufgefüllt, nur, wenn Sie ein Anruf, um *Daten* vorhanden ist.

* **Pushbenachrichtigungen**: Pushbenachrichtigungen ist ein Vorgang auf dem Kontext synchronisieren und sendet alle CUD Änderungen seit der letzten Pushbenachrichtigungen. Beachten Sie, dass es nicht möglich, nur eine einzelne Tabelle Änderungen zu senden, da andernfalls Vorgänge außerhalb der Reihenfolge gesendet werden konnten. Pushbenachrichtigungen führt eine Reihe von REST Anrufe an Ihre Mobile-App Azure Back-End, die wiederum die Serverdatenbank ändern.

* **Ziehen Sie**: Abruf auf Basis pro Tabelle ausgeführt wird, und kann mit einer Abfrage zum Abrufen von nur einer Teilmenge der Serverdaten angepasst werden. Die Azure Mobile-Client-SDKs einfügen klicken Sie dann die resultierenden Daten in den lokalen Speicher.

* **Implizite legt**: Wenn Sie ein Abruf für eine Tabelle ausgeführt wird, ausstehende lokale Updates hat, Ausführen die Abruf einer Pushbenachrichtigungen zuerst auf dem Kontext synchronisieren. Auf diese Weise können Konflikte zwischen Änderungen, die bereits in der Warteschlange befinden und neue Daten vom Server zu minimieren.

* **Inkrementell synchronisieren**: der erste Parameter für den Abruf Vorgang ist ein *Abfragenamen* , die nur auf dem Client verwendet wird. Wenn Sie einen Abfragenamen nicht-Null verwenden, wird der Azure Mobile SDK eine *inkrementell synchronisieren*ausgeführt.
  Jedes Mal ein Abruf-Vorgang gibt eine Reihe von Ergebnisse, die neueste `updatedAt` Zeitstempel aus diesem Resultset wird in der lokalen System SDK gespeichert. Nachfolgende Abruf Vorgänge werden nur Datensätze nach dem jeweiligen Zeitstempel abgerufen.

  Um inkrementell synchronisieren verwenden zu können, muss Ihr Server aussagekräftigen zurückgeben `updatedAt` Werte und Sortieren nach diesem Feld muss ebenfalls unterstützen. Da das SDK eine eigene Sortieren klicken Sie auf das Feld UpdatedAt hinzufügt, Sie jedoch können nicht verwenden, Anforderns abrufen, die über ein eigenes `$orderBy$` Klausel.

  Name der Abfrage eine beliebige Zeichenfolge, die Sie auswählen, werden kann, aber für jede logische Abfrage in Ihrer app eindeutig sein.
  Andernfalls anderen Abruf Vorgänge konnte denselben inkrementell synchronisieren Zeitstempel überschreiben und Ihre Abfragen können falsche Ergebnisse zurück.

  Wenn die Abfrage Parameter enthält, ist eine Möglichkeit zum Erstellen von eindeutigen Abfragenamen Parameterwert einfügen.
  Wenn Sie Benutzer-ID filtern möchten, konnte der Abfragenamen beispielsweise wie folgt (in c#) werden:

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Wenn Suchbegriffen nicht inkrementell synchronisiert werden soll, übergeben `null` als die Abfrage-ID. In diesem Fall werden alle Datensätze abgerufen werden, auf jeder eines Anrufs an die `PullAsync`, potenziell aufgeführt.

* **Löschen**: können Sie den Inhalt der lokalen Speicher mit löschen `IMobileServiceSyncTable.PurgeAsync`.
  Dies kann erforderlich sein, wenn Sie veraltete Daten in der Clientdatenbank verfügen oder wenn Sie alle ausstehende Änderungen verwerfen möchten.

  Eine bereinigen wird eine Tabelle aus dem lokalen Speicher deaktivieren. Wenn Vorgänge ausstehend Synchronisierung mit der Server-Datenbank vorhanden sind, wird der Löschvorgang eine Ausnahme auslösen, wenn der Parameter *Force bereinigen* festgelegt ist.

  Ein Beispiel für veraltete Daten auf dem Client nehmen Sie an im Beispiel "Aufgabenliste" Gerät1 nur Elemente abruft, die nicht abgeschlossen sind. Klicken Sie dann eine Todoitem "Milch kaufen" steht auf dem Server durch ein anderes Gerät abgeschlossen. Jedoch müssen Gerät1 weiterhin die Todoitem "Kaufen Milch" im lokalen Speicher, da nur Elemente abgerufen wird, die nicht als abgeschlossen gekennzeichnet sind. Eine bereinigen wird diese veraltete Element zu löschen.

## <a name="next-steps"></a>Nächste Schritte

* [iOS: Aktivieren der offlinesynchronisierung]
* [IOS Xamarin: Aktivieren der offlinesynchronisierung]
* [Xamarin Android: Aktivieren der offlinesynchronisierung]
* [Universal Windows-Plattform: Aktivieren der offlinesynchronisierung]

<!-- Links -->
[.NET Client SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Aktivieren der offlinesynchronisierung]: app-service-mobile-android-get-started-offline-data.md
[iOS: Aktivieren der offlinesynchronisierung]: app-service-mobile-ios-get-started-offline-data.md
[IOS Xamarin: Aktivieren der offlinesynchronisierung]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Aktivieren der offlinesynchronisierung]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Universal Windows-Plattform: Aktivieren der offlinesynchronisierung]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
