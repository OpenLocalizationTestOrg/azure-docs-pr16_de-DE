<properties
   pageTitle="Verwenden Sie die Daten dem Store .NET SDK zum Entwickeln von Applications | Microsoft Azure"
   description="Verwenden Sie zum Entwickeln von Applications Azure Daten dem Store .NET SDK"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Erste Schritte mit Azure Lake Datenspeicher mit .NET SDK

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informationen Sie zum Verwenden von [Azure Daten dem Store .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) Standardvorgänge ausführen, die beispielsweise als Erstellen von Ordnern, hoch- und Herunterladen von Datendateien usw.. Weitere Informationen zu den Daten Lake finden Sie unter [Azure dem Datenspeicher](data-lake-store-overview.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* **Visual Studio-2013 oder 2015**. Verwenden Sie Visual Studio 2015 aufgeführten Schritte ausführen.

* **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure dem Datenspeicher-Konto**. Anweisungen zum Erstellen eines Kontos finden Sie unter [Erste Schritte mit Azure dem Datenspeicher](data-lake-store-get-started-portal.md)

* **Erstellen einer Azure-Active Directory-Anwendung**. Die Anwendung Lake Datenspeicher mit Azure AD-authentifizieren mithilfe Sie die Azure AD-Anwendung. Es gibt verschiedene Ansätze mit Azure AD Authentifizierung die **Endbenutzer** oder **Dienst - Authentifizierung**sind. Anleitungen und Weitere Informationen zum Authentifizieren finden Sie unter [Authentifizieren mit dem Datenspeicher verwenden Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Erstellen Sie eine Anwendung .NET

1. Öffnen Sie Visual Studio und erstellen Sie eine.

2. Im Menü **Datei** klicken Sie auf **neu**, und klicken Sie dann auf **Projekt**.

3. Geben Sie ein **Neues Projekt**oder wählen Sie die folgenden Werte aus:

  	| Eigenschaft | Wert                       |
  	|----------|-----------------------------|
  	| Kategorie | Vorlagen/Visual c# / Windows |
  	| Vorlage | Console-Anwendung         |
  	| Namen     | CreateADLApplication        |

4. Klicken Sie auf **OK** , um das Projekt zu erstellen.

5. Hinzufügen von Nuget-Paketen zu einem Projekt.

    1. Mit der rechten Maustaste in des Namens des Projekts in der Lösung-Explorer, und klicken Sie auf **NuGet-Pakete verwalten**.
    2. Die Registerkarte **Nuget Package Manager** sicherstellen Sie, dass **Paketquelle** auf **"NuGet.org"** festgelegt ist und das **Vorabversion gehören** Kontrollkästchen aktiviert ist.
    3. Suchen Sie und installieren Sie die folgenden NuGet-Paketen:

        * `Microsoft.Azure.Management.DataLake.Store`– In diesem Lernprogramm verwendet v0.12.5-Vorschau.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`– In diesem Lernprogramm verwendet v0.10.6-Vorschau.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`– In diesem Lernprogramm verwendet v2.2.8-Vorschau.

        ![Hinzufügen einer Quelle Nuget] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Erstellen eines neuen Kontos mit dem Azure-Daten")

    4. Schließen Sie den **Nuget-Paket-Manager**.

6. Öffnen Sie **Program.cs**, löschen Sie den vorhandenen Code, und fügen Sie dann die folgenden Aussagen Verweise auf Namespaces hinzuzufügen.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Deklarieren Sie die Variablen, wie unten dargestellt, und geben Sie die Werte für Lake Datenspeicher Namen und den Namen der Ressource-Gruppe, die bereits vorhanden sind. Vergewissern Sie sich außerdem den lokalen Pfad und Dateinamen ein, die hier eingegebenen muss auf dem Computer vorhanden sein. Fügen Sie den folgenden Codeausschnitt nach Namespace-Deklarationen hinzu.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

In den restlichen Abschnitten dieses Artikels sehen Sie, wie die verfügbaren .NET Methoden zum Ausführen von Vorgängen, wie z. B. Authentifizierung, Datei hochladen, usw. verwendet werden.

## <a name="authentication"></a>Authentifizierung

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Wenn Sie die Endbenutzer-Authentifizierung (in diesem Lernprogramm empfohlen) verwenden

Verwenden Sie diese mit einer vorhandenen Azure AD "Native Client" Anwendung; eine wird unten bereitgestellt. Damit Sie dieses Lernprogramm schneller abgeschlossen werden können, empfehlen wir, dass Sie dieses Verfahren verwenden.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Paar Dinge zu diesem Codeausschnitt oben kennen.

* Damit Sie das Lernprogramm schneller abschließen können, diese Codeausschnitt verwendet einen einer Azure AD Domäne und Client-ID, die standardmäßig für alle Azure-Abonnements zur Verfügung. Ja, können Sie **Verwenden dieser Ausschnitt als-befindet sich in Ihrer Anwendung**.
* Hingegen, wenn Sie Ihre eigenen Azure AD-Domäne und Anwendung Client-ID verwenden möchten, müssen Erstellen einer systemeigene Azure AD-Anwendung und dann verwenden Sie die Domäne Azure AD-, Client-ID, und umleiten URI für die Anwendung, die Sie erstellt haben. Anweisungen finden Sie unter [Erstellen einer Active Directory-Anwendung](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Die Anweisungen in den oben Links sind für eine Azure AD-Web-Anwendung. Jedoch sind die Schritte genau gleich, auch wenn Sie stattdessen Erstellen einer systemeigenen Clientanwendung ignoriert. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Wenn Sie mit Client geheim Dienst-Authentifizierung verwenden 

Im folgende Codeausschnitt kann verwendet werden, um Ihrer Anwendung nicht interaktiv authentifizieren über den Client geheimen / Schlüssel für eine Anwendung / Service Hauptbenutzer. Verwenden Sie diese mit einer vorhandenen [Azure AD "Web App"-Anwendung](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Wenn Sie mit dem Zertifikat-Dienst Authentifizierung verwenden

Als dritte option, kann der folgende Codeausschnitt verwendet werden mit Ihrer Anwendung nicht interaktiv authentifiziert mithilfe des Zertifikats für eine Anwendung / Service Tilgungsanteile. Verwenden Sie diese mit einer vorhandenen [Azure AD "Web App"-Anwendung](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Erstellen Sie Objekte

Der folgende Codeausschnitt erstellt den Lake Datenspeicher-Konto und Dateisystem-Client-Objekte, die verwendet werden, um Anfragen an den Dienst ausgeben.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Liste aller Lake Datenspeicher Konten innerhalb eines Abonnements

Im folgende Codeausschnitt Listet alle Lake Datenspeicher Konten innerhalb eines bestimmten Azure-Abonnements.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Erstellen Sie ein Verzeichnis

Der folgende Ausschnitt zeigt eine `CreateDirectory` Methode, die Sie zum Erstellen eines Verzeichnisses innerhalb eines Kontos Lake Datenspeicher verwenden können.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Hochladen einer Datei

Der folgende Ausschnitt zeigt eine `UploadFile` Methode, die Sie zum Hochladen von Dateien mit einem Konto Lake Datenspeicher verwenden können.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`Rekursive Upload und Download zwischen einer lokalen Dateipfad und einen Dateipfad Lake Datenspeicher unterstützt.    

## <a name="get-file-or-directory-info"></a>Datei oder ein Verzeichnis Informationen

Der folgende Ausschnitt zeigt eine `GetItemInfo` Methode, die Sie zum Abrufen von Informationen zu einer Datei oder ein Verzeichnis verfügbar Lake Datenspeicher verwenden können. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Listendatei oder Verzeichnisse durchsuchen

Der folgende Ausschnitt zeigt eine `ListItem` Methode, um die Dateien und Verzeichnisse in einem Konto Lake Datenspeicher Liste verwenden können.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Concatenate-Dateien

Der folgende Ausschnitt zeigt eine `ConcatenateFiles` Methode, die Sie verwenden, um Dateien zu verketten. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Anfügen einer Datei

Der folgende Ausschnitt zeigt eine `AppendToFile` Methode, mit denen Sie Daten in eine Datei bereits in einem Konto Lake Datenspeicher gespeicherten anfügen.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Herunterladen einer Datei

Der folgende Ausschnitt zeigt eine `DownloadFile` Methode, die Sie verwenden, um eine Datei nicht von einem Konto Lake Datenspeicher herunterladen.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Nächste Schritte

- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Lake Datenspeicher .NET SDK-Referenz](https://msdn.microsoft.com/library/mt581387.aspx)
- [Lake Datenspeicher REST Bezug](https://msdn.microsoft.com/library/mt693424.aspx)
