<properties
   pageTitle="Diagnose von Fehlern des Logik apps | Microsoft Azure"
   description="Gemeinsamer Ansätze für Grundlegendes zu, in dem Logik apps weiß nicht"
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

# <a name="diagnosing-logic-app-failures"></a>Diagnose von Logik app-Fehlern

Wenn Probleme und Fehler bei mit der Logik Apps-Funktion von Azure-App-Dienst auftreten, können einige Vorgehensweisen helfen Ihnen bei besser zu verstehen, in dem der Fehler von stammen.  

## <a name="azure-portal-tools"></a>Azure Portals tools

Azure-Portal bietet viele Tools, um zu jeder app Logik bei jedem Schritt diagnostizieren.

### <a name="trigger-history"></a>Auslösen Verlauf

Jede Logik app enthält mindestens ein auslösen. Wenn Sie feststellen, dass apps ausgelöst nicht zur Verfügung, ist der erste Ort an, um weitere Informationen zu suchen des Verlaufs auslösen. Sie können den Verlauf Trigger auf das Hauptfenster Logik app-Blade zugreifen.

![Suchen den Verlauf auslösen][1]

Dies sind alle der Trigger versucht, die Ihre app Logik vorgenommen hat. Sie können jede Trigger beim Abrufen der nächsten Detailebene (insbesondere Eingaben oder Ausgaben, die der Trigger Versuch generiert) klicken. Wenn Sie alle Fehler beim Trigger angezeigt wird, klicken Sie auf der Trigger Versuch und Drillinto auf den Link **Ausgaben** Fehlermeldungen angezeigt, die möglicherweise (zum Beispiel für ungültige FTP-Anmeldeinformationen) generiert wurden.

Die verschiedenen Status, die möglicherweise angezeigt werden:

* **Übersprungen**. Abgefragt den Endpunkt nach Daten suchen, und erhalten eine Antwort, dass keine Daten verfügbar waren.
* **Erfolgreich verlaufen ist**. Der Trigger erhalten Reaktionen Daten verfügbar waren. Dies kann aus einem manuellen auslösen, Auslösen einer Serie oder eines Triggers Umfragen sein. Dies wahrscheinlich wird beizufügen mit dem Status **Fired**, aber es möglicherweise nicht, wenn Sie eine Bedingung oder einen SplitOn Befehl in Code anzeigen verfügen, die wurde nicht erfüllt.
* **Fehler bei**. Ein Fehler ausgelöst wurde.

#### <a name="starting-a-trigger-manually"></a>Manuelles Starten eines Triggers

Wenn Sie die app Logik eines Triggers zur Verfügung sofort (ohne warten auf die nächste Wiederholung) überprüfen möchten, können Sie **Trigger auswählen** auf das Hauptfenster Blade So erzwingen Sie ein Häkchen klicken. Klicken auf diesen Link zu einem Dropbox Trigger verursacht z. B. den Workflow sofort Dropbox für neue Dateien abzufragen.

### <a name="run-history"></a>Ausführen des Verlaufs

Jeder Trigger, der ausgelöst wird, die Ergebnisse in einer Instanz. Sie können ausführen Informationen aus dem Hauptfenster Blade, zugreifen, die eine große Datenmenge enthält, die Ihnen das Verständnis geblieben während des Workflows werden können.

![Suchen den Verlauf ausführen][2]

Eine ausführen zeigt einen der folgenden Status:

* **Erfolgreich verlaufen ist**. Alle Aktionen erfolgreich oder, wenn ein Fehler aufgetreten ist, wurde sie von eine Aktion, die später im Workflow aufgetreten sind behandelt. D. h., wurde sie von einer Aktion behandelt, die zur Ausführung nach eine fehlgeschlagene Aktion festgelegt wurde.
* **Fehler bei**. Mindestens eine Aktion hat einen Fehler an, der von einer Aktion später im Workflow nicht bearbeitet wurde.
* **Abgebrochen**. Der Workflow ausgeführt wurde, aber hat eine Anforderung abbrechen.
* **Ausgeführt wird**. Der Workflow wird derzeit ausgeführt. Dies kann für Workflows, die gedrosselt wird sind oder aufgrund der aktuellen App Serviceplan auftreten. Aktion Grenzwerte finden Sie auf der [Seite Preise](https://azure.microsoft.com/pricing/details/app-service/plans/) Details. Konfigurieren von Diagnose können (die Diagramme aufgeführten des Verlaufs ausführen) auch Informationen zu Ereignissen Einschränkung bereitstellen, die auftreten.

Wenn Sie bei einer ausführen Verlauf suchen, können Sie weitere Details anzeigen.  

#### <a name="trigger-outputs"></a>Auslösen Ausgaben

Trigger Ausgaben anzeigen die Daten, die von der Trigger empfangen wurde. Dies kann Ihnen helfen festzustellen, ob alle Eigenschaften zurückgegeben, wie erwartet.

>[AZURE.NOTE] Möglicherweise sollten Sie wissen, wie [verschiedene Inhaltstypen verarbeitet](app-service-logic-content-type.md) der Logik Apps bereitstellen, wenn alle Inhalte, die Sie verstehen nicht angezeigt.

![Beispiele für die Ausgabe Trigger][3]

#### <a name="action-inputs-and-outputs"></a>Aktion ein- und Ausgaben

Sie können ein Drillinto Eingaben und Ausgaben, die eine Aktion empfangen. Dies ist nützlich für das Verständnis der Größe und Form der Ausgaben, auch unter ', um alle Fehlermeldungen, die möglicherweise generiert wurden, finden Sie unter.

![Aktion ein- und Ausgaben][4]

## <a name="debugging-workflow-runtime"></a>Für das Debuggen Laufzeit des Workflows

Zusätzlich zur Überwachung der Eingaben, Ausgaben und Trigger eines Testlaufs, könnte es hilfreich, einige Schritte innerhalb eines Workflows mit Debuggen helfen hinzufügen. [RequestBin](http://requestb.in) ist ein leistungsfähiges Tool, das Sie als einen Schritt in einem Workflow hinzufügen können. Mithilfe von RequestBin können Sie ein HTTP-Anforderung Inspektor einrichten, um die genaue Größe, Form und Formatieren einer HTTP-Anforderung zu bestimmen. Sie können eine neue RequestBin erstellen und fügen Sie die URL in eine app Logik HTTP POST Aktion zusammen mit Textkörpers, die Sie testen möchten (beispielsweise einen Ausdruck oder ein anderes Ergebnis Schritt). Nachdem Sie die app Logik ausführen, können Sie Ihre RequestBin um anzuzeigen, wie die Anfrage formatiert wurde aktualisieren während sie aus der Logik Apps-Engine erstellt wurde.




<!-- image references -->
[1]: ./media/app-service-logic-diagnosing-failures/triggerHistory.PNG
[2]: ./media/app-service-logic-diagnosing-failures/runHistory.PNG
[3]: ./media/app-service-logic-diagnosing-failures/triggerOutputsLink.PNG
[4]: ./media/app-service-logic-diagnosing-failures/ActionOutputs.PNG
