<properties
pageTitle="HBase Anwendung mit Maven erstellen und Bereitstellen für Windows-basiertem HDInsight | Microsoft Azure"
description="Informationen Sie zum Verwenden von Apache Maven zum Erstellen einer Java-basierte Apache HBase-Anwendungs, und klicken Sie dann auf einem Windows-basierten Azure HDInsight Cluster bereitstellen."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"
tags="azure-portal"/>

<tags
ms.service="hdinsight"
ms.workload="big-data"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Verwenden von Maven Java Applications erstellen, die HBase mit Windows-basierten HDInsight (Hadoop) verwenden

Informationen Sie zum Erstellen und Einrichten einer [Apache HBase](http://hbase.apache.org/) Anwendungs in Java mithilfe von Apache Maven. Verwenden Sie dann die Anwendung mit Azure HDInsight (Hadoop) ein.

[Maven](http://maven.apache.org/) ist eine Software Projektmanagement und Verständnis Tool, das Sie Software, Dokumentation und Berichte für Java Projekte erstellen kann. In diesem Artikel erfahren Sie, wie zur gemeinsamen Nutzung von einer grundlegenden Java-Anwendung, die erstellen, die erstellt werden, Abfragen und löscht eine HBase Tabelle auf einem Cluster Azure HDInsight.

> [AZURE.NOTE] Die Schritte in diesem Dokument wird davon ausgegangen, dass Sie mit einen Windows-basierten HDInsight Cluster arbeiten. Informationen zur Verwendung von eines Clusters Linux-basierten HDInsight finden Sie unter [Verwenden Maven Java Applications erstellen, die mit HDInsight Linux-basierten HBase verwenden](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Anforderungen

* [Java-Plattform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 oder höher

* [Maven](http://maven.apache.org/)


* [Ein Windows-basiertes HDInsight Cluster mit HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] Die Schritte in diesem Dokument wurden mit HDInsight Cluster Versionen 3,2 und 3.3 getestet. In den Beispielen bereitgestellten Standardwerte sind für einen HDInsight 3.3 Cluster.

##<a name="create-the-project"></a>Erstellen Sie das Projekt

1. Über die Befehlszeile in Ihrer Entwicklungsumgebung, wechseln Sie in die Stelle, an der Sie das Projekt erstellen `cd code\hdinsight`.

2. Verwenden Sie den Befehl __Mvn__ , die mit Maven installiert wird, um das Gerüst für das Projekt zu generieren.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Dieser Befehl erstellt ein Verzeichnis in der aktuellen Position aus, mit dem Namen angegeben haben, indem Sie den Parameter __ArtifactID__ (**Hbaseapp** in diesem Beispiel). Dieses Verzeichnis enthält die folgenden Elemente:

    * __pom.XML__: der Projektobjektmodell ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) enthält Informationen und Konfiguration Details zum Erstellen des Projekts verwendet.

    * __Src__: das Verzeichnis, das Verzeichnis __Main\java\com\microsoft\examples__ enthält, in dem Sie die Anwendung erstellen werden.

3. Löschen Sie die Datei __src\test\java\com\microsoft\examples\apptest.java__ aus, da sie nicht in diesem Beispiel verwendet wird.

##<a name="update-the-project-object-model"></a>Aktualisieren von Project-Objektmodell

1. Bearbeiten Sie die Datei __pom.xml__ , und fügen Sie den folgenden Code innerhalb der `<dependencies>` Abschnitt:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    In diesem Abschnitt erfahren Maven, dass das Projekt __Hbase-Client__ -Version __1.1.2__erforderlich ist. Zum Zeitpunkt der Kompilierung wird diese Abhängigkeit von der standardmäßigen Maven Repository heruntergeladen. Sie können die [Maven zentralen Repository suchen](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) verwenden, erfahren Sie mehr über diese Abhängigkeit.

    > [AZURE.IMPORTANT] Die Versionsnummer muss die Version von HBase übereinstimmen, die mit Ihren Cluster HDInsight bereitgestellt wird. Anhand der folgenden Tabelle finden Sie die richtige Versionsnummer an.

  	| HDInsight Clusterversion | HBase Version verwendet werden soll |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Weitere Informationen zu HDInsight Versionen und Komponenten finden Sie unter [Was sind die verschiedenen Hadoop-Komponenten HDInsight erhältlich](hdinsight-component-versioning.md).

2. Wenn Sie einen HDInsight 3.3 Cluster verwenden, müssen Sie auch mit die folgenden Hinzufügen der `<dependencies>` Abschnitt:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Diese Abhängigkeit lädt die Karlsruhe-Core-Komponenten, die von Hbase Version verwendet werden 1.1.x.

2. Fügen Sie den folgenden Code zu der Datei __pom.xml__ . In diesem Abschnitt muss innerhalb der `<project>...</project>` Tags in der Datei, z. B. zwischen `</dependencies>` und `</project>`.

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

    Die `<resources>` Abschnitt konfiguriert eine Ressource (__Conf\hbase-site.xml__), die von Konfigurationsinformationen für HBase enthält.

    > [AZURE.NOTE] Sie können auch die Konfigurationswerte über Code festlegen. Finden Sie die Kommentare im Beispiel __CreateTable__ , das dazu wie folgt aus.

    Dies `<plugins>` Abschnitt konfiguriert die [Maven Compiler-Plug-Ins](http://maven.apache.org/plugins/maven-compiler-plugin/) und [Maven schattieren-Plug-in](http://maven.apache.org/plugins/maven-shade-plugin/). Der Compiler-Plug-Ins wird verwendet, um der Suchtopologie kompilieren. Die Schattierung-Plug-Ins wird verwendet, um zu verhindern, dass das Paket JAR-Lizenz Duplikate, die vom Maven erstellt wird. Der Grund dafür, dass dies verwendet wird, ist, dass die doppelten Lizenzdateien zur Laufzeit auf dem Cluster HDInsight einen Fehler verursachen. Mithilfe von Maven-schattieren-Plug-in, mit dem `ApacheLicenseResourceTransformer` Implementierung wird verhindert, dass dieser Fehler.

    Das Maven schattieren Plug-in erzeugt auch eine Uber Jar (oder fat Jar), die alle Abhängigkeiten, die von der Anwendung benötigte enthält.

3. Speichern Sie die Datei __pom.xml__ .

4. Erstellen Sie ein neues Verzeichnis __ü__ im Verzeichnis __Hbaseapp__ . Erstellen Sie eine Datei namens __Hbase-site.xml__, in dem Verzeichnis __ü__ . Verwenden Sie die folgenden als den Inhalt der Datei ein:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Diese Datei wird beim Laden der HBase Konfiguration für einen HDInsight Cluster verwendet werden.

    > [AZURE.NOTE] Dies ist eine minimale Hbase-site.xml-Datei, und die minimalen absolut erforderlichen Einstellungen für den Cluster HDInsight enthält.

3. Speichern Sie die __Hbase site.xml__ Datei ein.

##<a name="create-the-application"></a>Erstellen Sie die Anwendung

1. Wechseln Sie zu dem Verzeichnis __Hbaseapp\src\main\java\com\microsoft\examples__ , und benennen Sie die Datei app.java in __CreateTable.java__.

2. Öffnen Sie die Datei __CreateTable.java__ , und Ersetzen Sie den vorhandenen Inhalt mit den folgenden Code ein:

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. Erstellen Sie eine neue Datei namens __SearchByEmail.java__im Verzeichnis __Hbaseapp\src\main\java\com\microsoft\examples__ . Verwenden Sie den folgenden Code als Inhalt dieser Datei ein:

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

    Die __SearchByEmail__ Klasse kann verwendet werden, zum Abfragen von Zeilen nach e-Mail-Adresse. Da er einen Filter für reguläre Ausdrücke verwendet, können Sie entweder eine Zeichenfolge oder eine reguläre Ausdrücke die Klasse nutzen können.

5. Speichern Sie die Datei __SearchByEmail.java__ .

6. Erstellen Sie eine neue Datei namens __DeleteTable.java__im Verzeichnis __Hbaseapp\src\main\hava\com\microsoft\examples__ . Verwenden Sie den folgenden Code als Inhalt dieser Datei ein:

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

1. Öffnen Sie ein Eingabeaufforderungsfenster, und wechseln Sie in das Verzeichnis __Hbaseapp__ .

2. Verwenden Sie den folgenden Befehl aus, um einer JAR-Datei zu erstellen, die die Anwendung enthält:

        mvn clean package

    Alle vorherigen Build Elemente bereinigt, downloads Abhängigkeiten, die noch nicht installiert haben, klicken Sie dann erstellt und die Anwendung verpackt.

3. Wenn der Befehl abgeschlossen ist, enthält __Hbaseapp\target__ Verzeichnis eine Datei namens __Hbaseapp-1.0-SNAPSHOT.jar__aus.

    > [AZURE.NOTE] Die Datei __Hbaseapp-1.0-SNAPSHOT.jar__ ist ein Uber Jar (auch einem fat Glas, genannt) die enthält alle Abhängigkeiten, die zum Ausführen der Anwendung erforderlich.

##<a name="upload-the-jar-file-and-start-a-job"></a>Hochladen der JAR-Datei, und starten Sie ein Projekt

Es gibt viele Methoden zum Hochladen einer Datei auf Ihrem Cluster HDInsight wie in [Upload von Daten für Hadoop Aufträge in HDInsight](hdinsight-upload-data.md)beschrieben. Verwenden Sie die folgenden Schritte Azure PowerShell aus.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Nach dem Installieren und Konfigurieren von Azure PowerShell, erstellen Sie eine neue Datei namens __Hbase-runner.psm1__aus. Verwenden Sie die folgenden als den Inhalt dieser Datei ein:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Diese Datei enthält zwei Module:

    * __Hinzufügen-HDInsightFile__ - zum Hochladen von Dateien mit HDInsight verwendet

    * __Start-HBaseExample__ - verwendet, um den zuvor erstellten Klassen ausführen

2. Speichern Sie die Datei __Hbase-runner.psm1__ .

3. Öffnen eines neuen Azure PowerShell-Fensters, wechseln Sie in das Verzeichnis __Hbaseapp__ , und führen Sie dann den folgenden Befehl aus.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Ändern Sie den Pfad zu dem Speicherort der zuvor erstellten __Hbase runner.psm1__ Datei ein. Dadurch wird das Modul für diese Sitzung Azure PowerShell registriert.

2. Verwenden Sie den folgenden Befehl aus, um die __Hbaseapp-1.0-SNAPSHOT.jar__ zum HDInsight Cluster hochzuladen.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Ersetzen Sie __Hdinsightclustername__ durch den Namen des HDInsight Cluster ein. Der Befehl uploads der __Hbaseapp-1.0-SNAPSHOT.jar__ zum __Beispiel/Gläser__ Speicherort in der primären Speicher für Ihren Cluster HDInsight.

3. Sobald die Dateien hochgeladen werden, verwenden Sie den folgenden Code zum Erstellen einer Tabelle mithilfe der __Hbaseapp__aus:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Ersetzen Sie __Hdinsightclustername__ durch den Namen des HDInsight Cluster ein.

    Dieser Befehl erstellt eine neue Tabelle namens __Personen__ in Ihrer HDInsight Cluster. Dieser Befehl wird keine Ausgabe im Konsolenfenster nicht angezeigt werden.

2. Um die Einträge in der Tabelle zu suchen, verwenden Sie den folgenden Befehl aus:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Ersetzen Sie __Hdinsightclustername__ durch den Namen des HDInsight Cluster ein.

    Dieser Befehl verwendet die **SearchByEmail** -Klasse für die Suche nach alle Zeilen, in denen die Familie __Contactinformation__ Spalte und die __e-Mail__ -Spalte, die Zeichenfolge __"contoso.com"__enthält. Sie sollten die folgenden Ergebnisse angezeigt:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Verwenden __fabrikam.com__ für die `-emailRegex` Wert gibt den Benutzer, den __fabrikam.com__ im Feld e-Mail-haben. Da diese Suche mithilfe eines regulären ausdrucksbasierte Filters implementiert ist, Sie können auch eingeben reguläre Ausdrücke wie __^ R__, welche gibt Posten, an dem die e-Mail mit dem Buchstaben 'R beginnt'.

##<a name="delete-the-table"></a>Löschen Sie die Tabelle

Wenn Sie mit dem Beispiel fertig sind, verwenden Sie den folgenden Befehl aus der Azure-PowerShell-Sitzung, um die in diesem Beispiel verwendete __Personen__ Tabelle löschen:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Ersetzen Sie __Hdinsightclustername__ durch den Namen des HDInsight Cluster ein.

##<a name="troubleshooting"></a>Behandlung von Problemen

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Keine Ergebnisse oder unerwartete Ergebnisse beim Start-HBaseExample verwenden

Verwenden der `-showErr` Parameter den Standardfehler (STDERR) anzeigen möchten, die während der Ausführung des Auftrags erstellt wird.
