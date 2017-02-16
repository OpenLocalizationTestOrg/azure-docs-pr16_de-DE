<properties
   pageTitle="Filtern von Zeichenfolgen für den Tabellen-Designer bauen | Microsoft Azure"
   description="Erstellen von Filterzeichenfolgen für den Tabellen-designer"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Erstellen von Filterzeichenfolgen für den Tabellen-Designer

## <a name="overview"></a>(Übersicht)

Zum Filtern von Daten in einer Azure-Tabelle, die in Visual Studio- **Tabellen-Designer**angezeigt wird, wenn Sie eine Filterzeichenfolge erstellen und geben Sie ihn in das Feld "Filter". Die Syntax der Filters Zeichenfolge wird durch die WCF Data Services definiert und ist vergleichbar mit einer SQL-WHERE-Klausel, jedoch zum Dienst Tabelle über eine HTTP-Anforderung gesendet wird. **Tabellen-Designer** verarbeitet die richtige Codierung für Sie also zum Filtern nach einem gewünschten Eigenschaftswert, geben Sie benötigen nur Boolesch Eigenschaftenname, Vergleichsoperator, Kriterienwert sowie optional Operator in das Feld "Filter". Sie müssen nicht die Option $filter Abfrage aufnehmen möchten, wie Sie vorgehen, wenn Sie einen URL aus, um die Tabelle über den [Speicherplatz Services REST-API-Referenz](http://go.microsoft.com/fwlink/p/?LinkId=400447)Abfragen bauen wurden.

Der WCF Data Services basieren auf dem [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Details auf die Option Filter System Abfrage (**$filter**) finden Sie unter der [OData-URI-Konventionen Spezifikation](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Vergleichsoperatoren

Die folgenden logischen Operatoren werden für alle Typen von unterstützt:

|Logische Operatoren|Beschreibung|Beispiel für Filter-Zeichenfolge|
|---|---|---|
|EQ|Gleich|Ort Eq 'Redmond'|
|gt|Größer als|Kurs Gt 20|
|Seite|Größer als oder gleich|Kurs Seite 10|
|lt|Kleiner als|Kurs Lt 20|
|LE|Kleiner oder gleich|Kurs le 100|
|Neuer|Ungleich|Ort neuer 'London'|
|und|Und|Kurs le 200 und Preis Gt 3.5|
|oder|Oder|Kurs le 3.5 oder Kurs Gt 200|
|nicht|Nicht|nicht isAvailable|

Beim Erstellen einer Filterzeichenfolge, werden die folgenden Regeln:

- Verwenden Sie die logischen Operatoren, um eine Eigenschaft auf einen anderen Wert zu vergleichen. Beachten Sie, dass es nicht möglich, eine Eigenschaft auf einen dynamischen Wert verglichen werden soll. eine Seite des Ausdrucks muss eine Konstante sein.

- Alle Teile der Filterzeichenfolge Groß-/Kleinschreibung.

- Konstante Wert müssen den gleichen Datentyp wie die Eigenschaft in der Reihenfolge für den Filter um gültige Ergebnisse zurückzugeben. Weitere Informationen zu unterstützten Eigenschaft Datentypen finden Sie unter [Grundlegendes zu Tabelle Dienst Datenmodell](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Klicken Sie auf Eigenschaften von Zeichenfolge filtern

Wenn Sie auf Zeichenfolgeneigenschaften filtern, setzen Sie die Zeichenfolgenkonstante in einfache Anführungszeichen.

Im folgenden Beispiel Filter auf die Eigenschaften **PartitionKey** und **RowKey** ; zusätzliche Schlüsselfelder Eigenschaften konnten auch mit der Filterzeichenfolge hinzugefügt werden:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Sie können jedes Filter-Ausdruck in Klammern setzen, auch wenn es nicht erforderlich ist:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Beachten Sie, dass die Tabelle Service keine Platzhalterzeichen Abfragen unterstützt, und sie nicht im Tabellen-Designer entweder unterstützt werden ein. Allerdings können Sie mithilfe von Vergleichsoperatoren auf das gewünschte Präfix übereinstimmenden Präfix durchführen. Im folgende Beispiel gibt die Elemente, wobei die Eigenschaft Nachname beginnt mit dem Buchstaben "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Klicken Sie auf numerischen Eigenschaften filtern

Geben Sie die Zahl ohne Anführungszeichen, um auf eine ganze Zahl oder Gleitkommazahl zu filtern.

In diesem Beispiel gibt alle Elemente mit einer ALTER Eigenschaft, deren Wert größer als 30 ist:

    Age gt 30

In diesem Beispiel gibt alle Elemente mit einer AmountDue Eigenschaft, deren Wert kleiner oder gleich 100.25 ist:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Klicken Sie auf booleschen Eigenschaften filtern

Geben Sie zum Filtern nach einem boolesche Wert **true** oder **false** ohne Anführungszeichen ein.

Im folgende Beispiel gibt alle Personen, die IsActive-Eigenschaft auf **true**festgelegt ist:

    IsActive eq true

Sie können auch diese Filter-Ausdruck ohne logischen Operators schreiben. Im folgenden Beispiel wird der Tabelle Dienst auch alle Personen zurückgegeben, IsActive **Wahr**ist:

    IsActive

Um alle Personen zurückzukehren IsActive false ist, können Sie die nicht Operator:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Klicken Sie auf Eigenschaften von "DateTime" Filtern

Geben Sie im **Datetime** -Schlüsselwort, gefolgt von dem Datum/Uhrzeit-Konstante in einfache Anführungszeichen, um nach einem DateTime-Wert zu filtern. Die Konstante Datum/Uhrzeit muss kombinierte UTC-Format, wie im [DateTime-Immobilienwerte Formatierung](http://go.microsoft.com/fwlink/p/?LinkId=400449)beschrieben.

Im folgende Beispiel gibt die Personen, in dem die Eigenschaft Kunde 10 Juli 2008 gleich ist:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
