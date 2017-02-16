<properties
    pageTitle="Beispieldaten in Tabellen Azure HDInsight Struktur | Microsoft Azure"
    description="Nach unten Stichproben von Daten in Azure HDInsight (Hadopop) Struktur Tabellen"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Beispieldaten in Tabellen Azure HDInsight Struktur

In diesem Artikel beschrieben wir unten Stichproben in Azure HDInsight Struktur Tabellen mit Struktur Abfragen gespeicherte Daten. Drei allgemein verwendeten werden Methoden Fragestellungen:

* Uniform Stichproben
* Stichproben nach Gruppen
* Stratified werden

**Warum Beispieldaten?**
Wenn das Dataset aus, den, das Sie analysieren möchten, groß ist, ist es normalerweise eine gute Idee, die Daten, um es auf eine kleinere aber tragen und überschaubarer Größe reduzieren unten Stichproben. Dadurch erleichtert Daten Grundlegendes zu, damit arbeiten und technisch Feature. Seine Rolle im Team Daten Wissenschaft Prozess besteht darin schnelle Prototypen Datenverarbeitung Funktionen und Computer learning Modelle ermöglichen.

Klicken Sie im **Menü** unten Links zu Themen, für die Stichprobe Daten aus verschiedenen Speicher-Umgebungen beschrieben.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Diese Aufgabe werden ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>So senden Struktur Abfragen
Über die Befehlszeile Hadoop-Konsole auf dem am Knoten des Cluster Hadoop können Struktur Abfragen gesendet werden. Dazu melden Sie sich bei der am Knoten im Cluster Hadoop, öffnen Sie die Befehlszeile Hadoop-Verwaltungskonsole, und übermitteln Sie die Struktur Abfragen von dort aus. Anweisungen Struktur Abfragen in der Verwaltungskonsole Hadoop Befehlszeile übermitteln finden Sie unter [Struktur Abfragen absenden](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="a-nameuniforma-uniform-random-sampling"></a><a name="uniform"></a>Uniform Stichproben
Uniform Stichproben bedeutet, dass jede Zeile in der Datenmenge ein gleich Wahrscheinlichkeit als Stichprobe verwendet. Dies kann durch Hinzufügen von eine zusätzliche Feld Zufallszahl () in der Datenmenge in die innere "select" Abfrage, und klicken Sie in der äußeren "select" Abfrage die Bedingung auf dieses Feld zufällige implementiert werden.

Hier ist eine Beispiel für eine Abfrage:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Hier `<sample rate, 0-1>` gibt an, das Verhältnis von Datensätzen, die die Benutzer aufnehmen möchten.

## <a name="a-namegroupa-random-sampling-by-groups"></a><a name="group"></a>Stichproben nach Gruppen

Wenn kategorisierten Daten werden, sollten Sie entweder ein-oder Ausschließen von allen Instanzen von einigen bestimmten Wert einer kategorisierten Variablen. Dies ist, was mit "werden nach Gruppen" gemeint ist.
Beispielsweise wenn Sie eine kategorisierte Variable "Status" haben, weist die Werte NY, MA, CA, NJ, PA usw., Datensätze zu demselben Zustand immer zusammen, werden, ob sie oder nicht als Stichprobe verwendet werden.

Hier ist eine Beispiel für eine Abfrage die Beispiele nach Gruppen:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="a-namestratifiedastratified-sampling"></a><a name="stratified"></a>Stratified werden

Stichproben ist in Bezug auf eine kategorisierte Variable stratified, wenn die Beispiele für Ihren Kunden-Werte enthalten, dass Kategorieliste, die in das Verhältnis wie die übergeordnete Population werden aus dem in den Beispielen abgerufen wurden. Verwenden das gleiche Beispiel wie obigen nehmen Sie an, dass Ihre Daten Staaten untergeordnete Bevölkerung hat, Angenommen Sie NJ hat 100 Beobachtungen, NY weist 60 Beobachtungen und WA 300 Beobachtungen. Wenn Sie die Anzahl der stratified werden 0,5 sein, die angeben, sollte die Stichprobe abgerufen ungefähr 50, 30 und 150 Beobachtung der NJ, NY und WA Hilfethemas haben.

Hier ist eine Beispiel für eine Abfrage:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Informationen zu erweiterte werden Methoden, die in die Struktur verfügbar sind, finden Sie unter [LanguageManual werden](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
