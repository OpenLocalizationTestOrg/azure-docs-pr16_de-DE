<properties
    pageTitle="Daten mit dem Microsoft Avro Library serialisieren | Microsoft Azure"
    description="Erfahren Sie, wie Azure HDInsight Avro verwendet, um große Daten zu serialisieren."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Serialisieren von Daten in Hadoop mit Microsoft Avro Library

In diesem Thema zeigt, wie Sie die <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Bibliothek</a> verwenden, um Objekte und andere Datenstrukturen in Streams serialisieren, damit diese Arbeitsspeicher, einer Datenbank oder einer Datei erhalten bleiben, und wie sie zum Wiederherstellen der ursprünglichen Objekte deserialisiert.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
Die <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Bibliothek</a> implementiert das Apache Avro Daten Serialisierungssystem für die Microsoft.NET-Umgebung. Apache Avro bietet eine compact binäre Data Interchange-Format für die Serialisierung. <a href="http://www.json.org" target="_blank">JSON</a> verwendet, um eine Sprache-unabhängige Schema zu definieren, die Sprache Interoperability abschließt. In einer Sprache serialisierte Daten können in einer anderen gelesen werden. C, C++, c#, Java, PHP, Python und Ruby werden derzeit unterstützt. Ausführliche Informationen zum Format finden Sie in der <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Spezifikation</a>. Beachten Sie, dass die aktuelle Version von Microsoft Avro Library den Remoteprozeduraufruf Anrufe (RPC) Teil dieser Spezifikation nicht unterstützt.

Die serialisierte Darstellung eines Objekts in das System Avro besteht aus zwei Teilen: Schema und der tatsächliche Wert. Das Schema Avro beschreibt das Sprache unabhängig Datenmodell der serialisierten Daten mit JSON. Es ist auf eine binäre Darstellung der Daten präsentieren. Haben Sie das Schema aus der binäre Darstellung separaten lässt jedes Objekt mit keine-Wert bei gleichzeitigem ausführenden Serialisierung schnelle und die Darstellung kleine geschrieben werden sollen.

##<a name="the-hadoop-scenario"></a>Das Hadoop-Szenario
Apache Avro das Format wird in Azure HDInsight und anderen Umgebungen Apache Hadoop stark verwendet. Avro bietet eine geeignete Möglichkeit, komplexe Datenstrukturen innerhalb eines Auftrags Hadoop MapReduce dargestellt. Das Format von Avro-Dateien (Avro Objektdatei Container) wurde entwickelt, um die verteilte MapReduce Modell Programmierung unterstützen. Die wichtigsten Features, die die Verteilung ermöglicht wird, dass die Dateien "es", d. h., eine einer beliebigen Stelle in einer Datei anfordern und Lesebereich ab einem bestimmten Block beginnen kann.

##<a name="serialization-in-avro-library"></a>Serialisierung in Avro-Bibliothek
Bibliothek für Avro .NET unterstützt zwei Arten von Serialisierungsobjekte:

- **Spiegelung** – das JSON-Schema für die Typen wird aus den Daten Vertragsattribute .NET Typen serialisiert werden automatisch erstellt.
- **generische Datensatz** - A JSON-Schema in einem Datensatz, dargestellt durch die [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) -Klasse, wenn keine .NET Typen beschreiben Sie das Schema für die Daten zu serialisierende vorhanden sind explizit angegeben ist.

Wenn das Schema der Autor und die Reader des Streams bekannt ist, können die Daten ohne dessen Schema gesendet werden. Wenn eine Avro Container Objektdatei verwendet wird, wird das Schema in Fällen innerhalb der Datei gespeichert. Andere Parameter, wie der für die Daten Komprimierung verwendete Codec können angegeben werden muss. Diese Szenarios werden ausführlicher beschrieben und in den folgenden Codebeispielen dargestellt.


## <a name="install-avro-library"></a>Installieren von Avro-Bibliothek

Die folgenden sind erforderlich, bevor Sie die Bibliothek installieren:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 oder höher)

Beachten Sie, dass die Newtonsoft.Json.dll Abhängigkeit mit der Installation von Microsoft Avro Library automatisch heruntergeladen wird. Im folgenden Abschnitt wird das Verfahren dafür bereitgestellt.


Microsoft Avro Library wird als NuGet-Paket verteilt, die über das folgende Verfahren aus Visual Studio installiert werden kann:

