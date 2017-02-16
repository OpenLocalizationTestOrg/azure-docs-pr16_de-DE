<properties
pageTitle="Verwenden Sie eine Java User defined-Funktion (UDFs) mit Struktur in HDInsight | Microsoft Azure"
description="Informationen Sie zum Erstellen eine Java User defined-Funktion (UDFs) aus und Struktur in HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Verwenden einer Java UDFs mit Struktur in HDInsight

Struktur eignet sich hervorragend zum Arbeiten mit Daten in HDInsight, aber manchmal benötigen Sie einen Weitere allgemeine Zweck Sprache. Struktur können Sie benutzerdefinierte Funktionen (UDFs) mithilfe einer Vielzahl von Sprachen zu erstellen. In diesem Dokument erfahren Sie, wie Java UDFs von Struktur verwendet werden.

## <a name="requirements"></a>Anforderungen

* Ein Azure-Abonnement

* Ein HDInsight Cluster (Windows oder Linux-basierten)

    > [AZURE.NOTE] Die meisten Schritte in diesem Dokument funktionieren auf beiden Clustertypen; die Schritte zum Hochladen des kompilierte UDFs zum Cluster, und führen Sie es gibt jedoch speziell für Linux-basierten Cluster. Links werden Informationen bereitgestellt, die mit Windows-basierten Cluster verwendet werden kann.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 oder höher (oder einer ähnlichen, z. B. OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Einen Text-Editor oder Java IDE

    > [AZURE.IMPORTANT] Wenn Sie einen HDInsight Linux-basierten Server verwenden, aber die Python-Dateien auf einem Windows-Client erstellen, müssen Sie einen Editor verwenden verwendet, LF als eine Zeile Enddatum. Wenn Sie nicht sicher sind, ob Editor Zeilenvorschub- oder CRLF verwendet wird, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Schritte zum Entfernen des Dienstprogramme auf dem HDInsight Cluster mit Wagenrücklauf-Zeichens.

## <a name="create-an-example-udf"></a>Erstellen eines Beispiels UDFs

1. Verwenden Sie folgende über eine Befehlszeile zum Erstellen eines neuen Projekts von Maven:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Wenn Sie PowerShell verwenden, müssen Sie die Parameter in Anführungszeichen setzen. Beispielsweise `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Dies erstellt ein neues Verzeichnis mit dem Namen __Exampleudf__, die das Projekt Maven enthalten sind.

2. Nachdem das Projekt erstellt wurde, löschen Sie das __Exampleudf/Src/Test__ -Verzeichnis, das als Teil des Projekts erstellt wurde; Es wird nicht in diesem Beispiel verwendet werden.

3. Öffnen Sie die __exampleudf/pom.xml__, und Ersetzen Sie das vorhandene `<dependencies>` mit den folgenden Eintrag:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Diese Einträge geben Sie die Version von Hadoop und Struktur enthaltenen HDInsight 3.3 und 3.4 Cluster an. Sie können die Versionen von Hadoop und Struktur mit HDInsight aus dem Dokument [HDInsight Komponente Versioning](hdinsight-component-versioning.md) bereitgestellten Informationen suchen.

    Hinzufügen eines `<build>` Abschnitt vor der `</project>` Zeile am Ende der Datei. In diesem Abschnitt sollte Folgendes enthalten:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
                        <filters>
                            <filter>
                                <artifact>*:*</artifact>
                                <excludes>
                                    <exclude>META-INF/*.SF</exclude>
                                    <exclude>META-INF/*.DSA</exclude>
                                    <exclude>META-INF/*.RSA</exclude>
                                </excludes>
                            </filter>
                        </filters>
                    </configuration>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Diese Einträge definieren, wie Sie das Projekt erstellen. Insbesondere der Version von Java, die das Projekt verwendet und wie Sie eine Uberjar für die Bereitstellung auf den Cluster zu erstellen.

    Speichern Sie die Datei aus, sobald die Änderungen vorgenommen wurden.

4. Benennen Sie __exampleudf/src/main/java/com/microsoft/examples/App.java__ in __ExampleUDF.java__, und öffnen Sie die Datei in Editor.

5. Ersetzen Sie den Inhalt der Datei __ExampleUDF.java__ mit den folgenden, speichern Sie die Datei dann.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Dadurch wird, die einen Zeichenfolgenwert akzeptiert und gibt die Zeichenfolge in Kleinbuchstaben UDFs implementiert.

## <a name="build-and-install-the-udf"></a>Erstellen und Installieren des UDFs

1. Verwenden Sie den folgenden Befehl aus, um kompilieren und UDFs Verpacken:

        mvn compile package

    Dies wird erstellen und dann UDFs in __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__packen.

2. Verwenden der `scp` Befehl aus, um die Datei zum Cluster HDInsight zu kopieren.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Ersetzen Sie __Myuser__ durch das Benutzerkonto SSH für Ihren Cluster ein. Ersetzen Sie __MeinCluster__ durch den Clusternamen ein. Wenn Sie ein Kennwort verwendet, um das Konto SSH zu sichern, werden Sie aufgefordert, das Kennwort einzugeben. Wenn Sie ein Zertifikat verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter, um eine Datei mit dem privaten Schlüssel anzugeben.

3. Verbinden Sie mit dem Cluster SSH verwenden. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden Dokumenten.

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Kopieren Sie aus der Sitzung SSH JAR-Datei in HDInsight Speicher ein.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Verwenden von UDFs auf Struktur

1. Verwenden Sie vor, um den Beeline Client aus der SSH-Sitzung zu starten.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Dieser Befehl wird davon ausgegangen, dass Sie der Standardwert __Administrator__ für Ihren Cluster für das Login-Konto verwendet.

2. Nachdem Sie sich bei Eintreffen der `jdbc:hive2://localhost:10001/>` dazu aufgefordert werden, geben Sie Folgendes ein, um die Struktur des UDFs hinzu, und machen Sie es als eine Funktion verfügbar.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Verwenden Sie UDFs, um Werte aus einer Tabelle in Kleinbuchstaben Zeichenfolgen konvertieren.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    Dies wird ausgewählt Plattform des (Android, Windows, iOS usw.) aus der Tabelle, die Zeichenfolge in Kleinbuchstaben konvertiert, und klicken Sie dann angezeigt werden. Die Ausgabe wird ähnlich wie der folgende angezeigt.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Nächste Schritte

Andere Methoden für die Arbeit mit Struktur finden Sie unter [Verwendung mit HDInsight Struktur](hdinsight-use-hive.md).

Weitere Informationen zu Funktionen finden Sie unter [Operatoren Struktur und benutzerdefinierten Funktionen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) Abschnitt des Wiki Struktur am apache.org Hive User-Defined.
