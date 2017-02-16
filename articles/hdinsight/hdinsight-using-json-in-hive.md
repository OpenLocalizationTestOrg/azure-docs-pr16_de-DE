<properties
   pageTitle="Analysieren und Prozess JSON-Dokumente mit Struktur in HDInsight | Microsoft Azure"
   description="Informationen zum Verwenden von JSON-Dokumente und Analysieren der Antworten Struktur in HDInsight verwenden."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Verarbeiten und Analysieren von JSON-Dokumente mit Struktur in HDInsight

Informationen Sie zum Verarbeiten und Analysieren von JSON-Dateien, die mit der Struktur in HDInsight. Das folgende JSON-Dokument wird im Lernprogramm verwendet werden

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Die Datei finden Sie unter wasbs://processjson@hditutorialdata.blob.core.windows.net/. Weitere Informationen zum Verwenden von Azure Blob-Speicher mit HDInsight finden Sie unter [verwenden HDFS kompatiblen Azure Blob-Speicher mit Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md). Wenn Sie möchten, können Sie die Datei in den standardmäßigen Container Ihren Cluster kopieren.

In diesem Lernprogramm verwenden Sie die Struktur Konsole.  Öffnen der Konsole Struktur Anweisungen finden Sie unter [Verwenden Struktur mit Hadoop auf HDInsight mit Remotedesktop](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Reduzieren von JSON-Dokumente

Im nächsten Abschnitt aufgeführten Methoden erfordern das JSON-Dokument in einer einzelnen Zeile an. Daher müssen Sie das JSON-Dokument in eine Zeichenfolge zusammenfassen. Wenn Ihr Dokument JSON bereits vereinfacht ist, können Sie diesen Schritt überspringen und wechseln Sie auf Analysieren von JSON-Daten direkt mit dem nächsten Abschnitt.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Unformatierte JSON-Datei befindet sich unter **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. Die *StudentsRaw* strukturtabelle verweist auf die unformatierten dauerhaften flache JSON-Dokument.

Die *StudentsOneLine* strukturtabelle werden die Daten im Dateisystem HDInsight Standard unter den Pfad ** /json/Kursteilnehmer/speichern.

Die INSERT-Anweisung füllen Sie die StudentOneLine-Tabelle mit den flache JSON-Daten.

Die SELECT-Anweisung wird nur 1 Zeile zurück.

So sieht das Ergebnis der SELECT-Anweisung aus:

![Reduzieren der JSON-Dokuments ein.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>Analysieren von JSON-Dokumenten in die Struktur

Struktur bietet drei verschiedene Arten, um Abfragen auf JSON-Dokumenten ausgeführt werden:

- Verwenden Sie die erste\_JSON\_Objekt UDFs (benutzerdefinierte Funktion)
- Verwenden des JSON_TUPLE UDFs
- Verwenden von benutzerdefinierten SerDe
- Schreiben Sie, dass Sie UDFs mit Python oder anderen Sprachen besitzen. [In diesem Artikel] finden Sie unter[ hdinsight-python] zum Ausführen von eigenem Codes Python mit Struktur.

### <a name="use-the-getjsonobject-udf"></a>Verwenden Sie die erste\_JSON_OBJECT UDFs
Struktur bietet die integrierten UDFs [Json-Objekt erhalten](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) die JSON-Abfragen zur Laufzeit ausführen können. Diese Methode akzeptiert zwei Argumente – dem Tabellennamen und den Namen der Methode der weist das flache JSON-Dokument und das JSON-Feld, das analysiert werden muss. Sehen wir uns ein Beispiel zu sehen, wie diese UDFs funktioniert.

Abrufen von vor- und Nachnamen für jeden Kursteilnehmer

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Hier ist die Ausgabe beim Ausführen dieser Abfrage Console-Fenster.

![Get_json_object UDFs][image-hdi-hivejson-getjsonobject]

Es gibt ein paar Schwächen UDFs Get-Json_object aus.

- Da jedes Feld in der Abfrage erfordert eine Analyse erneut auf die Abfrage, wirkt sich diese auf die Leistung.
- Abrufen von\_JSON_OBJECT() gibt die Zeichenfolge einer Matrix zurück. Um diesen Wert in einem Array Struktur konvertieren, müssen Sie reguläre Ausdrücke zu verwenden, um die eckigen Klammern ersetzen ' [' und ']' und dann zum Abrufen des Arrays geteilten auch aufrufen.


Dies ist, warum das Struktur Wiki empfiehlt die Verwendung von Json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Verwenden des JSON_TUPLE UDFs

Eine andere UDFs von Struktur bereitgestellten heißt [Json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) die besser als [Get_ Json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object)ausführt. Diese Methode akzeptiert eine Reihe von Tasten und eine JSON-Zeichenfolge und ein Tupels von Werten mit einer Funktion gibt. Die folgende Abfrage gibt die Studenten-Id und die Noten aus dem JSON-Dokument an:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Die Ausgabe dieses Skripts in der Struktur-Verwaltungskonsole:

![Json_tuple UDFs][image-hdi-hivejson-jsontuple]

JSON\_TUPELS verwendet die Syntax der [seitliche Ansicht](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) in die Struktur der Json kann\_Tupels aus, um eine virtuelle Tabelle erstellen, indem Sie die Funktion UDT in jeder Zeile der ursprünglichen Tabelle anwenden.  Komplexe JSONs werden durch die wiederholte Verwendung von SEITLICHE Ansicht zu schwerfällig. Darüber hinaus kann nicht JSON_TUPLE geschachtelte JSONs behandeln.


###<a name="use-custom-serde"></a>Verwenden von benutzerdefinierten SerDe

SerDe ist die beste Wahl für die Analyse verschachtelte JSON-Dokumente, die es ermöglicht Ihnen, das JSON-Schema zu definieren, und das Schema verwenden, um die Dokumente zu analysieren. In diesem Lernprogramm wird die Beliebtheit SerDe Verwendung einer, die durch [Rcongiu](https://github.com/rcongiu)entwickelt wurde.

**So verwenden Sie die benutzerdefinierten SerDe**

1. [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR)zu installieren. Wählen Sie die Version Windows X64 JDK, wenn Sie beabsichtigen, die Windows-Bereitstellung von HDInsight verwenden

    >[AZURE.WARNING] JDK 1.8 funktioniert nicht mit diesem SerDe.

    Nach Abschluss die Installation, fügen Sie eine neue Benutzer-Umgebungsvariable hinzu:

    1. Öffnen Sie auf dem Windows-Bildschirm **Erweiterte Systemeinstellungen anzeigen** .
    2. Klicken Sie auf **Umgebungsvariablen**.  
    3. Fügen Sie eine neue **JAVA_HOME** Umgebungsvariablen zeigt **C:\Programme Files\Java\jdk1.7.0_55** oder wo Ihre JDK installiert ist hinzu.

    ![Einrichten der richtigen Config Werte für JDK][image-hdi-hivejson-jdk]

2. Installieren von [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    Bin-Ordner auf Ihrem Pfad Steuerelement Anzeigebereich durch Hinzufügen Systemsteuerung--> Bearbeiten die Variablen System für Ihr Konto Environment Variablen. Das Bildschirmabbild unten zeigt Sie, wie Sie dies tun.

    ![Einrichten von Maven][image-hdi-hivejson-maven]

3. Klonen des Projekts aus [Struktur-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) Github Website an. Dies möglich, indem Sie auf die Schaltfläche "Herunterladen eines Zip", wie im folgenden Screenshot dargestellt.

    ![Klonen des Projekts][image-hdi-hivejson-serde]

4: Navigieren Sie zu dem Ordner, in dem Typ "Mvn Paket" und dem Paket heruntergeladen haben. Dies sollte die notwendigen JAR-Dateien erstellen, die Sie dann über dem Cluster kopieren können.

5: Wechseln Sie zum Zielordner unterhalb des Stammordners, in dem Sie das Paket heruntergeladen haben. Hochladen der Datei json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar Kopf-Knoten der Cluster. In der Regel unter dem Struktur binäre Ordner abgelegt: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin oder ähnlichem.

6: Struktur dazu aufgefordert werden, geben Sie "JAR-/path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar hinzufügen". Da in meinem Fall JAR-Datei im Ordner "C:\apps\dist\hive-0.13.x\bin" ist, kann ich die JAR-Datei mit dem Namen direkt hinzufügen, wie unten dargestellt:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Jetzt sind Sie bereit sind, verwenden Sie den SerDe Abfragen für das JSON-Dokument ausführen.

Die folgende Anweisung wird eine Tabelle mit einem definierten Schema erstellen.

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Listen Sie vor- und Nachnamen des Studenten

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

So sieht das Ergebnis aus der Struktur Konsole aus.

![SerDe Abfrage 1][image-hdi-hivejson-serde_query1]

Um die Summe der Bewertungen der Lesbarkeit des Dokuments JSON zu berechnen.

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Die oben genannten Abfrage verwendet [seitliche Ansicht Explosion](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDFs, um die Matrix von Ergebnissen zu erweitern, dass er summiert werden können.

Hier ist das Ergebnis aus der Struktur Konsole aus.

![SerDe Abfrage 2][image-hdi-hivejson-serde_query2]

Wählen Sie eine Zusammenfassung die Themen einer angegebenen-Student mehr als 80 Punkte bewertet hat finden  
      JT StudentClassCollection.ClassId aus Json_table Jt seitliche Ansicht Explosion (JT StudentClassCollection.Score) Auflistung als Punktzahl, wo Punktzahl > 80;

Die obige Abfrage gibt ein Array Struktur im Gegensatz zu Get\_Json\_Objekt gibt eine Zeichenfolge zurück.

![SerDe Abfrage 3][image-hdi-hivejson-serde_query3]

Wenn Sie Skil möchten fehlerhafte JSON, und klicken Sie dann wie auf der [Wiki-Seite](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) dieses SerDe erläutert können Sie die erzielen, indem Sie den folgenden Code eingeben:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Zusammenfassung
Der Typ des JSON-Operators in die Struktur, die Sie auswählen, hängt in Ihrem Szenario. Wenn Sie ein einfaches JSON-Dokument haben und Sie müssen nur ein Feld zum Nachschlagen auf – Sie können auch die Struktur UDFs Get verwenden\_Json\_Objekt. Wenn Sie mehr als eine Taste auf gesucht haben, können Sie Json_tuple verwenden. Wenn Sie ein geschachteltes Dokument haben, sollten Sie das JSON-SerDe verwenden.

Eine Liste weiterer Artikel finden Sie unter

- [Verwenden von Struktur und HiveQL mit Hadoop in HDInsight zum Analysieren einer Apache log4j-Beispieldatei](hdinsight-use-hive.md)
- [Analysieren Sie mithilfe der Struktur in HDInsight Verzögerung Flugdaten](hdinsight-analyze-flight-delay-data.md)
- [Analysieren von Twitter-Daten, die mithilfe der Struktur in HDInsight](hdinsight-analyze-twitter-data.md)
- [Führen Sie einen Hadoop Auftrag mithilfe von DocumentDB und HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