1. Wählen Sie auf der Registerkarte **Projekt** -> **NuGet-Pakete verwalten...**
2. Suchen Sie nach "Microsoft.Hadoop.Avro" im Feld **Online suchen** .
3. Klicken Sie auf die Schaltfläche **Installieren** neben **Microsoft Azure HDInsight Avro Bibliothek**.

Beachten Sie, dass die Newtonsoft.Json.dll (> = 6.0.4) Abhängigkeit wird auch mit Microsoft Avro Library automatisch heruntergeladen.

Sie möchten möglicherweise besuchen Sie die <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Homepage von Microsoft Avro Bibliothek</a> , um die aktuellen Versionsinformationen zu lesen.


Der Microsoft Avro Library-Quellcode ist auf der <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Homepage von Microsoft Avro Bibliothek</a>zur Verfügung.

##<a name="compile-schemas-using-avro-library"></a>Kompilieren Sie Schemas mit Avro-Bibliothek

Microsoft Avro Library enthält ein Code Generation Programm, C#-Typen automatisch basierend auf dem zuvor definierten JSON-Schema zu erstellen. Der Code der zweiten Generation Programm wird nicht als eine ausführbare Binärdatei verteilt, jedoch kann einfach über das folgende Verfahren erstellt werden:

1. Laden Sie die ZIP-Datei mit der neuesten Version des Quellcodes HDInsight SDK von <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK für Hadoop</a>aus. (Klicken Sie auf das Symbol **herunterladen** , nicht die Registerkarte **Downloads** ).

2. Extrahieren der HDInsight SDK, um ein Verzeichnis auf dem Computer mit .NET Framework 4 installiert und mit dem Internet zum Herunterladen der erforderlichen Abhängigkeit NuGet-Paketen verbunden. Im folgenden wird davon ausgegangen, dass der Quellcode C:\SDK extrahiert gespeichert ist.

3. Wechseln Sie zum Ordner C:\SDK\src\Microsoft.Hadoop.Avro.Tools, und führen Sie build.bat. (Die Datei ruft MSBuild von der 32-Bit-Verteilung von .NET Framework. Wenn Sie die 64-Bit-Version verwenden möchten, bearbeiten Sie build.bat, die Kommentare in der Datei folgen.) Stellen Sie sicher, dass die Erstellung erfolgreich ist. (Bei einigen Systemen möglicherweise MSBuild Warnungen erzeugen. Diese Warnungen wirken das Programm sich nicht so lange keine Buildfehler vorliegen.)

4. Das kompilierte Programm befindet sich in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Wenn Sie die Syntax für die kennenzulernen, führen Sie den folgenden Befehl aus dem Ordner, in dem Code Generation Programm befindet:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Um das Programm zu testen, können Sie aus der Stichprobe JSON-Schemadatei im Lieferumfang des Quellcodes C#-Klassen generieren. Führen Sie den folgenden Befehl ein:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Dies sieht in zwei C#-Dateien im aktuellen Verzeichnis zu erstellen: SensorData.cs und Location.cs.

Finden Sie die Logik, die dem Code Generation Programm beim Konvertieren des JSON-Schemas in C#-Typen verwendet wird, die Datei, die in der C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc GenerationVerification.feature ansässig.

Bitte beachten Sie, dass Namespaces aus dem JSON-Schema, verwenden die in der Datei, die im vorherigen Absatz erwähnten beschriebenen Logik extrahiert werden. Extrahiert aus dem Schema Namespaces haben Vorrang vor den Inhalt mit dem Parameter/n in die Befehlszeile Programm bereitgestellt wird. Wenn Sie die im Schema enthaltenen Namespaces außer Kraft setzen möchten, verwenden Sie den Parameter/nf. Um alle Namespaces aus der SampleJSONSchema.avsc in my.own.nspace zu ändern, führen Sie beispielsweise den folgenden Befehl aus:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Beispiele
Sechs Beispielen in diesem Thema veranschaulichen verschiedene Szenarien, die von der Microsoft Avro Library unterstützt. Microsoft Avro Library dient für die Arbeit mit einem beliebigen Stream. In diesen Beispielen werden Daten über Arbeitsspeicherstreams statt Dateistreams oder Datenbanken für Vereinfachung und Konsistenz geändert. Ansatz wird in einer Umgebung für die Herstellung werden die genauen Szenario Anforderungen, Datenquelle und Lautstärke, Leistung Einschränkungen und andere Faktoren abhängig sind.

