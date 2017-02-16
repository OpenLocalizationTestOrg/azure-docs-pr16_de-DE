<properties
    pageTitle="Mobile Engagement Konzepte | Microsoft Azure"
    description="Azure Mobile Engagement Konzepte"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile Engagement Konzepte

Mobile Engagement definiert einige allgemeine Konzepte zu allen unterstützten Plattformen. In diesem Artikel werden diese Konzepte kurz beschrieben.

In diesem Artikel ist ein guter Anfang, wenn Sie neu bei Mobile Engagement sind. Stellen Sie auch sicher ist, lesen in Ihrer Dokumentation zur Plattform, die Sie verwenden, die Wenn sie die in diesem Artikel mit mehr Details und Beispielen sowie mögliche Einschränkungen beschriebenen Konzepte verfeinern wird.

## <a name="devices-and-users"></a>Geräte und Benutzer
Mobile Engagement identifiziert Benutzer durch einen eindeutigen Bezeichner für jedes Gerät generieren. Dieser Bezeichner heißt die Geräte-ID (oder `deviceid`). Es wird in so, dass alle Applikationen Ausführung von dem gleichen Gerät freigeben die gleichen Geräte-ID generiert.

Implizit, bedeutet dies, dass Mobile Engagement betrachtet ein Gerät genau ein Benutzer angehören, und auf diese Weise Benutzer und Geräte Konzepte gleichzusetzen sind.

## <a name="sessions-and-activities"></a>Sitzungen und Aktivitäten
Eine Sitzung ist eine Verwendung der Anwendung von einem Benutzer ausgeführt, ab dem Zeitpunkt der Benutzer beginnt, bis der Benutzer Stopps verwenden.

Eine Aktivität ist eine Verwendung eines bestimmten untergeordnete Teils der Anwendung ausgeführte Arbeit von einem Benutzer (normalerweise ein Bildschirm ist, aber nichts geeignet ist, die Anwendung kann sein).

Ein Benutzer kann nur eine Aktivität nacheinander ausführen.

Eine Aktivität wird durch einen Namen (begrenzt auf 64 Zeichen) gekennzeichnet und kann optional einige zusätzlichen Daten (in der maximal 1024 Byte) einbetten.

Sitzungen werden aus der Reihenfolge der Aktivitäten, die von Benutzern durchgeführt werden automatisch berechnet. Eine Sitzung wird gestartet, wenn der Benutzer seinen ersten Aktivität startet und hält an, wenn er seine letzte Aktivität fertig gestellt wurde. Dies bedeutet, dass eine Sitzung nicht explizit gestartet oder beendet werden muss. Aktivitäten werden stattdessen explizit gestartet oder beendet. Wenn keine Aktivität gemeldet wird, wird keine Sitzung gemeldet.

## <a name="events"></a>Ereignisse
Ereignisse werden verwendet, um instant Aktionen (wie gedrückt oder Artikel lesen von Benutzern) zu melden.

Die aktuelle Sitzung, einer laufenden Projekt ein Ereignis verknüpft werden kann oder ein Ereignis eigenständigen.

Ein Ereignis wird durch einen Namen (begrenzt auf 64 Zeichen) gekennzeichnet und kann optional einige zusätzlichen Daten (in der maximal 1024 Byte) einbetten.

## <a name="error"></a>Fehler
Fehler werden verwendet, um die Probleme, die von der Anwendung (wie falsche Benutzeraktionen oder Fehler beim Aufrufen eines API) nicht richtig erkannt zu informieren.

Die aktuelle Sitzung, zu einer laufenden Auftrag, ein Fehler verknüpft werden kann oder einem eigenständigen zurück.

Ein Fehler wird durch einen Namen (begrenzt auf 64 Zeichen) gekennzeichnet und kann optional einige zusätzlichen Daten (in der maximal 1024 Byte) einbetten.

## <a name="job"></a>Position
Aufträge werden verwendet, um die Aktivitäten, die Dauer ein Berichts (wie Dauer der API-Aufrufe, Anzeigen der anzeigen, Dauer von Vorgängen auf Hintergrund oder die Dauer von Aktionen des Benutzers).

