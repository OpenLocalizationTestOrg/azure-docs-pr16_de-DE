<properties 
    pageTitle="Beispiele für die MongoDB DocumentDB | Microsoft Azure" 
    description="Finden Sie Beispiele für DocumentDBs Protokoll Unterstützung für MongoDB aus." 
    keywords="MongoDB Beispiele"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-protocol-support-for-mongodb-examples"></a>DocumentDB Protokoll Unterstützung für MongoDB Beispiele
Wenn Sie in diesen Beispielen verwenden möchten, müssen Sie folgende Aktionen ausführen:

- [Erstellen](documentdb-create-mongodb-account.md) eines Kontos Azure DocumentDB mit Protokoll Unterstützung für MongoDB.
- Abrufen von Ihrem Konto DocumentDB Protokoll Support MongoDB [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) Informationen an.

## <a name="get-started-with-a-sample-aspnet-mvc-task-list-application"></a>Erste Schritte mit einer Stichprobe Vorgang Liste ASP.NET-MVC-Anwendung

Können Sie das Lernprogramm [Erstellen einer Web-app in Azure, die mit MongoDB auf einem virtuellen Computer verbindet,](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) mit minimalen ändern, schnell eine Anwendung MongoDB setup (entweder lokal oder auf einer Azure Web app veröffentlicht), die mit einem Konto DocumentDB mit Protokoll Unterstützung für MongoDB eine Verbindung herstellt.  

1. Führen Sie das Lernprogramm, mit der eine Änderung vorgenommen.  Ersetzen Sie den Code Dal.cs mit:
    
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;
        using MyTaskListApp.Models;
        using MongoDB.Driver;
        using MongoDB.Bson;
        using System.Configuration;
        using System.Security.Authentication;

        namespace MyTaskListApp
        {
            public class Dal : IDisposable
            {
                //private MongoServer mongoServer = null;
                private bool disposed = false;

                // To do: update the connection string with the DNS name
                // or IP address of your server. 
                //For example, "mongodb://testlinux.cloudapp.net
                private string connectionString = "mongodb://localhost:27017";
                private string userName = "<your user name>";
                private string host = "<your host>";
                private string password = "<your password>";

                // This sample uses a database named "Tasks" and a 
                //collection named "TasksList".  The database and collection 
                //will be automatically created if they don't already exist.
                private string dbName = "Tasks";
                private string collectionName = "TasksList";

                // Default constructor.        
                public Dal()
                {
                }

                // Gets all Task items from the MongoDB server.        
                public List<MyTask> GetAllTasks()
                {
                    try
                    {
                        var collection = GetTasksCollection();
                        return collection.Find(new BsonDocument()).ToList();
                    }
                    catch (MongoConnectionException)
                    {
                        return new List<MyTask>();
                    }
                }

                // Creates a Task and inserts it into the collection in MongoDB.
                public void CreateTask(MyTask task)
                {
                    var collection = GetTasksCollectionForEdit();
                    try
                    {
                        collection.InsertOne(task);
                    }
                    catch (MongoCommandException ex)
                    {
                        string msg = ex.Message;
                    }
                }
        
                private IMongoCollection<MyTask> GetTasksCollection()
                {
                    MongoClientSettings settings = new MongoClientSettings();
                    settings.Server = new MongoServerAddress(host, 10250);
                    settings.UseSsl = true;
                    settings.SslSettings = new SslSettings();
                    settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
        
                    MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                    MongoIdentityEvidence evidence = new PasswordEvidence(password);
        
                    settings.Credentials = new List<MongoCredential>()
                    {
                        new MongoCredential("SCRAM-SHA-1", identity, evidence)
                    };

                    MongoClient client = new MongoClient(settings);
                    var database = client.GetDatabase(dbName);
                    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                    return todoTaskCollection;
                }
        
                private IMongoCollection<MyTask> GetTasksCollectionForEdit()
                {
                    MongoClientSettings settings = new MongoClientSettings();
                    settings.Server = new MongoServerAddress(host, 10250);
                    settings.UseSsl = true;
                    settings.SslSettings = new SslSettings();
                    settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
        
                    MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                    MongoIdentityEvidence evidence = new PasswordEvidence(password);
        
                    settings.Credentials = new List<MongoCredential>()
                    {
                        new MongoCredential("SCRAM-SHA-1", identity, evidence)
                    };
                    MongoClient client = new MongoClient(settings);
                    var database = client.GetDatabase(dbName);
                    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                    return todoTaskCollection;
                }

                # region IDisposable
        
                public void Dispose()
                {
                    this.Dispose(true);
                    GC.SuppressFinalize(this);
                }

                protected virtual void Dispose(bool disposing)
                {
                    if (!this.disposed)
                    {
                        if (disposing)
                        {
                        }
                    }

                    this.disposed = true;
                }

                # endregion
            }
        }

2.  Ändern Sie die folgenden Variablen in der Datei Dal.cs pro Einstellungen für Ihr Konto ein:

        private string userName = "<your user name>";
        private string host = "<your host>";
        private string password = "<your password>";

3. Verwenden Sie die app!

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie mit einem Konto DocumentDB mit Protokoll [verwenden MongoChef](documentdb-mongodb-mongochef.md) Unterstützung für MongoDB.

 