Den ersten beiden Beispielen wird so serialisieren und Deserialisieren von Daten in Arbeitsspeicher Stream Puffer mithilfe von Spiegelung und generische Datensätze. Das Schema in beiden Fällen wird davon ausgegangen, dass für die Leser und Autoren freigegeben werden Out-of-Band.

Der dritte und vierte Beispielen wird so serialisieren und Deserialisieren von Daten mithilfe der Avro Objekt Container-Dateien. Wenn Sie Daten in einer Avro Container-Datei gespeichert ist, wird deren Schema immer mit gespeichert, da das Schema für die Deserialisierung freigegeben werden muss.

Die Stichprobe, die die ersten vier Beispiele kann von der <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Azure Codebeispielen</a> -Website heruntergeladen werden.

Das fünfte Beispiel zeigt Gewusst wie einen Komprimierungscodec benutzerdefinierte für Avro Objekt Container-Dateien verwendet werden soll. Eine Stichprobe, die mit dem Code in diesem Beispiel kann der <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Azure Codebeispielen</a> -Website heruntergeladen werden.

Im sechste Beispiel wird gezeigt, wie Avro Serialisierung zum Hochladen von Daten in Azure Blob-Speicher und analysieren Sie es mithilfe der Struktur mit einem Cluster HDInsight (Hadoop) verwendet wird. Sie können von der <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure Codebeispielen</a> -Website heruntergeladen werden.

Hier sind Links zu den sechs Beispielen erläutert, die im Thema:

 * <a href="#Scenario1">**Serialisierung mit Spiegelung**</a> - Typen serialisiert werden das JSON Schema wird automatisch aus den Daten Vertragsattribute erstellt.
 * <a href="#Scenario2">**Serialisierung mit allgemeinen Datensatz**</a> - das JSON-Schema wird in einem Datensatz explizit angegeben, wenn kein .NET Typ für Spiegelung verfügbar ist.
 * <a href="#Scenario3">**Serialisierung mit Objekt Container-Dateien mit Spiegelung**</a> – das JSON-Schema wird automatisch erstellt und zusammen mit den serialisierten Daten über eine Avro Container Objektdatei freigegeben.
 * <a href="#Scenario4">**Serialisierung mit Objekt Container-Dateien mit allgemeinen Datensatz**</a> - das JSON-Schema ist explizit vor der Serialisierung angegeben und zusammen mit den Daten über eine Avro Container Objektdatei freigegeben.
 * <a href="#Scenario5">**Serialisierung mit Objekt Container-Dateien mit einem benutzerdefinierten Komprimierungscodec**</a> - Beispiel wird gezeigt, wie eine Avro Container Objektdatei mit einer angepassten .NET Implementierung des Codecs Komprimierung Deflate Daten erstellen.
 * <a href="#Scenario6">**Mithilfe von Avro zum Hochladen von Daten für den Dienst Microsoft Azure HDInsight**</a> - Beispiel veranschaulicht, wie Avro Serialisierung mit dem Dienst HDInsight interagiert. Zuweisen einer aktiven Azure-Abonnement und Zugriff auf eine Azure HDInsight Cluster sind zum Ausführen des Beispiels erforderlich.

###<a name="a-namescenario1asample-1-serialization-with-reflection"></a><a name="Scenario1"></a>Beispiel 1: Serialisierung mit Spiegelung

Das JSON-Schema für die Typen kann automatisch vom Microsoft Avro Library über Spiegelung aus den Daten Vertragsattribute der C#-Objekte zu serialisierende erstellt werden sollen. Die Microsoft Avro Library erstellt eine [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) identifizieren Sie die Felder, die serialisiert werden müssen.

In diesem Beispiel Objekte (einer **SensorData** Klasse ein Mitglied **Speicherort** Struktur) werden in einem Speicherstream serialisiert, und diesen Stream wiederum deserialisiert wird. Das Ergebnis wird dann verglichen, auf die erste Instanz, zu bestätigen, dass das wiederhergestellte **SensorData** Objekt mit dem Original identisch ist.

Das Schema in diesem Beispiel wird davon ausgegangen, dass zwischen den Leser und Autoren, freigegeben werden, damit das Avro Objekt Container-Format nicht erforderlich ist. Ein Beispiel für So serialisieren und Deserialisieren von Daten in Arbeitsspeicher Puffer mithilfe Spiegelung mit dem Objekt Container-Format, wenn das Schema für die Daten freigegeben werden muss finden Sie unter <a href="#Scenario3">Serialisierung Objekt Container-Dateien mit Spiegelung verwenden</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Beispiel 2: Serialisierung mit einen allgemeinen Datensatz

