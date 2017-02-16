<properties
    pageTitle="HBase Anwendung mit Maven und Java erstellen und dann auf Linux-basierten HDInsight | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Apache Maven zum Erstellen einer Apache HBase Java-basierte Anwendungs, und klicken Sie dann auf Linux-basierten HDInsight in der Cloud Azure bereitstellen."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Verwenden von Maven Java Applications erstellen, die HBase mit Linux-basierten HDInsight (Hadoop) verwenden

Informationen Sie zum Erstellen und Einrichten einer [Apache HBase](http://hbase.apache.org/) Anwendungs in Java mithilfe von Apache Maven. Verwenden Sie dann die Anwendung mit einem HDInsight Linux-basierten Cluster ein.

[Maven](http://maven.apache.org/) ist eine Software Projektmanagement und Verständnis Tool, das Sie Software, Dokumentation und Berichte für Java Projekte erstellen kann. In diesem Artikel erfahren, wie verwenden, um eine einfache Java-Anwendung erstellen, die erstellt werden, Abfragen und löscht eine HBase Tabelle auf einem Linux-basierten HDInsight Cluster.

> [AZURE.NOTE] Die Schritte in diesem Dokument wird davon ausgegangen, dass Sie mit einen HDInsight Linux-basierten Cluster arbeiten. Informationen zur Verwendung von eines Windows-basierten HDInsight Clusters finden Sie unter [Maven verwenden, um Java-Anwendungen zu erstellen, die mit Windows-basierten HDInsight HBase verwenden](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Anforderungen

* [Java-Plattform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 oder höher

* [Maven](http://maven.apache.org/)

* [Ein Azure HDInsight Linux-basierten Cluster mit HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Die Schritte in diesem Dokument wurden mit HDInsight Cluster Versionen 3,2, 3.3 und 3.4 getestet. In den Beispielen bereitgestellten Standardwerte sind für einen Cluster HDInsight 3.4.

* **Vertrautheit mit SSH und SCP**. Weitere Informationen zum Verwenden von SSH und SCP mit HDInsight finden Sie unter den folgenden:

    * **Linux, Unix oder OS X-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Erstellen Sie das Projekt

1. Über die Befehlszeile in Ihrer Entwicklungsumgebung, wechseln Sie in die Stelle, an der Sie das Projekt erstellen `cd code/hdinsight`.

2. Verwenden Sie den Befehl __Mvn__ , die mit Maven installiert wird, um das Gerüst für das Projekt zu generieren.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Dies erstellt ein neues Verzeichnis im aktuellen Verzeichnis, mit dem Namen angegeben haben, indem Sie den Parameter __ArtifactID__ (**Hbaseapp** in diesem Beispiel). Dieses Verzeichnis enthält die folgenden Elemente:

    * __pom.XML__: der Projektobjektmodell ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) enthält Informationen und Konfiguration Details zum Erstellen des Projekts verwendet.

    * __Src__: das Verzeichnis, das Verzeichnis __Primär/Java/com/Microsoft/Beispiele__ enthält, in dem Sie die Anwendung erstellen werden.

3. Löschen Sie die Datei __src/test/java/com/microsoft/examples/apptest.java__ aus, da sie nicht in diesem Beispiel verwendet wird.

##<a name="update-the-project-object-model"></a>Aktualisieren von Project-Objektmodell

1. Bearbeiten Sie die Datei __pom.xml__ , und fügen Sie den folgenden Code innerhalb der `<dependencies>` Abschnitt:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Dies weist Maven, dass das Projekt __Hbase-Client__ -Version __1.1.2__erforderlich ist. Zum Zeitpunkt der Kompilierung wird dies aus dem standardmäßigen Maven Repository heruntergeladen werden. Sie können die [Maven zentralen Repository suchen](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) verwenden, erfahren Sie mehr über diese Abhängigkeit.

    > [AZURE.IMPORTANT] Die Versionsnummer muss die Version von HBase übereinstimmen, die mit Ihren Cluster HDInsight bereitgestellt wird. Anhand der folgenden Tabelle finden Sie die richtige Versionsnummer an.

  	| HDInsight Clusterversion | HBase Version verwendet werden soll |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 und 3.4 | 1.1.2 |

    Weitere Informationen zu HDInsight Versionen und Komponenten finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten HDInsight erhältlich](hdinsight-component-versioning.md).

2. Wenn Sie eine HDInsight 3.3 oder 3,4 Cluster verwenden, müssen Sie auch mit die folgenden Hinzufügen der `<dependencies>` Abschnitt:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Dies wird Laden Sie die Karlsruhe-Core-Komponenten, die mit Hbase Version benötigt werden 1.1.x.

2. Fügen Sie den folgenden Code zu der Datei __pom.xml__ . Dies muss innerhalb der `<project>...</project>` Tags in der Datei, z. B. zwischen `</dependencies>` und `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                <source>1.7</source>
                <target>1.7</target>
              </configuration>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
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

    Dadurch wird eine Ressource (__ü/Hbase-site.xml__,), die von Konfigurationsinformationen für HBase enthält konfiguriert.

    > [AZURE.NOTE] Sie können auch die Konfigurationswerte über Code festlegen. Finden Sie die Kommentare im Beispiel __CreateTable__ , das dazu wie folgt aus.

    Dadurch wird auch der [Maven Compiler-Plug-Ins](http://maven.apache.org/plugins/maven-compiler-plugin/) und [Maven schattieren-Plug-Ins](http://maven.apache.org/plugins/maven-shade-plugin/)konfiguriert. Der Compiler-Plug-Ins wird verwendet, um der Suchtopologie kompilieren. Die Schattierung-Plug-Ins wird verwendet, um zu verhindern, dass das Paket JAR-Lizenz Duplikate, die vom Maven erstellt wird. Der Grund dafür, dass dies verwendet wird, ist, dass die doppelten Lizenzdateien zur Laufzeit auf dem Cluster HDInsight einen Fehler verursachen. Mithilfe von Maven-schattieren-Plug-in, mit dem `ApacheLicenseResourceTransformer` Implementierung wird verhindert, dass dieser Fehler.

    Das Maven schattieren Plug-in erzeugt auch eine Uber Jar (oder fat Jar,), die alle Abhängigkeiten, die von der Anwendung benötigte enthält.

3. Speichern Sie die Datei __pom.xml__ .

4. Erstellen Sie ein neues Verzeichnis __ü__ im Verzeichnis __Hbaseapp__ . Dies wird zum Speichern von Konfigurationsinformationen für die Verbindung mit HBase verwendet werden.

5. Verwenden Sie den folgenden Befehl aus, um die Konfiguration HBase vom Server HDInsight im Verzeichnis __ü__ zu kopieren. Ersetzen Sie **Benutzername** der Ihr Benutzername SSH. Ersetzen Sie durch den Namen Ihrer HDInsight Cluster **CLUSTERNAME** :

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Wenn Sie ein Kennwort für Ihr Konto SSH verwendet haben, werden Sie aufgefordert, das Kennwort einzugeben. Wenn Sie einen Schlüssel SSH mit dem Konto verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter, um den Pfad für die Datei anzugeben. Im folgende Beispiel wird den privaten Schlüssel aus laden `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Erstellen Sie die Anwendung

1. Wechseln Sie zu dem Verzeichnis __Hbaseapp/Src/primär/Java/com/Microsoft/Beispiele__ , und benennen Sie die Datei app.java in __CreateTable.java__.

2. Öffnen Sie die Datei __CreateTable.java__ , und Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Dies ist die __CreateTable__ -Klasse, erstellen Sie eine Tabelle mit der Bezeichnung __Personen__ und füllen Sie es für einige Benutzer vordefinierte wird.

3. Speichern Sie die Datei __CreateTable.java__ .

4. Erstellen Sie eine neue Datei namens __SearchByEmail.java__im Verzeichnis __Hbaseapp/Src/primär/Java/com/Microsoft/Beispiele__ . Verwenden Sie die folgenden als den Inhalt dieser Datei ein:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    Die __SearchByEmail__ Klasse kann verwendet werden, zum Abfragen von Zeilen nach e-Mail-Adresse. Da sie einen Filter für reguläre Ausdrücke verwendet, können Sie entweder eine Zeichenfolge oder eine reguläre Ausdrücke die Klasse nutzen können.

5. Speichern Sie die Datei __SearchByEmail.java__ .

6. Erstellen Sie eine neue Datei namens __DeleteTable.java__im Verzeichnis __Hbaseapp/Src/primär/Hava/com/Microsoft/Beispiele__ . Verwenden Sie die folgenden als den Inhalt dieser Datei ein:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Diese Klasse ist für das Bereinigen in diesem Beispiel wird durch Deaktivieren und die von der Klasse __CreateTable__ erstellte Tabelle ablegen.

7. Speichern Sie die Datei __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Erstellen und Verpacken der Anwendungs

2. Verwenden Sie den folgenden Befehl aus dem Verzeichnis __Hbaseapp__ um eine JAR-Datei zu erstellen, die die Anwendung enthält:

        mvn clean package

    Alle vorherigen Build Elemente bereinigt, downloads Abhängigkeiten, die noch nicht installiert haben, klicken Sie dann erstellt und die Anwendung verpackt.

3. Wenn der Befehl abgeschlossen ist, wird das Verzeichnis __Hbaseapp/Ziel__ eine Datei namens __Hbaseapp-1.0-SNAPSHOT.jar__enthalten.

    > [AZURE.NOTE] Die Datei __Hbaseapp-1.0-SNAPSHOT.jar__ ist ein Uber Jar (auch einem fat Glas, genannt) die enthält alle Abhängigkeiten, die zum Ausführen der Anwendung erforderlich.

##<a name="upload-the-jar-file-and-run-jobs"></a>Hochladen der JAR-Datei, und führen Sie Aufträge

1. Verwenden Sie die folgenden JAR-Datei zum Cluster HDInsight hochladen aus. Ersetzen Sie **Benutzername** der Ihr Benutzername SSH. Ersetzen Sie durch den Namen Ihrer HDInsight Cluster **CLUSTERNAME** :

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Dadurch wird die Datei im Stammverzeichnis für Ihr Benutzerkonto SSH hochladen.

    > [AZURE.NOTE] Wenn Sie ein Kennwort für Ihr Konto SSH verwendet haben, werden Sie aufgefordert, das Kennwort einzugeben. Wenn Sie einen Schlüssel SSH mit dem Konto verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter, um den Pfad für die Datei anzugeben. Im folgende Beispiel wird den privaten Schlüssel aus laden `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Verwenden Sie für die Verbindung mit dem Cluster HDInsight SSH ein. Ersetzen Sie **Benutzername** der Ihr Benutzername SSH. Ersetzen Sie durch den Namen Ihrer HDInsight Cluster **CLUSTERNAME** :

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Wenn Sie ein Kennwort für Ihr Konto SSH verwendet haben, werden Sie aufgefordert, das Kennwort einzugeben. Wenn Sie einen Schlüssel SSH mit dem Konto verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter, um den Pfad für die Datei anzugeben. Im folgende Beispiel wird den privaten Schlüssel aus laden `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Nachdem die Verbindung hergestellt wurde, verwenden Sie zum Erstellen einer neuen HBase Tabelle mithilfe der Java-Anwendung folgende:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    Erstellt eine neue HBase-Tabelle mit der Bezeichnung __Personen__, und mit Daten zu füllen.

4. Als Nächstes verwenden Sie die folgenden So suchen Sie nach e-Mail-Adressen in der Tabelle gespeichert:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Sie sollten die folgenden Ergebnisse angezeigt:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Löschen Sie die Tabelle

Wenn Sie mit dem Beispiel fertig sind, verwenden Sie den folgenden Befehl aus der Azure-PowerShell-Sitzung, um die in diesem Beispiel verwendete __Personen__ Tabelle löschen:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

