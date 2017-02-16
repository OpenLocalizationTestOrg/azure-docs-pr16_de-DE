<properties
   pageTitle="API Implementierung Anleitungen | Microsoft Azure"
   description="Leitfäden nach wie eine API implementiert wird."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="api-implementation-guidance"></a>API Implementierung Anleitungen

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Einige Themen in diesem Handbuch unter Diskussion sind und in der Zukunft ändern können. Wir Willkommen Ihr Feedback!

## <a name="overview"></a>(Übersicht)
Eine sorgfältig entwickelt Rest Web API definiert die Ressourcen, Beziehungen und Navigation bieten, die in Clientanwendungen zugegriffen werden kann. Wenn Sie implementieren und eine Web-API bereitstellen, sollten Sie erwägen, dass die physischen Anforderungen der Umgebung Hostinganbieter im Web API und wie in der Web-API statt der logischen Struktur der Daten erstellt wird. Dieser Leitfaden focusses auf bewährte Methoden zum Implementieren eines-API und veröffentlichen, um es Clientanwendungen zur Verfügung stellen. Sicherheitsgründen werden im Dokument API Sicherheit Anleitungen separat beschrieben. Ausführliche Informationen zu Web API Entwurf finden Sie im Dokument-API Entwurf Anleitungen.

## <a name="considerations-for-implementing-a-restful-web-api"></a>Aspekte für Implementieren eines REST-API
In den folgenden Abschnitten erläutern Sie bewährte Methode für die ASP.NET Web API Vorlage verwenden, um ein Rest Web API zu erstellen. Ausführliche Informationen zur Verwendung der Web-API-Vorlage finden Sie auf der Seite [ASP.NET Web API](http://www.asp.net/web-api) auf der Microsoft-Website.

## <a name="considerations-for-implementing-request-routing"></a>Aspekte bei der Implementierung Anforderung routing

In einem Dienst mithilfe der ASP.NET Web API implementiert wird jede Anforderung an eine Methode in einer Klasse _Controller_ weitergeleitet. Im Rahmen der Web-API bietet zwei primäre Optionen für das routing implementieren. _Messe-basierten_ routing und _Attribut-basierte_ routing. Beachten Sie die folgenden Punkte, wenn Sie die beste Methode zum Weiterleiten von Besprechungsanfragen in Ihrem Web API ermitteln:

- **Verstehen die Einschränkungen und den Anforderungen der Messe-basierten routing**.

    Standardmäßig verwendet das Web-API Framework Messe-basierten routing. Im Rahmen der Web-API erstellt eine anfängliche routing-Tabelle mit dem folgenden Eintrag:

    ```C#
    config.Routes.MapHttpRoute(
        name: "DefaultApi",
        routeTemplate: "api/{controller}/{id}",
        defaults: new { id = RouteParameter.Optional }
    );
    ```

    Leitet können generisch, umfasst literalen z. B. _-api_ und Variablen wie _{Controller}_ und _{Id}_sein. Messe-basierten routing kann einige Elemente der Routing optional sein. Das Web-API Framework bestimmt festzulegen, welche Methode im Controller aufzurufen, indem Sie die HTTP-Methode in der Anforderung an den im ersten Teil den Namen der Methode in der-API und dann nach übereinstimmenden optionale Parameter. Beispielsweise ein Controller mit dem Namen _Orders_ Methoden _GetAllOrders()_ oder _GetOrderByInt(int id)_ enthält dann an die _GetAlllOrders()_ -Methode der GET-Anforderung _http://www.adventure-works.com/api/orders/_ weitergeleitet und an die _GetOrderByInt(int id)_-Methode der GET-Anforderung _http://www.adventure-works.com/api/orders/99_ weitergeleitet werden. Wenn es keine übereinstimmende Methode zur Verfügung, die mit dem Präfix Get im Controller beginnt ist, Antworten das Web-API Framework mit einer Nachricht HTTP 405 (Methode nicht zulässig). Darüber hinaus wird Namen des Parameters (Id) in der Tabelle routing angegebenen den Namen des Parameters für die Methode _GetOrderById_ identisch sein muss, andernfalls das Web-API Framework mit einer Antwort HTTP 404 (nicht gefunden) Antworten.

    Die gleichen Regeln gelten für Beitrag, sich und Löschen von HTTP-Anfragen; Anforderung einer bereitstellen, die die Details der Ordnung 101 aktualisiert würde an den URI- _http://www.adventure-works.com/api/orders/101_weitergeleitet werden, der Hauptteil der Nachricht werden die neuen Details der Reihenfolge enthalten, und diese Informationen werden als Parameter an eine Methode im Bestellungen Controller mit einem Namen, der mit dem Präfix _setzen_, wie z. B. _PutOrder_beginnt übergeben.

    Die Weiterleitung Standardtabelle entspricht keine Anforderung, die untergeordneten Ressourcen in einem Rest Web-API, wie z. B. _http://www.adventure-works.com/api/customers/1/orders_ (suchen die Details aller Aufträge platziert nach Kunde 1) verweist. Diese Fälle, können Sie benutzerdefinierte leitet zur Weiterleitung Tabelle hinzufügen:

    ```C#
    config.Routes.MapHttpRoute(
        name: "CustomerOrdersRoute",
        routeTemplate: "api/customers/{custId}/orders",
        defaults: new { controller="Customers", action="GetOrdersForCustomer" })
    );
    ```

    Diese Routing weist Besprechungsanfragen, die den URI an die Methode _GetOrdersForCustomer_ im Controller _Kunden_ entsprechen. Diese Methode muss einen einzelnen Parameter, die mit dem Namen _CustI_annehmen:

    ```C#
    public class CustomersController : ApiController
    {
        ...
        public IEnumerable<Order> GetOrdersForCustomer(int custId)
        {
            // Find orders for the specified customer
            var orders = ...
            return orders;
        }
        ...
    }
    ```

    > [AZURE.TIP] Nutzen Sie die Standard-routing möglichst und zu vermeiden Sie, definieren viele komplizierte benutzerdefinierte weiterleitet, da dies unzulänglichen führen kann (es ist sehr einfach, fügen Sie zu einem herauf, die mehrdeutige leitet führen Methoden) und Leistung (die vergrößern routing-Tabelle, die mehr Arbeit, die im Rahmen der Web-API zum ausarbeiten weist, welche Routing dieses Zeichen einen angegebenen URI entspricht) verringert. Halten Sie die API und leitet einfach. Weitere Informationen finden Sie unter Abschnitt der Web-API um Ressourcen der API Entwurf Anleitungen zu organisieren. Wenn Sie benutzerdefinierte leitet definieren müssen, stellt ein vorzugsweise Attribut-basierte routing weiter unten in diesem Abschnitt beschriebenen verwenden.

    Weitere Informationen zum routing Messe-basierten finden Sie unter der Seite [Routing in ASP.NET Web-API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) auf der Microsoft-Website.

- **Mehrdeutigkeit in routing zu vermeiden**.

    Messe-basierten routing kann mehrdeutige Wege ergeben, wenn mehrere Methoden in einem Controller die gleichen Routing entsprechen. In diesen Fällen die Absichten des Web-API Rahmen mit eine Antwortnachricht zum HTTP 500 (Interner Serverfehler) mit dem Text "mehrere Aktionen wurden, die die Anforderung entsprechen gefunden".

- **Möchten Sie lieber Attribut-basierte routing**.

    Attribut-basierte routing stellt eine Alternative zum Herstellen einer leitet Methoden in einem Controller Umgebung. Anstatt Muster übereinstimmenden Features von Messe-basierten routing, können Sie explizit Methoden in einem Controller mit den Details der Routing kommentieren, die sie zugeordnet werden soll. Dieser Ansatz Hilfe mögliche Mehrdeutigkeiten zu entfernen. Darüber hinaus definierten explizite leitet zur Entwurfszeit ist diese Vorgehensweise effizienter als Messe-basierten routing zur Laufzeit. Mit dem folgende Code veranschaulicht, wie das Attribut _Routing_ auf Methoden in den Kunden Controller anzuwenden. Diese Methoden verwenden auch das Attribut HttpGet, um anzugeben, dass sie auf _HTTP-GET_ -Anfragen reagieren sollte. Dieses Attribut ermöglicht es Ihnen, benennen Ihre Methoden mit einem beliebigen geeignete naming Schema statt, die durch routing Messe-basierten erwartet. Sie können auch mit den Attributen _HttpPost_, _HttpPut_und _HttpDelete_ Methoden zu definieren, die auf anderen Typen von HTTP-Anfragen reagieren Methoden kommentieren.

    ```C#
    public class CustomersController : ApiController
    {
        ...
        [Route("api/customers/{id}")]
        [HttpGet]
        public Customer FindCustomerByID(int id)
        {
            // Find the matching customer
            var customer = ...
            return customer;
        }
        ...
        [Route("api/customers/{id}/orders")]
        [HttpGet]
        public IEnumerable<Order> FindOrdersForCustomer(int id)
        {
            // Find orders for the specified customer
            var orders = ...
            return orders;
        }
        ...
    }
    ```

    Attribut-basierte auch routing wirkt sich hilfreiche Seite-von fungiert als Dokumentation für Entwickler benötigen, den Code in der Zukunft verwalten; Es ist sofort klar, welche Routing welches Verfahren gehört, und das Attribut _HttpGet_ verdeutlicht den Typ der HTTP-Anforderung, dem die Methode reagiert.

    Attribut-basierte routing ermöglicht es Ihnen, Einschränkungen definieren, die einschränken, wie die Parameter zugeordnet sind. Einschränkungen können Geben Sie den Typ des Parameters an, und in einigen Fällen können sie auch angeben den zulässigen Wertebereich Parameter. Im folgenden Beispiel muss der ID-Parameter der Methode _FindCustomerByID_ eine positive ganze Zahl. Wenn eine Anwendung eine HTTP GET-Anforderung mit einer negative Kundennummer sendet, wird das Web-API Framework mit einer Nachricht HTTP 405 (Methode nicht zulässig) beantworten:

    ```C#
    public class CustomersController : ApiController
    {
        ...
        [Route("api/customers/{id:int:min(0)}")]
        [HttpGet]
        public Customer FindCustomerByID(int id)
        {
            // Find the matching customer
            var customer = ...
            return customer;
        }
        ...
    }
    ```

    Weitere Informationen zum routing Attribut-basierte finden Sie unter der Seite [Attribut Routing im Web API 2](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2) auf der Microsoft-Website.

- **Unterstützung von Unicode-Zeichen in weitergeleitet**.

    Die Tasten verwendet, um Ressourcen GET-Anfragen identifizieren könnte Zeichenfolgen. In einem globalen Anwendung müssen Sie daher unterstützen URIs, die nicht lateinische Zeichen enthalten.

- **Formatvorlagenquelle kennzeichnen Methoden, die nicht weitergeleitet werden sollen**.

    Wenn Sie Messe-basierten routing verwenden, zeigen Sie Methoden, die nicht mit HTTP-Aktionen übereinstimmen, indem sie mit dem Attribut _NonAction_ ergänzen an. Dies gilt in der Regel für Helper definierten Methoden für die Verwendung durch andere Methoden innerhalb einer Controller und dieses Attribut verhindert werden diese Methoden wird übereinstimmen, und durch eine fehlerhafte HTTP-Anforderung aufgerufen.

- **Erwägen Sie die vor- und Nachteile die-API in einer Unterdomäne zu platzieren**.

    Standardmäßig organisiert ASP.NET-Web API APIs, in das _Verzeichnis/API_ in einer Domäne, beispielsweise _http://www.adventure-works.com/api/orders_. Dieses Verzeichnis befindet sich in derselben Domäne wie andere Dienste von demselben Host verfügbar gemacht werden. Es kann im Web API sich in einem eigenen Unterdomäne auf einem separaten Host, mit URIs, z. B. _http://api.adventure-works.com/orders_ausgeführt Teilen hilfreich sein. Diese Trennung ermöglicht es Ihnen zu unterteilen und Skalieren im Web API effizienter ohne Auswirkung auf einer beliebigen anderen Webanwendungen oder Dienste, die in der Domäne _www.adventure-works.com_ ausgeführt.

    Eine Web-API in einer anderen Unterdomäne platzieren kann jedoch zu Sicherheitsgründen auch führen. Alle Webanwendungen oder Dienste bei _www.adventure-works.com_ gehostet, die eine Web-API an anderer Stelle ausgeführt aufrufen verstoßen gegen die Richtlinie same Origin vieler Webbrowser. In diesem Fall wird es Cross-Origin Ressource freigeben (CORS) zwischen den Hosts ermöglichen erforderlich. Weitere Informationen finden Sie unter API Sicherheit Anleitungen Dokument.

## <a name="considerations-for-processing-requests"></a>Aspekte, die für die Verarbeitung von Besprechungsanfragen

Nachdem eine Anforderung von einer Clientanwendung an eine Methode in einem Web-API erfolgreich weitergeleitet wurde, muss die Anfrage in als effizient wie möglich verarbeitet werden. Beim Implementieren des Codes zum Behandeln von Besprechungsanfragen, erwägen Sie die folgenden Punkte:

- **Aktionen abrufen, sich, löschen, Kopf, und PATCH Idempotent sein**.

    Der Code, der diese Anfragen implementiert, sollte nicht einer beliebigen Seite-Effekten vorgangseinschränkung. Die gleiche Anforderung wiederholt über dieselbe Ressource sollte im gleichen Zustand führen. Beispielsweise Senden von mehreren Löschvorgängen auf den gleichen URI sollte haben die gleiche Wirkung, zwar der HTTP-Status in der Antwort Code abweichen kann (die erste löschen Anforderung möglicherweise Status-Code zurückgegeben (kein Inhalt) 204 während eine nachfolgende löschen Anforderung Statuscode 404 (nicht gefunden) zurückgegeben werden kann).

> [AZURE.NOTE] Im Artikel [Messagingtransport Muster](http://blog.jonathanoliver.com/idempotency-patterns/) in der Jonathan Oliver Blog bietet es sich um einen Überblick über die Messagingtransport und Beziehung an Management Datenoperationen.

- **Bereitstellen von Aktionen, die beim Erstellen neuer Ressourcen, sollten tun Sie dies ohne nicht verknüpften Seite-Effekte**.

    Wenn eine POST-Anforderung zum Erstellen einer neuen Ressource dient, die Auswirkungen der Anfrage, um die neue Ressource sein (und möglicherweise eine direkt Ressourcen zugehörige ist es eine Art von Verknüpfung verbindet) beispielsweise in einem e-Commerce-System, eine POST-Anforderung, die einen neuen Auftrag für einen Kunden erstellt wird möglicherweise auch ändern Inventory Ebenen und Abrechnungsinformationen generieren , jedoch nicht Informationen nicht direkt im Zusammenhang mit der Reihenfolge ändern oder eine andere Seite-Effekte auf den allgemeinen Status von System haben sollten.

- **Vermeiden implementieren ausführliche Beitrag, sich, und löschen Vorgängen**.

    Unterstützung für Beitrag, sich und löschen Anforderungen über Ressource Websitesammlungen. Eine POST-Anforderung enthalten die Details für mehrere neue Ressourcen und alle dieselbe Auflistung hinzufügen kann, die Anforderung einer sich ersparen Sie sich den ganzen Satz von Ressourcen in einer Websitesammlung und eine Besprechungsanfrage löschen kann eine vollständige Auflistung entfernen.

    Beachten Sie, dass die OData-Unterstützung in ASP.NET Web API 2 enthalten die Möglichkeit zum Stapel Anfragen bietet. Eine Clientanwendung kann Verpacken von mehreren Web-API Besprechungsanfragen in einer einzigen HTTP-Anforderung an den Server zu senden und empfangen eine einzelne HTTP-Antwort, die die Antworten auf jede Anforderung enthält. Weitere Informationen finden Sie unter [Einführung in Stapel Unterstützung in Web-API und Web API OData](http://blogs.msdn.com/b/webdev/archive/2013/11/01/introducing-batch-support-in-web-api-and-web-api-odata.aspx) -Seite auf der Microsoft-Website.

- **Durch das HTTP-Protokoll beim Senden einer Antwort an eine Clientanwendung Abide**.

    Eine Web-API muss Nachrichten zurück, die den richtigen HTTP-Statuscode, damit den Client zu ermitteln, wie das Ergebnis, die entsprechenden HTTP-Header verarbeitet, damit der Client die Art von einer entsprechend formatierten Text so aktivieren Sie den Kunden, das Ergebnis zu analysieren und das Ergebnis versteht enthalten. Wenn Sie die Vorlage ASP.NET Web API verwenden, ist die Standardstrategie für die Implementierung von Methoden, die auf HTTP-POST-Anfragen reagieren einfach, um eine Kopie der neu erstellten Ressource zurückzukehren wie im folgenden Beispiel dargestellt:

    ```C#
    public class CustomersController : ApiController
    {
        ...
        [Route("api/customers")]
        [HttpPost]
        public Customer CreateNewCustomer(Customer customerDetails)
        {
            // Add the new customer to the repository
            // This method returns a customer with a unique ID allocated
            // by the repository
            var newCust = repository.Add(customerDetails);
            // Return the newly added customer
            return newCust;
        }
        ...
    }
    ```

    Wenn der Beitrag Vorgang erfolgreich ist, erstellt das Web-API Framework eine HTTP-Antwort mit Statuscode 200 (OK) und die Details des Kunden als Nachrichtentext an. In diesem Fall gemäß dem Protokoll HTTP POST-Operation sollte Statuscode jedoch zurück 201 (erstellt) und die Antwortnachricht sollte den URI der neu erstellten Ressource in der Kopfzeile Speicherort der Antwortnachricht enthalten.

    Um diese Features bereitzustellen, zurückgeben eigene HTTP-Antwortnachricht mithilfe der `IHttpActionResult` Schnittstelle. Dieser Ansatz gibt Ihnen genaue Kontrolle über die HTTP-Statuscode, der die Spaltenüberschriften in die Antwortnachricht, und auch das Format der Daten in den Textbereich der Nachricht Antwort wie im folgenden Beispiel dargestellt. Diese Version von der `CreateNewCustomer` Methode entspricht genauer erwartet Client das HTTP-Protokoll folgen. Die `Created` Methode zum der `ApiController` Klasse erstellt die Antwortnachricht aus den angegebenen Daten, und fügt die Ergebnisse die Kopfzeile Speicherort:

    ```C#
    public class CustomersController : ApiController
    {
        ...
        [Route("api/customers")]
        [HttpPost]
        public IHttpActionResult CreateNewCustomer(Customer customerDetails)
        {
            // Add the new customer to the repository
            var newCust = repository.Add(customerDetails);

            // Create a value for the Location header to be returned in the response
            // The URI should be the location of the customer including its ID,
            // such as http://adventure-works.com/api/customers/99
            var location = new Uri(...);

            // Return the HTTP 201 response,
            // including the location and the newly added customer
            return Created(location, newCust);
        }
        ...
    }
    ```

- **Inhalt verhandeln Support**.

    Der Text, der eine Antwortnachricht enthalten möglicherweise Daten in einer Vielzahl von Formaten. Beispielsweise eine HTTP GET-Anforderung eine Daten in JSON, zurückgeben oder XML-Format. Wenn der Client eine Anforderung sendet, können sie eine Kopfzeile akzeptieren enthalten, die den Datenformaten gibt an, die sie behandelt werden kann. Diese Formate werden als Medientypen angegeben. Ein Client, der eine GET-Anforderung Probleme, die ein Bild abruft kann beispielsweise eine Kopfzeile akzeptieren angeben, die die Medientypen, die der Client kann Listen, wie etwa "Image/Jpeg, Image/Gif, Image/Png behandeln".  Wenn das Web-API das Ergebnis zurück, sollten sie formatieren Sie die Daten mithilfe einer der folgenden Medientypen und geben Sie das Format in der Kopfzeile Inhaltstyp der Antwort.

    Wenn der Client eine Kopfzeile akzeptieren keinen angibt, verwenden Sie eine sinnvolle Standardformat für den Textkörper der Antwort ein. Beispielsweise wird standardmäßig das ASP.NET Web API Framework JSON für textbasierte Daten.

    > [AZURE.NOTE] Das ASP.NET Web API Framework führt einige automatische Erkennung von Kopfzeilen akzeptieren und sie selbst basierend auf den Typ der Daten im Textkörper der Antwortnachricht verarbeitet. Beispielsweise den Textkörper einer Antwortnachricht ein CLR (CLR)-Objekt enthält, formatiert die ASP.NET Web-API automatisch die Antwort als JSON mit dem Inhaltstyp-Header der Antwort auf "Application/Json" festgelegt werden, es sei denn, der Client zeigt an, dass es die Ergebnisse als XML, erfordert, in dem Fall die ASP.NET Web API Framework die Antwort als XML formatiert und legt den Inhaltstyp-Header der Antwort "Text/xml". Es ist wahrscheinlich jedoch akzeptieren Kopfzeilen verarbeitet, die verschiedenen Medientypen explizit in der Implementierungscode für einen Vorgang angeben.

- **Bereitstellen von Links zur Unterstützung von HATEOAS-Schreibweise Navigation und Suche nach Ressourcen**.

    Die API Entwurf Anleitung beschreibt, wie den HATEOAS Ansatz folgen kann ein Client navigieren und Ressourcen aus einer anfänglichen Ausgangspunkt ermitteln. Dies ist durch die Verwendung von Hyperlinks mit URIs; Wenn ein Client eine HTTP GET-Anforderung, um eine Ressource abzurufen, sollte die Antwort enthalten URIs, mit denen eine Clientanwendung direkt Ressourcenübersicht schnell zu finden. Angenommen, in einem Web-API, die eine e-Commerce-Lösung unterstützt, ein Kunden möglicherweise viele Aufträge erteilt haben. Wenn eine Clientanwendung die Details für einen Kunden abruft, sollte die Antwort Links enthalten, mit denen die Clientanwendung HTTP GET-Anfragen zu senden, die diese Aufträge abrufen können. Darüber hinaus sollte HATEOAS-Schreibweise Links andere Vorgänge (Beitrag, sich, löschen usw.) beschreiben Sie, dass jeder Ressource unterstützt zusammen mit den entsprechenden URI Ausführen jeder Anforderung verknüpft. Dieser Ansatz wird im Dokument API Entwurf Anleitungen ausführlicher beschrieben.

    Es sind derzeit keine Standards, die die Implementierung von HATEOAS Regeln, aber das folgende Beispiel zeigt einen möglichen Ansatz. In diesem Beispiel gibt eine HTTP GET-Anforderung, die die Details für einen Kunden findet eine Antwort, die HATEOAS Links enthalten, in denen die Bestellungen für diesen Kunden verwiesen:

    ```HTTP
    GET http://adventure-works.com/customers/2 HTTP/1.1
    Accept: text/json
    ...
    ```

    ```HTTP
    HTTP/1.1 200 OK
    ...
    Content-Type: application/json; charset=utf-8
    ...
    Content-Length: ...
    {"CustomerID":2,"CustomerName":"Bert","Links":[
      {"rel":"self",
       "href":"http://adventure-works.com/customers/2",
       "action":"GET",
       "types":["text/xml","application/json"]},
      {"rel":"self",
       "href":"http://adventure-works.com/customers/2",
       "action":"PUT",
       "types":["application/x-www-form-urlencoded"]},
      {"rel":"self",
       "href":"http://adventure-works.com/customers/2",
       "action":"DELETE",
       "types":[]},
      {"rel":"orders",
       "href":"http://adventure-works.com/customers/2/orders",
       "action":"GET",
       "types":["text/xml","application/json"]},
      {"rel":"orders",
       "href":"http://adventure-works.com/customers/2/orders",
       "action":"POST",
       "types":["application/x-www-form-urlencoded"]}
    ]}
    ```

    In diesem Beispiel werden die Kundendaten dargestellt, durch die `Customer` Klasse im folgenden Codeausschnitt dargestellt. Die HATEOAS Links bleiben an die `Links` Websitesammlungs-Eigenschaft:

    ```C#
    public class Customer
    {
        public int CustomerID { get; set; }
        public string CustomerName { get; set; }
        public List<Link> Links { get; set; }
        ...
    }

    public class Link
    {
        public string Rel { get; set; }
        public string Href { get; set; }
        public string Action { get; set; }
        public string [] Types { get; set; }
    }
    ```

    Der HTTP-GET-Vorgang ruft die Kundendaten von Speicher und Konstrukte einer `Customer` Objekt, und klicken Sie dann füllt die `Links` Websitesammlung. Das Ergebnis wird als eine Antwortnachricht JSON formatiert. Jeder Link umfasst die folgenden Felder:

    - Die Beziehung zwischen dem Objekt, das zurückgegeben wird und das Objekt, das durch den Link beschrieben. In diesem Fall "self" gibt an, dass die Verknüpfung ein Verweis auf das Objekt selbst wieder (ähnlich wie eine `this` Zeiger in vielen objektorientierten Sprachen), und "Orders" ist der Name einer Websitesammlung, die die verwandten Informationen enthält.

    - Hyperlink (`Href`) für das Objekt, indem Sie den Link in Form von einem URI beschrieben wird.

    - Den Typ der HTTP-Anforderung (`Action`), die an diesen URI gesendet werden.

    - Das Format der Daten (`Types`), die sollten, sofern Sie in der HTTP-Anforderung oder, die in der Antwort, je nach Art der Anfrage zurückgegeben werden kann.

    HATEOAS Links angezeigt, die im Beispiel HTTP-Antwort darauf hinzuweisen, dass eine Clientanwendung die folgenden Vorgänge ausführen kann:

    - Eine HTTP GET-Anforderung an den URI _http://adventure-works.com/customers/2_ die Details des Kunden (erneut) abgerufen werden sollen. Die Daten können als XML oder JSON zurückgegeben werden.

    - Eine Anforderung HTTP setzen, der URI _http://adventure-works.com/customers/2_ so ändern Sie die Details des Kunden. Die neuen Daten in der Anfragenachricht im X-www-form-urlencoded Format vorzulegen.

    - Eine Anforderung HTTP löschen, die URI- _http://adventure-works.com/customers/2_ So löschen Sie den Kunden. Die Anfrage keine erwarten zusätzliche Informationen und Daten im Nachrichtentext Antwort zurückgibt.

    - Eine HTTP GET-Anforderung an den URI _http://adventure-works.com/customers/2/orders_ aller Aufträge für den Kunden zu finden. Die Daten können als XML oder JSON zurückgegeben werden.

    - Eine Anforderung HTTP setzen, die URI- _http://adventure-works.com/customers/2/orders_ zum Erstellen eines neuen Auftrags für diesen Kunden. Die Daten in der Anfragenachricht im X-www-form-urlencoded Format vorzulegen.

## <a name="considerations-for-handling-exceptions"></a>Überlegungen für den Umgang mit Ausnahmen
Standardmäßig Wenn ein Vorgang eine nicht abgefangen Ausnahme gibt in Framework ASP.NET Web API Rahmen eine Antwortnachricht mit HTTP-Statuscode 500 (Interner Serverfehler). In vielen Fällen diesem Ansatz ist nicht in Grad der Isolation nützlich und macht die Ursache der Ausnahme schwierig zu ermitteln. Daher sollten Sie eine umfassendere Verfahren zur Behandlung von Ausnahmen, einführen, erwägen die folgenden Punkte:

- **Ausnahmen erfassen und eine aussagekräftige Antwort auf Clients zurückzukehren**.

    Der Code, der einen Vorgang HTTP implementiert sollten umfassende Ausnahme behandeln, anstatt ganz können nicht abgefangene wird an die im Rahmen der Web-API weitergegeben Ausnahmen bereitstellen. Wenn eine Ausnahme dies nicht möglich, der Vorgang erfolgreich abgeschlossen ist, die Ausnahme wieder in der Antwortnachricht übergeben werden kann, aber es sollte eine aussagekräftige Beschreibung des Fehlers die Ausnahme verantwortlichen enthalten. Die Ausnahme sollte auch die entsprechenden HTTP-Statuscode anstelle von einfach Rückgabe Statuscode 500 für jede Situation enthalten. Beispielsweise, wenn eine Anforderung Benutzer eine Aktualisierung der Datenbank, die eine Einschränkung ergibt (beispielsweise beim Versuch, einen Kunden zu löschen, die über ausstehende Bestellungen) verletzt, Sie sollten zurückgegeben Status 409 (Konflikt) und einen Nachrichtentext, die die Ursache für den Konflikt code. Wenn andere Umstände die Anfrage nicht realisierbar wiedergibt, können Sie Statuscode 400 (Ungültige Anforderung) zurück. Sie können eine vollständige Liste der HTTP-Statuscodes finden Sie auf der Seite [Status Codedefinitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) auf der W3C-Website.

    Der folgende Code zeigt ein Beispiel für unterschiedliche Situationen traps und gibt eine entsprechende Antwort an.

    ```C#
    [HttpDelete]
    [Route("customers/{id:int}")]
    public IHttpActionResult DeleteCustomer(int id)
    {
        try
        {
            // Find the customer to be deleted in the repository
            var customerToDelete = repository.GetCustomer(id);

            // If there is no such customer, return an error response
            // with status code 404 (Not Found)
            if (customerToDelete == null)
            {
                    return NotFound();
            }

            // Remove the customer from the repository
            // The DeleteCustomer method returns true if the customer
            // was successfully deleted
            if (repository.DeleteCustomer(id))
            {
                // Return a response message with status code 204 (No Content)
                // To indicate that the operation was successful
                return StatusCode(HttpStatusCode.NoContent);
            }
            else
            {
                // Otherwise return a 400 (Bad Request) error response
                return BadRequest(Strings.CustomerNotDeleted);
            }
        }
        catch
        {
            // If an uncaught exception occurs, return an error response
            // with status code 500 (Internal Server Error)
            return InternalServerError();
        }
    }
    ```

    > [AZURE.TIP] Fügen Sie Informationen ein, die sich ein Angreifer versucht, das Web-API zugelassen werden könnten. Weitere Informationen finden Sie auf der Seite [Ausnahme Handling in ASP.NET Web-API](http://www.asp.net/web-api/overview/error-handling/exception-handling) auf der Microsoft-Website.

    > [AZURE.NOTE] Viele Webserver überfüllt Fehler Bedingungen selbst aus, bevor sie das Web-API erreichen. Wenn Sie konfigurieren die Authentifizierung für eine Website und der Benutzer nicht die richtigen Authentifizierungsinformationen bereitstellen, sollte der Webserver mit Statuscode 401 (nicht autorisiert) Antworten. Nachdem ein Client authentifiziert wurde, kann Code ausführen, eine eigene überprüft, um sicherzustellen, dass der Client die angeforderte Ressource zugreifen sollte. Wenn diese Autorisierung fehlschlägt, sollten Sie den Statuscode 403 (Verboten) zurückgeben.

- **Behandeln von Ausnahmen in einer konsistent und die Protokollinformationen zu Fehlern**.

    Erwägen Sie zum Behandeln von Ausnahmen konsistent Implementieren eines globalen Fehlerbehandlung Strategie über das gesamte Web-API ein. Erzielen Sie diesen Teil, erstellen einen Ausnahmefilter, die ausgeführt wird, wenn ein Controller alle Ausnahmefehler auslöst, der keiner `HttpResponseException` Ausnahme. Dieser Ansatz wird auf der Seite [Ausnahme Handling in ASP.NET Web-API](http://www.asp.net/web-api/overview/error-handling/exception-handling) auf der Microsoft-Website beschrieben.

    Es gibt jedoch Situationen erfordern ein Ausnahmefilter, keine Ausnahme abgefangen, einschließlich:

    - Von Controller Konstruktoren ausgelösten Ausnahmen.

    - Aus der Nachricht Ereignishandler ausgelöste Ausnahmen.

    - Während routing ausgelösten Ausnahmen.

    - Beim Serialisieren des Inhalts für eine Antwortnachricht ausgelöste Ausnahmen.

    Diese Fälle, müssen Sie möglicherweise einen Weitere angepassten Ansatz implementieren. Sie sollten auch einbinden zurück, die die vollständigen Details der einzelnen Ausnahme erfasst Protokollierung; Dieses Fehlerprotokoll kann detaillierten Informationen enthalten, solange sie nicht über das Web an Clients zugänglich ist. Im Artikel [Web API globalen Fehlerbehandlung](http://www.asp.net/web-api/overview/error-handling/web-api-global-error-handling) auf der Microsoft-Website ist eine Möglichkeit zum Ausführen dieser Aufgabe.

- **Formatvorlagenquelle kennzeichnen zwischen clientseitige und serverseitige Fehlern**.

    Das HTTP-Protokoll unterscheidet zwischen aufgrund der Clientanwendung (HTTP 4xx Status-Codes) aufgetretenen und Fehlern, die durch ein Problem auf dem Server (HTTP 5xx Status-Codes) verursacht werden. Stellen Sie sicher, dass Sie diese Messe in eine beliebige Antwort Fehlermeldungen berücksichtigen.

<a name="considerations-for-optimizing"></a>
## <a name="considerations-for-optimizing-client-side-data-access"></a>Hinweise zum Optimieren clientseitige Daten zugreifen

In einer verteilten Umgebung wie z. B., die im Zusammenhang mit einem Webserver und Clientanwendungen ist eine der primären Quellen von Belang im Netzwerk. Dies kann als Engpassressource dazu, fungieren, insbesondere dann, wenn eine Clientanwendung häufig Besprechungsanfragen senden oder Empfangen von Daten. Daher sollten Sie Ziel die Menge des Datenverkehrs minimieren, die im Netzwerk fließt. Beachten Sie die folgenden Punkte, beim Implementieren des Codes zum Abrufen und Verwalten von Daten:

- **Support clientseitige Zwischenspeichern**.

    HTTP 1.1-Protokoll unterstützt das Zwischenspeichern von in-Clients und mittlere Servern über denen eine Anforderung durch die Verwendung der Cache-Control Kopfzeile geroutet werden. Wenn eine Clientanwendung eine HTTP GET Anfordern der Web-API sendet, die Antwort kann eine Cache-Control Kopfzeile hinzufügen, die angibt, ob die Daten in den Textkörper der Antwort sicheres zwischengespeichert werden können, indem Sie die Client- oder eine mittlere bis zu dem die Anfrage weitergeleitet wurde, und wie lange vor dem ablaufen soll und veraltet angesehen werden. Im folgenden Beispiel wird eine HTTP GET-Anforderung und die zugehörige Antwort, die eine Kopfzeile Cache-Steuerelemente enthält:

    ```HTTP
    GET http://adventure-works.com/orders/2 HTTP/1.1
    ...
    ```

    ```HTTP
    HTTP/1.1 200 OK
    ...
    Cache-Control: max-age=600, private
    Content-Type: text/json; charset=utf-8
    Content-Length: ...
    {"orderID":2,"productID":4,"quantity":2,"orderValue":10.00}
    ```

    In diesem Beispiel gibt der Cache-Control-Header an, dass die Daten zurückgegeben 600 Sekunden abgelaufen sein sollte, eignet sich nur für einen einzelnen Client und nicht in einem freigegebenen Cache verwendet, die von anderen Clients (es wird _als "Privat"_) gespeichert werden. Die Kopfzeile Cache-Control konnte geben _öffentlichen_ anstelle von _als "Privat"_ in diesem Fall die Daten in einer gemeinsam genutzten Cache gespeichert werden können, oder es konnte _kein-Store_ angeben, der die Daten in diesem Fall müssen **nicht** vom Client zwischengespeichert werden. Im folgenden Code wird gezeigt, wie eine Kopfzeile Cache-Control in eine Antwortnachricht erstellt:

    ```C#
    public class OrdersController : ApiController
    {
        ...
        [Route("api/orders/{id:int:min(0)}")]
        [HttpGet]
        public IHttpActionResult FindOrderByID(int id)
        {
            // Find the matching order
            Order order = ...;
            ...
            // Create a Cache-Control header for the response
            var cacheControlHeader = new CacheControlHeaderValue();
            cacheControlHeader.Private = true;
            cacheControlHeader.MaxAge = new TimeSpan(0, 10, 0);
            ...

            // Return a response message containing the order and the cache control header
            OkResultWithCaching<Order> response = new OkResultWithCaching<Order>(order, this)
            {
                CacheControlHeader = cacheControlHeader
            };
            return response;
        }
        ...
    }
    ```

    Dieser Code macht Verwenden eines benutzerdefinierten `IHttpActionResult` Klasse mit dem Namen `OkResultWithCaching`. Diese Klasse ermöglicht den Controller Festlegen des Inhalts der Kopfzeile Cache:

    ```C#
    public class OkResultWithCaching<T> : OkNegotiatedContentResult<T>
    {
        public OkResultWithCaching(T content, ApiController controller)
            : base(content, controller) { }

        public OkResultWithCaching(T content, IContentNegotiator contentNegotiator, HttpRequestMessage request, IEnumerable<MediaTypeFormatter> formatters)
            : base(content, contentNegotiator, request, formatters) { }

        public CacheControlHeaderValue CacheControlHeader { get; set; }
        public EntityTagHeaderValue ETag { get; set; }

        public override async Task<HttpResponseMessage> ExecuteAsync(CancellationToken cancellationToken)
        {
            HttpResponseMessage response = await base.ExecuteAsync(cancellationToken);

            response.Headers.CacheControl = this.CacheControlHeader;
            response.Headers.ETag = ETag;

            return response;
        }
    }
    ```

    > [AZURE.NOTE] Das HTTP-Protokoll definiert auch die Richtlinie _keine-Cache_ für den Cache-Control-Header. Lieber bisschen, bedeutet dieser Richtlinie nicht "nicht zwischenspeichern" aber "erneut überprüfen lieber die zwischengespeicherte Informationen mit dem Server vor der Rückgabe"; können die Daten weiterhin zwischengespeichert werden, aber es wird jedes Mal überprüft wird verwendet, um sicherzustellen, dass es immer noch aktiv ist.

    Cache-Verwaltung ist rein der Clientanwendung oder zwischen-XT für Server, aber wenn ordnungsgemäß implementiert können sie Bandbreite speichern und Verbessern der Leistung durch das Entfernen von Daten abzurufen, die bereits zuletzt abgerufen werden müssen.

    Der _Max-Alter_ Wert in der Kopfzeile Cache-Control ist nur eine Führungslinie und keine Garantie für die die entsprechenden Daten nicht im angegebenen Zeitraum ändern. Das Max-Alter sollte einen geeigneten Wert je nach der erwarteten Flüchtigkeit der Daten im Web-API festgelegt. Beim Ablauf dieses Zeitraums sollte der Client das Objekt aus dem Cache verwerfen.

    > [AZURE.NOTE] Die meisten modernen Webbrowser unterstützen clientseitige Zwischenspeichern durch Hinzufügen der entsprechenden Cache-Control-Header auf Anfragen und untersuchen die Kopfzeilen der Ergebnisse, wie beschrieben. Allerdings werden einige ältere Browser nicht im cache die Werte zurückgegeben, die von einer URL, die eine Abfragezeichenfolge enthält. Dies ist nicht in der Regel ein Problem für benutzerdefinierte Clientanwendungen die eigene basierend auf das Protokoll, die hier beschriebenen Cache Management-Strategie implementieren.
    >
    > Einige ältere Proxys derselben Verhalten aufweisen und Besprechungsanfragen basierend auf URLs mit Abfragezeichenfolgen möglicherweise nicht zwischenspeichern. Dies kann ein Problem für benutzerdefinierte Clientanwendungen sein, die auf einen Webserver über solche Proxy verbinden.

- **Bereitstellen ETags zum Abfrageverarbeitung zu optimieren**.

    Wenn eine Clientanwendung ein Objekt abruft, kann die Antwortnachricht auch eine _ETag_ (Entitätstag) enthalten. Ein ETag handelt es sich um eine undurchsichtig Zeichenfolge, die die Version einer Ressource angibt; jedes Mal, die eine Ressource das Etag ändert sich wird auch geändert. Dieser ETag sollten als Teil der Daten von der Clientanwendung zwischengespeichert werden. Im folgenden Code wird gezeigt, wie eine ETag als Teil der Antwort auf eine HTTP GET-Anforderung hinzufügen. In diesem Code wird die `GetHashCode` Methode eines Objekts in einen numerischen Wert zu erzeugen, die das Objekt (Sie können diese Methode überschreiben, falls notwendig und generieren eigene Hash mit einem Algorithmus wie MD5) identifiziert:

    ```C#
    public class OrdersController : ApiController
    {
        ...
        public IHttpActionResult FindOrderByID(int id)
        {
            // Find the matching order
            Order order = ...;
            ...

            var hashedOrder = order.GetHashCode();
            string hashedOrderEtag = String.Format("\"{0}\"", hashedOrder);
            var eTag = new EntityTagHeaderValue(hashedOrderEtag);

            // Return a response message containing the order and the cache control header
            OkResultWithCaching<Order> response = new OkResultWithCaching<Order>(order, this)
            {
                ...,
                ETag = eTag
            };
            return response;
        }
        ...
    }
    ```

    Die Antwortnachricht, die von der Web-API bereitgestellt wurde, sieht wie folgt aus:

    ```HTTP
    HTTP/1.1 200 OK
    ...
    Cache-Control: max-age=600, private
    Content-Type: text/json; charset=utf-8
    ETag: "2147483648"
    Content-Length: ...
    {"orderID":2,"productID":4,"quantity":2,"orderValue":10.00}
    ```

    > [AZURE.TIP] Aus Gründen der Sicherheit nicht lassen Sie vertrauliche Daten oder Daten über eine authentifizierte (HTTPS) Verbindung zwischengespeichert werden zurückgegeben zu.

    Eine Clientanwendung kann eine nachfolgende GET-Anforderung zum Abrufen von dieselbe Ressource zu einem beliebigen Zeitpunkt, Emission und die Ressource geändert wurden (es hat einen anderen ETag) die zwischengespeicherte Version verworfen werden muss und die neue Version, die dem Cache hinzugefügt. Wenn eine Ressource umfangreich ist und erfordert sehr viel Bandbreite an den Client übertragen, können wiederholte Abfragen dieselben Daten abgerufen werden sollen nicht effizient dar. Um dies zu begegnen, definiert das HTTP-Protokoll folgen dieser Vorgehensweise zum Optimieren GET-Anfragen, die in einem Web-API unterstützt werden sollen:

    - Der Client erstellt eine GET-Anforderung, enthält das ETag für die aktuell zwischengespeicherte Version der Ressource in einem If-keine-Match HTTP-Header verwiesen wird:

    ```HTTP
    GET http://adventure-works.com/orders/2 HTTP/1.1
    If-None-Match: "2147483648"
    ...
    ```

    - Der GET-Vorgang im Web API erhält das aktuelle ETag für die angeforderten Daten (im obigen Beispiel Reihenfolge 2) und der Wert in der Kopfzeile If-keine-Match verglichen.

    - Entspricht der aktuellen ETag für die angeforderten Daten das ETag zur Verfügung gestellt, indem Sie die Anforderung die Ressource nicht geändert, und das Web-API sollte eine HTTP-Antwort mit einem leeren Nachrichtentext und einen Statuscode der 304 (nicht verändert) zurückgeben.

    - Wenn das aktuelle ETag für die angeforderten Daten nicht das ETag zur Verfügung gestellt, indem Sie die Anforderung übereinstimmt, die Daten geändert hat, und im Web API sollte eine HTTP-Antwort mit den neuen Daten in den Nachrichtentext und einen Statuscode 200 (OK) zurückgeben.

    - Wenn die angeforderten Daten nicht mehr vorhanden ist, sollte im Web-API eine HTTP-Antwort mit dem Statuscode 404 (nicht gefunden) zurückgeben.

    - Der Client verwendet den Statuscode, um den Cache verwalten. Wenn die Daten nicht (Statuscode 304) und dann auf das Objekt kann Cache bleiben und die Clientanwendung sollten weiterhin verwenden diese Version des Objekts geändert hat. Wenn die Daten (Statuscode 200) geändert haben, und klicken Sie dann das zwischengespeicherte Objekt verworfen werden muss, und das neue Bild eingefügt. Wenn die Daten nicht mehr verfügbar ist (Statuscode 404) und dann auf das Objekt aus dem Cache entfernt werden soll.

    > [AZURE.NOTE] Wenn der Antwort-Header Cache-Control Kopfzeile keine-Store enthält, und klicken Sie dann das Objekt immer aus dem Cache unabhängig von den HTTP-Code, der entfernt werden soll.

    Der folgende Code zeigt die `FindOrderByID` Methode zur Unterstützung der Kopfzeile If-keine-Match erweitert. Beachten Sie, dass die Kopfzeile If-keine-Match ausgelassen wird, die angegebene Reihenfolge immer abgerufen wird:

    ```C#
    public class OrdersController : ApiController
    {
        ...
        [Route("api/orders/{id:int:min(0)}")]
        [HttpGet]
        public IHttpActionResult FindOrderById(int id)
        {
            try
            {
                // Find the matching order
                Order order = ...;

                // If there is no such order then return NotFound
                if (order == null)
                {
                    return NotFound();
                }

                // Generate the ETag for the order
                var hashedOrder = order.GetHashCode();
                string hashedOrderEtag = String.Format("\"{0}\"", hashedOrder);

                // Create the Cache-Control and ETag headers for the response
                IHttpActionResult response = null;
                var cacheControlHeader = new CacheControlHeaderValue();
                cacheControlHeader.Public = true;
                cacheControlHeader.MaxAge = new TimeSpan(0, 10, 0);
                var eTag = new EntityTagHeaderValue(hashedOrderEtag);

                // Retrieve the If-None-Match header from the request (if it exists)
                var nonMatchEtags = Request.Headers.IfNoneMatch;

                // If there is an ETag in the If-None-Match header and
                // this ETag matches that of the order just retrieved,
                // then create a Not Modified response message
                if (nonMatchEtags.Count > 0 &&
                    String.Compare(nonMatchEtags.First().Tag, hashedOrderEtag) == 0)
                {
                    response = new EmptyResultWithCaching()
                    {
                        StatusCode = HttpStatusCode.NotModified,
                        CacheControlHeader = cacheControlHeader,
                        ETag = eTag
                    };
                }
                // Otherwise create a response message that contains the order details
                else
                {
                    response = new OkResultWithCaching<Order>(order, this)
                    {
                        CacheControlHeader = cacheControlHeader,
                        ETag = eTag
                    };
                }

                return response;
            }
            catch
            {
                return InternalServerError();
            }
        }
    ...
    }
    ```

    In diesem Beispiel übernimmt eine zusätzliche benutzerdefinierte `IHttpActionResult` Klasse mit dem Namen `EmptyResultWithCaching`. Diese Klasse fungiert einfach als Wrapper um eine `HttpResponseMessage` Objekt, das keine Antworttext enthält:

    ```C#
    public class EmptyResultWithCaching : IHttpActionResult
    {
        public CacheControlHeaderValue CacheControlHeader { get; set; }
        public EntityTagHeaderValue ETag { get; set; }
        public HttpStatusCode StatusCode { get; set; }
        public Uri Location { get; set; }

        public async Task<HttpResponseMessage> ExecuteAsync(CancellationToken cancellationToken)
        {
            HttpResponseMessage response = new HttpResponseMessage(StatusCode);
            response.Headers.CacheControl = this.CacheControlHeader;
            response.Headers.ETag = this.ETag;
            response.Headers.Location = this.Location;
            return response;
        }
    }
    ```

    > [AZURE.TIP] In diesem Beispiel wird das ETag für die Daten durch die Daten aus der zugrunde liegenden Datenquelle abgerufen hashing generiert. Wenn das ETag auf andere Weise berechnet werden kann, der Prozess weiter optimiert werden kann, und nur die Daten aus der Datenquelle abgerufen werden, wenn es geändert hat müssen.  Dieser Ansatz ist besonders hilfreich, wenn die Daten umfangreich ist oder den Zugriff auf die Datenquelle im Wesentlichen Wartezeit ergeben kann (z. B., wenn die Datenquelle einer Remotedatenbank ist).

- **ETags verwenden, um die Unterstützung der vollständigen Parallelität**.

    Zum Aktivieren des Updates über zuvor zwischengespeicherte Daten unterstützt das HTTP-Protokoll eine vollständige Parallelität Strategie. Wenn nach das Abrufen und Zwischenspeichern eine Ressource, die Clientanwendung später eine Anforderung setzen oder löschen sendet, ändern oder Entfernen der Ressource, sollte im If-Match-Header enthalten, die das ETag verweist. Das Web-API können Sie diese Informationen zu bestimmen, ob die Ressource bereits von einem anderen Benutzer geändert wurde, seit sie abgerufen wurde, und senden Sie eine entsprechende Antwort an die Clientanwendung wie folgt verwenden:

    - Der Client erstellt die Anforderung einer sich mit den neuen Details für die Ressource und das ETag für die aktuell zwischengespeicherte Version der Ressource in einem If-Match HTTP-Header verwiesen wird. Im folgenden Beispiel wird die Anforderung einer sich die Ordnung aktualisiert:

    ```HTTP
    PUT http://adventure-works.com/orders/1 HTTP/1.1
    If-Match: "2282343857"
    Content-Type: application/x-www-form-urlencoded
    ...
    Date: Fri, 12 Sep 2014 09:18:37 GMT
    Content-Length: ...
    productID=3&quantity=5&orderValue=250
    ```

    - Der Vorgang sich im Web API erhält das aktuelle ETag für die angeforderten Daten (im obigen Beispiel Reihenfolge 1) und der Wert in der Kopfzeile If-Match verglichen.

    - Entspricht der aktuellen ETag für die angeforderten Daten das ETag zur Verfügung gestellt, indem Sie die Anforderung die Ressource nicht geändert, und das Web-API sollte das Update, eine Nachricht mit HTTP-Statuscode 204 (kein Inhalt) zurückgeben, ist es successful ausführen. Die Antwort kann Cache-Control und ETag Kopfzeilen für die aktualisierte Version der Ressource enthalten. Die Antwort sollte immer Location-Header enthalten, der den URI der aktualisierten Ressource verweist.

    - Wenn das aktuelle ETag für die angeforderten Daten nicht das ETag zur Verfügung gestellt, indem Sie die Anfrage übereinstimmt, wurde dann die Daten geändert von einem anderen Benutzer seit dem Abruf wurde und im Web API sollte eine HTTP-Antwort mit einem leeren Nachrichtentext und einen Statuscode der 412 (Vorbedingung fehlgeschlagen) zurückgeben.

    - Wenn die Ressource aktualisiert werden nicht mehr vorhanden ist sollte das Web-API eine HTTP-Antwort mit dem Statuscode 404 (nicht gefunden) zurück.

    - Der Client verwendet die Status Code und Antwort Überschriften, um den Cache zu verwalten. Wenn die Daten wurde aktualisiert (Statuscode 204) aus, und klicken Sie dann das Objekt Cache bleiben, (solange die Kopfzeile Cache-Control kein keine-Store angegeben wird) aber das ETag aktualisiert werden sollen. Wenn die Daten von einem anderen Benutzer geändert (Statuscode 412) geändert wurden oder (Statuscode 404) nicht gefunden wird, und klicken Sie dann das zwischengespeicherte Objekt verworfen werden sollen.

    Im folgenden Beispiel wird zeigt eine Implementierung des Vorgangs sich für den Bestellungen Controller an:

    ```C#
    public class OrdersController : ApiController
    {
        ...
        [HttpPut]
        [Route("api/orders/{id:int}")]
                public IHttpActionResult UpdateExistingOrder(int id, DTOOrder order)
        {
            try
            {
                var baseUri = Constants.GetUriFromConfig();
                var orderToUpdate = this.ordersRepository.GetOrder(id);
                if (orderToUpdate == null)
                {
                    return NotFound();
                }

                var hashedOrder = orderToUpdate.GetHashCode();
                string hashedOrderEtag = String.Format("\"{0}\"", hashedOrder);

                // Retrieve the If-Match header from the request (if it exists)
                var matchEtags = Request.Headers.IfMatch;

                // If there is an Etag in the If-Match header and
                // this etag matches that of the order just retrieved,
                // or if there is no etag, then update the Order
                if (((matchEtags.Count > 0 &&
                     String.Compare(matchEtags.First().Tag, hashedOrderEtag) == 0)) ||
                     matchEtags.Count == 0)
                {
                    // Modify the order
                    orderToUpdate.OrderValue = order.OrderValue;
                    orderToUpdate.ProductID = order.ProductID;
                    orderToUpdate.Quantity = order.Quantity;

                    // Save the order back to the data store
                    // ...

                    // Create the No Content response with Cache-Control, ETag, and Location headers
                    var cacheControlHeader = new CacheControlHeaderValue();
                    cacheControlHeader.Private = true;
                    cacheControlHeader.MaxAge = new TimeSpan(0, 10, 0);

                    hashedOrder = order.GetHashCode();
                    hashedOrderEtag = String.Format("\"{0}\"", hashedOrder);
                    var eTag = new EntityTagHeaderValue(hashedOrderEtag);

                    var location = new Uri(string.Format("{0}/{1}/{2}", baseUri, Constants.ORDERS, id));
                    var response = new EmptyResultWithCaching()
                    {
                        StatusCode = HttpStatusCode.NoContent,
                        CacheControlHeader = cacheControlHeader,
                        ETag = eTag,
                        Location = location
                    };

                    return response;
                }

                // Otherwise return a Precondition Failed response
                return StatusCode(HttpStatusCode.PreconditionFailed);
            }
            catch
            {
                return InternalServerError();
            }
        }
        ...
    }
    ```

    > [AZURE.TIP] Verwenden der If-Match Kopfzeile ist vollständig optional, und es ist weggelassen im Web API immer versucht wird, aktualisieren die angegebene Reihenfolge oftmals Blind überschrieben wird eine Aktualisierung von einem anderen Benutzer vorgenommen. Zur Vermeidung von Problemen aufgrund von verlorenen Aktualisierungen immer bieten Sie einen If-Match-Header.

<a name="considerations-for-handling-large"></a>
## <a name="considerations-for-handling-large-requests-and-responses"></a>Überlegungen für den Umgang mit großen Besprechungsanfragen und Antworten

Wann muss eine Clientanwendung Anfragen Emission, das Senden oder Empfangen von Daten, die möglicherweise mehrere Megabyte Anlässe möglicherweise (oder vergrößern) Größe. Warten, während der Übertragung dieser Datenmenge könnte die Clientanwendung zu reagieren. Beachten Sie die folgenden Punkte, wenn Sie Besprechungsanfragen behandeln, die große Datenmengen enthalten müssen:

- **Optimieren Besprechungsanfragen und Antworten, die große Objekten verlangsamen**.

    Große Objekte einige Ressourcen ist entweder oder große Felder, wie etwa Grafiken, Bildern oder anderen Arten von binäre Daten enthalten. Eine Web-API sollte unterstützen streaming um optimierten hochladen und Herunterladen von diese Ressourcen zu ermöglichen.

    Das HTTP-Protokoll bietet aufgeteilter durchstellen Codierung Verfahren zum Streamen große Datenobjekte Zurücksenden an einen Client. Sendet der Client eine HTTP GET-Anforderung für ein großes Objekt, kann im Web-API die Antwort in stückweisem _Blöcken_ über eine HTTP-Verbindung senden. Die Länge der Daten in die Antwort möglicherweise nicht Anfangs bekannt sein (es generiert werden kann), damit der Server Hosten im Web-API eine Antwortnachricht, wobei jeder Block senden soll, die die Übertragung Codierung angibt: aufgeteilter Kopfzeile statt einer Content-Length-Kopfzeile. Die Clientanwendung kann jeder Ausschnitt wiederum zum Erstellen von vollständige Antwort erhalten. Die Datenübertragung abgeschlossen ist, wenn der Server wieder ein abgeschlossen Block mit 0 (null) Größe sendet. Sie können implementieren Aufteilen in Abschnitte in der ASP.NET Web-API mithilfe der `PushStreamContent` Class.

    Im folgenden Beispiel wird einen Vorgang, der auf HTTP GET-Anfragen für Produktbilder reagiert:

    ```C#
    public class ProductImagesController : ApiController
    {
        ...
        [HttpGet]
        [Route("productimages/{id:int}")]
        public IHttpActionResult Get(int id)
        {
            try
            {
                var container = ConnectToBlobContainer(Constants.PRODUCTIMAGESCONTAINERNAME);

                if (!BlobExists(container, string.Format("image{0}.jpg", id)))
                {
                    return NotFound();
                }
                else
                {
                    return new FileDownloadResult()
                    {
                        Container = container,
                        ImageId = id
                    };
                }
            }
            catch
            {
                return InternalServerError();
            }
        }
        ...
    }
    ```

    In diesem Beispiel `ConnectBlobToContainer` ist eine Helper-Methode, die mit einem angegebenen Container (Name nicht dargestellt) verbindet Azure Blob-Speicher. `BlobExists`ist eine andere Helper-Methode, die einen booleschen Wert, der angibt zurückgibt, ob im Container BLOB-Speicher ein Blob mit dem angegebenen Namen vorhanden ist.

    Jedes Produkt verfügt über eine eigene Bild frei, die im BLOB-Speicher. Die `FileDownloadResult` Klasse ist ein benutzerdefiniertes `IHttpActionResult` Klasse, verwendet eine `PushStreamContent` -Objekt, das die Bilddaten aus der entsprechenden Blob gelesen und asynchrone als Inhalt der Antwortnachricht zu übertragen:

    ```C#
    public class FileDownloadResult : IHttpActionResult
    {
        public CloudBlobContainer Container { get; set; }
        public int ImageId { get; set; }

        public async Task<HttpResponseMessage> ExecuteAsync(CancellationToken cancellationToken)
        {
            var response = new HttpResponseMessage();
            response.Content = new PushStreamContent(async (outputStream, _, __) =>
            {
                try
                {
                    CloudBlockBlob blockBlob = Container.GetBlockBlobReference(String.Format("image{0}.jpg", ImageId));
                    await blockBlob.DownloadToStreamAsync(outputStream);
                }
                finally
                {
                    outputStream.Close();
                }
            });

            response.StatusCode = HttpStatusCode.OK;
            response.Content.Headers.ContentType = new MediaTypeHeaderValue("image/jpeg");
            return response;
        }
    }
    ```

    Sie können auch anwenden, um Vorgänge hochladen, wenn eine neue Ressource bereitstellen, die ein großes Objekt enthält ein Client muss streaming. Im nächste Beispiel zeigt die Beitrag Methode für die `ProductImages` Controller. Mit dieser Methode können den Kunden ein neues Produktbild hochladen:

    ```C#
    public class ProductImagesController : ApiController
    {
        ...
        [HttpPost]
        [Route("productimages")]
        public async Task<IHttpActionResult> Post()
        {
            try
            {
                if (!Request.Content.Headers.ContentType.MediaType.Equals("image/jpeg"))
                {
                    return StatusCode(HttpStatusCode.UnsupportedMediaType);
                }
                else
                {
                    var id = new Random().Next(); // Use a random int as the key for the new resource. Should probably check that this key has not already been used
                    var container = ConnectToBlobContainer(Constants.PRODUCTIMAGESCONTAINERNAME);
                    return new FileUploadResult()
                    {
                        Container = container,
                        ImageId = id,
                        Request = Request
                    };
                }
            }
            catch
            {
                return InternalServerError();
            }
        }
        ...
    }
    ```

    Dieser Code verwendet einen anderen benutzerdefinierten `IHttpActionResult` Klasse mit dem Namen `FileUploadResult`. Diese Klasse enthält die Logik für asynchrone Hochladen der Daten:

    ```C#
    public class FileUploadResult : IHttpActionResult
    {
        public CloudBlobContainer Container { get; set; }
        public int ImageId { get; set; }
        public HttpRequestMessage Request { get; set; }

        public async Task<HttpResponseMessage> ExecuteAsync(CancellationToken cancellationToken)
        {
            var response = new HttpResponseMessage();
            CloudBlockBlob blockBlob = Container.GetBlockBlobReference(String.Format("image{0}.jpg", ImageId));
            await blockBlob.UploadFromStreamAsync(await Request.Content.ReadAsStreamAsync());
            var baseUri = string.Format("{0}://{1}:{2}", Request.RequestUri.Scheme, Request.RequestUri.Host, Request.RequestUri.Port);
            response.Headers.Location = new Uri(string.Format("{0}/productimages/{1}", baseUri, ImageId));
            response.StatusCode = HttpStatusCode.OK;
            return response;
        }
    }
    ```

    > [AZURE.TIP] Die Menge der Daten, die Sie zu einem Webdienst hochladen können nicht durch streamen eingeschränkt ist, und eine einzelne Anforderung denkbar in einer umfangreichen Objekt, der dazu Ressourcen ergeben. Wenn während des Vorgangs streaming im Web API bestimmt, dass die Menge der Daten in einer Anforderung einige zulässigen Grenzen überschritten hat, können Sie den Vorgang abzubrechen und eine Antwortnachricht mit Statuscode 413 (Anforderung Entität zu groß) zurückzukehren.

    Sie können die Größe des großen Objekte im Netzwerk mithilfe von HTTP-Komprimierung übertragenen minimieren. Dieser Ansatz hilft, um den Netzwerkverkehr und die zugehörigen Netzwerkwartezeit Typfehler weitere Verarbeitung auf dem Client und dem Server Hosten im Web API zu verringern. Beispielsweise kann eine Clientanwendung, die erwartet komprimierte Daten erhalten eine akzeptieren Codierung einbeziehen: Gzip Anforderung Kopfzeile (andere Daten Komprimierung Algorithmen ebenfalls angegeben werden können). Wenn der Server Komprimierung unterstützt sollte die Antworten für den Inhalt frei, die im Gzip-Format in den Nachrichtentext und die Inhalte Codierung: Gzip Antwort Kopfzeile.

    > [AZURE.TIP] Sie können codierte Komprimierung ohne streaming kombinieren. Komprimieren Sie die Daten zunächst vor streaming, und geben Sie im Inhalt Gzip-Codierung und aufgeteilter übertragen, die in den Kopfzeilen Codierung. Beachten Sie auch, dass einige Webserver (wie etwa Internet Information Server) konfiguriert werden können, um HTTP-Antworten, unabhängig davon, ob das Web-API Daten komprimiert automatisch zu komprimieren.

- **Implementieren teilweise Antworten für Clients, die keine asynchrone Vorgänge unterstützen**.

    Als Alternative zum asynchrone streaming kann eine Clientanwendung Daten für große Objekte in Blöcken, bekannt als teilweise Antworten explizit anfordern. Die Clientanwendung sendet eine Anforderung HTTP Kopf zu Informationen über das Objekt zu erhalten. Wenn das Web-API Antworten teilweise unterstützt, wenn sollte Antworten auf die Anfrage Kopf mit eine Antwortnachricht, die enthält eine annehmen-Bereiche Kopf- und einen Content-Length-Header, der die Gesamtgröße des Objekts angibt, aber der Hauptteil der Nachricht leer sein sollte. Die Clientanwendung kann diese Informationen verwenden, um eine Reihe von GET-Anfragen erstellen, die einen Bereich von Bytes erhalten angeben. Die Web-API sollte eine Antwortnachricht mit HTTP-Status 206 (teilweise Inhalt), einer Kopfzeile Content-Length zurückgeben, die angibt, die tatsächliche Datenmenge im Textkörper der Antwortnachricht und einer Content-Range Kopfzeile, die angibt, welcher Teil (z. B. Bytes 4000 bis 8000) das Objekt diese Daten dar enthalten.

    HTTP-Kopf Besprechungsanfragen und Antworten teilweise werden im Dokument API Entwurf Anleitungen ausführlicher beschrieben.

- **Vermeiden unnötige weiter Statusnachrichten in Clientanwendungen senden**.

    Eine Clientanwendung, die eine große Datenmenge auf einem Server sendet möglicherweise zuerst bestimmen, ob der Server tatsächlich die Anforderung akzeptiert wird. Vor dem Senden der Daten, kann die Clientanwendung eine HTTP-Anforderung mit einer erwartete übermitteln: 100-Fortsetzen der Kopfzeile einer Content-Length-Kopfzeile, die die Größe der Daten, aber einen leeren Nachrichtentext angibt. Wenn der Server zum Bearbeiten der Anforderung bereit ist, sollte es mit einer Nachricht antworten, die angibt, den HTTP-Status 100 (Continue). Dann können Sie die Clientanwendung fortfahren und abgeschlossen einschließlich der Daten im Nachrichtentext Anforderung senden.

    Wenn Sie einen Dienst mithilfe von IIS hosten, der HTTP-Treiber werden automatisch erkannt und erwartete verarbeitet: 100-Kopfzeilen vor der Übergabe Anfragen an Ihrer Webanwendung fortsetzen. Dies bedeutet, dass Sie sind wahrscheinlich nicht, wenn diese Überschriften in Ihrer Anwendungscode sehen, können Sie davon ausgehen, dass IIS noch keine Nachrichten gefiltert wurde, dass es hält ungenießbar oder zu groß sein.

    Wenn Sie beim Erstellen von Clientanwendungen mithilfe von .NET Framework, und klicken Sie dann alle Beitrag und sich Nachrichten Nachrichten mit erwartete zuerst senden: 100-Kopfzeilen standardmäßig fortsetzen. Wie bei der serverseitigen, wird der Prozess transparent von .NET Framework behandelt. Dieses Verfahren führt jedoch jede Beitrag und sich Anforderung, die bewirken, dass auf dem Server, auch für kleine Anfragen 2 Schleifen. Wenn die Anwendung nicht Anfragen mit große Datenmengen senden ist, können Sie dieses Feature deaktivieren, mithilfe der `ServicePointManager` Klasse erstellen `ServicePoint` Objekte in der Clientanwendung. A `ServicePoint` Ziehpunkte Verbindungen, der der Client auf einem Server basierend auf das Farbschema und Host Fragmente von URIs, die Ressourcen auf dem Server zu identifizieren ist. Anschließend können Sie festlegen der `Expect100Continue` -Eigenschaft der `ServicePoint` Objekt falsch. Alle nachfolgenden Beitrag und sich Anfragen durch den Client über einen URI, das Farbschema und Host Fragmente des entspricht der `ServicePoint` Objekt ohne erwartete gesendet: 100-Kopfzeilen fortsetzen. Mit dem folgende Code veranschaulicht, wie Konfigurieren eines `ServicePoint` Objekt, das alle Anfragen gesendet URIs mit einem Schema der konfiguriert `http` sowie eine Vielzahl `www.contoso.com`.

    ```C#
    Uri uri = new Uri("http://www.contoso.com/");
    ServicePoint sp = ServicePointManager.FindServicePoint(uri);
    sp.Expect100Continue = false;
    ```

    Sie können auch die statische festlegen `Expect100Continue` Eigenschaft den `ServicePointManager` Klasse angeben des Standardwerts für diese Eigenschaft für alle neu erstellten `ServicePoint` Objekte. Weitere Informationen finden Sie unter der [Klasse ServicePoint](https://msdn.microsoft.com/library/system.net.servicepoint.aspx) -Seite auf der Microsoft-Website.

- **Support Seitenumbruch für Besprechungsanfragen, die möglicherweise großen Anzahl von Objekten zurückgibt**.

    Wenn eine Auflistung eine große Anzahl von Ressourcen enthält, konnte eine GET-Anforderung an den entsprechenden URI ausgeben Ergebnis im Wesentlichen Verarbeitung auf dem Server mit der Web-API und die Leistung beeinträchtigen und sehr viel Netzwerkverkehr in höhere Wartezeit resultierender generieren.

    Diese Fälle, sollte das Web-API Abfragezeichenfolgen unterstützen, mit denen die Clientanwendung zum Optimieren Anfragen oder Abrufen von Daten in überschaubarer, separate Blöcke (oder Seiten). Das ASP.NET Web API Framework analysiert Abfragezeichenfolgen und teilt sie Sie in eine Reihe von Parameter/Wert-Paare, die für die entsprechende Methode, übergeben werden die zuvor beschriebenen Weiterleitungsregeln folgen. Damit diese Parameter verwenden die gleichen Namen in der Abfragezeichenfolge angegebenen akzeptiert, sollte die Methode implementiert werden. Darüber hinaus sollte diese Parameter werden optional (für den Fall, dass der Client die Abfragezeichenfolge aus einer Anforderung lässt) und aussagekräftige Standardwerte aufweisen. Der folgende Code zeigt die `GetAllOrders` Methode in der `Orders` Controller. Diese Methode ruft die Details der Bestellungen ab. Wenn diese Methode beschränkten wurde, kann es eine große Datenmenge denkbar zurückgeben. Die `limit` und `offset` Parameter dienen zum Verringern der Lautstärke von Daten auf eine kleinere Teilmenge in diesem Fall nur die ersten 10 Aufträge standardmäßig:

    ```C#
    public class OrdersController : ApiController
    {
        ...
        [Route("api/orders")]
        [HttpGet]
        public IEnumerable<Order> GetAllOrders(int limit=10, int offset=0)
        {
            // Find the number of orders specified by the limit parameter
            // starting with the order specified by the offset parameter
            var orders = ...
            return orders;
        }
        ...
    }
    ```

    Eine Clientanwendung kann eine Anforderung zum Abrufen von 30 Bestellungen ab dem Offset 50 mithilfe der URI _http://www.adventure-works.com/api/orders?limit=30&offset=50_ausgeben.

    > [AZURE.TIP] Verhindern Sie die Aktivierung Clientanwendungen Abfragezeichenfolgen angeben, die sich in einem URI ergeben, die mehr als 2000 Zeichen lang ist. Viele web-Clients und Server können keine verarbeiten URIs, die diesem lang sind.

<a name="considerations-for-maintaining-responsiveness"></a>
## <a name="considerations-for-maintaining-responsiveness-scalability-and-availability"></a>Aspekte, die für die Verwaltung von Reaktionszeiten, Skalierbarkeit und Verfügbarkeit

Die gleiche Web-API möglicherweise von vielen Clientanwendungen an einer beliebigen Stelle in der Welt ausgeführt genutzt werden. Es ist wichtig, um sicherzustellen, dass das Web-API zum Verwalten der Reaktionszeiten bei hoher Auslastung implementiert wird, werden skalierbare zur Unterstützung von einer hochgradig mit wechselnder Arbeitsbelastung und Verfügbarkeit für Clients sichergestellt ist, die Business kritische Vorgänge ausführen. Berücksichtigen Sie beim bestimmen wie diese Anforderungen entsprechen die folgenden Punkte:

- **Langer Anfragen asynchrone unterstützen**.

    Eine Anforderung, die eine lange Zeit in Anspruch nehmen möglicherweise sollten ausgeführt werden, ohne Blockierung des Clients, der die Anforderung übermittelt. Im Web API kann einige anfänglichen überprüfen, überprüfen Sie die Anforderung, starten einen eigenen Vorgang aus, um die Arbeit auszuführen, und klicken Sie dann zurückzukehren eine Antwortnachricht mit HTTP-Code 202 (akzeptiert) ausführen. Der Vorgang konnte asynchrone als Teil der Web-API Verarbeitung ausführen oder es (wenn das Web-API als Azure-Cloud-Dienst implementiert wird) zu einer Azure WebJob (wenn das Web-API von einer Website Azure gehostet wird) oder eine Worker-Rolle übertragen werden konnte.

    > [AZURE.NOTE] Weitere Informationen zur Verwendung von WebJobs mit Azure-Website finden Sie auf der Seite [Verwenden WebJobs auszuführenden Hintergrundaufgaben in Microsoft Azure-Websites](../articles/app-service-web/web-sites-create-web-jobs.md) auf der Microsoft-Website.

    Im Web API sollten auch ein Verfahren, um die Ergebnisse der Verarbeitung an die Clientanwendung zurückgeben bereitstellen. Sie können dies erreichen, durch die Bereitstellung Umfragen dafür, Client Applications regelmäßig Abfrage, ob er ausgeführt hat und aktivieren das Web-API zum Senden einer Benachrichtigung, wenn der Vorgang abgeschlossen ist, oder das Ergebnis zu erhalten.

    Sie können einen einfachen Abrufmechanismus implementieren können, indem Sie eine _Abfrage_ URI, der als virtuelle Ressource wie folgt vorgehen:

    1. Die Clientanwendung sendet die ursprüngliche Anforderung im Web-API.

    2. Das Web-API speichert Informationen über die Anfrage in einer Tabelle in Tabellenspeicher oder Microsoft Azure Cache frei, und einen eindeutigen Schlüssel für diesen Posten oftmals in Form einer GUID generiert.

    3. Im Web API initiiert die Verarbeitung als separate Aufgaben. Im Web API Einträge den Status des Vorgangs in der Tabelle als _ausgeführt_.

    4. Das Web-API gibt eine Antwortnachricht mit HTTP-Statuscode 202 (akzeptiert), und die GUID des Eintrags Tabelle im Textkörper der Nachricht ein.

    5. Wenn die Aufgabe abgeschlossen wurde, im Web-API speichert die Ergebnisse in der Tabelle und wird der Status des Vorgangs auf _abgeschlossen_. Beachten Sie, wenn die Aufgabe fehlschlägt, im Web-API konnte auch Speichern von Informationen über den Fehler und den Status auf _fehlgeschlagen ist_.

    6. Während der Vorgang ausgeführt wird, kann der Client weiterhin eigenen Verarbeitung ausführt. Sie können regelmäßig sendet eine Anforderung an den URI _/polling/ {Guid}_ _{Guid}_ Wo finde ich die im Web-API in der 202 Antwortnachricht zurückgegebene GUID.

    7. Im Web-API unter _/polling/ {Guid}_ URI fragt den Status der entsprechenden Aufgabe in der Tabelle aus und gibt eine Antwortnachricht mit HTTP-Statuscode 200 (OK), die diesem Zustand (_ausgeführt_, _abgeschlossen_oder _fehlgeschlagen_) enthält. Wenn die Aufgabe abgeschlossen oder fehlgeschlagen ist, kann die Antwortnachricht ebenfalls die Ergebnisse der Verarbeitung oder Informationen zu den Grund für den Fehler verfügbar sind.

    Wenn Sie Benachrichtigungen implementieren lieber, gehören die Optionen zur Verfügung:

    - Verwenden eine Benachrichtigung Azure-Hub auf asynchrone Antworten auf Clientanwendungen Pushbenachrichtigungen. Klicken Sie auf der Microsoft-Website die Seite [Azure Benachrichtigung Hubs benachrichtigen Benutzer](notification-hubs/notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) enthält weitere Details.

    - Das Modell Comet eine dauerhafte Verbindung zwischen im Client und Server Hosten im Web API beibehalten und verwenden diese Verbindung Pushbenachrichtigungen Nachrichten vom Server zurück an den Client. [Erstellen einer einfachen Comet-Anwendung in Microsoft .NET Framework](https://msdn.microsoft.com/magazine/jj891053.aspx) im Magazin MSDN-Artikel erläutert die Lösung Beispiel.

    - Verwenden SignalR zum in Echtzeit vom Webserver an den Client über eine beständigen-Verbindung senden der Daten ein. SignalR ist als NuGet-Paket für ASP.NET-Webanwendungen verfügbar. Sie können die Informationen auf der Website [ASP.NET SignalR](http://signalr.net/) suchen.

    > [AZURE.NOTE] Comet und SignalR nutzen beide beständigen Netzwerk Verbindungen zwischen dem Webserver und der Clientanwendung. Dies kann Skalierbarkeit auswirken, wie eine große Anzahl von Clients eine gleichmäßig große Anzahl der aktiven Verbindungen erfordern.

- **Stellen Sie sicher, dass jede Anforderung statusfrei ist**.

    Jede Anforderung sollte atomaren berücksichtigt werden. Keine Abhängigkeiten zwischen eine Anforderung durch eine Clientanwendung und alle nachfolgenden Anforderungen vom gleichen Client übermittelten sollten. Dieser Ansatz hilft beim Skalierbarkeit; Instanzen des Webdiensts können auf einer Reihe von Servern bereitgestellt werden. Client-Anfragen in diesen Fällen geleitet werden können, und die Ergebnisse sollten immer gleich sein. Es verbessert die Verfügbarkeit auch einem ähnlichen Grund; Wenn Sie während den Server er sich ein Web-Server fehlschlägt, Anfragen zu einer anderen Instanz weitergeleitet werden können (mithilfe von Azure Datenverkehr Manager) werden mit keine lleffekte auf Clientanwendungen neu gestartet.

- **Nachverfolgen von Clients und implementieren begrenzungsebene, um die Wahrscheinlichkeit von DOS-Angriffen zu verringern**.

    Wenn ein bestimmter Client eine große Anzahl von Anfragen stellt innerhalb einer bestimmten Zeitspanne es möglicherweise vollständig belegen den Dienst und Einfluss auf die Leistung von anderen Clients. Um dieses Problem zu verringern, kann eine Web-API Anrufe von Clientanwendungen durch die IP-Adresse aller eingehenden Anfragen nachverfolgen oder durch Protokollierung jeder authentifizierten Zugriff überwachen. Diese Informationen können Ressourcenzugriff einschränken. Wenn ein Client einen definierten überschritten hat, kann im Web API zurückgeben eine Antwortnachricht mit Status 503 (Dienst nicht verfügbar) und eine, der angibt, wenn der Client ohne das abgelehnt wird die nächste Anforderung senden kann, die versuchen Sie es erneut nach Kopfzeile hinzufügen. Diese Strategie hilft, um die Wahrscheinlichkeit von Angriffen DOS-Of Service (DOS) aus einer Gruppe von Clients, die das System zögert zu reduzieren.

- **Ständige HTTP-Verbindungen sorgfältig verwalten**.

    Das HTTP-Protokoll unterstützt ständige HTTP-Verbindungen, wo sie zur Verfügung stehen. Die Initialisierungszeichenfolge HTTP 1.0 hinzugefügt Verbindung: beibehalten-aktiv Kopfzeile, mit dem eine Clientanwendung auf dem Server darauf hinzuweisen, dass es dieselbe Verbindung verwenden, um neue zu öffnen, anstatt nachfolgenden Anforderungen senden kann. Die Verbindung wird automatisch geschlossen, wenn der Client nicht die Verbindung innerhalb eines Zeitraums vom Host definierte wieder verwenden. Dieses Verhalten unterscheidet Standard in HTTP 1.1 als von Azure-Diensten verwendet, damit es nicht erforderlich ist-Header in Nachrichten aufnehmen möchten.

    Planmäßigen eine Verbindung zu öffnen kann zur Verbesserung der Reaktionszeiten durch Reduzierung Wartezeit und Überlastung des Netzwerks helfen können kann, aber beeinträchtigen Skalierbarkeit hält unnötige Verbindungen mehr als erforderlich, Einschränken von der anderen Clients gleichzeitig eine Verbindung herstellen, öffnen. Sie können auch Lebensdauer beeinflussen, wenn die Clientanwendung auf einem mobilen Gerät ausgeführt wird; Wenn die Anwendung auf dem Server nur gelegentliche Anfragen ist, kann das Verwalten von einer geöffneten Verbindung Akku, um schneller abzuleiten verursachen. Um sicherzustellen, dass eine Verbindung nicht mit HTTP 1.1 dauerhaft gespeichert ist, kann der Client eine Verbindung: Schließen Kopfzeile mit Nachrichten, um das Standardverhalten überschreiben enthalten. Auf ähnliche Weise, wenn ein Server eine sehr große Anzahl von Clients kann eine Verbindung: Schließen Kopfzeile in Antwortnachrichten eingeschlossen werden dem Serverressourcen speichern und schließen Sie die Verbindung.

    > [AZURE.NOTE] Ständige HTTP-Verbindungen sind eine rein optionales Feature, das Netzwerk Verwaltungsaufwand wiederholt einrichten einen Kommunikation Channel zugeordnet zu verringern. Eine dauerhafte HTTP-Verbindung nicht verfügbar sein sollten weder im Web API noch der Clientanwendung abhängig. Verwenden Sie keine ständige HTTP-Verbindungen Comet-Schreibweise Benachrichtigungssysteme implementiert wird. Sie sollten stattdessen nutzen Sockets (oder Websockets, falls vorhanden) auf der Ebene TCP. Beachten Sie, dass-Header von begrenztem Nutzen sind, wenn eine Clientanwendung mit einem Server über einen Proxy kommuniziert; nur die Verbindung mit dem Client und dem Proxy werden beständigen.

## <a name="considerations-for-publishing-and-managing-a-web-api"></a>Aspekte, die für das Veröffentlichen und Verwalten von einer Web-API

Um ein Web-API für Clientanwendungen verfügbar zu machen, muss die Web-API in einer Host-Umgebung bereitgestellt werden. Diese Umgebung ist in der Regel einen Webserver, es kann jedoch eine andere Art von Hostprozess sein. Berücksichtigen Sie beim Veröffentlichen einer Webs-API die folgenden Punkte:

- Alle Anfragen authentifiziert und autorisiert werden müssen, und die entsprechende Zugriffsebene Access-Steuerelements erzwungen werden muss.
- Eine professionelle Web API möglicherweise unterliegen verschiedene Qualität Garantien betreffend Reaktionszeiten. Es ist wichtig, um sicherzustellen, dass die Umgebung Host skalierbar ist, wenn die Last über einen Zeitraum stark variieren kann.
- Wenn möglicherweise zu Meter Besprechungsanfragen für Monetization Zwecke erforderlich.
- Möglicherweise müssen Sie zu den Datenfluss im Web API Regeln und implementieren begrenzungsebene für bestimmte Clients, die ihre Kontingente erreicht haben.
- Gesetzlich möglicherweise geben vor Protokollierung und Überwachung aller Besprechungsanfragen und Antworten.
- Zur Sicherstellung der Verfügbarkeit ist es möglicherweise erforderlich sind, um die Integrität des Servers mit dem Internet-API überwachen und starten Sie ihn erneut, falls erforderlich.

Es ist sinnvoll, diese Probleme aus der technischen Problemen, die über die Durchführung von im Web API entkoppeln können. Aus diesem Grund erwägen Sie das Erstellen einer [Fassade](http://en.wikipedia.org/wiki/Facade_pattern), Ausführung als einen separaten Prozess und die Arbeitspläne im Web API anfordert. Der Fassade kann die Verwaltungsoperationen bereitstellen und Weiterleiten Anfragen im Web API überprüft. Verwenden einer Fassade kann auch viele funktionsübergreifendes Vorteile, einschließlich übertragen:

- Fungiert als Point Integration für mehrere Web-APIs.
- Umwandeln von Nachrichten und übersetzen Kommunikationsprotokolle für Clients, die mit unterschiedlichen Technologien erstellt.
- Laden auf dem Server mit dem Internet-API Zwischenspeichern Besprechungsanfragen und Antworten zu verringern.

## <a name="considerations-for-testing-a-web-api"></a>Überlegungen zum Testen einer Webs-API
Eine Web-API sollten als sorgfältig als normaler Software getestet werden. Sie sollten Einheit Tests durch, um die Funktionalität der einzelnen Vorgänge und überprüfen erstellen, wie Sie mit einem anderen Typ von Anwendung verwenden. Weitere Informationen finden Sie unter der Seite [Überprüfung Code by Using Unit Tests](https://msdn.microsoft.com/library/dd264975.aspx) auf der Microsoft-Website.

> [AZURE.NOTE] Im Beispiel Web API zur Verfügung, der in diesem Handbuch enthält ein Projekt, das wird gezeigt, wie die ausgewählten Vorgänge über Testen von Einheiten ausführen.

Die Art der ein Web-API bringt eine eigene zusätzlichen Vorschriften zu überprüfen, ob er ordnungsgemäß arbeitet. Sie sollten besonderes Augenmerk auf folgende Aspekte:

- Testen Sie alle weitergeleitet, um sicherzustellen, dass sie die richtigen Operationen aufzurufen. Achten Sie vor allem HTTP-Statuscode 405 (Methode nicht zulässig) zurückgegeben wird unerwartet beendet, wie dies kann angeben, dass einen Konflikt zwischen einer Routing und die HTTP-Methoden (GET, POST, sich löschen), die an diese Routing verteilt werden können.

    Senden von HTTP-Anfragen zu Strecken, die keine können, wie das Senden einer POST-Anforderung an eine bestimmte Ressource unterstützen (POST-Anfragen sollte nur an Websitesammlungen Ressource gesendet werden). In diesen Fällen der einzige gültige Antwort _sollte_ Statuscode sein 405 (nicht zulässig).

- Stellen Sie sicher, dass alle leitet ordnungsgemäß geschützt sind und die entsprechenden Authentifizierung und Autorisierung überprüft werden.

    > [AZURE.NOTE] Einige Aspekte der Sicherheit wie Benutzerauthentifizierung sind wahrscheinlich den Zuständigkeitsbereich der Umgebung Host statt der Web-API sein, aber es ist weiterhin erforderlich Sicherheitstests als Teil des Bereitstellungsprozesses aufnehmen möchten.

- Testen der Behandlung von Ausnahmen von jedem Vorgang ausgeführt, und stellen Sie sicher, dass eine entsprechende und aussagekräftige HTTP-Antwort an die Clientanwendung zurückgegeben werden.
- Überprüfen Sie die Anfrage und Antwort Nachrichten wohl geformt. Beispielsweise HTTP POST-Anforderung die Daten bei einer neuen Ressource im X-www-form-urlencoded Format enthält, bestätigen Sie, dass die entsprechende Vorgang ordnungsgemäß analysiert die Daten, die Ressourcen erstellt und gibt eine Antwort, die die Details der neuen Ressource, einschließlich den richtigen Ort Header enthält.
- Überprüfen Sie alle Links und URIs in Antwortnachrichten ein. Beispielsweise sollte eine HTTP POST-Nachricht der URI der neu erstellten Ressource zurückgegeben. Alle HATEOAS Links sollte gültig sein.

    > [AZURE.IMPORTANT] Wenn Sie im Web-API über eine API-Verwaltungsdienst veröffentlichen, muss diese URIs die URL der Verwaltungsdienst und nicht mit dem Webserver mit dem Internet-API entsprechen.

- Stellen Sie sicher, dass jede Operation die richtige Statuscodes für verschiedene Kombinationen der Eingabe zurückgibt. Beispiel:
    - Wenn eine Abfrage erfolgreich ist, muss er Statuscode 200 (OK) zurückgeben.
    - Wenn eine Ressource nicht gefunden wird, sollte der Vorgang Returs HTTP-Statuscode 404 (nicht gefunden).
    - Wenn der Client eine Anforderung, die eine Ressource erfolgreich löscht sendet, sollte der Statuscode 204 (kein Inhalt) werden.
    - Wenn der Client eine Anforderung, die eine neue Ressource erstellt wird sendet, sollte der Statuscode 201 (erstellt) werden.

Achten Sie auf unerwartete Antwort Statuscodes im Bereich 5xx. Diese Nachrichten werden in der Regel vom Server gemeldet, um anzugeben, dass eine gültige Anforderung nicht erfüllen konnte.

- Testen Sie die andere Anforderung Kopfzeile Kombinationen, die eine Clientanwendung angeben kann, und sicherzustellen, dass das Web-API die erwartete Informationen in Antwortnachrichten zurückgibt.

- Testen Sie die Abfragezeichenfolgen. Wenn ein Vorgang optionale Parameter (z. B. Seitenumbruch Anfragen) ausführen kann, testen Sie die verschiedenen Kombinationen und die Reihenfolge der Parameter aus.

- Stellen Sie sicher, dass asynchrone Vorgänge erfolgreich abgeschlossen. Das Web-API unterstützt das streaming für Besprechungsanfragen, die große binäre Objekte (wie etwa Video oder Audio) zurückgeben, sicher, dass Client-Anfragen nicht gesperrt sind, während die Daten gestreamt werden. Wenn das Web-API Umfragen für langer Datenoperationen Änderung implementiert, stellen Sie sicher, dass die Vorgänge ihren Status Berichten ordnungsgemäß, wenn sie den Vorgang fortsetzen.

Sie sollten auch erstellen und Ausführen der Leistung überprüft, um zu überprüfen, dass das Web-API unter Duress korrekt ausgeführt wird. Sie können eine Web Leistung und Laden Testen des Projekts mit Visual Studio Ultimate erstellen. Weitere Informationen finden Sie unter der [Leistungstests in eine Anwendung vor eine Freigabe ausführen](https://msdn.microsoft.com/library/dn250793.aspx) Seite auf der Microsoft-Website.

## <a name="publishing-and-managing-a-web-api-by-using-the-azure-api-management-service"></a>Veröffentlichen und Verwalten von einem Web-API mithilfe der Azure-API-Verwaltungsdienst

Azure bietet [API-Verwaltungsdienst](https://azure.microsoft.com/documentation/services/api-management/) zum Veröffentlichen und Verwalten einer Webs-API verwenden können. Diese Funktion können Sie einen Dienst generieren, der Fassade für eine oder mehrere Web APIs fungiert. Der Dienst ist selbst eine skalierbare Webdienst, den Sie erstellen und mithilfe der Azure-Verwaltungsportal konfigurieren können. Sie können diesen Dienst verwenden, veröffentlichen und Verwalten von einem Web-API wie folgt:

1. Bereitstellen Sie im Web API auf einer Website, Azure-Cloud-Dienst oder Azure-virtuellen Computern.

2. Herstellen einer Verbindung im Web-API mit der API-Verwaltungsdienst. Anfragen an die URL der Management-API gesendet werden URIs im Web API zugeordnet. Der gleiche API-Verwaltungsdienst kann Anfragen an mehrere Web API weiterleiten. So können Sie mehrere Web-APIs in einer einzelnen Verwaltungsdienst aggregieren. Auf ähnliche Weise kann die gleiche Web-API aus mehreren API-Verwaltungsdienst verwiesen werden, wenn Sie einschränken oder Partitionieren der jeweiligen Anwendung verfügbaren Funktionen müssen.

    > [AZURE.NOTE] Die URL der API-Verwaltungsdienst und nicht die im Web API Hostinganbieter Webserver sollte die URIs HATEOAS Hyperlinks als Teil der Antwort für HTTP GET-Anfragen generiert verwiesen werden.

3. Geben Sie für jede Web-API, die HTTP-Vorgänge, die im Web-API zusammen mit jedem optionalen Parameter verfügbar macht, die ein Vorgang ausführen können als Eingabe. Sie können auch konfigurieren, ob der API-Verwaltungsdienst aus der Web-API wiederholte Abfragen für dieselben Daten optimiert empfangene Antwort zwischenspeichern soll. Zeichnen Sie die Details der HTTP-Antworten, die jedem Vorgang generieren können. Diese Informationen werden verwendet, um Dokumentation für Entwickler, zu generieren, sodass es wichtig ist, dass sie genau und abgeschlossen ist.

    Sie können entweder manuell mithilfe der Assistenten Azure Verwaltungsportal Vorgänge definieren oder können Sie diese aus einer Datei mit den Definitionen im Format WADL oder Swagger importieren.

4. Konfigurieren Sie die Einstellungen für die Kommunikation zwischen der Verwaltungsdienst API und dem Webserver mit dem Internet-API. Der Verwaltungsdienst API unterstützt derzeit Standardauthentifizierung und gemeinsamen Authentifizierung mithilfe von Zertifikaten OAuth 2.0 Benutzer Autorisierung.

5. Erstellen eines Produkts. Ein Produkt ist die Einheit Publikation. Sie fügen im Web-APIs, die Sie zuvor-Verwaltungsdienst mit dem Produkt verbunden. Wenn das Produkt veröffentlicht wird, werden im Web APIs Entwickler zur Verfügung.

    > [AZURE.NOTE] Vor der Veröffentlichung eines Produkts, können Sie auch Benutzergruppen definieren, die das Produkt zugreifen können, und diesen Gruppen Benutzer hinzuzufügen. Diese bietet, die Sie die Kontrolle über die Entwickler und Anwendungen, die im Web-API verwenden können. Wenn ein Web API unterliegen Genehmigung ist, muss bevor er darauf zugreifen ein Entwickler eine Anforderung an den Administrator Produkt senden. Der Administrator kann erteilen oder den Zugriff verweigern, für den Entwickler. Vorhandene Entwickler können auch blockiert werden, wenn Umstände ändern.

6.  Konfigurieren von Richtlinien für jede Web-API. Richtlinien Aufsicht Aspekten wie z. B., ob Domain-übergreifende Anrufe soll wie Clients, authentifizieren zulässig sein, ob zum Konvertieren zwischen XML und JSON Daten transparent, ob Anrufe aus einem angegebenen IP-Bereich einschränken Formate, Verwendung von Kontingenten und fest, ob der Anruf Zins beschränkt. Richtlinien können global über das gesamte Produkt für ein einzelnes Web API in einem Produkt oder für einzelne Vorgänge in einem Web-API angewendet werden.

Sie können alle Details zu wie für folgende Aufgaben auf der Seite [Management API](https://azure.microsoft.com/services/api-management/) auf der Microsoft-Website suchen. Der Azure-API Verwaltungsdienst bietet auch eine eigene REST-Oberfläche, aktivieren Sie zum Erstellen einer benutzerdefinierten Benutzeroberfläche für die Vereinfachung von einem Web-API konfigurieren. Weitere Informationen finden Sie auf der Seite [Azure API Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn776326.aspx) auf der Microsoft-Website.

> [AZURE.TIP] Azure stellt den Azure Datenverkehr-Manager, wodurch Sie Failover und Lastenausgleich Implementierung und verringern Wartezeit über mehrere Instanzen einer Website in einem anderen geografischen Standorten gehostet wird. Azure Datenverkehr-Manager können Sie in Verbindung mit der Verwaltungsdienst-API; Der Verwaltungsdienst-API können Anfragen an Instanzen einer Website über Azure Datenverkehr Manager weiterleiten.  Weitere Informationen finden Sie auf der Seite [Datenverkehr Manager routing Methoden](../articles/traffic-manager/traffic-manager-routing-methods.md) auf der Microsoft-Website.

> Wenn Sie benutzerdefinierte DNS-Namen für Ihre Websites verwenden, sollten Sie in dieser Struktur hat den entsprechenden CNAME-Eintrag für jede Website verweisen auf den Namen der DNS-Einträge der Azure Datenverkehr Manager-Website konfigurieren.

## <a name="supporting-developers-building-client-applications"></a>Unterstützung von Entwicklern erstellen-Clientanwendungen
Entwickler-Clientanwendungen normalerweise bauen erfordern Informationen zum Zugreifen auf das Web-API und Unterlagen über den Parameter, Datentypen, Rückgabetyp Typen und Absenderadresse Codes, die das andere Besprechungsanfragen und Antworten zwischen den Webdienst und der Clientanwendung beschreiben.

### <a name="documenting-the-rest-operations-for-a-web-api"></a>Dokumentieren die restlichen Vorgänge für ein Web-API
Der Azure-API Verwaltungsdienst enthält ein Entwicklerportal, die die restlichen Vorgänge durch ein Web-API verfügbar gemacht werden. Wenn ein Produkt veröffentlicht wurde, wird er auf diesem Portal angezeigt. Dieses Portal können Entwickler registrieren für den Zugriff; der Administrator kann dann genehmigen oder ablehnen. Wenn der Entwickler genehmigt wurde, werden sie einen Abonnementschlüssel zugewiesen, mit dem Anrufe von den Clientanwendungen authentifizieren, die diese entwickeln. Schlüssel jedes Web API Anruf andernfalls vorzulegen, dass er abgelehnt werden sollen.

Dieses Portal enthält auch aus:

- Dokumentation für das Produkt, wobei die Vorgänge, die verfügbar gemacht, die erforderlichen Parameter und die verschiedenen Antworten, die zurückgegeben werden können. Beachten Sie, dass diese Informationen aus der in Schritt 3 in der Liste im Abschnitt [Veröffentlichen einer Webs-API mithilfe der Microsoft Azure API-Verwaltungsdienst](#publishing-a-web-API)bereitgestellten Details generiert wird.

- Codeausschnitte, die zeigen, wie Vorgänge in mehreren Sprachen, einschließlich JavaScript, c#, Java, Ruby, Python und PHP aufrufen.

- Ein Entwickler-Konsole, können einen Entwickler zum Senden einer HTTP-Anforderung zu jedem Vorgang in das Produkt testen und die Ergebnisse anzuzeigen.

- Eine Seite, wo der Entwickler melden kann Probleme gefunden.

Verwaltungsportal Azure ermöglicht es Ihnen zum Anpassen der Entwicklerportal, um die Sucherergebnisse und das Layout zu entsprechen, das branding Ihrer Organisation zu ändern.

### <a name="implementing-a-client-sdk"></a>Implementieren eines Clients SDK
Eine Clientanwendung erstellen, die ruft REST Anfragen auf eine Web zugreifen, dass API erfordert schreiben sehr viel Code jede Anforderung erstellen und formatieren Sie ihn ordnungsgemäß, senden Sie die Anfrage auf dem Server der Webdienst gehostet und Verarbeiten der Antwort entwickelt, ob die Anforderung wurde erfolgreich abgeschlossen oder fehlgeschlagen ist, und alle zurückgegebenen Daten zu extrahieren. Um die Clientanwendung aus dieser Bedenken isolieren, können Sie ein SDK bereitstellen, umgebrochen wird die REST-Schnittstelle und fasst diese Low-Level-Details innerhalb der funktionaler Methoden. Eine Clientanwendung verwendet die folgenden Methoden, das Konvertieren transparent Anrufe in REST Anfragen und dann wieder in Methodenrückgabewerte konvertieren die Antworten. Dies ist ein gebräuchliches Verfahren, das von vielen Services, einschließlich der Azure SDK implementiert wird.

Erstellen eine clientseitige SDK ist ein dazu Unternehmen wie vorsichtig getestet und konsistente implementiert werden. Jedoch viele dieser Vorgang vorgenommen werden kann mechanischen und viele Anbieter liefern Tools, die viele dieser Aufgaben automatisieren können.

## <a name="monitoring-a-web-api"></a>Für die Überwachung einer Webs-API

Abhängig von der Sie veröffentlicht haben und das Web-API, die Sie überwachen können bereitgestellt können direkt im Web API oder Sie sammeln Informationen zur Verwendung und Gesundheit durch den Datenverkehr, der durch die API Verwaltungsdienst verläuft analysieren.

### <a name="monitoring-a-web-api-directly"></a>Eine Web-API Überwachung direkt
Wenn Sie Ihr Web API mithilfe der ASP.NET Web API-Vorlage (entweder als ein Web-API Projekt oder einer Webrolle in einer Azure-Cloud-Dienst) und Visual Studio 2013 implementiert haben, können Sie mithilfe von ASP.NET Anwendung Einsichten Verfügbarkeit, Leistung und von Verwendungsdaten sammeln. Anwendung Einsichten ist ein Paket, das transparent verfolgt und Einträge Informationen Besprechungsanfragen und Antworten, wenn das Web-API in der Cloud bereitgestellt wird. Nachdem das Paket installiert und konfiguriert ist, brauchen Sie keinen Code in Ihrem Web API zur gemeinsamen Nutzung zu ändern. Wenn Sie eine Azure-Website im Web API bereitstellen, wird der gesamte Datenverkehr untersucht und die folgenden Statistiken werden gesammelt:

- Server Antwortzeit.

- Anzahl der Server Besprechungsanfragen und die Details jeder Anforderung.

- Die oberen langsamsten Besprechungsanfragen in Bezug auf die durchschnittliche Reaktionszeiten.

- Die Details für einen Fehler bei Besprechungsanfragen

- Die Anzahl der Sitzungen initiiert von verschiedenen Browsern und Benutzer-Agents.

- Die am häufigsten angezeigten häufig Seiten (hauptsächlich nützlich für Webanwendungen statt web-APIs).

- Die verschiedenen Benutzerrollen den Zugriff auf das Web-API.

Sie können diese Daten in Echtzeit aus dem Azure-Verwaltungsportal anzeigen. Sie können auch Webtests erstellen, die die Integrität des Web-API zu überwachen. Ein Webtest sendet eine periodische Anforderung an einen angegebenen URI im Web API und die Antwort erfasst. Sie können angeben, dass die Definition der eine erfolgreiche Antwort (z. B. HTTP-Statuscode 200), und die Anfrage keinen dieser Antwort zurückgibt ordnen Sie können für eine Benachrichtigung an einen Administrator gesendet werden. Falls erforderlich, kann der Administrator den Server neu starten im Web API Hostinganbieter, wenn es fehlgeschlagen ist.

Die [Anwendung Einsichten - erste Schritte mit ASP.NET](../articles/application-insights/app-insights-asp-net.md) Seite auf der Microsoft-Website enthält weitere Informationen.

### <a name="monitoring-a-web-api-through-the-api-management-service"></a>Für die Überwachung einer Webs-API über der Verwaltungsdienst-API

Wenn Sie Ihr Web API mithilfe den API Verwaltungsdienst veröffentlicht haben, enthält der API Verwaltungsseite auf Azure Verwaltungsportal ein Dashboards, das Ihnen ermöglicht, die allgemeine Leistung des Diensts anzuzeigen. Die Analytics-Seite können Sie einen Drilldown in die Details des wie das Produkt verwendet wird. Diese Seite enthält die folgenden Registerkarten:

- **Verwendung**. Diese Registerkarte bietet Informationen über die Anzahl der API-Aufrufe und die Bandbreite verwendet, um diese Anrufe über einen Zeitraum zu behandeln. Sie können nach Produkt, API und Vorgang Verwendungsdetails filtern.

- **Dienststatus**. In diesem Registerkarte-können Sie das Ergebnis der API Anfragen (die HTTP-Status-Codes zurückgegeben), die Effizienz von der Richtlinie zwischenspeichern, die API Antwortzeit und der Dienst Antwortzeit anzeigen. In diesem Fall können Sie Gesundheitsdaten nach Produkt, API und Vorgang filtern.

- **Aktivität**. Diese Registerkarte bietet eine Zusammenfassung Text die Zahlen der erfolgreich, Fehler bei aufgerufen, gesperrte Anrufe und Mittelwert Antwort Uhrzeit Reaktionszeiten für jedes Produkt, die Web-API und den Vorgang an. Diese Seite enthält auch die Anzahl der von jeder Entwickler Aufrufe.

- **Auf einen Blick**. Diese Registerkarte zeigt eine Zusammenfassung der Leistungsdaten, einschließlich der Entwickler verantwortlich, dass die meisten API-Aufrufe, und der Produkte, Web-APIs und Vorgänge, die diese Anrufe empfangen.

Diese Informationen können Sie verwenden, um festzustellen, ob ein bestimmtes Web API oder ein Vorgang einen Engpass und erforderlichen Skalierung der Host-Umgebung, und fügen Sie weitere Server hinzu. Sie können auch festzustellen, ob einer oder mehrerer Anmeldungen ein einem Volumen von Ressourcen verwenden und anwenden die entsprechenden Richtlinien zum Festlegen von Kontingenten und Anruf Sätzen zu beschränken.

> [AZURE.NOTE] Sie können die Details für ein Produkt veröffentlichten ändern, und die Änderungen werden sofort angewendet. Sie können beispielsweise hinzufügen oder entfernen ein Vorgangs aus einem Web-API, ohne dass Sie das Produkt erneut veröffentlichen, das im Web-API enthält.

## <a name="related-patterns"></a>Verwandte Muster
- Das [Fassade](http://en.wikipedia.org/wiki/Facade_pattern) Muster beschrieben, wie eine Benutzeroberfläche für ein Web-API bereitstellen.

## <a name="more-information"></a>Weitere Informationen
- Klicken Sie auf der Microsoft-Website die Seite [ASP.NET Web API](http://www.asp.net/web-api) bietet eine detaillierte Einführung in Rest-Webdiensten mithilfe der Web-API erstellen.
- Klicken Sie auf der Microsoft-Website die Seite [Routing in ASP.NET Web-API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) beschrieben, wie Messe-basierten routing funktioniert in ASP.NET Web API-Framework.
- Weitere Informationen zum routing Attribut-basierte finden Sie unter der Seite [Attribut Routing im Web API 2](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2) auf der Microsoft-Website.
- Die [Grundlegende Lernprogramm](http://www.odata.org/getting-started/basic-tutorial/) Seite der OData-Website stellt eine Einführung in die Features von OData-Protokoll.
- Die [ASP.NET Web API OData](http://www.asp.net/web-api/overview/odata-support-in-aspnet-web-api) -Seite auf der Microsoft-Website enthält Beispiele sowie weitere Informationen zum Implementieren einer OData-Web-API mithilfe von ASP.NET.
- Auf der Microsoft-Website die Seite [Einführung in Stapel Unterstützung in Web-API und Web API OData](http://blogs.msdn.com/b/webdev/archive/2013/11/01/introducing-batch-support-in-web-api-and-web-api-odata.aspx) beschrieben, wie mithilfe von OData Stapel Vorgänge in einem Web-API zu implementieren.
- Im Artikel [Messagingtransport Muster](http://blog.jonathanoliver.com/idempotency-patterns/) in der Jonathan Oliver Blog bietet es sich um einen Überblick über die Messagingtransport und Beziehung an Management Datenoperationen.
- Die Seite [Status Codedefinitionen](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) auf der W3C-Website enthält eine vollständige Liste der HTTP-Statuscodes und deren Beschreibung an.
- Detaillierte Informationen zur Behandlung von HTTP-Ausnahmen mit der ASP.NET Web-API finden Sie auf der Seite [Ausnahme Handling in ASP.NET Web-API](http://www.asp.net/web-api/overview/error-handling/exception-handling) auf der Microsoft-Website.
- Im Artikel [Web API globalen Fehlerbehandlung](http://www.asp.net/web-api/overview/error-handling/web-api-global-error-handling) auf der Microsoft-Website beschrieben, wie einen globalen Fehler Behandlung und Protokollierung Strategie für ein Web-API implementiert wird.
- Die Seite [Hintergrundaufgaben mit WebJobs ausführen](../articles/app-service-web/web-sites-create-web-jobs.md) , klicken Sie auf der Microsoft-Website enthält Informationen und Beispiele zur Verwendung von WebJobs Hintergrundoperationen für eine Website Azure ausführen.
- Klicken Sie auf der Microsoft-Website die Seite [Azure Benachrichtigung Hubs benachrichtigen Benutzer](notification-hubs/notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) zeigt, wie Sie mit einer Benachrichtigung Azure-Hub asynchrone Antworten auf Clientanwendungen Pushbenachrichtigungen.
- Der [API](https://azure.microsoft.com/services/api-management/) -Verwaltungsseite auf der Microsoft-Website beschrieben, wie Sie ein Produkt, liefert gesteuert veröffentlichen und sicheren Zugriff auf ein Web-API.
- Die Seite [Azure API Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn776326.aspx) auf der Microsoft-Website beschrieben, wie die API Management REST-API verwenden, um Programme für die benutzerdefinierte Verwaltung zu erstellen.
- Die Seite [Datenverkehr Manager Routing Methoden](../articles/traffic-manager/traffic-manager-routing-methods.md) auf der Microsoft-Website fasst zusammen, wie Azure Datenverkehr Manager auf einen Lastenausgleich Anfragen über mehrere Instanzen einer Website, die eine Web-API Hostinganbieter verwendet werden kann.
- Die [Anwendung Einsichten - erste Schritte mit ASP.NET](../articles/application-insights/app-insights-asp-net.md) Seite auf der Microsoft-Website enthält ausführliche Informationen zum Installieren und Konfigurieren von Anwendung Einsichten in einem Projekt ASP.NET Web API.
- Klicken Sie auf der Microsoft-Website die Seite [Überprüfung Code by Using Unit Tests](https://msdn.microsoft.com/library/dd264975.aspx) enthält ausführliche Informationen zum Erstellen und Verwalten von Einheit Tests mit Visual Studio.
- Die Seite [Leistungstests in eine Anwendung vor eine Freigabe ausführen](https://msdn.microsoft.com/library/dn250793.aspx) , klicken Sie auf der Microsoft-Website beschrieben, wie Visual Studio Ultimate zum Erstellen einer Web-Leistung und Laden Testprojekt verwendet wird.
