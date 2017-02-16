<properties
   pageTitle="Microsoft Power BI eingebettete - Herstellen einer Verbindung mit einer Datenquelle"
   description="Power BI eingebettete, eine Verbindung mit Datenquellen herstellen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Verbinden Sie mit einer Datenquelle

Mit **Power BI eingebettete**können Sie Berichte in Ihre Anwendung einbetten. Wenn Sie einen Power BI-Bericht in der app einbetten, verbindet der Bericht mit der zugrunde liegenden Daten durch **Importieren** eine Kopie der Daten oder indem Sie mit der Datenquelle mithilfe der **DirectQuery** **direkt verbinden** .

Hier sind die Unterschiede zwischen der Verwendung **Importieren** und **DirectQuery**aus.

|Importieren | DirectQuery
|---|---
|Tabellen, Spalten *und Daten* werden importiert oder in des Berichts Dataset kopiert. Wenn Änderungen anzeigen möchten, die auf die zugrunde liegenden Daten aufgetreten sind, müssen Sie aktualisieren oder erneut ein abgeschlossen, aktuelle Dataset zu importieren.|Nur die *Tabellen und Spalten* werden importiert oder in des Berichts Dataset kopiert. Sie anzeigen immer von Daten auf dem neuesten Stand.
Mit Power BI eingebettete Sie können DirectQuery mit Cloud-Datenquellen verwenden, aber nicht auf lokale Datenquellen zu diesem Zeitpunkt.

## <a name="benefits-of-using-directquery"></a>Vorteile von DirectQuery

Es gibt zwei Hauptvorteile bei Verwendung von **DirectQuery**:

   -    **DirectQuery** können Sie zum Erstellen von Visualisierungen mit sehr großen Datasets, wo sie andernfalls zum ersten Importvorgangs nicht realisierbar wäre aller Daten.
   -    Zugrundeliegenden Daten Änderungen können Sie eine Aktualisierung von Daten erforderlich, und für einige Berichte kann die aktuellen Daten anzeigen müssen große Datenübertragung, wodurch erneut importierende Daten nicht realisierbar erforderlich. Im Gegensatz dazu Berichte **DirectQuery** immer aktuellen Daten verwenden.

## <a name="limitations-of-directquery"></a>Schwächen DirectQuery

   Es bestehen einige Einschränkungen bei der Verwendung von **DirectQuery**:

   -    Alle Tabellen müssen aus einer einzelnen Datenbank stammen.
   -    Wenn die Abfrage zu komplex ist, tritt ein Fehler auf. Um den Fehler zu beheben müssen Sie die Abfrage gestalten, damit er weniger komplex ist. Wenn die Quuery komplexe sein muss, müssen Sie die Daten anstelle von **DirectQuery**zu importieren.
   -    Filtern der Beziehung ist auf beide Richtungen, statt einer Richtung beschränkt.
   -    Sie können den Datentyp einer Spalte nicht ändern.
   -    Standardmäßig befinden sich die Einschränkungen für DAX-Ausdrücke in Measures zulässig sind. Finden Sie unter [DirectQuery und Measures](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery und measures

Um sicherzustellen, dass die Abfragen gesendet, um die zugrunde liegende Datenquelle ausreichende Leistung haben, werden die Einschränkungen auf Measures auferlegt. Verwenden von **Power BI-Desktop**, fortgeschrittene Benutzer beim auswählen kann, umgehen dieser Einschränkung durch auswählen **Datei > Optionen und Einstellungen > Optionen**. Klicken Sie im Dialogfeld **Optionen** wählen Sie **DirectQuery**, und wählen Sie die Option **Zulassen der uneingeschränkter Measures im DirectQuery-Modus**. Wenn diese Option ausgewählt ist, kann ein DAX-Ausdruck, der für ein Measure gilt verwendet werden. Benutzer müssen sich bewusst sein; jedoch können, dass einige Ausdrücke, die sehr gut auszuführen, wenn die Daten importiert werden sehr langsam Abfragen an die Back-End-Quelle im **DirectQuery** -Modus führen. 

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Microsoft Power BI eingebettete](power-bi-embedded-get-started.md)
- [Power BI-Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