Ein Projekt ist nicht mit einer Sitzung für verknüpft, da ein Vorgangs im Hintergrund, ohne Benutzereingriff ausgeführt werden kann.

Ein Auftrag wird durch einen Namen (begrenzt auf 64 Zeichen) gekennzeichnet und kann optional einige zusätzlichen Daten (in der maximal 1024 Byte) einbetten.

## <a name="crash"></a>Absturz
Absturz werden automatisch vom Mobile Engagement SDK Anwendungsfehler, wo Sie Probleme, die durch die Anwendung nicht erkannt nutzen, Absturz melden ausgestellt.

## <a name="application-information"></a>Informationen zur Anwendung
Anwendungsinformationen (oder app Info) wird verwendet, Kategorisieren von Benutzern, d. h., einige Daten für die Benutzer einer Anwendung zugeordnet werden soll (Dies ist ähnlich wie Web Cookies, außer dass app Informationen auf der Plattform Azure Mobile Engagement auf dem Server gespeichert ist).

App Informationen kann mithilfe der Mobile Engagement SDK API oder mithilfe der Mobile Engagement Plattform Device-API registriert werden.

App-Informationen ist ein Schlüssel/Wertpaar mit einem Gerät zugeordnet ist. Der Schlüssel ist der Name des app Info (begrenzt auf 64 ASCII-Buchstaben [a-zA-Z], [0-9] von Zahlen und Unterstriche [_]). Der Wert (1024 Zeichen beschränkt) kann jede Zeichenfolge, ganze Zahl, Datum (JJJJ / MM / TT) oder jeder Boolesch (WAHR oder falsch) sein.

Eine beliebige Anzahl von app Informationen kann auf einem Gerät, durch die Mobile Engagement Preisgestaltung Angabe festgelegten Grenzwerte verknüpft werden. Für einen angegebenen Schlüssel werden von nachverfolgt Mobile Engagement nur die neuesten Wertemenge (keine History). Festlegen oder Ändern des Werts einer app Info erzwingt Mobile Engagement Zielgruppe erneut auszuwerten Kriterien festlegen auf diese app Informationen (falls vorhanden) d. h., diese app Informationen in Echtzeit schiebt auslösen verwendet werden kann.

## <a name="extra-data"></a>Zusätzliche Daten
Zusätzliche Daten (oder Extras) sind ein paar beliebigen Daten, die Ereignisse, Fehler, Aktivitäten und Aufträge angefügt werden können.

Extras auf JSON-Objekte auf ähnliche Weise strukturiert werden: einer Struktur von Schlüssel/Wert-Paare hergestellt werden. Schlüssel können höchstens 64 ASCII-Buchstaben [a-zA-Z], [0-9]-Zahlen und Unterstriche [_]) und die Gesamtgröße von Extras auf 1024 Zeichen (einmal in JSON vom Mobile Engagement SDK codierte) beschränkt ist.

Die gesamte Struktur der Schlüssel/Wert-Paare wird als ein JSON-Objekt gespeichert. Trotzdem ist nur für die erste Ebene von/Werte für einige erweiterten Funktionen wie Segmente direkt zugänglich zerlegte (beispielsweise Sie können einfach Definieren eines Namens "SciFi Lüfter" alle Benutzer müssen mindestens 10 Mal das Ereignis mit dem Namen "Content_viewed" gesendet erfolgt Segment mit der zusätzliche Key "Content_type" legen Sie den Wert "Scifi" in den letzten Monat). Somit ist es dringend empfohlen, zu senden, die nur Extras von einfachen Listen der Schlüssel/Wert-Paare von skalaren Werten (z. B. Zeichenfolgen, Datumsangaben, Zahlen oder boolesch) gemacht.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über das Universal SDK Windows Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
- [Übersicht über das Windows Phone Silverlight SDK Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK für Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
- [Android SDK für Azure mobilen Engagement](mobile-engagement-android-sdk-overview.md)
