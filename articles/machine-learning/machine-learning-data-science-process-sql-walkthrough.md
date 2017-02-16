<properties
    pageTitle="Team von Daten Wissenschaft in Aktion: Verwenden von SQL Server | Microsoft Azure"
    description="Erweiterte Analytics Prozess und Technologie in Aktion"  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Team von Daten Wissenschaft in Aktion: Verwenden von SQL Server

In diesem Lernprogramm Sie Exemplarische Vorgehensweise erstellen und Bereitstellen eines Computer Learning-Modells mithilfe von SQL Server und einer öffentlich zugänglichen Dataset – [NYC Taxi Schleifen](http://www.andresmh.com/nyctaxitrips/) Dataset. Das Verfahren folgt einen Standarddaten Wissenschaft Workflow: Aufnahme und Untersuchen von Daten, Engineering Features zu erleichtern Schulung, und klicken Sie dann erstellen und Bereitstellen eines Modells.


## <a name="a-namedatasetanyc-taxi-trips-dataset-description"></a><a name="dataset"></a>NYC Taxi Reisen Datasetbeschreibung

Die Daten NYC Taxi Geschäftsreise ist ungefähr 20GB komprimierte CSV-Dateien (~ 48GB nicht komprimiert), für jede Reise bezahlt, umfasst mehr als 173 Millionen einzelnen Schleifen und die Flugpreise. Jeder Geschäftsreise Datensatz enthält eine Abholung und Ladengeschäft Ort und Zeit, anonymes Hacker (Treiber) Anzahl Lizenzen und Medallion (einmalige Nr. des Taxi) Zahl. Die Daten umfasst alle Schleifen im Jahr 2013 und werden in den folgenden zwei Datasets für jeden Monat bereitgestellt:

1. Trip_data CSV enthält Geschäftsreise Details an wie die Anzahl der Personen, Abholung und Dropoff Punkte, Geschäftsreise Dauer und Geschäftsreise Länge. Hier sind ein paar Stichprobe Einträge aus:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Die 'Trip_fare' enthält die CSV-Datei Details der für jede Reise, z. B. Zahlungstyp, Fahrpreis Betrag, Aufschlag und steuern, Tipps und Maut-, gezahlten Flugpreis und den Gesamtbetrag. Hier sind ein paar Stichprobe Einträge aus:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Der eindeutige Schlüssel für die Reise Teilnahme an\_Daten und Geschäftsreise\_Fahrpreis besteht aus den Feldern: Medallion, Hack\_Lizenz und Abholverzeichnisse\_"DateTime".

## <a name="a-namemltasksaexamples-of-prediction-tasks"></a><a name="mltasks"></a>Beispiele für Vorhersage Aufgaben

Wir werden drei Vorhersage Probleme basierend auf Formulierung der *Tipp\_Betrag*, nämlich:

1. Binäre Klassifizierung: Vorhersagen, und zwar unabhängig davon, ob ein Tipp einer Reise, d. h. bezahlt wurde ein *Tipp\_Betrag* , die größer ist als $0 eine positive Beispiel ist, während er sich eine *Tipp\_Betrag* $0 ist ein Beispiel für negativen.

2. Multiclass Klassifizierung: den Zellbereich, der für die Reise bezahlt Tipp Vorhersagen. Dividieren von wir die *Tipp\_Betrag* in fünf Papierkörben oder Klassen:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Regression Aufgabe: die Menge der QuickInfo für eine Geschäftsreise gezahlte Vorhersagen.  


## <a name="a-namesetupasetting-up-the-azure-data-science-environment-for-advanced-analytics"></a><a name="setup"></a>Einstellung der Azure Daten Wissenschaft Umgebung für erweiterte analytics

Wie Sie zwischen der Führungslinie [Planen Ihrer Umgebung](machine-learning-data-science-plan-your-environment.md) sehen können, gibt es mehrere Optionen für die Arbeit mit NYC Taxi Reisen Dataset in Azure:

- Arbeiten Sie mit Daten in Azure Blobs und Modell Azure Computer interessante
- Laden Sie die Daten in einer SQL Server-Datenbank und Modell Azure Computer interessante

In diesem Lernprogramm erfahren wir parallele Massenimport von Daten zu einem SQL Server Durchsuchen von Daten, Feature Technik und nach unten werden mithilfe von SQL Server Management Studio sowie IPython Notizbuch verwenden. [Beispiel für Skripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) und [IPython Notizbücher](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) sind in GitHub freigegeben. Ein Beispiel für IPython Notizbuch für die Arbeit mit den Daten in Azure Blobs steht auch an derselben Stelle.

So richten Sie Ihre Daten für Wissenschaft Azure-Umgebung

1. [Erstellen Sie ein Speicherkonto](../storage/storage-create-storage-account.md)

2. [Erstellen eines Arbeitsbereichs Azure maschinellen Schulung](machine-learning-create-workspace.md)

3. [Bereitstellen einer Daten Wissenschaft virtuellen Computern](machine-learning-data-science-setup-sql-server-virtual-machine.md), die als SQL Server sowie einen IPython Notizbuch Server dienen soll.

    > [AZURE.NOTE] Skripts und IPython Notizbücher werden auf Ihre Daten Wissenschaft virtuellen Computern während des Setupprozesses heruntergeladen werden. Beim Abschluss des virtuellen Computer nach der Installation Skripts werden in den Beispielen in Ihrer virtuellen Computers Dokumentbibliothek:  
    > - Beispiel für Skripts:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Beispiel für IPython Notizbücher:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > wo `<user_name>` Ihrer virtuellen Computers Windows Anmeldename ist. Wir bezieht sich auf die Stichprobe Ordner als **Skripts** und **Stichprobe IPython Notizbücher**.


Basierend auf die Größe des Datasets, Speicherort der Datenquelle und den ausgewählten Azure zielumgebung, dieses Szenario ähnelt dem [Szenario \#5: großes Dataset in eine lokale Dateien, SQL Server auf virtuellen Azure-Computer als Ziel](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="a-namegetdataaget-the-data-from-public-source"></a><a name="getdata"></a>Abrufen von Daten aus öffentlichen Datenquelle

Um das Dataset [NYC Taxi Schleifen](http://www.andresmh.com/nyctaxitrips/) von ihrem öffentlichen Speicherort zu gelangen, können Sie eines der in [Verschieben von Daten zwischen Azure BLOB-Speicher](machine-learning-data-science-move-azure-blob.md) beschriebenen Verfahren zum Kopieren der Daten zu Ihren neuen PC verwenden.

So kopieren Sie die Daten mithilfe von AzCopy:

1. Melden Sie sich bei Ihrem virtuellen Computern (virtueller Computer)

2. Erstellen Sie ein neues Verzeichnis in Datenträger des virtuellen Computers (Notiz: Verwenden Sie nicht die temporären Datenträger, die mit dem virtuellen Computer als einen Datenträger stammt).

3. Führen Sie die folgende Azcopy Befehlszeile, ersetzen < Path_to_data_folder > mit Ordners Daten in (2) erstellt in ein Eingabeaufforderungsfenster:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Klicken Sie nach Abschluss der AzCopy ZIP insgesamt 24 CSV-Dateien (12 für Geschäftsreise\_Daten und 12 für Geschäftsreise\_Fahrpreis) sollten im Ordner "Daten".

4. Entzippen Sie die heruntergeladenen Dateien ein. Beachten Sie den Ordner, in denen die nicht komprimierten Dateien befinden. Diesen Ordner werden nachstehend die < Pfad\_auf\_Daten\_Dateien\>.

## <a name="a-namedbloadabulk-import-data-into-sql-server-database"></a><a name="dbload"></a>Massen Importieren von Daten in SQL Server-Datenbank

Die Leistung von laden/übertragen große Datenmengen mit einer SQL-Datenbank und nachfolgende Abfragen kann mithilfe von _Tabellen aufgeteilt und Ansichten_verbessert werden. In diesem Abschnitt werden wir die Anweisungen in [Parallele Massen Daten importieren mithilfe von SQL Partitionstabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) , um eine neue Datenbank erstellen, und Laden Sie die Daten in partitionierten Tabellen parallel beschrieben.

1. Während in Ihrer virtuellen Computer angemeldet, starten Sie **SQL Server Management Studio**aus.

2. Verbinden Sie mit Windows-Authentifizierung.

    ![SSMS verbinden][12]

3. Wenn Sie noch nicht geändert, den SQL Server-Authentifizierungsmodus und einen neuen SQL-Benutzer erstellt haben, öffnen Sie die Skriptdatei namens **Ändern\_auth.sql** in den Ordner **Skripts Stichprobe** . Ändern Sie die Standard-Benutzernamen und das Kennwort ein. Klicken Sie auf **! Führen Sie** in der Symbolleiste, um das Skript auszuführen.

    ![Führen Sie Skript][13]

4. Überprüfen Sie und/oder ändern Sie die SQL Server-Datenbank und der Protokolldateien Standardordner um sicherzustellen, die neu erstellten Datenbanken in einen Datenträger gespeichert werden sollen. Das Bild von SQL Server virtueller Computer, das für Data Warehousing lädt optimiert ist ist vorkonfiguriert mit Daten und der Protokolldateien Festplatten. Wenn Ihre virtuellen Computer haben einen Datenträger nicht eingeschlossen, und Sie neuen virtuellen Festplatten während des Setupprozesses virtueller Computer hinzugefügt haben, ändern Sie die Standardordner wie folgt:

    - Mit der rechten Maustaste im linken Bereich des SQL Server-Namens, und klicken Sie auf **Eigenschaften**.

        ![SQL Server-Eigenschaften][14]

    - Wählen Sie **Datenbank-Einstellungen** aus der Liste **Wählen Sie eine Seite** nach links.

    - Überprüfen Sie und/oder ändern Sie die **Datenbank Standardspeicherorte** an die Speicherorte **Der Daten** Ihrer Wahl. Dies ist die Stelle, an der neue Datenbanken gespeichert sind, wenn mit den Speicherort Standardeinstellungen erstellt.

        ![SQL-Datenbank Standardeinstellungen][15]  

5. Um eine neue Datenbank und eine Reihe von Dateigruppen zu halten Sie die partitionierten Tabellen zu erstellen, öffnen Sie das Beispielskript **Erstellen\_Db\_default.sql**. Das Skript erstellt eine neue Datenbank mit dem Namen **TaxiNYC** und 12 Dateigruppen am Standardspeicherort für die Daten. Jede Dateigruppe enthält einen Monat Reise\_Daten und Geschäftsreise\_Ergebnisse so ausfallen Daten. Ändern Sie bei Bedarf den Datenbanknamen. Klicken Sie auf **! Führen Sie** zum Ausführen des Skripts.

6. Erstellen Sie dann zwei Partitionstabellen, eine für die Reise\_Daten und eine andere für die Reise\_Fahrpreis. Öffnen Sie das Skript Stichprobe **Erstellen\_partitionierten\_speichern**, welche wird:

    - Erstellen einer Partitionsfunktion, um die Daten nach Monat zu teilen.
    - Erstellen eines Partitionsschemas Daten des Monats zu einer anderen Dateigruppe zuordnen.
    - Erstellen von zwei partitionierten Tabellen Partitionsschema zugeordneten: **Nyctaxi\_Geschäftsreise** wird die Reise halten\_Daten und **Nyctaxi\_Fahrpreis** wird die Reise halten\_Ergebnisse so ausfallen Daten.

    Klicken Sie auf **! Führen Sie** führen Sie das Skript und die partitionierten Tabellen erstellen.

7. Es gibt zwei Stichproben PowerShell-Skripts zur Verfügung gestellt, um zu veranschaulichen, dass parallele Massen von Daten in SQL Server-Tabellen importiert, in den Ordner **Skripts** .

    - **Bcp\_parallele\_generic.ps1** ist ein allgemeines Skript zu parallele Massen Importieren von Daten in einer Tabelle. Ändern Sie dieses Skript, um die Eingabe- und Ziel Variablen festzulegen, wie in den Kommentarzeilen in das Skript angegeben.
    - **Bcp\_parallele\_nyctaxi.ps1** ist eine vorkonfigurierte Version des generische Skripts und zum Laden der beiden Tabellen für die Daten NYC Taxi Schleifen zu verwendet werden können.  

8. Mit der rechten Maustaste im **Bcp\_parallele\_nyctaxi.ps1** Skripts Namen, und klicken Sie auf **Bearbeiten** , um es in der PowerShell zu öffnen. Die vordefinierten Variablen überprüfen und ändern Sie entsprechend der ausgewählten Datenbankname, Eingabedaten Ordner, Log Zielordner und Pfade für die Stichprobe Format Dateien **nyctaxi_trip.xml** und **Nyctaxi\_fare.xml** (in den Ordner **Stichprobe Skripts** bereitgestellt).

    ![Massen Importieren von Daten][16]

    Sie können auch den Authentifizierungsmodus auswählen, standardmäßig ist die Windows-Authentifizierung. Klicken Sie auf den grünen Pfeil in der Symbolleiste auf ausführen. Das Skript startet 24 Massen Importvorgänge in parallelen, 12 für jede partitionierte Tabelle. Sie können den Fortschritt der Daten Importieren Öffnen den SQL Server-Standardordner Daten gemäß oben überwachen.

9. Die Start- und Endzeiten das PowerShell-Skript-Berichte. Wenn alle Importe abgeschlossen gruppenweise, wird die Endzeit gemeldet. Kontrollkästchen Log Zielordner, stellen Sie sicher, dass das gleichzeitige importiert wurden erfolgreich ist, d. h., kein Fehler gemeldet im Zielordner Protokoll.

10. Die Datenbank kann nun für datenauswertung, Feature technisch und andere Operationen wie gewünscht. Da die entsprechend die Tabellen aufgeteilt werden die **Abholung\_Datetime** Feld Abfragen das einschließen **Abholung\_Datetime** Bedingungen in der **WHERE** -Klausel werden das Partitionsschema nutzbringend.

11. Durchsuchen Sie das bereitgestellten Beispiel-Skript in **SQL Server Management Studio** **Beispiel\_queries.sql**. Wenn die Beispiele für Abfragen ausführen möchten, markieren Sie die Abfragezeilen aus, und klicken Sie auf **! Führen Sie** in der Symbolleiste.

12. Die NYC Taxi Schleifen Daten werden in zwei getrennten Tabellen geladen. Zur Verbesserung der Join-Operationen ist es dringend empfohlen, die Tabellen indiziert. Das Beispielskript **Erstellen\_partitionierten\_index.sql** partitionierten Indizes zusammengesetzte Join-Schlüssels erstellt **Medallion, Hack\_Lizenz und Abholverzeichnisse\_Datetime**.

## <a name="a-namedbexploreadata-exploration-and-feature-engineering-in-sql-server"></a><a name="dbexplore"></a>Durchsuchen von Daten und Features technisch in SQLServer

In diesem Abschnitt führen wir Durchsuchen von Daten und Features der ersten Generation, indem Sie die SQL-Abfragen direkt in der zuvor erstellten SQL Server-Datenbank mit **SQL Server Management Studio** ausgeführt. Ein Beispiel für Skript namens **Beispiel\_queries.sql** in den Ordner **Stichprobe Skripts** bereitgestellt wird. Ändern Sie das Skript zum Ändern des Datenbanknamens aus, wenn sie von der Standardeinstellung abweicht: **TaxiNYC**.

In dieser Übung werden folgende Themen erläutert:

- Verbinden Sie mit **SQL Server Management Studio** , verwenden entweder Windows-Authentifizierung oder SQL-Authentifizierung und den SQL-Benutzernamen und das Kennwort verwenden.
- Untersuchen von Daten Verteilung einige Felder in unterschiedlichen Zeitfenster.
- Ermitteln Sie Daten für Quality of Felder Länge und Breite ein.
- Generieren binäre und multiclass Klassifizierung Etiketten auf der Grundlage der **Tipp\_Betrag**.
- Generieren von Features und Abstände Geschäftsreise berechnen/vergleichen.
- Verknüpfen Sie die beiden Tabellen und extrahieren Sie zufällige zu eine Stichprobe, die zur Erstellung von Datenmodellen verwendet werden.

Wenn Sie zu Azure Computer lernen fortfahren möchten, können Sie:  

1. Speichern Sie die endgültige SQL-Abfrage zum Extrahieren der Beispieldaten und die Abfrage direkt in einer [Importierten Daten] kopieren und Einfügen[ import-data] Modul Azure Computer interessante, oder
2. Beibehalten der aufgenommenen und Engineering Daten, die Sie verwenden möchten, verwenden Sie für das Modell in eine neue Datenbanktabelle erstellen und die neue Tabelle in den [Daten importieren] [ import-data] Modul Azure Computer interessante.

In diesem Abschnitt werden wir die fertige Abfrage zum Extrahieren und die Beispieldaten speichern. Die zweite Methode wird im Abschnitt [Durchsuchen von Daten und Features Engineering IPython Notizbuch](#ipnb) veranschaulicht.

Zum schnellen Überprüfen der Anzahl von Zeilen und Spalten in Tabellen mithilfe einer früheren Version auf parallele Massenimport gefüllt,

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Damit arbeiten: Geschäftsreise Verteilung nach medallion

In diesem Beispiel wird die Medallion (Taxi Zahlen) mit mehr als 100 Reisen innerhalb eines bestimmten Zeitraums. Die Abfrage würde der partitionierten Tabelle des Zugriffs profitieren, da es nach dem Partitionsschema der bedingte ist **Abholung\_Datetime**. Vollständige Dataset Abfragen werden auch Stellen verwenden der partitionierten Tabelle und/oder das Scan indizieren.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Damit arbeiten: Geschäftsreise Verteilung nach Medallion und hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Beurteilung Daten: Überprüfen von Datensätzen mit falsche Länge und/oder Breite

In diesem Beispiel untersucht, wenn eines der Felder Länge und/oder Breite entweder einen ungültigen Wert enthalten (Radiant Grad sollten sich zwischen-90 und 90), oder Sie haben (0; 0) Koordinaten zurück.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Damit arbeiten: Tipped im Vergleich zu nicht hinterlassen hat Schleifen Verteilung

In diesem Beispiel ermittelt die Anzahl der Reisen, die im Vergleich zu hinterlassen nicht hinterlassen hat in einem bestimmten Zeitraum Periode (oder im vollständigen Dataset hat wurden ist das vollständige Jahr darüber liegendem). Diese Verteilung spiegeln die Verteilung binäre Bezeichnung für binäre Klassifizierung Modellierung später verwendet werden sollen.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Damit arbeiten: Tipp Klasse-Bereich Verteilung

In diesem Beispiel berechnet die Verteilung der Tipp Bereiche in einem bestimmten Zeitraum (oder im vollständigen Dataset ist das vollständige Jahr darüber liegendem). Dies ist die Verteilung der Bezeichnung Klassen, die später für multiclass Klassifizierung Modellierung verwendet wird.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Damit arbeiten: Berechnen und Vergleichen von Geschäftsreise Abstand

In diesem Beispiel wandelt die Abholung und Ladengeschäft Länge und Breite in SQL Geography verweist, berechnet den Geschäftsreise Abstand mit SQL Geography Punkte Unterschied und gibt eine Stichprobe von der Ergebnisse für den Vergleich. Im Beispiel werden die Ergebnisse an einen gültigen Koordinaten nur mithilfe der Qualität Bewertung Datenabfrage zuvor verdeckt beschränkt.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Feature technisch in SQL-Abfragen

Die Beschriftung der zweiten Generation Geography Konvertierung datenauswertung Abfragen und können auch auf Etiketten/Features zu generieren, indem Sie das Webpart zählen verwendet werden. Zusätzliche Features engineering SQL-Beispiele werden im Abschnitt [Durchsuchen von Daten und Features Engineering IPython Notizbuch](#ipnb) bereitgestellt. Es ist effizienter für das Feature ausgeführt Generation Abfragen vollständigen Dataset oder eine große Teilmenge unter Verwendung der SQL-Abfragen direkt auf die Instanz der SQL Server-Datenbank ausgeführt werden. In **SQL Server Management Studio**, IPython Notizbuch oder eine beliebige Tool/Entwicklungsumgebung die Datenbank lokal oder Remote zugreifen können, können die Abfragen ausgeführt werden.

#### <a name="preparing-data-for-model-building"></a>Vorbereiten von Daten für Modell Gebäude

Die folgende Abfrage Verknüpfungen der **Nyctaxi\_Geschäftsreise** und **Nyctaxi\_Fahrpreis** Tabellen, generiert eine binäre Klassifizierung Bezeichnung **hinterlassen hat**, eine Beschriftung mit mehreren Klasse Klassifizierung **Tipp\_Class**, und eine Stichprobe von 1 % aus dem vollständigen verknüpfte Dataset extrahiert. Diese Abfrage kopiert dann direkt in die [Azure maschinellen Learning Studio](https://studio.azureml.net) [Importieren von Daten] eingefügt werden kann[ import-data] -Modul für direkte Daten Erfassung von SQL Server-Datenbank in Azure Instanz. Die Abfrage schließt Datensätze mit falsch (0; 0) Koordinaten zurück.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="a-nameipnbadata-exploration-and-feature-engineering-in-ipython-notebook"></a><a name="ipnb"></a>Durchsuchen von Daten und Features technisch IPython Notizbuch

In diesem Abschnitt werden wir Durchsuchen von Daten und Features der zweiten Generation mit sowohl Python und SQL-Abfragen für die SQL Server-Datenbank, die zuvor erstellten ausführen. Ein Beispiel für IPython Notizbuch mit dem Namen **machine-Learning-data-science-process-sql-story.ipynb** wird in der **Stichprobe IPython Notizbücher** Ordner bereitgestellt. Dieses Notizbuch ist auch auf [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks)verfügbar.

Der empfohlene Ablauf bei der Arbeit mit big Data lautet wie folgt:

- Lesen Sie in ein kurzes Beispiel für die Daten in einen Datenrahmen in-Memory-aus.
- Führen Sie einige Bandbreite von Darstellungen und mit der erfassten Daten kennen.
- Experimentieren Sie mit dem Feature technisch mithilfe der erfassten Daten.
- Verwenden Sie für größere Durchsuchen von Daten, Bearbeiten von Daten und Features technisch Python, SQL-Abfragen direkt mit der SQL Server-Datenbank auf dem Azure-virtuellen Computer auszustellen.
- Entscheiden Sie zum Erstellen von Azure maschinellen Learning Modell zu verwendenden der Stichprobenumfang.

Wenn Sie bereit sind, um Azure maschinellen Learning aufzurufen, können Sie:  

1. Speichern Sie die endgültige SQL-Abfrage zum Extrahieren der Beispieldaten und die Abfrage direkt in einer [Importierten Daten] kopieren und Einfügen[ import-data] Modul Azure Computer interessante. Diese Methode wird im Abschnitt [Von Datenmodellen in Azure maschinellen Learning](#mlmodel) veranschaulicht.    
2. Beibehalten der aufgenommenen und Engineering Daten, die Sie für das Modell in eine neue Datenbanktabelle erstellen verwenden möchten, und verwenden Sie dann die neue Tabelle in den [Daten importieren] [ import-data] Modul.

Im folgenden finden Sie einige Durchsuchen von Daten, Visualisierung von Daten und Features Beispiele Technik. Wenn Sie weitere Beispiele finden Sie unter der Stichprobe SQL IPython Notizbuch in den Ordner für die **Stichprobe IPython Notizbücher** .

#### <a name="initialize-database-credentials"></a>Initialisierung Datenbankbenutzers

Initialisierung der Datenbank Verbindungseinstellungen in den folgenden Variablen:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Create Database Connection

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Bericht Anzahl von Zeilen und Spalten in der Tabelle nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Gesamtanzahl der Zeilen = 173179759  
- Gesamtzahl der Spalten = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Lesen in einer Stichprobe kleine Daten aus der SQL Server-Datenbank

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Zeit zum Lesen, dass die Beispieltabelle 6.492000 Sekunden ist  
Anzahl von Zeilen und Spalten, die abgerufen = (84952, 21)

#### <a name="descriptive-statistics"></a>Populationskenngrößen

Jetzt sind die aufgenommenen Daten nun auswerten. Zunächst von betrachtet Populationskenngrößen für die **Geschäftsreise\_Abstand** (oder eine beliebige andere) Felder aus:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Visualisierung: Beispiel für Zeichnungsfläche

Sehen Sie sich die Zeichnung im Feld für den Abstand Geschäftsreise visualisiert werden sollen, die Quantiles anschließend

    df1.boxplot(column='trip_distance',return_type='dict')

![1 # darstellen][1]

#### <a name="visualization-distribution-plot-example"></a>Visualisierung: Beispiel Verteilung Zeichnungsfläche

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Zeichnen Sie #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Visualisierung: Balken- und Linie Flächen

In diesem Beispiel werden den Abstand Geschäftsreise in fünf Papierkörben bin und die Ergebnisse binning darstellen.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Wir können die oben angegebenen Papierkorb Verteilung in einer Leiste darstellen oder Zeichnung als unten ausrichten

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Zeichnen Sie #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 darstellen][4]

#### <a name="visualization-scatterplot-example"></a>Visualisierung: Scatterplot Beispiel

Wir anzeigen Punktdiagramm zwischen **Geschäftsreise\_Zeit\_in\_Sekunden** und **Geschäftsreise\_Abstand** um festzustellen, ob alle Korrelationskoeffizienten vorhanden ist

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 darstellen][6]

Wir können auf ähnliche Weise überprüfen Sie die Beziehung zwischen **Zins\_Code** und **Geschäftsreise\_Abstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 darstellen][8]

### <a name="sub-sampling-the-data-in-sql"></a>Untergeordnete Stichproben der Daten in SQL

Bei der Vorbereitung für Datenmodell in [Azure maschinellen Learning Studio](https://studio.azureml.net)erstellen, Sie möglicherweise entscheiden Sie sich für die **SQL-Abfrage direkt im Modul "Daten importieren" verwenden** oder beibehalten die Engineering und Stichprobe Daten in einer neuen Tabelle, die Sie in das [Importieren von Daten] verwenden können[ import-data] Modul mit einem einfachen * *auswählen* von < Ihre\_neue\_Tabelle\_Name >**.

In diesem Abschnitt wird eine neue Tabelle zum Speichern der Daten aufgenommenen und Engineering erstellt. Ein Beispiel für eine direkte SQL-Abfrage zum Modell erstellen, werden im Abschnitt [Durchsuchen von Daten und Engineering Feature in SQL Server](#dbexplore) bereitgestellt.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Erstellen eine Stichprobe Tabelle, und füllen Sie mit 1 % von verknüpften Tabellen. Legen Sie den ersten Tabelle an, falls vorhanden.

In diesem Abschnitt wir verknüpfen Sie die Tabellen **Nyctaxi\_Geschäftsreise** und **Nyctaxi\_Fahrpreis**extrahieren eine Stichprobe von 1 % und beibehalten die erfassten Daten in einer neuen Tabelle namens **Nyctaxi\_eine\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Durchsuchen von Daten mit SQL-Abfragen in IPython Notizbuch

In diesem Abschnitt untersuchen wir die Verteilung von Daten mithilfe der Stichprobe von 1 %-Daten, die in der neuen Tabelle, den wir beibehalten werden über erstellt haben. Beachten Sie, dass ähnliche kennen mit der ursprünglichen Tabellen mit optional **TABLESAMPLE** zur Einschränkung der datenauswertung Beispiel für oder indem Sie die Ergebnisse zu einem beliebigen Zeitpunkt Periode mit ausgeführt werden können der **Abholung\_Datetime** teilt den, wie im Abschnitt [Durchsuchen von Daten und Engineering Feature in SQL Server](#dbexplore) dargestellt.

#### <a name="exploration-daily-distribution-of-trips"></a>Damit arbeiten: Tägliche Verteilung der Schleifen

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Damit arbeiten: Geschäftsreise Verteilung pro medallion

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Features der zweiten Generation mit SQL-Abfragen in IPython Notizbuch

In diesem Abschnitt werden wir neue Etiketten generieren und Features, die direkt mit SQL-Abfragen, klicken Sie auf der 1 % Beispieltabelle Betrieb wir im vorherigen Abschnitt erstellt.

#### <a name="label-generation-generate-class-labels"></a>Beschriftung der zweiten Generation: Generieren Sie Class Etiketten

Im folgenden Beispiel generieren wir zwei Sätze von Etiketten für Modellierung verwendet werden soll:

1. Binäre Klasse Etiketten **hinterlassen hat** (Vorhersage, wenn ein Tipp gewährt wird)
2. Multiclass Etiketten **Tipp\_Class** (Vorhersage Tipp Lagerplatz oder Bereich)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Feature technisch: Zählen Features für kategorisierte Spalten

In diesem Beispiel transformiert ein kategorisierten Feld in ein numerisches Feld jede Kategorie durch die Anzahl der Vorkommen in den Daten ersetzt werden.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Feature technisch: Papierkorb Features für numerische Spalten

In diesem Beispiel wandelt ein fortlaufender numerisches Feld in vordefinierte Kategorie Bereiche, d. h., Transformation numerisches Feld in einem kategorisierten Feld.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Feature technisch: Extrahieren Sie Speicherort Features von Dezimalstellen Breite/Länge

In diesem Beispiel unterteilt die dezimale Darstellung eines Felds Breite und/oder Länge in mehrere Region Felder auf verschiedenen, sei als, Land, Ort, Stadt, blockieren usw.. Beachten Sie, dass die neuen Geo-Felder nicht tatsächlichen Speicherorte zugeordnet sind. Informationen zu Zuordnung Geocode Orte finden Sie unter [Bing Maps REST-Dienste](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Überprüfen der endgültigen Form der Tabelle featurized

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Wir werden nun Modell Gebäude und Modell Bereitstellung [Azure Computer](https://studio.azureml.net)interessante fortfahren. Die Daten sind für die Vorhersage Probleme identifiziert einer früheren Version, nämlich bereit:

1. Binäre Klassifizierung: Vorhersagen, unabhängig davon, ob ein Tipp für eine Geschäftsreise bezahlt wurde.

2. Multiclass Klassifizierung: den Zellbereich, der Tipp bezahlt, Übereinstimmung mit den zuvor definierten Klassen Vorhersagen.

3. Regression Aufgabe: die Menge der QuickInfo für eine Geschäftsreise gezahlte Vorhersagen.  


## <a name="a-namemlmodelabuilding-models-in-azure-machine-learning"></a><a name="mlmodel"></a>Baustein-Modelle interessante Azure-Computern

Um die Modellierung Übung zu beginnen, melden Sie sich bei dem Arbeitsbereich Azure maschinellen Schulung an. Wenn Sie noch keinen Computer learning Arbeitsbereich erstellt haben, finden Sie unter [Erstellen einer Arbeitsbereich Azure Computer lernen](machine-learning-create-workspace.md).

1. Um mit Azure maschinellen Learning anzufangen, finden Sie unter [Neuigkeiten Azure maschinellen Learning Studio?](machine-learning-what-is-ml-studio.md)

2. Melden Sie sich bei [Azure-Computern Learning Studio](https://studio.azureml.net).

3. Die Studio-Startseite bietet eine Fülle von Informationen, Videos, Lernprogramme, Links zu den Bezug Module und anderen Ressourcen. Weitere Informationen über Azure maschinellen Schulung und wenden Sie sich an den [Azure Arbeitsplatzes Learning Dokumentation](https://azure.microsoft.com/documentation/services/machine-learning/).

Eine typische Schulung experimentieren setzt sich wie folgt:

1. Erstellen einer **+ neu** experimentieren.
2. Abrufen von Daten Azure maschinellen Schulung.
3. Vorab verarbeiten Sie, transformieren Sie und bearbeiten Sie die Daten, je nach Bedarf.
4. Generieren Sie Features, je nach Bedarf.
5. Aufteilen von Daten in Datasets Schulung, Überprüfung, testen (oder separaten Datasets für jede).
6. Wählen Sie einen oder mehrere Computer Learning Algorithmen in Abhängigkeit von der Learning Problem zu lösen. Z. B. binäre Klassifizierung, multiclass Klassifizierung, Regression.
7. Schulen von ein oder mehrere Modelle mit dem Dataset Schulung.
8. Faktor Überprüfung Dataset mithilfe der ausgebildeten Modelle.
9. Auswerten der Modelle, um die relevanten Kennzahlen für das Problem Learning zu berechnen.
10. Schnelles optimieren Sie der Modelle aus, und wählen Sie das beste Modell bereitstellen.

In dieser Übung haben wir bereits untersucht und Engineering die Daten in SQL Server und entschieden Azure Computer interessante Aufnahme auf der Stichprobenumfang. So erstellen Sie eine oder mehrere der Vorhersagemodelle, die wir entschieden:

1. Abrufen der Daten zu Azure Computer lernen das [Importieren von Daten] mit[ import-data] Modul, im Abschnitt **Dateneingabe und Ausgabe** zur Verfügung. Weitere Informationen finden Sie unter [Importieren von Daten] [ import-data] Modul Bezug Seite.

    ![Importieren von Daten Learning Azure-Computern][17]

2. Wählen Sie als **Datenquelle** **im Eigenschaftenbereich** **Azure SQL-Datenbank** ein.

3. Geben Sie den DNS-Datenbanknamen im Feld **Datenbankservername** ein. Format:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Geben Sie den **Namen der Datenbank** in das entsprechende Feld ein.

5. Geben Sie den **SQL-Benutzername** in den **Server Aqccount Benutzernamen und das Kennwort in die **Server-Benutzer-Kontos Kennwort **.

6. Aktivieren Sie Option **Alle Serverzertifikat annehmen** .

7. In den Textbereich der Bearbeiten **Datenbankabfrage** Beispiele einfügen die Abfrage, die extrahiert werden, die erforderlichen Datenbankfelder (einschließlich aller berechnete Felder wie die Beschriftungen) und nach unten die Daten in der gewünschten Stichprobenumfang.

In der folgenden Abbildung ist ein Beispiel für eine Einstufung binäre experimentieren Daten direkt aus der SQL Server-Datenbank zu lesen. Ähnliche Versuche können für multiclass Klassifizierung und Regressionsprobleme erstellt werden.

![Azure-Computern Learning Zug][10]

> [AZURE.IMPORTANT] In den Daten Modellierung zur Verfügung gestellt Extraktion und Beispiele für die Stichprobe in den vorherigen Abschnitten **Alle Etiketten für den drei Modellierung Übungen in der Abfrage enthalten sind**. Ein wichtiger (erforderlich) Schritt in jedem der Modellierung Übungen ist der unnötigen Etiketten für die anderen zwei Probleme sowie alle anderen **Ziel verliert** **ausschließen** aus. Für z. B., wenn binäre Klassifikation verwenden zu können, verwenden Sie die Bezeichnung **hinterlassen hat** und Ausschließen der Felder **Tipp\_Class**, **Tipp\_Betrag**, und **total\_Betrag**. Letztere können eine Ziel Verluste, da beide Tipps und Tricks implizieren bezahlt.
>
> Zum Ausschließen überflüssiger Spalten und/oder Verluste adressieren, können die [Spalten im Dataset auswählen] [ select-columns] Modul oder die [Metadaten bearbeiten][edit-metadata]. Weitere Informationen finden Sie unter [Wählen Sie Spalten im Dataset] [ select-columns] und [Bearbeiten von Metadaten] [ edit-metadata] Seiten verweisen.

## <a name="a-namemldeployadeploying-models-in-azure-machine-learning"></a><a name="mldeploy"></a>Bereitstellen von Datenmodellen interessante Azure-Computern

Wenn Ihr Modell bereit ist, können Sie problemlos als Webdienst direkt aus dem Versuch, bereitstellen. Weitere Informationen zum Bereitstellen von Azure maschinellen Learning-Webdiensten finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).

Wenn Sie einen neuen Webdienst bereitstellen zu können, müssen Sie:

1. Erstellen einer Punktzahl experimentieren.
2. Bereitstellen des Webdiensts.

Zum Erstellen einer Punktzahl experimentieren aus einer **erledigt** Schulung Versuch, klicken Sie auf der in der unteren Aktionsleiste **EXPERIMENTIEREN bewerten erstellen** .

![Azure bewerten][18]

Azure maschinellen Learning versucht, erstellen eine Punktzahl experimentieren basierend auf den Komponenten von der Schulung experimentieren. Es werden insbesondere folgende Aktionen ausführen:

1. Speichern Sie das Modell ausgebildete und entfernen Sie das Modell Schulungsmodule.
2. Benennen Sie ein logischer **Eingabewerte Port** , um das Schema der erwarteten Eingabedaten darstellen.
3. Ein logischer **Ausgang** zum Darstellen des erwarteten Web Service Ausgabe Schemas zu identifizieren.

Wenn die Punktzahl experimentieren erstellt wird, überprüfen Sie, und passen Sie nach Bedarf. Eine typische Anpassung besteht darin, ersetzen Sie die Eingabe-Dataset und/oder Abfrage mit einem welche Beschriftung Feldern ausschließt, wie diese nicht verfügbar sind, wenn der Dienst aufgerufen wird. Es ist auch empfiehlt sich das verringern die Größe der Eingabe-Dataset und/oder Abfrage mit wenigen Datensätzen nur genügend an, dass die Eingabewerte Schema. Für den Port Ausgabe üblicherweise alle Eingabefelder und ausschließen nur die **Beschriftungen bewertet** und **Bewertet Wahrscheinlichkeiten** bei der Ausgabe mit der [Spalten im Dataset auswählen] wird[ select-columns] Modul.

Ein Beispiel bewerten experimentieren ist in der folgenden Abbildung. Wenn Sie für die Bereitstellung bereit sind, klicken Sie auf die Schaltfläche **Webdienst veröffentlichen** in der unteren Aktionsleiste.

![Veröffentlichen von Azure Computer-Schulung][11]

Wenn Sie in diesem Lernprogramm Anleitung zusammenfassen haben Sie eine Azure Daten Wissenschaft-Umgebung gearbeitet haben, mit einem großen öffentlichen Dataset ganz aus Daten Acquisition modellieren Schulung und Bereitstellen eines Webdiensts Azure maschinellen Learning erstellt.

### <a name="license-information"></a>Lizenzinformationen

Diese Anleitung für die Stichprobe und deren öffentlichem Skripts und IPython Notebook(s) von Microsoft freigegeben, klicken Sie unter MIT-Lizenz. Aktivieren Sie die Datei, im Verzeichnis des Codes Stichprobe auf GitHub für weitere Details.

### <a name="references"></a>Verweise

• [Andrés Monroy NYC Taxi Schleifen Downloadseite](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing des NYC Taxi Geschäftsreise Daten durch Christian Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi und Limousine Commission Recherchieren und Statistik](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
