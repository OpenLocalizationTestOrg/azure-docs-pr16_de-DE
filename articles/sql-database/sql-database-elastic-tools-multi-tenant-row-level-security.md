<properties 
    pageTitle="Mehrere Mandanten Applikationen mit flexible Datenbanktools und Sicherheit auf Benutzerebene Zeile" 
    description="Informationen Sie zum flexible Datenbanktools in Verbindung mit Sicherheit auf Benutzerebene Zeile verwenden, um eine Anwendung mit einem hochgradig skalierbare Datenebene Azure SQL-Datenbank zu erstellen, die mehrere Mandanten mehrere Shards hinweg unterstützt." 
    metaKeywords="azure sql database elastic tools multi tenant row level security rls" 
    services="sql-database" 
    documentationCenter=""  
    manager="jhubbard" 
    authors="tmullaney"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="thmullan;torsteng" />

# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Mehrere Mandanten Applikationen mit flexible Datenbanktools und Sicherheit auf Benutzerebene Zeile 

[Flexible Datenbanktools](sql-database-elastic-scale-get-started.md) und [Zeile Ebene Sicherheit (RLS)](https://msdn.microsoft.com/library/dn765131) bieten eine Reihe von Funktionen für die Skalierung der Datenebene einer Anwendung mit mehreren Mandanten mit Azure SQL-Datenbank von flexibel und effizient leistungsfähige. Weitere Informationen finden Sie unter [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md) . 

In diesem Artikel wird veranschaulicht, wie diese Technologien gemeinsam verwendet, um eine Anwendung mit einer hochgradig skalierbare Datenebene zu erstellen, die mehrere Mandanten mehrere Shards hinweg, mit **ADO.NET SqlClient** und/oder **Entität Framework**unterstützt.  

* **Flexible Datenbanktools** ermöglicht Entwicklern das Skalieren der Datenebene einer Anwendung über Industriestandard Sharding Methoden mithilfe eines Satzes von .NET Bibliotheken und Azure Service-Vorlagen. Verwalten mehrere Shards hinweg mit mithilfe der flexible Datenbank-Client-Bibliothek können Sie automatisieren und optimieren vieler infrastrukturelle Aufgaben in der Regel Sharding zugeordnet. 

* **Sicherheit auf Benutzerebene Zeile** ermöglicht Entwicklern das Speichern von Daten für mehrere Mandanten in derselben Datenbank mithilfe von Sicherheitsrichtlinien Zeilen herausfiltern, die nicht zu den Mandanten Ausführen einer Abfrage gehören. Zentrale Access Logik mit RLS innerhalb der Datenbank, statt in der Anwendung vereinfacht die Wartung und reduziert sich das Risiko des Fehlers der Anwendung Codebasis vergrößert wird. RLS erfordert die neuesten [Azure SQL-Datenbank aktualisieren (V12)](sql-database-v12-whats-new.md). 

Verwenden diese Features zusammen, kann eine Anwendung von Kosten Spareinlagen und Effizienz Gewinne profitieren durch das Speichern von Daten für mehrere Mandanten in derselben Datenbank Shard. Zur gleichen Zeit enthält die Anwendung immer noch die Flexibilität, isoliert, Single-Mandanten mehrere Shards hinweg für "Premium" Mandanten anzubieten, die strengere Leistung Garantien benötigen, da mehrere Shards mit mehreren Mandanten hinweg nicht gleich Ressource Verteilung zwischen Mandanten gewährleistet.  

Kurz gesagt, flexible Datenbank Clientbibliothek APIs [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md) automatisch Mandanten herstellen einer Verbindung mit der richtigen Shard Datenbank, deren Schlüssel Sharding (in der Regel ein "TenantId") enthält. Nachdem die Verbindung hergestellt wurde, ist eine Sicherheitsrichtlinie RLS innerhalb der Datenbank sichergestellt, dass Mandanten nur Zeilen zugreifen können, die ihre TenantId enthalten. Es wird vorausgesetzt, dass alle Tabellen eine Spalte TenantId enthält, um anzugeben, welche Zeilen auf jeder Mandanten gehören. 

![Blogging-app-Architektur][1]

## <a name="download-the-sample-project"></a>Laden Sie das Beispielprojekt

### <a name="prerequisites"></a>Erforderliche Komponenten
* Verwenden Sie Visual Studio (2012 oder höher) 
* Erstellen Sie drei Azure SQL-Datenbanken 
* Herunterladen der Stichprobe Projekt: [Flexible DB Tools für Azure SQL - Mandanten mit mehreren mehrere Shards hinweg](http://go.microsoft.com/?linkid=9888163)
  * Füllen Sie die Informationen für Ihre Datenbanken am Anfang der **Program.cs** 

Dieses Projekt erweitert, die durch Hinzufügen von Unterstützung für mehrere Mandanten Shard Datenbanken in [Flexible DB Tools für Azure SQL - Entität Framework Integration](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) beschrieben. Erstellt eine einfache Console-Anwendung für das Erstellen von Blogs und Beiträge, mit vier Mandanten und zwei mit mehreren Mandanten Shard-Datenbanken wie in der obigen Abbildung gezeigt. 

Erstellen Sie, und führen Sie die Anwendung. Dieses flexible Datenbanktools Shard Karte-Manager starten, und führen Sie die folgenden Tests durch: 

1. Verwenden von Entität Framework und LINQ, erstellen Sie einen neuen Blog, und zeigen Sie dann alle Blogs für jeden Mandanten
2. Verwenden ADO.NET SqlClient, alle Blogs für einen Mandanten anzeigen
3. Versuchen Sie, einen Blog für den falschen Mandanten zu überprüfen, ob ein Fehler ausgelöst wird, einfügen  

Beachten Sie, dass da RLS noch nicht in den Shard Datenbanken aktiviert wurde, der Tests ein Problem werden: Mandanten sind Blogs sehen, die Ihnen nicht gehören, und die Anwendung wird nicht verhindert, einen Blog für den Mandanten falschen einfügen. Der Rest der in diesem Artikel wird beschrieben, wie diese Probleme zu lösen, indem Sie Mandanten Isolation mit RLS erzwingen. Es gibt zwei Schritte aus: 

1. **Schicht Anwendung**: Ändern Sie den Anwendungscode, um die aktuelle TenantId in die SESSION_CONTEXT immer festzulegen, nach dem Öffnen einer Verbindungs. Das Beispielprojekt hat dies bereits erledigt. 
2. **Datenebene**: erstellen eine Sicherheitsrichtlinie RLS in jeder Shard Datenbank zum Filtern von Zeilen basierend auf der TenantId in SESSION_CONTEXT gespeichert. Sie müssen für jede Ihrer Datenbanken Shard Aktion, andernfalls Zeilen in mehrere Mandanten mehrere Shards hinweg nicht gefiltert werden. 


## <a name="step-1-application-tier-set-tenantid-in-the-sessioncontext"></a>Schritt 1) Anwendungsebene: Festlegen von TenantId in der SESSION_CONTEXT

Welche TenantId ist nach dem Herstellen einer Verbindung mit einer Shard-Datenbank mithilfe der flexible Datenbank-Client-Bibliothek-abhängige Weiterleitung APIs, die Anwendung noch die Datenbank feststellen, muss diese Verbindung mithilfe von, sodass die Zeilen, die zu anderen Mandanten gehören eine Sicherheitsrichtlinie RLS herausfiltern kann. Zum Speichern der aktuellen TenantId für diese Verbindung in der [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx)wird empfohlen, diese Informationen zu übergeben. (Notiz: Alternativ können Sie [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), aber SESSION_CONTEXT ist eine bessere Option weil es einfacher zu verwenden, ist NULL Standardmäßig gibt und unterstützt Schlüssel-Wert-Paare.)

### <a name="entity-framework"></a>Entität Framework

Ist am einfachsten für Applikationen Entität Framework verwenden die SESSION_CONTEXT in die ElasticScaleContext Außerkraftsetzung [Daten abhängige Routing mithilfe von EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext)beschrieben festlegen. Vor dem Beenden der Verbindungs über Daten abhängige Weiterleitung vermittelte erstellen Sie und führen Sie SqlCommand, der festlegt 'TenantId aus' in der SESSION_CONTEXT auf die ShardingKey für diese Verbindung angegeben. Auf diese Weise müssen Sie nur einmal Schreiben von Code der SESSION_CONTEXT festlegen. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call will fail for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

Jetzt wird der SESSION_CONTEXT automatisch mit der angegebenen TenantId festgelegt, wenn ElasticScaleContext aufgerufen wird: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;
        
        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient 

Für Applikationen ADO.NET SqlClient verwenden wird empfohlen, eine Wrapperfunktion um ShardMap.OpenConnectionForKey() zu erstellen, die automatisch 'TenantId' in der SESSION_CONTEXT, um die richtige TenantId setzt, vor der Rückgabe einer Verbindungs. Um sicherzustellen, dass SESSION_CONTEXT immer festgelegt ist, sollten Sie mithilfe dieser Wrapperfunktion Verbindungen nur öffnen.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs will be listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>Schritt 2) Datenebene: Zeile Ebene Sicherheitsrichtlinie erstellen

### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Erstellen Sie eine Sicherheitsrichtlinie aus, um die Zeilen zu filtern, die jeder Mandanten zugreifen können

Jetzt, da die Anwendung SESSION_CONTEXT mit den aktuellen TenantId festlegen ist, bevor Sie Abfragen, können Sie eine Sicherheitsrichtlinie RLS Abfragen filtern und Ausschließen von Zeilen, die eine andere TenantId aufweisen.  

RLS wird in T-SQL implementiert: eine benutzerdefinierte Funktion definiert die Access-Logik und eine Sicherheitsrichtlinie bindet diese Funktion an eine beliebige Anzahl von Tabellen. Für dieses Projekt wird die Funktion einfach vergewissern, dass die Anwendung (statt einen anderen SQL-Benutzer) mit der Datenbank verbunden ist, und, dass die 'TenantId' in der SESSION_CONTEXT gespeichert der TenantId einer Zeile entspricht. Eine Filterprädikat bestimmter Zeilen ermöglichen, die diese Bedingung passieren des benutzerdefinierten Filters für auswählen, aktualisieren und Löschen von Abfragen genügen; und ein blockieren Prädikat wird verhindert, dass Zeilen, die diese Qualifikation, nicht mehr eingefügte oder aktualisierten verletzt. Wenn SESSION_CONTEXT nicht festgelegt wurde, gibt es NULL und keine Zeilen sichtbar oder mehr eingefügt werden, werden zurück. 

Klicken Sie zum Aktivieren RLS folgende T-SQL-Anweisung ausführen, klicken Sie auf alle mehrere Shards hinweg mit Visual Studio (SSDT), SSMS oder der PowerShell-Skript im Projekt enthalten (oder wenn Sie [Flexible Datenbank Einzelvorgänge](sql-database-elastic-jobs-overview.md)verwenden, können Sie sie zur Ausführung von diesem T-SQL auf alle mehrere Shards hinweg Automatisierung): 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [AZURE.TIP] Für komplexere Projekte, die das Prädikat auf mehreren hundert Tabellen hinzufügen müssen, können Sie eine Helper gespeicherte Prozedur aus, die automatisch eine Sicherheitsrichtlinie Hinzufügen eines Prädikats auf alle Tabellen in einem Schema generiert. Finden Sie unter [Anwenden von Zeile Sicherheit auf Benutzerebene für alle Tabellen – Helper Skript (Blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  

Wenn Sie die Stichprobe Anwendung erneut ausführen, werden jetzt Mandanten kann nur Zeilen sehen, die sie angehören. Darüber hinaus kann die Anwendung keine Zeilen einfügen, die anderen als den aktuell eine mit der Datenbank Shard Verbindung und nicht sichtbare Zeilen, damit eine andere TenantId aktualisieren Mandanten angehören. Wenn die Anwendung versucht, führen Sie entweder, wird eine DbUpdateException ausgelöst.

Wenn Sie eine neue Tabelle höher hinzufügen, einfach die Sicherheitsrichtlinie ALTER Filter hinzufügen und blockieren Prädikate für die neue Tabelle: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Fügen Sie Standard-Einschränkungen zum Auffüllen von TenantId für fügt automatisch hinzu 

Setzen Sie eine Default-Einschränkung für jede Tabelle, um die TenantId automatisch mit dem Wert derzeit im SESSION_CONTEXT gespeichert, wenn Sie beim Einfügen von Zeilen zu füllen. Beispiel: 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Jetzt muss die Anwendung keine TenantId angeben, wenn Zeilen einfügen: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [AZURE.NOTE] Wenn Sie die standardmäßigen Einschränkungen für ein Projekt Entität Framework verwenden, empfiehlt es sich, dass Sie nicht die Spalte TenantId in Ihr Datenmodell EF einschließen. Dies liegt daran Entität Framework Abfragen automatisch Standardwerte bereit, die die standardmäßige Einschränkungen erstellt in T-SQL überschrieben werden, die SESSION_CONTEXT verwenden. Um Standard Einschränkungen im Projekt Stichprobe zu verwenden, sollten Sie beispielsweise daraus TenantId DataClasses.cs (und Ausführen Hinzufügen-Migration in der Paket-Manager-Konsole) und T-SQL verwenden, um sicherzustellen, dass das Feld nur in der Datenbank vorhanden ist. Auf diese Weise wird EF nicht automatisch falsche Standardwerte angeben beim Einfügen von Daten. 

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(Optional) Aktivieren Sie ein "Hauptbenutzer", um den Zugriff auf alle Zeilen
Einige Applikationen möchten möglicherweise erstellen ein "Hauptbenutzer", die alle Zeilen, z. B. zugreifen können, um Berichte über alle Mandanten auf alle mehrere Shards hinweg aktivieren, oder Teilen/zusammenführen Vorgänge für mehrere Shards hinweg auszuführen, die Verschieben von Mandanten Zeilen zwischen Datenbanken betreffen. Um dies zu aktivieren, sollten Sie einen neuen SQL-Benutzer (in diesem Beispiel "Hauptbenutzer") in jeder Shard Datenbank erstellen. Ändern Sie dann die Sicherheitsrichtlinie mit einer neuen Prädikatfunktion, die diese Benutzer alle Zeilen zugreifen kann:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Wartung 

* **Hinzufügen von neuen mehrere Shards hinweg**: Sie müssen das T-SQL-Skript zum Aktivieren der RLS auf alle neuen mehrere Shards hinweg ausführen, andernfalls Abfragen auf diese mehrere Shards hinweg nicht gefiltert werden.

* **Hinzufügen von neuen Tabellen**: Sie müssen die Sicherheitsrichtlinie auf alle mehrere Shards hinweg ein Prädikats filtern und blockieren hinzufügen, wenn eine neue Tabelle erstellt wird, andernfalls Abfragen auf die neue Tabelle nicht gefiltert werden. Dies kann dem automatischen werden mithilfe eines Triggers DDL in [Anwenden Zeile Sicherheit auf Benutzerebene automatisch in neu erstellten Tabellen (Blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx)beschriebenen.


## <a name="summary"></a>Zusammenfassung 

Flexible Datenbanktools und Sicherheit auf Benutzerebene Zeile können mehrere Shards verwendeten zusammen eine Skalierung der Anwendung Datenebene mit Unterstützung für beide mit mehreren Mandanten und Single-Mandanten hinweg sein. Mehrere Mandanten mehrere Shards hinweg zum Daten effizienter zu speichern (besonders in Fällen, wo eine große Anzahl von Mandanten nur ein paar Zeilen mit Daten haben) verwendet werden können, während der Single-Mandanten mehrere Shards hinweg zur Unterstützung von Premium-Mandanten mit strengere Leistung und Isolation Anforderungen verwendet werden können.  Weitere Informationen finden Sie unter [Sicherheit auf Benutzerebene Zeile verweisen](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Was ist ein Ressourcenpool flexible Azure-Datenbank?](sql-database-elastic-pool.md)
- [Skalierung mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md)
- [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Authentifizierung in mandantenfähigen-apps, Azure AD- und OpenID verbinden](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin Umfragen Anwendung](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Fragen und Features Besprechungsanfragen

Fragen bitte erreichen Sie uns im [Forum SQL-Datenbank](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) und für Features Besprechungsanfragen, wenden Sie sich bitte fügen Sie die [SQL-Datenbank Feedback Forum hinzu](https://feedback.azure.com/forums/217321-sql-database/).


<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->

 
