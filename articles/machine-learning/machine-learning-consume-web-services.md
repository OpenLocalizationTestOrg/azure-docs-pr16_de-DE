<properties
    pageTitle="Einen Computer Learning Webdienst nutzen | Microsoft Azure"
    description="Nachdem Sie ein Computer learning Dienst bereitgestellt wird, kann der Rest-Webdienst, der zur Verfügung gestellt wird als Anforderung / Antwort-Dienst oder als Stapel Ausführung Dienst genutzt werden."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="garye" />


# <a name="how-to-consume-an-azure-machine-learning-web-service-that-has-been-deployed-from-a-machine-learning-experiment"></a>Wie Sie einen Azure maschinellen Learning Webdienst nutzen, der von einem Computer Learning experimentieren bereitgestellt wurde

## <a name="introduction"></a>Einführung

Wenn als Webdienst bereitgestellt, bieten Azure maschinellen Learning Versuche, dass ein REST-API und JSON Nachrichten formatiert, die von einer Vielzahl von Geräten und Plattformen genutzt werden können. Das Azure maschinellen Learning-Portal enthält Code, der zum Aufrufen des Webdiensts in R, c# und Python verwendet werden kann. 

Dienste können in einer beliebigen Programmiersprache und von jedem Gerät, das drei Kriterien erfüllt aufgerufen werden:

* Verfügt über eine Netzwerkverbindung
* Verfügt über SSL-Funktionen, die zum Ausführen von HTTPS-Anfragen
* JSON (von Hand oder Support-Bibliotheken) können analysiert werden.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Ein Azure Computer Learning Webdienst kann auf zwei Arten als Anforderung / Antwort-Dienst oder als Stapel Ausführung Dienst genutzt werden. In jedem Szenario wird die Funktionalität über den Rest Webdienst bereitgestellt, die für die Ernährung zur Verfügung gestellt wird, nachdem Sie die experimentieren bereitstellen.

