<properties
    pageTitle="Fügen Sie die Aktion Abfrage Logik Apps | Microsoft Azure"
    description="Übersicht über die Aktion Abfrage zum Ausführen von Aktionen wie Filter Matrix zurück."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Erste Schritte mit der Aktion Abfrage

Mithilfe der Aktion Abfrage können Sie mit der Stapeln und Matrizen zufriedenzustellen Workflows zu arbeiten:

- Erstellen Sie eine Aufgabe für alle wichtigen Datensätze aus einer Datenbank ein.
- Speichern Sie alle PDF-Anlagen in einer Azure Blob-e-Mails.

Um anzufangen mithilfe der Aktion für die Abfrage in einer app Logik, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>Verwenden Sie die Aktion Abfrage

Eine Aktion ist ein Vorgang, der über der Workflow ausgeführt wird, die in einer app Logik definiert ist. [Erfahren Sie mehr über Aktionen](connectors-overview.md).  

Die Aktion Abfrage enthält derzeit einen Vorgang, die Filters-Matrix, die im Designer verfügbar gemacht wird aufgerufen. So können Sie ein Array Abfragen und eine Reihe von gefilterten Ergebnisse zurückgeben.

Hier ist, wie Sie es in eine app Logik hinzufügen können:

1. Wählen Sie die Schaltfläche für den **Neuen Schritt** aus.
2. Wählen Sie **eine Aktion hinzufügen**.
3. Geben Sie in das Suchfeld der Aktion **Filtern** , um die Liste der **Filters Array** Aktion aus.

    ![Wählen Sie die Aktion Abfrage](./media/connectors-native-query/using-action-1.png)

4. Wählen Sie aus einer Matrix zurück zu filtern. (Der folgende Screenshot zeigt die Matrix der Ergebnisse einer Suche Twitter.)
5. Erstellen Sie eine Bedingung für jedes Element ausgewertet werden soll. (Der folgende Screenshot filtert Tweets von Benutzern, die mehr als 100 Followers haben.)

    ![Durchführen der Aktion für die Abfrage](./media/connectors-native-query/using-action-2.png)

    Die Aktion wird ein neues Array ausgeben, das nur Ergebnisse enthält, die die Filter Anforderungen erfüllt.
6. Klicken Sie auf die obere linke Ecke der Symbolleiste auf Speichern, und Ihre app Logik wird sowohl speichern und veröffentlichen (aktivieren).

## <a name="query-action"></a>' ÖffnenAbfrage '

Hier sind die Details für die Aktion, die dieser Connector unterstützt. Der Verbinder ist eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|Filter-Matrix|Wertet eine Bedingung für jedes Element in einem Array und gibt das Ergebnis zurück|

## <a name="action-details"></a>Aktionsdetails

Die Aktion Abfrage verfügt über eine mögliche Aktion. Die folgenden Tabellen beschreiben die erforderlichen und optionalen Eingabefelder für die Aktion und den entsprechenden Ausgabedetails, die mit der Aktion zugeordnet sind.

### <a name="filter-array"></a>Filter-Matrix
Im folgenden werden die Eingabefelder für die Aktion, wodurch eine ausgehende HTTP-Anforderung.
A * bedeutet, dass es ein Feld erforderlich ist.

|Anzeigename|Eigenschaftsname|Beschreibung|
|---|---|---|
|Aus *|Von|Die Matrix zum Filtern|
|Bedingung *|WHERE|Die Bedingung für für jedes Element ausgewertet werden soll|
<br>

### <a name="output-details"></a>Die Ausgabedetails

Im folgenden sind die Ausgabedetails für die HTTP-Antwort.

|Eigenschaftsname|Datentyp|Beschreibung|
|---|---|---|
|Gefilterte Matrix|Matrix|Ein Array, ein Objekt für jedes gefilterten Ergebnis enthält.|

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Sie können der verfügbaren Connectors Logik Apps vertraut machen, indem Sie die [Liste der APIs](apis-list.md).
