<properties
    pageTitle="Mobile exportieren API Projektübersicht"
    description="Erlernen Sie die Grundlagen zum Exportieren von Ihrer unformatierten Daten, die auf des Benutzers Geräten nutzen Sie es in Ihre eigenen tools"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Mobile exportieren API Projektübersicht

## <a name="introduction"></a>Einführung

In diesem Dokument erfahren Sie die Grundlagen zum Exportieren von Ihrem unformatierten Daten von Geräten des Benutzers nutzen Sie es in Ihre eigenen Tools generiert.

## <a name="pre-requisites"></a>Erforderliche Komponenten

Exportieren der unformatierten Daten aus Mobile Engagement erfordert:

- API Authentifizierung Setup kann die APIs verwenden (siehe [Manuelles Setup Authentifizierung](mobile-engagement-api-authentication-manual.md))
- Verwenden Sie entweder die REST-APIs oder die [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
- Ein Konto Azure-Speicher.

>[AZURE.NOTE] Wir empfehlen, auch die hervorragende [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/)mindestens der Entwicklung Phase, wie sie eine benutzerfreundliche Benutzeroberfläche enthält, die für die Interaktion mit Azure-Speicher.

## <a name="what-can-be-exported"></a>Was exportiert werden können?

Mobile-Initiative dessen Benutzer zu viele Arten von Daten zu sammeln und daher verfügt über verschiedene Arten von exportieren, die für diese verschiedenen Datentypen geeignet sind.
Es gibt 2 grundlegende Typen von exportieren:

- Snapshot: verwendet in der Regel zum Exportieren von Daten, die einen Zustand darstellt und für die Mobile Engagement hat keinen Verlauf. Dies umfasst beispielsweise Kategorien (app Info), Token oder Pushbenachrichtigungen für eine Marketingkampagne Feedback. Daher beziehen sich diese Dateitypen exportieren nicht zu einem Datum.
- Zurückliegende: dieses Typs exportieren für Daten, die über einen Zeitraum wie Ereignisse oder Aktivitäten beispielsweise sammelt verwendet wird.

Die folgende Tabelle beschreibt umfassend alle möglichen Exporte:

| Exporttyp | Datentyp | Beschreibung                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Snapshot    | Pushbenachrichtigungen      | Generiert eine des Exports von Pushbenachrichtigungen massensendungen zu ermitteln das Feedback auf Basis pro Geräte-ID-Benutzer-ID                                                              |
| Snapshot    | Kategorie       | Generiert eine Exportieren der Kategorien (app-Info), die jeder Geräte zugeordnet sind                                                                       |
| Snapshot    | Gerät    | Generiert einen Export die meisten Daten zu Geräten, wie etwa die Technicals (Modell, Gebietsschema, Zeitzone,...), die Kategorien, Sicht-Erstmaliges... |
| Snapshot    | Token     | Generiert eine des Exports von allen gültigen Token                                                                                                 |
| Zurückliegende  | Aktivität  | Generiert eine Exportieren aller Aktivitäten für jede Geräte auf einen bestimmten Zeitraum                                                           |
| Zurückliegende  | Ereignis     | Generiert eine Exportieren aller Aktivitäten für jede Geräte auf einen bestimmten Zeitraum                                                           |
| Zurückliegende  | Position       | Generiert eine Exportieren aller Einzelvorgänge für jeden Geräte auf einen bestimmten Zeitraum                                                                 |
| Zurückliegende  | Fehler     | Generiert eine Exportieren aller Fehler für jede Geräte auf einen bestimmten Zeitraum                                                               |

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Exporte sind lang Aufgaben ausführen, die möglicherweise große Datendateien Naturprodukte. Sie können keine Gründen um eine Datei zum Herunterladen sofort zurückzukehren aufgerufen werden.
Um Daten aus Mobile Engagement exportieren, müssen Sie zum Erstellen eines **Auftrags exportieren** über-API, wenn Sie in der Regel angeben:

- Der Typ des Exports (Snapshot oder zurückliegenden)
- Der Datentyp
- Der **Azure-Speichercontainer** (einschließlich einer gültigen SAS mit Schreibzugriff), wird das Ergebnis des Exports geschrieben werden.

Bitte beachten Sie, dass für den Job gestartet werden einige Minuten dauern, und Sie dann es möglicherweise von ein paar Sekunden für sehr apps auf mehrere Stunden für zahlreiche Benutzer oder Aktivität-apps führen.

Nachdem Sie der Auftrag erstellt wurde, ist es möglich, prüfen Sie den Status, um festzustellen, ob es noch aktiv ist, oder wenn sie durchgeführt wurde.

Nachdem Sie der Auftrag wurde erfolgreich abgeschlossen ist, ist die resultierende Datendatei für den Speichercontainer bereitgestellten verfügbar.