Ein JSON-Schema kann in einen allgemeinen Datensatz explizit angegeben werden, wenn Spiegelung verwendet werden kann, da die Daten über .NET Klassen mit einem Datenvertrag dargestellt werden können. Diese Methode ist im Allgemeinen langsamer als Spiegelung verwenden. In diesem Fall sein das Schema für die Daten auch dynamische, d. h., möglicherweise nicht bekannt zum Zeitpunkt der Kompilierung. Daten, die als durch Trennzeichen getrennte Werte (CSV)-Dateien, deren Schema bekannt ist, bevor er zur Laufzeit in das Format Avro transformiert wird, dargestellt ist ein Beispiel für diese Art von dynamischen Szenario.

Dieses Beispiel zeigt das Erstellen und Verwenden einer [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) ein Schema JSON explizit angeben, wie Sie es mit den Daten zu füllen und dann so serialisieren und deserialisieren. Das Ergebnis wird dann verglichen, auf die erste Instanz, zu bestätigen, dass der wiederhergestellte Datensatz mit dem Original identisch ist.

Das Schema in diesem Beispiel wird davon ausgegangen, dass zwischen den Leser und Autoren, freigegeben werden, damit das Avro Objekt Container-Format nicht erforderlich ist. Ein Beispiel für So serialisieren und Deserialisieren von Daten in Arbeitsspeicher Puffer mithilfe einen allgemeinen Datensatz mit dem Objekt Container-Format, wenn Sie das Schema in der serialisierten Daten enthalten sein muss finden Sie unter <a href="#Scenario4">Serialisierung mit Objekt Container-Dateien mit generische Eintrag</a> Beispiel.


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Beispiel 3: Serialisierung Objekt Container-Dateien und die mit Spiegelung verwenden

In diesem Beispiel ähnelt dem Szenario im <a href="#Scenario1">ersten Beispiel</a>, in dem das Schema implizit mit Spiegelung angegeben wird. Die Differenz, die hier ist, wird das Schema nicht als dem Leser bekannt sein, die es deserialisiert. Die **SensorData** Objekte serialisiert werden und deren implizit angegebene Schema werden in eine Avro Container Objektdatei dargestellt durch die [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) -Klasse gespeichert.

