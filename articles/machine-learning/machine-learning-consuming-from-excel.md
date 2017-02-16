<properties
    pageTitle="Einen Computer Learning-Webdienst aus Excel nutzen | Microsoft Azure"
    description="Nutzen Sie einen Azure-Computern Learning-Webdienst aus Excel"
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="tedway"/>

# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a>Verarbeitung eines Azure-Computern Learning-Webdiensts aus Excel

 Azure maschinellen Learning Studio erleichtert die anzurufende Webdienste direkt in Excel ohne Code schreiben zu müssen.

Wenn Sie Excel 2013 (oder höher) oder Excel Online verwenden, empfehlen wir, dass Sie des Excel [Excel-add-Ins mithilfe](machine-learning-excel-add-in-for-web-services.md).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a>Schritte

Veröffentlichen von einem Webdienst. [Auf dieser Seite](machine-learning-walkthrough-5-publish-web-service.md) wird erläutert, wie erledigen. Aktuell wird das Feature der Excel-Arbeitsmappe nur für Anforderung/Antwort-Dienste unterstützt, die eine einzelne Ausgabe (d. h., ein einzelnes Punktzahl Etikett) aufweisen. 

Sobald Sie einen Webdienst haben, klicken Sie im Abschnitt **Webdienste** auf der linken Seite von Studio auf, und wählen Sie dann auf des Webdiensts aus Excel zu nutzen.

**Klassische Webdienst**

1. Auf dem **DASHBOARD** wird die Registerkarte für den Webdienst einer Zeile für den Dienst **Anforderung/Antwort** . Wenn dieser Dienst eine einzelne Ausgabe hatten, sollte den **Excel-Arbeitsmappe herunterladen** Link in dieser Zeile angezeigt werden.

    ![][1]

2. Klicken Sie auf **Excel-Arbeitsmappe herunterladen**.

**Neuen Webdienst**

1. Wählen Sie im Portal Azure maschinellen Learning-Webdiensts **verbrauchen**.
2. Klicken Sie auf der Seite im Abschnitt **Web Verbrauch Dienstoptionen** verbrauchen auf das Excel-Symbol.

**Verwendung der Arbeitsmappe**

1. Öffnen Sie die Arbeitsmappe ein.

2. Eine Sicherheitswarnung wird angezeigt. Klicken Sie auf die Schaltfläche **Bearbeiten aktivieren** .

    ![][2]

3. Eine Sicherheitswarnung angezeigt wird. Klicken Sie auf die Schaltfläche **Inhalt aktivieren** , zum Ausführen von Makros in der Arbeitsmappe.

    ![][3]
4. Nachdem Makros aktiviert sind, wird eine Tabelle generiert. Spalten in Blau sind, die als Eingabe für die RRS Webdienst oder **Parameter**erforderlich ist. Beachten Sie die Ausgabe der Dienst RRS **REGRESSIONSGLEICHUNG Werte** in Grün. Wenn alle Spalten für eine bestimmte Zeile ausgefüllt wurden, die Arbeitsmappe automatisch Punktzahl API aufgerufen, und zeigt die bewertete Ergebnisse.

    ![][4]

5. Wenn Sie um mehr als eine Zeile zu erreichen, Ausfüllen der zweiten Zeile mit Daten und die geschätzte Werte Werkzeuge hergestellt werden Sie können sogar mehrere Zeilen gleichzeitig einfügen.

Die Excel-Features (Diagramme, Power Map, bedingte Formatierung usw.) können mit den geschätzten Werten Sie um die Daten zu visualisieren.    


## <a name="sharing-your-workbook"></a>Freigeben einer Arbeitsmappe

Für die Makros arbeiten können muss der API-Schlüssel Teil der Kalkulationstabelle. Dies bedeutet, dass Sie die Arbeitsmappe nur mit Personen/Einzelpersonen freigeben sollte, denen Sie vertrauen.

## <a name="automatic-updates"></a>Automatische updates

In beiden Fällen wird die RRS aufgerufen:

1. Das erste Mal ist eine Zeile Inhalt im sämtliche **Parameter**

2. Bei jeder Änderung einer der **Parameter** Änderung in einer Zeile, die sämtliche **Parameter** eingegeben hatten.

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
