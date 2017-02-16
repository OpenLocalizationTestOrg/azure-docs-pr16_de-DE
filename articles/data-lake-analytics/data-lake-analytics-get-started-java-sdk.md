<properties
   pageTitle="Verwenden Sie die Daten dem Analytics Java SDK zum Entwickeln von Applications | Azure"
   description="Verwenden Sie zum Entwickeln von Applications Azure Daten dem Analytics Java SDK"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-java-sdk"></a>Lernprogramm: Erste Schritte mit Azure Daten dem Analytics Java SDK verwenden

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Erfahren Sie, wie Sie mithilfe der Azure Daten dem Analytics Java SDK erstellen Sie ein Konto Azure Daten Sees und grundlegende Vorgänge, die beispielsweise als Erstellen von Ordnern, hochladen und Herunterladen von Datendateien, löschen Sie Ihr Konto und Arbeiten mit Projekten, ausführen. Weitere Informationen zu den Daten Lake finden Sie unter [Azure Daten dem Analytics](data-lake-analytics-overview.md).

In diesem Lernprogramm entwickeln Sie eine Java Console-Anwendung die Beispiele für allgemeine Verwaltungsaufgaben ebenso wie Testdaten erstellen und Senden eines Auftrags enthält.  Um durch die gleichen Lernprogramm mit anderen unterstützte Tools zu wechseln, klicken Sie auf den Registerkarten am oberen Rand der in diesem Abschnitt.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Java Development Kit (JDK) 8 (mit Java Version 1.8).
* IntelliJ oder eine andere geeignete Java Entwicklungsumgebung. Dies ist optional, aber empfohlen. Verwenden Sie die folgenden Anweisungen IntelliJ aus.
* **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
* **Aktivieren Sie Ihr Abonnement Azure** für Öffentliche Daten dem Analytics-Vorschau. [Anweisungen](data-lake-analytics-get-started-portal.md#signup)finden Sie unter.
* Erstellen einer Anwendung Azure Active Directory (AAD) und deren **Client-ID**, **Mandanten-ID**und **Schlüssel**abzurufen. Weitere Informationen zu AAD Applikationen und Anweisungen zum Abrufen einer Client-ID finden Sie unter [Erstellen von Active Directory-Anwendung und Dienst wichtigsten Portal verwenden](../resource-group-create-service-principal-portal.md). Die Antwort URI und Schlüssel auch zur Verfügung aus dem Portal Nachdem Sie die Anwendung erstellt und Schlüssel generiert haben.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Wie authentifiziert ich mit Azure Active Directory?

Der folgende Codeausschnitt stellt Code für **nicht interaktiven** Authentifizierung, in denen die Anwendung eine eigene Anmeldeinformationen bereitstellt.

Sie müssen Ihre Anwendung die Berechtigung zum Erstellen von Ressourcen in Azure in diesem Lernprogramm entwickelt. Es wird **dringend empfohlen** , dass Sie nur diese Anwendung Teilnehmerberechtigungen auf einer neue, leere und nicht verwendete Ressourcengruppe in Ihrem Azure-Abonnement für die Zwecke dieses Lernprogramms gewähren.

## <a name="create-a-java-application"></a>Erstellen einer Java-Anwendungs

1. Öffnen Sie IntelliJ, und Erstellen eines neuen Java-Projekts mithilfe der **Befehlszeile App** -Vorlage.

2. Mit der rechten Maustaste auf das Projekt auf der linken Seite des Bildschirms, und klicken Sie auf **Add Framework Support**. Wählen Sie **Maven** aus, und klicken Sie auf **OK**.

3. Öffnen Sie die Datei neu erstellten **"pom.xml"** , und fügen Sie den folgenden Codeausschnitt Text zwischen den ** \</Version >** Tag und die ** \</project >** Kategorie:

    Hinweis: Dieser Schritt ist temporäre, bis das Azure Daten dem Analytics SDK in Maven verfügbar ist. In diesem Artikel wird aktualisiert, nachdem das SDK in Maven verfügbar ist. Alle zukünftigen Updates für dieses SDK werden durch Maven verfügbar.

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-20160513.000802-24</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-runtime</artifactId>
                <version>1.0.0-20160513.000812-28</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.rest</groupId>
                <artifactId>client-runtime</artifactId>
                <version>1.0.0-20160513.000825-29</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-analytics</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>


4. Wechseln Sie zu der **Datei**, und klicken Sie dann **Einstellungen**und dann **Erstellen**, **Ausführung**, **Bereitstellung**. Wählen Sie **Tools erstellen**, **Maven**, **Importieren**aus. Aktivieren Sie dann **Importieren Maven Projekte automatisch**ein.

5. Öffnen Sie **Main.java** , und Ersetzen Sie den vorhandenen Codeblock mit den folgenden Code. Darüber hinaus geben Sie die Werte für Parameter, die Hervorhebung im Codeausschnitt, z. B. **LocalFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_resourceGroupName** und Ersetzen von Platzhaltern für **CLIENT-ID**, **CLIENT-geheim**, **MANDANTEN-ID**und **Abonnement-ID**.

    Dieser Code führt den Prozess der Lake Datenspeicher und Daten dem Analytics-Konten erstellen, Erstellen von Dateien im Speicher, Ausführen eines Auftrags, erste Projektstatus, Position Ausgabe herunterladen und schließlich beim Löschen des Kontos.

    >[AZURE.NOTE] Es gibt es zurzeit ein bekanntes Problem mit dem Azure Daten dem Dienst.  Wenn die app Stichprobe unterbrochen wird oder ein Fehler auftritt, müssen Sie die Konten Lake Datenspeicher und Daten dem Analytics manuell zu löschen, die das Skript erstellt.  Wenn Sie nicht mit dem Portal vertraut sind, erhalten der Leitfaden [Verwalten Azure Daten dem Analytics Azure-Portal mit](data-lake-analytics-manage-use-portal.md) Ihnen einen Einstieg.


        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.analytics.*;
        import com.microsoft.azure.management.datalake.analytics.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.nio.file.Files;
        import java.nio.file.Paths;
        import java.util.ArrayList;
        import java.util.UUID;
        import java.util.List;
        
        public class Main {
            private static String _adlsAccountName;
            private static String _adlaAccountName;
            private static String _resourceGroupName;
            private static String _location;
        
            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;
        
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
        
            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";
        
                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";
        
                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
        
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.
        
                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);
        
                // Create Data Lake Store and Analytics accounts
                WaitForNewline("Authenticated.", "Creating NEW accounts.");
                CreateAccounts();
                WaitForNewline("Accounts created.", "Displaying accounts.");
        
                // List Data Lake Store and Analytics accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                System.out.println(String.format("All ADL Analytics accounts that this app can access in subscription %s:", _subId));
                List<DataLakeAnalyticsAccount> adlaListResult = _adlaClient.getAccountOperations().list().getBody();
                for (DataLakeAnalyticsAccount acct : adlaListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Accounts displayed.", "Creating files.");
        
                // Create a file in Data Lake Store: input1.csv
                // TODO: these change order in the next patch
                byte[] bytesContents = "123,abc".getBytes();
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);
        
                WaitForNewline("File created.", "Submitting a job.");
        
                // Submit a job to Data Lake Analytics
                UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();", "testJob");
                WaitForNewline("Job submitted.", "Getting job status.");
        
                // Wait for job completion and output job status
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                System.out.println("Waiting for job completion.");
                WaitForJob(jobId);
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                WaitForNewline("Job completed.", "Downloading job output.");
        
                // Download job output from Data Lake Store
                DownloadFile("/output1.csv", localFolderPath + "output1.csv");
                WaitForNewline("Job output downloaded.", "Deleting file.");
        
                // Delete file from Data Lake Store
                DeleteFile("/output1.csv");
                WaitForNewline("File deleted.", "Deleting account.");
        
                // Delete account
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
                _adlaClient.getAccountOperations().delete(_resourceGroupName, _adlaAccountName);
                WaitForNewline("Account deleted.", "DONE.");
            }
        
            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlaClient = new DataLakeAnalyticsAccountManagementClientImpl(creds);
                _adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);
                _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClientImpl(creds);
                _adlsClient.setSubscriptionId(_subId);
                _adlaClient.setSubscriptionId(_subId);
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
        
                System.out.println(reason + "\r\nPress ENTER to continue...");
                try{System.in.read();}
                catch(Exception e){}
        
                if (!nextAction.isEmpty())
                {
                    System.out.println(nextAction);
                }
            }
        
            // Create Data Lake Store and Analytics accounts
            public static void CreateAccounts() throws InterruptedException, CloudException, IOException {
                // Create ADLS account
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);
        
                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
        
                // Create ADLA account
                DataLakeStoreAccountInfo adlsInfo = new DataLakeStoreAccountInfo();
                adlsInfo.setName(_adlsAccountName);
        
                DataLakeStoreAccountInfoProperties adlsInfoProperties = new DataLakeStoreAccountInfoProperties();
                adlsInfo.setProperties(adlsInfoProperties);
        
                List<DataLakeStoreAccountInfo> adlsInfoList = new ArrayList<DataLakeStoreAccountInfo>();
                adlsInfoList.add(adlsInfo);
        
                DataLakeAnalyticsAccountProperties adlaProperties = new DataLakeAnalyticsAccountProperties();
                adlaProperties.setDataLakeStoreAccounts(adlsInfoList);
                adlaProperties.setDefaultDataLakeStoreAccount(_adlsAccountName);
        
                DataLakeAnalyticsAccount adlaParameters = new DataLakeAnalyticsAccount();
                adlaParameters.setLocation(_location);
                adlaParameters.setName(_adlaAccountName);
                adlaParameters.setProperties(adlaProperties);
        
                    /* If this line generates an error message like "The deep update for property 'DataLakeStoreAccounts' is not supported", please delete the ADLS and ADLA accounts via the portal and re-run your script. */
        
                _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }
        
            //todo: this changes in the next version of the API
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }
        
            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
            }
        
            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(srcPath, _adlsAccountName).getBody();
        
                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());
        
                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();
        
                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }
        
                pWriter.println(fileContents);
                pWriter.close();
            }
        
            // Submit a U-SQL job by providing script contents.
            // Returns the job ID
            public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException {
                UUID jobId = java.util.UUID.randomUUID();
                USqlJobProperties properties = new USqlJobProperties();
                properties.setScript(script);
                JobInformation parameters = new JobInformation();
                parameters.setName(jobName);
                parameters.setJobId(jobId);
                parameters.setType(JobType.USQL);
                parameters.setProperties(properties);
        
                JobInformation jobInfo = _adlaJobClient.getJobOperations().create(_adlaAccountName, jobId, parameters).getBody();
        
                return jobId;
            }
        
            // Wait for job completion
            public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                while (jobInfo.getState() != JobState.ENDED)
                {
                    jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
                }
                return jobInfo.getResult();
            }
        
            // Get job status
            public static String GetJobStatus(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                return jobInfo.getState().toValue();
            }
        }

6. Folgen Sie den Anweisungen zum Ausführen, und führen Sie die Anwendung.


## <a name="see-also"></a>Siehe auch

- Um die gleichen Lernprogramm mit anderen Tools angezeigt wird, klicken Sie auf die Registerkarte Selektoren am oberen Rand der Seite.
- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle Azure Daten dem Analytics verwenden](data-lake-analytics-analyze-weblogs.md).
- Um anzufangen U SQL Anwendungen entwickeln, finden Sie unter [entwickeln U-SQL-Skripts, die mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md), und [U-SQL-Sprache verweisen](http://go.microsoft.com/fwlink/?LinkId=691348).
- Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Daten dem Analytics Azure-Portal verwenden](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick über die Daten dem Analytics zu gelangen, finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).
