<properties
    pageTitle="Zum Migrieren von Logik apps zu Schema Version 2015-08-01-Vorschau | Microsoft Azure-App-Verwaltungsdienst"
    description="Sie können Ihre apps Logik einfach auf die neueste Schemaversion migrieren. Befolgen Sie diese Schritte aus."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Zum Migrieren von Logik apps zu Schema Version 2015-08-01-Vorschau

Wenn Ihre vorhandene Logik apps in das neue Schema verschieben möchten, führen Sie folgende Schritte aus:  
1. Öffnen Sie Ihre app Logik im Azure-Portal  
2. Klicken Sie auf Update Schema:

 ![API-Symbol][step1]   
Die Seite Update Schema zeigt und enthält einen Link zu einem Dokument, die Details zu den Verbesserungen in das neue Schema bereitstellen: ![API-Symbol][step2]

>[AZURE.NOTE] Wenn Sie **Update Schema**auswählen, führen Sie die Migrationsschritte wir automatisch, und stellen den Code Ausgaben für Sie. Hiermit können Sie der Definition Ihrer, jedoch aktualisieren, stellen Sie sicher, dass Sie gute Codierung wie im Abschnitt **bewährte Methoden** beschriebenen Vorgehensweisen.

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Bewährte Methoden bei der Migration Ihrer apps Logik auf die neueste Schemaversion:  

- Kopieren Sie das migrierte Skript in eine neue Logik App - überschreiben Sie nicht die alte, die eine bis Abschluss der Tests und die migrierte app bestätigt haben wie erwartet funktioniert.
- Testen Sie Ihrer Logik app **vor dem** einfügen in der Herstellung
- Starten Sie nach Abschluss der Migration Aktualisieren Ihrer apps Logik, um die [verwalteten APIs](./apis-list.md) mögliche verwenden. Beispielsweise können Sie beginnen, verwenden Dropbox Version 2, überall verwendetem DropBox Version 1.


## <a name="whats-next"></a>Nächste Schritte
-  [Erfahren Sie, wie Ihre apps Logik manuell migrieren](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