Die Daten werden in diesem Beispiel mit serialisiert [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) und deserialisierter mit [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Das Ergebnis wird dann mit der anfänglichen Instanzen um sicherzustellen, dass Identität verglichen.

Die Daten in der Container Objektdatei werden über die Standardeinstellung [**Deflate**] komprimiert[ deflate-100] Komprimierungscodec von .NET Framework 4. Finden Sie im <a href="#Scenario5">fünften wird</a> in diesem Artikel erfahren, wie eine neuere und übergeordnete Version von der [**Deflate**] verwenden[ deflate-110] Komprimierungscodec in .NET Framework 4.5 verfügbar.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Beispiel 4: Serialisierung mit Objekt Container-Dateien und die allgemeinen Datensatz

In diesem Beispiel ähnelt dem Szenario im <a href="#Scenario2">zweiten Beispiel</a>, in dem das Schema explizit mit JSON angegeben wird. Die Differenz, die hier ist, wird das Schema nicht als dem Leser bekannt sein, die es deserialisiert.

Testen der Datenmenge in einer Liste von Objekten [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) über ein explizit festgelegten JSON-Schema erfasst und dann in eine Container, dargestellt durch die [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) Klasse Objektdatei gespeichert. Diese Containerdatei erstellt ein Autor, mit dem Serialisieren die Daten, die nicht komprimiert, in einen Arbeitsspeicherstream, der dann in eine Datei gespeichert ist. Der [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) -Parameter, die für das Erstellen des Readers verwendet gibt an, dass diese Daten nicht komprimiert werden sollen.

Die Daten aus der Datei gelesen und in eine Auflistung von Objekten deserialisiert dann. Die ursprüngliche Liste der Avro Einträge, um zu bestätigen, dass sie identisch sind, wird dieser Websitesammlung verglichen.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Beispiel 5: Serialisierung Objekt Container-Dateien mit einem benutzerdefinierten Komprimierungscodec verwenden

Das fünfte Beispiel zeigt Gewusst wie einen Komprimierungscodec benutzerdefinierte für Avro Objekt Container-Dateien verwendet werden soll. Eine Stichprobe, die mit dem Code in diesem Beispiel kann der [Azure Codebeispielen](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) -Website heruntergeladen werden.

Der [Avro Spezifikation](http://avro.apache.org/docs/current/spec.html#Required+Codecs) ermöglicht die Verwendung der eine optionale Komprimierungscodec (zusätzlich zu **Null** und **Deflate** Standardeinstellungen). In diesem Beispiel ist einen vollständig neuen Codec wie Snappy (als einen unterstützten optional Codec in der [Spezifikation Avro](http://avro.apache.org/docs/current/spec.html#snappy)erwähnt) nicht implementieren. Es veranschaulicht, wie die .NET Framework 4.5 Durchführung der [**Deflate**] [ deflate-110] Codec, die einen besseren Komprimierungsalgorithmus basierend auf [die Komprimierung als die Standardversion von .NET Framework 4](http://zlib.net/) bereitstellt.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Beispiel 6: Hochladen der Daten für den Dienst Microsoft Azure HDInsight mithilfe von Avro

Im sechste Beispiel veranschaulicht einige programming Techniken im Zusammenhang mit der Interaktion mit dem Dienst Azure HDInsight. Eine Stichprobe, die mit dem Code in diesem Beispiel kann der [Azure Codebeispielen](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) -Website heruntergeladen werden.

Im Beispiel geschieht Folgendes:

* Herstellen der Verbindung zu einem vorhandenen HDInsight Dienst Cluster.
* Serialisiert mehrere CSV-Dateien und das Ergebnis auf Azure Blob-Speicher hochgeladen wird. (CSV-Dateien zusammen mit der Stichprobe verteilt werden und ein Extrahieren von American Express Stock zurückliegende Daten nach [Infochimps](http://www.infochimps.com/) für den Zeitraum 1970-2010 verteilt darstellen. Im Beispiel liest CSV-Dateidaten, die Datensätze zu Instanzen der Klasse **Stock** konvertiert und anschließend mithilfe von Spiegelung wird serialisiert. Definition der vordefinierten Typ wird aus einem JSON-Schema über dem Microsoft Avro Library Code Generation Programm erstellt.
* Erstellt eine neue externe Tabelle namens **Bestände** in Struktur und Links zu den Daten im vorherigen Schritt hochladen.
* Führt eine Abfrage mithilfe von Struktur über der Tabelle **Bestände** an.

Darüber hinaus führt die Stichprobe eine Prozedur bereinigen vor und nach der wichtigsten Vorgänge. Während der bereinigen werden alle zugehörigen Azure Blob-Daten und Ordner entfernt, und die strukturtabelle gelöscht. Sie können auch das Bereinigen Verfahren über die Befehlszeile Stichprobe aufrufen.

Im Beispiel weist die folgenden Komponenten:

* Ein aktives Microsoft Azure-Abonnement und dessen Abonnement-ID.
* Ein Management Zertifikat für das Abonnement mit dem zugehörigen privaten Schlüssel. Das Zertifikat sollte im aktuellen Benutzer privaten Speicher auf dem Computer verwendet, um die Ausführung des Beispiels installiert werden.
* Eine aktive HDInsight Cluster.
* Ein Azure-Speicher-Konto verknüpft zum HDInsight Cluster aus der vorherigen Voraussetzung, zusammen mit den entsprechenden primären oder sekundären Zugriffstaste.

Alle Informationen über die erforderlichen Komponenten sollten in der Stichprobe Konfigurationsdatei eingegeben werden, bevor das Beispiel ausgeführt wird. Es gibt zwei mögliche Vorgehensweisen dafür ein:

* Bearbeiten Sie die App in der Stichprobe Stammverzeichnis und erstellen Sie das Beispiel
* Erstellen Sie zuerst das Beispiel, und bearbeiten Sie AvroHDISample.exe.config im Build-Verzeichnis

In beiden Fällen alle Bearbeitungen Vorgang sollte der **<appSettings>** Einstellungsabschnitt. Führen Sie die Kommentare in der Datei.
Das Beispiel ist über die Befehlszeile auszuführen, indem Sie den folgenden Befehl ausführen (Stelle, an der die ZIP-Datei mit der Stichprobe C:\AvroHDISample; extrahiert werden angenommen wurde, wenn den relevanten Dateipfad andernfalls verwenden):

    AvroHDISample run C:\AvroHDISample\Data

Um den Cluster bereinigen, führen Sie den folgenden Befehl aus:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
