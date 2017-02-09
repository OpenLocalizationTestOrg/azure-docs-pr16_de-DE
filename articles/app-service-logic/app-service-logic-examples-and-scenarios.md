<properties
   pageTitle="Logik Apps Beispielen und Szenarios | Microsoft Azure"
   description="Beispiele für allgemeine Logik apps und erfahren, wie Sie allgemeine Szenarien implementieren."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Beispiele für Logik Apps und häufige Szenarien

Dieses Dokument Details häufige Szenarien und Beispiele helfen zu verstehen, einige Möglichkeiten des Sie können Logik apps Sie automatisieren von Geschäftsprozessen. 

## <a name="custom-triggers-and-actions"></a>Benutzerdefinierte Trigger und Aktionen

Es gibt mehrere Methoden, die Sie eine Logik app aus einer anderen Anwendung auslösen können. Hier sind einige allgemeine Beispiele:

- [Erstellen eines benutzerdefinierten Trigger oder einer Aktion](app-service-logic-create-api-app.md)
- [Langer Aktionen](app-service-logic-create-api-app.md)
- [HTTP-Anforderung Trigger (POST)](app-service-logic-http-endpoint.md)
- [Webhook Trigger und Aktionen](app-service-logic-create-api-app.md)
- [Umfragen Trigger](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Szenarien

- [Anforderung synchron Antwort](app-service-logic-http-endpoint.md)
- [Anfordern der Antwort mit SMS](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Fehlerbehandlung und Protokollierung

- [Ausnahme und Fehlerbehandlung](app-service-logic-exception-handling.md)
- [Konfigurieren von Azure Benachrichtigungen und Diagnose](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Szenarien

- [Anwendungsfall-: Fehler und Behandlung von Ausnahmen](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Bereitstellen und Verwalten von

- [Erstellen Sie eine automatische Bereitstellung](app-service-logic-create-deploy-template.md)
- [Erstellen und Bereitstellen von Logik apps von Visual Studio](app-service-logic-deploy-from-vs.md)
- [Überwachen der apps Logik](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Inhaltstypen, die bei Konvertierungen des und Transformationen

Die Logik Apps [Workflow Definition Language](http://aka.ms/logicappsdocs) enthält viele Funktionen, um Sie zum Konvertieren von und Arbeiten mit verschiedenen Inhaltstypen zulassen.  Darüber hinaus werden die-Engine erfolgt kann es um Inhaltstypen als Daten Zahlungen durch den Workflow zu erhalten.

- [Behandlung von Inhaltstypen](app-service-logic-content-type.md) , wie die Anwendung/Json-Anwendung/XML- und nur-text
- [Definitionen der Workflow verfassen](app-service-logic-author-definitions.md)
- [Workflow Definition Language Bezug](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Blattnamen und Schleifen

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Bis zu](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integration mit Azure-Funktionen

- [Azure-Funktionen-integration](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Szenarien

- [Azure-Funktion als Dienstbus trigger](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP, REST und SOAP

 - [Aufrufen von SOAP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Wir werden Beispiele und Szenarien dieses Dokument hinzufügen zu behalten. Verwenden Sie den Abschnitt "Kommentare", um uns mitzuteilen, welche Beispiele oder Szenarien Sie hier sehen möchten.