> [AZURE.TIP] Eine einfache Möglichkeit zum Erstellen einer Web-app, um den Vorhersage Webdienst zuzugreifen finden Sie unter [verbrauchen einen Azure maschinellen Learning Webdienst mit einer Web app-Vorlage](machine-learning-consume-web-service-with-web-app-template.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning Web service endpoints using the REST API, see **Azure machine learning Web service endpoints**.
-->

Informationen zum Erstellen und Bereitstellen eines Azure maschinellen Learning-Webdiensts, finden Sie unter [Bereitstellen eines Azure maschinellen Learning-Webdiensts][publish]. Eine schrittweise Anleitung durch einen Computer Learning experimentieren erstellen und Bereitstellen sondern, finden Sie unter [Entwicklung einer Vorhersage Lösung mithilfe von Azure maschinellen Learning][walkthrough].

## <a name="request-response-service-rrs"></a>Anforderung / Antwort-Dienst (RRS)

Eine Anforderung-Antwort-Dienst (RRS) ist ein Webdienst Tiefst-Wartezeit, hochgradig skalierbare verwendet, um eine Schnittstelle zu der statusfreien Datenmodellen bereitzustellen, die aus einem Versuch, Azure Computer Learning Studio bereitgestellt und erstellt wurden. Szenarios, in dem die Anwendung in Anspruch nehmen eine Antwort in Echtzeit erwartet, sind möglich.

RRS akzeptiert eine einzelne Zeile oder mehrere Zeilen der Eingabeparameter und eine einzelne Zeile oder mehrere Zeilen in der Ausgabe generieren kann. Die Ausgabe Zeile(n) kann mehrere Spalten enthalten.

Ein Beispiel für RRS überprüft die Echtheit der Anwendung. In diesem Fall können mehrere Hundert Millionen von Installationen von Anwendung erwartet werden. Wenn die Anwendung gestartet wird, wird den RRS-Dienst mit der relevanten Eingabe aufgerufen. Die Anwendung dann empfängt eine Überprüfung Antwort vom Dienst, der entweder zulässt oder die Anwendung von Durchführung blockiert.


## <a name="batch-execution-service-bes"></a>Stapel Ausführung Service (l)

Ein Stapel Ausführung Service (l) ist ein Dienst, der hohes Volumen, asynchrone, verarbeitet Bewerten eines Stapels von Datensätzen. Die Eingabe für die l enthält eine Reihe von Datensätzen aus verschiedenen Quellen, z. B. Blobs, Tabellen in Azure, SQL Azure, HDInsight (Ergebnisse einer Abfrage Struktur, beispielsweise) und HTTP-Quellen. Die Ausgabe für die l enthält die Ergebnisse der bewerten. Ergebnisse werden in eine Datei im Azure BLOB-Speicher ausgeben und Daten aus den Endpunkt Speicher werden in der Antwort zurückgegeben.

Eine l würde nützlich, wenn Antworten nicht sofort, benötigt werden wie für das regelmäßige geplant für Einzelpersonen oder Internet Dinge (IOT) Geräte bewerten.

## <a name="examples"></a>Beispiele

Um anzuzeigen, wie sowohl RRS und l arbeiten, verwenden wir ein Beispiel Azure-Webdiensts ein. Dieser Dienst würde in einem Szenario IOT (Internet der Dinge) verwendet werden. Einfache, behalten unsere Gerät sendet nur nach einem Wert, `cog_speed`, und dieselbe Antwort wieder erhält.

Nachdem der Versuch bereitgestellt wurde, gibt es vier Arten von Informationen zu den RRS oder l Serviceanfrage müssen wir ein.

* Der Dienst **API-Schlüssel** oder **Primärschlüssel**
* Der Dienst **Anfrage-URI**
* Die erwarteten API **Anforderung Überschriften** und **Textkörper**
* Die erwarteten API **Antwort Überschriften** und **Textkörper**

Welche Art von Dienst, Sie bereitgestellt die Art und Weise, in dem Sie diese Informationen finden, hängt: A neue Webdienst oder einem klassischen Webdienst.

### <a name="information-location-in-the-azure-machine-learning-web-services-portal"></a>Speicherort der Informationen im Portal Azure maschinellen Learning-Webdiensten 

Um die benötigten Informationen zu finden:

1. Melden Sie sich bei der [Azure Computer Learning-Webdiensten] [ webservicesportal] Portal.
2. Klicken Sie auf **Web Services** oder **klassische Webdienste**.
3. Klicken Sie auf den Webdienst, mit denen Sie arbeiten. 
4. Wenn Sie mit einem klassischen Webdienst arbeiten, klicken Sie auf den Endpunkt, mit dem Sie arbeiten.

Die Informationen, befindet sich diese Seiten:

* Der **Primärschlüssel** ist auf der Seite **verbrauchen** verfügbar
* Die **Anfrage-URI** ist auf der Seite **verbrauchen** verfügbar 
* **Die erwarteten API Anforderung Kopf-** **Antwort Überschriften**und **Textkörper** stehen auf der Seite **Swagger API**

### <a name="information-locations-in-machine-learning-studio-classic-web-service-only"></a>Speicherorte der Informationen in Computer Learning Studio (nur klassische Webdienst)

Sie finden die benötigte Informationen aus zwei Stellen zu finden: Computer Learning Studio oder im Portal Azure maschinellen Learning-Webdiensten.

So finden Sie die benötigte Informationen in den Computer Learning studio

1. Melden Sie sich bei [maschinellen Learning Studio][mlstudio].
2. Klicken Sie auf der linken Seite des Bildschirms auf **Webdienste**.
3. Klicken Sie auf den Webdienst, mit dem Sie arbeiten. 

Die Informationen, befindet sich diese Seiten:

* Die **API-Schlüssel** steht **Dashboard** -Dienst. 
* Die **Anfrage-URI** ist auf der Hilfeseite API verfügbar
* **Die erwarteten API Anforderung Kopf-** **Antwort Überschriften**und **Textkörper** stehen auf der Hilfeseite API


Um auf die Hilfeseite API zuzugreifen, klicken Sie auf die **Anforderung/Antwort** oder **Stapel Ausführung** Link abhängig von der Aufgabe.

So suchen Sie die benötigte Informationen im Portal Azure maschinellen Learning-Webdiensten

1. Melden Sie sich bei der [Azure maschinellen Learning-Webdiensten] [ webservicesportal] Portal.
2. Klicken Sie auf **klassische Webdienste**.
3. Klicken Sie auf den Webdienst, mit dem Sie arbeiten. 
4. Klicken Sie auf den Endpunkt, mit dem Sie arbeiten.

Die Informationen befindet sich diese Seiten:

* Der **Primärschlüssel** ist auf der Seite **verbrauchen** verfügbar
* Die **Anfrage-URI** ist auf der Seite **verbrauchen** verfügbar 
* **Die erwarteten API Anforderung Kopf-** **Antwort Überschriften**und **Textkörper** stehen auf der Seite **Swagger API**

In den beiden folgenden Beispielen wird die C#-Sprache zum veranschaulichen des Codes erforderlich.

### <a name="rrs-example"></a>RRS-Beispiel



Das folgende Beispiel Anforderung zeigt der Eingabe API den Inhalt für den Anruf API unseres Diensts Stichprobe. Bei einem klassischen Webdienst finden Nutzlast Beispiele auf der **Seite "Hilfe" API** oder auf der Seite **Swagger API** des Portals Computer Learning Web Services Sie. Für einen neuen Webdienst finden Sie auf der Seite **Swagger API** des Portals maschinellen Learning-Webdiensten Nutzlast Beispiele.

**Beispiel für eine Anforderung**

    {
      "Inputs": {
        "input1": {
          "ColumnNames": [
            "cog_speed"
          ],
          "Values": [
            [
              "0"
            ],
            [
              "1"
            ]
          ]
        }
      },
      "GlobalParameters": {}
    }


Im folgende Beispiel wird auf ähnliche Weise die Antwort API für den Dienst.

**Beispiel für Antwort**

    {
      "Results": {
        "output1": {
          "type": "DataTable",
          "value": {
            "ColumnNames": [
              "cog_speed"
            ],
            "ColumnTypes": [
              "Numeric"
            ].
          "Values": [
            [
              "0"
            ],
            [
              "1"
            ]
          ]
        }
      },
      "GlobalParameters": {}
    }

Im folgenden finden im Beispiel für die C#-Implementierung. Bei einem klassischen Webdienst finden Sie Codebeispielen am unteren Rand der **Seite "Hilfe" API** oder am unteren Rand der Seite **verbrauchen** . Sie können für einen neuen Webdienst Codebeispielen am unteren Rand der Seite **verbrauchen** suchen.

**Beispiel-Code in c#**

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Formatting;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;

    namespace CallRequestResponseService
    {
        public class StringTable
        {
            public string[] ColumnNames { get; set; }
            public string[,] Values { get; set; }
        }

        class Program
        {
            static void Main(string[] args)
            {
                InvokeRequestResponseService().Wait();
            }

            static async Task InvokeRequestResponseService()
            {
                using (var client = new HttpClient())
                {
                    var scoreRequest = new
                    {
                        Inputs = new Dictionary<string, StringTable> () {
                            {
                                "input1",
                                new StringTable()
                                {
                                    ColumnNames = new string[] {"cog_speed"},
                                    Values = new string[,] {  { "0"},  { "1"}  }
                                }
                            },
                        GlobalParameters = new Dictionary<string, string>() { }
                    };

                    const string apiKey = "abc123"; // Replace this with the API key for the Web service
                    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);

                    client.BaseAddress = new Uri("https://ussouthcentral.services.azureml.net/workspaces/<workspace id>/services/<service id>/execute?api-version=2.0&details=true");

                    // WARNING: The 'await' statement below can result in a deadlock if you are calling this code from the UI thread of an ASP.Net application.
                    // One way to address this would be to call ConfigureAwait(false) so that the execution does not attempt to resume on the original context.
                    // For instance, replace code such as:
                    //      result = await DoSomeTask()
                    // with the following:
                    //      result = await DoSomeTask().ConfigureAwait(false)

                    HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

                    if (response.IsSuccessStatusCode)
                    {
                        string result = await response.Content.ReadAsStringAsync();
                        Console.WriteLine("Result: {0}", result);
                    }
                    else
                    {
                        Console.WriteLine("Failed with status code: {0}", response.StatusCode);
                    }
                }
            }
        }
    }

**Beispielcode in Java**

Der folgende Code veranschaulicht eine Anforderung REST-API in Java erstellen. Es wird davon ausgegangen, dass die Variablen (Apikey und Apiurl) über die notwendigen API Details und die Variable JsonBody hat eine richtige JSON-Objekt aus, wie erwartet durch die REST-API. Sie können den vollständigen Code aus Github - [https://github.com/nk773/AzureML_RRSApp](https://github.com/nk773/AzureML_RRSApp)herunterladen. In diesem Beispiel Java erfordert [Apache HTTP-Client-Bibliothek](https://hc.apache.org/downloads.cgi).

    /**
     * Download full code from github - [https://github.com/nk773/AzureML_RRSApp](https://github.com/nk773/AzureML_RRSApp)
     */
        /**
          * Call REST API for retrieving prediction from Azure ML 
          * @return response from the REST API
          */    
        public static String rrsHttpPost() {
        
            HttpPost post;
            HttpClient client;
            StringEntity entity;
        
            try {
                    // create HttpPost and HttpClient object
                    post = new HttpPost(apiurl);
                    client = HttpClientBuilder.create().build();
            
                    // setup output message by copying JSON body into 
                    // apache StringEntity object along with content type
                    entity = new StringEntity(jsonBody, HTTP.UTF_8);
                    entity.setContentEncoding(HTTP.UTF_8);
                    entity.setContentType("text/json");

                    // add HTTP headers
                    post.setHeader("Accept", "text/json");
                    post.setHeader("Accept-Charset", "UTF-8");
        
                    // set Authorization header based on the API key
                    post.setHeader("Authorization", ("Bearer "+apikey));
                    post.setEntity(entity);

                    // Call REST API and retrieve response content
                    HttpResponse authResponse = client.execute(post);
            
                    return EntityUtils.toString(authResponse.getEntity());
            
            }
            catch (Exception e) {
            
                    return e.toString();
            }
    
        }
    
        
 

### <a name="bes-example"></a>Beispiel für l

Im Gegensatz zu den Dienst RRS ist der Dienst l asynchrone. Dies bedeutet, dass der l-API einfach Warteschlange befindet, um eine Aufgabe, die ausgeführt werden, und der Anrufer fragt den Projektstatus angezeigt, wenn sie durchgeführt wurde. Hier sind die Vorgänge, die derzeit für Stapelverarbeitung unterstützt:

1. Erstellen (senden) einer Stapelverarbeitung
1. Starten Sie diese Stapelverarbeitung
1. Erhalten Sie den Status oder das Ergebnis der Stapelverarbeitung
1. Abbrechen einer laufenden Stapelverarbeitung

**1. erstellen Sie 1. eine Ausführung Stapelverarbeitung**

Wenn Sie eine Stapelverarbeitung des Diensts Azure maschinellen Learning erstellen, können Sie mehrere Parameter angeben, die zur Ausführung des Stapels definieren:

* **Eingabe**: entspricht ein, in dem die Stapelverarbeitung Eingabe des, Blob-Verweis wird gespeichert.
* **GlobalParameters**: stellt die Gruppe der globalen Parameter, die für ihre experimentieren definiert werden können. Ein Azure maschinellen Learning experimentieren kann haben beide erforderlich und optionale Parameter, die Ausführung des Diensts und dem Anrufer anpassen wird erwartet, dass alle erforderlichen Parameter bereitstellt falls zutreffend. Diese Parameter werden als eine Auflistung von Schlüssel-Wert-Paare angegeben.
* **Gibt**: Wenn Sie der Dienst eine oder mehrere Ausgaben festgelegt wurden, der Anrufer kann umleiten jede hiervon an einem Speicherort Azure Blob. Durch die Einstellung für diesen Parameter an, Sie können die Ausgabe des Diensts in einen bevorzugten Speicherort und unter einem vorhersehbar Namen speichern, andernfalls wird der Name der Blob zufällig generiert. 

    Der Dienst erwartet die Ausgabe Inhalte basierend auf deren Typ als unterstützten Formate gespeichert werden:
  - Gibt Dataset: als **CSV, TSV, .arff** gespeichert werden können
  - Modell Ausgaben gelernt: muss als **.ilearner** gespeichert werden

    Sie geben die Ausgabe Speicherort überschreibt als eine Auflistung von Ausgabenamen an, oder BLOB-Referenz-Paare. Den *Ausgabenamen* ist die benutzerdefinierten Namen für einen bestimmten Ausgabeknoten und *Blob Bezug* ist ein Bezug auf eine Azure Blob-Position, an die die Ausgabe umgeleitet wird. Den *Ausgabenamen* wird auf der Seite des Diensts API "Hilfe" angezeigt.

Alle Parameter der Erstellung der Position sind je nach Art der Ihrem Dienst optional. Beispielsweise erforderlich Services mit kein Eingabewerte Knoten definiert keine Übergabe in *einem Eingabeparameter* . Entsprechend ist das Feature Ausgabe Standort Außerkraftsetzung optional, wie Ausgaben andernfalls in Standardkonto Storage gespeichert sind, die für den Arbeitsbereich Azure maschinellen Learning eingerichtet wurde. Im folgenden Beispiel Anforderung Fracht für einen Dienst, in dem nur die eingegebenen Informationen bereitgestellt wird:

**Beispiel für eine Anforderung**

    {
      "Input": {
        "ConnectionString":     
        "DefaultEndpointsProtocol=https;AccountName=mystorageacct;AccountKey=mystorageacctKey",
        "RelativeLocation": "/mycontainer/mydatablob.csv",
        "BaseLocation": null,
        "SasBlobToken": null
      },
      "Outputs": null,
      "GlobalParameters": null
    }

Die Antwort auf die Stapel Auftrag Erstellung API ist die eindeutige Auftrags-ID, die mit Ihrem Projekt verknüpft ist. Diese ID ist wichtig, da es die einzige Möglichkeit für dieses Projekt in dem System für andere Vorgänge verweisen auf bereitstellt.  

**Beispiel für Antwort**

    "539d0bc2fde945b6ac986b851d0000f0" // The JOB_ID

**2. Starten Sie eine Ausführung Stapelverarbeitung**

Beim Erstellen einer Stapelverarbeitung im System registriert ist, und stellen es in einem Zustand *nicht gestartet* . Wenn Sie um den Auftrag für die Ausführung tatsächlich zu planen, Sie **Starten** auf den Dienstendpunkt API Hilfeseite oder den Webdienst Swagger API beschrieben API aufrufen und geben Sie den Auftrag ID erhalten, wenn Sie der Auftrag erstellt wurde.

**3 holen Sie 3 sich den Status der Ausführung Stapelverarbeitung**

Sie können den Status Ihrer asynchrone Stapelverarbeitung zu einem beliebigen Zeitpunkt abrufen, indem Sie das Job ID an *GetJobStatus* API übergeben. Die Antwort API enthält einen Indikator für den Auftrag des aktuellen Status und die Ergebnisse der Stapelverarbeitung angibt, ob sie erfolgreich durchgeführt wurde. Ist ein Fehler, wird in der Eigenschaft *Details* Weitere Informationen zu den tatsächlichen Grund hinter den Fehler zurückgegeben, wie hier dargestellt:

**Antwort Nutzlast**

    {
        "StatusCode": STATUS_CODE,
        "Results": RESULTS,
        "Details": DETAILS
    }

*StatusCode* kann eine der folgenden Aktionen aus:

* Nicht gestartet
* Ausführen
* Fehler beim
* Abgebrochen
* Abgeschlossen

Die *Ergebnisse* -Eigenschaft wird aufgefüllt, wenn Sie der Auftrag erfolgreich abgeschlossen wurde (es ist **null** andernfalls.) Nachdem der Job abgeschlossen ist, und der Dienst mindestens einen Ausgabeknoten definiert wurde, werden die Ergebnisse als eine Zusammenstellung von *[Ausgabename, Blob Referenz]* Paare zurückgegeben, in dem die Blob-Referenz SAS schreibgeschützt Verweis auf die Blob, das das Ergebnis ist.

**Beispiel für Antwort**

    {
        "Status Code": "Finished",
        "Results":
        {
            "dataOutput":
            {              
                "ConnectionString": null,
                "RelativeLocation": "outputs/dataOutput.csv",
                "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
                "SasBlobToken": "?sv=2013-08-15&sr=b&sig=ABCD&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
            },
            "trainedModelOutput":
            {              
                "ConnectionString": null,
                "RelativeLocation": "models/trainedModel.ilearner",
                "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
                "SasBlobToken": "?sv=2013-08-15&sr=b&sig=EFGH%3D&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
            },           
        },
        "Details": null
    }

**4. Abbrechen einer Ausführung Stapelverarbeitung**

Sie können eine laufenden Stapelverarbeitung zu einem beliebigen Zeitpunkt abbrechen, indem Sie aufrufen vorgesehenen *CancelJob* API und dem Job Id übergeben. Sie möglicherweise aus verschiedenen Gründen kündigen, wie der Auftrag zu lange ausführt.

#### <a name="using-the-bes-sdk"></a>Mithilfe des SDK l

Das [l SDK Nuget-Paket](http://www.nuget.org/packages/Microsoft.Azure.MachineLearning/) enthält Funktionen, die einen l um im Stapelverarbeitungsmodus bewerten zu vereinfachen. Zum Installieren des Nuget-Pakets in Visual Studio im Menü **Extras** **Nuget-Paket-Manager** wählen Sie aus, und klicken Sie auf **Paket-Manager-Konsole**.

Azure maschinellen Learning Versuche, die als Webdienste bereitgestellt werden können Web Service Eingabewerte Module einbeziehen. Dies bedeutet, dass die Eingabe in den Webdienst erfolgt über einen Web Service-Anruf in Form von ein Bezug auf einen Blob-Speicherort. Es gibt auch die Möglichkeit, nicht mithilfe eines Web Service Eingabewerte Moduls und verwenden stattdessen ein Modul **Daten importieren** . In diesem Fall liest das Modul **Importieren von Daten** aus einer Datenquelle, z. B. einer SQL-DB mithilfe einer Abfrage zur Laufzeit ein. Parameter der Web-Dienst können dynamisch verweisen auf anderen Servern oder Tabellen usw. verwendet werden. Das SDK unterstützt beide Vorgehensweisen.

Im folgenden Beispiel veranschaulicht, wie übermitteln und eine Stapelverarbeitung bei einem Computer-Schulung Azure Service mit dem SDK l überwachen. Die Kommentare enthalten Details auf Einstellungen und Anrufe.

#### <a name="sample-code"></a>**Beispiel-Code**

    // This code requires the Nuget package Microsoft.Azure.MachineLearning to be installed.
    // Instructions for doing this in Visual Studio:
    // Tools -> Nuget Package Manager -> Package Manager Console
    // Install-Package Microsoft.Azure.MachineLearning

      using System;
      using System.Collections.Generic;
      using System.Threading.Tasks;

      using Microsoft.Azure.MachineLearning;
      using Microsoft.Azure.MachineLearning.Contracts;
      using Microsoft.Azure.MachineLearning.Exceptions;

    namespace CallBatchExecutionService
    {
        class Program
        {
            static void Main(string[] args)
            {               
                InvokeBatchExecutionService().Wait();
            }

            static async Task InvokeBatchExecutionService()
            {
                // First collect and fill in the URI and access key for your Web service endpoint.
                // These are available on your service's API help page.
                var endpointUri = "https://ussouthcentral.services.azureml.net/workspaces/YOUR_WORKSPACE_ID/services/YOUR_SERVICE_ENDPOINT_ID/";
                string accessKey = "YOUR_SERVICE_ENDPOINT_ACCESS_KEY";

                // Create an Azure Machine Learning runtime client for this endpoint
                var runtimeClient = new RuntimeClient(endpointUri, accessKey);

                // Define the request information for your batch job. This information can contain:
                // -- A reference to the AzureBlob containing the input for your job run
                // -- A set of values for global parameters defined as part of your experiment and service
                // -- A set of output blob locations that allow you to redirect the job's results

                // NOTE: This sample is applicable, as is, for a service with explicit input port and
                // potential global parameters. Also, we choose to also demo how you could override the
                // location of one of the output blobs that could be generated by your service. You might
                // need to tweak these features to adjust the sample to your service.
                //
                // All of these properties of a BatchJobRequest shown below can be optional, depending on
                // your service, so it is not required to specify all with any request.  If you do not want to
                // use any of the parameters, a null value should be passed in its place.

                // Define the reference to the blob containing your input data. You can refer to this blob by its
                    // connection string / container / blob name values; alternatively, we also support references
                    // based on a blob SAS URI

                    BlobReference inputBlob = BlobReference.CreateFromConnectionStringData(connectionString:                                         "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                        containerName: "YOUR_CONTAINER_NAME",
                        blobName: "YOUR_INPUT_BLOB_NAME");

                    // If desired, one can override the location where the job outputs are to be stored, by passing in
                    // the storage account details and name of the blob where we want the output to be redirected to.

                    var outputLocations = new Dictionary<string, BlobReference>
                        {
                          {
                           "YOUR_OUTPUT_NODE_NAME",
                           BlobReference.CreateFromConnectionStringData(                                     connectionString: "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                                containerName: "YOUR_CONTAINER_NAME",
                                blobName: "YOUR_DESIRED_OUTPUT_BLOB_NAME")
                           }
                        };

                // If applicable, you can also set the global parameters for your service
                var globalParameters = new Dictionary<string, string>
                {
                    { "YOUR_GLOBAL_PARAMETER", "PARAMETER_VALUE" }
                };

                var jobRequest = new BatchJobRequest
                {
                    Input = inputBlob,
                    GlobalParameters = globalParameters,
                    Outputs = outputLocations
                };

                try
                {
                    // Register the batch job with the system, which will grant you access to a job object
                    BatchJob job = await runtimeClient.RegisterBatchJobAsync(jobRequest);

                    // Start the job to allow it to be scheduled in the running queue
                    await job.StartAsync();

                    // Wait for the job's completion and handle the output
                    BatchJobStatus jobStatus = await job.WaitForCompletionAsync();
                    if (jobStatus.JobState == JobState.Finished)
                    {
                        // Process job outputs
                        Console.WriteLine(@"Job {0} has completed successfully and returned {1} outputs", job.Id, jobStatus.Results.Count);
                        foreach (var output in jobStatus.Results)
                        {
                            Console.WriteLine(@"\t{0}: {1}", output.Key, output.Value.AbsoluteUri);
                        }
                    }
                    else if (jobStatus.JobState == JobState.Failed)
                    {
                        // Handle job failure
                        Console.WriteLine(@"Job {0} has failed with this error: {1}", job.Id, jobStatus.Details);
                    }
                }
                catch (ArgumentException aex)
                {
                    Console.WriteLine("Argument {0} is invalid: {1}", aex.ParamName, aex.Message);
                }
                catch (RuntimeException runtimeError)
                {
                    Console.WriteLine("Runtime error occurred: {0} - {1}", runtimeError.ErrorCode, runtimeError.Message);
                    Console.WriteLine("Error details:");
                    foreach (var errorDetails in runtimeError.Details)
                    {
                        Console.WriteLine("\t{0} - {1}", errorDetails.Code, errorDetails.Message);
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Unexpected error occurred: {0} - {1}", ex.GetType().Name, ex.Message);
                }
            }
        }
    }

#### <a name="sample-code-in-java-for-bes"></a>Beispielcode in Java für l

Stapel Ausführung Dienst REST-API dauert die JSON ein Bezug auf eine Stichprobe von CSV- und eine Ausgabe Stichprobe Csv und aus, wie im folgenden Beispiel dargestellt, und eine Position in der Azure-ML, führen Sie die Stapel Vorhersagen erstellt. Sie können den vollständigen Code in [Github](https://github.com/nk773/AzureML_BESApp/tree/master/src/azureml_besapp)anzeigen. In diesem Beispiel Java erfordert [Apache HTTP-Client-Bibliothek](https://hc.apache.org/downloads.cgi). 


    { "GlobalParameters": {}, 
        "Inputs": { "input1": { "ConnectionString":     "DefaultEndpointsProtocol=https;
            AccountName=myAcctName; AccountKey=Q8kkieg==", 
            "RelativeLocation": "myContainer/sampleinput.csv" } }, 
        "Outputs": { "output1": { "ConnectionString":   "DefaultEndpointsProtocol=https;
            AccountName=myAcctName; AccountKey=kjC12xQ8kkieg==", 
            "RelativeLocation": "myContainer/sampleoutput.csv" } } 
    } 


##### <a name="create-a-bes-job"></a>Erstellen Sie einen Auftrag l  
        
        /**
         * Call REST API to create a job to Azure ML 
         * for batch predictions
         * @return response from the REST API
         */ 
        public static String besCreateJob() {
            
            HttpPost post;
            HttpClient client;
            StringEntity entity;
            
            try {
                // create HttpPost and HttpClient object
                post = new HttpPost(apiurl);
                client = HttpClientBuilder.create().build();
                
                // setup output message by copying JSON body into 
                // apache StringEntity object along with content type
                entity = new StringEntity(jsonBody, HTTP.UTF_8);
                entity.setContentEncoding(HTTP.UTF_8);
                entity.setContentType("text/json");
    
                // add HTTP headers
                post.setHeader("Accept", "text/json");
                post.setHeader("Accept-Charset", "UTF-8");
            
                // set Authorization header based on the API key
                // note a space after the word "Bearer " - don't miss that
                post.setHeader("Authorization", ("Bearer "+apikey));
                post.setEntity(entity);
    
                // Call REST API and retrieve response content
                HttpResponse authResponse = client.execute(post);
                
                jobId = EntityUtils.toString(authResponse.getEntity()).replaceAll("\"", "");
                
                
                return jobId;
                
            }
            catch (Exception e) {
                
                return e.toString();
            }
        
        }
        
##### <a name="start-a-previously-created-bes-job"></a>Starten einer zuvor erstellten Auftrag "l"        
    
        /**
         * Call REST API for starting prediction job previously submitted 
         * 
         * @param job job to be started 
         * @return response from the REST API
         */ 
        public static String besStartJob(String job){
            HttpPost post;
            HttpClient client;
            StringEntity entity;
            
            try {
                // create HttpPost and HttpClient object
                post = new HttpPost(startJobUrl+"/"+job+"/start?api-version=2.0");
                client = HttpClientBuilder.create().build();
             
                // add HTTP headers
                post.setHeader("Accept", "text/json");
                post.setHeader("Accept-Charset", "UTF-8");
            
                // set Authorization header based on the API key
                post.setHeader("Authorization", ("Bearer "+apikey));
    
                // Call REST API and retrieve response content
                HttpResponse authResponse = client.execute(post);
                
                if (authResponse.getEntity()==null)
                {
                    return authResponse.getStatusLine().toString();
                }
                
                return EntityUtils.toString(authResponse.getEntity());
                
            }
            catch (Exception e) {
                
                return e.toString();
            }
        }
##### <a name="cancel-a-previously-created-bes-job"></a>Abbrechen einer zuvor erstellten Auftrag "l"
        
        /**
         * Call REST API for canceling the batch job 
         * 
         * @param job job to be started 
         * @return response from the REST API
         */ 
        public static String besCancelJob(String job) {
            HttpDelete post;
            HttpClient client;
            StringEntity entity;
            
            try {
                // create HttpPost and HttpClient object
                post = new HttpDelete(startJobUrl+job);
                client = HttpClientBuilder.create().build();
             
                // add HTTP headers
                post.setHeader("Accept", "text/json");
                post.setHeader("Accept-Charset", "UTF-8");
            
                // set Authorization header based on the API key
                post.setHeader("Authorization", ("Bearer "+apikey));
    
                // Call REST API and retrieve response content
                HttpResponse authResponse = client.execute(post);
             
                if (authResponse.getEntity()==null)
                {
                    return authResponse.getStatusLine().toString();
                }
                return EntityUtils.toString(authResponse.getEntity());
                
            }
            catch (Exception e) {
                
                return e.toString();
            }
        }
        
### <a name="other-programming-environments"></a>Andere programming Umgebungen

Sie können den Code auch in vielen anderen Sprachen, folgen die Anweisungen auf der Website [swagger.io](http://swagger.io/) bereitgestellten generieren. Für einen klassischen Webdienst können Sie das Dokument Swagger erhalten:

* Auf der Hilfeseite API 
* Telefonisch erhalten API Dokument für Endpunkt gefunden Sie auf der Seite Swagger API maschinellen Learning-Webdienste-Portal. 

Wechseln Sie zu der [swagger.io](http://swagger.io/swagger-codegen/) , und folgen Sie den Anweisungen Swagger Code, Java und Apache Mvn herunterladen. 

Die folgende Liste ist die Zusammenfassung der Anweisungen zum Einrichten von Swagger für andere programming Umgebungen.

* Sicherstellen Sie, dass Java 7 oder höher installiert ist
* Installieren von Apache Mvn (auf Ubuntu, können *apt Get installieren Mvn*)
* Gehe zu Github für swagger, und Laden Sie das Projekt Swagger als Zip-Datei
* Entzippen Sie ihn swagger
* Erstellen von Swagger Tools *Mvn Paket* aus der Swagger der Quellverzeichnis ausführen

Jetzt können Sie eines der Tools Swagger verwenden. Folgen Sie die Anweisungen, um Java-Client-Code generieren. 

* Wechseln Sie zu der Seite Azure ML API Hilfe (Beispiel [hier](https://studio.azureml.net/apihelp/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/jobs))
* Ermitteln der URL für swagger.json für Azure ML REST-APIs (zweites letzte Aufzählungszeichen oben auf der Seite "Hilfe" API)
* Klicken Sie auf die Dokumentverknüpfung Swagger (Beispiel [hier](https://management.azureml.net/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/apidocument))
* Verwenden Sie den folgenden Befehl zum Generieren des Codes Client wie in der [Info-Datei von Swagger](https://github.com/swagger-api/swagger-codegen/blob/master/README.md) dargestellt.

**Beispiel-Befehlszeile Client-Code generieren**

    java -jar swagger-codegen-cli.jar generate\
     -i https://ussouthcentral.services.azureml.net:443/workspaces/\
    fb62b56f29fc4ba4b8a8f900c9b89584/services/26a3afce1767461ab6e73d5a206fbd62/swagger.json\
     -l java -o /home/username/sample

* Kombinieren Sie die Werte in den Feldern-Host BasePath und "/ swagger.json" in der Stichprobe von einer Swagger [API Hilfeseite](https://management.azureml.net/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/apidocument) zum Erstellen abgebildet swagger URL in der vorstehenden Befehlszeile verwendet werden.

**Beispiel-API Hilfeseite**


    {
      "swagger": "2.0",
      "info": {
        "version": "2.0",
        "title": "Sample 5: Binary Classification with Web service: Adult Dataset [Predictive Exp.]",
        "description": "No description provided for this Web service.",
        "x-endpoint-name": "default"
      },
      "host": "ussouthcentral.services.azureml.net:443",
      "basePath": "/workspaces/afbd553b9bac4c95be3d040998943a4f/services/26a3afce1767461ab6e73d5a206fbd62",
      "schemes": [
        "https"
      ],
      "consumes": [
        "application/json"
      ],
      "produces": [
        "application/json"
      ],
      "paths": {
        "/swagger.json": {
          "get": {
            "summary": "Get swagger API document for the Web service",
            "operationId": "getSwaggerDocument",
            
<!-- Relative Links -->

[publish]: machine-learning-publish-a-machine-learning-web-service.md
[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- External Links -->
[webservicesportal]: https://services.azureml.net/
[mlstudio]: https://studio.azureml.net